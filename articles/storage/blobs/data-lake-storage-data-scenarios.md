---
title: Сценарии работы с данными с использованием Azure Data Lake Storage 2-ого поколения | Документация Майкрософт
description: Сведения о разных сценариях и средствах для приема, обработки, скачивания и визуализации данных в Data Lake Storage 2-ого поколения (прежнее название — Azure Data Lake Store)
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: b0ebe6cb505fa2a145dd3cbb94398912f2933a4b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77369714"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Использование Azure Data Lake Storage 2-ого поколения для обеспечения соответствия требованиям больших данных

Процесс обработки больших данных состоит из указанных далее четырех ключевых этапов.

> [!div class="checklist"]
> * Прием больших объемов данных в хранилище данных в режиме реального времени или в пакетах
> * Обработка данных
> * Загрузка данных
> * Визуализация данных

В этой статье описываются параметры и средства для каждой фазы обработки.

Полный список служб Azure, которые можно использовать с Azure Data Lake Storage 2-го поколения, см. в статье [интеграция Azure Data Lake Storage со службами Azure](data-lake-storage-integrate-with-azure-services.md) .

## <a name="ingest-the-data-into-data-lake-storage-gen2"></a>Прием данных в Data Lake Storage 2-го поколения

В этом разделе описываются различные источники данных и способы поступления этих данных в учетную запись Azure Data Lake Storage 2-го поколения.

![Прием данных в Azure Data Lake Storage 2-го поколения](./media/data-lake-storage-data-scenarios/ingest-data.png "Прием данных в Azure Data Lake Storage 2-го поколения")

### <a name="ad-hoc-data"></a>Специальные данные

Это небольшие наборы данных, которые используются для создания прототипов приложений для работы с большими данными. В зависимости от источника данных применяются разные способы приема специальных данных. 

Ниже приведен список инструментов, которые можно использовать для приема специальных данных.

