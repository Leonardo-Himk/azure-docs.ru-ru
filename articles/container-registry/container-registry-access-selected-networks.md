---
title: Настройка правил брандмауэра службы
description: Настройте правила IP-адресов, чтобы разрешить доступ к реестру контейнеров Azure из выбранных общедоступных IP-адресов или диапазонов адресов.
ms.topic: article
ms.date: 05/04/2020
ms.openlocfilehash: f6459061ca486b4bf229409e6ec1ed1bd808a474
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82984620"
---
# <a name="configure-public-ip-network-rules"></a>Настройка правил общедоступной IP-сети

По умолчанию реестр контейнеров Azure принимает подключения через Интернет от узлов в любой сети. В этой статье показано, как настроить реестр контейнеров, чтобы разрешить доступ только с конкретных общедоступных IP-адресов или диапазонов адресов. Предусмотрены эквивалентные действия с использованием Azure CLI и портал Azure.

Правила IP-сети настроены в общедоступной конечной точке реестра. Правила IP-сети не применяются к частным конечным точкам, настроенным с помощью [частного канала](container-registry-private-link.md)

Настройка правил доступа к IP-адресу доступна на уровне службы реестра " **премиум** ". Сведения об уровнях и ограничениях служб реестра см. в статье [уровни реестра контейнеров Azure](container-registry-skus.md).

## <a name="access-from-selected-public-network---cli"></a>Доступ из выбранной общедоступной сети — CLI

### <a name="change-default-network-access-to-registry"></a>Изменение сетевого доступа по умолчанию к реестру

Чтобы ограничить доступ к выбранной общедоступной сети, сначала измените действие по умолчанию, чтобы запретить доступ. Замените имя реестра в следующей команде [AZ контроля][az-acr-update] доступа на обновление:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

### <a name="add-network-rule-to-registry"></a>Добавить сетевое правило в реестр

Чтобы добавить сетевое правило в реестр, которое разрешает доступ с общедоступного IP-адреса или диапазона, используйте команду AZ запись в [сеть-правило Add][az-acr-network-rule-add] . Например, замените имя реестра контейнеров и общедоступный IP-адрес виртуальной машины в виртуальной сети.

```azurecli
az acr network-rule add \
  --name mycontainerregistry \
  --ip-address <public-IP-address>
```

> [!NOTE]
> После добавления правила для его вступления в силу потребуется несколько минут.

## <a name="access-from-selected-public-network---portal"></a>Доступ из выбранной общедоступной сети — портал

1. На портале перейдите к реестру контейнеров.
1. В разделе **Параметры**выберите **сеть**.
1. На вкладке **общий доступ** выберите этот параметр, чтобы разрешить общий доступ из **выбранных сетей**.
1. В разделе **брандмауэр**введите общедоступный IP-адрес, например общедоступный IP-адрес виртуальной машины в виртуальной сети. Или введите диапазон адресов в нотации CIDR, которая содержит IP-адрес виртуальной машины.
1. Нажмите кнопку **Сохранить**.

![Настройка правила брандмауэра для реестра контейнеров][acr-access-selected-networks]

> [!NOTE]
> После добавления правила для его вступления в силу потребуется несколько минут.

> [!TIP]
> При необходимости включите доступ к реестру с локального клиентского компьютера или диапазона IP-адресов. Чтобы разрешить этот доступ, необходим общедоступный IPv4-адрес компьютера. Этот адрес можно найти, выполнив поиск по слову "адрес IP-адреса" в Интернет-браузере. Текущий адрес IPv4 клиента также отображается автоматически при настройке параметров брандмауэра на странице **сеть** на портале.

## <a name="disable-public-network-access"></a>Отключить доступ к общедоступной сети

Чтобы ограничить трафик между виртуальными сетями с помощью [частной ссылки](container-registry-private-link.md), отключите общедоступную конечную точку в реестре. Отключение общедоступной конечной точки переопределяет все конфигурации брандмауэра.

### <a name="disable-public-access---portal"></a>Отключить открытый доступ — портал

1. На портале перейдите к реестру контейнеров и выберите **параметры > сети**.
1. На вкладке **общий доступ** в параметре **Разрешить общий доступ**выберите значение **отключено**. Затем нажмите кнопку **Save** (Сохранить).

![Отключить общий доступ][acr-access-disabled]

## <a name="restore-default-registry-access"></a>Восстановление доступа к реестру по умолчанию

Чтобы восстановить реестр, чтобы разрешить доступ по умолчанию, обновите действие по умолчанию. 

### <a name="restore-default-registry-access---portal"></a>Восстановление доступа к реестру по умолчанию — портал

1. На портале перейдите к реестру контейнеров и выберите **параметры > сети**.
1. В разделе **брандмауэр**выберите каждый диапазон адресов, а затем щелкните значок Удалить.
1. На вкладке **общий доступ** в поле **Разрешить общий доступ**выберите **все сети**. Затем нажмите кнопку **Save** (Сохранить).

![Общий доступ из всех сетей][acr-access-all-networks]

## <a name="next-steps"></a>Дальнейшие действия

* Сведения об ограничении доступа к реестру с помощью частной конечной точки в виртуальной сети см. в статье [Настройка частного канала Azure для реестра контейнеров Azure](container-registry-private-link.md).
* Если необходимо настроить правила доступа к реестру из брандмауэра клиента, см. раздел [Настройка правил для доступа к реестру контейнеров Azure за брандмауэром](container-registry-firewall-access-rules.md).

[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com

[acr-access-selected-networks]: ./media/container-registry-access-selected-networks/acr-access-selected-networks.png
[acr-access-disabled]: ./media/container-registry-access-selected-networks/acr-access-disabled.png
[acr-access-all-networks]: ./media/container-registry-access-selected-networks/acr-access-all-networks.png
