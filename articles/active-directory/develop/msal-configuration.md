---
title: Файл конфигурации MSAL для Android | Службы
titleSuffix: Microsoft identity platform
description: Обзор файла конфигурации библиотеки аутентификации Microsoft для Android (MSAL), который представляет конфигурацию приложения в Azure Active Directory.
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/12/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman
ms.openlocfilehash: 9e35ba5a3f3705a52e80262da9bbfbfda489bf83
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80050378"
---
# <a name="android-microsoft-authentication-library-configuration-file"></a>Файл конфигурации библиотеки проверки подлинности Майкрософт для Android

Библиотека аутентификации Microsoft для Android (MSAL) поставляется с [ФАЙЛОМ JSON конфигурации по умолчанию](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json) , который можно настроить для определения поведения общедоступного клиентского приложения, например центра по умолчанию, используемых центров и т. д.

Эта статья поможет вам понять различные параметры в файле конфигурации и указать файл конфигурации для использования в приложении на основе MSAL.

## <a name="configuration-settings"></a>Параметры конфигурации

### <a name="general-settings"></a>Общие параметры

| Свойство | Тип данных | Обязательный | Примечания |
|-----------|------------|-------------|-------|
| `client_id` | Строка | Да | Идентификатор клиента приложения на [странице регистрации приложения](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) |
| `redirect_uri`   | Строка | Да | URI перенаправления приложения со [страницы регистрации приложения](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) |
| `authorities` | >\<центра списка | Нет | Список органов, необходимых для приложения |
| `authorization_user_agent` | Аусоризатионажент (enum) | Нет | Возможные значения: `DEFAULT`, `BROWSER`,`WEBVIEW` |
| `http` | HttpConfiguration | Нет | Настройка `HttpUrlConnection` `connect_timeout` и`read_timeout` |
| `logging` | логгингконфигуратион | Нет | Задает уровень детализации журнала. К дополнительным конфигурациям относятся `pii_enabled`:, принимающее логическое значение `log_level`, и, `ERROR`принимающее, `WARNING` `INFO`, `VERBOSE`или. |

### <a name="client_id"></a>client_id

Идентификатор клиента или приложения, которые были созданы при регистрации приложения.

### <a name="redirect_uri"></a>redirect_uri

