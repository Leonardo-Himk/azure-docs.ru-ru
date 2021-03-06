---
title: Управление уровнями согласованности в Azure Cosmos DB
description: Узнайте, как настраивать уровни согласованности и управлять ими в Azure Cosmos DB с помощью портал Azure, пакета SDK для .NET, пакета SDK для Java и различных других пакетов SDK.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: mjbrown
ms.openlocfilehash: 28266471fb1e440a45e412ee889e0706cfc2ce49
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82870097"
---
# <a name="manage-consistency-levels-in-azure-cosmos-db"></a>Управление уровнями согласованности в Azure Cosmos DB

В этой статье рассматривается управление уровнями согласованности в Azure Cosmos DB. Вы узнаете, как настроить уровень согласованности по умолчанию, а также как переопределить ее и управлять маркерами сеанса вручную. Кроме того, здесь приведены общие сведения о метрике вероятностного ограниченного устаревания (PBS).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-the-default-consistency-level"></a>Настройка уровня согласованности по умолчанию

Клиенты по умолчанию используют [стандартный уровень согласованности](consistency-levels.md).

# <a name="azure-portal"></a>[Портал Azure](#tab/portal)

Чтобы просмотреть или изменить уровень согласованности по умолчанию, войдите на портал Azure. Найдите учетную запись Azure Cosmos DB и откройте область **Согласованность по умолчанию**. Выберите требуемый уровень согласованности в качестве нового значения по умолчанию, а затем выберите **Сохранить**. На портале Azure также представлена визуализация разных уровней согласованности на основе музыкальных нот. 

![Меню согласованности на портале Azure](./media/how-to-manage-consistency/consistency-settings.png)

# <a name="cli"></a>[CLI](#tab/cli)

Создайте учетную запись Cosmos с согласованностью сеанса, а затем обновите согласованность по умолчанию.

```azurecli
# Create a new account with Session consistency
az cosmosdb create --name $accountName --resource-group $resourceGroupName --default-consistency-level Session

# update an existing account's default consistency
az cosmosdb update --name $accountName --resource-group $resourceGroupName --default-consistency-level Strong
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Создайте учетную запись Cosmos с согласованностью сеанса, а затем обновите согласованность по умолчанию.

```azurepowershell-interactive
# Create a new account with Session consistency
New-AzCosmosDBAccount -ResourceGroupName $resourceGroupName `
  -Location $locations -Name $accountName -DefaultConsistencyLevel "Session"

# Update an existing account's default consistency
Update-AzCosmosDBAccount -ResourceGroupName $resourceGroupName `
  -Name $accountName -DefaultConsistencyLevel "Strong"
```

---

## <a name="override-the-default-consistency-level"></a>Переопределение уровня согласованности по умолчанию

Клиенты могут переопределить уровень согласованности по умолчанию, который задается службой. Уровень согласованности можно задать для каждого запроса. В этом случае будет переопределен уровень согласованности по умолчанию на уровне учетной записи.

> [!TIP]
> Согласованность может быть **ослаблена** только на уровне запроса. Чтобы перейти от более слабого к более надежному согласованию, обновите согласованность по умолчанию для учетной записи Cosmos.

### <a name="net-sdk"></a><a id="override-default-consistency-dotnet"></a>Пакет SDK для .NET

# <a name="net-sdk-v2"></a>[ПАКЕТ SDK ДЛЯ .NET ВЕРСИИ 2](#tab/dotnetv2)

```csharp
// Override consistency at the client level
documentClient = new DocumentClient(new Uri(endpoint), authKey, connectionPolicy, ConsistencyLevel.Eventual);

// Override consistency at the request level via request options
RequestOptions requestOptions = new RequestOptions { ConsistencyLevel = ConsistencyLevel.Eventual };

var response = await client.CreateDocumentAsync(collectionUri, document, requestOptions);
```

# <a name="net-sdk-v3"></a>[ПАКЕТ SDK ДЛЯ .NET V3](#tab/dotnetv3)

```csharp
// Override consistency at the request level via request options
ItemRequestOptions requestOptions = new ItemRequestOptions { ConsistencyLevel = ConsistencyLevel.Strong };

var response = await client.GetContainer(databaseName, containerName)
    .CreateItemAsync(
        item,
        new PartitionKey(itemPartitionKey),
        requestOptions);
```
---

### <a name="java-sdk"></a><a id="override-default-consistency-java"></a>Пакет SDK для Java

# <a name="java-async-sdk"></a>[Пакет SDK для Java Async](#tab/javaasync)

```java
// Override consistency at the client level
ConnectionPolicy policy = new ConnectionPolicy();

AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConsistencyLevel(ConsistencyLevel.Eventual)
                .withConnectionPolicy(policy).build();
```

# <a name="java-sync-sdk"></a>[Пакет SDK для синхронизации Java](#tab/javasync)

```java
// Override consistency at the client level
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy, ConsistencyLevel.Eventual);
```
---

### <a name="nodejsjavascripttypescript-sdk"></a><a id="override-default-consistency-javascript"></a>Пакет SDK для Node.js, JavaScript и TypeScript

```javascript
// Override consistency at the client level
const client = new CosmosClient({
  /* other config... */
  consistencyLevel: ConsistencyLevel.Eventual
});

