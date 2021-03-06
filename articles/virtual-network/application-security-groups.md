---
title: Общие сведения о группах безопасности приложений Azure
titlesuffix: Azure Virtual Network
description: Сведения об использовании групп безопасности приложений.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2020
ms.author: kumud
ms.reviewer: kumud
ms.openlocfilehash: 775ef92a0ca486d1f8a6c44c78a4df04cd5ef467
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78274714"
---
# <a name="application-security-groups"></a>Группы безопасности приложений

Группы безопасности приложений позволяют настроить сетевую безопасность как естественное расширение в структуре приложения. Это позволяет группировать виртуальные машины и определять политики безопасности сети на основе таких групп. Вы можете повторно использовать политику безопасности в нужном масштабе без обслуживания явных IP-адресов вручную. Платформа сама обрабатывает явные IP-адреса и множество наборов правил, позволяя вам сосредоточиться на бизнес-логике. Чтобы лучше понять группы безопасности приложений, рассмотрим следующий пример:

![Группы безопасности приложений](./media/security-groups/application-security-groups.png)

На предыдущем рисунке *NIC1* и *NIC2* — это члены группы безопасности приложений *AsgWeb*. *NIC3* — это член группы безопасности приложений *AsgLogic*. *NIC4* — это член группы безопасности приложений *AsgDb*. Несмотря на то что в этом примере каждый сетевой интерфейс является членом только одной группы безопасности приложений, он может быть членом нескольких групп безопасности приложений в соответствии с [ограничениями Azure](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Ни один из сетевых интерфейсов не имеет связанных групп безопасности сети. *NSG1* связана с обеими подсетями и содержит следующие правила.

## <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Это правило требуется для разрешения трафика из Интернета на веб-серверы. Так как входящий трафик из Интернета запрещен правилом безопасности по умолчанию **DenyAllInbound**, для групп безопасности приложений *AsgLogic* или *AsgDb* дополнительных правил не требуется.

|Priority|Источник|Исходные порты| Назначение | Конечные порты | Протокол | Доступ |
|---|---|---|---|---|---|---|
| 100 | Интернет | * | AsgWeb | 80 | TCP | Allow |

## <a name="deny-database-all"></a>Deny-Database-All

Так как правило безопасности по умолчанию **AllowVNetInBound** разрешает весь обмен данными между ресурсами в одной и той же виртуальной сети, это правило необходимо для запрета трафика из всех ресурсов.

|Priority|Источник|Исходные порты| Назначение | Конечные порты | Протокол | Доступ |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Любой | Запрет |

## <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Это правило разрешает трафик из группы безопасности приложений *AsgLogic* в группу безопасности приложений *AsgDb*. Приоритет для этого правила выше, чем приоритет правила *Deny-Database-All*. Таким образом, это правило обрабатывается до правила *Deny-Database-All*, поэтому трафик из группы безопасности приложений *AsgLogic* разрешен, тогда как весь остальной трафик запрещен.

|Priority|Источник|Исходные порты| Назначение | Конечные порты | Протокол | Доступ |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | Allow |

Правила, определяющие группу безопасности приложений в качестве источника или назначения, применяются только к сетевым интерфейсам, которые входят в ее состав. Если сетевой интерфейс не является членом группы безопасности приложений, правило не применяется к нему, несмотря на то, что группа безопасности сети связана с подсетью.

Группы безопасности приложений имеют следующие ограничения:

-    Есть ограничения на количество групп безопасности приложений в подписке, а также другие ограничения, связанные с группами безопасности приложений. Дополнительные сведения см. в разделе [Ограничения сети](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Вы можете указать одну группу безопасности приложений в правиле безопасности как источник и назначение. Указать несколько групп безопасности приложений как источник или назначение нельзя.
- Все сетевые интерфейсы, назначенные группе безопасности приложений, должны существовать в той же виртуальной сети, где размещался первый сетевой интерфейс, назначенный этой группе безопасности приложений. Например, если группе безопасности приложений был первым назначен сетевой интерфейс с именем *AsgWeb* в виртуальной сети *VNet1*, все остальные сетевые интерфейсы, назначаемые *ASGWeb*, должны существовать в *VNet1*. Невозможно добавить сетевые интерфейсы из разных виртуальных сетей в одну группу безопасности приложения.
- При указании группы безопасности приложений в качестве источника и назначения в правиле безопасности сетевые интерфейсы в обеих группах безопасности приложения должны существовать в одной виртуальной сети. Например, если *AsgLogic* содержит сетевые интерфейсы из *VNet1*, а *AsgDb* — сетевые интерфейсы из *VNet2*, вы не сможете назначить *AsgLogic* в качестве источника и *AsgDb* в качестве назначения в правиле. Все сетевые интерфейсы для групп безопасности приложений источника и назначения должны существовать в одной и той же виртуальной сети.

> [!TIP]
> Чтобы свести к минимуму количество требуемых правил безопасности и необходимость изменять правила, составьте план необходимых групп безопасности приложений и создайте правила, по возможности используя теги служб или группы безопасности приложений вместо отдельных IP-адресов или диапазонов IP-адресов.

## <a name="next-steps"></a>Дальнейшие шаги

* Узнайте, как [создать группу безопасности сети](tutorial-filter-network-traffic.md).
