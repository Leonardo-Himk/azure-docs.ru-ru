---
title: Копирование данных из Marketo с помощью фабрики данных Azure (предварительная версия)
description: Узнайте, как копировать данные из Marketo на поддерживаемые приемники хранилища данных с помощью действия копирования в конвейере фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: efb450f4da58c73c134d9f6b6aad6193f786912d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81415003"
---
# <a name="copy-data-from-marketo-using-azure-data-factory-preview"></a>Копирование данных из Marketo с помощью фабрики данных Azure (предварительная версия)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные из Marketo. Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

> [!IMPORTANT]
> Сейчас этот соединитель доступен в режиме предварительной версии. Попробуйте поработать с ним и оставьте свой отзыв. Если вы хотите использовать в своем решении зависимость от соединителей в предварительной версии, обратитесь в службу [поддержки Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот соединитель Marketo поддерживается для следующих действий:

- [Действие копирования](copy-activity-overview.md) с [поддерживаемой матрицей источника и приемника](copy-activity-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)

Данные из Marketo можно скопировать в любое поддерживаемое хранилище данных, используемое в качестве приемника. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

Фабрика данных Azure имеет встроенный драйвер для настройки подключения. Поэтому с использованием этого соединителя вам не нужно устанавливать драйверы вручную.

>[!NOTE]
>Соединитель Marketo создан на основе REST API Marketo. Имейте в виду, что на стороне службы у Marketo есть [ограничение на число одновременных запросов](https://developers.marketo.com/rest-api/). Если вы столкнетесь с ошибками наподобие "Ошибка при попытке использовать REST API: максимальный предел скорости — 100 превышен в течение 20 секунд (606)" или "Ошибка при попытке использовать REST API: достигнуто ограничение на параллельный доступ — 10 (615)", попробуйте сократить количество одновременных операций копирования, чтобы уменьшить количество запросов к службе.

## <a name="getting-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, используемых для определения сущностей фабрики данных, относящихся к соединителю Marketo.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Marketo поддерживаются следующие свойства:

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **Marketo** | Да |
| endpoint | Конечная точка сервера Marketo. (т. е. 123-ABC-321.mktorest.com)  | Да |
| clientid | Идентификатор клиента службы Marketo.  | Да |
| clientSecret | Секрет клиента службы Marketo. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Да |
| useEncryptedEndpoints | Указывает, шифруются ли конечные точки источника данных с помощью протокола HTTPS. Значение по умолчанию — true.  | Нет |
| useHostVerification | Указывает, должно ли имя узла в сертификате сервера совпадать с именем узла сервера при подключении по протоколу TLS. Значение по умолчанию — true.  | Нет |
| usePeerVerification | Указывает, следует ли проверять удостоверение сервера при подключении по протоколу TLS. Значение по умолчанию — true.  | Нет |

**Пример.**

```json
{
    "name": "MarketoLinkedService",
    "properties": {
        "type": "Marketo",
        "typeProperties": {
            "endpoint" : "123-ABC-321.mktorest.com",
            "clientId" : "<clientId>",
            "clientSecret": {
                "type": "SecureString",
                "value": "<clientSecret>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md) . В этом разделе содержится список свойств, поддерживаемых набором данных Marketo.

Чтобы скопировать данные из Marketo, установите свойство типа набора данных **MarketoObject**. Поддерживаются следующие свойства:

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Свойство Type набора данных должно иметь значение **маркетубжект** . | Да |
| tableName | Имя таблицы. | Нет (если свойство query указано в источнике действия) |

**Пример**

```json
{
    "name": "MarketoDataset",
    "properties": {
        "type": "MarketoObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Marketo linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых источником Marketo.

### <a name="marketo-as-source"></a>Marketo в качестве источника

Чтобы копировать данные из Marketo, установите тип источника **MarketoSource** в действии копирования. В разделе **источник** действия копирования поддерживаются следующие свойства.

| Свойство | Описание | Обязательный |
|:--- |:--- |:--- |
| type | Свойство type источника действия копирования должно иметь значение **MarketoSource** | Да |
| query | Используйте пользовательский SQL-запрос для чтения данных. Например: `"SELECT * FROM Activitiy_Types"`. | Нет (если для набора данных задано свойство tableName) |

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromMarketo",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Marketo input dataset name>",
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
                "type": "MarketoSource",
                "query": "SELECT top 1000 * FROM Activitiy_Types"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Свойства действия поиска

Чтобы получить сведения о свойствах, проверьте [действие поиска](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Дальнейшие шаги
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
