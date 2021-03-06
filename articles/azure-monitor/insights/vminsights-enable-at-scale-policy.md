---
title: Включение Azure Monitor для виртуальных машин с помощью Политики Azure
description: В этой статье описывается, как включить Azure Monitor для виртуальных машин для нескольких виртуальных машин Azure или масштабируемых наборов виртуальных машин с помощью политики Azure.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 73c18d45136eea90ad29dc1bd40c4539dddc0ee6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81767257"
---
# <a name="enable-azure-monitor-for-vms-by-using-azure-policy"></a>Включение Azure Monitor для виртуальных машин с помощью Политики Azure

В этой статье объясняется, как включить Azure Monitor для виртуальных машин для виртуальных машин Azure или масштабируемых наборов виртуальных машин с помощью политики Azure. В конце этого процесса вы успешно настроили включение Log Analytics и агентов зависимостей и идентифицированных виртуальных машин, которые не соответствуют требованиям.

Для обнаружения, управления и включения Azure Monitor для виртуальных машин для всех виртуальных машин Azure или масштабируемых наборов виртуальных машин можно использовать либо политику Azure, либо Azure PowerShell. Мы рекомендуем использовать политику Azure, так как вы можете управлять определениями политик для эффективного управления подписками, чтобы обеспечить соответствие требованиям и автоматически включать новые подготовленные виртуальные машины. Эти определения политик:

* Развернуть агент Log Analytics и Dependency Agent.
* Создать отчет о результатах проверки на соответствие.
* Исправление для несоответствующих виртуальных машин.

Если вы заинтересованы в выполнении этих задач с помощью Azure PowerShell или шаблона Azure Resource Manager, см. статью [включение Azure Monitor для виртуальных машин с помощью Azure PowerShell или Azure Resource Manager шаблонов](vminsights-enable-at-scale-powershell.md).

## <a name="prerequisites"></a>Предварительные условия
Прежде чем использовать политику для подключения виртуальных машин Azure и масштабируемых наборов виртуальных машин к службе мониторинга Azure для виртуальных машин, необходимо включить решение Вминсигхтс в рабочей области, которая будет использоваться для хранения данных мониторинга. Эту задачу можно выполнить на странице Начало **работы** в Azure Monitor на вкладке **другие параметры адаптации** .  Выберите **настроить рабочую область**, после чего будет предложено выбрать рабочую область для настройки.

![Настройка рабочей области](media/vminsights-enable-at-scale-policy/configure-workspace.png)

Вы также можете настроить рабочую область, выбрав **включить с помощью политики** , а затем нажав кнопку **Настройка рабочей области** на панели инструментов.  Будет установлено решение Вминсигхтс в выбранной рабочей области, которое позволит рабочей области хранить данные мониторинга, отправленные виртуальными машинами и масштабируемыми наборами виртуальных машин, которые можно включить с помощью политики. 

![Включить использование политики](media/vminsights-enable-at-scale-policy/enable-using-policy.png)

## <a name="manage-policy-coverage-feature-overview"></a>Обзор возможностей управления покрытием политик

Azure Monitor для виртуальных машин политика позволяет упростить обнаружение, управление и включение в масштабе действия " **включить Azure Monitor для виртуальных машин** ", включая определения политик, упомянутые ранее. Чтобы получить доступ к этой функции, выберите **другие параметры адаптации** на вкладке " **Начало работы** " в Azure Monitor для виртуальных машин. Выберите **Управление покрытием политики** , чтобы открыть страницу " **покрытие Azure Monitor для виртуальных машин политик** ".

![Вкладка "Начало работы" Azure Monitor с виртуальных машин](./media/vminsights-enable-at-scale-policy/get-started-page.png)

Здесь вы можете проверять покрытие инициативы и управлять им во всех группах управления и подписках. Вы можете понять, сколько виртуальных машин существует в каждой из групп управления и подписок, а также о их состоянии соответствия.