| источника данных | Средство для приема |
| --- | --- |
| Локальный компьютер |[Azure PowerShell](data-lake-storage-directory-file-acl-powershell.md)<br><br>[Azure CLI](data-lake-storage-directory-file-acl-cli.md)<br><br>[Обозреватель службы хранилища](https://azure.microsoft.com/features/storage-explorer/)<br><br>[Средство AzCopy ](../common/storage-use-azcopy-v10.md)|
| Большой двоичный объект хранилища Azure |[Фабрика данных Azure](../../data-factory/connector-azure-data-lake-store.md).<br><br>[Средство AzCopy ](../common/storage-use-azcopy-v10.md)<br><br>[DistCp, запущенный на кластере HDInsight](data-lake-storage-use-distcp.md)|

### <a name="streamed-data"></a>Потоковые данные

Представляет данные, которые могут быть созданы различными источниками, такими как приложения, устройства, датчики и т. д. Эти данные можно принимать в Data Lake Storage 2-го поколения с помощью различных средств. Как правило, эти средства собирают и обрабатывают данные на основе событий в режиме реального времени, а затем записывают события в пакетном режиме в Data Lake Storage 2-го поколения для последующей обработки.

Ниже приведен список инструментов, которые можно использовать для приема потоковых данных.

|Инструмент | Руководство |
|---|--|
|Azure Stream Analytics|[Краткое руководство по созданию задания Stream Analytics с помощью портала Azure](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Исходящий трафик в Azure Data Lake Gen2](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|Azure HDInsight Storm | [Запись данных в Apache Hadoop HDFS из Apache Storm в HDInsight](https://docs.microsoft.com/azure/hdinsight/storm/apache-storm-write-data-lake-store) |

### <a name="relational-data"></a>Реляционные данные

Можно также извлекать данные из реляционных баз данных. В течение определенного периода времени реляционные базы данных собирают огромные объемы данных, которые после обработки с помощью конвейера больших данных могут предоставлять ценные сведения. Для перемещения таких данных в Data Lake Storage 2-го поколения можно использовать следующие средства.

Ниже приведен список инструментов, которые можно использовать для приема реляционных данных.

|Инструмент | Руководство |
|---|--|
|Фабрика данных Azure | [Действие копирования в Фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |

### <a name="web-server-log-data-upload-using-custom-applications"></a>Данные журналов веб-сервера (отправка с помощью настраиваемых приложений)

Этот тип набора данных вызывается специально, так как анализ данных журналов веб-сервера часто используется в приложениях по работе с большими данными, и для его выполнения требуется отправка больших объемов файлов журналов в Data Lake Storage 2-го поколения. Для отправки таких данных воспользуйтесь следующими средствами или напишите собственные сценарии или приложения.

Ниже приведен список инструментов, которые можно использовать для приема данных журнала веб-сервера.

|Инструмент | Руководство |
|---|--|
|Фабрика данных Azure | [Действие копирования в Фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)  |
|Azure CLI|[Azure CLI](data-lake-storage-directory-file-acl-cli.md)|
|Azure PowerShell|[Azure PowerShell](data-lake-storage-directory-file-acl-powershell.md)|

Отличным способом отправки данных журналов веб-сервера и других типов данных (например данных общественных мнений) является использование собственных написанных сценариев или приложений, поскольку вы можете включить компонент отправки данных в состав более масштабного приложения по работе с большими объемами данных. В одних случаях этот код может иметь форму сценария или простой программы командной строки. В других случаях код может использоваться для интеграции обработки больших данных в бизнес-приложение или решение.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Данные, связанные с кластерами HDInsight Azure

Большинство типов кластеров HDInsight (Hadoop, HBase, Storm) поддерживают Data Lake Storage 2-го поколения в качестве репозитория хранения данных. Кластеры HDInsight обращаются к данным из BLOB-объектов хранилища Azure (WASB). Для повышения производительности можно скопировать данные из WASB в учетную запись Data Lake Storage 2-го поколения, связанную с кластером. Для копирования данных можно использовать указанные далее средства.

Ниже приведен список инструментов, которые можно использовать для приема данных, связанных с кластерами HDInsight.

|Инструмент | Руководство |
|---|--|
|Apache DistCp | [Использование средства DistCp для копирования данных между Azure Storage Blob и Azure Data Lake Storage 2-го поколения](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|Инструмент AzCopy | [Передача данных с помощью AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10) |
|Фабрика данных Azure | [Копирование данных в Azure Data Lake Storage 2-го поколения или из них с помощью фабрики данных Azure](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2) |

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Данные, хранящиеся в локальных кластерах Hadoop или кластерах Hadoop в IaaS

Большие объемы данных могут храниться в кластерах Hadoop, размещенных локально на компьютерах, использующих HDFS. Кластеры Hadoop могут быть развернуты локально или работать в кластере IaaS в Azure. К копированию таких данных в Azure Data Lake Storage 2-го поколения могут предъявляться требования, в зависимости от того, является ли эта операция одноразовой или повторяющейся. Существуют различные возможности выполнить их. Ниже приведен список альтернативных вариантов и связанные с ними компромиссы.

| Метод | Сведения | Преимущества | Рекомендации |
| --- | --- | --- | --- |
| Использование Фабрики данных Azure (ADF) для копирования данных напрямую из кластеров Hadoop в Azure Data Lake Storage 2-го поколения |[ADF поддерживает HDFS в качестве источника данных.](../../data-factory/connector-hdfs.md) |ADF реализована готовая поддержка HDFS, а также первоклассные инструменты комплексного управления и мониторинга. |Требуется развернуть шлюз управления данными в локальном кластере или кластере IaaS. |
| Использование Distcp для копирования данных из Hadoop в службу хранилища Azure. Затем копирование данных из службы хранилища Azure в Data Lake Storage 2-го поколения с помощью соответствующего механизма. |Скопировать данные из службы хранилища Azure в Data Lake Storage 2-го поколения можно с помощью следующих элементов. <ul><li>[Фабрика данных Azure](../../data-factory/copy-activity-overview.md).</li><li>[Средство AzCopy ](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp, запущенный в кластерах HDInsight](data-lake-storage-use-distcp.md)</li></ul> |Можно использовать инструменты с открытым кодом. |Многоэтапный процесс с использованием нескольких технологий. |

### <a name="really-large-datasets"></a>Очень большие наборы данных

Для отправки наборов данных размером в несколько терабайт использование описанных выше методов иногда может быть медленным и затратным процессом. В таких случаях вы можете использовать Azure ExpressRoute.  

Azure ExpressRoute позволяет создавать закрытые подключения между центрами обработки данных Azure и вашей локальной инфраструктурой. Это надежный вариант для передачи больших объемов данных. Дополнительные сведения см. в [документации по Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="process-the-data"></a>Обработка данных

Данные, доступные в Data Lake Storage 2-го поколения, можно проанализировать с помощью поддерживаемых приложений для работы с большими данными. 

![Анализ данных в Data Lake Storage 2-го поколения](./media/data-lake-storage-data-scenarios/analyze-data.png "Анализ данных в Data Lake Storage 2-го поколения")

Ниже приведен список инструментов, которые можно использовать для выполнения заданий анализа данных, хранящихся в Azure Data Lake Storage 2-го поколения.

|Инструмент | Руководство |
|---|--|
|Azure HDInsight | [Использование Azure Data Lake Storage 2-го поколения с кластерами Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Databricks | [Azure Data Lake Storage 2-го поколения](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)<br><br>[Краткое руководство. анализ данных в Azure Data Lake Storage 2-го поколения с помощью Azure Databricks](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br><br>[Руководство по Извлечение, преобразование и загрузка данных с помощью Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="visualize-the-data"></a>Визуализация данных

Соединитель Power BI используется для создания визуальных представлений данных, хранящихся в Data Lake Storage 2-го поколения. См. статью [анализ данных в Azure Data Lake Storage 2-го поколения с помощью Power BI](https://docs.microsoft.com/power-query/connectors/datalakestorage).

## <a name="download-the-data"></a>Скачивание данных

Возможно, вам потребуется скачать или переместить данные из Azure Data Lake Storage 2-го поколения в сценариях указанных далее.

* Перемещение данных в другие репозитории для взаимодействия с существующими конвейерами обработки данных. Например, можно переместить данные из Data Lake Storage 2-го поколения в базу данных SQL Azure или на локальный сервер SQL Server.

* Загрузка данных на локальный компьютер для обработки в средах IDE при создании прототипов приложений.

![Исходящие данные из Data Lake Storage 2-го поколения](./media/data-lake-storage-data-scenarios/egress-data.png "Исходящие данные из Data Lake Storage 2-го поколения")

Ниже приведен список инструментов, которые можно использовать для загрузки данных из Data Lake Storage 2-го поколения.

|Инструмент | Руководство |
|---|--|
|Фабрика данных Azure | [Действие копирования в Фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |
|Apache DistCp | [Использование средства DistCp для копирования данных между Azure Storage Blob и Azure Data Lake Storage 2-го поколения](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|Обозреватель службы хранилища Azure|[Использование Обозреватель службы хранилища Azure для управления каталогами, файлами и списками ACL в Azure Data Lake Storage 2-го поколения](data-lake-storage-explorer.md)|
|Инструмент AzCopy|[Перенос данных с помощью AzCopy и хранилища BLOB-объектов](../common/storage-use-azcopy-blobs.md)|