URI перенаправления, зарегистрированный при регистрации приложения. Если URI перенаправления относится к приложению брокера, обратитесь к [URI перенаправления для общедоступных клиентских приложений](msal-client-application-configuration.md#redirect-uri-for-public-client-apps) , чтобы убедиться, что вы используете правильный формат URI перенаправления для приложения брокера.

### <a name="authorities"></a>органы

Список известных и надежных центров. Помимо перечисленных здесь центров, MSAL также запрашивает у корпорации Майкрософт список облаков и органов, известных корпорации Майкрософт. В этом списке центров укажите тип центра и любые дополнительные параметры, такие как `"audience"`, которые должны быть согласованы с аудиторией приложения на основе регистрации приложения. Ниже приведен пример списка центров.

```javascript
// Example AzureAD and Personal Microsoft Account
{
    "type": "AAD",
    "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
    },
    "default": true // Indicates that this is the default to use if not provided as part of the acquireToken or acquireTokenSilent call
},
// Example AzureAD My Organization
{
    "type": "AAD",
    "audience": {
        "type": "AzureADMyOrg",
        "tenantId": "contoso.com" // Provide your specific tenant ID here
    }
},
// Example AzureAD Multiple Organizations
{
    "type": "AAD",
    "audience": {
        "type": "AzureADMultipleOrgs"
    }
},
//Example PersonalMicrosoftAccount
{
    "type": "AAD",
    "audience": {
        "type": "PersonalMicrosoftAccount"
    }
}
```

#### <a name="map-aad-authority--audience-to-microsoft-identity-platform-endpoints"></a>Сопоставление центра AAD & аудитории с конечными точками платформы удостоверений Майкрософт

| Тип | Аудитория | Tenant ID | Authority_Url | Результирующая конечная точка | Примечания |
|------|------------|------------|----------------|----------------------|---------|
| AAD | азуреадандперсоналмикрософтаккаунт | | | `https://login.microsoftonline.com/common` | `common`псевдоним клиента для учетной записи. Например, определенного клиента Azure Active Directory или учетная запись Майкрософт системы. |
| AAD | азуреадмйорг | contoso.com | | `https://login.microsoftonline.com/contoso.com` | Только учетные записи, имеющиеся в contoso.com, могут получить маркер. В качестве идентификатора клиента может использоваться любой проверенный домен или идентификатор GUID клиента. |
| AAD | азуреадмултиплеоргс | | | `https://login.microsoftonline.com/organizations` | С этой конечной точкой можно использовать только учетные записи Azure Active Directory. Учетные записи Майкрософт могут быть членами организаций. Чтобы получить маркер с помощью учетная запись Майкрософт для ресурса в Организации, укажите клиент Организации, из которого требуется получить маркер. |
| AAD | персоналмикрософтаккаунт | | | `https://login.microsoftonline.com/consumers` | Только учетные записи Майкрософт могут использовать эту конечную точку. |
| B2C | | | Просмотреть результирующую конечную точку | `https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/` | Только учетные записи, имеющиеся в клиенте contoso.onmicrosoft.com, могут получить маркер. В этом примере политика B2C является частью пути URL-адреса центра. |

> [!NOTE]
> Проверка центра не может быть включена и отключена в MSAL.
> Центры сертификации должны быть известны как разработчик, как указано в конфигурации, так и корпорацией Майкрософт через метаданные.
> Если MSAL получает запрос маркера для неизвестного центра сертификации, `MsalClientException` тип `UnknownAuthority` результатов.

#### <a name="authority-properties"></a>Свойства центра

| Свойство | Тип данных  | Обязательный | Примечания |
|-----------|-------------|-----------|--------|
| `type` | Строка | Да | Отражает аудиторию или тип учетной записи, для которой предназначено приложение. Возможные значения: `AAD`,`B2C` |
| `audience` | Объект | Нет | Применяется только при типе =`AAD`. Указывает удостоверение, для которого предназначено ваше приложение. Использовать значение из регистрации приложения |
| `authority_url` | Строка | Да | Требуется только при типе =`B2C`. Указывает URL-адрес центра или политику, которые должно использовать приложение  |
| `default` | Логическое | Да | Если указан `"default":true` один или несколько центров сертификации, необходимо указать один из них. |

#### <a name="audience-properties"></a>Свойства аудитории

| Свойство | Тип данных  | Обязательный | Примечания |
|-----------|-------------|------------|-------|
| `type` | Строка | Да | Указывает аудиторию, для которой нужно назначить приложение. Возможные значения: `AzureADandPersonalMicrosoftAccount`, `PersonalMicrosoftAccount`, `AzureADMultipleOrgs`,`AzureADMyOrg` |
| `tenant_id` | Строка | Да | Требуется, только `"type":"AzureADMyOrg"`если. Необязательно для `type` других значений. Это может быть домен клиента `contoso.com`, например, или идентификатор клиента, например.) `72f988bf-86f1-41af-91ab-2d7cd011db46` |

### <a name="authorization_user_agent"></a>authorization_user_agent

Указывает, следует ли использовать встроенный WebView или браузер по умолчанию на устройстве при входе в учетную запись или авторизацию доступа к ресурсу.

Возможные значения:
- `DEFAULT`: Предпочитает системный браузер. Использует внедренное веб-представление, если браузер недоступен на устройстве.
- `WEBVIEW`: Используйте встроенное веб-представление.
- `BROWSER`: Использует браузер по умолчанию на устройстве.

### <a name="multiple_clouds_supported"></a>multiple_clouds_supported

Для клиентов, поддерживающих несколько национальных облаков `true`, укажите. После этого платформа Microsoft Identity автоматически перенаправит в правильное Национальный облако во время авторизации и активации маркеров. Вы можете определить национальные облака учетной записи, в которой выполнен вход, изучив центр, связанный с `AuthenticationResult`. Обратите внимание `AuthenticationResult` , что не предоставляет национальный облачный адрес конечной точки ресурса, для которого запрашивается маркер.

### <a name="broker_redirect_uri_registered"></a>broker_redirect_uri_registered

