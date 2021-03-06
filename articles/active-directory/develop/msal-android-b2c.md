---
title: Azure AD B2C (MSAL Android) | Службы
titleSuffix: Microsoft identity platform
description: Ознакомьтесь с конкретными соображениями при использовании Azure AD B2C с библиотекой проверки подлинности Майкрософт для Android (MSAL. Android
services: active-directory
author: brianmel
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 9/18/2019
ms.author: brianmel
ms.reviewer: rapong
ms.custom: aaddev
ms.openlocfilehash: 0998bb04b0dfc69db4696f2e390cfe259eba6718
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76696527"
---
# <a name="use-msal-for-android-with-b2c"></a>Использование MSAL для Android с B2C

Библиотека аутентификации Майкрософт (MSAL) позволяет разработчикам приложений аутентифицировать пользователей с помощью удостоверений социальных сетей и локальных удостоверений, используя [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/). Azure AD B2C — это служба управления удостоверениями. Используйте его для настройки и управления регистрацией клиентов, входом в систему и управлением их профилями при использовании ваших приложений.

## <a name="configure-known-authorities-and-redirect-uri"></a>Настройка известных центров и URI перенаправления

В MSAL для Android политики B2C (пути взаимодействия пользователя) настраиваются как отдельные центры.

При наличии B2C приложения с двумя политиками:
- Регистрация и вход в систему
    * Имя`B2C_1_SISOPolicy`
- Изменить профиль
    * Имя`B2C_1_EditProfile`

Файл конфигурации для приложения будет объявлять два `authorities`. По одному для каждой политики. `type` Свойство каждого из полномочий имеет `B2C`значение.

### `app/src/main/res/raw/msal_config.json`
```json
{
    "client_id": "<your_client_id_here>",
    "redirect_uri": "<your_redirect_uri_here>",
    "authorities": [{
            "type": "B2C",
            "authority_url": "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/",
            "default": true
        },
        {
            "type": "B2C",
            "authority_url": "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_EditProfile/"
        }
    ]
}
```

`redirect_uri` Должен быть зарегистрирован в конфигурации приложения, а также в `AndroidManifest.xml` для поддержки перенаправления во время [потока предоставления кода авторизации](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-oauth-code).

## <a name="initialize-ipublicclientapplication"></a>Инициализация Ипубликклиентаппликатион

`IPublicClientApplication`создается с помощью фабричного метода, позволяющего выполнять синтаксический анализ конфигурации приложения в асинхронном режиме.

```java
PublicClientApplication.createMultipleAccountPublicClientApplication(
    context, // Your application Context
    R.raw.msal_config, // Id of app JSON config
    new IPublicClientApplication.ApplicationCreatedListener() {
        @Override
        public void onCreated(IMultipleAccountPublicClientApplication pca) {
            // Application has been initialized.
        }

        @Override
        public void onError(MsalException exception) {
            // Application could not be created.
            // Check Exception message for details.
        }
    }
);
```

## <a name="interactively-acquire-a-token"></a>Интерактивное получение маркера

Чтобы получить маркер в интерактивном режиме с помощью MSAL, создайте `AcquireTokenParameters` экземпляр и предоставьте его `acquireToken` методу. В запросе токена ниже используется `default` центр.

```java
IMultipleAccountPublicClientApplication pca = ...; // Initialization not shown

AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withScopes(Arrays.asList("https://contoso.onmicrosoft.com/contosob2c/read")) // Provide your registered scope here
    .withPrompt(Prompt.LOGIN)
    .callback(new AuthenticationCallback() {
        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            // Token request was successful, inspect the result
        }

        @Override
        public void onError(MsalException exception) {
            // Token request was unsuccessful, inspect the exception
        }

        @Override
        public void onCancel() {
            // The user cancelled the flow
        }
    }).build();

pca.acquireToken(parameters);
```

## <a name="silently-renew-a-token"></a>Автоматическое продление срока действия токена

Чтобы получить маркер в автоматическом режиме с помощью MSAL, `AcquireTokenSilentParameters` создайте экземпляр и предоставьте его `acquireTokenSilentAsync` методу. В `authority` отличие от `acquireToken` метода, необходимо указать для получения маркера в автоматическом режиме.

```java
IMultilpeAccountPublicClientApplication pca = ...; // Initialization not shown
AcquireTokenSilentParameters parameters = new AcquireTokenSilentParameters.Builder()
    .withScopes(Arrays.asList("https://contoso.onmicrosoft.com/contosob2c/read")) // Provide your registered scope here
    .forAccount(account)
    // Select a configured authority (policy), mandatory for silent auth requests
    .fromAuthority("https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/")
    .callback(new SilentAuthenticationCallback() {
        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            // Token request was successful, inspect the result
        }

        @Override
        public void onError(MsalException exception) {
            // Token request was unsuccesful, inspect the exception
        }
    })
    .build();

pca.acquireTokenSilentAsync(parameters);
```

## <a name="specify-a-policy"></a>Укажите политику

Так как политики в B2C представляются в виде отдельных центров, вызов политики, отличной от значения по умолчанию, достигается путем указания `fromAuthority` предложения `acquireToken` при `acquireTokenSilent` создании или настройке параметров.  Пример:

```java
AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withScopes(Arrays.asList("https://contoso.onmicrosoft.com/contosob2c/read")) // Provide your registered scope here
    .withPrompt(Prompt.LOGIN)
    .callback(...) // provide callback here
    .fromAuthority("<url_of_policy_defined_in_configuration_json>")
    .build();
```

## <a name="handle-password-change-policies"></a>Работа с политиками изменения паролей

Пользовательский поток регистрации или входа в локальную учетную запись отображает "**забыли пароль?**" . При переходе по этой ссылке поток пользователя сброса паролей не активируется автоматически.

Вместо этого в приложение возвращается код ошибки `AADB2C90118`. Приложение должно справиться с этим кодом ошибки, запустив конкретный поток пользователя, который сбрасывает пароль.

Чтобы перехватить код ошибки сброса пароля, можно использовать следующую реализацию в `AuthenticationCallback`:

```java
new AuthenticationCallback() {

    @Override
    public void onSuccess(IAuthenticationResult authenticationResult) {
        // ..
    }

    @Override
    public void onError(MsalException exception) {
        final String B2C_PASSWORD_CHANGE = "AADB2C90118";

        if (exception.getMessage().contains(B2C_PASSWORD_CHANGE)) {
            // invoke password reset flow
        }
    }

    @Override
    public void onCancel() {
        // ..
    }
}
```

## <a name="use-iauthenticationresult"></a>Использование Иаусентикатионресулт

Успешное получение маркера приводит к `IAuthenticationResult` получению объекта. Он содержит маркер доступа, пользовательские утверждения и метаданные.

### <a name="get-the-access-token-and-related-properties"></a>Получение маркера доступа и связанных свойств

```java
// Get the raw bearer token
String accessToken = authenticationResult.getAccessToken();

// Get the scopes included in the access token
String[] accessTokenScopes = authenticationResult.getScope();

// Gets the access token's expiry
Date expiry = authenticationResult.getExpiresOn();

// Get the tenant for which this access token was issued
String tenantId = authenticationResult.getTenantId();
```

### <a name="get-the-authorized-account"></a>Получение полномочной учетной записи

```java
// Get the account from the result
IAccount account = authenticationResult.getAccount();

// Get the id of this account - note for B2C, the policy name is a part of the id
String id = account.getId();

// Get the IdToken Claims
//
// For more information about B2C token claims, see reference documentation
// https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens
Map<String, ?> claims = account.getClaims();

// Get the 'preferred_username' claim through a convenience function
String username = account.getUsername();

// Get the tenant id (tid) claim through a convenience function
String tenantId = account.getTenantId();
```

### <a name="idtoken-claims"></a>Утверждения IdToken

Утверждения, возвращенные в IdToken, заполняются службой маркеров безопасности (STS), а не MSAL. В зависимости от используемого поставщика удостоверений (IdP) некоторые утверждения могут отсутствовать. Некоторые поставщиков удостоверений в настоящее время не `preferred_username` предоставляют заявку. Так как это утверждение используется MSAL для кэширования, вместо него используется значение `MISSING FROM THE TOKEN RESPONSE`заполнителя. Дополнительные сведения об утверждениях B2C IdToken см. [в разделе Обзор маркеров в Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#claims).

## <a name="managing-accounts-and-policies"></a>Управление учетными записями и политиками

B2C рассматривает каждую политику как отдельный центр. Поэтому маркеры доступа, маркеры обновления и маркеры идентификации, возвращаемые из каждой политики, не взаимозаменяемы. Это означает, что каждая политика возвращает `IAccount` отдельный объект, маркеры которого нельзя использовать для вызова других политик.

Каждая политика добавляет `IAccount` в кэш для каждого пользователя. Если пользователь входит в приложение и вызывает две политики, у них будет два `IAccount`. Чтобы удалить этого пользователя из кэша, необходимо вызвать `removeAccount()` для каждой политики.

При обновлении маркеров для политики с помощью `acquireTokenSilent`укажите то же `IAccount` значение, которое было возвращено предыдущими вызовами политики в. `AcquireTokenSilentParameters` Предоставление учетной записи, возвращаемой другой политикой, приведет к ошибке.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Azure Active Directory B2C (Azure AD B2C) о том [, что Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