![Страница "Управление политикой Azure Monitor для виртуальных машин"](media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Эти сведения помогут вам спланировать и выполнить сценарий управления для Azure Monitor для виртуальных машин из одного центрального расположения. Хотя политика Azure обеспечивает представление соответствия, когда политика или инициатива назначены области, на этой новой странице можно обнаружить, где не назначена политика или инициатива, и назначить их на месте. Все такие действия, как назначение, просмотр и изменение прямого перенаправления в политику Azure. Страница " **покрытие политики Azure Monitor для виртуальных машин** " — это расширенный и интегрированный интерфейс, предназначенный только для инициативы, которые **включают Azure Monitor для виртуальных машин**.

На этой странице можно также настроить рабочую область Log Analytics для Azure Monitor для виртуальных машин, которая:

- Устанавливает Сопоставление служб решение.
- Включает счетчики производительности операционной системы, используемые диаграммами производительности, книгами и пользовательскими запросами и оповещениями журналов.

![Настройка рабочей области Azure Monitor для виртуальных машин](media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Этот параметр не связан ни с одним действием политики. Он доступен для обеспечения простого способа удовлетворения [необходимых условий](vminsights-enable-overview.md) , необходимых для включения Azure Monitor для виртуальных машин.  

### <a name="what-information-is-available-on-this-page"></a>Какие сведения доступны на этой странице?

В следующей таблице приведена информация, представленная на странице "покрытие политики" и способах ее интерпретации.

| Функция | Описание | 
|----------|-------------| 
| **Область** | Группа управления и подписки, к которым вы имеете доступ или которые наследуются с возможностью детализации по иерархии группы управления.|
| **Роль** | Роль в области, которая может быть читателя, владельцем или участником. В некоторых случаях он может быть пустым, чтобы указать, что у вас может быть доступ к подписке, но не к группе управления, к которой он принадлежит. Сведения в других столбцах зависят от конкретной роли. Роль является ключевым определением того, какие данные вы видите и какие действия можно выполнять с точки зрения назначения политик или инициатив (владельца), их редактирования или просмотра соответствия. |
| **Всего виртуальных машин** | Число виртуальных машин в этой области. Для группы управления это сумма виртуальных машин, вложенных в подписки или дочернюю группу управления. |
| **Покрытие назначений** | Процент виртуальных машин, охваченных политикой или инициативой. |
| **Состояние назначения** | Сведения о состоянии вашей политики или назначения инициативы. |
| **Совместимые виртуальные машины** | Число виртуальных машин, соответствующих политике или инициативе. Для включения инициативы **Azure Monitor для виртуальных машин**это число виртуальных машин, у которых есть агент log Analytics и агент зависимостей. В некоторых случаях она может быть пустой из-за отсутствия назначения, отсутствия виртуальных машин или отсутствия достаточных разрешений. Сведения предоставляются в разделе **состояние соответствия**. |
| **Соответствие** | Общий номер соответствия — это сумма отдельных ресурсов, которые соответствуют сумме всех различных ресурсов. |
| **Состояние соответствия** | Сведения о состоянии соответствия для политики или назначения инициативы.|

При назначении политики или инициативы область, выбранная в назначении, может быть указанной областью или ее подмножеством. Например, вы могли создать назначение для подписки (область политики), а не группы управления (область охвата). В этом случае значение **покрытия** назначений указывает на виртуальные машины в области политики или инициативы, разделенные виртуальными машинами в области покрытия. В другом случае вы могли исключить некоторые виртуальные машины, группы ресурсов или подписку из области политики. Если значение пустое, это означает, что политика или инициатива не существует или у вас нет разрешения. Сведения предоставляются в разделе **состояние назначения**.

## <a name="enable-by-using-azure-policy"></a>Включение при помощи Политики Azure

Чтобы включить Azure Monitor для виртуальных машин с помощью Политики Azure в своем арендаторе, выполните следующие действия:

- Назначьте инициативу область: Группа управления, подписка или группа ресурсов.
- Проверка и исправление результатов проверки соответствия.

Дополнительные сведения о назначении Политики Azure см. в [обзоре Политики Azure](../../governance/policy/overview.md#assignments). Кроме того, прежде чем продолжить, ознакомьтесь с [обзором групп управления](../../governance/management-groups/overview.md).

### <a name="policies-for-azure-vms"></a>Политики для виртуальных машин Azure

Определения политик для виртуальной машины Azure перечислены в следующей таблице.

|Имя |Описание |Type |
|-----|------------|-----|
|Включение Azure Monitor для виртуальных машин |Включите Azure Monitor для виртуальных машин в указанной области (Группа управления, подписка или группа ресурсов). В качестве параметра принимается рабочая область Log Analytics. |Инициатива |
|Аудит развертывания агента зависимостей — образ виртуальной машины (ОС), который не был в списке |Сообщает о несоответствии виртуальных машин, если образ виртуальной машины (ОС) не определен в списке и агент не установлен. |Политика |
|Аудит Log Analytics развертывания агента — образ виртуальной машины (ОС), который не был в списке |Сообщает о несоответствии виртуальных машин, если образ виртуальной машины (ОС) не определен в списке и агент не установлен. |Политика |
|Развертывание Dependency Agent для виртуальных машин Linux |Разверните агент зависимостей для виртуальных машин Linux, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |Политика |
|Развертывание Dependency Agent для виртуальных машин Windows |Развертывание агента зависимостей для виртуальных машин Windows, если образ виртуальной машины (ОС) определен в списке и агент не установлен. |Политика |
|Развертывание агента Log Analytics для виртуальных машин Linux |Развертывание агента Log Analytics для виртуальных машин Linux, если образ виртуальной машины (ОС) определен в списке и агент не установлен. |Политика |
|Развертывание агента Log Analytics для виртуальных машин Windows |Развертывание агента Log Analytics для виртуальных машин Windows, если образ виртуальной машины (ОС) определен в списке и агент не установлен. |Политика |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Политики для масштабируемых наборов виртуальных машин Azure

Определения политик для масштабируемого набора виртуальных машин Azure перечислены в следующей таблице.

|Имя |Описание |Type |
|-----|------------|-----|
|Включение Azure Monitor для масштабируемых наборов виртуальных машин |Включите Azure Monitor для масштабируемых наборов виртуальных машин в указанной области (Группа управления, подписка или группа ресурсов). В качестве параметра принимается рабочая область Log Analytics. Примечание. Если для политики обновления масштабируемого набора задано значение вручную, примените расширение ко всем виртуальным машинам в наборе, вызвав для них обновление. В CLI это `az vmss update-instances`. |Инициатива |
|Аудит развертывания агента зависимостей в масштабируемых наборах виртуальных машин — образ виртуальной машины (ОС), который не был в списке |Сообщает о том, что масштабируемый набор виртуальных машин не соответствует требованиям, если образ ВИРТУАЛЬНОЙ машины (ОС) не определен в списке и агент не установлен. |Политика |
|Аудит развертывания агента Log Analytics в масштабируемых наборах виртуальных машин — образ виртуальной машины (ОС), который не был в списке |Сообщает о том, что масштабируемый набор виртуальных машин не соответствует требованиям, если образ ВИРТУАЛЬНОЙ машины (ОС) не определен в списке и агент не установлен. |Политика |
|Развертывание Dependency Agent для масштабируемых наборов виртуальных машин Linux |Развертывание агента зависимостей для масштабируемых наборов виртуальных машин Linux, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |Политика |
|Развертывание Dependency Agent для масштабируемых наборов виртуальных машин Windows |Развертывание агента зависимостей для масштабируемых наборов виртуальных машин Windows, если образ ВИРТУАЛЬНОЙ машины (ОС) определен в списке, а агент не установлен. |Политика |
|Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Linux |Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Linux, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |Политика |
|Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Windows |Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Windows, если образ ВИРТУАЛЬНОЙ машины (ОС) определен в списке, а агент не установлен. |Политика |

Автономная политика (не включена в инициативу) описана здесь:

|Имя |Описание |Type |
|-----|------------|-----|
|Log Analytics рабочей области аудита для ВМ — несоответствие отчетов |Сообщать о несоответствующих виртуальных машинах, если они не входят в рабочую область Log Analytics, указанную в политике или назначении инициативы. |Политика |

### <a name="assign-the-azure-monitor-initiative"></a>Назначение инициативы Azure Monitor

Чтобы создать назначение политики на странице " **покрытие Azure Monitor для виртуальных машин политик** ", выполните следующие действия. Чтобы понять, как выполняются эти шаги, изучите статью  [Создание назначения политики для идентификации ресурсов, не соответствующих требованиям, в среде Azure](../../governance/policy/assign-policy-portal.md).

При назначении политики или инициативы область, выбранная в назначении, может быть указанной здесь областью или ее подмножеством. Например, вы могли создать назначение для подписки (область политики), а не группы управления (область охвата). В этом случае процент покрытия указывает виртуальным машинам в области политики или инициативе, деленную на виртуальные машины в области охвата. В другом случае вы могли исключить некоторые виртуальные машины, группы ресурсов или подписку из области политики. Если оно пустое, это означает, что политика или инициатива не существует или у вас нет разрешений. Сведения предоставляются в разделе **состояние назначения**.

1. Войдите на [портал Azure](https://portal.azure.com).

2. В портал Azure выберите **мониторинг**. 

3. Выберите **виртуальные машины** в разделе **Insights** .
 
4. Перейдите на вкладку **Начало работы** . На странице выберите **Управление покрытием политики**.

5. Выберите либо группу управления, либо подписку из таблицы. Выберите **область** , нажав кнопку с многоточием (...). В этом примере область ограничивает назначение политики группированием виртуальных машин для принудительного применения.

6. На странице **назначения политики Azure** предварительно заполняется инициатива **включить Azure Monitor для виртуальных машин**. 
    Поле **имя назначения** автоматически заполняется именем инициативы, но его можно изменить. Кроме того, можно добавить необязательное описание. Поле **назначено** автоматически заполняется в зависимости от того, кто вошел в систему. Это значение является необязательным.

7. Чтобы удалить один или несколько ресурсов из области, выберите **Исключения** (необязательно).

8. В раскрывающемся списке **Рабочая область Log Analytics** для поддерживаемого региона выберите рабочую область.

   > [!NOTE]
   > Если рабочая область находится за пределами области назначения, предоставьте разрешения *участника Log Analytics* для идентификатора участника назначения политики. Если этого не сделать, может отобразиться ошибка развертывания, например `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` , чтобы предоставить доступ, а также [настроить управляемое удостоверение вручную](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  Установлен флажок **управляемое удостоверение** , так как назначаемая инициатива включает политику с *deployIfNotExistsным* результатом.
    
9. В раскрывающемся списке **Manage Identity location** (Расположение для управления удостоверениями) выберите соответствующий регион.

10. Выберите **назначить**.

После создания назначения страница " **покрытие политики" Azure Monitor для виртуальных машин** обновляет **покрытие назначения**, **состояние назначения**, **соответствующие виртуальные машины**и **состояние соответствия** , чтобы отразить изменения. 

Следующая матрица сопоставляет каждое возможное состояние соответствия для инициативы.  

| Состояние соответствия | Описание | 
|------------------|-------------|
| **Соответствует** | На всех виртуальных машинах в области разворачиваются Log Analytics и агенты зависимостей.|
| **Не соответствует** | Не все виртуальные машины в области имеют развернутые на них Log Analytics и агенты зависимостей и могут потребовать исправления.|
| **Не запущено** | Добавлено новое назначение. |
| **Скрыть** | У вас недостаточно прав доступа к группе управления. <sup>1</sup> | 
| **Пустой** | Политика не назначена. | 

<sup>1</sup> если у вас нет доступа к группе управления, попросите владельца предоставить доступ. Или просмотрите сведения о соответствии и управляйте назначениями с помощью дочерних групп управления или подписок. 

В следующей таблице сопоставлены все возможные состояния назначения для инициативы.

| Состояние назначения | Описание | 
|------------------|-------------|
| **Success** | На всех виртуальных машинах в области разворачиваются Log Analytics и агенты зависимостей.|
| **!** | Подписка не входит в группу управления.|
| **Не запущено** | Добавлено новое назначение. |
| **Скрыть** | У вас недостаточно прав доступа к группе управления. <sup>1</sup> | 
| **Пустой** | Виртуальные машины не существуют, или политика не назначена. | 
| **Действие** | Назначение политики или изменение назначения. | 

<sup>1</sup> если у вас нет доступа к группе управления, попросите владельца предоставить доступ. Или просмотрите сведения о соответствии и управляйте назначениями с помощью дочерних групп управления или подписок.

## <a name="review-and-remediate-the-compliance-results"></a>Просмотр результатов проверки на соответствие и внесение исправлений

Ниже приведен пример для ВИРТУАЛЬНОЙ машины Azure, но он также применяется к масштабируемым наборам виртуальных машин. Сведения о том, как просмотреть результаты проверки соответствия, см. в разделе [Определение результатов несоответствия](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). На странице " **покрытие Azure Monitor для виртуальных машин политик** " выберите либо группу управления, либо подписку из таблицы. Выберите **Просмотреть соответствие** , нажав кнопку с многоточием (...).   

![Соответствие политикам для виртуальных машин Azure](./media/vminsights-enable-at-scale-policy/policy-view-compliance.png)

В зависимости от результатов политик, включенных в инициативу, виртуальные машины сообщаются в следующих сценариях как несоответствующие.

* Агент Log Analytics или агент зависимостей не развернут.  
    Этот сценарий характерен для области с существующими виртуальными машинами. Чтобы устранить эту неполадку, разверните необходимые агенты, [создав задачи исправления](../../governance/policy/how-to/remediate-resources.md) для политики, не соответствующей политике.  
    - Развертывание Dependency Agent для виртуальных машин Linux
    - Развертывание Dependency Agent для виртуальных машин Windows
    - Развертывание агента Log Analytics для виртуальных машин Linux
    - Развертывание агента Log Analytics для виртуальных машин Windows

* Образ виртуальной машины (ОС) не определен в определении политики.  
    Условия политики развертывания включают в себя только виртуальные машины, развернутые на основе известных образов виртуальных машин Azure. Проверьте документацию, чтобы узнать, поддерживается ли ОС виртуальных машин. Если ОС не поддерживается, создайте копию политики развертывания и измените ее таким образом, чтобы образ соответствовал ее требованиям.  
    - Аудит развертывания агента зависимостей — образ виртуальной машины (ОС), который не был в списке
    - Аудит Log Analytics развертывания агента — образ виртуальной машины (ОС), который не был в списке

* Виртуальные машины не выполняют вход в указанную рабочую область Log Analytics.  
    Возможно, некоторые виртуальные машины в области инициативы выполняют вход в рабочую область Log Analytics, отличную от той, которая указана в назначении политики. Эта политика позволяет выяснить, какие виртуальные машины сообщают о несоответствующей рабочей области.  
    - Log Analytics рабочей области аудита для ВМ — несоответствие отчетов

## <a name="edit-an-initiative-assignment"></a>Изменение назначения инициативы

В любое время после назначения инициативы группе управления или подписке его можно изменить, чтобы изменить следующие свойства.

- Имя назначения
- Описание
- Кем назначено
- Рабочая область Log Analytics
- Исключения

## <a name="next-steps"></a>Дальнейшие шаги

Теперь, когда наблюдение включено для виртуальных машин, эти сведения доступны для анализа с Azure Monitor для виртуальных машин. 

- Для просмотра обнаруженных зависимостей приложений см. статью о [просмотре схемы Azure Monitor для виртуальных машин](vminsights-maps.md). 

- Чтобы определить узкие места и общее использование с производительностью виртуальной машины, см. статью [Просмотр производительности виртуальной машины Azure](vminsights-performance.md). 
