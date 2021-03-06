---
title: Выбор протокола Федерации для приложения с несколькими клиентами
description: Руководство для независимых поставщиков программного обеспечения при интеграции с Azure Active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b3edbbe037c3874d639476e516b3732b7573d9b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75443373"
---
# <a name="choose-the-right-federation-protocol-for-your-multi-tenant-application"></a>Выбор правильного протокола Федерации для приложения с несколькими клиентами

При разработке приложения SaaS необходимо выбрать протокол Федерации, который наилучшим образом соответствует потребностям ваших клиентов. Это решение основано на вашей платформе разработки, а ваше желание интегрироваться с данными, доступными в Office 365 и экосистеме Azure AD.

См. полный список [протоколов, доступных для интеграции единого входа](what-is-single-sign-on.md) с Azure Active Directory.
В следующей таблице сравниваются 
* Открытая проверка подлинности 2,0 (OAuth 2,0)
* Open ID Connect (OIDC)
* Security Assertion Markup Language (SAML)
* Web Services Federation (WSFed)

| Функция| OAuth/OIDC| SAML/WSFed |
| - |-|-|
| Единый вход на основе веб-сайта| √| √ |
| Единый выход через Интернет| √| √ |
| Единый вход на основе мобильных устройств| √| √* |
| Единый выход на основе мобильных устройств| √| √* |
| Политики условного доступа для мобильных приложений| √| X |
| Простой интерфейс MFA для мобильных приложений| √| X |
| Microsoft Graph доступа| √| X |

* Возможно, но корпорация Майкрософт не предоставляет примеры или рекомендации.

## <a name="oauth-20-and-open-id-connect"></a>OAuth 2,0 и Open ID Connect

OAuth 2,0 является [стандартным промышленным](https://oauth.net/2/) протоколом для авторизации. OIDC (OpenID Connect Connect) — это [стандартный](https://openid.net/connect/) уровень проверки подлинности идентификации на основе протокола OAuth 2,0.

### <a name="benefits"></a>Преимущества

Корпорация Майкрософт рекомендует использовать OIDC/OAuth 2,0, так как они имеют встроенную проверку подлинности и авторизацию для протоколов. При использовании SAML необходимо дополнительно реализовать авторизацию.

Авторизация, реализованная в этих протоколах, позволяет приложению получать доступ к данным с богатыми данными пользователя и Организации через API Microsoft Graph и интегрировать их с ними.

Использование OAuth 2,0 и OIDC упрощает работу пользователей с конечными пользователями при внедрении единого входа для вашего приложения. Вы можете легко определить необходимые наборы разрешений, которые затем автоматически представлены администратором или конечным пользователем.

Кроме того, использование этих протоколов позволяет клиентам использовать политики условного доступа и MFA для управления доступом к приложениям. Корпорация Майкрософт предоставляет библиотеки и [примеры кода на различных технологических платформах](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples) , чтобы помочь в разработке.  

### <a name="implementation"></a>Реализация

Вы регистрируете приложение с помощью удостоверения Майкрософт, которое является поставщиком OAuth 2,0. Затем вы можете зарегистрировать приложение на основе OAuth 2,0 с любым другим поставщиком удостоверений, с которым вы хотите интегрировать. 

Сведения о регистрации приложения и реализации этих протоколов для единого входа в веб-приложения см. в разделе [авторизация доступа к веб-приложениям с помощью OpenID Connect Connect и Azure Active Directory](../develop/sample-v2-code.md).  Сведения о том, как реализовать эти протоколы для единого входа в мобильных приложениях, см. в следующих статьях: 

* [Android](../develop/quickstart-v2-android.md)

* [iOS](../develop/quickstart-v2-ios.md)

* [Универсальная платформа Windows](../develop/quickstart-v2-uwp.md)

## <a name="saml-20-and-wsfed"></a>SAML 2,0 и WSFed

Язык разметки зявлений системы безопасности (SAML) (SAML) обычно используется для веб-приложений. Общие сведения см. [в статье Использование протокола SAML в Azure](../develop/active-directory-saml-protocol-reference.md) . 

Web Services Federation (WSFed) — это [промышленный стандарт](https://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html) , который обычно используется для веб-приложений, разработанных с помощью платформы .NET.

### <a name="benefits"></a>Преимущества

SAML 2,0 — это зрелый стандарт и большинство технологических платформ поддерживают библиотеки с открытым исходным кодом для SAML 2,0. Вы можете предоставить своим клиентам интерфейс администрирования для настройки единого входа SAML. Они могут настроить единый вход SAML для Microsoft Azure AD и любого другого поставщика удостоверений, поддерживающего SAML 2.

### <a name="trade-offs"></a>Компромиссы

При использовании протоколов SAML 2,0 или WSFed для мобильных приложений некоторые политики условного доступа, включая многофакторную проверку подлинности (MFA), будут работать с пониженной производительностью. Кроме того, если требуется получить доступ к Microsoft Graph, необходимо реализовать авторизацию с помощью OAuth 2,0 для создания необходимых токенов. 

### <a name="implementation"></a>Реализация

Корпорация Майкрософт не предоставляет библиотеки для реализации SAML или рекомендации по конкретным библиотекам. Доступно множество библиотек с открытым исходным кодом.

## <a name="sso-and-using-microsoft-graph-rest-api"></a>Единый вход и использование Microsoft Graph API 

Microsoft Graph — это структура данных для всех Microsoft 365, включая Office 365, Windows 10 и Корпоративная мобильность и безопасность, а также дополнительные продукты, такие как Dynamics 365. Сюда входят основные схемы сущностей, таких как пользователи, группы, календарь, почта, файлы и многое другое, что повышает производительность пользователей. Microsoft Graph предлагает три интерфейса для разработчиков: API на основе RESTFUL, Microsoft Graph подключения к данным и соединители, позволяющие разработчикам добавлять собственные данные в Microsoft Graph.  

Использование любого из приведенных выше протоколов для единого входа позволяет приложению получить доступ к расширенным данным через REST API Microsoft Graph. Это позволяет клиентам получать больше ценности из инвестиций в Microsoft 365. Например, приложение может вызвать API Microsoft Graph, чтобы интегрироваться с экземпляром Office 365 и пользователями Surface Microsoft Office и элементами SharePoint в приложении. 

Если для проверки подлинности используется открытая ИДЕНТИФИКАЦИя Connect, то процесс разработки будет непростым, так как вы будете использовать OAuth2, фундамент открытого идентификатора Connect, чтобы получить маркеры, которые можно использовать для вызова Microsoft Graph API. Если приложение использует SAML или WSFed, необходимо добавить дополнительный код в приложение, чтобы получить эти OAuth2 для получения маркеров, необходимых для вызова Microsoft Graph API. 

## <a name="next-steps"></a>Дальнейшие действия

[Включение единого входа для мультитенантного приложения](isv-sso-content.md)

[Создание документации для приложения с несколькими клиентами](isv-create-sso-documentation.md)
