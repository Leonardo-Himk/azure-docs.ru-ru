---
title: 'Azure AD Connect: простой единый вход | Документы Майкрософт'
description: В этом разделе описывается простой единый вход Azure Active Directory (Azure AD) и то, как он обеспечивает действительно единый вход пользователей корпоративных компьютеров в корпоративной сети.
services: active-directory
keywords: что такое Azure AD Connect, установка Active Directory, необходимые компоненты для Azure AD, единый вход
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/13/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1b7e4716e731e6b73e3ac60b64baa71043906fc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77483760"
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Простой единый вход Azure Active Directory

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Что такое простой единый вход Azure Active Directory?

Простой единый вход Azure Active Directory автоматически обеспечивает пользователям вход в систему, когда они работают на корпоративных устройствах, подключенных к корпоративной сети. Если он включен, пользователям не нужно вводить пароль для входа в Azure AD, а в большинстве случаев — даже вводить имя пользователя. Эта функция предоставляет пользователям удобный доступ к облачным приложениям и не требует установки каких-либо дополнительных локальных компонентов.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Простой единый вход можно использовать вместе с методами [синхронизация хэша паролей](how-to-connect-password-hash-synchronization.md) или [сквозной проверки подлинности](how-to-connect-pta.md). Простой единый вход _не_ применяется к службам федерации Active Directory (AD FS).

![Простой единый вход](./media/how-to-connect-sso/sso1.png)

>[!IMPORTANT]
>Для простого единого входа необходимо, чтобы устройство пользователя было **присоединено только к домену** , но оно не используется в [присоединенных к Azure AD](../devices/concept-azure-ad-join.md) или [гибридных устройствах, присоединенных к Azure AD](../devices/concept-azure-ad-join-hybrid.md) . Единый вход для присоединения к Azure AD и гибридного присоединенного Azure AD работает на основе [основного маркера обновления](../devices/concept-primary-refresh-token.md).

## <a name="key-benefits"></a>Основные преимущества

- *Удобство работы для пользователей*
  - Пользователи автоматически входят как в локальные, так и в облачные приложения.
  - Пользователям не нужно вводить пароль несколько раз.
- *Простота развертывания и администрирования*
  - Дополнительные локальные компоненты не требуются.
  - Эта функция работает с любым методом аутентификации в облаке: [синхронизацией хэша паролей](how-to-connect-password-hash-synchronization.md) и [сквозной аутентификацией](how-to-connect-pta.md).
  - Ее можно развернуть для всех пользователей или их части с помощью групповой политики.
  - Регистрация устройств не под управлением Windows 10 в Azure AD без необходимости в какой-либо инфраструктуре AD FS. Для использования этой возможности требуется версия 2.1 или более поздняя версия [клиента подключения к рабочему месту](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Краткие сведения о возможностях

- Именем пользователя для входа может быть либо локальное имя пользователя по умолчанию (`userPrincipalName`), либо другой атрибут, настроенный в Azure AD Connect (`Alternate ID`). Оба варианта использования работают, так как служба простого единого входа использует утверждение `securityIdentifier` в билете Kerberos для поиска соответствующего объекта-пользователя в Azure AD.
- Простой единый вход — ситуативно-обусловленная функция. Если в ней происходит сбой, процедура входа выполняется стандартно, то есть пользователь, как и прежде, должен просто ввести пароль на странице входа.
- `https://myapps.microsoft.com/contoso.com`Если приложение (например,) перенаправляет параметр `domain_hint` (OpenID Connect Connect) или `whr` (SAML), определяющий клиента, или `login_hint` параметр, идентифицирующий пользователя, в запросе на вход в Azure AD пользователи автоматически входят в систему, не вводя имена пользователей или пароли.
- Пользователи также получают возможность автоматического входа, если `https://contoso.sharepoint.com`приложение (например) отправляет запросы на вход в конечные точки Azure AD, настроенные как клиенты, `https://login.microsoftonline.com/contoso.com/<..>` то есть или `https://login.microsoftonline.com/<tenant_ID>/<..>` -вместо общей конечной точки Azure AD, то есть. `https://login.microsoftonline.com/common/<...>`
- Поддерживается выход. Это позволяет пользователям выбрать другую учетную запись Azure AD для входа вместо того, чтобы автоматически входить с помощью простого единого входа.
- Для поддержки клиентов Office 365 для 32-разрядной версии Windows (Outlook, Word, Excel и другие) версии 16.0.8730.xxxx и выше используется неинтерактивная процедура. Для OneDrive потребуется активировать [функцию автоматической настройки OneDrive](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894), чтобы включить автоматический вход в систему.
- Функцию можно включить с помощью Azure AD Connect.
- Это бесплатная функция, и для ее использования не требуются платные выпуски Azure AD.
- Она доступна в клиентах на основе веб-браузера и клиентах Office, поддерживающих [современную проверку подлинности](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) на платформах и в браузерах с поддержкой проверки подлинности Kerberos.

| Операционная система и браузер |Internet Explorer|Microsoft Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|быть под управлением ОС Windows 10;|Да\*|Да|Да|Да\*\*\*|Недоступно
|Windows 8.1|Да\*|Недоступно|Да|Да\*\*\*|Недоступно
|Windows 8|Да\*|Недоступно|Да|Да\*\*\*|Недоступно
|Windows 7|Да\*|Недоступно|Да|Да\*\*\*|Недоступно
|Windows Server 2012 R2 или более поздней версии|Да\*\*|Недоступно|Да|Да\*\*\*|Недоступно
|Mac OS X|Недоступно|Недоступно|Да\*\*\*|Да\*\*\*|Да\*\*\*


\*Требуется Internet Explorer версии 10 или более поздней.

\*\*Требуется Internet Explorer версии 10 или более поздней. Отключение расширенного защищенного режима

\*\*\*Требуется [дополнительная настройка](how-to-connect-sso-quick-start.md#browser-considerations).

>[!NOTE]
>Чтобы обеспечить максимальное удобство единого входа в Azure AD, мы рекомендуем использовать в Windows 10 функцию [присоединения к Azure AD](../devices/concept-azure-ad-join.md).

## <a name="next-steps"></a>Дальнейшие шаги

- [**Краткое руководство**](how-to-connect-sso-quick-start.md). Настройка и подготовка к работе простого единого входа Azure AD.
- [**План развертывания**](https://aka.ms/deploymentplans/sso). Пошаговый план развертывания.
- [**Техническое руководство по сквозной проверке подлинности Azure Active Directory**](how-to-connect-sso-how-it-works.md). Сведения о том, как работает эта функция.
- [**Часто задаваемые вопросы**](how-to-connect-sso-faq.md) — ответы на часто задаваемые вопросы.
- [**Устранение неполадок**](tshoot-connect-sso.md) . Узнайте, как устранить распространенные проблемы с этой функцией.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect). Отправка запросов новых функций.

