---
title: Копирование данных из DB2 с помощью фабрики данных Azure
description: Узнайте, как копировать данные из DB2 в поддерживаемые хранилища данных-приемники с помощью действия копирования в конвейере фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: jingwang
ms.openlocfilehash: 9f705a0a56975860cf07d8a9b09de9999a923501
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891442"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Копирование данных из DB2 с помощью фабрики данных Azure
> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-onprem-db2-connector.md)
> * [Текущая версия](connector-db2.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные из базы данных DB2. Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот соединитель базы данных DB2 поддерживается для следующих действий:

- [Действие копирования](copy-activity-overview.md) с [поддерживаемой матрицей источника и приемника](copy-activity-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)

Данные из базы данных DB2 можно скопировать в любое хранилище данных, поддерживаемое в качестве приемника. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

В частности, этот соединитель DB2 поддерживает приведенные ниже платформы и версии IBM DB2 с архитектурой распределенной реляционной базы данных (DRDA) SQL Access Manager (SQLAM) версии 9, 10 и 11:

* IBM DB2 для z/OS 12,1
* IBM DB2 для z/OS версии 11.1
* IBM DB2 для z/OS версии 10.1
* IBM DB2 для i версии 7.3
* IBM DB2 для i версии 7.2
* IBM DB2 для i версии 7.1
* IBM DB2 для LUW версии 11
* IBM DB2 для LUW версии 10.5
* IBM DB2 для LUW версии 10.1

>[!TIP]
>Соединитель DB2 строится на основе поставщик OLE DB для DB2 (Майкрософт). Сведения об устранении ошибок соединителя DB2 см. в статье [коды ошибок поставщика данных](https://docs.microsoft.com/host-integration-server/db2oledbv/data-provider-error-codes#drda-protocol-errors).

## <a name="prerequisites"></a>Предварительные условия

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Среда выполнения интеграции предоставляет встроенный драйвер DB2, поэтому при копировании данных из DB2 вам не потребуется устанавливать драйвер вручную.

## <a name="getting-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах JSON, которые используются для определения сущностей фабрики данных, относящихся к соединителю DB2.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы DB2 поддерживаются следующие свойства:

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Для свойства типа необходимо задать значение **Db2** | Да |
| connectionString | Укажите сведения, необходимые для подключения к экземпляру DB2.<br/> Вы можете также поместить пароль в Azure Key Vault и извлечь конфигурацию `password` из строки подключения. Ознакомьтесь с приведенными ниже примерами и подробными сведениями в статье [Хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md). | Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Дополнительные сведения см. в разделе " [Предварительные требования](#prerequisites) ". Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |нет |

Типичные свойства в строке подключения:

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| server |Имя сервера DB2. Вы можете указать номер порта следом за именем сервера, разделив их двоеточием, например `server:port`. |Да |
| База данных |Имя базы данных DB2. |Да |
| authenticationType |Тип проверки подлинности, используемый для подключения к базе данных DB2.<br/>Допустимое значение: **Базовый**. |Да |
| username |Укажите имя пользователя для подключения к базе данных DB2. |Да |
| password |Введите пароль для учетной записи пользователя, указанной для выбранного имени пользователя. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). |Да |
| паккажеколлектион | Укажите в разделе место, где необходимые пакеты создаются автозаменой службой ADF при запросе к базе данных. | нет |
| certificateCommonName | При использовании SSL (SSL) или шифрования TLS необходимо ввести значение для общего имени сертификата. | нет |

> [!TIP]
> Если появляется сообщение об ошибке `The package corresponding to an SQL statement execution request was not found. SQLSTATE=51002 SQLCODE=-805`, причина — необходимый пакет для пользователя не создается. По умолчанию ADF попытается создать пакет в коллекции с именем в качестве пользователя, который использовался для подключения к DB2. Укажите свойство коллекции пакетов, чтобы указать, где ADF должен создавать необходимые пакеты при запросе к базе данных.

**Пример.**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "connectionString": "server=<server:port>; database=<database>; authenticationType=Basic;username=<username>; password=<password>; packageCollection=<packagecollection>;certificateCommonName=<certname>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```
**Пример: хранение пароля в Azure Key Vault**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "connectionString": "server=<server:port>; database=<database>; authenticationType=Basic;username=<username>; packageCollection=<packagecollection>;certificateCommonName=<certname>;",
            "password": { 
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

Если вы использовали связанную службу DB2 со следующими полезными данными, она по-прежнему поддерживается "как есть", хотя вы предлагаете использовать новую пересылку.

**Предыдущие полезные данные:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername:port>",
            "database": "<dbname>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md) . Этот раздел содержит список свойств, поддерживаемых набором данных DB2.

Для копирования данных из DB2 поддерживаются следующие свойства:

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство Type набора данных должно иметь значение **Db2Table** . | Да |
| схема | Имя схемы. |Нет (если свойство query указано в источнике действия)  |
| table | Имя таблицы. |Нет (если свойство query указано в источнике действия)  |
| tableName | Имя таблицы со схемой. Это свойство поддерживается для обеспечения обратной совместимости. Используйте `schema` и `table` для новой рабочей нагрузки. | Нет (если свойство query указано в источнике действия) |

**Пример**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "Db2Table",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Если вы использовали `RelationalTable` типизированный набор данных, он по-прежнему поддерживается "как есть", хотя вы можете использовать новый объект, который будет использоваться в дальнейшем.

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником DB2.

### <a name="db2-as-source"></a>DB2 в качестве источника

Чтобы скопировать данные из DB2, в разделе **источник** действия копирования поддерживаются следующие свойства.

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство Type источника действия копирования должно иметь значение **Db2Source** . | Да |
| query | Используйте пользовательский SQL-запрос для чтения данных. Например: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Нет (если для набора данных задано свойство tableName) |

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromDB2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<DB2 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "Db2Source",
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Если вы использовали `RelationalSource` типизированный источник, он по-прежнему поддерживается как есть, хотя вы предлагаете использовать новый.

## <a name="data-type-mapping-for-db2"></a>Сопоставление типов данных для DB2

При копировании данных из DB2 используются следующие сопоставления из типов данных DB2 с промежуточными типами данных фабрики данных Azure. Дополнительные сведения о том, как действие копирования сопоставляет исходную схему и типы данных для приемника, см. в статье [Сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

| Тип базы данных DB2 | Тип промежуточных данных фабрики данных |
|:--- |:--- |
| BigInt |Int64 |
| Двоичные данные |Byte[] |
| BLOB-объект |Byte[] |
| Char |Строка |
| Clob |Строка |
| Дата |Datetime |
| DB2DynArray |Строка |
| DbClob |Строка |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| Double |Double |
| Float |Double |
| GRAPHIC |Строка |
| Целое число |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |Строка |
| LongVarGraphic |Строка |
| Числовой |Decimal |
| Real |Single |
| SmallInt |Int16 |
| Time |TimeSpan |
| Отметка времени |Дата и время |
| VarBinary |Byte[] |
| VarChar |Строка |
| VarGraphic |Строка |
| Xml |Byte[] |

## <a name="lookup-activity-properties"></a>Свойства действия поиска

Чтобы получить сведения о свойствах, проверьте [действие поиска](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Дальнейшие действия
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
