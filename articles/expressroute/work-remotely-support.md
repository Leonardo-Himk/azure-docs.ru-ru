---
title: Использование Azure ExpressRoute для поддержки удаленных пользователей
description: На этой странице описывается, как можно использовать Azure ExpressRoute для удаленной работы из-за гриппа КОВИД-19.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 03/22/2020
ms.author: ajitbhu
ms.openlocfilehash: 2fe990fd9c5922dd2e059dbb212cd8bb1cd3726e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80336651"
---
# <a name="using-azure-expressroute-to-create-hybrid-connectivity-to-support-remote-users"></a>Использование Azure ExpressRoute для создания гибридного подключения для поддержки удаленных пользователей

В этой статье описывается, как можно использовать ExpressRoute, Azure, Microsoft Network и партнерскую экосистему Azure для удаленной работы.

## <a name="connecting-to-azure-services-with-expressroute"></a>Подключение к службам Azure с помощью ExpressRoute

>[!NOTE]
>В этой статье описывается, как можно использовать ExpressRoute, Azure, Microsoft Network и партнерскую экосистему Azure для удаленной работы и устранения проблем с сетью, связанных с КОВИД-19 критическими сбоями.
>

[ExpressRoute](expressroute-introduction.md) — это служба Azure, которая обеспечивает частное подключение между центрами обработки данных Майкрософт и инфраструктурой в вашей локальной среде или в средстве совместного размещения. Подключения ExpressRoute не проходят через общедоступный Интернет. Они обеспечивают безопасное подключение, надежность и скорость, с низкой и постоянной задержкой по сравнению с обычными подключениями через Интернет.

## <a name="useful-links"></a>Полезные ссылки

* [Увеличение пропускной способности для существующей цепи ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [Мониторинг ExpressRoute, метрики и оповещения](expressroute-monitoring-metrics-alerts.md#expressroute-gateway-connections-in-bitsseconds)
* [Оптимизация маршрутов через ExpressRoute](expressroute-optimize-routing.md)
* [Azure ExpressRoute для Office 365](https://docs.microsoft.com/office365/enterprise/azure-expressroute?redirectSourcePath=%252farticle%252f6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Рекомендации по асимметричной маршрутизации](expressroute-asymmetric-routing.md)
* [Как открыть запрос в службу поддержки в портал Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

## <a name="troubleshoot"></a>Устранение неполадок

* [Проверка подключения ExpressRoute](expressroute-troubleshooting-expressroute-overview.md).
* Получение таблицы ARP в [Диспетчер ресурсов](expressroute-troubleshooting-arp-resource-manager.md) и [классической](expressroute-troubleshooting-arp-classic.md) модели
* [Сброс канала с ошибками](reset-circuit.md)
* [Устранение неполадок производительности сети](expressroute-troubleshooting-network-performance.md)

## <a name="next-steps"></a>Дальнейшие шаги

* [Дополнительные сведения о моделях подключения ExpressRoute](expressroute-connectivity-models.md)
* Узнайте больше о подключениях ExpressRoute и доменах маршрутизации. См. раздел [каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md) .
* Найти поставщика услуг. См. раздел [партнеры и расположения пиринга ExpressRoute](expressroute-locations.md) .
* Убедитесь, что выполнены все необходимые условия. Ознакомьтесь с разделом [Предварительные требования и контрольный список для ExpressRoute](expressroute-prerequisites.md).
* [Создание и изменение канала ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
* [Создание и изменение пиринга для канала ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
* [Подключение виртуальной сети к каналу ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
