---
title: Копирование данных в хранилище таблиц Azure и из него
description: Узнайте, как копировать данные из поддерживаемых исходных хранилищ в службу "Хранилище таблиц Azure" или из службы "Хранилище таблиц" в поддерживаемые хранилища-приемники с помощью фабрики данных.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/27/2019
ms.openlocfilehash: 6edd32f8f3579238d1f08f55ce9fb1528fa5d211
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417494"
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Копирование данных в службу "Хранилище таблиц Azure" и обратно с помощью фабрики данных Azure

> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-azure-table-connector.md)
> * [Текущая версия](connector-azure-table-storage.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье объясняется, как с помощью действия копирования в фабрике данных Azure копировать данные из службы "Хранилище таблиц Azure" и обратно. Он основан на статье [Обзор действия копирования](copy-activity-overview.md) , в которой представлен общий обзор действия копирования.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот соединитель табличного хранилища Azure поддерживается для следующих действий:

- [Действие копирования](copy-activity-overview.md) с [поддерживаемой матрицей источника и приемника](copy-activity-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)

Можно копировать данные из любого поддерживаемого исходного хранилища данных в службу "Хранилище таблиц". Вы можете скопировать данные из службы "Хранилище таблиц" в любое хранилище данных, поддерживаемое в качестве приемника. Список хранилищ данных, поддерживаемых действием копирования в качестве источников или приемников, см. в таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) .

В частности, соединитель таблиц Azure поддерживает копирование данных с использованием проверки подлинности на основе ключа учетной записи и подписанного URL-адреса (SAS) уровня службы.

## <a name="get-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, относящихся к службе "Хранилище таблиц".

## <a name="linked-service-properties"></a>Свойства связанной службы

### <a name="use-an-account-key"></a>Использование ключа учетной записи

Вы можете создать связанную службу хранилища Azure с помощью ключа учетной записи. В этом случае фабрика данных получает глобальный доступ к службе хранилища. Поддерживаются следующие свойства.

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **AzureTableStorage**. |Да |
| connectionString | В свойстве connectionString указываются сведения, необходимые для подключения к службе хранилища. <br/>Вы можете также поместить ключ учетной записи в Azure Key Vault и извлечь конфигурацию `accountKey` из строки подключения. Ознакомьтесь с приведенными ниже примерами и подробными сведениями в статье [Хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md). |Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md) , используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

>[!NOTE]
>Если вы использовали связанную службу типа AzureStorage, она по-прежнему поддерживается как есть, но в дальнейшем рекомендуется использовать новый тип связанной службы — AzureTableStorage.

**Пример.**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример: ключ учетной записи хранения в Azure Key Vault**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;",
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-shared-access-signature-authentication"></a>Использование аутентификации SAS

Вы также можете создать связанную службу хранилища с помощью подписанного URL-адреса. В этом случае фабрика данных получает ограниченный или привязанный ко времени доступ ко всем или конкретным ресурсам в хранилище.

Подпись общего доступа обеспечивает делегированный доступ к ресурсам в вашей учетной записи хранения. Вы можете использовать ее, чтобы предоставить ограниченное право на работу с объектами в вашей учетной записи хранения на определенный период и с определенным набором разрешений. Не нужно предоставлять совместный доступ к ключам доступа для учетной записи. Подписанный URL-адрес — это универсальный код ресурса (URI), который в своих параметрах запроса содержит все сведения, необходимые для доступа к ресурсу хранилища с прохождением аутентификации. Для доступа к ресурсам хранилища с помощью подписанного URL-адреса клиенту достаточно передать SAS в соответствующий конструктор или метод. Дополнительные сведения о подписанном URL-адресе см. в статье [Использование подписанных URL-адресов (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Сейчас Фабрика данных поддерживает как **подписанные URL-адреса уровня службы**, так и **подписанные URL-адреса уровня учетной записи**. Дополнительные сведения о подписанных URL-адресах см. [в статье предоставление ограниченного доступа к ресурсам службы хранилища Azure с помощью подписанных URL (SAS)](../storage/common/storage-sas-overview.md). 

> [!TIP]
> Чтобы создать подписанный URL-адрес уровня службы для учетной записи хранения, можно выполнить следующие команды PowerShell. Замените заполнители и предоставьте необходимое разрешение.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

При использовании аутентификации SAS поддерживаются следующие свойства.

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **AzureTableStorage**. |Да |
| sasUri | Укажите универсальный код ресурса (URI) подписанного URL-адреса для таблицы. <br/>Пометьте это поле как SecureString, чтобы безопасно хранить его в Фабрике данных. Также можно разместить маркер SAS в Azure Key Vault, чтобы использовать автоматический поворот и удалить часть токена. Ознакомьтесь с приведенными ниже примерами и подробными сведениями в статье [Хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md). | Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md) , используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

>[!NOTE]
>Если вы использовали связанную службу типа AzureStorage, она по-прежнему поддерживается как есть, но в дальнейшем рекомендуется использовать новый тип связанной службы — AzureTableStorage.

**Пример.**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<account>.table.core.windows.net/<table>?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример: ключ учетной записи хранения в Azure Key Vault**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<account>.table.core.windows.net/<table>>"
            },
            "sasToken": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

