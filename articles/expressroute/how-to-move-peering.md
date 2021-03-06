---
title: 'Azure ExpressRoute: перемещение общедоступного пиринга в пиринг Майкрософт'
description: В этой статье приведены этапы перехода с общедоступного пиринга на пиринг Майкрософт в ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 48ecfcc0d6241e7926892a3ca1c9925b0dc07241
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75436839"
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Переход с общедоступного пиринга на пиринг Майкрософт

Эта статья поможет вам переместить конфигурацию общедоступного пиринга в пиринг Майкрософт без простоев. При использовании пиринга Майкрософт с фильтрами маршрутов ExpressRoute поддерживает службы Azure PaaS, такие как хранилище Azure и база данных SQL Azure. Требуется только один домен маршрутизации для доступа к службам Microsoft PaaS и SaaS. Воспользуйтесь фильтрами маршрутов, чтобы выборочно объявить префиксы службы PaaS для регионов Azure, которые нужно использовать.

Общедоступный пиринг Azure имеет 1 IP-адрес NAT, связанный с каждым сеансом BGP. Пиринг Майкрософт позволяет настраивать собственные выделения NAT, а также использовать фильтры маршрутов для избирательных объявлений префиксов. Общедоступный пиринг — это однонаправленная служба, с помощью которой подключение всегда инициируется из глобальной сети в Microsoft Azure Services. Службы Microsoft Azure не будут иметь возможность инициировать подключения к вашей сети через этот домен маршрутизации.

После включения общедоступного пиринга можно будет подключаться ко всем службам Azure. Мы не позволяем выбирать службы, для которых объявляются маршруты. Хотя пиринг Майкрософт — это двунаправленный способ подключения, при котором подключение можно инициировать из Microsoft Azure службы вместе с глобальной сетью. Дополнительные сведения о доменах маршрутизации и пиринга см. в разделе [каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).

## <a name="before-you-begin"></a><a name="before"></a>Перед началом

