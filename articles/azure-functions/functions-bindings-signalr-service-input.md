---
title: Входная привязка службы SignalR для функций Azure
description: Узнайте, как получить URL-адрес конечной точки службы SignalR и маркер доступа в функциях Azure.
author: craigshoemaker
ms.topic: reference
ms.date: 02/20/2020
ms.author: cshoe
ms.openlocfilehash: 53d336aff3177a76c5e02266ffb8484bd9945119
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77530267"
---
# <a name="signalr-service-input-binding-for-azure-functions"></a>Входная привязка службы SignalR для функций Azure

Прежде чем клиент сможет подключиться к службе Azure SignalR, необходимо получить URL-адрес конечной точки службы и действительный маркер доступа. Входная привязка *SignalRConnectionInfo* создает URL-адрес конечной точки службы SignalR и допустимый маркер, которые используются для подключения к службе. Так как маркер ограничен по времени и может использоваться для идентификации конкретного пользователя при подключении, не следует его кэшировать или передавать в совместное пользование между клиентами. С помощью этой привязки клиенты могут использовать триггер HTTP для получения сведений о подключении.

Дополнительные сведения о том, как эта привязка используется для создания функции "Negotiate", которая может использоваться клиентским пакетом SDK для SignalR, см. в [статье о разработке и настройке функций Azure](../azure-signalr/signalr-concept-serverless-development-config.md) в документации по основным понятиям службы SignalR.

Дополнительные сведения об установке и сведениях о конфигурации см. в [обзоре](functions-bindings-signalr-service.md).

## <a name="example"></a>Пример

# <a name="c"></a>[C#](#tab/csharp)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), получающая сведения о подключении SignalR с помощью входной привязки и возвращающая их по протоколу HTTP.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="c-script"></a>[Скрипт C#](#tab/csharp-script)

В следующем примере показана входная привязка SignalR в файле *Function. JSON* и [функция сценария C#](functions-reference-csharp.md) , которая использует привязку для возврата сведений о соединении.

Данные привязки в файле *function.json*:

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Вот код сценария C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

В следующем примере показана входная привязка для сведений о подключении SignalR в файле *function.json* и [функция JavaScript](functions-reference-node.md), использующая привязку для возврата сведений о подключении.

Данные привязки в файле *function.json*:

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Ниже показан код JavaScript.

```javascript
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

# <a name="python"></a>[Python](#tab/python)

В следующем примере показана входная привязка SignalR в файле *Function. JSON* и [функция Python](functions-reference-python.md) , которая использует привязку для возврата сведений о соединении.

Данные привязки в файле *function.json*:

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Ниже приведен код Python.

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="java"></a>[Java](#tab/java)

В следующем примере показана [функция Java](functions-reference-java.md) , которая получает сведения о соединении SignalR с помощью входной привязки и возвращает ее по протоколу HTTP.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

---

## <a name="authenticated-tokens"></a>Прошедшие проверку подлинности маркеры

Если функцию активирует прошедший проверку подлинности клиент, вы можете добавить утверждение идентификатора пользователя для созданного маркера. Вы можете легко добавить проверку подлинности в приложение-функцию с помощью [проверки подлинности службы приложений](../app-service/overview-authentication-authorization.md).

Проверка подлинности службы приложений задает заголовки HTTP `x-ms-client-principal-id` и `x-ms-client-principal-name`, содержащие имя и идентификатор субъекта клиента прошедшего проверку подлинности пользователя соответственно.

# <a name="c"></a>[C#](#tab/csharp)

Можно задать для `UserId` свойства привязки значение из любого заголовка с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или. `{headers.x-ms-client-principal-name}`

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

# <a name="c-script"></a>[Скрипт C#](#tab/csharp-script)

Можно задать для `userId` свойства привязки значение из любого заголовка с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или. `{headers.x-ms-client-principal-name}`

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Вот код сценария C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    return connectionInfo;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Можно задать для `userId` свойства привязки значение из любого заголовка с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или. `{headers.x-ms-client-principal-name}`

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Ниже показан код JavaScript.

```javascript
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

# <a name="python"></a>[Python](#tab/python)

Можно задать для `userId` свойства привязки значение из любого заголовка с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или. `{headers.x-ms-client-principal-name}`

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Ниже приведен код Python.

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    # connectionInfo contains an access key token with a name identifier
    # claim set to the authenticated user
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="java"></a>[Java](#tab/java)

Можно задать для `userId` свойства привязки значение из любого заголовка с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или. `{headers.x-ms-client-principal-name}`

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat",
            userId = "{headers.x-ms-client-principal-id}") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

---

## <a name="next-steps"></a>Дальнейшие шаги

- [Отправка сообщений службы SignalR (Выходная привязка)](./functions-bindings-signalr-service-output.md) 
