---
title: Поиск содержимого хранилища BLOB-объектов Azure
titleSuffix: Azure Cognitive Search
description: Узнайте, как индексировать хранилище BLOB-объектов Azure и извлекать текст из документов с помощью Azure Когнитивный поиск.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 5df1198e6681431738f886eb7c3ad549936eab1a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80067644"
---
# <a name="how-to-index-documents-in-azure-blob-storage-with-azure-cognitive-search"></a>Индексирование документов в хранилище BLOB-объектов Azure с помощью Azure Когнитивный поиск

В этой статье показано, как использовать Когнитивный поиск Azure для индексирования документов (например, документов PDF, документов Microsoft Office и некоторых других стандартных форматов), хранящихся в хранилище BLOB-объектов Azure. Во-первых, объясняются основные принципы установки и настройки индексатора больших двоичных объектов. Затем предлагается более углубленно изучить возможные сценарии и поведения.

<a name="SupportedFormats"></a>

## <a name="supported-document-formats"></a>Поддерживаемые форматы документов
Индексатор больших двоичных объектов может извлечь текст из документов следующих форматов:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>Настройка индексирования больших двоичных объектов
Можно настроить индексатор хранилище BLOB-объектов Azure с помощью следующих инструментов:

* [Портал Azure](https://ms.portal.azure.com)
* [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations) когнитивный Поиск Azure
* [Пакет SDK](https://aka.ms/search-sdk) Azure когнитивный Поиск для .NET

> [!NOTE]
> Некоторые функции (например, сопоставление полей) еще не доступны на портале и должны быть использованы посредством кода.
>

Здесь демонстрируется поток с использованием интерфейса REST API.

### <a name="step-1-create-a-data-source"></a>Шаг 1. Создание источника данных
Источник данных определяет, какие данные следует индексировать, какие учетные данные требуются для доступа к данным и какие политики нужны, чтобы эффективно выявлять изменения в данных (новые, измененные или удаленные строки). Источник данных могут использовать несколько индексаторов в одной службе поиска.

Чтобы индексировать большие двоичные объекты, источник данных должен иметь следующие необходимые свойства.

* Свойство **name** — уникальное имя источника данных в службе поиска.
* Свойство **type** должно иметь значение `azureblob`.
* Свойство **credentials** предоставляют строку подключения к учетной записи как параметр `credentials.connectionString`. Дополнительные сведения см. в разделе [Как указать учетные данные](#Credentials) ниже.
* Свойство **container** определяет контейнер в вашей учетной записи хранения. По умолчанию все большие двоичные объекты в контейнере доступны для извлечения. Если требуется индексирование больших двоичных объектов из определенного виртуального каталога, можно указать этот каталог с помощью дополнительного параметра **query**.

Создание источника данных

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Чтобы больше узнать об API создания источника данных, ознакомьтесь с [созданием источника данных](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Как указать учетные данные ####

Учетные данные для контейнера BLOB-объектов можно указать одним из описанных ниже способов.

- **Строка подключения учетной записи хранения с полным доступом**. строку подключения можно получить из портал Azure. `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` для этого перейдите в колонку учетной записи хранения > параметры > ключи (для классических учетных записей хранения) или параметры > ключи доступа (для Azure Resource Manager учетных записей хранения).
- **Строка подключения с подписанным URL-адресом (SAS) учетной записи хранения**. `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` Подписанный URL-адрес должен иметь разрешения на перечисление и чтение для контейнеров и объектов (в этом случае BLOB-объектов).
-  **Подписанный** `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` URL для контейнера. SAS должен иметь разрешения List и Read для контейнера.

Дополнительные сведения о подписанных URL-адресах см. в статье [Использование подписанных URL-адресов (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Если используются учетные данные SAS, то необходимо периодически обновлять учетные данные источника данных с помощью обновленных подписей, чтобы не истек их срок действия. В случае истечения срока действия учетных данных SAS произойдет сбой индексатора и появится сообщение об ошибке примерно следующего содержания: `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Шаг 2. Создание индекса
Индекс задает поля в документе, атрибуты, и другие компоненты, которые определяют процедуру поиска.

Ниже описан процесс создания индекса с доступным для поиска полем `content` для хранения текста, извлеченного из больших двоичных объектов:   

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Дополнительные сведения о создании индексов см. в разделе [Создание индекса](https://docs.microsoft.com/rest/api/searchservice/create-index) .

### <a name="step-3-create-an-indexer"></a>Шаг 3. Создание индексатора
Индексатор соединяет источник данных с целевым индексом поиска и предоставляет расписание для автоматизации обновления данных.

Когда индекс и источник данных уже созданы, можно создать индексатор:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Этот индексатор будет выполняться каждые два часа (интервал расписания имеет значение "PT2H"). Чтобы запускать индексатор каждые 30 минут, задайте интервал "PT30M". Самый короткий интервал, который можно задать, составляет 5 минут. Расписание является необязательным. Если оно не указано, то индексатор выполняется только один раз при его создании. Однако индексатор можно запустить по запросу в любое время.   

Дополнительные сведения об API создания индексатора см. в разделе [Создание индексатора](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Дополнительные сведения об определении расписаний индексаторов см. [в статье Планирование индексаторов для когнитивный Поиск Azure](search-howto-schedule-indexers.md).

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-azure-cognitive-search-indexes-blobs"></a>Как Azure Когнитивный поиск индексирует большие двоичные объекты

В зависимости от [конфигурации](#PartsOfBlobToIndex) индексатор больших двоичных объектов может индексировать только метаданные хранилища (удобно, если вам необходимо индексировать только метаданные и не требуется индексировать содержимое больших двоичных объектов), метаданные хранилища и содержимое или и метаданные, и текстовое содержимое. По умолчанию индексатор извлекает и метаданные, и содержимое.

> [!NOTE]
> По умолчанию большие двоичные объекты со структурированным содержимым, например JSON- или CSV-файлы, индексируются как один блок текста. Чтобы получить дополнительные сведения об индексировании больших двоичных объектов JSON и CSV, см. раздел [индексирование больших](search-howto-index-json-blobs.md) двоичных объектов JSON и [индексирование больших двоичных объектов CSV](search-howto-index-csv-blobs.md) .
>
> Составной или внедренный документ (например, ZIP-файл или документ Word с внедренным электронным сообщением Outlook, содержащим вложения) также индексируется как один документ.

* Текстовое содержимое документа извлекается в строковое поле `content`.

> [!NOTE]
> Azure Когнитивный поиск ограничивает объем извлекаемого текста в зависимости от ценовой категории: 32 000 символов для уровня Free, 64 000 для Basic, 4 000 000 для Standard, 8 000 000 для Standard S2 и 16 000 000 для уровня Standard S3. Предупреждение об усеченных документах отобразится в возвращенном состоянии индексатора.  

* В большом двоичном объекте определяемые пользователем свойства метаданных извлекаются без изменений. Обратите внимание, что для этого требуется, чтобы поле было определено в индексе с тем же именем, что и ключ метаданных большого двоичного объекта. Например, если у большого двоичного объекта есть ключ метаданных `Sensitivity` со значением `High`, необходимо определить поле с именем `Sensitivity` в индексе поиска, которое будет заполнено значением. `High`
* Стандартные свойства метаданных большого двоичного объекта извлекаются в указанные ниже поля.

  * **metadata\_storage\_name** (Edm.String) — имя файла большого двоичного объекта. Например, для большого двоичного объекта /my-container/my-folder/subfolder/resume.pdf значение этого поля — `resume.pdf`.
  * **metadata\_storage\_path** (Edm.String) — полный универсальный код ресурса (URI) большого двоичного объекта, включая учетную запись хранения. Например `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`.
  * **metadata\_storage\_content\_type** (Edm.String) — тип содержимого, указанный в коде для отправки большого двоичного объекта. Например, `application/octet-stream`.
  * **metadata\_storage\_last\_modified** (Edm.DateTimeOffset) — последняя измененная метка времени для большого двоичного объекта. Когнитивный поиск Azure использует эту метку времени для обнаружения измененных больших двоичных объектов, чтобы избежать повторного индексирования всех объектов после первоначального индексирования.
  * **metadata\_storage\_size** (Edm.Int64) — размер большого двоичного объекта в байтах.
  * **metadata\_storage\_content\_metadatastoragecontentmd5** (Edm.String) — хэш MD5 содержимого большого двоичного объекта, если он доступен.
  * **metadata\_storage\_sas\_token** (Edm.String) — временный маркер SAS, который может использоваться [пользовательскими навыками](cognitive-search-custom-skill-interface.md) для получения доступа к большому двоичному объекту. Этот токен не должен храниться для дальнейшего использования, так как срок его действия может истечь.

* Определенные для каждого формата документа свойства метаданных извлекаются в поля, список которых приводится [здесь](#ContentSpecificMetadata).

Вам не нужно определять поля для всех перечисленных свойств в индексе поиска — достаточно записать свойства, необходимые для приложения.

> [!NOTE]
> Как правило, имена полей в существующем индексе отличаются от имен полей, созданных при извлечении документа. **Сопоставления полей** можно использовать для сопоставления имен свойств, предоставленных когнитивный Поиск Azure, с именами полей в индексе поиска. Ниже вы увидите пример использования сопоставления полей.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Определение ключей документа и сопоставлений полей
В Azure Когнитивный поиск ключ документа однозначно определяет документ. Каждый индекс поиска должен содержать только одно поле ключа типа Edm.String. Поле ключа является обязательным для каждого документа, который добавляется к индексу (собственно, это единственное обязательное для заполнения поле).  

Следует определить, какие извлеченные поля должны сопоставляться с полем ключа индекса. Основные варианты представлены ниже.

* **metadata\_storage\_name** — удобный вариант, но следует учесть следующее. 1) Имена могут не быть уникальными, так как возможно наличие больших двоичных объектов с одинаковыми именами в разных папках. 2) Имя может содержать знаки, недопустимые для использования в ключах документов, например дефисы. Устранить проблему недопустимых знаков можно с помощью  [функции сопоставления полей](search-indexer-field-mappings.md#base64EncodeFunction)`base64Encode`. После ее применения обязательно закодируйте ключи документов для передачи в вызовах API, таких как вызов для поиска. (Например, в .NET для этого можно использовать [метод UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx)).
* **metadata\_storage\_path** — использование полного пути гарантирует уникальность, но такой путь определенно содержит знаки `/`, [недопустимые в ключе документа](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Как упоминалось выше, вы можете закодировать ключи с помощью  [функции](search-indexer-field-mappings.md#base64EncodeFunction)`base64Encode`.
* Если ни один вариант не подходит, вы можете добавить пользовательское свойство метаданных в большие двоичные объекты. Однако для этого варианта требуется, чтобы при передаче эти свойства метаданных добавлялись ко всем большим двоичным объектам. Поскольку ключ является обязательным свойством, все большие двоичные объекты без этого свойства индексироваться не будут.

> [!IMPORTANT]
> Если для ключевого поля в индексе нет явного сопоставления, Azure Когнитивный поиск автоматически использует `metadata_storage_path` в качестве ключа, а base-64 кодирует значения ключей (второй параметр выше).
>
>

В этом примере мы выберем поле `metadata_storage_name` в качестве ключа документа. Предположим, что индекс содержит поле ключа с именем `key` и поле `fileSize` для хранения данных о размере документа. Чтобы правильно связать все компоненты при создании или обновлении индексатора, укажите следующие сопоставления полей:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Чтобы объединить все компоненты, можно добавить сопоставления полей и активировать кодировку ключей base-64 для существующего индексатора:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> Дополнительные сведения о сопоставлении полей см. в [этой статье](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Управление индексированием больших двоичных объектов
Вы можете выбирать, какие большие двоичные объекты необходимо индексировать, а какие — пропустить.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Индексация только BLOB-объектов с определенными расширениями файлов
Параметр конфигурации индексатора `indexedFileNameExtensions` позволяет индексировать большие двоичные объекты только с указанными расширениями имен файлов. Значение представлено строкой, содержащей разделенный запятыми список расширений файлов (с предшествующей точкой). Например, чтобы индексировать только большие двоичные объекты PDF и DOCX, выполните следующую команду:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Исключение больших двоичных объектов с определенными расширениями файлов
Большие двоичные объекты с определенными расширениями файлов можно исключить из индексации с помощью параметра конфигурации `excludedFileNameExtensions`. Значение представлено строкой, содержащей разделенный запятыми список расширений файлов (с предшествующей точкой). Например, для индексации всех больших двоичных объектов, кроме объектов с расширениями PNG и JPEG, выполните следующую команду:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Если существуют `indexedFileNameExtensions` оба `excludedFileNameExtensions` параметра и, Azure когнитивный поиск сначала просматривает `indexedFileNameExtensions`, а затем по `excludedFileNameExtensions`адресу. Это означает, что если одно расширение файла присутствует в обоих списках, объект будет исключен из индексации.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Управление индексированием частей большого двоичного объекта

Вы можете выбрать, какие части большого двоичного объекта необходимо индексировать, с помощью параметра конфигурации `dataToExtract`. Этот параметр может принимать перечисленные ниже значения.

* `storageMetadata` — указывает, что необходимо индексировать только [стандартные свойства большого двоичного объекта и метаданные, определяемые пользователем](../storage/blobs/storage-properties-metadata.md).
* `allMetadata` — указывает, что необходимо индексировать метаданные хранилища и [метаданные определенного типа содержимого](#ContentSpecificMetadata), извлеченные из содержимого большого двоичного объекта.
* `contentAndMetadata` — указывает, что необходимо индексировать все метаданные и текстовое содержимое, извлеченное из большого двоичного объекта. Это значение по умолчанию.

Например, чтобы индексировать только метаданные хранилища, используйте следующую команду:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Использование метаданных большого двоичного объекта для определения способа индексирования больших двоичных объектов

Описанные выше параметры конфигурации применяются ко всем большим двоичным объектам. В некоторых случаях вам может потребоваться определять способ индексирования *отдельных больших двоичных объектов*. Это можно сделать, добавив следующие свойства и значения метаданных большого двоичного объекта.

| Имя свойства. | Значение свойства | Объяснение |
| --- | --- | --- |
| AzureSearch_Skip |True |Указывает индексатору больших двоичных объектов пропустить весь большой двоичный объект. Не извлекаются ни метаданные, ни содержимое. Это полезно, когда обработка определенного большого двоичного объекта постоянно завершается сбоем и индексирование прерывается. |
| AzureSearch_SkipContent |True |Это эквивалент параметра `"dataToExtract" : "allMetadata"`, описанного [выше](#PartsOfBlobToIndex), относящегося к определенному большому двоичному объекту. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Обработка ошибок

По умолчанию индексатор больших двоичных объектов останавливается, как только обнаружит большой двоичный объект с неподдерживаемым типом содержимого (например, изображение). Чтобы пропустить определенные типы содержимого, можно воспользоваться параметром `excludedFileNameExtensions`. Тем не менее может потребоваться индексировать большие двоичные объекты, не зная все возможные типы их содержимого. Чтобы продолжить индексирование при обнаружении неподдерживаемого типа содержимого, задайте для параметра конфигурации `failOnUnsupportedContentType` значение `false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

Для некоторых больших двоичных объектов Azure Когнитивный поиск не может определить тип содержимого или обработать документ из другого типа содержимого, поддерживаемого другим способом. Чтобы пропустить этот сбой, задайте для `failOnUnprocessableDocument` параметра конфигурации значение False:

      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }

Azure Когнитивный поиск ограничивает размер индексируемых больших двоичных объектов. Эти ограничения описаны в статье [ограничения службы в Azure когнитивный Поиск](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity). Большие двоичные объекты слишком большого размера по умолчанию считаются ошибками. Но вы все равно можете индексировать метаданные хранилища больших двоичных объектов слишком большого размера, если для параметра конфигурации `indexStorageMetadataOnlyForOversizedDocuments` задать значение true: 

    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }

Вы также можете продолжить индексирование, если ошибки возникают в какой-либо момент обработки — при анализе больших двоичных объектов или при добавлении документов в индекс. Чтобы пропустить определенное количество ошибок, задайте для параметров конфигурации `maxFailedItems` и `maxFailedItemsPerBatch` нужные значения. Пример:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

## <a name="incremental-indexing-and-deletion-detection"></a>Добавочное индексирование и обнаружение удаления 

Когда для индексатора больших двоичных объектов вы настраиваете запуск по расписанию, повторно индексируются только измененные большие двоичные объекты. Они определяются по метке времени большого двоичного объекта `LastModified`.

> [!NOTE]
> Нет необходимости указывать политику обнаружения изменений — добавочное индексирование будет активировано автоматически.

Для удаления документов используйте механизм обратимого удаления. Если просто удалить большие двоичные объекты, соответствующие документы останутся в индексе поиска.

Существует два способа реализации подхода обратимого удаления. Ниже описаны оба варианта.

### <a name="native-blob-soft-delete-preview"></a>Обратимое удаление собственного BLOB-объекта (Предварительная версия)

> [!IMPORTANT]
> Поддержка обратимого удаления больших двоичных объектов доступна в предварительной версии. Для предварительной версии функции соглашение об уровне обслуживания не предусмотрено. Мы не рекомендуем использовать ее в рабочей среде. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Эта функция предоставляется в [версии REST API 2019-05-06-Preview](https://docs.microsoft.com/azure/search/search-api-preview). В настоящее время нет поддержки портала или пакета SDK для .NET.

> [!NOTE]
> При использовании политики обратимого удаления BLOB-объектов ключи документов для документов в индексе должны быть свойством большого двоичного объекта или метаданными большого двоичного объекта.

В этом методе будет использоваться [собственная функция обратимого удаления больших двоичных объектов](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) , предоставляемая хранилищем BLOB-объектов Azure. Если в вашей учетной записи хранения включено встроенное обратимое удаление BLOB-объектов, то в источнике данных будет установлена собственная политика обратимого удаления, и индексатор обнаружит большой двоичный объект, который был перенесен в обратимо Удаленное состояние, индексатор удалит этот документ из индекса. Политика обратимого удаления больших двоичных объектов не поддерживается при индексировании больших двоичных объектов из Azure Data Lake Storage 2-го поколения.

Выполните указанные ниже действия.
1. Включите [встроенное обратимое удаление для хранилища BLOB-объектов Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete). Рекомендуется задать для политики хранения значение, превышающее расписание интервала индексатора. Таким образом, если возникла проблема с индексатором или если имеется большое количество документов для индексирования, то в конечном итоге индексатор будет обрабатывать обратимо удаленные большие двоичные объекты. Индексаторы Когнитивный поиск Azure удаляют документ из индекса, только если он обрабатывает большой двоичный объект, когда он находится в обратимо удаленном состоянии.
1. Настройте собственную политику обнаружения обратимого удаления больших двоичных объектов в источнике данных. Ниже приведен пример такого файла. Так как эта функция находится на этапе предварительной версии, необходимо использовать предварительную версию REST API.
1. Запустите индексатор или задайте выполнение индексатора по расписанию. Когда индексатор запускается и обрабатывает большой двоичный объект, документ будет удален из индекса.

    ```
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

#### <a name="reindexing-undeleted-blobs"></a>Повторное индексирование неудаленных больших двоичных объектов

При удалении большого двоичного объекта из хранилища BLOB-объектов Azure с включенным встроенным обратимым удалением в учетной записи хранения большой двоичный объект переходит в обратимое Удаленное состояние, что позволяет отменить удаление этого большого двоичного объекта в течение срока хранения. Если источник данных Когнитивный поиск Azure имеет собственную политику обратимого удаления больших двоичных объектов, а индексатор обрабатывает Обратимо удаленный большой двоичный объект, этот документ будет удален из индекса. При последующем удалении этого большого двоичного объекта индексатор не всегда будет переиндексации этого большого двоичного объекта. Это обусловлено тем, что индексатор определяет, какие большие двоичные объекты следует индексировать `LastModified` , на основе метки времени большого двоичного объекта. При отмене удаления обратимо удаляемого большого двоичного объекта его `LastModified` метка времени не обновляется, поэтому, если индексатор уже обработал большие `LastModified` двоичные объекты с метками времени более поздней, чем неудаленный BLOB-объект, он не будет повторно индексировать неудаленный большой двоичный объект. Чтобы гарантировать повторное индексирование неудаленного большого двоичного объекта, необходимо обновить `LastModified` метку времени большого двоичного объекта. Один из способов сделать это — повторное сохранение метаданных этого большого двоичного объекта. Вам не нужно изменять метаданные, но при повторном сохранении метаданных будет обновлена `LastModified` метка времени большого двоичного объекта, чтобы индексатор знал, что ему нужно переиндексировать этот большой двоичный объект.

### <a name="soft-delete-using-custom-metadata"></a>Обратимое удаление с помощью пользовательских метаданных

В этом методе вы будете использовать метаданные большого двоичного объекта, чтобы указать, когда документ должен быть удален из индекса поиска.

Выполните указанные ниже действия.

1. Добавьте пару "ключ — значение" пользовательских метаданных в большой двоичный объект, чтобы указать Когнитивный поиск Azure логически удалена.
1. Настройте политику обнаружения столбцов обратимого удаления в источнике данных. Ниже приведен пример такого файла.
1. После того как индексатор обработал большой двоичный объект и удалил документ из индекса, можно удалить большой двоичный объект для хранилища BLOB-объектов Azure.

Например, в соответствии с приведенной ниже политикой большой двоичный объект считается удаленным, если у него есть свойство метаданных `IsDeleted` со значением `true`.

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }

#### <a name="reindexing-undeleted-blobs"></a>Повторное индексирование неудаленных больших двоичных объектов

Если задать для источника данных политику обнаружения обратимого удаления столбцов, а затем добавить пользовательские метаданные в большой двоичный объект со значением маркера, а затем запустить индексатор, индексатор удалит этот документ из индекса. Если вы хотите переиндексировать этот документ, просто измените значение метаданных обратимого удаления для этого большого двоичного объекта и перезапустите индексатор.

## <a name="indexing-large-datasets"></a>Индексирование больших наборов данных

Индексирование больших двоичных объектов может занимать много времени. Если вам необходимо индексировать миллионы больших двоичных объектов, этот процесс можно ускорить, секционировав данные и используя несколько индексаторов для обработки данных в параллельном режиме. Вот как это можно сделать.

- Секционируйте данные в несколько контейнеров больших двоичных объектов или виртуальных папок.
- Настройте несколько источников данных Azure Когнитивный поиск, по одному на контейнер или папку. Чтобы указать папку большого двоичного объекта, используйте параметр `query`:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Создайте соответствующий индексатор для каждого источника данных. Все индексаторы могут указывать на один и тот же целевой индекс поиска.  

- Одна единица поиска в службе в определенный момент времени может запустить один индексатор. Создание нескольких индексаторов, как описано выше, полезно только при их фактическом параллельном выполнении. Для параллельного выполнения нескольких индексаторов следует увеличить масштаб службы поиска, создав достаточное число секций и реплик. Например, если служба поиска содержит 6 единиц поиска (скажем, 2 секции x 3 реплики), то параллельно могут выполняться 6 индексаторов. Это позволяет достичь шестикратного увеличения пропускной способности индексации. Дополнительные сведения о масштабировании и планировании емкости см. [в статье масштабирование уровней ресурсов для рабочих нагрузок запросов и индексирования в Azure когнитивный Поиск](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>Индексация документов и связанных данных

Возможно, вам нужно создать в индексе "сборку" документов из нескольких источников. Например, можно объединить текст из BLOB-объектов с другими метаданными, хранящимися в Cosmos DB. Можно даже использовать API принудительного индексирования вместе с другими индексаторами, чтобы создавать документы для поиска из нескольких частей. 

Чтобы это работало, ключ документа должен быть согласован для всех индексаторов и других компонентов. Дополнительные сведения об этом разделе см. в статье [индексирование нескольких источников данных Azure](https://docs.microsoft.com/azure/search/tutorial-multiple-data-sources). Подробное пошаговое руководство см. в этой внешней статье: [Объединение документов с другими данными в когнитивный Поиск Azure](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Индексирование обычного текста 

Если все BLOB-объекты содержат обычный текст в одной и той же кодировке, можно существенно повысить производительность индексирования, используя **текстовый режим анализа**. Чтобы использовать текстовый режим анализа, задайте для свойства конфигурации `parsingMode` значение `text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

По умолчанию предполагается кодировка `UTF-8`. Чтобы указать другую кодировку, используйте свойство конфигурации `encoding`: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Свойства метаданных определенного типа содержимого
В следующей таблице приводится сводка по обработке для каждого формата документа и описываются свойства метаданных, извлеченные Когнитивный поиск Azure.

| Формат документа или тип содержимого | Свойства метаданных определенного типа содержимого | Сведения об обработке |
| --- | --- | --- |
| HTML (text/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Удаление разметки HTML и извлечение текста |
| PDF (приложение/PDF) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Извлечение текста, включая внедренные документы (кроме изображений) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| DOCM (приложение/vnd. MS-Word. Document. макроенаблед. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| WORD XML (application/vnd. MS-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Удаление разметки XML и извлечение текста |
| WORD 2003 XML (application/vnd. МС-вордмл) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |Удаление разметки XML и извлечение текста |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| XLSM (приложение/vnd. MS-Excel а. sheet. макроенаблед. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Извлечение текста, включая внедренные документы |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Извлечение текста, включая внедренные документы |
| PPTM (приложение/vnd. МС-поверпоинт. Presentation. макроенаблед. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Извлечение текста, включая внедренные документы |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Извлечение текста, включая вложения. `metadata_message_to_email``metadata_message_cc_email` и `metadata_message_bcc_email` являются строковыми коллекциями, остальные поля являются строками.|
| ODT (application/vnd. Oasis. OpenDocument. Text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| ODS (application/vnd. Oasis. OpenDocument. электронных таблиц) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| ODP (приложение/vnd. Oasis. OpenDocument. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Извлечение текста, включая внедренные документы |
| ZIP (application/zip) |`metadata_content_type` |Извлечение текста из всех документов в архиве |
| GZ (приложение/gzip) |`metadata_content_type` |Извлечение текста из всех документов в архиве |
| ЕПУБ (Application/епуб + ZIP) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Извлечение текста из всех документов в архиве |
| XML (application/xml) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |Удаление разметки XML и извлечение текста |
| JSON (application/json) |`metadata_content_type`<br/>`metadata_content_encoding` |Извлечение текста<br/>Примечание. Инструкции по извлечению нескольких полей документа из большого двоичного объекта JSON см. в статье [Индексирование BLOB-объектов JSON с помощью индексатора BLOB-объектов службы поиска Azure](search-howto-index-json-blobs.md). |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Извлечение текста, включая вложения |
| RTF (приложение или RTF) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Извлечение текста|
| Обычный текст (text/plain) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Извлечение текста|


## <a name="help-us-make-azure-cognitive-search-better"></a>Помогите нам сделать Azure Когнитивный поиск лучше
Если вам нужна какая-либо функция или у вас есть идеи, которые можно было бы реализовать, сообщите об этом на [сайте UserVoice](https://feedback.azure.com/forums/263029-azure-search/).
