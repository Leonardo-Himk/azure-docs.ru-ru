---
title: Средства управления безопасностью
description: Контрольный список встроенных средств управления безопасностью для оценки службы Azure Resource Manager.
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: d0a0625153e428a0d261e52d40b31ef5142eddfd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75485628"
---
# <a name="security-controls-for-azure-resource-manager"></a>Элементы управления безопасностью для Azure Resource Manager

В этой статье описываются элементы управления безопасностью, встроенные в Azure Resource Manager.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Защита данных

| Управление безопасностью | Да/нет | Примечания |
|---|---|--|
| Шифрование неактивных на стороне сервера: ключи, управляемые корпорацией Майкрософт | Да |  |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование виртуальной сети)| Да | HTTPS/TLS. |
| Шифрование неактивных на стороне сервера: ключи, управляемые клиентом (BYOK) | Недоступно | Azure Resource Manager не хранит содержимое клиента, контролируйте только данные. |
| Шифрование на уровне столбцов (службы данных Azure)| Да | |
| Вызовы API в зашифрованном виде| Да | |

## <a name="network"></a>Сеть

| Управление безопасностью | Да/нет | Примечания |
|---|---|--|
| Поддержка конечных точек службы| Нет | |
| Поддержка внедрения виртуальной сети| Да | |
| Поддержка сетевой изоляции и брандмауэров| Нет |  |
| Поддержка принудительного туннелирования| Нет |  |

## <a name="monitoring--logging"></a>Мониторинг & ведения журнала

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Поддержка мониторинга Azure (log Analytics, App Insights и т. д.)| Нет | |
| Ведение журнала и аудит в плоскости управления и управления| Да | Журналы действий предоставляют все операции записи (размещение, публикация, удаление), выполняемые с ресурсами. см. раздел [Просмотр журналов действий для аудита действий с ресурсами](view-activity-logs.md). |
| Ведение журнала и аудит в плоскости данных| Недоступно | |

## <a name="identity"></a>Идентификация

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Аутентификация| Да | На основе [Azure Active Directory](/azure/active-directory) .|
| Авторизация| Да | |

## <a name="configuration-management"></a>Управление конфигурацией

| Управление безопасностью | Да/нет | Примечания|
|---|---|--|
| Поддержка управления конфигурацией (управление версиями конфигураций и т. д.)| Да |  |

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о [встроенных средствах управления безопасностью в службах Azure](../../security/fundamentals/security-controls.md).