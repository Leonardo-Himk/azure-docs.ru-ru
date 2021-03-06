---
title: Развертывание виртуальных машин точки Azure с помощью интерфейса командной строки
description: Узнайте, как с помощью интерфейса командной строки развертывать виртуальные машины в машинном обучении Azure для экономии затрат.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/25/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 3f341271c208cc56a704c836433c33af0129a4ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81758360"
---
# <a name="deploy-spot-vms-using-the-azure-cli"></a>Развертывание плашечных виртуальных машин с помощью Azure CLI

[Виртуальные машины Azure для машинного](spot-vms.md) размещения позволяют использовать преимущества неиспользуемой емкости с значительной экономией затрат. В любой момент, когда Azure нужна емкость, инфраструктура Azure будет выключать плашечные виртуальные машины. Таким образом, точечные виртуальные машины отлично подходят для рабочих нагрузок, которые могут обрабатывать прерывания, такие как задания пакетной обработки, среды разработки и тестирования, большие вычислительные рабочие нагрузки и многое другое.

Цены для плашечных виртуальных машин являются переменными в зависимости от региона и SKU. Дополнительные сведения см. в статье цены на виртуальные машины для [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) и [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 

Вы можете задать максимальную цену, которую вы хотите платить, в час, для виртуальной машины. Максимальная цена на плашечную виртуальную машину можно установить в долларах США (USD), используя до 5 десятичных разрядов. Например, значением `0.98765`будет максимальная цена $0,98765 долл. США в час. Если для параметра Максимальная цена задано значение `-1`, виртуальная машина не будет удалена на основе цены. Цена на виртуальную машину будет соответствовать текущей цене или цене для стандартной виртуальной машины, которая когда-либо будет меньше, если есть доступная емкость и квота. Дополнительные сведения о настройке максимальной цены см. в разделе [точки на виртуальных машинах — цены](spot-vms.md#pricing).

Процесс создания виртуальной машины с использованием точки, использующей Azure CLI, аналогичен описанному в [статье Краткое руководство](/azure/virtual-machines/linux/quick-create-cli). Просто добавьте параметр "--Priority плашка" и укажите максимальную цену или `-1`.


## <a name="install-azure-cli"></a>Установка Azure CLI

Чтобы создать плашечные виртуальные машины, необходимо запустить Azure CLI версии 2.0.74 или более поздней. Чтобы узнать версию, выполните команду **az --version**. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI](/cli/azure/install-azure-cli). 

Затем войдите в Azure с помощью команды [az login](/cli/azure/reference-index#az-login).

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Создание плашечной виртуальной машины

В этом примере показано, как развернуть плашечную виртуальную машину Linux, которая не будет исключена на основе цены. 

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1
```

После создания виртуальной машины можно выполнить запрос, чтобы увидеть максимальную стоимость выставления счетов для всех виртуальных машин в группе ресурсов.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Дальнейшие действия**

Можно также создать плашечную виртуальную машину с помощью [Azure PowerShell](../windows/spot-powershell.md) или [шаблона](spot-template.md).

Если возникает ошибка, см. раздел [коды ошибок](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