Логическое значение, указывающее, используется ли универсальный код ресурса (URI) перенаправления в брокере Microsoft Identity. Задайте значение `false` , если вы не хотите использовать брокер в приложении.

Если вы используете центр AAD с аудиторией со значением `"MicrosoftPersonalAccount"`, брокер будет использоваться не будет.

### <a name="http"></a>http

Настройте глобальные параметры для времени ожидания HTTP, например:

| Свойство | Тип данных | Обязательный | Примечания |
| ---------|-----------|------------|--------|
| `connect_timeout` | INT | Нет | Время в миллисекундах |
| `read_timeout` | INT | Нет | Время в миллисекундах |

### <a name="logging"></a>Ведение журналов

Для ведения журнала используются следующие глобальные параметры.

| Свойство | Тип данных  | Обязательный | Примечания |
| ----------|-------------|-----------|---------|
| `pii_enabled`  | Логическое | Нет | Следует ли выдавать персональные данные |
| `log_level`   | Логическое | Нет | Какие сообщения журнала должны выводиться |
| `logcat_enabled` | Логическое | Нет | Следует ли выводить журнал Cat в дополнение к интерфейсу ведения журнала |

### <a name="account_mode"></a>account_mode

Указывает, сколько учетных записей может использоваться в приложении за раз. Допустимые значения:

- `MULTIPLE` (по умолчанию)
- `SINGLE`

Создание экземпляра `PublicClientApplication` с использованием режима учетной записи, который не соответствует этому параметру, приведет к исключению.

Дополнительные сведения о различиях между одной и несколькими учетными записями см. в разделе [приложения одной и нескольких учетных записей](single-multi-account.md).

### <a name="browser_safelist"></a>browser_safelist

Список разрешенных браузеров, совместимых с MSAL. Эти браузеры правильно обрабатывали перенаправления на пользовательские намерения. В этот список можно добавить. Значение по умолчанию указывается в конфигурации по умолчанию, показанной ниже.
``
## <a name="the-default-msal-configuration-file"></a>Файл конфигурации MSAL по умолчанию

Ниже приведена конфигурация MSAL по умолчанию, входящая в состав MSAL. Вы можете увидеть последнюю версию на сайте [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json).

Эта конфигурация дополняется предоставленными значениями. Предоставленные значения переопределяют параметры по умолчанию.

