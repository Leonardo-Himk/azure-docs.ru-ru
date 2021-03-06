---
title: Двоичный формат в фабрике данных Azure
description: В этом разделе описывается, как работать с двоичным форматом в фабрике данных Azure.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: jingwang
ms.openlocfilehash: 4560560b3677030a66e277e96eb552d39f5c82c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416328"
---
# <a name="binary-format-in-azure-data-factory"></a>Двоичный формат в фабрике данных Azure
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Двоичный формат поддерживается для следующих соединителей: [Amazon S3](connector-amazon-simple-storage-service.md), [большой двоичный объект Azure](connector-azure-blob-storage.md), [Azure Data Lake Storage 1-го поколения](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md), [хранилище файлов Azure](connector-azure-file-storage.md), [Файловая система](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)и [SFTP](connector-sftp.md).

Двоичный набор данных можно использовать в действии [копирования](copy-activity-overview.md), [действиях с метаданными](control-flow-get-metadata-activity.md)или [действиях удаления](delete-activity.md). При использовании двоичного набора данных ADF не анализирует содержимое файла, но обрабатывает его как есть. 

>[!NOTE]
>При использовании двоичного набора данных в действии копирования можно копировать только из двоичного набора данных в двоичный набор данных.

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). В этом разделе содержится список свойств, поддерживаемых двоичным набором данных.

| Свойство         | Описание                                                  | Обязательный |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Свойство Type набора данных должно иметь значение **binary**. | Да      |
| location         | Параметры расположения файлов. Каждый файловый соединитель имеет собственный тип расположения и поддерживаемые свойства в разделе `location`. **См. сведения в статье о соединителе. раздел свойств набора данных >**. | Да      |
| compression | Группа свойств для настройки сжатия файла. Настройте этот раздел, если требуется сжатие и распаковка во время выполнения действия. | Нет |
| type | Кодек сжатия, используемый для чтения и записи двоичных файлов. <br>Допустимые значения: **bzip2**, **gzip**, **Deflate**, **ZipDeflate**. для использования при сохранении файла.<br>Примечание. при использовании действия копирования для распаковки файлов ZipDeflate и записи в хранилище данных приемника файлов файлы будут извлечены в папку: `<path specified in dataset>/<folder named as source zip file>/`. | Нет       |
| level | Коэффициент сжатия. Применяется, когда набор данных используется в приемнике действия копирования.<br>Допустимые значения: **оптимальный** или **самый быстрый**.<br>- **Самый быстрый:** Операция сжатия должна завершиться как можно быстрее, даже если итоговый файл не сжимается оптимально.<br>- **Оптимально**. операция сжатия должна быть оптимально сжата, даже если выполнение операции займет больше времени. Дополнительные сведения см. в разделе [Уровень сжатия](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx). | Нет       |

Ниже приведен пример двоичного набора данных в хранилище BLOB-объектов Azure.

```json
{
    "name": "BinaryDataset",
    "properties": {
        "type": "Binary",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "ZipDeflate"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых двоичным источником и приемником.

>[!NOTE]
>При использовании двоичного набора данных в действии копирования можно копировать только из двоичного набора данных в двоичный набор данных.

### <a name="binary-as-source"></a>Двоичный файл в качестве источника

В разделе *** \*источник\* *** действия копирования поддерживаются следующие свойства.

| Свойство      | Описание                                                  | Обязательный |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Свойство Type источника действия копирования должно иметь значение **бинарисаурце**. | Да      |
| сторесеттингс | Группа свойств для чтения данных из хранилища данных. Каждый файловый соединитель имеет собственные Поддерживаемые параметры чтения в разделе `storeSettings`. Дополнительные **сведения см. в статье о соединителе — > свойства действия копирования**. | Нет       |

### <a name="binary-as-sink"></a>Двоичный в качестве приемника

В разделе *** \*приемника\* *** действия копирования поддерживаются следующие свойства.

| Свойство      | Описание                                                  | Обязательный |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Свойство Type источника действия копирования должно иметь значение **бинарисинк**. | Да      |
| сторесеттингс | Группа свойств для записи данных в хранилище данных. Каждый соединитель на основе файлов имеет собственные Поддерживаемые параметры записи в `storeSettings`. Дополнительные **сведения см. в статье о соединителе — > свойства действия копирования**. | Нет       |

## <a name="next-steps"></a>Дальнейшие шаги

- [Общие сведения о действии копирования](copy-activity-overview.md)
- [Действие получения метаданных в Фабрике данных Azure](control-flow-get-metadata-activity.md)
- [Удалить действие](delete-activity.md)
