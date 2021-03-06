---
title: Определение технического профиля OAuth2 в пользовательской политике
titleSuffix: Azure AD B2C
description: Определите технический профиль OAuth2 в пользовательской политике в Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 86ec7a5745a58546faf6f0ff15d6dc5f452baa88
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78184049"
---
# <a name="define-an-oauth2-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Определение технического профиля OAuth2 в настраиваемой политике Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) обеспечивает поддержку поставщика удостоверений протокола OAuth2. OAuth2 — это основной протокол авторизации и делегированной проверки подлинности. Дополнительные сведения см. в документе [RFC 6749 о системе авторизации OAuth 2.0](https://tools.ietf.org/html/rfc6749). С помощью технического профиля OAuth2 можно создать федерацию с помощью поставщика удостоверений на основе OAuth2, например Facebook. Федерацию с поставщиком удостоверений позволяет пользователям входить в систему, используя существующие социальные или корпоративные удостоверения.

## <a name="protocol"></a>Протокол

Атрибуту **Name** элемента **Protocol** необходимо присвоить значение `OAuth2`. Например, `OAuth2` — это протокол для технического профиля **Facebook-OAUTH**.

```XML
<TechnicalProfile Id="Facebook-OAUTH">
  <DisplayName>Facebook</DisplayName>
  <Protocol Name="OAuth2" />
  ...
```

## <a name="input-claims"></a>Входящие утверждения

Элементы **InputClaims** и **InputClaimsTransformations** не являются обязательными. Но при необходимости можно отправлять дополнительные параметры поставщику удостоверений. В следующем примере к запросу авторизации добавляется параметр строки запроса **domain_hint** со значением `contoso.com`.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Исходящие утверждения

Элемент **OutputClaims** содержит список утверждений, возвращаемых поставщиком удостоверений OAuth2. Возможно, потребуется сопоставить имя утверждения, определенное в вашей политике, с именем, определенным у поставщика удостоверений. Вы также можете добавить утверждения, которые не возвращаются поставщиком удостоверений, установив атрибут `DefaultValue`.

Элемент **OutputClaimsTransformations** может содержать коллекцию элементов **OutputClaimsTransformation**, которые используются для изменения исходящих утверждений или создания новых.

В этом примере показаны утверждения, возвращаемые поставщиком удостоверений Facebook:

- Утверждение **first_name** сопоставляется с утверждением **givenName**.
- Утверждение **last_name** сопоставляется с утверждением **surname**.
- Утверждение **DisplayName** без сопоставления имен.
- Утверждение **email** не сопоставляется с именем.

Технический профиль также возвращает утверждения, которые не возвращаются поставщиком удостоверений:

- Утверждение **IdentityProvider**, содержащее имя поставщика удостоверений.
- утверждение **AuthenticationSource** со значением по умолчанию **socialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="facebook.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Метаданные