```javascript
{
  "authorities": [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
      },
      "default": true
    }
  ],
  "authorization_user_agent": "DEFAULT",
  "multiple_clouds_supported": false,
  "broker_redirect_uri_registered": false,
  "http": {
    "connect_timeout": 10000,
    "read_timeout": 30000
  },
  "logging": {
    "pii_enabled": false,
    "log_level": "WARNING",
    "logcat_enabled": false
  },
  "shared_device_mode_supported": false,
  "account_mode": "MULTIPLE",
  "browser_safelist": [
    {
      "browser_package_name": "com.android.chrome",
      "browser_signature_hashes": [
        "7fmduHKTdHHrlMvldlEqAIlSfii1tl35bxj1OXN5Ve8c4lU6URVu4xtSHc3BVZxS6WWJnxMDhIfQN0N0K2NDJg=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "45"
    },
    {
      "browser_package_name": "com.android.chrome",
      "browser_signature_hashes": [
        "7fmduHKTdHHrlMvldlEqAIlSfii1tl35bxj1OXN5Ve8c4lU6URVu4xtSHc3BVZxS6WWJnxMDhIfQN0N0K2NDJg=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "org.mozilla.firefox",
      "browser_signature_hashes": [
        "2gCe6pR_AO_Q2Vu8Iep-4AsiKNnUHQxu0FaDHO_qa178GByKybdT_BuE8_dYk99G5Uvx_gdONXAOO2EaXidpVQ=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "org.mozilla.firefox",
      "browser_signature_hashes": [
        "2gCe6pR_AO_Q2Vu8Iep-4AsiKNnUHQxu0FaDHO_qa178GByKybdT_BuE8_dYk99G5Uvx_gdONXAOO2EaXidpVQ=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "57"
    },
    {
      "browser_package_name": "com.sec.android.app.sbrowser",
      "browser_signature_hashes": [
        "ABi2fbt8vkzj7SJ8aD5jc4xJFTDFntdkMrYXL3itsvqY1QIw-dZozdop5rgKNxjbrQAd5nntAGpgh9w84O1Xgg=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "4.0"
    },
    {
      "browser_package_name": "com.sec.android.app.sbrowser",
      "browser_signature_hashes": [
        "ABi2fbt8vkzj7SJ8aD5jc4xJFTDFntdkMrYXL3itsvqY1QIw-dZozdop5rgKNxjbrQAd5nntAGpgh9w84O1Xgg=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.cloudmosa.puffinFree",
      "browser_signature_hashes": [
        "1WqG8SoK2WvE4NTYgr2550TRhjhxT-7DWxu6C_o6GrOLK6xzG67Hq7GCGDjkAFRCOChlo2XUUglLRAYu3Mn8Ag=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.duckduckgo.mobile.android",
      "browser_signature_hashes": [
        "S5Av4cfEycCvIvKPpKGjyCuAE5gZ8y60-knFfGkAEIZWPr9lU5kA7iOAlSZxaJei08s0ruDvuEzFYlmH-jAi4Q=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.explore.web.browser",
      "browser_signature_hashes": [
        "BzDzBVSAwah8f_A0MYJCPOkt0eb7WcIEw6Udn7VLcizjoU3wxAzVisCm6bW7uTs4WpMfBEJYf0nDgzTYvYHCag=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.ksmobile.cb",
      "browser_signature_hashes": [
        "lFDYx1Rwc7_XUn4KlfQk2klXLufRyuGHLa3a7rNjqQMkMaxZueQfxukVTvA7yKKp3Md3XUeeDSWGIZcRy7nouw=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.microsoft.emmx",
      "browser_signature_hashes": [
        "Ivy-Rk6ztai_IudfbyUrSHugzRqAtHWslFvHT0PTvLMsEKLUIgv7ZZbVxygWy_M5mOPpfjZrd3vOx3t-cA6fVQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.opera.browser",
      "browser_signature_hashes": [
        "FIJ3IIeqB7V0qHpRNEpYNkhEGA_eJaf7ntca-Oa_6Feev3UkgnpguTNV31JdAmpEFPGNPo0RHqdlU0k-3jWJWw=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.opera.mini.native",
      "browser_signature_hashes": [
        "TOTyHs086iGIEdxrX_24aAewTZxV7Wbi6niS2ZrpPhLkjuZPAh1c3NQ_U4Lx1KdgyhQE4BiS36MIfP6LbmmUYQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "mobi.mgeek.TunnyBrowser",
      "browser_signature_hashes": [
        "RMVoXuK1sfJZuGZ8onG1yhMc-sKiAV2NiB_GZfdNlN8XJ78XEE2wPM6LnQiyltF25GkHiPN2iKQiGwaO2bkyyQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "org.mozilla.focus",
      "browser_signature_hashes": [
        "L72dT-stFqomSY7sYySrgBJ3VYKbipMZapmUXfTZNqOzN_dekT5wdBACJkpz0C6P0yx5EmZ5IciI93Q0hq0oYA=="
      ],
      "browser_use_customTab" : false
    }
  ]
}
```
## <a name="example-basic-configuration"></a>Пример базовой конфигурации

В следующем примере показана базовая конфигурация, в которой указывается идентификатор клиента, URI перенаправления, зарегистрировано ли перенаправление брокера, а также список центров.

```javascript
{
  "client_id" : "4b0db8c2-9f26-4417-8bde-3f0e3656f8e0",
  "redirect_uri" : "msauth://com.microsoft.identity.client.sample.local/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
      }
      "default": true
    }
  ]
}
```

## <a name="how-to-use-a-configuration-file"></a>Использование файла конфигурации

1. Создайте файл конфигурации. Рекомендуется создать пользовательский файл конфигурации в `res/raw/auth_config.json`. Но вы можете разместить его в любом месте.
2. Укажите MSAL, `PublicClientApplication`где следует искать конфигурацию при создании. Пример:

   ```java
   //On Worker Thread
   IMultipleAccountPublicClientApplication sampleApp = null; 
   sampleApp = new PublicClientApplication.createMultipleAccountPublicClientApplication(getApplicationContext(), R.raw.auth_config);
   ```
