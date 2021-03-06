---
title: Решение для запуска и остановки виртуальных машин в нерабочее время
description: Это решение для управления виртуальными машинами запускает и останавливает виртуальные машины Azure по расписанию и отслеживает Azure Monitor журналов.
services: automation
ms.subservice: process-automation
ms.date: 04/28/2020
ms.topic: conceptual
ms.openlocfilehash: f7e30fd0d53af7ee61d919b56e9ffcd1f1b6bd36
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82207604"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Решение для запуска и остановки виртуальных машин в нерабочее время в службе автоматизации Azure

Решение " **Запуск и остановка виртуальных машин в нерабочее время** " запускает или останавливает виртуальные машины Azure. Он запускает или останавливает компьютеры в пользовательских расписаниях, предоставляет аналитические сведения через журналы Azure Monitor и отправляет дополнительные сообщения электронной почты с помощью [групп действий](../azure-monitor/platform/action-groups.md). Решение поддерживает как Azure Resource Manager, так и классические виртуальные машины для большинства сценариев. 

Это решение использует командлет [Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm?view=azurermps-6.13.0) для запуска виртуальных машин. Для остановки виртуальных машин используется [остановить-AzureRmVM](https://docs.microsoft.com/powershell/module/AzureRM.Compute/Stop-AzureRmVM?view=azurermps-6.13.0) .

> [!NOTE]
> Решение " **Запуск и завершение виртуальных машин в нерабочее время** " Обновлено для поддержки последних версий доступных модулей Azure. Обновленная версия этого решения, доступная в Marketplace, не поддерживает модули AzureRM, так как мы перешли с AzureRM на AZ modules.

Решение предоставляет децентрализованную автоматизацию с низкими затратами для пользователей, желающих оптимизировать затраты на виртуальные машины. Это решение обеспечивает следующие возможности:

- [Запланируйте запуск и завершение виртуальных машин](automation-solution-vm-management-config.md#schedule).
- Запланируйте запуск и завершение виртуальных машин в возрастающем порядке с [помощью тегов Azure](automation-solution-vm-management-config.md#tags) (не поддерживается для классических виртуальных машин).
- Автозавершение виртуальных машин на основе [низкой загрузки ЦП](automation-solution-vm-management-config.md#cpuutil).

Ниже перечислены ограничения, связанные с текущим решением.

- Она управляет виртуальными машинами в любом регионе, но может использоваться только в той же подписке, что и учетная запись службы автоматизации Azure.
- Она доступна в Azure и Azure для государственных организаций в любом регионе, поддерживающем рабочую область Log Analytics, учетную запись службы автоматизации Azure и оповещения. Сейчас регионы Azure для государственных организаций не поддерживают функции электронной почты.

## <a name="solution-prerequisites"></a>Необходимые условия для решения

Аутентификация модулей runbook в этом решении осуществляется с помощью [учетной записи запуска от имени Azure](automation-create-runas-account.md). Предпочтительным методом проверки подлинности является учетная запись запуска от имени, так как она использует проверку подлинности на основе сертификата вместо пароля, который может истечь или часто меняться.

Мы рекомендуем использовать отдельную учетную запись службы автоматизации для решения " **Запуск и завершение виртуальных машин в нерабочее время** ". Версии модулей Azure часто обновляются, и их параметры могут измениться. Решение не обновляется в той же ритмичности и может не работать с более новыми версиями используемых командлетов. Рекомендуется протестировать обновления модуля в учетной записи службы автоматизации тестов, прежде чем импортировать их в рабочую учетную запись автоматизации.

## <a name="solution-permissions"></a>Разрешения решения

Для развертывания **виртуальных машин в нерабочее время** необходимо иметь определенные разрешения. Разрешения отличаются, если решение использует предварительно созданную учетную запись службы автоматизации и Log Analytics рабочую область из разрешений, необходимых, если решение создает новую учетную запись и рабочую область во время развертывания. 

Вам не нужно настраивать разрешения, если вы являетесь участником подписки и глобальным администратором в Azure Active Directory клиенте. Если у вас нет этих прав или вам не нужно настраивать пользовательскую роль, убедитесь, что у вас есть разрешения, описанные ниже.

### <a name="permissions-for-pre-existing-automation-account-and-log-analytics-workspace"></a>Разрешения для уже существующей учетной записи службы автоматизации и рабочей области Log Analytics

Для развертывания решения " **Запуск и завершение виртуальных машин в нерабочее время** " в существующей учетной записи службы автоматизации и log Analytics рабочей области пользователь, развертывающий решение, должен иметь следующие разрешения в области группы ресурсов. Дополнительные сведения о ролях см. в статье [пользовательские роли для ресурсов Azure](../role-based-access-control/custom-roles.md).

| Разрешение | Область|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Группа ресурсов |
| Microsoft.Automation/automationAccounts/variables/write | Группа ресурсов |
| Microsoft.Automation/automationAccounts/schedules/write | Группа ресурсов |
| Microsoft.Automation/automationAccounts/runbooks/write | Группа ресурсов |
| Microsoft.Automation/automationAccounts/connections/write | Группа ресурсов |
| Microsoft.Automation/automationAccounts/certificates/write | Группа ресурсов |
| Microsoft.Automation/automationAccounts/modules/write | Группа ресурсов |
| Microsoft.Automation/automationAccounts/modules/read | Группа ресурсов |
| Microsoft. Automation/automationAccounts/Жобсчедулес/запись | Группа ресурсов |
| Microsoft.Automation/automationAccounts/jobs/write | Группа ресурсов |
| Microsoft.Automation/automationAccounts/jobs/read | Группа ресурсов |
| Microsoft.OperationsManagement/solutions/write | Группа ресурсов |
| Microsoft. OperationalInsights/workspaces/* | Группа ресурсов |
| Microsoft. Insights/diagnosticSettings/запись | Группа ресурсов |
| Microsoft.Insights/ActionGroups/Write | Группа ресурсов |
| Microsoft. Insights/Актионграупс/чтение | Группа ресурсов |
| Microsoft.Resources/subscriptions/resourceGroups/read | Группа ресурсов |
| Microsoft.Resources/deployments/* | Группа ресурсов |

### <a name="permissions-for-new-automation-account-and-new-log-analytics-workspace"></a>Разрешения для новой учетной записи службы автоматизации и новой рабочей области Log Analytics

Вы можете развернуть решение **Start/остановки виртуальных машин в нерабочее время** в новой учетной записи службы автоматизации и log Analytics рабочей области. В этом случае пользователь, развертывающий решение, должен обладать разрешениями, определенными в предыдущем разделе, а также разрешениями, определенными в этом разделе. 

Пользователь, развертывающий решение, должен иметь следующие роли:

- Соадминистратор в подписке. Эта роль необходима для создания классической учетной записи запуска от имени, если вы собираетесь управлять классическими виртуальными машинами. [Классические учетные записи запуска от имени](automation-create-standalone-account.md#create-a-classic-run-as-account) больше не создаются по умолчанию.
- Членство в роли разработчика приложения [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) . Дополнительные сведения о настройке учетных записей запуска от имени см. в разделе [разрешения на настройку учетных записей запуска от имени](manage-runas-account.md#permissions).
- Участник подписки или следующие разрешения.

| Разрешение |Область|
| --- | --- |
| Microsoft. Authorization/Operations/Read | Подписка|
| Microsoft.Authorization/permissions/read |Подписка|
| Microsoft.Authorization/roleAssignments/read | Подписка |
| Microsoft.Authorization/roleAssignments/write. | Подписка |
| Microsoft.Authorization/roleAssignments/delete | Подписка |
| Microsoft.Automation/automationAccounts/connections/read | Группа ресурсов |
| Microsoft.Automation/automationAccounts/certificates/read | Группа ресурсов |
| Microsoft.Automation/automationAccounts/write | Группа ресурсов |
| Microsoft.OperationalInsights/workspaces/write | Группа ресурсов |

## <a name="solution-components"></a>Компоненты решения

**Решение для запуска и остановки виртуальных машин в нерабочее время** включает предварительно настроенные модули Runbook, расписания и интеграцию с журналами Azure Monitor. Эти элементы можно использовать для настройки запуска и завершения работы виртуальных машин в соответствии с потребностями бизнеса.

### <a name="runbooks"></a>Модули runbook

В следующей таблице перечислены модули Runbook, которые решение развертывает в учетной записи службы автоматизации. НЕ вносите изменения в код Runbook. Вместо этого напишите собственный runbook для новых функциональных возможностей.

> [!IMPORTANT]
> Не запускайте модуль Runbook напрямую с добавленным **дочерним элементом** к его имени.

Все родительские модули Runbook включают `WhatIf` параметр. Если задано значение true, параметр поддерживает детализацию точного поведения, выполняемого модулем Runbook при запуске без параметра, и проверяет, что целевые виртуальные машины нацелены. Модуль Runbook выполняет только определенные действия, `WhatIf` если параметр имеет значение false.

|Модуль Runbook | Параметры | Описание|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Вызывается из родительского runbook. Этот модуль Runbook создает предупреждения для каждого отдельного ресурса в сценарии автоматической отмены.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: true или false.  | Создает или обновляет правила генерации оповещений Azure для виртуальных машин в целевой подписке или группах ресурсов. <br> `VMList`— Это список виртуальных машин с разделителями-запятыми. Например, `vm1, vm2, vm3`.<br> `WhatIf`включает проверку логики модуля Runbook без выполнения.|
|AutoStop_Disable | Нет | Отключает автоматическое оповещение и расписание по умолчанию.|
|AutoStop_VM_Child | WebHookData | Вызывается из родительского runbook. Правила генерации оповещений вызывают этот модуль Runbook для отключения классической виртуальной машины.|
|AutoStop_VM_Child_ARM | WebHookData |Вызывается из родительского runbook. Правила генерации оповещений вызывают этот модуль Runbook для завершения виртуальной машины.  |
|ScheduledStartStop_Base_Classic; | CloudServiceName<br> Действие: Start или Stop<br> VMList  | Выполняет действие Запуск или завершение действия в классической группе виртуальных машин по облачным службам. |
|ScheduledStartStop_Child | VMName <br> Действие: Start или Stop <br> ResourceGroupName | Вызывается из родительского runbook. Выполняет действие Start или Stop для запланированной остановки.|
|ScheduledStartStop_Child_Classic; | VMName<br> Действие: Start или Stop<br> ResourceGroupName | Вызывается из родительского runbook. Выполняет действие запуска или окончания запланированной отмены для классических виртуальных машин. |
|ScheduledStartStop_Parent | Действие: Start или Stop <br>VMList <br> WhatIf: true или false. | Запускает или останавливает все виртуальные машины в подписке. Измените переменные `External_Start_ResourceGroupNames` и `External_Stop_ResourceGroupNames` для выполнения только в этих целевых группах ресурсов. Можно также исключить определенные виртуальные машины, обновив `External_ExcludeVMNames` переменную.|
|SequencedStartStop_Parent | Действие: Start или Stop <br> WhatIf: true или false.<br>VMList| Создает теги с именами **sequencestart** и **sequencestop** на каждой виртуальной машине, для которой необходимо выполнить последовательность действий запуска и отмены. В именах этих тегов учитывается регистр. Значением тега должно быть положительное целое число (1, 2, 3 и т. д.), которое соответствует очередности запуска или остановки. <br>**Примечание**. виртуальные машины должны находиться в группах ресурсов, `External_Start_ResourceGroupNames`определенных `External_Stop_ResourceGroupNames`в переменных `External_ExcludeVMNames` , и. Чтобы эти действия выполнялись, они должны иметь соответствующие теги.|

### <a name="variables"></a>Переменные

В таблице ниже перечислены переменные, создаваемые в вашей учетной записи службы автоматизации. Измените только переменные с `External`префиксом. Изменение переменных с `Internal` префиксом приводит к нежелательным результатам.

> [!NOTE]
> Ограничения на имя виртуальной машины и группу ресурсов в основном являются результатом переменного размера. См. раздел [ресурсы переменных в службе автоматизации Azure](https://docs.microsoft.com/azure/automation/shared-resources/variables).

|Переменная | Описание|
|---------|------------|
|External_AutoStop_Condition | Условный оператор, необходимый для настройки условия перед активацией оповещения. Допустимые значения `GreaterThan`: `GreaterThanOrEqual`, `LessThan`, и `LessThanOrEqual`.|
|External_AutoStop_Description | Оповещение для остановки виртуальной машины в случае, если процент использования ЦП превышает пороговое значение.|
|External_AutoStop_Frequency | Периодичность оценки для правила. Этот параметр принимает входные данные в формате временного диапазона. Допустимые значения составляют от 5 минут до 6 часов. |
|External_AutoStop_MetricName; | Имя метрики производительности, для которой настраивается правило генерации оповещений Azure.|
|External_AutoStop_Severity | Серьезность оповещения метрики, которое может находиться в диапазоне от 0 до 4. |
|External_AutoStop_Threshold; | Пороговое значение для правила генерации оповещений Azure, `External_AutoStop_MetricName`указанное в переменной. Процентные значения находятся в диапазоне от 1 до 100.|
|External_AutoStop_TimeAggregationOperator; | Оператор агрегирования времени, применяемый к выбранному размеру окна для вычисления условия. Допустимые значения `Average`: `Minimum`, `Maximum`, `Total`, и `Last`.|
|External_AutoStop_TimeWindow. | Размер окна, в течение которого Azure анализирует выбранные метрики для активации предупреждения. Этот параметр принимает входные данные в формате временного диапазона. Допустимые значения составляют от 5 минут до 6 часов.|
|External_EnableClassicVMs| Значение, указывающее, предназначены ли классические виртуальные машины для решения. По умолчанию используется значение True. Присвойте этой переменной значение false для подписок поставщика облачных решений Azure (CSP). Для классических виртуальных машин требуется [Классическая учетная запись запуска от имени](automation-create-standalone-account.md#create-a-classic-run-as-account).|
|External_ExcludeVMNames | Разделенный запятыми список имен виртуальных машин, которые нужно исключить, с ограничением в 140 виртуальных машин. Если добавить в список более 140 виртуальных машин, то виртуальные машины, для которых настроено их исключение, могут быть случайно запущены или остановлены.|
|External_Start_ResourceGroupNames | Список с разделителями-запятыми для одной или нескольких групп ресурсов, предназначенных для начальных действий.|
|External_Stop_ResourceGroupNames | Список с разделителями-запятыми для одной или нескольких групп ресурсов, предназначенных для действий при ошибке.|
|External_WaitTimeForVMRetrySeconds |Время ожидания в секундах для выполнения действий на виртуальных машинах для **SequencedStartStop_Parentного** модуля Runbook. Эта переменная позволяет модулю Runbook ожидать дочерние операции в течение указанного числа секунд, прежде чем переходить к следующему действию. Максимальное время ожидания — 10800 или три часа. Значение по умолчанию — 2100 секунд.|
|Internal_AutomationAccountName | Задает имя учетной записи службы автоматизации Azure.|
|Internal_AutoSnooze_ARM_WebhookURI | URI веб-перехватчика, вызываемый для сценария автозавершения для виртуальных машин.|
|Internal_AutoSnooze_WebhookUri | URI веб-перехватчика, вызываемый для сценария автозавершения для классических виртуальных машин.|
|Internal_AzureSubscriptionId | Идентификатор подписки Azure.|
|Internal_ResourceGroupName | Имя группы ресурсов учетной записи службы автоматизации.|

>[!NOTE]
>Для переменной `External_WaitTimeForVMRetryInSeconds`значение по умолчанию было изменено с 600 на 2100. 

`External_Start_ResourceGroupNames`Во всех сценариях переменные, `External_Stop_ResourceGroupNames`и `External_ExcludeVMNames` необходимы для целевых виртуальных машин, за исключением списков виртуальных машин с разделителями-запятыми для **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent**и **ScheduledStartStop_Parent** модулей Runbook. То есть виртуальные машины должны принадлежать к целевым группам ресурсов для выполнения действий запуска и отмены. Логика работает аналогично политике Azure, в которой можно ориентироваться на подписку или группу ресурсов и выполнять действия, наследуемые только что созданными виртуальными машинами. Такой подход позволяет избежать необходимости использовать отдельное расписание для каждой виртуальной машины и управлять запуском и остановкой в соответствующем масштабе.

### <a name="schedules"></a>Расписания

В таблице ниже перечислены расписания по умолчанию, создаваемые в вашей учетной записи службы автоматизации.Можно изменить их или создать собственные пользовательские расписания.По умолчанию все расписания отключены, за исключением расписаний **Scheduled_StartVM** и **Scheduled_StopVM** .

Не включайте все расписания, так как это может привести к созданию перекрывающихся действий расписания. Лучше определить, какие оптимизации необходимо выполнить, и изменить их соответствующим образом. Дополнительные пояснения можно получить, ознакомившись с примерами сценариев в разделе "Обзор".

|Имя расписания | Частота | Описание|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Каждые 8 часов | Запускает **AutoStop_CreateAlert_Parent** Runbook каждые 8 часов, что, в свою очередь, останавливает значения на основе виртуальных `External_Start_ResourceGroupNames`машин `External_Stop_ResourceGroupNames`в переменных `External_ExcludeVMNames` , и. Кроме того, можно указать список виртуальных машин с разделителями-запятыми `VMList` с помощью параметра.|
|Scheduled_StopVM | Определенный пользователем, ежедневно | Запускает **ScheduledStopStart_Parent** Runbook с параметром `Stop` каждый день в указанное время.Автоматически останавливает все виртуальные машины, соответствующие правилам, определенным в переменных ресурсах.Включите связанное расписание Scheduled **-StartVM**.|
|Scheduled_StartVM | Определенный пользователем, ежедневно | Запускает **ScheduledStopStart_Parent** Runbook со значением параметра `Start` каждый день в указанное время. Автоматически запускает все виртуальные машины, соответствующие правилам, определенным в переменных ресурсах.Включите связанное расписание Scheduled **-StopVM**.|
|Sequenced-StopVM | 01:00 (UTC), каждую пятницу | Запускает **Sequenced_StopStop_Parent** Runbook со значением параметра `Stop` каждую пятницу в указанное время.Последовательно (по возрастанию) останавливает все виртуальные машины с тегом **SequenceStop**, которые указаны в соответствующих переменных. Дополнительные сведения о значениях тегов и переменных ресурсов см. в разделе [модули Runbook](#runbooks).Включите связанное расписание **Sequenced-StartVM**.|
|Sequenced-StartVM | 13:00 (UTC), каждый понедельник | Запускает **SequencedStopStart_Parent** Runbook со значением параметра `Start` каждый понедельник в указанное время. Последовательно (по убыванию) запускает все виртуальные машины с тегом **SequenceStart**, которые указаны в соответствующих переменных. Дополнительные сведения о значениях тегов и ресурсах переменных см. в разделе [модули Runbook](#runbooks). Включите связанное расписание **Sequenced-StopVM**.

## <a name="use-of-the-solution-with-classic-vms"></a>Использование решения с классическими виртуальными машинами

Если вы используете решение " **Запуск и завершение виртуальных машин в нерабочее время** " для классических виртуальных машин, служба автоматизации обрабатывает все виртуальные машины последовательно в каждой облачной службе. Виртуальные машины по-прежнему обрабатываются параллельно между разными облачными службами. 

Для использования решения с классическими виртуальными машинами необходима Классическая учетная запись запуска от имени, которая не создается по умолчанию. Инструкции по созданию классической учетной записи запуска от имени см. в статье [Создание классической учетной записи запуска от имени](automation-create-standalone-account.md#create-a-classic-run-as-account).

При наличии более 20 виртуальных машин на одну облачную службу ниже приведены некоторые рекомендации.

* Создайте несколько расписаний с родительским модулем Runbook **ScheduledStartStop_Parent** и укажите 20 виртуальных машин по расписанию. 
* В свойствах расписания используйте `VMList` параметр, чтобы указать имена виртуальных машин в виде списка с разделителями-запятыми. 

В противном случае, если задание службы автоматизации для этого решения выполняется более трех часов, оно будет временно выгружено или остановлено в течение предельного количества [ресурсов](automation-runbook-execution.md#fair-share) .

Подписки Azure CSP поддерживают только модель Azure Resource Manager. Службы, не относящиеся к Azure Resource Manager, недоступны в программе. При запуске **решения "Запуск и завершение виртуальных машин в нерабочее время** " могут возникать ошибки, так как она содержит командлеты для управления классическими ресурсами. Дополнительные сведения о CSP см. в разделе [Комментарии](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services). При использовании подписки CSP следует присвоить переменной [External_EnableClassicVMs](#variables) значение false после развертывания.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-the-solution"></a>Включение решения

Чтобы приступить к использованию решения, выполните действия, описанные в разделе [Включение виртуальной машины для запуска и отмены виртуальных машин](automation-solution-vm-management-enable.md).

## <a name="view-the-solution"></a>Просмотр решения

Используйте один из следующих механизмов для доступа к решению после его включения:

* В учетной записи службы автоматизации выберите **Пуск/останавливает виртуальную машину** в разделе **связанные ресурсы**. На странице Запуск и завершение виртуальной машины выберите **Управление решением** в правой части страницы в разделе **Управление решениями для запуска и отмены виртуальных машин**.

* Перейдите в рабочую область Log Analytics, связанную с учетной записью службы автоматизации. После выбора рабочей области выберите **решения** в левой области. На странице решения выберите решение **Запуск-останавливает — виртуальная машина [Рабочая область]** из списка.  

При выборе решения отобразится страница решения **Start-Stop-VM[рабочая_область]**. Здесь можно просматривать важные сведения, например сведения на плитке **startstopvm View (** . Как и в рабочей области Log Analytics, в нем отображаются сведения о количестве заданий runbook, которые были запущены и завершены для решения, а также их графическое представление.

![Страница обновления решения по управлению в службе автоматизации](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Вы можете выполнить дальнейший анализ записей задания, щелкнув плитку кольца. На панели мониторинга решения отображаются журнал заданий и предопределенные запросы поиска по журналам. Перейдите на расширенный портал log Analytics для поиска в соответствии с поисковыми запросами.

## <a name="update-the-solution"></a>Обновление решения

Если вы развернули предыдущую версию этого решения, удалите его из учетной записи перед развертыванием обновленного выпуска. Выполните действия, чтобы [удалить решение](#remove-the-solution) , а затем выполните действия по [развертыванию решения](automation-solution-vm-management-enable.md).

## <a name="remove-the-solution"></a>Удаление решения

Если вам больше не нужно использовать решение, его можно удалить из учетной записи службы автоматизации. При этом удаляются только модули runbook. Расписания или переменные, которые были созданы при добавлении решения, не удаляются. Удалите эти ресурсы вручную, если они не используются с другими модулями Runbook.

Чтобы удалить решение, выполните следующие действия.

1. В учетной записи службы автоматизации выберите в разделе **связанные ресурсы**пункт **связанная Рабочая область** .

2. Выберите **Переход к рабочей области**.

3. В разделе **Общие**щелкните **решения** . 

4. На странице Решения выберите решение **Start-Stop-VM[Workspace]**. 

5. На странице **VMManagementSolution [Рабочая область]** выберите в меню пункт **Удалить** .<br><br> ![Удаление решения VMManagement](media/automation-solution-vm-management/vm-management-solution-delete.png)

6. В окне **Удаление решения** подтвердите, что хотите удалить решение.

7. При проверке сведений и удалении решения можно отслеживать ход выполнения в разделе **уведомления**, выбранные в меню. После запуска процесса удаления решения вы вернетесь на страницу решений.

В ходе этого процесса рабочая область Log Analytics и учетная запись службы автоматизации не удаляются. Если вы не хотите использовать рабочую область Log Analytics, необходимо вручную удалить ее из портал Azure:

1. Найдите и выберите **log Analytics рабочие области**.

2. На странице Log Analytics Рабочая область выберите рабочую область.

3. В меню, расположенном на странице параметров рабочей области, выберите **Удалить**.

4. Если вы не хотите использовать [компоненты решения](#solution-components)для учетной записи службы автоматизации Azure, их можно удалить вручную.

## <a name="next-steps"></a>Дальнейшие шаги

[Включите](automation-solution-vm-management-enable.md) решение " **Запуск и завершение виртуальных машин в нерабочее время** " для виртуальных машин Azure.
