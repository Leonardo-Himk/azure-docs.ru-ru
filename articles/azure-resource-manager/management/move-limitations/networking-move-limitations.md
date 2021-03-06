---
title: Перемещение сетевых ресурсов Azure в новую подписку или группу ресурсов
description: Используйте Azure Resource Manager для перемещения виртуальных сетей и других сетевых ресурсов в новую группу ресурсов или подписку.
ms.topic: conceptual
ms.date: 10/16/2019
ms.openlocfilehash: 0cd6887d3489f2ffede0f5e3d63533a33a6ccc04
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75485238"
---
# <a name="move-guidance-for-networking-resources"></a>Руководство по перемещению сетевых ресурсов

В этой статье описывается перемещение виртуальных сетей и других сетевых ресурсов для конкретных сценариев.

## <a name="dependent-resources"></a>Зависимые ресурсы

При перемещении виртуальной сети также необходимо переместить зависимые от нее ресурсы, Для VPN-шлюзов необходимо переместить IP-адреса, шлюзы виртуальной сети и все связанные с ними ресурсы подключения. Локальные сетевые шлюзы могут находиться в другой группе ресурсов.

Чтобы переместить виртуальную машину с сетевой картой в новую подписку, необходимо переместить все зависимые ресурсы. Переместите виртуальную сеть для сетевой карты, все остальные сетевые адаптеры для виртуальной сети и VPN-шлюзы.

Дополнительные сведения см. в разделе [сценарий перемещения по подпискам](../move-resource-group-and-subscription.md#scenario-for-move-across-subscriptions).

## <a name="peered-virtual-network"></a>Одноранговая виртуальная сеть

Чтобы переместить виртуальную сеть с пиринговым подключением, сначала нужно отключить это подключение. После отключения виртуальную сеть можно переместить. После перемещения установите пиринговое подключение виртуальной сети заново.

## <a name="subnet-links"></a>Ссылки на подсети

Виртуальную сеть невозможно переместить в другую подписку, если эта сеть содержит подсеть со ссылками перехода к ресурсам. Например, если ресурс кэша Azure для Redis развернут в подсеть, эта подсеть содержит ссылку навигации по ресурсам.

## <a name="next-steps"></a>Дальнейшие действия

Команды для перемещения ресурсов см. в статье [Перемещение ресурсов в новую группу ресурсов или подписку](../move-resource-group-and-subscription.md).
