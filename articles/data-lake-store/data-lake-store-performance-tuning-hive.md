---
title: Настройка производительности — Hive на Azure Data Lake Storage 1-го поколения
description: Рекомендации по настройке производительности для Hive в HdInsight и Azure Data Lake Storage 1-го поколения.
author: stewu
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 2e44332ddab9387c05a45d15101ccd2bdec3ada4
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690525"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen1"></a>Рекомендации по настройке производительности для Hive в HDInsight и Azure Data Lake Storage 1-го поколения

По умолчанию устанавливаются параметры, которые должны обеспечить оптимальную производительность для самых разных сценариев использования.  Но для запросов, предполагающих интенсивное выполнение операций ввода-вывода, производительность Hive в Azure Data Lake Storage 1-го поколения можно повысить, изменив некоторые настройки.  

## <a name="prerequisites"></a>Предварительные условия

* **Подписка Azure**. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Учетная запись Data Lake Storage 1-го поколения**. Инструкции по ее созданию см. в статье [Приступая к работе с Azure Data Lake Storage 1-го поколения](data-lake-store-get-started-portal.md)
* **Кластер Azure HDInsight** с доступом к учетной записи Data Lake Storage 1-го поколения. Дополнительные сведения см. в статье [Создание кластеров HDInsight, использующих Data Lake Store, с помощью портала Azure](data-lake-store-hdinsight-hadoop-use-portal.md). Убедитесь, что вы включили удаленный рабочий стол для кластера.
* **Запустите Hive в HDInsight**.  Дополнительные сведения о выполнении заданий Hive в HDInsight см. в статье [Обзор Apache Hive и HiveQL в Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive).
* **Рекомендации по настройке производительности для Data Lake Storage 1-го поколения**.  Общие понятия производительности см. в разделе [Data Lake Storage 1-го поколения рекомендации по настройке производительности](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance) .

## <a name="parameters"></a>Параметры

Ниже приведены наиболее важные параметры, которые помогут повысить производительность Azure Data Lake Storage 1-го поколения:

* **hive.tez.container.size** — объем памяти, используемой для каждой задачи;

* **tez.grouping.min-size** — минимальный размер каждого модуля сопоставления;

* **tez.grouping.max-size** — максимальный размер каждого модуля сопоставления;

* **hive.exec.reducer.bytes.per.reducer** — размер каждого модуля уменьшения.

**hive.tez.container.size** — размер контейнера, который определяет объем доступной памяти для каждого задания.  Это главный параметр для управления параллелизмом в Hive.  

**tez.grouping.min-size** — этот параметр позволяет задать минимальный размер каждого модуля сопоставления.  Если число модулей сопоставления, которые выбирает Tez, окажется меньше, чем значение этого параметра, Tez будет использовать заданное здесь значение.

**tez.grouping.max-size** — этот параметр позволяет задать максимальный размер каждого модуля сопоставления.  Если число модулей сопоставления, которые выбирает Tez, окажется больше, чем значение этого параметра, Tez будет использовать заданное здесь значение.

**hive.exec.reducer.bytes.per.reducer** — этот параметр задает размер каждого модуля уменьшения.  По умолчанию используется размер 256 МБ.  

## <a name="guidance"></a>Руководство

**Задайте hive.exec.reducer.bytes.per.reducer** — значение по умолчанию хорошо работает для несжатых данных.  Если используются сжатые данные, попробуйте сократить размер модуля уменьшения редукции.  

**Задайте hive.tez.container.size** — объем памяти для каждого узла, который определяется параметром yarn.nodemanager.resource.memory-mb (должен быть правильно определен для кластера HDI по умолчанию).  Дополнительные сведения о настройке памяти в YARN см. в этой [записи блога](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

Для интенсивных нагрузок ввода-вывода будет полезным увеличить параллелизм, снижая размер контейнера Tez. Это предоставит пользователю дополнительные контейнеры, то есть увеличит параллелизм.  Но некоторые запросы Hive требуют значительного объема памяти (например, MapJoin).  Если задача не получит достаточного объема памяти, во время выполнения возникнет соответствующее исключение.  Если вы заметите такие исключения, увеличьте объем памяти.   

Число одновременно выполняемых задач для параллелизма ограничивается общим объемом памяти YARN.  Число контейнеров YARN определяет, сколько параллельных задач можно запустить.  Чтобы узнать, сколько памяти YARN доступно для каждого узла, зайдите в Ambari.  Перейдите по адресу YARN и просмотрите вкладку configs (конфигурации).  В этом окне отображается память YARN.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
При использовании Azure Data Lake Storage 1-го поколения повышение производительности достигается за счет максимально возможного уровня параллелизма.  Tez автоматически вычисляет число задач, которые нужно создать, поэтому нет необходимости настраивать этот параметр.   

## <a name="example-calculation"></a>Пример вычисления

Предположим, что у вас есть кластер D14 с 8 узлами.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Ограничения

**Регулирование Data Lake Storage 1-го поколения** 

Если вы достигнете пределов пропускной способности, предоставленной Data Lake Storage 1-го поколения, вы увидите сбои задач. Это можно заметить, отслеживая ошибки регулирования в журналах задач.  Можно уменьшить параллелизм, увеличив размер контейнера Tez.  Если для обработки задания требуется больший параллелизм, свяжитесь с нами.

Чтобы проверить, применяется ли для вас регулирование, включите ведение журнала отладки на стороне клиента. Вот как это сделать.

1. Добавьте следующее свойство в свойства log4j в конфигурации Hive. Это можно сделать из представления Ambari: log4j. Logger. com. Microsoft. Azure. Data Lake. Store = DEBUG перезапустите все узлы и службы, чтобы конфигурация вступила в силу.

2. Если регулирование выполняется, вы увидите код ошибки HTTP 429 в файле журнала Hive. Файл журнала находится в каталоге /tmp/&lt;имя_пользователя&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Дополнительные сведения о настройке Hive

Ниже приведены некоторые блоги, которые помогут в настройке производительности запросов Hive.
* [Оптимизация запросов Hive для Hadoop в HDInsight](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Troubleshooting Hive query performance](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) (Устранение неполадок с производительностью запросов Hive)
* [Примите участие в дискуссии об оптимизации Hive на HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
