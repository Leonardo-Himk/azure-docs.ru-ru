---
title: Настройка веб-приложения, которое входит в систему пользователей — платформа Microsoft Identity | Службы
description: Узнайте, как создать веб-приложение, которое входит в систему пользователей (конфигурация кода)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: b1eef510e6389b551e128877ffde723955a1084d
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2020
ms.locfileid: "82734643"
---
# <a name="web-app-that-signs-in-users-code-configuration"></a>Веб-приложение, которое входит в систему пользователей: конфигурация кода

Узнайте, как настроить код для веб-приложения, которое входит в систему пользователей.

## <a name="libraries-for-protecting-web-apps"></a>Библиотеки для защиты веб-приложений

<!-- This section can be in an include for web app and web APIs -->
Библиотеки, используемые для защиты веб-приложения (и веб-API):

| Платформа | Библиотека | Описание |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [Расширения модели удостоверений для .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | По ASP.NET и ASP.NET Core, расширения модели идентификации Майкрософт для .NET предлагают набор библиотек DLL, выполняющихся как в .NET Framework, так и в .NET Core. Из веб-приложения ASP.NET или ASP.NET Core можно управлять проверкой маркера с помощью класса **TokenValidationParameters** (в частности, в некоторых сценариях партнеров). |
| ![Java](media/sample-v2-code/small_logo_java.png) | [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | Поддержка веб-приложений Java |
| ![Python](media/sample-v2-code/small_logo_python.png) | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | Поддержка веб-приложений Python |

Выберите вкладку, соответствующую интересующей вас платформе:

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Фрагменты кода в этой статье и следующие извлекаются из [пошагового руководства по ASP.NET Core веб-приложению, главе 1](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg).

Для получения подробных сведений о реализации может потребоваться обратиться к этому учебнику.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Фрагменты кода в этой статье и следующие извлекаются из [примера веб-приложения ASP.NET](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect).

Для получения полной информации о реализации можно использовать этот пример.

# <a name="java"></a>[Java](#tab/java)

Фрагменты кода в этой статье и следующие извлекаются из примера [веб-приложения Java, вызывающего Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp) в MSAL Java.

Для получения полной информации о реализации можно использовать этот пример.

# <a name="python"></a>[Python](#tab/python)

Фрагменты кода в этой статье и следующие извлекаются из примера [веб-приложения Python, вызывающего Microsoft Graph](https://github.com/Azure-Samples/ms-identity-python-webapp) в MSAL Python.

Для получения полной информации о реализации можно использовать этот пример.

---

## <a name="configuration-files"></a>Файлы конфигурации.

Веб-приложения, которые входят в систему пользователей с помощью платформы Microsoft Identity, обычно настраиваются с помощью файлов конфигурации. Параметры, которые необходимо заполнить:

- Облачный экземпляр (`Instance`), если вы хотите, чтобы приложение выполнялось в национальных облаках, например
- Аудитория в ИДЕНТИФИКАТОРе клиента (`TenantId`)
- Идентификатор клиента (`ClientId`) для приложения, скопированный из портал Azure

В некоторых случаях приложения могут быть параметризованным `Authority`с помощью, что является объединением `Instance` и `TenantId`.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

В ASP.NET Core эти параметры находятся в файле [appSettings. JSON](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/appsettings.json#L2-L8) в разделе "AzureAd".

```Json
{
  "AzureAd": {
    // Azure cloud instance among:
    // - "https://login.microsoftonline.com/" for Azure public cloud
    // - "https://login.microsoftonline.us/" for Azure US government
    // - "https://login.microsoftonline.de/" for Azure AD Germany
    // - "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD audience among:
    // - "TenantId" as a GUID obtained from the Azure portal to sign in users in your organization
    // - "organizations" to sign in users in any work or school account
    // - "common" to sign in users with any work or school account or Microsoft personal account
    // - "consumers" to sign in users with a Microsoft personal account only
    "TenantId": "[Enter the tenantId here]",

    // Client ID (application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

В ASP.NET Core другой файл ([properties\launchSettings.JSON](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/Properties/launchSettings.json#L6-L7)) содержит URL-адрес (`applicationUrl`) и порт TLS/SSL (`sslPort`) для приложения и различных профилей.

```Json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

В портал Azure идентификаторы URI ответа, которые необходимо зарегистрировать на странице **проверки подлинности** для вашего приложения, должны соответствовать этим URL-адресам. Для двух предыдущих файлов конфигурации они бы были `https://localhost:44321/signin-oidc`. Причина в том, `applicationUrl` что `http://localhost:3110`это, `sslPort` но указано (44321). `CallbackPath`имеет `/signin-oidc`, как определено `appsettings.json`в.

Таким же образом URI выхода будет иметь значение `https://localhost:44321/signout-callback-oidc`.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

В ASP.NET приложение настраивается с помощью файла [Web. config](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Web.config#L12-L15) , строки 12 – 15.

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

В портал Azure идентификаторы URI ответа, которые необходимо зарегистрировать на странице **проверки подлинности** для вашего приложения, должны соответствовать этим URL-адресам. То есть они должны быть `https://localhost:44326/`.

# <a name="java"></a>[Java](#tab/java)

В Java конфигурация находится в файле [приложения. Properties](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/resources/application.properties) , расположенном в папке `src/main/resources`.

```Java
aad.clientId=Enter_the_Application_Id_here
aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
aad.secretKey=Enter_the_Client_Secret_Here
aad.redirectUriSignin=http://localhost:8080/msal4jsample/secure/aad
aad.redirectUriGraph=http://localhost:8080/msal4jsample/graph/me
```

В портал Azure коды URI ответа, которые необходимо зарегистрировать на странице **проверки подлинности** для приложения, должны совпадать с `redirectUri` экземплярами, которые определяет приложение. То есть они должны быть `http://localhost:8080/msal4jsample/secure/aad` и. `http://localhost:8080/msal4jsample/graph/me`

# <a name="python"></a>[Python](#tab/python)

Ниже приведен файл конфигурации Python в [app_config. Корректировка](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app_config.py):

```Python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
AUTHORITY = "https://login.microsoftonline.com/common""
CLIENT_ID = "Enter_the_Application_Id_here"
ENDPOINT = 'https://graph.microsoft.com/v1.0/users'
SCOPE = ["User.ReadBasic.All"]
SESSION_TYPE = "filesystem"  # So the token cache will be stored in a server-side session
```

> [!NOTE]
> В этом кратком руководстве предлагается сохранить секрет клиента в файле конфигурации для простоты. В рабочем приложении необходимо использовать другие способы хранения секрета, например хранилище ключей или переменную среды, как описано в [документации по Flask](https://flask.palletsprojects.com/en/1.1.x/config/#configuring-from-environment-variables).
>
> ```python
> CLIENT_SECRET = os.getenv("CLIENT_SECRET")
> if not CLIENT_SECRET:
>     raise ValueError("Need to define CLIENT_SECRET environment variable")
> ```

---

## <a name="initialization-code"></a>Код инициализации

Код инициализации различается в зависимости от платформы. Для ASP.NET Core и ASP.NET при входе в систему пользователи делегируются по промежуточного слоя OpenID Connect Connect. Шаблон ASP.NET или ASP.NET Core создает веб-приложения для конечной точки Azure Active Directory (Azure AD) v 1.0. Для адаптации их к конечной точке платформы Microsoft Identity Platform (v 2.0) требуется определенная конфигурация. В случае с Java оно обрабатывается пружиной с совместным использованием приложения.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

В ASP.NET Core веб-приложениях (и веб-API) приложение защищено, так как у `[Authorize]` вас есть атрибут на контроллерах или действия контроллера. Этот атрибут проверяет, прошел ли пользователь проверку подлинности. Код, который инициализирует приложение, находится в файле *Startup.CS* .

Чтобы добавить проверку подлинности на платформе Microsoft Identity (прежнее название — Azure AD 2.0), необходимо добавить следующий код. Комментарии в коде должны быть описательными.

> [!NOTE]
> Если вы запускаете проект с веб-проектом ASP.NET Core по умолчанию в Visual Studio или `dotnet new mvc --auth SingleAuth` с `dotnet new webapp --auth SingleAuth`помощью или, вы увидите код, подобный `services.AddAuthentication(AzureADDefaults.AuthenticationScheme).AddAzureAD(options => Configuration.Bind("AzureAd", options));`следующему:.
> 
> В этом коде используется устаревший пакет NuGet **Microsoft. AspNetCore. Authentication. AzureAD. UI** , который используется для создания приложения Azure AD версии 1.0. В этой статье объясняется, как создать приложение платформы удостоверений Microsoft Identity (Azure AD 2.0), заменяющее этот код.

1. Добавьте в проект пакеты NuGet [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web) и [Microsoft. Identity. Web. UI](https://www.nuget.org/packages/Microsoft.Identity.Web.UI) . Удалите пакет NuGet Microsoft. AspNetCore. Authentication. AzureAD. UI, если он имеется.

2. Обновите код в `ConfigureServices` , чтобы он использовал методы `AddSignIn` и `AddMicrosoftIdentityUI` .

   ```c#
   public class Startup
   {
    ...
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
     services.AddSignIn(Configuration, "AzureAd");

     services.AddRazorPages().AddMvcOptions(options =>
     {
      var policy = new AuthorizationPolicyBuilder()
                    .RequireAuthenticatedUser()
                    .Build();
      options.Filters.Add(new AuthorizeFilter(policy));
     }).AddMicrosoftIdentityUI();
    ```

3. В `Configure` методе в *Startup.CS*включите проверку подлинности с помощью вызова`app.UseAuthentication();`

   ```c#
   // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
   public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
   {
    // more code here
    app.UseAuthentication();
    app.UseAuthorization();
    // more code here
   }
   ```

В приведенном выше коде:
- Метод `AddSignIn` расширения определен в **Microsoft. Identity. Web**. Им
  - Добавляет службу проверки подлинности.
  - Настраивает параметры для чтения файла конфигурации (здесь из раздела "AzureAD").
  - Настраивает параметры OpenID Connect Connect, чтобы центр сертификации был конечной точкой платформы Microsoft Identity.
  - Проверяет издателя маркера.
  - Гарантирует, что утверждения, соответствующие имени, сопоставляются с `preferred_username` утверждением в маркере идентификации.

- В дополнение к объекту конфигурации можно указать имя раздела конфигурации при вызове `AddSignIn`. По умолчанию это `AzureAd`.

- `AddSignIn`имеет другие параметры для расширенных сценариев. Например, трассировка событий по промежуточного слоя OpenID Connect позволяет устранить неполадки в веб-приложении, если проверка подлинности не работает. При установке `subscribeToOpenIdConnectMiddlewareDiagnosticsEvents` необязательного `true` параметра будет показано, как обрабатывается информация по по промежуточного слоя ASP.NET Core по мере продвижения от HTTP-ответа к удостоверению пользователя в `HttpContext.User`.

- Метод `AddMicrosoftIdentityUI` расширения определен в **Microsoft. Identity. Web. UI**. Он предоставляет контроллер по умолчанию для управления выходом.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Код, связанный с проверкой подлинности в веб-приложении ASP.NET и веб-API, находится в файле [App_Start/стартуп.АУС.КС](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs#L17-L61) .

```csharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // `Authority` represents the identity platform endpoint - https://login.microsoftonline.com/common/v2.0.
     // `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/.
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

# <a name="java"></a>[Java](#tab/java)

В примере Java используется пружинная платформа. Приложение защищено, так как вы реализуете фильтр, который перехватывает каждый HTTP-ответ. В кратком руководстве по веб-приложениям Java этот фильтр `AuthFilter` находится `src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java`в.

Фильтр обрабатывает поток кода авторизации OAuth 2,0 и проверяет, прошел ли пользователь проверку подлинности (`isAuthenticated()` метод). Если пользователь не прошел проверку подлинности, он рассчитывает URL-адрес конечных точек авторизации Azure AD и перенаправляет браузер на этот универсальный код ресурса (URI).

Когда получен ответ, содержащий код авторизации, он получает маркер с помощью MSAL Java. Когда он наконец получает маркер из конечной точки маркера (в URI перенаправления), пользователь вошел в систему.

Дополнительные сведения см. в `doFilter()` описании метода в [аусфилтер. Java](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/msal-java-webapp-sample/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java).

> [!NOTE]
> Код `doFilter()` написан немного иначе, но последовательность описана в этом порядке.

Дополнительные сведения о потоке кода авторизации, который запускается этим методом, см. в [статье поток кода авторизации платформы Microsoft Identity и OAuth 2,0](v2-oauth2-auth-code-flow.md).

# <a name="python"></a>[Python](#tab/python)

В образце Python используется Flask. Инициализация Flask и MSAL Python выполняется в [app. копировать # L1-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L1-L28).

```Python
import uuid
import requests
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)
```

---

## <a name="next-steps"></a>Дальнейшие действия

В следующей статье вы узнаете, как активировать вход и выход.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Функции входа и выхода](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Функции входа и выхода](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Функции входа и выхода](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Функции входа и выхода](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=python)

---
