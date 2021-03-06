---
title: API Microsoft Graph
description: Microsoft Graph API — это веб-API RESTFUL, который позволяет получать доступ к Microsoft Cloud ресурсам службы.
author: davidmu1
services: active-directory
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/13/2020
ms.author: davidmu
ms.custom: aaddev
ms.openlocfilehash: 67dbf696903e7a930d75762deb00ad58ed1a4f69
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80886472"
---
# <a name="microsoft-graph-api"></a>API Microsoft Graph

Microsoft Graph API — это веб-API RESTFUL, который позволяет получать доступ к Microsoft Cloud ресурсам службы. После регистрации приложения и получения маркеров проверки подлинности для пользователя или службы можно выполнить запросы к Microsoft Graph API. Дополнительные сведения см. в разделе Общие сведения о [Microsoft Graph](https://docs.microsoft.com/graph/overview).

Microsoft Graph предоставляет интерфейсы API и клиентские библиотеки для доступа к данным в следующих Microsoft 365 службах:
- Office 365 Services: Delve, Excel, Microsoft Books, Microsoft Teams, OneDrive, OneNote, Outlook/Exchange, Planner и SharePoint
- Корпоративные мобильные службы и безопасность: Advanced Threat Analytics, Advanced Threat Protection, Azure Active Directory, Configuration Manager и Intune
- Службы Windows 10: действия, устройства, уведомления
- Dynamics 365 Business Central

## <a name="versions"></a>Версии

В настоящее время Microsoft Graph поддерживает две версии: v 1.0 и Beta. Версия 1.0 включает в себя общедоступные API. Используйте версию 1.0 для всех рабочих приложений. Бета-версия включает API, которые сейчас доступны в предварительной версии. Поскольку мы можем внести критические изменения в наши бета-версии API, мы рекомендуем использовать бета-версию только для тестирования приложений, которые находятся в разработке. не используйте бета-версии API в рабочих приложениях. Дополнительные сведения см. в разделе [Управление версиями, поддержка и критические политики изменения для Microsoft Graph](https://docs.microsoft.com/graph/versioning-and-support).

Чтобы приступить к использованию бета-версий API, см. статью [Справочник по конечной точке бета-версии Microsoft Graph](https://docs.microsoft.com/graph/api/overview?view=graph-rest-beta)

Чтобы начать работу с API-интерфейсами версии 1.0, ознакомьтесь со статьей [Справочник по Microsoft Graph REST API v 1.0](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)

## <a name="get-started"></a>Начало работы

Для чтения или записи ресурса, например пользователя или сообщения электронной почты, создается запрос, который выглядит следующим образом:

`{HTTP method} https://graph.microsoft.com/{version}/{resource}?{query-parameters}`

Дополнительные сведения об элементах сконструированного запроса см. в разделе [Использование API Microsoft Graph](https://docs.microsoft.com/graph/use-the-api) .

Примеры быстрого запуска позволяют продемонстрировать, как получить доступ к преимуществам API Microsoft Graph. Примеры, доступные для доступа к двум службам с использованием одной проверки подлинности: учетная запись Майкрософт и Outlook. Каждое краткое руководство обращается к информации из профилей пользователей учетная запись Майкрософт и отображает события из их календаря.
В кратком руководстве используются четыре шага:
- Выбор платформы
- Получение идентификатора приложения (идентификатор клиента)
- Сборка примера
- Вход и Просмотр событий в календаре

По завершении работы с кратким руководством вы получите приложение, готовое к запуску. Дополнительные сведения см. в разделе [часто задаваемые вопросы о Microsoft Graph](https://docs.microsoft.com/graph/quick-start-faq). Чтобы приступить к работе с примерами, см. [Microsoft Graph краткое руководство](https://developer.microsoft.com/graph/quick-start).

## <a name="tools"></a>Инструменты

Microsoft Graph Explorer — это веб-инструмент, который можно использовать для создания и тестирования запросов с помощью Microsoft Graph API. Доступ к Microsoft Graph Explorer можно получить по `https://developer.microsoft.com/graph/graph-explorer`адресу:.

POST представляет собой средство, которое можно также использовать для построения и тестирования запросов с помощью API-интерфейсов Microsoft Graph. Вы можете загрузить posts по адресу: `https://www.getpostman.com/`. Для взаимодействия с Microsoft Graph в POST используется коллекция Microsoft Graph в POST. Дополнительные сведения см. [в статье Использование POST с API Microsoft Graph](/graph/use-postman?context=graph%2Fapi%2Fbeta&view=graph-rest-beta).
