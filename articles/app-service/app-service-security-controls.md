---
title: Средства управления безопасностью
description: Найдите контрольный список элементов управления безопасностью для оценки службы приложений Azure в Организации.
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 2586821c4c48f809efb5408c3cdae5e42e3b3fcf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74671452"
---
# <a name="security-controls-for-azure-app-service"></a>Средства управления безопасностью для службы приложений Azure

В этой статье описываются элементы управления безопасностью, встроенные в службу приложений Azure.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Сеть

| Управление безопасностью | Да/нет | Примечания | Документация
|---|---|--|
| Поддержка конечных точек службы| Да | Доступно для службы приложений.| [Ограничения доступа для службы приложений Azure](app-service-ip-restrictions.md)
| Поддержка внедрения виртуальной сети| Да | Среды службы приложений — это частные реализации службы приложений, предназначенные для одного клиента, внедренного в виртуальную сеть клиента. | [Общие сведения о средах службы приложений](environment/intro.md)
| Поддержка сетевой изоляции и брандмауэров| Да | Для общедоступного варианта с несколькими клиентами службы приложений клиенты могут настроить сетевые списки управления доступом (ограничения IP-адресов), чтобы заблокировать разрешенный входящий трафик.  Среды службы приложений развертываются непосредственно в виртуальных сетях, поэтому их можно защитить с помощью группы безопасности сети. | [Ограничения доступа для службы приложений Azure](app-service-ip-restrictions.md)
| Поддержка принудительного туннелирования| Да | Среды службы приложений можно развернуть в виртуальной сети клиента, где настроено принудительное туннелирование. | [Настройка принудительного туннелирования в среде службы приложений](environment/forced-tunnel-support.md)

## <a name="monitoring--logging"></a>Мониторинг & ведения журнала

| Управление безопасностью | Да/нет | Примечания | Документация
|---|---|--|
| Поддержка мониторинга Azure (log Analytics, App Insights и т. д.)| Да | Служба приложений интегрируется с Application Insights для языков, поддерживающих Application Insights (полный .NET Framework, .NET Core, Java и Node. JS).  См. раздел [мониторинг производительности службы приложений Azure](../azure-monitor/app/azure-web-apps.md). Служба приложений также отправляет метрики приложения в Azure Monitor. | [Мониторинг приложений в Службе приложений Azure](web-sites-monitor.md)
| Ведение журнала и аудит в плоскости управления и управления| Да | Все операции управления, выполняемые с объектами службы приложений, выполняются с помощью [Azure Resource Manager](../azure-resource-manager/index.yml). Журналы журнала этих операций доступны как на портале, так и через интерфейс командной строки. | [Операции поставщика ресурсов Azure Resource Manager](../role-based-access-control/resource-provider-operations.md#microsoftweb), [AZ Monitor активности-log](/cli/azure/monitor/activity-log)
| Ведение журнала и аудит в плоскости данных | Нет | Плоскость данных для службы приложений — это удаленная общая папка, содержащая развернутое содержимое веб-сайта клиента.  Аудит удаленного файлового ресурса отсутствует. |

## <a name="identity"></a>Идентификация

| Управление безопасностью | Да/нет | Примечания |  Документация
|---|---|--|
| Аутентификация| Да | Клиенты могут создавать приложения в службе приложений, которые автоматически интегрируются с [Azure Active Directory (Azure AD)](../active-directory/index.yml) , а также с другими совместимыми поставщиками удостоверений OAuth для управления доступом к ресурсам службы приложений. весь доступ управляется сочетанием проверки подлинности участника и Azure Resource Manager ролей RBAC Azure AD. | [Проверка подлинности и авторизация в службе приложений Azure](overview-authentication-authorization.md)
| Авторизация| Да | Для управления доступом к ресурсам службы приложений весь доступ управляется сочетанием авторизованного участника Azure AD и Azure Resource Manager ролей RBAC.  | [Проверка подлинности и авторизация в службе приложений Azure](overview-authentication-authorization.md)

## <a name="data-protection"></a>Защита данных

| Управление безопасностью | Да/нет | Примечания | Документация
|---|---|--|
| Шифрование неактивных на стороне сервера: ключи, управляемые корпорацией Майкрософт | Да | Содержимое файла веб-сайта хранится в службе хранилища Azure, которая автоматически шифрует неактивных данных. <br><br>Секретные секреты, предоставляемые клиентом, шифруются неактивных. Секреты шифруются при хранении в базах данных конфигурации службы приложений.<br><br>Локально подключенные диски можно дополнительно использовать как временное хранилище на веб-сайтах (Д:\локал и% TMP%). Локально подключенные диски не шифруются. | [Шифрование неактивных данных в службе хранилища Azure](../storage/common/storage-service-encryption.md)
| Шифрование неактивных на стороне сервера: ключи, управляемые клиентом (BYOK) | Да | Клиенты могут хранить секреты приложений в Key Vault и извлекать их во время выполнения. | [Использование ссылок на Key Vault в Службе приложений и Функциях Azure (предварительная версия)](app-service-key-vault-references.md)
| Шифрование на уровне столбцов (службы данных Azure)| Недоступно | |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование виртуальной сети)| Да | Клиенты могут настраивать веб-сайты так, чтобы требовать и использовать HTTPS для входящего трафика.  | [Как создать только HTTPS для службы приложений Azure](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/) (запись блога)
| Вызовы API в зашифрованном виде| Да | Вызовы управления для настройки службы приложений выполняются через вызовы [Azure Resource Manager](../azure-resource-manager/index.yml) по протоколу HTTPS. |

## <a name="configuration-management"></a>Управление конфигурацией

| Управление безопасностью | Да/нет | Примечания | Документация
|---|---|--|
| Поддержка управления конфигурацией (управление версиями конфигураций и т. д.)| Да | Для операций управления состояние конфигурации службы приложений можно экспортировать как шаблон Azure Resource Manager и с течением времени с управлением версиями. Для операций среды выполнения клиенты могут поддерживать несколько различных динамических версий приложения с помощью функций слотов развертывания службы приложений. | 

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [встроенных средствах управления безопасностью в службах Azure](../security/fundamentals/security-controls.md).
