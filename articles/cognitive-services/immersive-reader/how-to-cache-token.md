---
title: Кэширование маркера проверки подлинности
titleSuffix: Azure Cognitive Services
description: В этой статье показано, как кэшировать маркер проверки подлинности.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: e652aa29b1c1935fcc4887dbe13ef9b683a8bd05
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "75946169"
---
# <a name="how-to-cache-the-authentication-token"></a>Кэширование маркера проверки подлинности

В этой статье показано, как кэшировать маркер проверки подлинности для повышения производительности приложения.

## <a name="using-aspnet"></a>Использование ASP.NET

Импортируйте пакет NuGet **Microsoft. IdentityModel. Clients. ActiveDirectory** , который используется для получения маркера. Затем используйте следующий код `AuthenticationResult`, чтобы получить, используя значения проверки подлинности, которые были получены при [создании иммерсивного ресурса чтения](./how-to-create-immersive-reader.md).

```csharp
private async Task<AuthenticationResult> GetTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext($"https://login.windows.net/{TENANT_ID}");
    ClientCredential clientCredential = new ClientCredential(CLIENT_ID, CLIENT_SECRET);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", clientCredential);
    return authResult;
}
```

`AuthenticationResult` Объект имеет `AccessToken` свойство, которое является фактическим маркером, который будет использоваться при запуске иммерсивное средство чтения с помощью пакета SDK. Он также имеет `ExpiresOn` свойство, которое обозначает, когда истечет срок действия маркера. Перед запуском иммерсивное средство чтения можно проверить, истек ли срок действия маркера, и получить новый токен, только если срок его действия истек.

## <a name="using-nodejs"></a>Использование Node. JS

Добавьте пакет [**request**](https://www.npmjs.com/package/request) NPM в проект. Используйте следующий код, чтобы получить маркер, используя значения проверки подлинности, полученные при [создании иммерсивного ресурса чтения](./how-to-create-immersive-reader.md).

```javascript
router.get('/token', function(req, res) {
    request.post(
        {
            headers: { 'content-type': 'application/x-www-form-urlencoded' },
            url: `https://login.windows.net/${TENANT_ID}/oauth2/token`,
            form: {
                grant_type: 'client_credentials',
                client_id: CLIENT_ID,
                client_secret: CLIENT_SECRET,
                resource: 'https://cognitiveservices.azure.com/'
            }
        },
        function(err, resp, json) {
            const result = JSON.parse(json);
            return res.send({
                access_token: result.access_token,
                expires_on: result.expires_on
            });
        }
    );
});
```

`expires_on` Свойство — это дата и время истечения срока действия маркера, выраженное в виде числа секунд, прошедших с 1 января 1970. в формате UTC. Используйте это значение, чтобы определить, истек ли срок действия токена, прежде чем пытаться получить новый.

```javascript
async function getToken() {
    if (Date.now() / 1000 > CREDENTIALS.expires_on) {
        CREDENTIALS = await refreshCredentials();
    }
    return CREDENTIALS.access_token;
}
```

## <a name="next-steps"></a>Следующие шаги

* Ознакомьтесь со справочной документацией по [пакету SDK для иммерсивного средства чтения](./reference.md).