При создании URI подписанного URL-адреса необходимо учитывать следующее.

- Задайте для объектов соответствующие разрешения на чтение или запись. Они устанавливаются с учетом назначения связанной службы (чтение, запись, чтение и запись) в фабрике данных.
- Задайте **время окончания срока действия** соответствующим образом. Убедитесь, что срок действия доступа к объектам хранилища не истекает в период активности конвейера.
- В зависимости от потребности URI следует создать на нужном уровне таблицы.

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). Этот раздел содержит список свойств, поддерживаемых набором данных таблицы Azure.

Чтобы скопировать данные в таблицу Azure или обратно, установите тип свойства набора данных **AzureTable**. Поддерживаются следующие свойства.

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Свойство Type набора данных должно иметь значение **AzureTable**. |Да |
| tableName |Имя таблицы в экземпляре базы данных службы "Хранилище таблиц", на которое ссылается связанная служба. |Да |

**Пример.**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Table storage linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Схема фабрики данных

Для хранилищ данных без схемы, таких как таблица Azure, фабрика данных определяет схему одним из следующих способов.

* Если указать сопоставление столбцов в действии копирования, фабрика данных будет использовать список столбцов на стороне источника для получения данных. В этом случае, если строка не содержит значение столбца, ему присваивается значение NULL.
* Если не указать сопоставление столбцов в действии копирования, фабрика данных определит схему с помощью первой строки данных. В этом случае, если первая строка не содержит полную схему (например, некоторые столбцы имеют значение null), в результате операции копирования будут пропущены некоторые столбцы.

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником и приемником таблицы Azure.

### <a name="azure-table-as-a-source-type"></a>Таблица Azure в качестве типа источника

Чтобы копировать данные из таблицы Azure, установите тип источника **AzureTableSource** в действии копирования. В разделе **source** действия копирования поддерживаются следующие свойства.

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Свойство Type источника действия копирования должно иметь значение **AzureTableSource**. |Да |
| AzureTableSourceQuery |Используйте пользовательский запрос службы "Хранилище таблиц" для чтения данных. Примеры приведены в следующем разделе. |Нет |
| azureTableSourceIgnoreTableNotFound |Указывает, следует ли разрешить исключение, связанное с отсутствием таблицы.<br/>Допустимые значения: **true** и **false** (по умолчанию). |Нет |

### <a name="azuretablesourcequery-examples"></a>Примеры azureTableSourceQuery

