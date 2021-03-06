---
title: Использование расширенного сервера журнала Spark для отладки приложений — Apache Spark в Azure синапсе
description: Используйте расширенный сервер журнала Spark для отладки и диагностики приложений Spark в Azure синапсе Analytics.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 4f03033942517f4778192e0b12f84610df8fd469
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429217"
---
# <a name="use-extended-apache-spark-history-server-to-debug-and-diagnose-apache-spark-applications"></a>Использование расширенного сервера журналов Apache Spark для отладки и диагностики Apache Spark приложений

В этой статье содержатся рекомендации по использованию расширенного сервера журналов Apache Spark для отладки и диагностики завершенных и выполняющихся приложений Spark.

Расширение включает вкладку "данные", вкладку графа и диагностику. Используйте вкладку " **данные** ", чтобы проверить входные и выходные данные задания Spark. На вкладке **граф** отображается поток данных и воспроизведение графа задания. На вкладке **Диагностика** отображаются **данные о неравномерном анализе данных**, **отклонения времени**и **использовании исполнителя**.

## <a name="access-the-apache-spark-history-server"></a>Доступ к серверу журнала Apache Spark

Apache Spark Server History — это пользовательский веб-интерфейс для завершенных и выполняющихся приложений Spark. Веб-интерфейс сервера журнала Apache Spark можно открыть из Azure синапсе Analytics.

### <a name="open-the-spark-history-server-web-ui-from-apache-spark-applications-node"></a>Открытие веб-интерфейса сервера журнала Spark из узла приложений Apache Spark

