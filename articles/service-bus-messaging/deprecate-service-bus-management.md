---
title: Службы обмена сообщениями Azure — Service Manager для диспетчер ресурсов
description: Эта статья содержит сведения о нерекомендуемых Service Manager Azure REST API & командлетах PowerShell для диспетчер ресурсов REST API командлетов PowerShell.
services: service-bus-messaging, event-hubs, event-grid
documentationcenter: na
author: spelluru
editor: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2020
ms.author: spelluru
ms.openlocfilehash: 71e64f4be44d835ad4eed0bb74177e55aef3a35a
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792264"
---
# <a name="deprecation-of-azure-service-manager-support-for-azure-service-bus-relay-and-event-hubs"></a>Нерекомендуемая поддержка Azure Service Manager для служебной шины, ретранслятора и концентраторов событий Azure

Диспетчер ресурсов, наш новый стек облачной инфраструктуры нового поколения полностью заменяет «классическую» модель управления службами Azure (классическая модель развертывания). В результате этого классическая модель развертывания API-интерфейсы RESTFUL и поддержка служебной шины, ретранслятора и концентраторов событий будут прекращены 1 ноября 2021 г. Эта устаревшая версия была впервые объявлена на [Конференции сообщества Майкрософт](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Deprecating-Service-Management-support-for-Azure-Service-Bus/ba-p/370909), но мы недавно решили продлить период устаревания еще на два года с момента первоначального объявления. Для простоты идентификации эти API- `management.core.windows.net` интерфейсы имеют свой универсальный код ресурса (URI). В следующей таблице приведен список устаревших API-интерфейсов и их Azure Resource Manager версии API, которые теперь следует использовать.

Чтобы продолжить использование служебной шины, ретранслятора и концентраторов событий, перейдите на диспетчер ресурсов 31 октября 2021. Мы рекомендуем всем клиентам, которые по-прежнему пользуются старыми API, сделать так, чтобы вы могли воспользоваться преимуществами дополнительных преимуществ диспетчер ресурсов, включая группирование ресурсов, теги, упрощенный процесс развертывания и управления, а также детализированный контроль доступа с помощью управления доступом на основе ролей (RBAC).

