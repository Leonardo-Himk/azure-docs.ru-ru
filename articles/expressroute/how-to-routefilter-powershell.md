---
title: 'ExpressRoute: фильтры маршрутов — пиринг Майкрософт: Azure PowerShell'
description: В этой статье описывается, как настраивать фильтры маршрутов для пиринга Майкрософт с помощью PowerShell.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 3fa53258321b22e1683122edca1816f6d4c291b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80618615"
---
# <a name="configure-route-filters-for-microsoft-peering-powershell"></a>Настройка фильтров маршрутов для пиринга Майкрософт с помощью PowerShell
> [!div class="op_single_selector"]
> * [Портал Azure](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Фильтры маршрутов — это способ использовать подмножество поддерживаемых служб через пиринг Майкрософт. Действия, описанные в этой статье, помогут настроить фильтры маршрутов для каналов ExpressRoute и управлять ими.

Службы Office 365, такие как Exchange Online, SharePoint Online, Skype для бизнеса и общедоступные службы Azure, такие как хранилище и база данных SQL, доступны через пиринг Майкрософт. Общедоступные службы Azure можно выбирать отдельно для каждого региона и нельзя определять для каждой общедоступной службы.

Если в канале ExpressRoute настроен пиринг Майкрософт и присоединен фильтр маршрутов, все префиксы, выбранные для этих служб, объявляются через установленные сеансы BGP. Значение сообщества BGP подключается к каждому префиксу для идентификации службы, предлагаемой через него. Список значений сообщества BGP и служб, с которыми они сопоставляются, см. в разделе [Поддержка сообществ BGP](expressroute-routing.md#bgp).

Если требуется подключение ко всем службам, большое число префиксов объявляется через BGP. Это значительно увеличивает размер таблиц маршрутов, которые обслуживают маршрутизаторы в вашей сети. Если вы планируете использовать только подмножество служб, предлагаемых через пиринг Майкрософт, размер таблиц маршрутизации можно уменьшить двумя способами. Можно выполнить следующие действия.

- Отфильтровывать нежелательные префиксы путем применения фильтров маршрутов к сообществам BGP. Это стандартная сетевая практика и обычно используется во многих сетях.

- Определять фильтры маршрутов и применять их к каналу ExpressRoute. Фильтр маршрутов — это новый ресурс, который позволяет выбрать список служб, которые вы собираетесь использовать через пиринг Майкрософт. Маршрутизаторы ExpressRoute отправляют только список префиксов, которые принадлежат службам, определяемым в фильтре маршрутов.

### <a name="about-route-filters"></a><a name="about"></a>О фильтрах маршрута

Если в канале ExpressRoute настроен пиринг Майкрософт, пограничные маршрутизаторы Microsoft Network подают пару сеансов BGP с пограничными маршрутизаторами (ваш поставщик услуг подключения). В вашей сети маршруты не объявляются. Чтобы включить объявление маршрутов в своей сети, необходимо связать с ней фильтр маршрутов.

Фильтр маршрутов позволяет вам идентифицировать службы, которые можно использовать через пиринг Майкрософт по каналу ExpressRoute. По сути, это список разрешений для всех значений сообщества BGP. После определения ресурса фильтра маршрута и его подключения к каналу ExpressRoute все префиксы, которые сопоставляются со значениями сообщества BGP, объявляются в сети.

Чтобы иметь возможность подключать фильтры маршрутов со службами Office 365, у вас должно быть разрешение на использование служб Office 365 через ExpressRoute. Если вы не авторизованы для использования служб Office 365 через ExpressRoute, произойдет сбой операции подключения фильтров маршрута. Дополнительные сведения о процессе авторизации см. в статье [Azure ExpressRoute для Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).

> [!IMPORTANT]
> Пиринг Майкрософт для каналов ExpressRoute, которые были настроены до 1 августа 2017 г., позволяет объявлять все префиксы служб, даже если не настроены фильтры маршрутов. Пиринг Майкрософт для каналов ExpressRoute, настроенных 1 августа 2017 г. или позднее, не будет объявлять префиксы, пока к каналу не будет присоединен фильтр маршрутов.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>Рабочий процесс

Чтобы успешно подключиться к службам через пиринг Майкрософт, необходимо выполнить следующие действия по настройке:

- Необходимо иметь активный канал ExpressRoute с подготовленным пирингом Майкрософт. Для выполнения этих задач можно использовать следующие инструкции:
  - [Создайте канал ExpressRoute](expressroute-howto-circuit-arm.md) и включите его на стороне поставщика услуг подключения. Канал ExpressRoute должен находиться в подготовленном и включенном состоянии.
  - [Создайте пиринг Майкрософт](expressroute-circuit-peerings.md), если вы управляете сеансом BGP напрямую или если у вашего поставщика услуг подключения подготовлен пиринг Майкрософт для вашего канала.

-  Необходимо создать и настроить фильтр маршрутов.
    - Определите службы, которые вы будете использовать через пиринг Майкрософт.
    - Определите список значений сообщества BGP, связанных со службами.
    - Создайте правило, чтобы разрешить список префиксов, соответствующих значениям сообщества BGP.

-  Фильтр маршрутов необходимо подключить к каналу ExpressRoute.

## <a name="before-you-begin"></a>Подготовка к работе

Перед началом настройки убедитесь, что выполнены следующие условия:

 - Изучите [предварительные требования](expressroute-prerequisites.md) и [рабочие процессы](expressroute-workflows.md), прежде чем приступить к настройке.

 - Вам потребуется активный канал ExpressRoute. Приступая к работе, [создайте канал ExpressRoute](expressroute-howto-circuit-arm.md) ; он должен быть затем включен на стороне поставщика услуг подключения. Канал ExpressRoute должен находиться в подготовленном и включенном состоянии.

 - Необходимо иметь активный пиринг Майкрософт. Следуйте инструкциям в статье [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).


### <a name="working-with-azure-powershell"></a>Работа с Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

### <a name="log-in-to-your-azure-account"></a>Вход в учетную запись Azure

Прежде чем приступить к настройке, необходимо войти в учетную запись Azure. Командлет запрашивает учетные данные входа для вашей учетной записи Azure. После выполнения входа он загружает параметры учетной записи, чтобы они были доступны в Azure PowerShell.

Откройте консоль PowerShell с повышенными привилегиями и подключитесь к своей учетной записи. Для подключения используйте следующий пример кода: Если вы используете Azure Cloud Shell, вход в систему будет выполнен автоматически, так что вам не нужно запускать этот командлет.

```azurepowershell
Connect-AzAccount
```

Если у вас есть несколько подписок Azure, проверьте подписки для этой учетной записи.

```azurepowershell-interactive
Get-AzSubscription
```

Укажите подписку, которую нужно использовать.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>Шаг 1. Получение списка префиксов и значений сообщества BGP

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Получение списка значений сообщества BGP

Чтобы получить список значений сообщества BGP, связанных со службами, которые доступны через пиринг Майкрософт, а также список префиксов, связанных с ними, используйте следующий командлет:

```azurepowershell-interactive
Get-AzBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Сделайте список значений, которые вы хотите использовать

Создайте список значений сообщества BGP, которые нужно использовать в фильтре маршрута.

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>Шаг 2. Создание фильтра маршрутов и правила фильтрации

Фильтр может иметь только одно правило, и оно должно иметь тип "Разрешить". С этим правилом может быть связан список значений сообщества BGP.

### <a name="1-create-a-route-filter"></a>1. Создание фильтра маршрутов

Сначала создайте фильтр маршрутов. Команда "New-Азраутефилтер" создает только ресурс фильтра маршрутов. После создания ресурса нужно создать правило и подключить его к объекту фильтра маршрута. Чтобы создать ресурс фильтра маршрута, выполните следующую команду:

```azurepowershell-interactive
New-AzRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Создание правила фильтра

Можно указать набор сообществ BGP в виде разделенного запятыми списка, как показано в примере. Чтобы создать новое правило, выполните следующую команду:
 
```azurepowershell-interactive
$rule = New-AzRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList 12076:5010,12076:5040
```

### <a name="3-add-the-rule-to-the-route-filter"></a>3. Добавьте правило в фильтр маршрутов.

Чтобы добавить правило фильтрации в маршрут фильтра, выполните следующую команду:
 
```azurepowershell-interactive
$routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzRouteFilter -RouteFilter $routefilter
```

## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>Шаг 3. Подключение фильтра маршрутов к каналу ExpressRoute

Чтобы подключить фильтр маршрутов к каналу ExpressRoute, выполните следующую команду, если вы установили только пиринг Майкрософт:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="common-tasks"></a><a name="tasks"></a>Стандартные задачи

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Получение свойств фильтра маршрута

Чтобы получить свойства фильтра маршрута, выполните следующие действия:

1. Чтобы получить ресурс фильтра маршрута, выполните следующую команду:

   ```azurepowershell-interactive
   $routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
   ```
2. Получите правила фильтра маршрута для ресурса фильтра маршрута, выполнив следующую команду:

   ```azurepowershell-interactive
   $routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
   $rule = $routefilter.Rules[0]
   ```

### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Обновление свойств фильтра маршрутов

Если фильтр маршрутов уже подключен к каналу, обновления в списке сообщества BGP автоматически распространяют соответствующие изменения объявлений префикса через установленные сеансы BGP. Вы можете обновить список сообщества BGP вашего фильтра маршрута с помощью следующей команды:

```azurepowershell-interactive
$routefilter = Get-AzRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzRouteFilter -RouteFilter $routefilter
```

### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Отсоединение фильтра маршрутов от канала ExpressRoute

Как только фильтр маршрутов отсоединяется от канала ExpressRoute, префиксы перестают объявляться через сеанс BGP. Фильтр маршрутов можно отсоединить от канала ExpressRoute с помощью следующей команды:
  
```azurepowershell-interactive
$ckt.Peerings[0].RouteFilter = $null
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Удаление фильтра маршрутов

Фильтр маршрутов можно удалить только, если он не подключен к каналу. Убедитесь, фильтр маршрутов не подключен к каналу, прежде чем пытаться удалить его. Фильтр маршрутов можно удалить, используя следующую команду:

```azurepowershell-interactive
Remove-AzRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения об ExpressRoute см. в статье [вопросы и ответы по expressroute](expressroute-faqs.md).
