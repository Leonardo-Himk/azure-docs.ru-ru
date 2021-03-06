---
title: Ядра для записной книжки Jupyter в кластерах Spark в Azure HDInsight
description: Сведения о ядрах PySpark, PySpark3 и Spark для записной книжки Jupyter, доступной с кластерами Spark в Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017,seoapr2020
ms.date: 04/24/2020
ms.openlocfilehash: f7f460b01674359847427296e4526fc5771658f0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82191963"
---
# <a name="kernels-for-jupyter-notebook-on-apache-spark-clusters-in-azure-hdinsight"></a>Ядра для записной книжки Jupyter в кластерах Apache Spark в Azure HDInsight

Кластеры HDInsight Spark предоставляют ядра, которые можно использовать с записной книжкой Jupyter в [Apache Spark](./apache-spark-overview.md) для тестирования приложений. Ядра — это программа, которая выполняет и интерпретирует ваш код. Вот эти ядра:

- **PySpark** — для приложений, написанных на языке Python2.
- **PySpark3** — для приложений, написанных на языке Python3.
- **Spark** — для приложений, написанных на языке Scala.

В этой статье вы узнаете, как использовать эти ядра, а также преимущества их использования.

## <a name="prerequisites"></a>Предварительные требования

Кластер Apache Spark в HDInsight. Инструкции см. в статье [Начало работы. Создание кластера Apache Spark в HDInsight на платформе Linux и выполнение интерактивных запросов с помощью SQL Spark](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Создание записной книжки Jupyter в Spark HDInsight

1. В [портал Azure](https://portal.azure.com/)выберите свой кластер Spark.  Инструкции см. в разделе [Отображение кластеров](../hdinsight-administer-use-portal-linux.md#showClusters). Откроется представление **Обзор** .

2. В представлении " **Обзор** " в области **панели мониторинга кластера** выберите **Записная книжка Jupyter**. При появлении запроса введите учетные данные администратора для кластера.

    ![Записная книжка Jupyter на Apache Spark](./media/apache-spark-jupyter-notebook-kernels/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Записная книжка Jupyter в Spark")
  
   > [!NOTE]  
   > Вы также можете получить доступ к записной книжке Jupyter в кластере Spark, открыв следующий URL-адрес в браузере. Замените **CLUSTERNAME** именем кластера:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. Выберите **создать**, а затем выберите **Pyspark**, **PySpark3**или **Spark** , чтобы создать записную книжку. Для приложений Scala используйте ядро Spark, для приложений Python2 — ядро PySpark, а для приложений Python3 — ядро PySpark3.

    ![Ядра для записной книжки Jupyter в Spark](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Ядра для записной книжки Jupyter в Spark")

4. Объект Notebook должен открыться с помощью выбранного ядра.

## <a name="benefits-of-using-the-kernels"></a>Преимущества использования ядер

Ниже приведены некоторые преимущества использования новых ядер для записной книжки Jupyter в кластерах Spark HDInsight.

- **Предустановленные контексты**. При использовании **PySpark**, **PySpark3**или ядер **Spark** не нужно явно задавать контексты Spark и Hive перед началом работы с приложениями. Эти контексты доступны по умолчанию. а именно:

  - **sc** для контекста Spark;
  - **sqlContext** для контекста Hive.

    Таким образом, для установки контекстов **не** нужно выполнять инструкции следующего вида:

         sc = SparkContext('yarn-client')
         sqlContext = HiveContext(sc)

    Вместо этого вы сможете сразу использовать в своем приложении предустановленные контексты.

- **Волшебные команды.** Ядро PySpark предоставляет несколько предопределенных "волшебных" специальных команд, которые можно вызывать с помощью `%%` (например, `%%MAGIC` `<args>`). Волшебная команда должна быть первым словом в ячейке кода и может состоять из нескольких строк содержимого. Волшебное слово должно быть первым словом в ячейке. Любые другие слова перед магической командой, даже комментарии, приведут к ошибке.     Дополнительные сведения о волшебных командах см. [здесь](https://ipython.readthedocs.org/en/stable/interactive/magics.html).

    В следующей таблице перечислены различные магические команды, доступные для ядер.

   | Волшебная команда | Пример | Описание |
   | --- | --- | --- |
   | справка |`%%help` |Формирует таблицу из всех доступных волшебных слов с примерами и описанием. |
   | сведения |`%%info` |Выводит сведения о сеансе для текущей конечной точки Livy. |
   | Настройка |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Настраивает параметры для создания сеанса. Флаг force (`-f`) является обязательным, если сеанс уже был создан, что гарантирует удаление и повторное создание сеанса. Список допустимых параметров приведен в разделе, посвященном [тексту запроса сеансов POST Livy](https://github.com/cloudera/livy#request-body) . Параметры должны передаваться в виде строки JSON, следующей после волшебной команды, как показано в столбце примера. |
   | sql |`%%sql -o <variable name>`<br> `SHOW TABLES` |Выполняет запрос Hive к sqlContext. Если передан параметр `-o` , результат запроса сохраняется в контексте Python %%local в качестве таблицы данных [Pandas](https://pandas.pydata.org/) . |
   | local |`%%local`<br>`a=1` |Весь код в последующих строках выполняется локально. Код должен быть допустимым кодом python2 независимо от того, какой ядро вы используете. Таким образом, даже если вы выбрали ядра **PySpark3** или **Spark** при создании записной книжки, при использовании `%%local` волшебия в ячейке эта ячейка должна содержать только допустимый код python2. |
   | журналы |`%%logs` |Выводит журналы для текущего сеанса Livy. |
   | "Удалить" |`%%delete -f -s <session number>` |Удаляет указанный сеанс для текущей конечной точки Livy. Нельзя удалить сеанс, запущенный для самого ядра. |
   | cleanup |`%%cleanup -f` |Удаляет все сеансы для текущей конечной точки Livy, включая сеанс этой записной книжки. Флаг -f является обязательным. |

   > [!NOTE]  
   > Помимо магических команд, добавленных ядром PySpark, можно также использовать [встроенные магические команды](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics) IPython, в том числе `%%sh`. Можно использовать магическую команду `%%sh` для выполнения сценариев и блоков кода на головном узле кластера.

- **Автоматическая визуализация**. Ядро Pyspark автоматически визуализирует выходные данные запросов Hive и SQL. Вы можете выбрать различные типы средства визуализации, включая таблицы, круговые диаграммы, графики, диаграммы с областями и линейчатые диаграммы.

## <a name="parameters-supported-with-the-sql-magic"></a>Параметры, поддерживаемые волшебной командой %%sql

Магическая команда `%%sql` поддерживает различные параметры, позволяющие управлять результатом выполнения запросов. Возможные результаты показаны в следующей таблице.

| Параметр | Пример | Описание |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |При использовании этого параметра результат запроса сохраняется в контексте Python %%local в качестве таблицы данных [Pandas](https://pandas.pydata.org/) . Именем переменной таблицы данных служит указанное вами имя переменной. |
| -Q |`-q` |Используйте этот параметр, чтобы отключить визуализации для ячейки. Если вы не хотите выполнять автовизуализацию содержимого ячейки и просто хотите записать ее как кадр данных, используйте `-q -o <VARIABLE>`. Если вы хотите отключить визуализацию, не записывая результаты (например, для выполнения запроса SQL, такого как инструкция `CREATE TABLE`), то используйте параметр `-q` без аргумента `-o`. |
| -M |`-m <METHOD>` |Параметр **METHOD** имеет значение **take** или **sample** (по умолчанию используется значение **take**). Если метод имеет **`take`** значение, ядро выбирает элементы из верхней части результирующего набора данных, заданного параметром MAXROWS (описывается далее в этой таблице). Если используется метод **sample**, то ядро выбирает элементы из набора данных случайным образом в соответствии с параметром `-r`, описанным далее в этой таблице. |
| -r |`-r <FRACTION>` |Здесь **FRACTION** — это число с плавающей запятой от 0,0 до 1,0. Если для SQL-запроса используется метод выборки `sample`, то ядро выбирает заданную долю элементов из результирующего набора случайным образом. Например, при выполнении SQL-запроса с аргументами `-m sample -r 0.01` из результирующего набора данных случайным образом отбирается 1 % строк. |
| -n |`-n <MAXROWS>` |**MAXROWS** должно быть выражено целым числом. Число выходных строк для параметра **MAXROWS** ограничивается ядром. Если **MAXROWS** является отрицательным числом, например **-1**, то число строк в результирующем наборе не ограничено. |

**Пример.**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

Приведенная выше инструкция выполняет следующие действия.

- Выбирает все записи из таблицы **hivesampletable**.
- Так как мы используем-q, он отключает автовизуализацию.
- Так как мы `-m sample -r 0.1 -n 500`используем, он случайным образом выбирает 10% строк в hivesampletable и ограничивает размер результирующего набора 500 строк.
- И наконец, сохраняет выходные данные в таблицу данных **query2**, так как включает параметр `-o query2`.

## <a name="considerations-while-using-the-new-kernels"></a>Рекомендации по использованию новых ядер

Какое бы ядро вы ни использовали, работающие объекты Notebook потребляют ресурсы кластера.  С этими ядрами, поскольку контексты являются предустановленными, просто выход из записных книжек не приводит к закрытию контекста. И поэтому ресурсы кластера продолжают использоваться. Рекомендуется использовать параметр **Закрыть и остановить** в меню **файл** записной книжки, когда вы закончите использовать записную книжку. Замыкание ликвидирует контекст, а затем выходит из записной книжки.

## <a name="where-are-the-notebooks-stored"></a>Где хранятся записные книжки?

Если кластер HDInsight использует службу хранилища Azure в качестве учетной записи хранения по умолчанию, записные книжки Jupyter сохраняются в папке **/HdiNotebooks** в учетной записи хранения.  Доступ к объектам Notebook, текстовым файлам и папкам, создаваемым в Jupyter, можно получить через учетную запись хранения.  Например, если вы используете Jupyter для создания папки **`myfolder`** и записной книжки **MyFolder/mynotebook. ipynb**, доступ к этой записной книжке `/HdiNotebooks/myfolder/mynotebook.ipynb` можно получить в учетной записи хранения.  Верно и обратное: если вы передаете объект Notebook непосредственно в свою учетную запись хранения в `/HdiNotebooks/mynotebook1.ipynb`, то этот объект также отображается в Jupyter.  Объекты Notebook хранятся в учетной записи хранения даже после удаления кластера.

> [!NOTE]  
> Для кластеров HDInsight, использующих Azure Data Lake Storage в качестве хранилища по умолчанию, записные книжки не сохраняются в связанном хранилище.

Записные книжки сохраняются в учетной записи хранения, совместимой с [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html). Если вы подключитесь к кластеру по протоколу SSH, вы можете использовать команды управления файлами:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                   # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it's visible from Jupyter

Независимо от того, использует ли кластер службу хранилища Azure или Azure Data Lake Storage в качестве учетной записи хранения по умолчанию, эти записные книжки также сохраняются в кластере головного узла по адресу `/var/lib/jupyter`.

## <a name="supported-browser"></a>Поддерживаемый браузер

Записные книжки Jupyter, выполняемые в кластерах HDInsight Spark, поддерживаются только браузером Google Chrome.

## <a name="feedback"></a>Отзывы

Новые ядра находятся в стадии развития и будут улучшаться со временем. Поэтому API-интерфейсы могут измениться по мере разработки этих ядер. Мы будем признательны вам за любые отзывы о работе с новыми ядрами. Обратная связь полезна при формировании окончательного выпуска этих ядер. Комментарии и отзывы можно оставить в разделе " **Отзывы** " в нижней части этой статьи.

## <a name="next-steps"></a>Дальнейшие действия

- [Обзор: Apache Spark в Azure HDInsight](apache-spark-overview.md)
- [Использование записных книжек Zeppelin с кластером Apache Spark в Azure HDInsight](apache-spark-zeppelin-notebook.md)
- [Использование внешних пакетов с записными книжками Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
- [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](apache-spark-jupyter-notebook-install-locally.md)