Чтобы подключиться к пирингу Майкрософт, необходимо настроить преобразование сетевых адресов (NAT) и управлять им. Поставщик услуг подключения может настроить NAT и управлять им как управляемой службой. Если вы планируете получить доступ к службам Azure PaaS и Azure SaaS при пиринге Майкрософт, очень важно правильно выбрать размер пула IP-адресов NAT. Дополнительные сведения о NAT для ExpressRoute см. в разделе [Требования NAT для пиринга Майкрософт](expressroute-nat.md#nat-requirements-for-microsoft-peering). При подключении к Майкрософт через Azure ExpressRoute (пиринг Майкрософт) у вас есть несколько ссылок на Майкрософт. Одно из них — существующее подключение к Интернету, а другое — подключение через ExpressRoute. Часть трафика, направляемого в Майкрософт, может проходить через Интернет, но возвращаться через ExpressRoute, и наоборот.

![Двунаправленное подключение](./media/how-to-move-peering/bidirectional-connectivity.jpg)

> [!Warning]
> Пул IP-адресов для NAT, объявленных для Майкрософт, не должен объявляться в Интернете. Это приведет к разрыву подключения к другим службам Microsoft.

Используйте [асимметричную маршрутизацию с несколькими сетевыми путями](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing) для предупреждения о асимметричной маршрутизации перед настройкой пиринга Майкрософт.

* Если вы используете общедоступный пиринг и для доступа к [службе хранилища Azure](../storage/common/storage-network-security.md) или [базе данных SQL Azure](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md) применяются правила IP-сети для общедоступных IP-адресов, убедитесь, что пул IP-адресов NAT, настроенный с пирингом Майкрософт, указан в списке общедоступных IP-адресов для учетной записи хранения Azure или учетной записи Azure SQL.<br>
* Чтобы переместить пиринг Майкрософт без простоев, выполните инструкции в этой статье в том порядке, в котором они представлены.

## <a name="1-create-microsoft-peering"></a><a name="create"></a>1. Создание пиринга Майкрософт

Если пиринг Майкрософт не создан, выполните инструкции в любой из статей по приведенным ниже ссылкам. Если поставщик услуг подключения предоставляет управляемые службы уровня 3, он может включить пиринг Майкрософт для вашего канала.

Если управление уровнем 3 осуществляется пользователем, перед продолжением необходимо указать следующие сведения.

* Подсеть /30 для основной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4, принадлежащего вам и зарегистрированного в RIR/IRR. Из этой подсети вы назначите первый готовый к применению IP-адрес для своего маршрутизатора, так как корпорация Майкрософт использует второй IP-адрес для маршрутизатора Майкрософт.<br>
* Подсеть /30 для дополнительной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4, принадлежащего вам и зарегистрированного в RIR/IRR. Из этой подсети вы назначите первый готовый к применению IP-адрес для своего маршрутизатора, так как корпорация Майкрософт использует второй IP-адрес для маршрутизатора Майкрософт.<br>
* Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале. Для основного и дополнительного соединений необходимо использовать тот же идентификатор виртуальной локальной сети.<br>
* Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS.<br>
* Объявленные префиксы: необходимо предоставить список всех префиксов, которые вы планируете объявить во время сеанса BGP. Допускаются только общедоступные префиксы IP-адресов. Если вы планируете отправить набор префиксов, их можно оформить в виде списка, разделенного запятыми. Эти префиксы должны быть зарегистрированы в RIR/IRR на ваше имя.<br>
* Имя реестра маршрутизации: можно указать RIR/IRR, в котором зарегистрированы номер AS и префиксы.

* **Необязательно** — клиент ASN. Если вы хотите объявить префиксы, не зарегистрированные для пиринга как номер, можно указать номер AS, в который они зарегистрированы.<br>
* **Необязательно** . хэш MD5, если вы решили использовать его.

Подробные инструкции по включению пиринга Майкрософт можно найти в следующих статьях:

* [Создание пиринга Майкрософт с помощью портала Azure](expressroute-howto-routing-portal-resource-manager.md#msft)<br>
* [Создание пиринга Майкрософт с помощью Azure Powershell](expressroute-howto-routing-arm.md#msft)<br>
* [Создание пиринга Майкрософт с помощью Azure CLI](howto-routing-cli.md#msft)

## <a name="2-validate-microsoft-peering-is-enabled"></a><a name="validate"></a>2. Проверка пиринга Майкрософт включена

Убедитесь, что включен пиринг Майкрософт и объявленные общедоступные префиксы находятся в настроенном состоянии.

* [Портал Azure](expressroute-howto-routing-portal-resource-manager.md#getmsft)<br>
* [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)<br>
* [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="3-configure-and-attach-a-route-filter-to-the-circuit"></a><a name="routefilter"></a>3. Настройка и присоединение фильтра маршрута к каналу

По умолчанию новый пиринг Майкрософт не объявляет префиксы до тех пор, пока к каналу не будет присоединен фильтр маршрутов. При создании правила фильтра маршрутов можно указать список сообщества служб для регионов Azure, которые будут использоваться для служб Azure PaaS. Это обеспечивает гибкость при фильтрации маршрутов в соответствии с требованиями, как показано на следующем снимке экрана:

![Объединение общедоступного пиринга](./media/how-to-move-peering/routefilter.jpg)

Настройте фильтры маршрутов, воспользовавшись инструкциями в любой из следующих статей:

* [Настройка фильтров маршрутов для пиринга Майкрософт с помощью портала Azure](how-to-routefilter-portal.md)<br>
* [Настройка фильтров маршрутов для пиринга Майкрософт с помощью PowerShell](how-to-routefilter-powershell.md)<br>
* [Настройка фильтров маршрутов для пиринга Майкрософт с помощью Azure CLI](how-to-routefilter-cli.md)

## <a name="4-delete-the-public-peering"></a><a name="delete"></a>4. Удаление общедоступного пиринга

Убедившись, что пиринг Майкрософт настроен и префиксы, которые вы хотите использовать, правильно объявлены, можно удалять общедоступный пиринг. Чтобы удалить общедоступный пиринг, воспользуйтесь инструкциями в любой из следующих статей:

* [Удаление общедоступного пиринга Azure с помощью Azure PowerShell](about-public-peering.md#powershell)
* [Удаление общедоступного пиринга Azure с помощью CLI](about-public-peering.md#cli)
  
## <a name="5-view-peerings"></a><a name="view"></a>5. Просмотр пиринга
  
На портале Azure вы можете просмотреть список всех каналов и пирингов ExpressRoute. Дополнительные сведения см. в разделе [Просмотр сведений о пиринге Майкрософт](expressroute-howto-routing-portal-resource-manager.md#getmsft).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об ExpressRoute см. в статье [вопросы и ответы по expressroute](expressroute-faqs.md).
