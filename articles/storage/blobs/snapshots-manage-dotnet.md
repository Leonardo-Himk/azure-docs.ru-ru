---
title: Создание моментального снимка BLOB-объекта и управление им в .NET
titleSuffix: Azure Storage
description: Узнайте, как создать моментальный снимок большого двоичного объекта, доступного только для чтения, для резервного копирования данных большого двоичного объекта в определенный момент времени.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/02/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 438692d5c142c3f617ee9d0c3f55b9b3740f9b7a
ms.sourcegitcommit: d815163a1359f0df6ebfbfe985566d4951e38135
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2020
ms.locfileid: "82884700"
---
# <a name="create-and-manage-a-blob-snapshot-in-net"></a>Создание моментального снимка BLOB-объекта и управление им в .NET

Моментальный снимок — это версия BLOB-объекта только для чтения, сделанная в определенный момент времени. В этой статье показано, как создавать моментальные снимки BLOB-объектов и управлять ими с помощью [клиентской библиотеки службы хранилища Azure для .NET](/dotnet/api/overview/azure/storage?view=azure-dotnet).

Дополнительные сведения о моментальных снимках BLOB-объектов в службе хранилища Azure см. [в статье Создание моментального снимка большого двоичного объекта в .NET и управление им](snapshots-overview.md)

## <a name="create-a-snapshot"></a>Создание моментального снимка

# <a name="net-version-12x"></a>[Версия .NET 12. x](#tab/v12)

Чтобы создать моментальный снимок блочного BLOB-объекта с помощью версии 12. x клиентской библиотеки службы хранилища Azure для .NET, используйте один из следующих методов.

- [CreateSnapshot](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.createsnapshot)
- [креатеснапшотасинк](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.createsnapshotasync)

В следующем примере кода показано, как создать моментальный снимок с версией 12. x. Включите ссылку на библиотеку [Azure. Identity](https://www.nuget.org/packages/azure.identity) , чтобы использовать учетные данные Azure AD для авторизации запросов к службе.

```csharp
private static async Task CreateBlockBlobSnapshot(string accountName, string containerName, string blobName, Stream data)
{
    const string blobServiceEndpointSuffix = ".blob.core.windows.net";
    Uri containerUri = new Uri("https://" + accountName + blobServiceEndpointSuffix + "/" + containerName);

    // Get a container client object and create the container.
    BlobContainerClient containerClient = new BlobContainerClient(containerUri,
        new DefaultAzureCredential());
    await containerClient.CreateIfNotExistsAsync();

    // Get a blob client object.
    BlobClient blobClient = containerClient.GetBlobClient(blobName);

    try
    {
        // Upload text to create a block blob.
        await blobClient.UploadAsync(data);

        // Add blob metadata.
        IDictionary<string, string> metadata = new Dictionary<string, string>
        {
            { "ApproxBlobCreatedDate", DateTime.UtcNow.ToString() },
            { "FileType", "text" }
        };
        await blobClient.SetMetadataAsync(metadata);

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        await blobClient.CreateSnapshotAsync();
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

# <a name="net-version-11x"></a>[.NET версии 11. x](#tab/v11)

Чтобы создать моментальный снимок блочного BLOB-объекта с помощью версии 11. x клиентской библиотеки службы хранилища Azure для .NET, используйте один из следующих методов.

- [CreateSnapshot](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshot)
- [креатеснапшотасинк](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshotasync)

В следующем примере кода показано, как создать моментальный снимок с версией 11. x. В этом примере мы указываем дополнительные метаданные для моментального снимка при его создании.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        Console.WriteLine(snapshot.SnapshotQualifiedStorageUri.PrimaryUri);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

---

## <a name="delete-snapshots"></a>Удаление моментальных снимков

Чтобы удалить большой двоичный объект, необходимо сначала удалить все моментальные снимки этого большого двоичного объекта. Можно удалить моментальный снимок отдельно или указать, чтобы при удалении исходного большого двоичного объекта были удалены и все его моментальные снимки. Попытка удалить большой двоичный объект, у которого еще есть моментальные снимки, завершится ошибкой.

# <a name="net-version-12x"></a>[Версия .NET 12. x](#tab/v12)

Чтобы удалить большой двоичный объект и его моментальные снимки с помощью версии 12. x клиентской библиотеки службы хранилища Azure для .NET, используйте один из следующих методов и включите перечисление [делетеснапшотсоптион](/dotnet/api/azure.storage.blobs.models.deletesnapshotsoption) :

- [Удалить](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.delete)
- [DeleteAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteasync)
- [DeleteIfExists](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteifexistsasync)

В следующем примере кода показано, как удалить большой двоичный объект и его моментальные снимки в `blobClient` .NET, где — это объект типа [блобклиент](/dotnet/api/azure.storage.blobs.blobclient).

```csharp
await blobClient.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, default);
```

# <a name="net-version-11x"></a>[.NET версии 11. x](#tab/v11)

Чтобы удалить большой двоичный объект и его моментальные снимки с помощью версии 11. x клиентской библиотеки службы хранилища Azure для .NET, используйте один из следующих методов удаления больших двоичных объектов и включите перечисление [делетеснапшотсоптион](/dotnet/api/microsoft.azure.storage.blob.deletesnapshotsoption) :

- [Удалить](/dotnet/api/microsoft.azure.storage.blob.cloudblob.delete)
- [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexistsasync)

В следующем примере кода показано, как удалить большой двоичный объект и его моментальные снимки в `blockBlob` .NET, где является объектом типа [CloudBlockBlob] [dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

---

## <a name="next-steps"></a>Дальнейшие действия

- [Моментальные снимки BLOB-объектов](snapshots-overview.md)
- [Версии BLOB-объектов (Предварительная версия)](versioning-overview.md)
- [Обратимое удаление больших двоичных объектов](storage-blob-soft-delete.md)