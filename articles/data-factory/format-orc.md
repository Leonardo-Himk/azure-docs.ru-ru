---
title: Формат ORC в фабрике данных Azure
description: В этом разделе описывается, как работать с форматом ORC в фабрике данных Azure.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jingwang
ms.openlocfilehash: 9b68d3724c6390fc5d30745924451e27ef9855b3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417731"
---
# <a name="orc-format-in-azure-data-factory"></a>Формат ORC в фабрике данных Azure

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Если вы хотите **проанализировать файлы ORC или записать данные в формат ORC**, следуйте этой статье. 

Формат ORC поддерживается для следующих соединителей: [Amazon S3](connector-amazon-simple-storage-service.md), [большой двоичный объект Azure](connector-azure-blob-storage.md), [Azure Data Lake Storage 1-го поколения](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md), [хранилище файлов Azure](connector-azure-file-storage.md), [Файловая система](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)и [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). В этом разделе содержится список свойств, поддерживаемых набором данных ORC.

| Свойство         | Описание                                                  | Обязательный |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Свойство Type набора данных должно иметь значение **ORC**. | Да      |
| location         | Параметры расположения файлов. Каждый файловый соединитель имеет собственный тип расположения и поддерживаемые свойства в разделе `location`. **См. сведения в статье о соединителе. раздел свойств набора данных >**. | Да      |

Ниже приведен пример набора данных ORC в хранилище BLOB-объектов Azure.

```json
{
    "name": "ORCDataset",
    "properties": {
        "type": "Orc",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            }
        }
    }
}
```

Обратите внимание на следующие моменты.

* Данные сложных типов (STRUCT, MAP, LIST, UNION) не поддерживаются.
* Пробелы в именах столбцов не поддерживаются.
* Для ORC-файлов используется три [параметра сжатия](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB и SNAPPY. Фабрика данных поддерживает чтение данных из ORC-файла в любом из этих форматов. Для чтения данных используется кодек сжатия из метаданных. Однако при записи в ORC-файл фабрика данных по умолчанию выбирает ZLIB. В настоящее время изменить это поведение нельзя.

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых источником и приемником ORC.

### <a name="orc-as-source"></a>ORC в качестве источника

В разделе *** \*источник\* *** действия копирования поддерживаются следующие свойства.

| Свойство      | Описание                                                  | Обязательный |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Свойство Type источника действия копирования должно иметь значение **орксаурце**. | Да      |
| сторесеттингс | Группа свойств для чтения данных из хранилища данных. Каждый файловый соединитель имеет собственные Поддерживаемые параметры чтения в разделе `storeSettings`. Дополнительные **сведения см. в статье о соединителе — > свойства действия копирования**. | Нет       |

### <a name="orc-as-sink"></a>ORC в качестве приемника

В разделе *** \*приемника\* *** действия копирования поддерживаются следующие свойства.

| Свойство      | Описание                                                  | Обязательный |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Свойство Type источника действия копирования должно иметь значение **орксинк**. | Да      |
| сторесеттингс | Группа свойств для записи данных в хранилище данных. Каждый соединитель на основе файлов имеет собственные Поддерживаемые параметры записи в `storeSettings`. Дополнительные **сведения см. в статье о соединителе — > свойства действия копирования**. | Нет       |

## <a name="using-self-hosted-integration-runtime"></a>Использование Integration Runtime с автономным размещением

> [!IMPORTANT]
> Для копирования с помощью автономных Integration Runtime например, между локальными и облачными хранилищами данных, если вы не копируете файлы ORC **как есть**, необходимо установить **64-разрядный пакет JRE 8 (среда выполнения Java) или OpenJDK** и **Microsoft Visual C++ 2010** на своем IR-компьютере. Дополнительные сведения см. в следующем абзаце.

Для копирования, выполняемой на автономной среде IR с ORC File Serialization/десериализацией, ADF находит среду выполнения Java, предварительно проверяя реестр *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* для JRE, если она не найдена, а вторая проверяет системную переменную *`JAVA_HOME`* для OpenJDK.

- **Для использования JRE**: для 64-разрядного IR требуется 64-разрядный JRE. Ее можно найти [здесь](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Чтобы использовать OpenJDK**, она поддерживается с версии IR 3,13. Упакуйте jvm.dll со всеми другими необходимыми сборками OpenJDK на компьютере с локальной IR и соответственно установите системную переменную среды JAVA_HOME.
- **Чтобы установить распространяемый пакет Visual C++ 2010**, распространяемый пакет Visual C++ 2010 не устанавливается с ЛОКАЛЬными инфракрасными установками. Ее можно найти [здесь](https://www.microsoft.com/download/details.aspx?id=14632).

> [!TIP]
> При копировании данных в формат ORC или из него с помощью автономного Integration Runtime и при возникновении ошибки "произошла ошибка при вызове Java, сообщение: **Java. lang. OutOfMemoryError: пространство кучи Java**", можно добавить переменную `_JAVA_OPTIONS` среды на компьютере, где размещена локальная среда IR, чтобы настроить минимальный или максимальный размер кучи для виртуальной машины Java, чтобы предоставить такую копию, а затем повторно запустить конвейер.

![Установка размера кучи виртуальной машины Java на локальной среде выполнения интеграции](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Пример: установите переменную `_JAVA_OPTIONS` со значением `-Xms256m -Xmx16g`. Флаг `Xms` указывает начальный пул выделения памяти для виртуальной машины Java (JVM), а `Xmx` указывает максимальный пул выделения памяти. Это означает, что JVM будет запущена с объемом памяти `Xms` и сможет использовать не более `Xmx` объема памяти. По умолчанию ADF использует min 64 МБ и максимальное значение 1 ГБ.

## <a name="next-steps"></a>Дальнейшие шаги

- [Общие сведения о действии копирования](copy-activity-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)
- [Действие получения метаданных в Фабрике данных Azure](control-flow-get-metadata-activity.md)
