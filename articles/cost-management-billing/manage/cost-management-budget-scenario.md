---
title: Сценарий работы с бюджетом для выставления счетов и управления затратами в Azure
description: Сведения об использовании средств автоматизации Azure для завершения работы виртуальных машин при превышении конкретных порогов бюджета.
author: bandersmsft
ms.reviewer: adwise
tags: billing
ms.service: cost-management-billing
ms.topic: reference
ms.date: 05/04/2020
ms.author: banders
ms.openlocfilehash: 633ca5cd16b8e730225900c30c575e74a0956ada
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791686"
---
# <a name="manage-costs-with-azure-budgets"></a>Управление затратами с помощью API управления бюджетом Azure

Управление расходами имеет огромное значение для повышения окупаемости инвестиций в облако. Есть несколько сценариев, где сведения о расходах, отчетность и оркестрация с учетом затрат очень важны для обеспечения непрерывных бизнес-операций. [Среди API-интерфейсов службы "Управление затратами Azure"](https://docs.microsoft.com/rest/api/consumption/) есть набор API, подходящий для каждого из этих сценариев. API предоставляют сведения об использовании, позволяя просматривать детальные данные о расходах на уровне экземпляра.

Бюджеты обычно используются в рамках управления затратами. В Azure бюджеты можно ограничить. Например, представление бюджета можно сузить до подписки, группы ресурсов или коллекции ресурсов. Помимо использования API управления бюджетом для отправки уведомления по электронной почте при достижении порога бюджета можно использовать [группы действий Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups), чтобы активировать согласованный набор действий в результате связанного с бюджетом события.

Если клиент, выполняющий некритическую рабочую нагрузку, хочет управлять бюджетом и видеть сведения о прогнозируемых расходах в ежемесячном счете, у него может возникнуть распространенный сценарий управления бюджетом. Для этого сценария требуется выполнить оркестрацию с учетом расходов на ресурсы, которые являются частью среды Azure. В этом сценарии месячный бюджет для подписки составляет 1000 долл. США. Кроме того, заданы пороги уведомлений для активации нескольких действий оркестрации. Этот сценарий начинается с порога расходов в 80 %, при достижении которого будут остановлены все виртуальные машины в группе ресурсов **Дополнительно**. Затем при достижении порога расходов в 100 % будут остановлены все экземпляры виртуальной машины.

Чтобы настроить этот сценарий, выполните следующие действия, описанные в каждом разделе этого руководства.

Действия, приведенные в этом руководстве, позволят сделать следующее:

- Создать runbook службы автоматизации Azure, чтобы останавливать виртуальные машины с помощью веб-перехватчиков.
- Создать приложение логики Azure, которое будет активироваться при достижении значения порога бюджета и вызывать runbook с правильными параметрами.
- Создать группу действий Azure Monitor, которая будет настроена для запуска приложения логики Azure при достижении порога бюджета.
- Создать бюджет на Azure с желаемыми порогами и привязать его к группе действий.

## <a name="create-an-azure-automation-runbook"></a>Создание runbook службы автоматизации Azure

[Служба автоматизации Azure](https://docs.microsoft.com/azure/automation/automation-intro) — это служба, позволяющая создавать сценарии большинства задач по управлению ресурсами и выполнять эти задачи по расписанию или по требованию. В рамках этого сценария вы создадите модуль [runbook службы автоматизации Azure](https://docs.microsoft.com/azure/automation/automation-runbook-types), который будет использоваться для остановки виртуальных машин. Для создания этого сценария вы воспользуетесь графическим модулем runbook [Stop Azure V2 VMs](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) из [коллекции](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Импортировав этот runbook в учетную запись Azure и опубликовав его, вы сможете остановить виртуальные машины при достижении порога бюджета.

### <a name="create-an-azure-automation-account"></a>Создание учетной записи службы автоматизации Azure

1. Войдите на [портал Azure](https://portal.azure.com/) с помощью учетных данных учетной записи Azure.
2. Нажмите кнопку **Создать ресурс** в верхнем левом углу окна портала Azure.
3. Выберите **Средства управления** > **Автоматизация**.
   > [!NOTE]
   > Если у вас нет учетной записи Azure, вы можете создать [бесплатную учетную запись](https://azure.microsoft.com/free/).
4. Введите сведения об учетной записи. Для параметра **Создать учетную запись запуска от имени Azure** выберите **Да**, чтобы автоматически включить параметры, необходимые для упрощения аутентификации в Azure.
5. По завершении выберите **Создать**, чтобы начать развертывание учетной записи службы автоматизации.

### <a name="import-the-stop-azure-v2-vms-runbook"></a>Импорт runbook для остановки виртуальных машин Azure версии 2

Используя [runbook службы автоматизации Azure](https://docs.microsoft.com/azure/automation/automation-runbook-types), импортируйте графический runbook [остановки виртуальных машин Azure версии 2](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) из коллекции.

1. Войдите на [портал Azure](https://portal.azure.com/) с помощью учетных данных учетной записи Azure.
1. Откройте учетную запись службы автоматизации, выбрав **Все службы** > **Учетные записи автоматизации**. Затем выберите учетную запись службы автоматизации Azure.
1. Выберите элемент **Коллекция модулей Runbook** в разделе **Автоматизация процессов**.
1. Для параметра **Источник коллекции** задайте значение **Центр сценариев** и нажмите кнопку **ОК**.
1. Найдите и выберите элемент коллекции [Stop Azure V2 VMs](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) (Остановка виртуальных машин Azure версии 2) на портале Azure.
1. Выберите элемент **Импорт**, чтобы отобразить область **Импорт**, а затем нажмите кнопку **ОК**. Отобразится область обзора модулей runbook.
1. После того как runbook завершит процесс импорта, выберите **Изменить**, чтобы отобразить графический редактор runbook и параметр публикации.  
    ![Изменение графических модулей runbook на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-01.png)
1. Выберите **Опубликовать**, чтобы опубликовать runbook, и нажмите кнопку **Да** в появившемся запросе. При публикации runbook имеющаяся опубликованная версия перезаписывается черновой версией. В нашем случае опубликованной версии не существует, так как вы создали runbook.
    Дополнительные сведения о публикации runbook см. в статье [Первый графический Runbook](https://docs.microsoft.com/azure/automation/automation-first-runbook-graphical).

## <a name="create-webhooks-for-the-runbook"></a>Создание веб-перехватчиков для runbook

С помощью графического модуля runbook [Stop Azure V2 VMs](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) вы создаете два веб-перехватчика, чтобы запустить runbook в службе автоматизации Azure с помощью одного HTTP-запроса. Первый веб-перехватчик вызывает runbook при достижении порога бюджета в 80 % с именем группы ресурсов в качестве параметра, что позволяет остановить дополнительные виртуальные машины. Затем второй веб-перехватчик вызывает runbook без параметров (при достижении 100 %), в результате чего останавливаются все остальные экземпляры виртуальных машин.

1. На странице **Модули Runbook** на [портале Azure](https://portal.azure.com/) выберите модуль runbook **StopAzureV2Vm**, который отображается в области обзора модулей runbook.
1. Выберите **Веб-перехватчик** в верхней части страницы, чтобы открыть область **Добавить веб-перехватчик**.
1. Выберите **Создать новый веб-перехватчик**, чтобы открыть область **создания веб-перехватчика**.
1. Для параметра **Имя** веб-перехватчика задайте значение **Дополнительно**. Для свойства **Включено** необходимо выбрать значение **Да**. Значение параметра **Срок действия** можно не менять. Дополнительные сведения о свойствах веб-перехватчиков см. [здесь](../../automation/automation-webhooks.md#webhook-properties).
1. Возле значения URL-адреса щелкните значок копирования, чтобы скопировать URL-адрес веб-перехватчика.
   > [!IMPORTANT]
   > Сохраните URL-адрес веб-перехватчика с именем **Дополнительно** в надежном месте. Этот URL-адрес понадобится позже в этом руководстве. Из соображений безопасности после создания веб-перехватчика URL-адрес нельзя просмотреть или получить повторно.
1. Нажмите кнопку **ОК**, чтобы создать новый веб-перехватчик.
1. Выберите **Настройка параметров и настроек запуска**, чтобы просмотреть значения параметров runbook.
   > [!NOTE]
   > Если модуль Runbook содержит обязательные параметры, то его нельзя будет создать, не указав значения.
1. Нажмите кнопку **ОК**, чтобы принять значения параметров веб-перехватчика.
1. Выберите элемент **Создать**, чтобы создать веб-перехватчик.
1. Затем выполните приведенные выше действия, чтобы создать второй веб-перехватчик с именем **Завершить**.
    > [!IMPORTANT]
    > Обязательно сохраните оба URL-адреса веб-перехватчиков для использования далее в этом руководстве. Из соображений безопасности после создания веб-перехватчика URL-адрес нельзя просмотреть или получить повторно.

Теперь у вас два настроенных веб-перехватчика, каждый из которых доступен по сохраненным URL-адресам.

![Веб-перехватчики — "Дополнительно" и "Завершить"](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-02.png)

Теперь установка службы автоматизации Azure завершена. Работу веб-перехватчиков можно проверить с помощью простого теста Postman. Далее необходимо создать приложение логики для оркестрации.

## <a name="create-an-azure-logic-app-for-orchestration"></a>Создание приложения логики Azure для оркестрации

Logic Apps помогает создавать, планировать и автоматизировать рабочие процессы, что позволяет интегрировать приложения, данные, системы и службы на разных предприятиях или в организациях. В этом случае создаваемое [приложение логики](https://docs.microsoft.com/azure/logic-apps/) будет делать нечто большее, чем просто вызывать созданный веб-перехватчик службы автоматизации.

Бюджеты можно настроить для запуска уведомления при достижении указанного порога. Вы можете предоставить несколько порогов, при достижении которых вам будет отправлено уведомление, и приложение логики будет демонстрировать возможности выполнения различных действий на основе достигнутого порога. В этом примере настраивается сценарий, в котором вы получаете несколько уведомлений. Первое уведомление приходит, когда достигнуто 80 % бюджета, а второе — при достижении 100 % бюджета. Приложение логики будет использоваться для завершения работы всех виртуальных машин в группе ресурсов. Сначала порог **Дополнительно** будет достигнут при использовании 80 % бюджета, а затем будет достигнут второй порог, когда все виртуальные машины в подписке завершат работу.

Приложения логики позволяют предоставить пример схемы для триггера HTTP, но для них требуется задать заголовок **Content-Type**. Так как группа действий не содержит пользовательские заголовки для веб-перехватчика, необходимо проанализировать полезные данные в рамках отдельного шага. Вы выполните действие **Анализировать** и предоставите ему пример полезных данных.

### <a name="create-the-logic-app"></a>Создание приложения логики

Приложение логики выполнит несколько действий. В списке ниже приведен высокоуровневый набор действий, которые будет выполнять приложение логики.

- Распознавание получения HTTP-запроса.
- Анализ переданного в данные JSON-файла, чтобы определить достигнутый порог.
- Использование условного оператора для проверки, достигнут ли порог 80 % или больший показатель использования бюджета, но не больше или равно 100 %.
  - При достижении этого порога отправьте HTTP-запрос POST с помощью веб-перехватчика с именем **Дополнительно**. Это действие завершит работу виртуальных машин в группе "Дополнительно".
- Используйте условный оператор для проверки того, достигнут ли или превышен порог в 100 % значения бюджета.
  - При достижении порога отправьте HTTP-запрос POST с помощью веб-перехватчика с именем **Завершить**. Это действие завершит работу всех остальных виртуальных машин.

Для создания приложения логики, которое будет выполнять описанные выше действия, необходимо сделать следующее:

1. На [портале Azure](https://portal.azure.com/) выберите **Создать ресурс** > **Интеграция** > **Приложение логики**.  
    ![Выбор ресурса приложения логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-03.png)
1. В области **Создание приложения логики** укажите сведения, необходимые для создания приложения логики, выберите **Закрепить на панели мониторинга** и нажмите кнопку **Создать**.  
    ![Создание приложения логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-03a.png)

После развертывания приложения логики в Azure открывается **конструктор Logic Apps** и отображается область с ознакомительным видео и часто используемыми триггерами.

### <a name="add-a-trigger"></a>Добавление триггера

Каждое приложение логики должно запускаться по триггеру, который активируется, когда происходит определенное событие или если выполняются заданные условия. При каждом срабатывании триггера обработчик Logic Apps создает экземпляр приложения логики, запускающий и выполняющий рабочий процесс. Действия — это все шаги, выполняемые после срабатывания триггера.

1. В разделе **Шаблоны** области **Конструктор Logic Apps** выберите **Пустое приложение логики**.
1. Добавьте [триггер](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts), введя "HTTP-запрос" в поле поиска **конструктора Logic Apps**, чтобы найти и выбрать триггер с именем **Request – When an HTTP request is received** (Запрос — при получении HTTP-запроса).  
    ![Триггер HTTP в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-04.png)
1. Выберите **New step** (Новый шаг) > **Добавить действие**.  
    ![Элементы "Новый шаг" > "Добавить действие" на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-05.png)
1. Выполните поиск по запросу "анализ JSON" в поле поиска **конструктора Logic Apps**, чтобы найти и выбрать [действие](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts) **Data Operations - Parse JSON** (Операции с данными — анализ JSON).  
    ![Добавление действия анализа JSON в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-06.png)
1. Введите Payload в качестве имени **содержимого** для полезных данных Parse JSON или используйте тег Body из динамического содержимого.
1. Выберите **Использовать пример полезной нагрузки, чтобы создать схему** в диалоговом поле **Анализ JSON**.  
    ![Использование примера данных JSON для создания схемы в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-07.png)
1. Вставьте следующий пример полезных данных JSON в текстовое поле: `{"schemaId":"AIP Budget Notification","data":{"SubscriptionName":"CCM - Microsoft Azure Enterprise - 1","SubscriptionId":"<GUID>","SpendingAmount":"100","BudgetStartDate":"6/1/2018","Budget":"50","Unit":"USD","BudgetCreator":"email@contoso.com","BudgetName":"BudgetName","BudgetType":"Cost","ResourceGroup":"","NotificationThresholdAmount":"0.8"}}`
   Текстовое поле будет выглядеть следующим образом:  
    ![Пример полезных данных JSON в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-08.png)
1. Нажмите кнопку **Готово**.

### <a name="add-the-first-conditional-action"></a>Добавление первого условного действия

Использование условного оператора для проверки, достигнут ли порог 80 % или больший показатель использования бюджета, но не больше или равно 100 %. При достижении этого порога отправьте HTTP-запрос POST с помощью веб-перехватчика с именем **Дополнительно**. Это действие завершит работу виртуальных машин в группе **Дополнительно**.

1. Выберите **Новый шаг** > **Добавить условие**.  
    ![Добавление условия в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-09.png)
1. В поле **Условие** выберите текстовое поле с текстом `Choose a value`, чтобы отобразить список доступных значений.  
    ![Поле "Условие" в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-10.png)
1. Выберите **Выражение** в верхней части списка и введите следующее выражение в редакторе выражений: `float()`  
    ![Выражение float в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-11.png)
1. Выберите **Динамическое содержимое**, поместите курсор внутри скобок () и выберите **NotificationThresholdAmount** из списка, чтобы заполнить полное выражение.
   Выражение будет выглядеть следующим образом:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`
1. Нажмите кнопку **ОК**, чтобы задать выражение.
1. Выберите **больше или равно** в раскрывающемся списке поля **Условие**.
1. В поле **Выберите значение** условия введите `.8`.  
    ![Выражение float со значением в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-12.png)
1. Выберите **Добавить** > **Добавить строку** в поле "Условия", чтобы добавить часть условия.
1. В поле **Условие** выберите текстовое поле с текстом `Choose a value`.
1. Выберите **Выражение** в верхней части списка и введите следующее выражение в редакторе выражений: `float()`
1. Выберите **Динамическое содержимое**, поместите курсор внутри скобок () и выберите **NotificationThresholdAmount** из списка, чтобы заполнить полное выражение.
1. Нажмите кнопку **ОК**, чтобы задать выражение.
1. В раскрывающемся списке поля **Условие** выберите **меньше**.
1. В поле **Выберите значение** условия введите `1`.  
    ![Выражение float со значением в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-13.png)
1. В поле **Если истинно** выберите **Добавить действие**. Добавится действие HTTP POST, которое будет завершать работу дополнительных виртуальных машин.  
    ![Добавление действия в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-14.png)
1. Введите **HTTP**, чтобы найти действие HTTP, и выберите действие **HTTP — HTTP**.  
    ![Добавление действия HTTP в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-15.png)
1. Выберите значение **Post** для параметра **Метод**.
1. Введите URL-адрес для веб-перехватчика с именем **Дополнительно**, созданного ранее в этом руководстве, в качестве значения **URI**.  
    ![URI действия HTTP в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-16.png)
1. В поле **Если истинно** выберите **Добавить действие**. Будет добавлено действие электронной почты, которое отправит сообщение электронной почты, уведомляющее получателя о завершении работы дополнительных виртуальных машин.
1. Выполните поиск по запросу "отправка сообщения" и выберите действие *отправки сообщения электронной почты* в зависимости от используемой службы электронной почты.  
    ![Действие отправки сообщения электронной почты в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-17.png)

    Для личных учетных записей Майкрософт выберите **Outlook.com**. Для рабочих или учебных учетных записей Azure выберите **Office 365 Outlook**. Если у вас еще нет подключения, появится запрос на вход в учетную запись электронной почты. Logic Apps создает подключение к учетной записи электронной почты.
   Вам понадобится предоставить приложению логики доступ к вашей электронной почте.  
    ![Уведомление о доступе в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-18.png)
1. Заполните текстовые поля **Кому**, **Тема** и **Текст** для сообщения электронной почты, которое уведомляет получателя о завершении работы дополнительных виртуальных машин. Используйте динамическое содержимое **BudgetName** и **NotificationThresholdAmount**, чтобы заполнить поля "Тема" и "Текст". 
    ![Сведения об электронной почте в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-19.png)

### <a name="add-the-second-conditional-action"></a>Добавление второго условного действия

Используйте условный оператор для проверки того, достигнут ли или превышен порог в 100 % значения бюджета. При достижении порога отправьте HTTP-запрос POST с помощью веб-перехватчика с именем **Завершить**. Это действие завершит работу всех остальных виртуальных машин.

1. Выберите **Новый шаг** > **Добавить условие**.  
    ![Добавление действия в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-20.png)
1. В поле **Условие** выберите текстовое поле с текстом `Choose a value`, чтобы отобразить список доступных значений.
1. Выберите **Выражение** в верхней части списка и введите следующее выражение в редакторе выражений: `float()`
1. Выберите **Динамическое содержимое**, поместите курсор внутри скобок () и выберите **NotificationThresholdAmount** из списка, чтобы заполнить полное выражение.
   Выражение будет выглядеть следующим образом:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`
1. Нажмите кнопку **ОК**, чтобы задать выражение.
1. Выберите **больше или равно** в раскрывающемся списке поля **Условие**.
1. В поле **Выберите значение** для условия введите `1`.  
    ![Установка значения условия в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-21.png)
1. В поле **Если истинно** выберите **Добавить действие**. Добавится действие HTTP POST, которое будет завершать работу всех оставшихся виртуальных машин.  
    ![Добавление действия в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-22.png)
1. Введите **HTTP**, чтобы найти действие HTTP, и выберите действие **HTTP — HTTP**.
1. Выберите значение **Post** для параметра **Метод**.
1. Введите URL-адрес для веб-перехватчика с именем **Завершить**, созданного ранее в этом руководстве, в качестве значения **URI**.  
    ![Добавление действия в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-23.png)
1. В поле **Если истинно** выберите **Добавить действие**. Будет добавлено действие электронной почты, которое отправит сообщение электронной почты, уведомляющее получателя о завершении работы оставшихся виртуальных машин.
1. Выполните поиск по запросу "отправка сообщения" и выберите действие *отправки сообщения электронной почты* в зависимости от используемой службы электронной почты.
1. Заполните текстовые поля **Кому**, **Тема** и **Текст** для сообщения электронной почты, которое уведомляет получателя о завершении работы дополнительных виртуальных машин. Используйте динамическое содержимое **BudgetName** и **NotificationThresholdAmount**, чтобы заполнить поля "Тема" и "Текст".  
    ![Сведения об отправке сообщений электронной почты в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-24.png)
1. Выберите элемент **Сохранить** в верхней части области **Конструктор Logic Apps**.

### <a name="logic-app-summary"></a>Сводная информация о приложении логики

По завершении приложение логики может выглядеть следующим образом. В базовых сценариях, где не нужна какая-либо оркестрация на основе порога, можно напрямую вызвать скрипт автоматизации из раздела **Монитор** и пропустить шаг **Приложение логики**.

![Полное представление в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-25.png)

После сохранения приложения логики будет создан URL-адрес, который можно вызвать. Этот URL-адрес будет использоваться в следующем разделе этого руководства.

## <a name="create-an-azure-monitor-action-group"></a>Создание группы действий Azure Monitor

Группа действий — это коллекция параметров уведомлений, которые вы определяете. При активации оповещения определенная группа действий может получить оповещение в уведомлении. Оповещение Azure заранее создает уведомление в зависимости от конкретных условий и предоставляет возможность выполнить действие. Оповещение может использовать данные из нескольких источников, включая метрики и журналы.

Группы действий являются единственной конечной точкой, которая будет интегрирована с вашим бюджетом. Вы можете настроить уведомления в нескольких каналах, но для этого сценария мы сосредоточимся на приложении логики, созданном по инструкциям из этого руководства.

### <a name="create-an-action-group-in-azure-monitor"></a>Создание группы действий в Azure Monitor

При создании группы действий вы перейдете в приложение логики, созданное по инструкциям из этого руководства.

1. Если вы еще не вошли на [портал Azure](https://portal.azure.com/), выполните вход и выберите **Все службы** > **Монитор**.
1. Выберите **Оповещения**, а затем **Управление действиями**.
1. Выберите **Add an action group** (Добавить группу действий) в области **Группы действий**.
1. Добавьте и проверьте следующее:
    - имя группы действий;
    - короткое имя;
    - Подписка
    - Группа ресурсов  
    ![Добавление группы действий в приложении логики на портале Azure](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-26.png)
1. На панели **Добавить группу действий** добавьте действие приложения логики. Присвойте действию имя **Budget-BudgetLA**. На панели **Приложение логики** выберите **Подписка** и **Группа ресурсов**. Выберите **приложение логики**, созданное ранее в этом руководстве.
1. Нажмите кнопку **ОК**, чтобы задать приложение логики. Нажмите кнопку **ОК** на панели **Добавить группу действий**, чтобы создать группу действий.

Теперь у вас есть все вспомогательные компоненты, необходимые для эффективного управления бюджетом. Теперь все, что вам нужно, — всего лишь создать бюджет и настроить его для использования созданной группы действий.

## <a name="create-the-azure-budget"></a>Создание бюджета Azure

Вы можете создать бюджет на портале Azure с помощью [компонента "Бюджет"](../costs/tutorial-acm-create-budgets.md) в службе "Управление затратами". Вы также можете создать бюджет с помощью интерфейсов REST API, командлетов PowerShell или CLI. В следующей процедуре используется REST API. Для вызова REST API вам потребуется маркер авторизации. Чтобы создать маркер авторизации, можно использовать проект [ARMClient](https://github.com/projectkudu/ARMClient). **ARMClient** позволяет пройти проверку подлинности в Azure Resource Manager и получить маркер для вызова API.

### <a name="create-an-authentication-token"></a>Создание маркера проверки подлинности

1. Перейдите к проекту [ARMClient](https://github.com/projectkudu/ARMClient) в GitHub.
1. Клонируйте репозиторий для получения локальной копии.
1. Откройте проект в Visual Studio и создайте его.
1. После успешного создания исполняемый файл должен находиться в папке *\bin\debug*.
1. Запустите ARMClient. Откройте командную строку и перейдите к папке *\bin\debug* из корневой папки проекта.
1. Чтобы войти в систему и пройти проверку подлинности, введите следующую команду в командной строке:<br>
    `ARMClient login prod`
1. Скопируйте **GUID подписки** из выходных данных.
1. Чтобы скопировать маркер авторизации в буфер обмена, введите следующую команду в командной строке, но обязательно используйте идентификатор подписки, скопированный на предыдущем шаге: <br>
    `ARMClient token <subscription GUID from previous step>`

    Выполнив предыдущий шаг, вы увидите следующее:<br>
    **Маркер успешно скопирован в буфер обмена.**
1. Сохраните маркер для выполнения шагов в следующем разделе данного руководства.

### <a name="create-the-budget"></a>Создание бюджета

Далее вы настроите **Postman** для создания бюджета путем вызова интерфейсов REST API потребления Azure. Postman — это среда разработки API. Вы импортируете файлы среды и коллекции в Postman. Коллекция содержит сгруппированные определения HTTP-запросов, которые вызывают REST API потребления Azure. Файл среды содержит переменные, которые используются коллекцией.

1. Загрузите и откройте [клиент REST Postman](https://www.getpostman.com/) для выполнения REST API.
1. Создайте запрос в Postman.  
    ![Создание запроса в Postman](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-27.png)
1. Сохраните новый запрос как коллекцию, чтобы новый запрос не был с ним связан.  
    ![Сохранение нового запроса в Postman](./media/cost-management-budget-scenario/billing-cost-management-budget-scenario-28.png)
1. Измените запрос с `Get` на действие `Put`.
1. Измените следующий URL-адрес, заменив `{subscriptionId}` на **идентификатор подписки**, использованный в предыдущем разделе этого руководства. Кроме того, добавьте в URL-адрес "SampleBudget" в качестве значения для `{budgetName}`: `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2018-03-31`
1. Выберите вкладку **Headers** (Заголовки) в Postman.
1. Добавьте новый **ключ** с именем "Authorization" в соответствующее поле.
1. Для параметра **Value** (Значение) задайте маркер, созданный с помощью ArmClient в конце последнего раздела.
1. Выберите вкладку **Body** (Текст) в Postman.
1. Установите переключатель **raw** (необработанный).
1. В текстовом поле вставьте приведенный ниже пример определения бюджета, заменив значения параметров `subscriptionID`, `resourcegroupname``actiongroupname` вашим идентификатором подписки, уникальным именем группы ресурсов именем созданной группы действий в URL-адресе и тексте запроса:

    ```
        {
            "properties": {
                "category": "Cost",
                "amount": 100.00,
                "timeGrain": "Monthly",
                "timePeriod": {
                "startDate": "2018-06-01T00:00:00Z",
                "endDate": "2018-10-31T00:00:00Z"
                },
                "filters": {
                },
            "notifications": {
                "Actual_GreaterThan_80_Percent": {
                    "enabled": true,
                    "operator": "GreaterThan",
                    "threshold": 80,
                    "contactEmails": [
                    ],
                    "contactRoles": [
                    ],
                    "contactGroups": [
                    "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}
                    ]
                },
            "Actual_EqualTo_100_Percent": {
                    "operator": "EqualTo",
                    "threshold": 100,
                    "contactGroups": [
                "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}"
                    ]
                }
            }
        }
    ```
1. Нажмите кнопку **Send** (Отправить), чтобы отправить запрос.

Теперь у вас есть все компоненты, необходимые для вызова [API бюджетов](https://docs.microsoft.com/rest/api/consumption/budgets). В справочнике по API бюджетов содержатся дополнительные сведения о конкретных запросах, включая следующие:

- **budgetName** — поддерживаются несколько бюджетов.  Имена бюджетов должны быть уникальными.
- **category** — должна быть либо **Cost**, либо **Usage**. API поддерживает бюджет как на основе затрат, так и на основе использования.
- **timeGrain** — бюджет за месяц, квартал или год. В конце периода сумма сбрасывается.
- **filters** — фильтры позволяют сузить бюджет до определенного набора ресурсов в выбранной области. Например, фильтром может быть коллекция групп ресурсов для бюджета уровня подписки.
- **notifications** — определяет сведения об уведомлении и пороги. Вы можете установить несколько порогов и указать адрес электронной почты или группу действий, чтобы получать уведомления.

## <a name="summary"></a>Сводка

Из этого руководства вы узнали следующее:

- Как создать runbook службы автоматизации Azure для остановки виртуальных машин.
- Как создать приложение логики Azure, которое активируется при достижении значений порога бюджета и вызывает связанный runbook с правильными параметрами.
- Как создать группу действий Azure Monitor, настроенную для запуска приложения логики Azure при достижении порога бюджета.
- Как создать бюджет на Azure с требуемыми порогами и привязать его к группе действий.

Теперь у вас есть полнофункциональный бюджет для вашей подписки, который будет завершать работу виртуальных машин по достижении настроенных порогов бюджета.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о сценариях выставления счетов Azure см. в разделе [Сценарии автоматизации для выставления счетов и управления затратами](cost-management-automation-scenarios.md).
