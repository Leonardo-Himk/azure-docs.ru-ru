---
title: 'ExpressRoute: фильтры маршрутов — пиринг Майкрософт: портал Azure'
description: В этой статье описывается, как настраивать фильтры маршрутов для пиринга Майкрософт с помощью портала Azure.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 07/01/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: f2be9b4e7152c61885b1a41e94ebd328059d437b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80618566"
---
# <a name="configure-route-filters-for-microsoft-peering-azure-portal"></a>Настройка фильтров маршрутов для пиринга Майкрософт с помощью портала Azure
> [!div class="op_single_selector"]
> * [Портал Azure](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Фильтры маршрутов — это способ использовать подмножество поддерживаемых служб через пиринг Майкрософт. Действия, описанные в этой статье, помогут настроить фильтры маршрутов для каналов ExpressRoute и управлять ими.

Службы Office 365, такие как Exchange Online, SharePoint Online и Skype для бизнеса, а также службы Azure, такие как хранилище и база данных SQL, доступны через пиринг Майкрософт. При настройке пиринга Майкрософт в канале ExpressRoute все префиксы, которые относятся к этим службам, объявляются через установленные сеансы BGP. Значение сообщества BGP подключается к каждому префиксу для идентификации службы, предлагаемой через него. Список значений сообщества BGP и служб, с которыми они сопоставляются, см. в разделе [Поддержка сообществ BGP](expressroute-routing.md#bgp).

Если требуется подключение ко всем службам, большое число префиксов объявляется через BGP. Это значительно увеличивает размер таблиц маршрутов, которые обслуживают маршрутизаторы в вашей сети. Если вы планируете использовать только подмножество служб, предлагаемых через пиринг Майкрософт, размер таблиц маршрутизации можно уменьшить двумя способами. Можно выполнить следующие действия.

- Отфильтровывать нежелательные префиксы путем применения фильтров маршрутов к сообществам BGP. Это стандартная сетевая практика и обычно используется во многих сетях.

- Определять фильтры маршрутов и применять их к каналу ExpressRoute. Фильтр маршрутов — это новый ресурс, который позволяет выбрать список служб, которые вы собираетесь использовать через пиринг Майкрософт. Маршрутизаторы ExpressRoute отправляют только список префиксов, которые принадлежат службам, определяемым в фильтре маршрутов.

### <a name="about-route-filters"></a><a name="about"></a>О фильтрах маршрута

При настроенном пиринге Майкрософт для канала ExpressRoute пограничные маршрутизаторы Майкрософт устанавливают пару сеансов BGP с пограничными маршрутизаторами (вашими или ваших поставщиков услуг подключения). В вашей сети маршруты не объявляются. Чтобы включить объявление маршрутов в своей сети, необходимо связать с ней фильтр маршрутов.

Фильтр маршрутов позволяет вам идентифицировать службы, которые можно использовать через пиринг Майкрософт по каналу ExpressRoute. По сути, это список всех значений сообщества BGP, которые вы хотите разрешить. После определения ресурса фильтра маршрута и его подключения к каналу ExpressRoute все префиксы, которые сопоставляются со значениями сообщества BGP, объявляются в сети.

Чтобы иметь возможность подключать фильтры маршрутов со службами Office 365, у вас должно быть разрешение на использование служб Office 365 через ExpressRoute. Если вы не авторизованы для использования служб Office 365 через ExpressRoute, произойдет сбой операции подключения фильтров маршрута. Дополнительные сведения о процессе авторизации см. в статье [Azure ExpressRoute для Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).

> [!IMPORTANT]
> Пиринг Майкрософт для каналов ExpressRoute, которые были настроены до 1 августа 2017 г., позволяет объявлять все префиксы служб, даже если не настроены фильтры маршрутов. Пиринг Майкрософт для каналов ExpressRoute, настроенных 1 августа 2017 г. или позднее, не будет объявлять префиксы, пока к каналу не будет присоединен фильтр маршрутов.
> 
> 

### <a name="workflow"></a><a name="workflow"></a>Рабочий процесс

Чтобы успешно подключиться к службам через пиринг Майкрософт, необходимо выполнить следующие действия по настройке:

- Необходимо иметь активный канал ExpressRoute с подготовленным пирингом Майкрософт. Для выполнения этих задач можно использовать следующие инструкции:
  - [Создайте канал ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) и включите его на стороне поставщика услуг подключения. Канал ExpressRoute должен находиться в подготовленном и включенном состоянии.
  - [Создайте пиринг Майкрософт](expressroute-howto-routing-portal-resource-manager.md), если вы управляете сеансом BGP напрямую или если у вашего поставщика услуг подключения подготовлен пиринг Майкрософт для вашего канала.

-  Необходимо создать и настроить фильтр маршрутов.
    - Определите службы, которые вы будете использовать через пиринг Майкрософт.
    - Определите список значений сообщества BGP, связанных со службами.
    - Создайте правило, чтобы разрешить список префиксов, соответствующих значениям сообщества BGP.

-  Фильтр маршрутов необходимо подключить к каналу ExpressRoute.

## <a name="before-you-begin"></a>Подготовка к работе

Перед началом настройки убедитесь, что выполнены следующие условия:

 - Изучите [предварительные требования](expressroute-prerequisites.md) и [рабочие процессы](expressroute-workflows.md), прежде чем приступить к настройке.

 - Вам потребуется активный канал ExpressRoute. Приступая к работе, [создайте канал ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) ; он должен быть затем включен на стороне поставщика услуг подключения. Канал ExpressRoute должен находиться в подготовленном и включенном состоянии.

 - Необходимо иметь активный пиринг Майкрософт. Следуйте инструкциям в статье [Создание и изменение канала ExpressRoute с помощью PowerShell](expressroute-howto-routing-portal-resource-manager.md).


## <a name="step-1-get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>Шаг 1. Получение списка префиксов и значений сообщества BGP

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Получение списка значений сообщества BGP

Значения BGP сообщества, связанные со службами, доступными через пиринг Майкрософт, приведены на странице [Требования к маршрутизации ExpressRoute](expressroute-routing.md).

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Сделайте список значений, которые вы хотите использовать

Создайте список [значений сообщества BGP](expressroute-routing.md#bgp) , которые вы хотите использовать в фильтре маршрутов. 

## <a name="step-2-create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>Шаг 2. Создание фильтра маршрутов и правила фильтрации

Фильтр может иметь только одно правило, и оно должно иметь тип "Разрешить". С этим правилом может быть связан список значений сообщества BGP.

### <a name="1-create-a-route-filter"></a>1. Создание фильтра маршрутов
Чтобы создать фильтр маршрута, необходимо выбрать команду создания ресурса. Щелкните **создать ресурс** > **Сетевые подключения** > **RouteFilter**, как показано на следующем рисунке.

![Создание фильтра маршрута](./media/how-to-routefilter-portal/CreateRouteFilter1.png)

Фильтр маршрута необходимо разместить в группе ресурсов. 

![Создание фильтра маршрута](./media/how-to-routefilter-portal/CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. Создание правила фильтра

Можно добавлять и обновлять правила, выбрав вкладку "Управление правилами фильтра маршрута".

![Создание фильтра маршрута](./media/how-to-routefilter-portal/ManageRouteFilter.png)


В раскрывающемся списке можно выбрать службы, к которым необходимо подключиться, и сохранить правило после завершения.

![Создание фильтра маршрута](./media/how-to-routefilter-portal/AddRouteFilterRule.png)


## <a name="step-3-attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>Шаг 3. Подключение фильтра маршрутов к каналу ExpressRoute

Фильтр маршрута можно присоединить к каналу, нажав кнопку "добавить цепь" и выбрав канал ExpressRoute из раскрывающегося списка.

![Создание фильтра маршрута](./media/how-to-routefilter-portal/AddCktToRouteFilter.png)

Если поставщик услуг подключения настраивает пиринг для канала ExpressRoute, обновите канал из колонки канала ExpressRoute, прежде чем нажать кнопку Add circuit (Добавить канал).

![Создание фильтра маршрута](./media/how-to-routefilter-portal/RefreshExpressRouteCircuit.png)

## <a name="common-tasks"></a><a name="tasks"></a>Стандартные задачи

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Получение свойств фильтра маршрута

Открыв ресурс на портале, можно просмотреть свойства фильтра маршрута.

![Создание фильтра маршрута](./media/how-to-routefilter-portal/ViewRouteFilter.png)


### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Обновление свойств фильтра маршрутов

Нажав кнопку "Управление правилом", можно обновить список значений сообщества BGP, присоединенного к схеме.


![Создание фильтра маршрута](./media/how-to-routefilter-portal/ManageRouteFilter.png)

![Создание фильтра маршрута](./media/how-to-routefilter-portal/AddRouteFilterRule.png) 


### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Отсоединение фильтра маршрутов от канала ExpressRoute

Чтобы отсоединить цепь от фильтра маршрутов, щелкните канал правой кнопкой мыши и выберите «разорвать связь».

![Создание фильтра маршрута](./media/how-to-routefilter-portal/DetachRouteFilter.png) 


### <a name="to-delete-a-route-filter"></a><a name="delete"></a>Удаление фильтра маршрутов

Фильтр маршрута можно удалить, нажав кнопку "Удалить". 

![Создание фильтра маршрута](./media/how-to-routefilter-portal/DeleteRouteFilter.png) 

## <a name="next-steps"></a>Дальнейшие шаги

* Дополнительные сведения об ExpressRoute см. в статье [вопросы и ответы по expressroute](expressroute-faqs.md).

* Сведения о примерах конфигурации маршрутизатора см. [в статье примеры конфигурации маршрутизаторов для настройки маршрутизации и управления ею](expressroute-config-samples-routing.md). 
