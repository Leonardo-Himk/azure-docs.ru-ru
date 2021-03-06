---
title: Элементы управления безопасностью для Azure Relay
description: В этой статье представлен контрольный список встроенных средств управления безопасностью для оценки Azure Relay.
services: service-bus-relay
ms.service: service-bus-relay
author: spelluru
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: f8165d994e998af4f15cd6aa2fd08b75191b8b64
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83211465"
---
# <a name="security-controls-for-azure-relay"></a>Элементы управления безопасностью для Azure Relay

В этой статье описываются элементы управления безопасностью, встроенные в Azure Relay.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Сеть

| Управление безопасностью | Да/нет | Примечания | Документация |
|---|---|--|--|
| Поддержка конечных точек службы| Нет |  |   |
| Поддержка сетевой изоляции и брандмауэров| Нет |  |   |
| Поддержка принудительного туннелирования| Н/Д | Ретранслятором является туннель TLS.  |   |

## <a name="monitoring--logging"></a>Мониторинг & ведения журнала

| Управление безопасностью | Да/нет | Примечания| Документация |
|---|---|--|--|
| Поддержка мониторинга Azure (log Analytics, App Insights и т. д.)| Да | |   |
| Ведение журнала и аудит в плоскости управления и управления| Да | С помощью [Azure Resource Manager](../azure-resource-manager/index.yml). |   |
| Ведение журнала и аудит в плоскости данных| Да | Успешное подключение, сбой, ошибки и журнал.  |   |

## <a name="identity"></a>Идентификация

| Управление безопасностью | Да/нет | Примечания| Документация |
|---|---|--|--|
| Аутентификация| Да | Через SAS. | [Проверка подлинности и авторизация в ретрансляторе Azure](relay-authentication-and-authorization.md) |
| Авторизация|  Да | Через SAS. | [Проверка подлинности и авторизация в ретрансляторе Azure](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Защита данных

| Управление безопасностью | Да/нет | Примечания | Документация |
|---|---|--|--|
| Шифрование неактивных на стороне сервера: ключи, управляемые корпорацией Майкрософт |  Н/Д | Ретранслятор — это веб-сокет, который не сохраняет данные. |   |
| Шифрование неактивных на стороне сервера: ключи, управляемые клиентом (BYOK) | Нет | Использует только сертификаты Microsoft TLS.  |   |
| Шифрование на уровне столбцов (службы данных Azure)| Н/Д | |   |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование виртуальной сети)| Да | Для службы требуется TLS. |   |
| Вызовы API в зашифрованном виде| Да | Протокол. |


## <a name="configuration-management"></a>Управление конфигурацией

| Управление безопасностью | Да/нет | Примечания| Документация |
|---|---|--|--|
| Поддержка управления конфигурацией (управление версиями конфигураций и т. д.)| Да | С помощью [Azure Resource Manager](../azure-resource-manager/index.yml).|   |

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [встроенных средствах управления безопасностью в службах Azure](../security/fundamentals/security-controls.md).