>[!NOTE]
>Время ожидания операции запроса таблицы Azure истекает через 30 секунд, как это [обеспечивается службой таблиц Azure](https://docs.microsoft.com/rest/api/storageservices/setting-timeouts-for-table-service-operations). Узнайте, как оптимизировать запрос в статье [Создание запросов](../storage/tables/table-storage-design-for-query.md) .

В фабрике данных Azure, если нужно отфильтровать данные по столбцам типа DateTime, см. Следующий пример:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Если нужно отфильтровать данные по столбцу строкового типа, см. Следующий пример:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

При использовании параметра конвейера приведите значение даты и времени в правильный формат в соответствии с предыдущими примерами.

### <a name="azure-table-as-a-sink-type"></a>Таблица Azure в качестве типа приемника

Чтобы копировать данные в таблицу Azure, установите тип приемника **AzureTableSink** в действии копирования. В разделе **sink** действия копирования поддерживаются следующие свойства.

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Свойство type приемника действия копирования должно иметь значение **AzureTableSink**. |Да |
| azureTableDefaultPartitionKeyValue |Значение ключа раздела по умолчанию, которое может использоваться приемником. |Нет |
| azureTablePartitionKeyName |Укажите имя столбца, значения которого используются в качестве ключей разделов. Если не указано, в качестве ключа раздела используется AzureTableDefaultPartitionKeyValue. |Нет |
| azureTableRowKeyName |Укажите имя столбца, значения которого используются в качестве ключа строки. Если имя не указано, используйте для каждой строки идентификатор GUID. |Нет |
| azureTableInsertType |Режим для вставки данных в таблицу Azure. Это свойство контролирует, будут ли заменены или объединены значения в существующих строках в выходной таблице с совпадающими ключами секций и строк. <br/><br/>Допустимые значения: **Merge** (по умолчанию) и **Replace**. <br/><br> Этот параметр применяется на уровне строк, а не на уровне таблицы. Ни один из параметров не удаляет строки выходной таблицы, которые не существуют во входной таблице. Чтобы узнать о действии этих параметров (merge и replace), ознакомьтесь со статьями [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) (Вставка или слияние сущностей) и [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) (Вставка или замена сущности). |Нет |
| writeBatchSize |Вставляет данные в таблицу Azure при достижении writeBatchSize или writeBatchTimeout.<br/>Допустимые значения: целое число (количество строк). |Нет (значение по умолчанию — 10 000) |
| writeBatchTimeout |Вставляет данные в таблицу Azure при достижении writeBatchSize или writeBatchTimeout.<br/>Допустимые значения: временной диапазон. Например, 00:20:00 (20 минут). |Нет (значение по умолчанию — 90 секунд, время ожидания клиента хранилища по умолчанию) |

**Пример.**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Чтобы вы могли использовать целевой столбец как azureTablePartitionKeyName, необходимо сопоставить исходный столбец с целевым столбцом с помощью свойства **translator**.

В следующем примере исходный столбец DivisionID сопоставляется с целевым столбцом: DivisionID.

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

DivisionID указывается в качестве ключа раздела.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Сопоставление типов данных для таблиц Azure

При копировании данных в таблицу Azure и из нее используются следующие сопоставления типов данных таблицы Azure с промежуточными типами данных фабрики данных. Дополнительные сведения о том, как действие копирования сопоставляет исходную схему и типы данных для приемника, см. в статье [Сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

При перемещении данных в таблицу Azure или из нее выполняется преобразование типов OData таблиц Azure в тип .NET (и наоборот) на основе указанных ниже [сопоставлений, определенных таблицей Azure](https://msdn.microsoft.com/library/azure/dd179338.aspx).

| Тип данных таблицы Azure | Тип промежуточных данных фабрики данных | Сведения |
|:--- |:--- |:--- |
| Edm.Binary |byte[] |Массив байтов размером до 64 КБ. |
| Edm.Boolean |bool |Значение типа Boolean. |
| Edm.DateTime |DateTime |64-битное значение времени, выраженное в формате UTC. Допустимый диапазон даты и времени начинается в полночь 1 января 1601 года н. э. Англиканское летоисчисление, часовой пояс — UTC. Заканчивается диапазон 31 декабря 9999 года. |
| Edm.Double |double |64-битное значение с плавающей запятой. |
| Edm.Guid |Guid |128-битный идентификатор GUID. |
| Edm.Int32 |Int32 |32-битное целое число. |
| Edm.Int64 |Int64 |64-битное целое число. |
| Edm.String |Строка |Значение в кодировке UTF-16. Размер строкового значения не должен превышать 64 КБ. |

## <a name="lookup-activity-properties"></a>Свойства действия поиска

Чтобы получить сведения о свойствах, проверьте [действие поиска](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Дальнейшие шаги
В таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных.
