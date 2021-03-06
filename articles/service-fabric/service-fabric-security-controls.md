---
title: Средства управления безопасностью для Azure Service Fabric
description: Сведения об элементах управления безопасностью для Azure Service Fabric. Содержит контрольный список встроенных средств управления безопасностью.
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: a8bb49e20ec5812a4882966c6918cf2bd59f36a0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75645435"
---
# <a name="security-controls-for-azure-service-fabric"></a>Средства управления безопасностью для Azure Service Fabric

В этой статье описываются элементы управления безопасностью, встроенные в Azure Service Fabric. 

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Сеть

| Управление безопасностью | Да/нет | Примечания |
|---|---|--|
| Поддержка конечных точек службы| Да |  |
| Поддержка внедрения виртуальной сети| Да |  |
| Поддержка сетевой изоляции и брандмауэров| Да | Использование групп безопасности сети (NSG). |
| Поддержка принудительного туннелирования| Да | Сеть Azure предоставляет принудительное туннелирование. |

## <a name="monitoring--logging"></a>Мониторинг & ведения журнала

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Поддержка мониторинга Azure (log Analytics, App Insights и т. д.)| Да | Использование поддержки мониторинга Azure и поддержки сторонних производителей. |
| Ведение журнала и аудит в плоскости управления и управления| Да | Все операции в плоскости управления проходят через процессы аудита и утверждения. |
| Ведение журнала и аудит в плоскости данных| Недоступно | Клиенту принадлежит кластер.  |

## <a name="identity"></a>Идентификация

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Аутентификация| Да | Аутентификация выполняется с помощью Azure Active Directory. |
| Авторизация| Да | Система управления идентификацией и доступом (IAM) для вызовов через SFRP. Вызовы непосредственно к конечной точке кластера поддерживают две роли: User и Admin. Клиент может сопоставлять API с любой из ролей. |

## <a name="data-protection"></a>Защита данных

| Управление безопасностью | Да/нет | Примечания |
|---|---|--|
| Шифрование неактивных на стороне сервера: ключи, управляемые корпорацией Майкрософт | Да | Клиент владеет кластером и масштабируемым набором виртуальных машин, на котором построен кластер. Шифрование дисков Azure можно включить в масштабируемом наборе виртуальных машин. |
| Шифрование неактивных на стороне сервера: ключи, управляемые клиентом (BYOK) | Да | Клиент владеет кластером и масштабируемым набором виртуальных машин, на котором построен кластер. Шифрование дисков Azure можно включить в масштабируемом наборе виртуальных машин. |
| Шифрование на уровне столбцов (службы данных Azure)| Недоступно |  |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование виртуальной сети)| Да |  |
| Вызовы API в зашифрованном виде| Да | Вызовы Service Fabric API выполняются через Azure Resource Manager. Требуется допустимый веб-маркер JSON (JWT). |

## <a name="configuration-management"></a>Управление конфигурацией

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Поддержка управления конфигурацией (управление версиями конфигураций и т. д.)| Да | |

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о [встроенных средствах управления безопасностью в службах Azure](../security/fundamentals/security-controls.md).