| Атрибут | Обязательный | Описание |
| --------- | -------- | ----------- |
| client_id | Да | Идентификатор приложения поставщика удостоверений. |
| IdTokenAudience | Нет | Аудитория id_token. Если этот параметр задан, Azure AD B2C проверяет, содержится ли маркер в утверждении, которое возвращает поставщик удостоверений, и соответствует ли маркер заданному параметру. |
| authorization_endpoint | Да | URL-адрес конечной точки авторизации, согласно RFC 6749. |
| AccessTokenEndpoint | Да | URL-адрес конечной точки маркера, согласно RFC 6749. |
| ClaimsEndpoint | Да | URL-адрес конечной точки информации о пользователе, согласно RFC 6749. |
| AccessTokenResponseFormat | Нет | Формат вызова конечной точки маркера доступа. Например, для Facebook требуется метод HTTP GET, но маркер доступа возвращается в формате JSON. |
| AdditionalRequestQueryParameters | Нет | Дополнительные параметры запроса. При необходимости можно отправлять дополнительные параметры поставщику удостоверений. Можно добавлять несколько параметров, используя запятую в качестве разделителя. |
| ClaimsEndpointAccessTokenName | Нет | Имя, присваиваемое параметру строки запроса на маркера доступа. У некоторых поставщиков удостоверений конечные точки утверждений поддерживают запрос HTTP GET. В этом случае маркер носителя отправляется с помощью параметра строки запроса, а не заголовка авторизации. |
| ClaimsEndpointFormatName | Нет | Имя, присваиваемое параметру строки запроса на формат. Например, можно задать имя `format` для этой конечной точки утверждений LinkedIn: `https://api.linkedin.com/v1/people/~?format=json`. |
| ClaimsEndpointFormat | Нет | Значение для параметра строки запроса на формат. Например, можно задать значение `json` для этой конечной точки утверждений LinkedIn: `https://api.linkedin.com/v1/people/~?format=json`. |
| ProviderName | Нет | Имя поставщика удостоверений. |
| response_mode | Нет | Метод, который использует поставщик удостоверений, чтобы отправить результат обратно в Azure AD B2C. Возможные значения: `query`, `form_post` (по умолчанию) или `fragment`. |
| scope | Нет | Область запроса, определяемая в соответствии со спецификацией поставщика удостоверений OAuth2. Возможные значения: `openid`, `profile` и `email`. |
| HttpBinding | Нет | Ожидаемая привязка HTTP для маркера доступа и конечных точек маркера утверждений. Возможные значения: `GET` или `POST`.  |
| ResponseErrorCodeParamName | Нет | Имя параметра, который содержит сообщение об ошибке, возвращаемое по HTTP 200 (ОК). |
| ExtraParamsInAccessTokenEndpointResponse | Нет | Содержит дополнительные параметры, которые могут возвращать в ответе от **AccessTokenEndpoint** некоторые поставщики удостоверений. Например, ответ от **AccessTokenEndpoint** содержит дополнительный параметр, такой как `openid`, который является обязательным параметром, помимо access_token, в строке запроса **ClaimsEndpoint**. При указании нескольких имен параметров их следует экранировать, разделяя запятой (,). |
| ExtraParamsInClaimsEndpointRequest | Нет | Содержит дополнительные параметры, которые могут возвращать в запросе **ClaimsEndpoint** некоторые поставщики удостоверений. При указании нескольких имен параметров их следует экранировать, разделяя запятой (,). |
| инклудеклаимресолвингинклаимшандлинг  | Нет | Для входных и выходных утверждений указывает, включено ли [разрешение утверждений](claim-resolver-overview.md) в технический профиль. Возможные значения: `true`или `false`  (по умолчанию). Если вы хотите использовать сопоставитель утверждений в техническом профиле, задайте для `true`этого параметра значение. |
| ресолвежсонпассинжсонтокенс  | Нет | Указывает, разрешает ли технический профиль пути JSON. Возможные значения: `true`или `false` (по умолчанию). Используйте эти метаданные для чтения данных из вложенного элемента JSON. В [OutputClaim](technicalprofiles.md#outputclaims)задайте в качестве значения `PartnerClaimType` элемент пути JSON, который требуется вывести. Например: `firstName.localized`или `data.0.to.0.email`.|

## <a name="cryptographic-keys"></a>Криптографические ключи

Элемент **CryptographicKeys** содержит следующий атрибут:

| Атрибут | Обязательный | Описание |
| --------- | -------- | ----------- |
| client_secret | Да | Секрет клиента приложения поставщика удостоверений. Ключ шифрования является обязательным, только если для метаданных **response_types** задано значение `code`. В этом случае Azure AD B2C выполняет другой вызов для обмена кода авторизации на маркер доступа. Если для `id_token`метаданных задано значение, криптографический ключ можно опустить. |

## <a name="redirect-uri"></a>URI перенаправления

При настройке URL-адреса перенаправления для поставщика удостоверений введите `https://login.microsoftonline.com/te/tenant/policyId/oauth2/authresp`. Замените **tenant** именем своего клиента (например, contosob2c.onmicrosoft.com), а **policyId** — идентификатором политики (например, b2c_1a_policy). URI перенаправления должен содержать только строчные символы.

Если вы используете домен **b2clogin.com** вместо **login.microsoftonline.com**, удостоверьтесь, что єто действительно требуется.

Примеры

- [Добавление Google+ в качестве поставщика удостоверений OAuth2 с помощью пользовательских политик](identity-provider-google-custom.md)













