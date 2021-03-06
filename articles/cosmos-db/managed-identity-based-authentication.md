---
title: Использование назначенного системой управляемого удостоверения для доступа к данным Azure Cosmos DB
description: Узнайте, как настроить Azure Active Directory (Azure AD) управляемое удостоверение (управляемое удостоверение службы) для доступа к ключам из Azure Cosmos DB.
author: j-patrick
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: justipat
ms.reviewer: sngun
ms.openlocfilehash: 8136ad7a1fe29bc3394e959c10aafc52988c0a23
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81641231"
---
# <a name="use-system-assigned-managed-identities-to-access-azure-cosmos-db-data"></a>Использование управляемых системой удостоверений для доступа к данным Azure Cosmos DB

В этой статье вы настроите *надежное, независимое от ключа* решение для доступа к Azure Cosmos DBным ключам с помощью [управляемых удостоверений](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md). В примере, приведенном в этой статье, используются функции Azure, но можно использовать любую службу, которая поддерживает управляемые удостоверения. 

Вы узнаете, как создать приложение-функцию, которое может получать доступ к данным Azure Cosmos DB без необходимости копировать ключи Azure Cosmos DB. Приложение функции запустит каждую минуту и запишет текущую температуру в акуариумной цистерне. Чтобы узнать, как настроить активируемое таймером приложение-функцию, см. статью [Создание функции в Azure, которая активируется с помощью таймера](../azure-functions/functions-create-scheduled-function.md) .

Для упрощения сценария параметр [срок жизни](./time-to-live.md) уже настроен на очистку старых температурных документов. 

## <a name="assign-a-system-assigned-managed-identity-to-a-function-app"></a>Назначение управляемого системой удостоверения для приложения-функции

На этом шаге вы назначите приложению-функции управляемое удостоверение, назначенное системой.

