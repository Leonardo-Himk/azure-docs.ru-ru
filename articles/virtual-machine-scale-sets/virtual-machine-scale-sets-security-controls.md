---
title: Элементы управления безопасностью для масштабируемых наборов виртуальных машин Azure
description: Контрольный список средств управления безопасностью для оценки масштабируемых наборов виртуальных машин Azure
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: security
ms.date: 09/05/2019
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 030e2c23d68a3fbbc96dd7591583cb27b650d011
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83200016"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Элементы управления безопасностью для масштабируемых наборов виртуальных машин Azure

В этой статье описываются элементы управления безопасностью, встроенные в масштабируемые наборы виртуальных машин Azure.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Сеть

| Управление безопасностью | Да/нет | Примечания |
|---|---|--|
| Поддержка конечных точек службы| Да | |
| Поддержка внедрения виртуальной сети| Да | |
| Поддержка сетевой изоляции и брандмауэров| Да |  |
| Поддержка принудительного туннелирования| Да | См. раздел [Настройка принудительного туннелирования с помощью модели развертывания Azure Resource Manager](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>Мониторинг & ведения журнала

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Поддержка мониторинга Azure (log Analytics, App Insights и т. д.)| Да | См. статью [мониторинг и обновление виртуальной машины Linux в Azure](/azure/virtual-machines/linux/tutorial-monitoring) и [Отслеживание и обновление виртуальной машины Windows в Azure](/azure/virtual-machines/windows/tutorial-monitoring). |
| Ведение журнала и аудит в плоскости управления и управления| Да |  |
| Ведение журнала и аудит в плоскости данных | Нет |  |

## <a name="identity"></a>Идентификация

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Аутентификация| Да |  |
| Авторизация| Да |  |

## <a name="data-protection"></a>Защита данных

| Управление безопасностью | Да/нет | Примечания |
|---|---|--|
| Шифрование неактивных на стороне сервера: ключи, управляемые корпорацией Майкрософт | Да | См. раздел [Шифрование дисков Azure для масштабируемых наборов виртуальных машин](disk-encryption-overview.md). |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование виртуальной сети)| Да | Виртуальные машины Azure поддерживают шифрование [ExpressRoute](/azure/expressroute) и виртуальной сети. См. раздел [Шифрование транзитного пути в виртуальных машинах](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Шифрование неактивных на стороне сервера: ключи, управляемые клиентом (BYOK) | Да | Ключи, управляемые клиентом, являются поддерживаемым сценарием шифрования Azure. см. раздел [о шифровании дисков Azure для масштабируемых наборов виртуальных машин](disk-encryption-overview.md) .|
| Шифрование на уровне столбцов (службы данных Azure)| Н/Д | |
| Вызовы API в зашифрованном виде| Да | Через HTTPS и TLS. |

## <a name="configuration-management"></a>Управление конфигурацией

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Поддержка управления конфигурацией (управление версиями конфигураций и т. д.)| Да |  | 

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [встроенных средствах управления безопасностью в службах Azure](../security/fundamentals/security-controls.md).
