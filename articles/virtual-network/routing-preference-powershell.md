---
title: Настройка предпочтительного варианта маршрутизации для общедоступного IP-адреса — Azure PowerShell
titlesuffix: Azure Virtual Network
description: Узнайте, как настроить предпочтительный вариант маршрутизации для общедоступного IP-адреса с помощью Azure PowerShell.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2020
ms.author: mnayak
ms.openlocfilehash: 2a0a876b885d2fe59262cad660a3ebc5b5210fbb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595993"
---
# <a name="configure-routing-preference-for-a-public-ip-address-using-azure-powershell"></a>Настройка предпочтительного варианта маршрутизации для общедоступного IP-адреса с помощью Azure PowerShell

В этой статье показано, как настроить предпочтительный вариант маршрутизации через сеть ISP (параметр **Интернет**) для общедоступного IP-адреса с помощью Azure PowerShell. Созданный общедоступный IP-адрес можно связать со следующими ресурсами Azure для входящего и исходящего Интернет-трафика.

* Виртуальная машина
* Набор масштабирования виртуальных машин
* Служба Azure Kubernetes (AKS)
* подсистема балансировки нагрузки с доступом в Интернет;
* Шлюз приложений
* Брандмауэр Azure

По умолчанию предпочтительный вариант маршрутизации для общедоступного IP-адреса настроен на глобальную сеть Майкрософт для всех служб Azure и его можно связать с любой службой Azure.

> [!IMPORTANT]
> Выбор предпочтительного варианта маршрутизации сейчас предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]
Чтобы установить и использовать PowerShell локально для работы с этой статьей, вам понадобится модуль Azure PowerShell 6.9.0 или более поздней версии. Выполните командлет `Get-Module -ListAvailable Az`, чтобы узнать установленную версию. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-Az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

## <a name="register-the-feature-for-your-subscription"></a>Регистрация функции для подписки
Функция выбора предпочтительного варианта маршрутизации в настоящее время доступна в предварительной версии. Зарегистрируйте функцию для своей подписки следующим образом:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowRoutingPreferenceFeature -ProviderNamespace Microsoft.Network
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Создайте группу ресурсов с помощью командлета [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup). В этом примере создается группа ресурсов с именем *myResourceGroup* в расположении *eastus*:

```azurepowershell
$rg = New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-public-ip-with-internet-routing-preference"></a>Создание общедоступного IP-адреса с предпочтительным вариантом маршрутизации через Интернет

Следующая команда создает новый общедоступный IP-адрес с предпочтительным вариантом маршрутизации *Интернет* в регионе Azure *Восточная часть США*:

```azurepowershell
$iptagtype="RoutingPreference"
$tagName = "Internet"
$ipTag = New-AzPublicIpTag -IpTagType $iptagtype -Tag $tagName 
# attach the tag
$publicIp = New-AzPublicIpAddress  `
-Name "MyPublicIP" `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location `
-IpTag $ipTag `
-AllocationMethod Static `
-Sku Standard `
-IpAddressVersion IPv4
```

Вы можете связать созданный выше общедоступный IP-адрес с виртуальной машиной [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Используйте раздел руководства, посвященный CLI: [Свяжите общедоступный IP-адрес с виртуальной машиной.](associate-public-ip-address-vm.md#azure-cli) Вы также можете связать общедоступный IP-адрес, созданный выше, с [Azure Load Balancer](../load-balancer/load-balancer-overview.md), назначив его конфигурации **внешнего интерфейса** подсистемы балансировки нагрузки. Общедоступный IP-адрес будет выполнять функцию виртуального IP-адреса с балансировкой нагрузки.

## <a name="clean-up-resources"></a>Очистка ресурсов

Вы можете удалить ставшую ненужной группу ресурсов, виртуальную машину и все связанные с ней ресурсы, выполнив команду [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [предпочтительном варианте маршрутизации в общедоступных IP-адресах](routing-preference-overview.md).
- [Настройка предпочтительного варианта маршрутизации для виртуальной машины с помощью Azure PowerShell](configure-routing-preference-virtual-machine-powershell.md)