Дополнительные сведения о Azure Resource Manager и Service Manager Azure см. в [блоге TechNet](https://blogs.technet.microsoft.com/meamcs/2016/12/22/difference-between-azure-service-manager-and-azure-resource-manager/).

Дополнительные сведения о Service Manager и диспетчер ресурсов API-интерфейсах для служебной шины Azure, ретранслятора и концентраторов событий см. в документации по REST API:

- [Служебная шина Azure](/rest/api/servicebus/)
- [Центры событий Azure](/rest/api/eventhub/)
- [Ретранслятор Azure](/rest/api/relay/)

## <a name="service-manager-rest-api---resource-manager-rest-api"></a>Service Manager REST API диспетчер ресурсов REST API

| Service Manager API (не рекомендуется) | Диспетчер ресурсов — API служебной шины | Диспетчер ресурсов — API концентратора событий | API ретрансляции диспетчер ресурсов |
| --------------- | ----------------- | ----------------- | ----------------- | 
| **Пространства имен — Жетнамеспацеасинк** <br/>[Получение пространства имен служебной шины](/rest/api/servicebus/get-namespace)<br/>[Пространство имен для получения концентратора событий](/rest/api/eventhub/get-event-hub)<br/>[Получение пространства имен ретранслятора](/rest/api/servicebus/get-relays)<br/> ```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](/rest/api/servicebus/namespaces/get) | get | [get](/rest/api/relay/namespaces/get) |
| **ConnectionDetails — Жетконнектиондетаилс**<br/>Служебная шина/концентратор событий/ретранслятор Жетконнектиондеталс<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/ConnectionDetails``` | [listkeys](/rest/api/servicebus/namespaces/listkeys) | listkeys | [listkeys](/rest/api/relay/namespaces/listkeys) |
| **Темы — Жеттопиксасинк**<br/>Служебная шина<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics? $skip={skip}&$top={top}``` | [list](/rest/api/servicebus/topics/listbynamespace) | &nbsp; | &nbsp; | 
| **Очереди — Жеткуеуеасинк** <br/>Служебная шина<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/queues/{queueName}``` | [get](/rest/api/servicebus/queues/get) | &nbsp; | &nbsp; | 
| **Реле — Жетрелайсасинк**<br/>[Получение ретрансляторов](/rest/api/servicebus/get-relays)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/relays? $skip={skip}&$top={top}```| &nbsp; | &nbsp; | [list](/rest/api/relay/wcfrelays/listbynamespace) | 
| **Намеспацеаусоризатионрулес — Жетнамеспацеаусоризатионрулеасинк**<br/>Служебная шина/концентратор событий/ретранслятор Жетнамеспацеаусруле<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/authorizationrules?``` | [жетаусоризатионруле](/rest/api/servicebus/namespaces/getauthorizationrule) |жетаусоризатионруле | [жетаусоризатионруле](/rest/api/relay/namespaces/getauthorizationrule) |
| **Пространства имен — Делетенамеспацеасинк**<br/>[Пространство имен для удаления служебной шины](/rest/api/servicebus/delete-namespace)<br/>[Пространство имен для концентраторов событий](/rest/api/eventhub/delete-event-hub)<br/>[Ретрансляция пространства имен удаления](/rest/api/servicebus/delete-namespace)<br/> ```DELETE  https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [delete](/rest/api/servicebus/namespaces/delete); | удалить | [delete](/rest/api/relay/namespaces/delete); | 
| **Мессагингскуплан — Жетпланасинк**<br/>Получение пространства имен служебной шины, концентратора событий/ретранслятора<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [get](/rest/api/servicebus/namespaces/get) | get | [get](/rest/api/relay/namespaces/get) |
| **Мессагингскуплан — Упдатепланасинк**<br/>Получение пространства имен служебной шины, концентратора событий/ретранслятора<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | createorupdate | [createorupdate](/rest/api/relay/namespaces/createorupdate) |
| **Намеспацеаусоризатионрулес — Упдатенамеспацеаусоризатионрулеасинк**<br/>Получение пространства имен служебной шины, концентратора событий/ретранслятора<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | креатеорупдатеаусоризатионруле | [креатеорупдатеаусоризатионруле](/rest/api/relay/namespaces/createorupdateauthorizationrule) | 
| **Намеспацеаусоризатионрулес — Креатенамеспацеаусоризатионрулеасинк**<br/> 
Служебная шина, концентратор событий/ретранслятор<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` |[createorupdate](/rest/api/servicebus/namespaces/createorupdate) | креатеорупдатеаусоризатионруле | [креатеорупдатеаусоризатионруле](/rest/api/relay/namespaces/createorupdateauthorizationrule) |
| **Намеспацепропертиес — Жетнамеспацепропертиесасинк**<br/>[Получение пространства имен служебной шины](/rest/api/servicebus/get-namespace)<br/>[Получение пространства имен концентраторов событий](/rest/api/eventhub/get-event-hub)<br/>[Получение пространства имен ретранслятора](/rest/api/servicebus/get-relays)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](/rest/api/servicebus/namespaces/get) | get | [get](/rest/api/relay/namespaces/get) |
| **Регионкодес — Жетрегионкодесасинк**<br/>Получение пространства имен служебной шины/EventHub/ретранслятора<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [листбиску](/rest/api/servicebus/regions/listbysku) | листбиску | &nbsp; | 
| **Намеспацепропертиес — Упдатенамеспацепропертясинк**<br/>Служебная шина/EventHub/ретранслятор<br/>```GET    https://management.core.windows.net/{subscription ID}/services/ServiceBus/Regions/``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | createorupdate | [createorupdate](/rest/api/relay/namespaces/createorupdate) |
| **Евенсубскруд — Листевенсубсасинк**<br/>[Список концентраторов событий](/rest/api/eventhub/list-event-hubs)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs?$skip={skip}&$top={top}``` | &nbsp; | [list](/rest/api/servicebus/eventhubs/listbynamespace) | &nbsp; | 
| **Евенсубскруд — Жетевенсубасинк**<br/>[Получение концентраторов событий](/rest/api/eventhub/get-event-hub)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs/{eventHubPath}``` | &nbsp; | [get](/rest/api/eventhub/get-event-hub) | &nbsp; | 
| **Намеспацеаусоризатионрулес — Делетенамеспацеаусоризатионрулеасинк**<br/>Служебная шина, концентратор событий/ретранслятор<br/>```DELETE https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [делетеаусоризатионруле](/rest/api/servicebus/namespaces/deleteauthorizationrule) | делетеаусоризатионруле | [делетеаусоризатионруле](/rest/api/relay/namespaces/deleteauthorizationrule) |
| **Намеспацеаусоризатионрулес — Жетнамеспацеаусоризатионрулесасинк**<br/>Служебная шина/EventHub/ретранслятор<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules``` | [листаусоризатионрулес](/rest/api/servicebus/namespaces/listauthorizationrules) | листаусоризатионрулес | [листаусоризатионрулес](/rest/api/relay/namespaces/listauthorizationrules) |
| **Намеспацеаваилабилити — Иснамеспацеаваилабле**<br/>[Доступность пространства имен служебной шины](/rest/api/servicebus/check-namespace-availability)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/CheckNamespaceAvailability/?namespace=<namespaceValue>``` | [checknameavailability](/rest/api/servicebus/namespaces/checknameavailability) | checknameavailability | [checknameavailability](/rest/api/relay/namespaces/checknameavailability) |
| **Пространства имен — Креатеорупдатенамеспацеасинк**<br/>Служебная шина, концентратор событий/ретранслятор<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | createorupdate | [createorupdate](/rest/api/relay/namespaces/createorupdate) | 
| **Темы — Жеттопикасинк**<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics/{topicPath}``` | [get](/rest/api/servicebus/topics/get) | &nbsp; | &nbsp; |

## <a name="service-manager-powershell---resource-manager-powershell"></a>Service Manager PowerShell диспетчер ресурсов PowerShell
| Service Manager команда PowerShell (не рекомендуется) | Новые команды диспетчер ресурсов | Новая команда диспетчер ресурсов |
| ----- | ----- | ----- | 
| [Get-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusAuthorizationRule.](/powershell/module/azurerm.servicebus/get-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Get-Азсервицебусаусоризатионруле](/powershell/module/az.servicebus/get-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Get-AzureSBLocation](/powershell/module/servicemanagement/azure/get-azuresblocation?view=azuresmps-4.0.0) | [Get-Азурермсервицебусжеодрконфигуратион](/powershell/module/azurerm.servicebus/get-azurermservicebusgeodrconfiguration?view=azurermps-6.13.0) | [Get-Азсервицебусжеодрконфигуратион](/powershell/module/az.servicebus/get-azservicebusgeodrconfiguration?view=azps-1.6.0) |
| [Get-AzureSBNamespace](/powershell/module/servicemanagement/azure/get-azuresbnamespace?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace?view=azurermps-6.13.0) | [Get-Азсервицебуснамеспаце](/powershell/module/az.servicebus/get-azservicebusnamespace?view=azps-1.6.0) |
| [New-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/new-azuresbauthorizationrule?view=azuresmps-4.0.0) | [New-AzureRmServiceBusAuthorizationRule.](/powershell/module/azurerm.servicebus/new-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [New-Азсервицебусаусоризатионруле](/powershell/module/az.servicebus/new-azservicebusauthorizationrule?view=azps-1.6.0) |
| [New-AzureSBNamespace](/powershell/module/servicemanagement/azure/new-azuresbnamespace?view=azuresmps-4.0.0) | [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace?view=azurermps-6.13.0) | [New-Азсервицебуснамеспаце](/powershell/module/az.servicebus/new-azservicebusnamespace?view=azps-1.6.0) |
| [Remove-Азурермрелайаусоризатионруле](/powershell/module/azurerm.relay/remove-azurermrelayauthorizationrule?view=azurermps-6.13.0) | [Remove-AzureRmEventHubAuthorizationRule](/powershell/module/azurerm.eventhub/remove-azurermeventhubauthorizationrule?view=azurermps-6.13.0) | [Remove-Азсервицебусаусоризатионруле](/powershell/module/az.servicebus/remove-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Remove-AzureSBNamespace](/powershell/module/servicemanagement/azure/remove-azuresbnamespace?view=azuresmps-4.0.0) | [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace?view=azurermps-6.13.0) | [Remove-Азсервицебуснамеспаце](/powershell/module/az.servicebus/remove-azservicebusnamespace?view=azps-1.6.0) |
| [Set-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/set-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Set-AzureRmServiceBusAuthorizationRule.](/powershell/module/azurerm.servicebus/set-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Set-Азсервицебусаусоризатионруле](/powershell/module/az.servicebus/set-azservicebusauthorizationrule?view=azps-1.6.0) |

## <a name="next-steps"></a>Дальнейшие действия
Обратитесь к следующей документации. 

- Последняя документация по REST API
    - [Служебная шина Azure](/rest/api/servicebus/)
    - [Центры событий Azure](/rest/api/eventhub/)
    - [Ретранслятор Azure](/rest/api/relay/)
- Последняя документация по PowerShell
    - [Служебная шина Azure](/powershell/module/azurerm.servicebus/?view=azurermps-6.13.0#service_bus)
    - [Центры событий Azure](/powershell/module/azurerm.eventhub/?view=azurermps-6.13.0#event_hub)
    - [Сетка событий Azure](/powershell/module/azurerm.eventgrid/?view=azurermps-6.13.0#event_grid)
