---
title: Общие сведения об Apache HBase в Azure HDInsight
description: Введение в Apache HBase в HDInsight — базу данных NoSQL на основе Hadoop. Изучите варианты использования и сравните HBase с другими кластерами Hadoop.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017,seoapr2020
ms.date: 04/20/2020
ms.openlocfilehash: afbf9aff09999a34a84d55634a868250fbb6d1ff
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82188966"
---
# <a name="what-is-apache-hbase-in-azure-hdinsight"></a>Общие сведения об Apache HBase в Azure HDInsight

[Apache HBase](https://hbase.apache.org/) — это база данных NoSQL с открытым кодом, созданная на основе Apache Hadoop по типу [Google BigTable](https://cloud.google.com/bigtable/). HBase обеспечивает прямой доступ и строгую согласованность для больших объемов данных в безсхемной базе данных. База данных упорядочивается по семействам столбцов.

С точки зрения пользователя HBase напоминает базу данных. Данные хранятся в строках и столбцах таблицы, данные в строке группируются по семейству столбцов. HBase — это безсхемная база данных. Столбцы и типы данных не обязательно определять перед использованием. Открытый код линейно масштабируется, чтобы обрабатывать петабайты данных на тысячах узлов. Он может полагаться на избыточность данных, пакетную обработку и другие возможности, предоставляемые распределенными приложениями в среде Hadoop.

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Как база данных Apache HBase реализована в Azure HDInsight?

HDInsight HBase предлагается в форме управляемого кластера, интегрированного в среду Azure. Кластеры настроены на хранение данных непосредственно в [службе хранилища Azure](./../hdinsight-hadoop-use-blob-storage.md), что обеспечивает небольшую задержку и повышенную гибкость в выборе соотношения производительности и стоимости. Это свойство позволяет клиентам создавать интерактивные веб-сайты, работающие с большими наборами данных, а также создавать службы для хранения данных датчиков и телеметрии от миллионов конечных точек. И, наконец, анализировать эти данные с помощью заданий Hadoop. HBase и Hadoop — это хорошие отправные точки для создания проекта для работы с большими данными в Azure. Благодаря этим службам приложения могут в реальном времени работать с большими наборами данных.

Реализация HDInsight использует архитектуру с горизонтальным увеличением масштаба HBase, чтобы обеспечить автоматическое сегментирование таблиц, а также строгую согласованность для операций чтения и записи и автоматический переход на другой ресурс. Производительность повышается за счет кэширования операций чтения в памяти и потоковой записи с высокой пропускной способностью Также для HDInsight HBase доступна подготовка виртуальных сетей. Кластер HBase можно создать внутри виртуальной сети. Дополнительные сведения см. в статье [Create HBase clusters on HDInsight in Azure Virtual Network](./apache-hbase-provision-vnet.md) (Создание кластеров HBase в виртуальной сети Azure).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Как происходит управление данными в HDInsight HBase?

Управление данными в HBase может осуществляться с помощью команд `create`, `get`, `put` и `scan` из оболочки HBase. Данные записываются в базу данных с использованием команды `put` и считываются с помощью команды `get`. Команда `scan` используется для получения данных из нескольких строк таблицы. Данными также можно управлять с использованием интерфейса HBase API для C#, для которого имеется клиентская библиотека, работающая поверх HBase REST API. Запросы к базе данных HBase также могут осуществляться с помощью [Apache Hive](https://hive.apache.org/). Начальные сведения об этих моделях программирования приведены в статье [Начало работы с Apache HBase с Apache Hadoop в HDInsight](./apache-hbase-tutorial-get-started-linux.md). Также доступны сопроцессоры, позволяющие обрабатывать данные в узлах, на которых размещена база данных.

> [!NOTE]  
> Thrift не поддерживается HBase в HDInsight.

## <a name="use-cases-for-apache-hbase"></a>варианты использования Apache HBase

Классическим вариантом использования (для которого, собственно и была создана технология BigTable и HBase как ее расширение) является веб-поиск. Поисковые системы строят индексы, которые сопоставлены терминам, содержащимся на веб-страницах. Но есть много других вариантов использования, для которых подходит HBase; некоторые из них перечислены в этом разделе.

|Сценарий |Описание |
|---|---|
|Хранилище ключ-значение|HBase можно использовать как хранилище пар "ключ — значение", и она подходит для управления системами обмена сообщениями. Facebook использует HBase для системы обмена сообщениями, она идеально подходит для хранения и управления интернет-коммуникациями. WebTable использует HBase для поиска и управления таблицами, извлеченными из веб-страниц.|
|Данные от датчиков|HBase удобно использовать для записи данных, накопленных при сборе из различных источников. К ним относятся аналитические данных из социальных сетей и временные ряды. Кроме того, HBase используется для обновления интерактивных панелей мониторинга и счетчиков, а также управления системами ведения журналов аудита. Примерами могут служить терминал для биржевой торговли Bloomberg и база данных Open Time Series (OpenTSDB), которая сохраняет и предоставляет доступ к метрикам работоспособности серверных систем.|
|Запросы в режиме реального времени|[Apache Phoenix](https://phoenix.apache.org/) — это система запросов SQL для Apache HBase. Доступ к системе осуществляется через драйвер JDBC, и она позволяет создавать запросы и управлять таблицами HBase с использованием SQL.|
|HBase как платформа|Поверх HBase могут выполняться приложения, используя ее как хранилище данных. Например, это используется в Phoenix, OpenTSDB, `Kiji` и Titan. Также возможна интеграция приложений с HBase. Примеры приведены ниже. [Apache Hive](https://hive.apache.org/), Apache Pig, [Solr](https://lucene.apache.org/solr/), Apache Storm, Apache Flume, [Apache Impala](https://impala.apache.org/), Apache Spark, `Ganglia` и Apache Drill.|

## <a name="next-steps"></a>Дальнейшие действия

* [Начало работы с примером Apache HBase в HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Создание кластеров HBase в виртуальной сети Azure](./apache-hbase-provision-vnet.md).
* [Настройка репликации кластера HBase в виртуальных сетях Azure](apache-hbase-replication.md)
