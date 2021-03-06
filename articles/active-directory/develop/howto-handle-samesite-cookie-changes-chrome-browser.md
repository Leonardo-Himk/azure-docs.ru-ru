---
title: Как управлять изменениями файлов cookie SameSite в браузере Chrome | Службы
titleSuffix: Microsoft identity platform
description: Узнайте, как управлять изменениями файлов cookie SameSite в браузере Chrome.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: jmprieur
ms.reviewer: kkrishna
ms.custom: aaddev
ms.openlocfilehash: f28d3722d56582bd925d31b43b4a0219bca2ae30
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81534607"
---
# <a name="handle-samesite-cookie-changes-in-chrome-browser"></a>Обработка изменений свойства SameSite в файлах cookie в браузере Chrome

## <a name="what-is-samesite"></a>Что такое SameSite?

`SameSite`— Это свойство, которое можно задать в cookie-файлах HTTP для предотвращения атак с подделкой межсайтовых запросов (CSRF) в веб-приложениях:

- Если `SameSite` параметр имеет значение " **нестрогий**", файл cookie отправляется в запросах на том же сайте и в запросах GET от других сайтов. Он не отправляется в запросах GET между доменами.
- Значение **, равное, гарантирует,** что файл cookie отправляется в запросах только в пределах того же сайта.

По умолчанию `SameSite` значение не задано в браузерах, поэтому нет никаких ограничений на отправку файлов cookie в запросах. Приложению нужно будет присоединиться к защите CSRF, **задав строгие или** **строгие** требования в соответствии с требованиями.

## <a name="samesite-changes-and-impact-on-authentication"></a>SameSite изменения и влияние на проверку подлинности

Последние [обновления стандартов в SameSite](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) предлагают защиту приложений, представляя поведение по умолчанию, `SameSite` если значение не задано как слабое. Это означает, что файлы cookie будут ограничены HTTP-запросами, за исключением случаев, выполненных с других сайтов. Кроме того, для удаления ограничений на отправляемые файлы cookie введено значение **None** . Эти обновления вскоре будут выпущены в следующей версии браузера Chrome.

Когда веб-приложения проходят проверку подлинности с помощью платформы Microsoft Identity, используя режим ответа "form_post", сервер входа отвечает на приложение, используя HTTP-запрос POST для отправки токенов или кода проверки подлинности. Так как этот запрос является междоменным запросом ( `login.microsoftonline.com` от вашего домена к примеру `https://contoso.com/auth`), файлы cookie, заданные приложением, теперь будут находиться под новыми правилами в Chrome. Файлы cookie, которые необходимо использовать в сценариях с несколькими узлами, — это файлы cookie, содержащие значения *State* и *nonce* , которые также отправляются в запросе на вход. Azure AD отбрасывает другие файлы cookie для хранения сеанса.

Если вы не обновите веб-приложения, это новое поведение приведет к сбоям проверки подлинности.

## <a name="mitigation-and-samples"></a>Устранение рисков и примеры

Чтобы преодолеть ошибки проверки подлинности, веб-приложения, прошедшие проверку подлинности `SameSite` на платформе `None` Microsoft Identity, могут задать для свойства значение для файлов cookie, которые используются в междоменных сценариях при запуске в браузере Chrome.
Другие браузеры (см. [здесь](https://www.chromium.org/updates/same-site/incompatible-clients) полный список) следуют предыдущему поведению `SameSite` и не включают файлы cookie, `SameSite=None` если задан параметр.
Поэтому для поддержки проверки подлинности в нескольких браузерах веб-приложения необходимо установить `SameSite` значение `None` только для Chrome и оставить значение пустым в других браузерах.

Этот подход показан в примерах кода ниже.

# <a name="net"></a>[.NET](#tab/dotnet)

В следующей таблице представлены запросы на включение внесенных изменений, обработавшие изменения SameSite в наших примерах ASP.NET и ASP.NET Core.

| Пример | Запрос на вытягивание |
| ------ | ------------ |
|  [Пошаговое руководство по ASP.NET Core веб-приложения](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)  |  [Исправление файла cookie того же сайта #261](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/pull/261)  |
|  [Пример веб-приложения ASP.NET MVC](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)  |  [Исправление файла cookie того же сайта #35](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/pull/35)  |
|  [Active-Directory-DotNet-Admin-ограниченные области — v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2)  |  [Исправление файла cookie того же сайта #28](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2/pull/28)  |

Дополнительные сведения об обработке файлов cookie SameSite в ASP.NET и ASP.NET Core см. в разделе также:

- [Работа с файлами cookie SameSite в ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/samesite) .
- [Блог ASP.NET о проблемах SameSite](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)

# <a name="python"></a>[Python](#tab/python)

| Пример |
| ------ |
|  [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp)  |

# <a name="java"></a>[Java](#tab/java)

| Пример | Запрос на вытягивание |
| ------ | ------------ |
|  [MS-Identity-Java-webapp](https://github.com/Azure-Samples/ms-identity-java-webapp)  | [Исправление файла cookie того же сайта #24](https://github.com/Azure-Samples/ms-identity-java-webapp/pull/24)
|  [MS-Identity-Java-webapi](https://github.com/Azure-Samples/ms-identity-java-webapi)  | [Исправление файла cookie того же сайта #4](https://github.com/Azure-Samples/ms-identity-java-webapi/pull/4)

---

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о SameSite и сценарии веб-приложения:

> [!div class="nextstepaction"]
> [Вопросы и ответы о Google Chrome в SameSite](https://www.chromium.org/updates/same-site/faq)

> [!div class="nextstepaction"]
> [Страница Chromium SameSite](https://www.chromium.org/updates/same-site)

> [!div class="nextstepaction"]
> [Сценарий: веб-приложение, которое входит в систему пользователей](scenario-web-app-sign-user-overview.md)