// Override consistency at the request level via request options
const { body } = await item.read({ consistencyLevel: ConsistencyLevel.Eventual });
```

### <a name="python-sdk"></a><a id="override-default-consistency-python"></a>Пакет SDK для Python

```python
# Override consistency at the client level
connection_policy = documents.ConnectionPolicy()
client = cosmos_client.CosmosClient(self.account_endpoint, {
                                    'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Eventual)
```

## <a name="utilize-session-tokens"></a>Использование маркеров сеанса

Один из этих уровней согласованности в Azure Cosmos DB — согласованность *сеансов*. Этот уровень по умолчанию применяются к учетным записям Cosmos. Если задана согласованность *сеансов*, то клиент будет использовать внутренний маркер сеанса для каждой операции чтения или запроса, чтобы обеспечить соблюдение заданного уровня согласованности.

Чтобы управлять токенами сеанса вручную, получите токен сеанса из ответа и установите их для каждого запроса. Если у вас нет необходимости управлять токенами сеанса вручную, то вам не нужно использовать эти примеры. Пакет SDK отслеживает токены сеанса автоматически. Если токен сеанса не задан вручную, по умолчанию в пакете SDK используется последний.

### <a name="net-sdk"></a><a id="utilize-session-tokens-dotnet"></a>Пакет SDK для .NET

# <a name="net-sdk-v2"></a>[ПАКЕТ SDK ДЛЯ .NET ВЕРСИИ 2](#tab/dotnetv2)

```csharp
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"));
string sessionToken = response.SessionToken;

RequestOptions options = new RequestOptions();
options.SessionToken = sessionToken;
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"), options);
```

# <a name="net-sdk-v3"></a>[ПАКЕТ SDK ДЛЯ .NET V3](#tab/dotnetv3)

```csharp
Container container = client.GetContainer(databaseName, collectionName);
ItemResponse<SalesOrder> response = await container.CreateItemAsync<SalesOrder>(salesOrder);
string sessionToken = response.Headers.Session;

ItemRequestOptions options = new ItemRequestOptions();
options.SessionToken = sessionToken;
ItemResponse<SalesOrder> response = await container.ReadItemAsync<SalesOrder>(salesOrder.Id, new PartitionKey(salesOrder.PartitionKey), options);
```
---

### <a name="java-sdk"></a><a id="utilize-session-tokens-java"></a>Пакет SDK для Java

# <a name="java-async-sdk"></a>[Пакет SDK для Java Async](#tab/javaasync)

```java
// Get session token from response
RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey(document.get("mypk")));
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
readObservable.single()           // we know there will be one response
  .subscribe(
      documentResourceResponse -> {
          System.out.println(documentResourceResponse.getSessionToken());
      },
      error -> {
          System.err.println("an error happened: " + error.getMessage());
      });

// Resume the session by setting the session token on RequestOptions
RequestOptions options = new RequestOptions();
requestOptions.setSessionToken(sessionToken);
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
```

# <a name="java-sync-sdk"></a>[Пакет SDK для синхронизации Java](#tab/javasync)

```java
// Get session token from response
ResourceResponse<Document> response = client.readDocument(documentLink, null);
String sessionToken = response.getSessionToken();

// Resume the session by setting the session token on the RequestOptions
RequestOptions options = new RequestOptions();
options.setSessionToken(sessionToken);
ResourceResponse<Document> response = client.readDocument(documentLink, options);
```
---

### <a name="nodejsjavascripttypescript-sdk"></a><a id="utilize-session-tokens-javascript"></a>Пакет SDK для Node.js, JavaScript и TypeScript

```javascript
// Get session token from response
const { headers, item } = await container.items.create({ id: "meaningful-id" });
const sessionToken = headers["x-ms-session-token"];

// Immediately or later, you can use that sessionToken from the header to resume that session.
const { body } = await item.read({ sessionToken });
```

### <a name="python-sdk"></a><a id="utilize-session-tokens-python"></a>Пакет SDK для Python

```python
// Get the session token from the last response headers
item = client.ReadItem(item_link)
session_token = client.last_response_headers["x-ms-session-token"]

// Resume the session by setting the session token on the options for the request
options = {
    "sessionToken": session_token
}
item = client.ReadItem(doc_link, options)
```

## <a name="monitor-probabilistically-bounded-staleness-pbs-metric"></a>Мониторинг метрики вероятностного ограниченного устаревания (PBS)

Насколько итоговая согласованность является итоговой? В среднем мы можем предложить границы устаревания с учетом журнала версий и времени. Метрика [**Probabilistically Bounded Staleness (PBS)**](https://pbs.cs.berkeley.edu/) (Вероятностное ограниченное устаревание) пытается количественно оценить вероятность устаревания и отображает полученные результаты. Чтобы просмотреть метрику PBS, перейдите к учетной записи Azure Cosmos DB на портале Azure. Откройте панель **метрики** и перейдите на вкладку **согласованность** . Просмотрите граф с именем **вероятность строго согласованных операций чтения на основе рабочей нагрузки (см. PBS)**.

![График PBS на портале Azure](./media/how-to-manage-consistency/pbs-metric.png)

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о том, как управлять конфликтами данных, или перейдите к следующей ключевой концепции в Azure Cosmos DB. См. следующие статьи:

* [Настраиваемые уровни согласованности данных в Azure Cosmos DB](consistency-levels.md)
* [Управление конфликтами между регионами](how-to-manage-conflicts.md)
* [Секционирование и масштабирование в Azure Cosmos DB](partition-data.md)
* [Компромиссы согласованности в современных распределенных системах баз данных](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
* [Высокая доступность](high-availability.md)
* [Соглашение об уровне обслуживания для Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