1. В [портал Azure](https://portal.azure.com/)откройте панель **функций Azure** и перейдите к приложению функции. 

1. Откройте вкладку > **удостоверение** **функции платформы**: 

   ![Снимок экрана, показывающий функции платформы и параметры идентификации для приложения функции.](./media/managed-identity-based-authentication/identity-tab-selection.png)

1. На вкладке **удостоверение** **включите** **состояние** удостоверения системы и нажмите кнопку **сохранить**. Панель **удостоверений** должна выглядеть следующим образом:  

   ![Снимок экрана с состоянием удостоверения системы, установленным в значение ON.](./media/managed-identity-based-authentication/identity-tab-system-managed-on.png)

## <a name="grant-access-to-your-azure-cosmos-account"></a>Предоставление доступа к учетной записи Azure Cosmos

На этом шаге вы назначите роль управляемому удостоверению, назначенному системой для приложения-функции. Azure Cosmos DB имеет несколько встроенных ролей, которые можно назначить управляемому удостоверению. Для этого решения вы будете использовать следующие две роли:

|Встроенные роли  |Описание  |
|---------|---------|
|[Участник учетной записи DocumentDB](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Может управлять учетными записями Azure Cosmos DB Позволяет получать ключи для чтения и записи. |
|[Средство чтения учетных записей Cosmos DB](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Позволяет считывать данные учетных записей Azure Cosmos DB. Позволяет получать ключи чтения. |

> [!IMPORTANT]
> Поддержка управления доступом на основе ролей в Azure Cosmos DB применяется только к операциям управления плоскостью. Операции с плоскостью данных защищаются с помощью главных ключей или маркеров ресурсов. Дополнительные сведения см. в статье [безопасный доступ к данным](secure-access-to-data.md) .

> [!TIP] 
> При назначении ролей назначьте только необходимый доступ. Если службе требуется только чтение данных, назначьте управляемому удостоверению роль **читателя учетной записи Cosmos DB** . Дополнительные сведения о важности минимальных прав доступа см. в статье о [низком уровне уязвимости привилегированных учетных записей](../security/fundamentals/identity-management-best-practices.md#lower-exposure-of-privileged-accounts) .

В этом сценарии приложение-функция будет считывать температуру акуариум, а затем записывать эти данные в контейнер в Azure Cosmos DB. Так как приложение-функция должно записать данные, необходимо назначить роль **участника учетной записи DocumentDB** . 

1. Войдите в портал Azure и перейдите к учетной записи Azure Cosmos DB. Откройте панель **управления доступом (IAM)** и перейдите на вкладку **назначения ролей** :

   ![Снимок экрана, показывающий панель управления доступом и вкладку назначения ролей.](./media/managed-identity-based-authentication/cosmos-db-iam-tab.png)

1. Выберите **+ Добавить** > **добавить назначение ролей**.

1. Справа откроется панель **Добавление назначения ролей** .

   ![Снимок экрана, показывающий панель "Добавление назначения ролей".](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane.png)

   * **Роль**: выбор **участника учетной записи DocumentDB**
   * **Назначение доступа к**: в подразделе **Выбор управляемого системой удостоверения** выберите **приложение-функция**.
   * **SELECT**: область будет заполнена всеми приложениями-функциями в подписке, которые имеют **управляемое удостоверение системы**. В этом случае выберите приложение функции **фиштанктемпературесервице** : 

      ![Снимок экрана, на котором отображается панель "Добавление назначения ролей", заполненная примерами.](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane-filled.png)

1. После выбора приложения функции нажмите кнопку **сохранить**.

## <a name="programmatically-access-the-azure-cosmos-db-keys"></a>Программный доступ к ключам Azure Cosmos DB

Теперь у нас есть приложение-функция, которое имеет назначенное системой управляемое удостоверение с ролью **участника учетной записи DocumentDB** в Azure Cosmos DB разрешений. Следующий код приложения-функции получит ключи Azure Cosmos DB, создать объект Космосклиент, получить температуру акуариум, а затем сохранить его в Azure Cosmos DB.

В этом примере для доступа к ключам учетной записи Azure Cosmos DB используется [API списка ключей](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListKeys) .

> [!IMPORTANT] 
> Если вы хотите [назначить роль читателя учетной записи Cosmos DB](#grant-access-to-your-azure-cosmos-account) , вам потребуется использовать [интерфейс API списка ключей только для чтения](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListReadOnlyKeys). Это приведет к заполнению только тех ключей, которые доступны только для чтения.

API списка ключей возвращает `DatabaseAccountListKeysResult` объект. Этот тип не определен в библиотеках C#. В следующем коде показана реализация этого класса:  

```csharp 
namespace Monitor 
{
  public class DatabaseAccountListKeysResult
  {
      public string primaryMasterKey {get;set;}
      public string primaryReadonlyMasterKey {get; set;}
      public string secondaryMasterKey {get; set;}
      public string secondaryReadonlyMasterKey {get;set;}
  }
}
```

В примере также используется простой документ с именем «Температуререкорд», который определяется следующим образом:

```csharp
using System;

namespace Monitor
{
    public class TemperatureRecord
    {
        public string id { get; set; } = Guid.NewGuid().ToString();
        public DateTime RecordTime { get; set; }
        public int Temperature { get; set; }

    }
}
```

Для получения управляемого системой маркера удостоверения используется библиотека [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) . Чтобы узнать о других способах получения маркера и получить дополнительные сведения о `Microsoft.Azure.Service.AppAuthentication` библиотеке, ознакомьтесь со статьей [Проверка подлинности между](../key-vault/general/service-to-service-authentication.md) службами.


```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Microsoft.Azure.Cosmos;
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

namespace Monitor
{
    public static class FishTankTemperatureService
    {
        private static string subscriptionId =
        "<azure subscription id>";
        private static string resourceGroupName =
        "<name of your azure resource group>";
        private static string accountName =
        "<Azure Cosmos DB account name>";
        private static string cosmosDbEndpoint =
        "<Azure Cosmos DB endpoint>";
        private static string databaseName =
        "<Azure Cosmos DB name>";
        private static string containerName =
        "<container to store the temperature in>";

        [FunctionName("FishTankTemperatureService")]
        public static async Task Run([TimerTrigger("0 * * * * *")]TimerInfo myTimer, ILogger log)
        {
            log.LogInformation($"Starting temperature monitoring: {DateTime.Now}");

            // AzureServiceTokenProvider will help us to get the Service Managed token.
            var azureServiceTokenProvider = new AzureServiceTokenProvider();

            // Authenticate to the Azure Resource Manager to get the Service Managed token.
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");

            // Setup the List Keys API to get the Azure Cosmos DB keys.
            string endpoint = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2019-12-12";

            // Setup an HTTP Client and add the access token.
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            // Post to the endpoint to get the keys result.
            var result = await httpClient.PostAsync(endpoint, new StringContent(""));

            // Get the result back as a DatabaseAccountListKeysResult.
            DatabaseAccountListKeysResult keys = await result.Content.ReadAsAsync<DatabaseAccountListKeysResult>();

            log.LogInformation("Starting to create the client");

            CosmosClient client = new CosmosClient(cosmosDbEndpoint, keys.primaryMasterKey);

            log.LogInformation("Client created");

            var database = client.GetDatabase(databaseName);
            var container = database.GetContainer(containerName);

            log.LogInformation("Get the temperature.");

            var tempRecord = new TemperatureRecord() { RecordTime = DateTime.UtcNow, Temperature = GetTemperature() };

            log.LogInformation("Store temperature");

            await container.CreateItemAsync<TemperatureRecord>(tempRecord);

            log.LogInformation($"Ending temperature monitor: {DateTime.Now}");
        }

        private static int GetTemperature()
        {
            // Fake the temperature sensor for this demo.
            Random r = new Random(DateTime.UtcNow.Second);
            return r.Next(0, 120);
        }
    }
}
```

Теперь вы готовы к [развертыванию приложения функции](../azure-functions/functions-create-first-function-vs-code.md).

## <a name="next-steps"></a>Дальнейшие шаги

* [Проверка подлинности на основе сертификатов с помощью Azure Cosmos DB и Azure Active Directory](certificate-based-authentication.md)
* [Защита ключей Azure Cosmos DB с помощью Azure Key Vault](access-secrets-from-keyvault.md)
* [Базовый уровень безопасности для Azure Cosmos DB](security-baseline.md)
