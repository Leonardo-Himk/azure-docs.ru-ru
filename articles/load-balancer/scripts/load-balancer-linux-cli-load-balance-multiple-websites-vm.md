---
title: Распределение нагрузки на нескольких веб-сайтах — Azure CLI — Azure Load Balancer
description: В этом примере скрипта Azure CLI показано, как распределить нагрузку нескольких веб-сайтах на одной виртуальной машине
documentationcenter: load-balancer
author: asudbring
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: sample
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: allensu
ms.openlocfilehash: 80ec18ca722b9833307fc270c710abd93c6fb6f9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "76274617"
---
# <a name="azure-cli-script-example-load-balance-multiple-websites"></a>Пример скрипта Azure CLI. Распределение нагрузки веб-сайтов

Этот пример скрипта Azure CLI создает виртуальную сеть с двумя виртуальными машинами, которые входят в группу доступности. Подсистема балансировки нагрузки направляет трафик с двух отдельных IP-адресов на две виртуальные машины. После выполнения сценария можно развернуть ПО веб-сервера на виртуальных машин и разместить несколько веб-сайтов, каждый из которых имеет собственный IP-адрес.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной сети, подсистемы балансировки нагрузки и всех связанных ресурсов этот сценарий использует приведенные ниже команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az-network-vnet-create) | Создает виртуальную сеть и подсеть Azure. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-create) | Создает общедоступный IP-адрес со статическим IP-адресом и связанным DNS-именем. |
| [az network lb create](https://docs.microsoft.com/cli/azure/network/lb#az-network-lb-create) | Создает службу Azure Load Balancer. |
| [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe#az-network-lb-probe-create) | Создает зонд подсистемы балансировки нагрузки. Зонд подсистемы балансировки нагрузки используется для мониторинга каждой виртуальной машины в наборе подсистемы балансировки нагрузки. Если любая виртуальная машина становится недоступной, к ней не направляется трафик. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule#az-network-lb-rule-create) | Создает правило подсистемы балансировки нагрузки. В этом примере создается правило для порта 80. Так как трафик HTTP поступает в подсистему балансировки нагрузки, он перенаправляется на порт 80 одной из виртуальных машин в наборе подсистемы балансировки нагрузки. |
| [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#az-network-lb-frontend-ip-create) | Создает интерфейсный IP-адрес для подсистемы балансировки нагрузки. |
| [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool#az-network-lb-address-pool-create) | Создает внутренний пул адресов. |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic#az-network-nic-create) | Создает виртуальную сетевую карту и подключает ее к виртуальной сети и подсети. |
| [az vm availability-set create](https://docs.microsoft.com/cli/azure/network/lb/rule#az-network-lb-rule-create) | Создает группу доступности. Группы доступности обеспечивают непрерывную работу приложения, распределяя виртуальные машины по физическим ресурсам. Таким образом, в случае сбоя он не затронет весь набор ресурсов. |
| [az network nic ip-config create](https://docs.microsoft.com/cli/azure/network/nic/ip-config#az-network-nic-ip-config-create) | Создает IP-конфигурацию. Для подписки должна быть включена функция Microsoft.Network/AllowMultipleIpConfigurationsPerNic. В качестве основной IP-конфигурации каждой сетевой карте можно назначить только одну конфигурацию. Для этого используется флаг --make-primary. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#az-vm-availability-set-create) | Создает виртуальную машину и подключает ее к сетевой карте, виртуальной сети, подсети и группе безопасности сети. Эта команда также указывает образ виртуальной машины, который будет использоваться, и учетные данные администратора.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры сценариев Azure CLI для сетей см. в статье [Примеры Azure CLI](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
