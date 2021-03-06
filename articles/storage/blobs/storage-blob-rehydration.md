---
title: Восстановление данных большого двоичного объекта из уровня архива
description: Восстановление больших двоичных объектов из архивного хранилища, чтобы получить доступ к данным.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/08/2020
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 82ea4ad23e3207f5641ade196f69595cd1e7b323
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81684108"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Восстановление данных большого двоичного объекта из уровня архива

Хотя большой двоичный объект находится в архивном уровне, он считается автономным и не может быть прочитан или изменен. Метаданные большого двоичного объекта остаются в сети и доступны, что позволяет перечислить большой двоичный объект и его свойства. Чтение и изменение данных большого двоичного объекта доступно только для веб-уровней, таких как горячий или крутой. Существует два варианта получения и доступа к данным, хранящимся в архивном уровне доступа.

1. Восстановление [архивного большого двоичного объекта на веб-уровне](#rehydrate-an-archived-blob-to-an-online-tier) . повторное сохранение архивного большого двоичного объекта на горячий или холодное изменение его уровня с помощью операции [задания уровня большого двоичного объекта](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) .
2. [Копирование архивного большого двоичного объекта на уровень "в сети](#copy-an-archived-blob-to-an-online-tier) ". Создание новой копии архивного большого двоичного объекта с помощью операции [копирования BLOB](https://docs.microsoft.com/rest/api/storageservices/copy-blob) -объектов. Укажите другое имя большого двоичного объекта и целевой уровень "горячий" или "холодное".

 Дополнительные сведения об уровнях см. в статье [хранилище BLOB-объектов Azure: горячий, стильный и архивный уровни доступа](storage-blob-storage-tiers.md).

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Восстановление архивного большого двоичного объекта на уровне сети

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Copy an archived blob to an online tier (Копирование архивного большого двоичного объекта на подключенный уровень)

Если вы не хотите воссоздать архивный BLOB-объект, вы можете выбрать операцию [копирования больших двоичных объектов](https://docs.microsoft.com/rest/api/storageservices/copy-blob) . Исходный большой двоичный объект останется неизменным в архиве, а новый большой двоичный объект создается на активном или холодном уровне для работы. В операции копирования больших двоичных объектов можно также задать для необязательного свойства *x-MS-rehydratя-Priority* значение Standard или High, чтобы указать приоритет, по которому должна быть создана копия большого двоичного объекта.

Копирование большого двоичного объекта из архива может занять несколько часов в зависимости от выбранного приоритета rehydratи. В фоновом режиме операция **копирования большого двоичного объекта** считывает исходный BLOB-объект архива для создания нового большого двоичного объекта в сети на выбранном целевом уровне. Новый большой двоичный объект может быть видимым при перечислении больших двоичных объектов, но данные недоступны до тех пор, пока чтение из исходного архивного объекта архива не будет завершено и данные будут записаны в новый большой двоичный объект в сети. Новый большой двоичный объект является независимой копией, и любое изменение или удаление этого объекта не влияет на большой двоичный объект архива источника.

Архивные BLOB-объекты могут быть скопированы только на уровни назначения в сети в пределах одной учетной записи хранения. Копирование большого двоичного объекта архива в другой архивный большой двоичный объект не поддерживается. В следующей таблице указаны возможности CopyBlob.

|                                           | **Источник "горячий" уровень**   | **Источник «холодного уровня»** | **Исходный уровень архива**    |
| ----------------------------------------- | --------------------- | -------------------- | ------------------- |
| **Назначение "горячий уровень"**                  | Поддерживается             | Поддерживается            | Поддерживается в одной учетной записи; Ожидание повтора               |
| **Назначение для холодного уровня**                 | Поддерживается             | Поддерживается            | Поддерживается в одной учетной записи; Ожидание повтора               |
| **Назначение уровня архива**              | Поддерживается             | Поддерживается            | Не поддерживается         |

## <a name="pricing-and-billing"></a>Цены и выставление счетов

Восстановление больших двоичных объектов из архива на горячий или холодной уровень осуществляется как операции чтения и извлечения данных. Использование высокого приоритета имеет более высокие затраты на операции и получение данных по сравнению с стандартным приоритетом. Восстановление с высоким приоритетом отображается как отдельный элемент строки в счете. Если запрос с высоким приоритетом, возвращающий большой двоичный объект архива на несколько гигабайт, занимает более 5 часов, плата за получение с высоким приоритетом не будет заряжена. Однако стандартные тарифные курсы по-прежнему применяются, так как для восстановления задано приоритетное восстановление по другим запросам.

Копирование больших двоичных объектов из архива в "горячий" или "холодный" уровень осуществляется в виде операций чтения и извлечения данных. Для создания новой копии большого двоичного объекта выставляются счета за операцию записи. При копировании в оперативный большой двоичный объект не применяются сборы за раннее удаление, так как исходный BLOB-объект остается неизменным на уровне архива. Если выбран этот флажок, то при выборе этого условия взимается плата за получение высокого приоритета.

Большие двоичные объекты на уровне архива должны храниться в течение не менее 180 дней. Удаление или восстановление архивных больших двоичных объектов до 180 дней приведет к более раннему удалению.

> [!NOTE]
> Дополнительные сведения о ценах на блочные BLOB-объекты и восстановление данных см. в разделе [цены на хранилище Azure](https://azure.microsoft.com/pricing/details/storage/blobs/). Дополнительные сведения о расходах на передачу исходящих данных см. в разделе [сведения о ценах на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart-scenarios"></a>Сценарии быстрого запуска

### <a name="rehydrate-an-archive-blob-to-an-online-tier"></a>Восстановление архивного большого двоичного объекта на уровне сети
# <a name="portal"></a>[Портал](#tab/azure-portal)
1. Войдите на [портал Azure](https://portal.azure.com).

1. В портал Azure найдите и выберите **все ресурсы**.

1. Затем выберите учетную запись хранения.

1. Выберите контейнер, а затем выберите свой BLOB-объект.

1. В **свойствах большого двоичного объекта**выберите **изменить уровень**.

1. Выберите уровень **горячего** или **холодного** доступа. 

1. Выберите уровень приоритета " **стандартный** " или " **высокий**".

1. Щелкните **сохранить** внизу.

![Изменить состояние возврата проверки](media/storage-tiers/blob-access-tier.png)
![для уровня учетной записи хранения](media/storage-tiers/rehydrate-status.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Следующий сценарий PowerShell можно использовать для изменения уровня большого двоичного объекта архива. `$rgName` Переменная должна быть инициализирована с именем группы ресурсов. `$accountName` Переменная должна быть инициализирована с использованием имени учетной записи хранения. `$containerName` Переменная должна быть инициализирована с помощью имени контейнера. `$blobName` Переменная должна быть инициализирована с помощью имени большого двоичного объекта. 
```powershell
#Initialize the following with your resource group, storage account, container, and blob names
$rgName = ""
$accountName = ""
$containerName = ""
$blobName = ""

#Select the storage account and get the context
$storageAccount =Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

#Select the blob from a container
$blobs = Get-AzStorageBlob -Container $containerName -Blob $blobName -Context $context

#Change the blob’s access tier to Hot using Standard priority rehydrate
$blob.ICloudBlob.SetStandardBlobTier("Hot", “Standard”)
```
---

### <a name="copy-an-archive-blob-to-a-new-blob-with-an-online-tier"></a>Копирование архивного большого двоичного объекта в новый большой двоичный объект на уровне сети
Следующий сценарий PowerShell можно использовать для копирования большого двоичного объекта архива в новый большой двоичный объект в той же учетной записи хранения. `$rgName` Переменная должна быть инициализирована с именем группы ресурсов. `$accountName` Переменная должна быть инициализирована с использованием имени учетной записи хранения. Переменные `$srcContainerName` и `$destContainerName` должны быть инициализированы с помощью имен контейнеров. Переменные `$srcBlobName` и `$destBlobName` должны быть инициализированы с помощью имен больших двоичных объектов. 
```powershell
#Initialize the following with your resource group, storage account, container, and blob names
$rgName = ""
$accountName = ""
$srcContainerName = ""
$destContainerName = ""
$srcBlobName = ""
$destBlobName = ""

#Select the storage account and get the context
$storageAccount =Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

#Copy source blob to a new destination blob with access tier hot using standard rehydrate priority
Start-AzStorageBlobCopy -SrcContainer $srcContainerName -SrcBlob $srcBlobName -DestContainer $destContainerName -DestBlob $destBlobName -StandardBlobTier Hot -RehydratePriority Standard -Context $ctx
```

## <a name="next-steps"></a>Дальнейшие шаги

* [Дополнительные сведения о уровнях хранилища BLOB-объектов](storage-blob-storage-tiers.md)
* [Цены на горячий, холодный и архивный уровни в учетных записях хранилища BLOB-объектов и учетных записях GPv2 по регионам](https://azure.microsoft.com/pricing/details/storage/)
* [Управление жизненным циклом хранилища BLOB-объектов Azure](storage-lifecycle-management-concepts.md)
* [Проверка сведений о ценах на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/)
