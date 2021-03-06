---
title: Индексные BLOB-объекты, содержащие несколько документов
titleSuffix: Azure Cognitive Search
description: Сканирование больших двоичных объектов Azure для текстового содержимого с помощью индексатора BLOB-объектов поиска Azure Конгитиве, где каждый большой двоичный объект может получить один или несколько документов поискового индекса.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 1840bda0ecc9462a5d8f796b616d728d0bb412f7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74112260"
---
# <a name="indexing-blobs-to-produce-multiple-search-documents"></a>Индексирование больших двоичных объектов для получения нескольких поисковых документов
По умолчанию индексатор BLOB-объектов будет рассматривать содержимое большого двоичного объекта как единый поисковый документ. Определенные значения **parsingMode** поддерживают сценарии, в которых отдельный большой двоичный объект может привести к созданию нескольких документов поиска. Различные типы **parsingMode** , позволяющие индексатору извлекать более одного документа поиска из большого двоичного объекта:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

## <a name="one-to-many-document-key"></a>Ключ документа "один ко многим"
Каждый документ, который отображается в индексе Azure Когнитивный поиск, однозначно идентифицируется ключом документа. 

Если режим синтаксического анализа не задан, и если отсутствует явное сопоставление для ключевого поля в индексе Azure Когнитивный поиск автоматически [сопоставляет](search-indexer-field-mappings.md) `metadata_storage_path` свойство как ключ. Это сопоставление гарантирует, что каждый большой двоичный объект будет выглядеть как отдельный документ поиска.

При использовании любого из перечисленных выше режимов синтаксического анализа один большой двоичный объект сопоставляется с "многими" документами поиска, что делает ключ документа исключительно на основе метаданных BLOB-объектов. Чтобы преодолеть это ограничение, Azure Когнитивный поиск способен создать ключ документа "один ко многим" для каждой отдельной сущности, извлеченной из большого двоичного объекта. Это свойство имеет имя `AzureSearch_DocumentKey` и добавляется к каждой отдельной сущности, извлеченной из большого двоичного объекта. Значение этого свойства гарантированно уникально для каждой отдельной сущности _в больших двоичных_ объектах, и сущности будут отображаться как отдельные поисковые документы.

По умолчанию, если не указаны явные сопоставления полей для поля индекс ключа, `AzureSearch_DocumentKey` с помощью функции сопоставления `base64Encode` полей сопоставляется с ним.

## <a name="example"></a>Пример
Предположим, что у вас есть определение индекса со следующими полями:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

И контейнер больших двоичных объектов имеет большие двоичные объекты со следующей структурой:

_Blob1. JSON_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2. JSON_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Когда вы создаете индексатор и устанавливаете `jsonLines` для **parsingMode** значение-без указания явных сопоставлений полей для ключевого поля, следующее сопоставление будет применено неявно.
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Эта установка приведет к тому, что индекс Когнитивный поиск Azure будет содержать следующие сведения (идентификатор в кодировке Base64 сокращен для краткости).

| идентификатор | Температура | pressure | TIMESTAMP |
|----|-------------|----------|-----------|
| aHR0 ... ижеуаннвбжскс | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... ижеуаннвбжси | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... ижиуаннвбжскс | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0 ... ижиуаннвбжси | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Сопоставление настраиваемых полей для поля ключа индекса

Предположим, что определение индекса, как и в предыдущем примере, имеет большой двоичный объект, имеющий следующую структуру:

_Blob1. JSON_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2. JSON_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

При создании индексатора с `delimitedText` **parsingMode**может быть естественным, чтобы настроить функцию сопоставления полей для ключевого поля следующим образом:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Однако это сопоставление _не_ приведет к отображению 4 документов в индексе, так как `recordid` поле не является уникальным _для больших двоичных объектов_. Поэтому рекомендуется использовать неявное сопоставление полей, применяемое в `AzureSearch_DocumentKey` свойстве, к полю индекса ключа для режимов анализа "один ко многим".

Если вы хотите настроить явное сопоставление полей, убедитесь, что _саурцефиелд_ является уникальным для каждой отдельной сущности **во всех больших двоичных**объектах.

> [!NOTE]
> Подход, `AzureSearch_DocumentKey` используемый для обеспечения уникальности в извлеченной сущности, может изменяться, и поэтому не следует полагаться на его ценность для нужд вашего приложения.

## <a name="next-steps"></a>Дальнейшие действия

Если вы еще не знакомы с базовой структурой и рабочим процессом индексирования больших двоичных объектов, сначала следует ознакомиться [с индексацией хранилища BLOB-объектов Azure с помощью Azure когнитивный Поиск](search-howto-index-json-blobs.md) . Дополнительные сведения о режимах синтаксического анализа для различных типов содержимого BLOB-объектов см. в следующих статьях.

> [!div class="nextstepaction"]
> [Индексирование](search-howto-index-csv-blobs.md)
> больших двоичных объектов[JSON](search-howto-index-json-blobs.md) индексация BLOB-объектов
