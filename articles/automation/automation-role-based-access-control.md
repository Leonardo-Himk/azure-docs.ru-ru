---
title: Управление доступом на основе ролей в службе автоматизации Azure
description: Контроль доступа на основе ролей (RBAC) Azure обеспечивает управление доступом к ресурсам Azure. В этой статье описывается настройка RBAC в службе автоматизации Azure.
keywords: автоматизация RBAC, контроль доступа на основе ролей, RBAC Azure
services: automation
ms.subservice: shared-capabilities
ms.date: 05/17/2018
ms.topic: conceptual
ms.openlocfilehash: a49f2596df91c44deafa1be83483f8972e223742
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81535576"
---
# <a name="role-based-access-control-in-azure-automation"></a>Управление доступом на основе ролей в службе автоматизации Azure

Контроль доступа на основе ролей (RBAC) Azure обеспечивает управление доступом к ресурсам Azure. С помощью [RBAC](../role-based-access-control/overview.md) вы сможете распределить обязанности внутри своей команды и предоставить доступ пользователям, группам и приложениям на том уровне, который им необходим для выполнения поставленных задач. Вы можете предоставить пользователям доступ на основе ролей, используя портал Azure, средства командной строки Azure или API управления Azure.

>[!NOTE]
>Эта статья была изменена и теперь содержит сведения о новом модуле Az для Azure PowerShell. Вы по-прежнему можете использовать модуль AzureRM, исправления ошибок для которого будут продолжать выпускаться как минимум до декабря 2020 г. Дополнительные сведения о совместимости модуля Az с AzureRM см. в статье [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Знакомство с новым модулем Az для Azure PowerShell). Инструкции по установке модуля Az в гибридной рабочей роли Runbook см. в статье об [установке модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Чтобы обновить модули в учетной записи службы автоматизации, см. руководство по [обновлению модулей Azure PowerShell в службе автоматизации Azure](automation-update-azure-modules.md).

## <a name="roles-in-automation-accounts"></a>Роли в учетных записях службы автоматизации

В службе автоматизации Azure доступ предоставляется путем назначения соответствующей роли RBAC пользователям, группам и приложениям в области учетной записи автоматизации. Ниже перечислены встроенные роли, поддерживаемые учетной записью автоматизации.

| **Роль** | **Описание** |
|:--- |:--- |
| Владелец |Роль владельца обеспечивает доступ ко всем ресурсам и действиям в учетной записи службы автоматизации, включая предоставление доступа на управление учетной записью службы автоматизации другим пользователям, группам и приложениям. |
| Участник |Роль участника позволяет управлять всем, кроме изменения разрешений других пользователей на доступ к учетной записи службы автоматизации. |
| Читатель |Роль читателя позволяет просматривать все ресурсы в учетной записи службы автоматизации, но не дает возможность вносить какие-либо изменения. |
| Оператор службы автоматизации |Роль оператора службы автоматизации позволяет просматривать имя и свойства модуля runbook, а также создавать задания и управлять ими для всех модулей runbook в учетной записи службы автоматизации. Эта роль полезна, если вы хотите защитить ресурсы учетной записи службы автоматизации, такие как активы учетных данных и модули Runbook, из просмотра или изменения, но по-прежнему разрешить членам вашей организации выполнять эти модули Runbook. |
|Оператор заданий службы автоматизации|Роль оператора заданий службы автоматизации позволяет создавать задания и управлять ими для всех модулей runbook в учетной записи службы автоматизации.|
|Оператор Runbook службы автоматизации|Роль оператора runbook службы автоматизации позволяет просматривать имя и свойства модуля runbook.|
| участник Log Analytics. | Участник Log Analytics может считывать все данные мониторинга и изменять его параметры. Изменение параметров мониторинга подразумевает добавление расширений в виртуальные машины, чтение ключей учетной записи хранения для настройки коллекции журналов в службе хранилища Microsoft Azure, создание и настройку учетных записей службы автоматизации, добавление решений и настройку диагностики Azure во всех ресурсах Azure.|
| читатель Log Analytics; | Читатель Log Analytics может просматривать параметры мониторинга, выполнять поиск всех данных мониторинга и просматривать эти данные. Читатель также может просматривать конфигурации диагностики Azure на всех ресурсах Azure. |
| Monitoring Contributor | Участник мониторинга может считывать все данные мониторинга и изменять его параметры.|
| Роль Monitoring Reader | Читатель мониторинга может считывать все данные мониторинга. |
| Администратор доступа пользователей |Роль администратора доступа пользователей позволяет управлять доступом пользователей к учетным записям службы автоматизации Azure. |

## <a name="role-permissions"></a>Разрешения ролей

В следующих таблицах описываются разрешения, предоставленные каждой роли. Это могут быть свойства Actions, которые предоставляют разрешения, и свойства NotActions, которые их ограничивают.

### <a name="owner"></a>Владелец

Владелец может управлять всем, включая доступ. В следующей таблице показаны разрешения, предоставленные для этой роли.

|Действия|Описание|
|---|---|
|Microsoft.Automation/automationAccounts/|Создание ресурсов всех типов и управление ими.|

### <a name="contributor"></a>Участник

Участник может управлять всем, кроме доступа. В следующей таблице показаны разрешения, которые предоставлены и запрещены для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Создание ресурсов всех типов и управление ими|
|**Запрещенные действия**||
|Microsoft.Authorization/*/Delete| Удаление и назначение ролей.       |
|Microsoft.Authorization/*/Write     |  Создание и назначение ролей.       |
|Microsoft.Authorization/elevateAccess/Action    | Запрет создания администратора доступа пользователей.       |

### <a name="reader"></a>Читатель

Читатель может просматривать все ресурсы в учетной записи службы автоматизации, но не может вносить какие-либо изменения.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Просмотр всех ресурсов в учетной записи службы автоматизации. |

### <a name="automation-operator"></a>Оператор службы автоматизации

Оператор службы автоматизации может создавать задания и управлять ими, а также просматривать имена и свойства всех модулей runbook в учетной записи службы автоматизации.  Примечание. Если вы хотите контролировать доступ операторов к отдельным модулям runbook, не используйте эту роль, а вместо нее назначьте роли "Оператор заданий службы автоматизации" и "Оператор runbook службы автоматизации". В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Authorization/*/read|Авторизация на чтение.|
|Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read|Считывает ресурсы гибридной рабочей роли Runbook.|
|Microsoft.Automation/automationAccounts/jobs/read|Вывод списка заданий Runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Возобновление приостановленного задания.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Отмена выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Чтение потоков и выходных данных задания.|
|Microsoft.Automation/automationAccounts/jobs/output/read|Возвращает выходные данные задания.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Остановка выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/write|Создание заданий.|
|Microsoft.Automation/automationAccounts/jobSchedules/read|Возвращает расписание заданий службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/jobSchedules/write|Создает расписание заданий службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/linkedWorkspace/read|Получите рабочую область, связанную с учетной записью службы автоматизации.|
|Microsoft.Automation/automationAccounts/read|Возвращает учетную запись службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/runbooks/read|Возвращает runbook службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/schedules/read|Возвращает ресурс расписания службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/schedules/write|Создает или обновляет ресурс расписания службы автоматизации Azure.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Чтение ролей и их назначений.         |
|Microsoft.Resources/deployments/*      |Создание развертываний группы ресурсов и управление ими.         |
|Microsoft.Insights/alertRules/*      | Создание правил оповещения и управление ими.        |
|Microsoft.Support/* |Создание запросов в службу поддержки и управление ими.|

### <a name="automation-job-operator"></a>Оператор заданий службы автоматизации

Роль оператора заданий службы автоматизации предоставляется на уровне учетной записи службы автоматизации.Она предоставляет разрешения на создание заданий и управление ими для всех модулей runbook в учетной записи. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Authorization/*/read|Авторизация на чтение.|
|Microsoft.Automation/automationAccounts/jobs/read|Вывод списка заданий Runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Возобновление приостановленного задания.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Отмена выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Чтение потоков и выходных данных задания.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Остановка выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/write|Создание заданий.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Чтение ролей и их назначений.       |
|Microsoft.Resources/deployments/*      |Создание развертываний группы ресурсов и управление ими.         |
|Microsoft.Insights/alertRules/*      | Создание правил оповещения и управление ими.        |
|Microsoft.Support/* |Создание запросов в службу поддержки и управление ими.|

### <a name="automation-runbook-operator"></a>Оператор Runbook службы автоматизации

Роль оператора Runbook службы автоматизации предоставляется в области Runbook. Оператор runbook службы автоматизации может просматривать имя и свойства модуля runbook.Эта роль в сочетании с ролью "Оператор заданий службы автоматизации" позволяет создавать задания и управлять ими для модуля runbook. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Вывод списка Runbook.        |
|Microsoft.Authorization/*/read      | Авторизация на чтение.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Чтение ролей и их назначений.         |
|Microsoft.Resources/deployments/*      | Создание развертываний группы ресурсов и управление ими.         |
|Microsoft.Insights/alertRules/*      | Создание правил оповещения и управление ими.        |
|Microsoft.Support/*      | Создание запросов в службу поддержки и управление ими.        |

### <a name="log-analytics-contributor"></a>участник Log Analytics.

Участник Log Analytics может читать все данные мониторинга и изменять его параметры. Изменение параметров мониторинга подразумевает добавление расширений в виртуальные машины, чтение ключей учетной записи хранения для настройки коллекции журналов в службе хранилища Azure, создание и настройку учетных записей службы автоматизации, добавление решений и настройку диагностики Azure во всех ресурсах Azure. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.Automation/automationAccounts/*|Управление учетными записями службы автоматизации.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Создание расширений виртуальных машин и управление ими.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Вывод списка ключей классической учетной записи хранения.|
|Microsoft.Compute/virtualMachines/extensions/*|Создание расширений классических виртуальных машин и управление ими.|
|Microsoft.Insights/alertRules/*|Чтение, запись и удаление правила генерации оповещений.|
|Microsoft.Insights/diagnosticSettings/*|Чтение, запись и удаление параметров диагностики.|
|Microsoft.OperationalInsights/*|Управление журналами Azure Monitor.|
|Microsoft.OperationsManagement/*|Управление решениями в рабочих пространствах.|
|Microsoft.Resources/deployments/*|Создание развертываний группы ресурсов и управление ими.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Создание развертываний группы ресурсов и управление ими.|
|Microsoft.Storage/storageAccounts/listKeys/action|Составление списка ключей учетной записи хранения.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими.|

### <a name="log-analytics-reader"></a>читатель Log Analytics;

Читатель Log Analytics может просматривать и искать все данные мониторинга, а также просматривать его параметры, в том числе конфигурацию Диагностики Azure во всех ресурсах Azure. В следующей таблице показаны разрешения, предоставленные или запрещенные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Управление запросами в журналах Azure Monitor.|
|Microsoft.OperationalInsights/workspaces/search/action|Поиск Azure Monitor данных журнала.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими.|
|**Запрещенные действия**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Чтение ключей общего доступа запрещено.|

### <a name="monitoring-contributor"></a>Monitoring Contributor

Участник мониторинга может читать все данные мониторинга и изменять его параметры. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.AlertsManagement/alerts/*|Управление предупреждениями.|
|Microsoft.AlertsManagement/alertsSummary/*|Управление панелью мониторинга оповещений.|
|Microsoft.Insights/AlertRules/*|Управление правилами генерации оповещений.|
|Microsoft.Insights/components/*|Управление компонентами Application Insights.|
|Microsoft.Insights/DiagnosticSettings/*|Управление параметрами диагностики.|
|Microsoft.Insights/eventtypes/*|Вывод списка событий журнала действий (событий управления) в подписке. Это разрешение применяется для доступа к журналу действий посредством кода или портала.|
|Microsoft.Insights/LogDefinitions/*|Это разрешение необходимо пользователям, которым требуется доступ к журналам действия на портале. Получение списка категорий журнала в журнале действий.|
|Microsoft.Insights/MetricDefinitions/*|Чтение определений метрик (вывод списка доступных типов метрик для ресурса).|
|Microsoft.Insights/Metrics/*|Чтение метрик для ресурса.|
|Microsoft.Insights/Register/Action|Регистрация поставщика Microsoft.Insights.|
|Microsoft.Insights/webtests/*|Управление веб-тестами Application Insights.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Управление Azure Monitor журналов пакеты решений.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Управление Azure Monitor журналов сохраненных поисков.|
|Microsoft.OperationalInsights/workspaces/search/action|Поиск в рабочих областях Log Analytics.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Получение списка ключей для рабочей области Log Analytics.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Управление Azure Monitor журналов конфигурации аналитики хранилища.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими.|
|Microsoft.WorkloadMonitor/workloads/*|Управление рабочими нагрузками.|

### <a name="monitoring-reader"></a>Роль Monitoring Reader

Читатель мониторинга может читать все данные мониторинга. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.OperationalInsights/workspaces/search/action|Поиск в рабочих областях Log Analytics.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими|

### <a name="user-access-administrator"></a>Администратор доступа пользователей

Администратор доступа пользователей может управлять доступом пользователей к ресурсам Azure. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение всех ресурсов|
|Microsoft.Authorization/*|Управление авторизацией|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими|

## <a name="onboarding-permissions"></a>Разрешения на подключение

В следующих разделах описываются минимальные необходимые разрешения, необходимые для адаптации виртуальных машин к решениям для отслеживания изменений или управления обновлениями.

### <a name="permissions-for-onboarding-from-a-vm"></a>Разрешения для адаптации с виртуальной машины

|**Действие**  |**Разрешение**  |**Минимальная область**  |
|---------|---------|---------|
|Запись нового развертывания      | Microsoft.Resources/deployments/*          |Подписка          |
|Запись новой группы ресурсов      | Microsoft.Resources/subscriptions/resourceGroups/write        | Подписка          |
|Создание рабочего пространства по умолчанию      | Microsoft.OperationalInsights/workspaces/write         | Группа ресурсов         |
|Создание учетной записи      |  Microsoft.Automation/automationAccounts/write        |Группа ресурсов         |
|Связывание рабочей области и учетной записи      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Рабочая область</br>Учетная запись службы автоматизации
|Создание расширения MMA      | Microsoft.Compute/virtualMachines/write         | Виртуальная машина         |
|Создание сохраненного поискового запроса      | Microsoft.OperationalInsights/workspaces/write          | Рабочая область         |
|Создание конфигурации области      | Microsoft.OperationalInsights/workspaces/write          | Рабочая область         |
|Проверка состояния подключения — чтение рабочей области      | Microsoft.OperationalInsights/workspaces/read         | Рабочая область         |
|Проверка состояния подключения — чтение свойств связанной рабочей области учетной записи     | Microsoft.Automation/automationAccounts/read      | Учетная запись службы автоматизации        |
|Проверка состояния подключения — чтение решения      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Решение         |
|Проверка состояния подключения — чтение виртуальной машины      | Microsoft.Compute/virtualMachines/read         | Виртуальная машина         |
|Проверка состояния подключения — чтение учетной записи      | Microsoft.Automation/automationAccounts/read  |  Учетная запись службы автоматизации   |
| Проверка рабочей области адаптации для ВМ<sup>1</sup>       | Microsoft.OperationalInsights/workspaces/read         | Подписка         |
| Регистрация поставщика Log Analytics |Microsoft. Insights, регистрация/действие | Подписка|

<sup>1</sup> это разрешение требуется для подключения к порталу виртуальной машины.

### <a name="permissions-for-onboarding-from-automation-account"></a>Разрешения для адаптации из учетной записи службы автоматизации

|**Действие**  |**Разрешение** |**Минимальная область**  |
|---------|---------|---------|
|Создание развертывания     | Microsoft.Resources/deployments/*        | Подписка         |
|Создание группы ресурсов     | Microsoft.Resources/subscriptions/resourceGroups/write         | Подписка        |
|Колонка AutomationOnboarding — создание рабочей области     |Microsoft.OperationalInsights/workspaces/write           | Группа ресурсов        |
|Колонка AutomationOnboarding — чтение связанной рабочей области     | Microsoft.Automation/automationAccounts/read        | Учетная запись службы автоматизации       |
|Колонка AutomationOnboarding — чтение решения     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Решение        |
|Колонка AutomationOnboarding — чтение рабочей области     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Рабочая область        |
|Создание связи для рабочей области и учетной записи     | Microsoft.OperationalInsights/workspaces/write        | Рабочая область        |
|Запись учетной записи для Shoebox      | Microsoft.Automation/automationAccounts/write        | Учетная запись        |
|Создание или изменение сохраненного поискового запроса     | Microsoft.OperationalInsights/workspaces/write        | Рабочая область        |
|Создание или изменение конфигурации области     | Microsoft.OperationalInsights/workspaces/write        | Рабочая область        |
| Регистрация поставщика Log Analytics |Microsoft. Insights, регистрация/действие | Подписка|
|**Шаг 2. Подключение нескольких виртуальных машин**     |         |         |
|Колонка VMOnboarding — создание расширения MMA     | Microsoft.Compute/virtualMachines/write           | Виртуальная машина        |
|Создание или изменение сохраненного поискового запроса     | Microsoft.OperationalInsights/workspaces/write           | Рабочая область        |
|Создание или изменение конфигурации области  | Microsoft.OperationalInsights/workspaces/write   | Рабочая область|

## <a name="update-management-permissions"></a>Разрешения на управление обновлениями

Управление обновлениями действует между несколькими службами для обеспечения их обслуживания. В следующей таблице показаны разрешения, необходимые для развертываний управления обновлениями.

|**Ресурс**  |**Роль**  |**Область**  |
|---------|---------|---------|
|Учетная запись службы автоматизации     | участник Log Analytics.       | Учетная запись службы автоматизации        |
|Учетная запись службы автоматизации    | Участник виртуальной машины        | Группа ресурсов для учетной записи        |
|Рабочая область Log Analytics     | участник Log Analytics.| Рабочая область Log Analytics        |
|Рабочая область Log Analytics |читатель Log Analytics;| Подписка|
|Решение     |участник Log Analytics.         | Решение|
|Виртуальная машина     | Участник виртуальной машины        | Виртуальная машина        |

## <a name="configure-rbac-for-your-automation-account"></a>Настройки RBAC для учетной записи службы автоматизации

В следующем разделе показано, как настроить RBAC в учетной записи службы автоматизации с помощью [портал Azure](#configure-rbac-using-the-azure-portal) и [PowerShell](#configure-rbac-using-powershell).

### <a name="configure-rbac-using-the-azure-portal"></a>Настройка RBAC с помощью портала Azure

1. Войдите на [портал Azure](https://portal.azure.com/) и откройте учетную запись службы автоматизации на странице "Учетные записи автоматизации".
2. Щелкните **Управление доступом (IAM)** , чтобы открыть страницу управления доступом (IAM). Эту страницу можно использовать для добавления новых пользователей, групп и приложений для управления учетной записью службы автоматизации и просмотра существующих ролей, которые можно настроить для учетной записи службы автоматизации.
3. Перейдите на вкладку **Назначения ролей**.

   ![Кнопка доступа](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Добавление нового пользователя и назначение роли

1. На странице Управление доступом (IAM) щелкните **+ добавить назначение ролей**. Это действие открывает страницу Добавление назначения ролей, на которой можно добавить пользователя, группу или приложение и назначить соответствующую роль.

2. Выберите роль из списка доступных ролей. Вы можете выбрать любую из доступных встроенных ролей, поддерживаемых учетной записью службы автоматизации, или пользовательскую роль, которую вы создали сами.

3. В поле **Выбор** введите имя пользователя, которому необходимо предоставить разрешения. Выберите пользователя из списка и нажмите кнопку **сохранить**.

   ![Добавление пользователей](media/automation-role-based-access-control/automation-04-add-users.png)

   Теперь вы должны увидеть пользователя, добавленного на страницу Пользователи, с назначенной ролью.

   ![Список пользователей](media/automation-role-based-access-control/automation-05-list-users.png)

   Кроме того, роль можно назначить пользователю на странице Роли.
4. На странице **Управление доступом (IAM)** щелкните Роли, чтобы открыть страницу Роли. Можно просмотреть имя роли и число пользователей и групп, назначенных этой роли.

    ![Назначение роли из странице пользователей](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Управление доступом на основе ролей можно задать только в области учетной записи службы автоматизации, а не в ресурсе ниже учетной записи службы автоматизации.

#### <a name="remove-a-user"></a>Удаление пользователя

Разрешение на доступ для пользователя, который не управляет учетной записью службы автоматизации или прекращает работу в организации, можно удалить. Чтобы удалить пользователя, выполните описанные ниже действия.

1. На странице Управление доступом (IAM) выберите удаляемого пользователя и нажмите кнопку **Удалить**.
2. Нажмите кнопку **Удалить** в области сведений о назначении.
3. Нажмите кнопку **Да** для подтверждения удаления.

   ![Удаление пользователей](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>Настройка RBAC с помощью PowerShell

Вы также можете настроить доступ на основе ролей к учетной записи службы автоматизации с помощью следующих [командлетов Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

[Get-азроледефинитион](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleDefinition?view=azps-3.7.0) перечисляет все роли RBAC, доступные в Azure Active Directory. С помощью этого командлета с `Name` параметром можно получить список всех действий, которые может выполнять конкретная роль.

```azurepowershell-interactive
Get-AzRoleDefinition -Name 'Automation Operator'
```

Ниже приведен пример выходных данных.

```azurepowershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action,
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
```

[Get-азролеассигнмент](https://docs.microsoft.com/powershell/module/az.resources/get-azroleassignment?view=azps-3.7.0) перечисляет назначения ролей RBAC Azure AD в указанной области. Без параметров этот командлет возвращает все назначения ролей, сделанные в подписке. Используйте `ExpandPrincipalGroups` параметр, чтобы вывести список назначений доступа для указанного пользователя, а также группы, к которым принадлежит пользователь.

**Пример:** Используйте следующий командлет, чтобы вывести список всех пользователей и их ролей в учетной записи службы автоматизации.

```azurepowershell-interactive
Get-AzRoleAssignment -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Ниже приведен пример выходных данных.

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

Используйте [New-азролеассигнмент](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzRoleAssignment?view=azps-3.7.0) , чтобы назначить доступ пользователям, группам и приложениям к определенной области.
    
**Пример.** Следующая команда назначает роль оператора службы автоматизации пользователю в области учетной записи службы автоматизации.

```azurepowershell-interactive
New-AzRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Ниже приведен пример выходных данных.

```azurepowershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

Используйте [Remove-азролеассигнмент](https://docs.microsoft.com/powershell/module/Az.Resources/Remove-AzRoleAssignment?view=azps-3.7.0) для удаления доступа указанного пользователя, группы или приложения из определенной области.

**Пример:** Используйте следующую команду, чтобы удалить пользователя из роли оператора службы автоматизации в области учетной записи службы автоматизации.

```azurepowershell-interactive
Remove-AzRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

В предыдущем примере `sign-in ID of a user you wish to remove`замените, `SubscriptionID` `Resource Group Name`, и `Automation account name` сведениями учетной записи. Выберите **Да** при появлении запроса на подтверждение, прежде чем продолжить удаление назначений ролей пользователей.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Взаимодействие с пользователем в роли оператора автоматизации — учетная запись автоматизации

Если пользователь, которому назначена роль оператора автоматизации в области учетной записи службы автоматизации, просматривает учетную запись службы автоматизации, которой назначена она, пользователь может только просматривать список модулей Runbook, заданий Runbook и расписаний, созданных в учетной записи службы автоматизации. Этот пользователь не может просматривать определения этих элементов. Пользователь может запускать, останавливать, приостанавливать, возобновлять или запланировать задание Runbook. Однако у пользователя нет доступа к другим ресурсам службы автоматизации, таким как конфигурации, группы гибридных рабочих ролей или узлы DSC.

![Нет доступа к ресурсам](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>Настройка RBAC для модулей Runbook

Служба автоматизации Azure позволяет назначать RBAC конкретным модулям Runbook. Для этого выполните следующий скрипт, чтобы добавить пользователя в конкретный модуль Runbook. Этот скрипт может выполнять администратор учетной записи службы автоматизации или администратор клиента.

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation account
$automationAccountName ="<Automation account name>" # Name of the Automation account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation account resource
$aa = Get-AzResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$automationAccountName/$rbName"

# The Automation Job Operator role only needs to be run once per user.
New-AzRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

После выполнения скрипта пользователь должен войти в портал Azure и выбрать **все ресурсы**. В списке пользователь может увидеть модуль Runbook, для которого он был добавлен в качестве оператора автоматизации Runbook.

![Роли RBAC для runbook на портале](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Возможности для пользователя с ролью оператора службы автоматизации. Модуль runbook

Когда пользователь, которому назначена роль оператора автоматизации в области Runbook, просматривает назначенный модуль Runbook, он может запускать только Runbook и просматривать задания Runbook.

![Может только запускать](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Дальнейшие шаги

* Сведения о способах настройки RBAC для службы автоматизации Azure см. в статье [Управление RBAC с помощью Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).
* Дополнительные сведения о способах запуска модуля Runbook см. в разделе [Запуск модуля Runbook](automation-starting-a-runbook.md).
* Дополнительные сведения о типах модулей Runbook см. в разделе [типы Runbook службы автоматизации Azure](automation-runbook-types.md).