1. Откройте [Azure Synapse Analytics](https://web.azuresynapse.net/).

2. Щелкните **мониторинг**, а затем выберите **Apache Spark приложения**.

    ![Щелкните Мониторинг, а затем выберите приложение Spark.](./media/apache-spark-history-server/click-monitor-spark-application.png)

3. Выберите приложение, а затем откройте **запрос журнала** , щелкнув его.

    ![Откройте окно запроса журнала.](./media/apache-spark-history-server/open-application-window.png)

4. Выберите **сервер журнала Spark**, после чего отобразится веб-интерфейс сервера журнала Spark.

    ![Откройте сервер журнала Spark.](./media/apache-spark-history-server/open-spark-history-server.png)

### <a name="open-the-spark-history-server-web-ui-from-data-node"></a>Открытие пользовательского веб-интерфейса сервера журнала Spark из узла данных

1. В записной книжке Azure синапсе Studio выберите **сервер журнала Spark** в выходной ячейке выполнения задания или на панели состояния в нижней части документа записной книжки. Выберите **Сведения о сеансе**.

   ![Запуск сервера журнала Spark](./media/apache-spark-history-server/launch-history-server2.png "Запуск сервера журнала Spark")

2. Выберите **сервер журнала Spark** на панели слайды.

   ![Запуск сервера журнала Spark](./media/apache-spark-history-server/launch-history-server.png "Запуск сервера журнала Spark")

## <a name="explore-the-data-tab-in-spark-history-server"></a>Просмотр вкладки "данные" на сервере журнала Spark

Выберите идентификатор задания, которое требуется просмотреть. Затем в меню инструментов выберите пункт **данные** , чтобы получить представление данных. В этом разделе показано, как выполнять различные задачи на вкладке данные.

* Выберите вкладку **Входы**, **Выходы** или **Операции с таблицей**, чтобы просмотреть соответствующие сведения.

    ![Данные для вкладок приложения Spark](./media/apache-spark-history-server/apache-spark-data-tabs.png)

* Скопируйте все строки, выбрав **Копировать**.

    ![Данные для копирования приложения Spark](./media/apache-spark-history-server/apache-spark-data-copy.png)

* Сохраните все данные как CSV-файл, выбрав **CSV**.

    ![Данные для сохранения приложения Spark](./media/apache-spark-history-server/apache-spark-data-save.png)

* Выполните поиск, введя ключевые слова в поле **поиска**полей. Результаты поиска отображаются немедленно.

    ![Данные для поиска приложений Spark](./media/apache-spark-history-server/apache-spark-data-search.png)

* Выберите заголовок столбца для сортировки таблицы, щелкните знак «плюс», чтобы развернуть строку, чтобы отобразить дополнительные сведения, или щелкните знак «минус», чтобы свернуть строку.

    ![Данные для таблицы приложений Spark](./media/apache-spark-history-server/apache-spark-data-table.png)

* Скачайте отдельный файл, выбрав **частично загрузить**. Выбранный файл загружается в локальную. Если файл больше не существует, появится новая вкладка с сообщением об ошибке.

    ![Данные для строки скачивания приложения Spark](./media/apache-spark-history-server/sparkui-data-download-row.png)

* Чтобы скопировать полный или относительный путь, выберите параметры **Копировать полный путь** или **Копировать относительный путь** , которые расширяются из раскрывающегося меню. Для Azure Data Lake Storage файлов **откройте в обозреватель службы хранилища Azure** запускает обозреватель службы хранилища Azure и находит папку, когда вы вошли в нее.

    ![Данные для пути копирования приложения Spark](./media/apache-spark-history-server/sparkui-data-copy-path.png)

* Выберите номера страниц под таблицей для навигации по страницам, если на одной странице слишком много строк для вывода.

    ![Страница "данные для приложения Spark"](./media/apache-spark-history-server/apache-spark-data-page.png)

* Наведите указатель на вопросительный знак рядом с полем **данные** , чтобы отобразить подсказку, или выберите вопросительный знак для получения дополнительных сведений.

    ![Дополнительные сведения о данных для приложения Spark](./media/apache-spark-history-server/sparkui-data-more-info.png)

* Отправьте отзыв о проблемах, выбрав параметр **отправить нам отзыв**.

    ![Граф Spark снова предоставляет нам отзыв](./media/apache-spark-history-server/sparkui-graph-feedback.png)

## <a name="graph-tab-in-apache-spark-history-server"></a>Вкладка "Граф" в Apache Spark сервере журналов

Выберите идентификатор задания, которое требуется просмотреть. Затем в меню инструментов выберите пункт **Диаграмма** , чтобы получить представление граф задания.

### <a name="overview"></a>Обзор

В созданном графе задания можно просмотреть общие сведения о задании. По умолчанию на диаграмме отображаются все задания. Это представление можно отфильтровать по **идентификатору задания**.

![Идентификатор задания приложения Spark и графа заданий](./media/apache-spark-history-server/apache-spark-graph-jobid.png)

### <a name="display"></a>Отображение

По умолчанию выбрано отображение **хода выполнения** . Чтобы проверить поток данных, выберите **Чтение** или **запись** в раскрывающемся списке **Отображение** .

![Отображение приложения Spark и графа заданий](./media/apache-spark-history-server/sparkui-graph-display.png)

Узел графа отображает цвета, показанные в условных обозначениях тепловой карты.

![Приложение Spark и тепловой карты графа заданий](./media/apache-spark-history-server/sparkui-graph-heatmap.png)

### <a name="playback"></a>Воспроизведение

Чтобы воспроизвести задание, выберите **Воспроизведение**. Вы можете в любое время нажать кнопку " **Отключить** ". При воспроизведении цветов задачи отображаются разные состояния.

|Цвет|Значение|
|-|-|
|Зеленый|Успешно: задание успешно завершено.|
|Оранжевый|Повторная попытка: экземпляры задач, которые завершились ошибкой, но не влияют на окончательный результат задания. Эти задачи имеют дублирующиеся или повторные экземпляры, которые могут быть успешно выполнены позже.|
|Синий|Выполнение: задача запущена.|
|White|Ожидание или пропущено: задача ожидает выполнения, или этап пропущен.|
|Красный|Сбой: сбой задачи.|

На следующем рисунке показаны зеленые, оранжевый и синие цвета состояния.

![Образец цвета для приложения и задания Spark, выполнение](./media/apache-spark-history-server/sparkui-graph-color-running.png)

На следующем рисунке показаны зеленые и белые цвета состояния.

![Пример цвета для приложения Spark и графика заданий, пропуск](./media/apache-spark-history-server/sparkui-graph-color-skip.png)

На следующем рисунке показаны цвета красного и зеленого состояний.

![Пример цвета для приложения Spark и графа заданий, сбой](./media/apache-spark-history-server/sparkui-graph-color-failed.png)

> [!NOTE]  
> Допускается воспроизведение каждого задания. Для незавершенных заданий воспроизведение не поддерживается.

### <a name="zoom"></a>Zoom

Используйте прокрутку мыши, чтобы увеличить или уменьшить график задания, или выберите **масштаб в соответствии** с размерами экрана.

![Масштабирование приложения Spark и графа заданий в соответствии с](./media/apache-spark-history-server/sparkui-graph-zoom2fit.png)

### <a name="tooltips"></a>Всплывающие подсказки

Наведите указатель мыши на узел графа, чтобы увидеть подсказку при наличии невыполненных задач, и выберите этап, чтобы открыть страницу его этапа.

![Подсказка для приложения Spark и графа заданий](./media/apache-spark-history-server/sparkui-graph-tooltip.png)

На вкладке граф заданий этапы имеют подсказку и маленький значок, если у них есть задачи, отвечающие следующим условиям.

|Условие|Описание|
|-|-|
|Неравномерное распределение данных|объем чтения данных > средний размер считанных данных для всех задач в этом этапе * 2 и размер чтения данных > 10 МБ|
|Отклонение по времени|время выполнения > среднее время выполнения всех задач в этом этапе * 2 и времени выполнения > 2 минуты|
   
![Значок рассогласования для приложения Spark и графа заданий](./media/apache-spark-history-server/sparkui-graph-skew-icon.png)

### <a name="graph-node-description"></a>Описание узла графа

В узле граф заданий отображаются следующие сведения о каждом этапе.

  * ID.
  * имя или описание;
  * общее количество задач;
  * Чтение данных: сумма размера входных данных и размер данных чтения в случайном порядке.
  * Запись данных: сумма размера вывода и размер записи в случайном порядке.
  * время выполнения: время от начала первой попытки до завершения последней попытки;
  * число строк: сумма входных записей, выходных записей, записей смешанного чтения и записей смешанной записи;
  * ход выполнения.

    > [!NOTE]  
    > По умолчанию узел граф заданий отображает сведения о последней попыток каждого этапа (за исключением времени выполнения этапа). Однако во время воспроизведения узел графа отображает сведения каждой попытки.
    >  
    > Размер данных для чтения и записи составляет 1 МБ = 1000 КБ = 1000 * 1000 байт.

### <a name="provide-feedback"></a>Предоставление отзыва

Отправьте отзыв о проблемах, выбрав параметр **отправить нам отзыв**.

![Отзывы о приложении Spark и графе заданий](./media/apache-spark-history-server/sparkui-graph-feedback.png)

## <a name="explore-the-diagnosis-tab-in-apache-spark-history-server"></a>Просмотр вкладки "Диагностика" в Apache Spark сервере журналов

Чтобы открыть вкладку Диагностика, выберите идентификатор задания. Затем в меню инструментов выберите **Диагностика** , чтобы получить представление диагностики заданий. На вкладке диагностики доступны вкладки **Неравномерное распределение данных**, **Неравномерное распределение времени** и **Анализ использования исполнителя**.

Выберите вкладку **Неравномерное распределение данных**, **Неравномерное распределение времени** или **Анализ использования исполнителя**, чтобы просмотреть соответствующие сведения.

![Повторная вкладка "неравномерное Спаркуи данных диагностики"](./media/apache-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Неравномерное распределение данных

При выборе вкладки « **отклонение данных** » соответствующие отклоненные задачи отображаются на основе указанных параметров.

* **Укажите параметры** — в первом разделе отображаются параметры, которые служат для обнаружения неравномерного распределения данных. Правило по умолчанию: чтение данных задачи больше трех раз из числа считанных данных задачи, а чтение данных задачи превышает 10 МБ. Если вы хотите определить собственное правило для отклоненных задач, можно выбрать параметры. разделы с **отклоненным этапом** и **наклоном символов** будут обновлены соответствующим образом.

* **Этап с неравномерным распределением** — во втором разделе отображаются этапы, имеющие задачи с неравномерным распределением, которые отвечают указанным выше условиям. Если на этапе более одной задачи с неравномерным распределением, в таблице этапа с неравномерным распределением отображается только одна задача с наибольшим неравномерным распределением (например, наибольшим размером неравномерно распределенных данных).

    ![Вкладка "неравномерность данных диагностики спаркуи"](./media/apache-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

* **Диаграмма** "неравномерные" — когда выбрана строка в таблице этап отклонения, на диаграмме с неравномерным распределением отображаются дополнительные сведения о распределении задач на основе времени чтения и выполнения данных. Задачи с неравномерным распределением отмечены красным цветом, а нормальные задачи — синим. На диаграмме отображается до 100 образцов задач, а сведения о задачах отображаются в правой нижней панели.

    ![спаркуиная диаграмма отклонений для этапа 10](./media/apache-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Неравномерное распределение времени

На вкладке **Неравномерное распределение времени** отображаются задачи с неравномерным распределением времени выполнения.

* **Укажите параметры** . в первом разделе отображаются параметры, которые используются для определения смещения времени. По умолчанию для обнаружения неравномерного распределения времени используется следующий критерий: время выполнения задачи больше среднего времени выполнения, умноженного на три, и превышает 30 секунд. Параметры можно изменить в соответствии с вашими потребностями. В разделах **Этап с неравномерным распределением** и **Диаграмма неравномерного распределения** отображаются соответствующие сведения об этапах и задачах, так же как на вкладке **Неравномерное распределение данных**, описанной выше.

* Выберите **отклонение по времени**, после чего отфильтрованный результат отобразится в разделе **отклоненный этап** в соответствии с параметрами, заданными в разделе **Указание параметров**. Выберите один элемент на **отклоненном этапе** , после чего соответствующая диаграмма будет черновиком в section3, а сведения о задаче отобразятся в правой нижней панели.

    ![Секция отклонения времени диагностики спаркуи](./media/apache-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Анализ использования исполнителя

На графике использования выполнителя отображаются сведения о выделении и выполнении исполнителя задания Spark.  

1. Выберите **Анализ использования исполнителей**, затем четыре типа кривых об использовании исполнителя являются черновиками, включая **выделенные исполнители**, **выполняющиеся исполнители**, **простаивающие исполнители**и **Максимальное количество экземпляров исполнителя**. В отношении выделенных исполнителей каждое событие "исполнитель" или "исполнитель удален" увеличивает или уменьшает выделенные исполнители. Дополнительные возможности сравнения доступны на временной шкале событий на вкладке "Задания".

   ![Вкладка "исполнители диагностики спаркуи"](./media/apache-spark-history-server/sparkui-diagnosis-executors.png)

2. Выберите значок цвета, чтобы выбрать или отменить выбор соответствующего содержимого во всех черновиках.

    ![спаркуи диагностика выбор диаграммы](./media/apache-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="known-issues"></a>Известные проблемы

Входные и выходные данные, использующие отказоустойчивые распределенные наборы данных (RDD), не отображаются на вкладке данные.

## <a name="next-steps"></a>Дальнейшие действия

- [Azure Synapse Analytics](../overview-what-is.md)
- [Документация по .NET для Apache Spark](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

