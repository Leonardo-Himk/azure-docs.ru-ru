---
title: Анализ и отслеживание смещения данных в наборах (Предварительная версия)
titleSuffix: Azure Machine Learning
description: Создание Машинное обучение Azure наборов данных. мониторы (Предварительная версия), отслеживание смещения данных в наборах и Настройка оповещений.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/04/2019
ms.openlocfilehash: e49c621d92a8aa604b5f95291c5d80c0141f41dd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81682723"
---
# <a name="detect-data-drift-preview-on-datasets"></a>Обнаружение смещения данных (Предварительная версия) в наборах
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

В этой статье вы узнаете, как создавать мониторы набора данных Машинное обучение Azure (Предварительная версия), отслеживать их смещение и статистические изменения в наборах данных, а также настраивать оповещения.

Мониторы Машинное обучение Azureного набора данных позволяют:
* **Проанализируйте смещение данных** , чтобы понять, как оно изменяется с течением времени.
* **Отслеживайте данные модели** на предмет различий между обучающими и обслуживающими наборами данных.
* **Отслеживайте новые данные** на предмет различий между базовым и целевым наборами данных.
* **Функции профилирования в данных** для трассировки изменения статистических свойств с течением времени.
* **Настройте оповещения о смещении данных** для раннего предупреждения о потенциальных проблемах. 

Метрики и аналитика доступны в ресурсе [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) , связанном с рабочей областью машинное обучение Azure.

> [!Important]
> Обратите внимание, что отслеживание смещения данных с помощью пакета SDK доступно во всех выпусках, в то время как при наблюдении за данными в студии в Интернете используется только выпуск Enterprise Edition.

## <a name="prerequisites"></a>Предварительные условия

Для создания мониторов набора данных и работы с ними требуются:
* Подписка Azure. Если у вас еще нет подписки Azure, создайте бесплатную учетную запись, прежде чем начинать работу. Опробуйте [бесплатную или платную версию Машинного обучения Azure](https://aka.ms/AMLFree) уже сегодня.
* [Рабочая область машинное обучение Azure](how-to-manage-workspace.md).
* [Установленный пакет SDK для машинное обучение Azure для Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), включающий пакет azureml-DataSets.
* Структурированные (табличные) данные с меткой времени, указанной в пути к файлу, имени файла или столбце в данных.

## <a name="what-is-data-drift"></a>Что такое смещение данных?

В контексте машинного обучения смещение данных — это изменение входных данных модели, которое приводит к снижению производительности модели. Это одна из основных причин, с которой точность снижается с течением времени, поэтому отслеживание смещения данных помогает выявить проблемы с производительностью модели.

Причины смещения данных: 

- Вышестоящее изменение процесса, например замена датчика, который изменяет единицы измерения с дюймов на сантиметры. 
- Проблемы с качеством данных, такие как неисправный датчик, всегда считывают 0.
- Естественное смещение данных, например среднее значение температуры, изменяющееся с сезонами.
- Измените отношение между компонентами или ковариаций Shift. 

С помощью мониторов Машинное обучение Azureного набора данных можно настроить оповещения, помогающие при обнаружении расхождения данных в наборах с течением времени. 

### <a name="dataset-monitors"></a>Мониторы набора данных 

Вы можете создать монитор набора данных для обнаружения и оповещения о смещении данных в новых данных в наборе данных, анализа исторических данных для смещения и профилирования новых данных с течением времени. Алгоритм смещения данных предоставляет общую меру изменения данных и указывает, какие функции отвечают за дальнейшее исследование. Мониторы наборов данных создают ряд других метрик путем профилирования новых данных в `timeseries` наборе данных. Пользовательское оповещение можно настроить во всех метриках, создаваемых монитором, с помощью [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Мониторы наборов данных можно использовать для быстрого перехвата проблем с данными и сокращения времени отладки проблемы путем выявления вероятных причин.  

Концептуально существует три основных сценария настройки мониторов набора данных в Машинное обучение Azure.

Сценарий | Описание
---|---
Мониторинг данных, обслуживающих модель, для отклонения от обучающих данных модели | Результаты из этого сценария можно интерпретировать как мониторинг прокси-сервера для точности модели, учитывая, что точность модели снижается, если данные передаются из обучающих данных.
Наблюдение за набором данных временных рядов для смещения за предыдущий период времени. | Этот сценарий является более общим и может использоваться для мониторинга наборов данных, участвующих в создании вышестоящего или переходящего в процессе создания модели.  Целевой набор данных должен иметь столбец timestamp, а базовый набор данных может быть любым табличным набором данных, в котором есть функции, общие с целевым набором данных.
Выполнение анализа прошлых данных. | Этот сценарий можно использовать для понимания исторических данных и информирования решений о параметрах мониторов набора данных.

## <a name="how-dataset-can-monitor-data"></a>Как набор данных может отслеживать данные

С помощью Машинное обучение Azure с помощью наборов данных отслеживается смещение. Для отслеживания смещения данных можно указать базовый набор данных — обычно это набор обучающих данных для модели. Целевой набор данных (обычно входные данные модели) сравнивается со временем для базового набора данных. Это сравнение означает, что в целевом наборе данных должен быть указан столбец timestamp.

### <a name="set-the-timeseries-trait-in-the-target-dataset"></a>`timeseries` Установка признака в целевом наборе данных

Для целевого набора данных необходимо установить набор `timeseries` характеристик, указав столбец timestamp из столбца в данных или виртуального столбца, производного от шаблона пути к файлам. Это можно сделать с помощью пакета SDK для Python или Машинное обучение Azure Studio. Чтобы добавить `timeseries` признаки к набору данных, необходимо указать столбец, представляющий "тонкую" отметку времени. Если данные разделены на структуру папок со сведениями о времени, например "{гггг/мм/дд}", можно создать виртуальный столбец с помощью параметра шаблона пути и задать его как отметку времени "грубое зерно", чтобы повысить важность функциональности временных рядов. 

#### <a name="python-sdk"></a>Пакет SDK для Python

[`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) Метод [`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) класса определяет столбец временной метки для набора данных. 

```python 
from azureml.core import Workspace, Dataset, Datastore

# get workspace object
ws = Workspace.from_config()

# get datastore object 
dstore = Datastore.get(ws, 'your datastore name')

# specify datastore paths
dstore_paths = [(dstore, 'weather/*/*/*/*/data.parquet')]

# specify partition format
partition_format = 'weather/{state}/{date:yyyy/MM/dd}/data.parquet'

# create the Tabular dataset with 'state' and 'date' as virtual columns 
dset = Dataset.Tabular.from_parquet_files(path=dstore_paths, partition_format=partition_format)

# assign the timestamp attribute to a real or virtual column in the dataset
dset = dset.with_timestamp_columns('date')

# register the dataset as the target dataset
dset = dset.register(ws, 'target')
```

Полный пример использования `timeseries` наборов данных см. в [примере записной книжки](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb) или в [документации по пакету SDK для наборов данных](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-).

#### <a name="azure-machine-learning-studio"></a>Студия машинного обучения Azure.
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

При создании набора данных с помощью Машинное обучение Azure Studio убедитесь, что путь к данным содержит сведения о метке времени, включает все вложенные папки с данными и задает формат раздела. 

В следующем примере берется все данные из вложенной папки *ноааисдфлорида/2019* , а в формате секции указывается год, месяц и день метки времени. 

[![Формат раздела](./media/how-to-monitor-datasets/partition-format.png)](media/how-to-monitor-datasets/partition-format-expand.png)

В параметрах **схемы** укажите столбец отметок времени из виртуального или вещественного столбца в указанном наборе данных:

![Отметка времени](./media/how-to-monitor-datasets/timestamp.png)

## <a name="dataset-monitor-settings"></a>Параметры монитора набора данных

После создания набора данных с указанными параметрами отметок времени вы можете настроить монитор набора данных.

Параметры монитора различных наборов данных разбиваются на три группы: **Основные сведения, параметры монитора** и **Параметры**обратной записи.

### <a name="basic-info"></a>Общие сведения

Эта таблица содержит основные параметры, используемые для монитора набора данных.

| Параметр | Описание | "Советы" | Изменяемый | 
| ------- | ----------- | ---- | ------- | 
| Имя | Имя монитора набора данных. | | Нет |
| Базовый набор данных | Табличный набор данных, который будет использоваться в качестве базового для сравнения целевого набора данных с течением времени. | Базовый набор данных должен иметь функции, общие с целевым набором данных. Как правило, базовая папка должна иметь набор данных для обучения модели или срез целевого набора данных. | Нет |
| Целевой набор данных | Табличный набор данных с заданным столбцом отметок времени, который будет проанализирован для смещения данных. | Целевой набор данных должен иметь функции, общие с базовым набором данных, а также `timeseries` набор данных, к которому добавляются новые данные. Исторические данные в целевом наборе данных можно анализировать, а также отслеживать новые данные. | Нет | 
| Частота | Частота, которая будет использоваться для планирования задания конвейера и анализа исторических данных при выполнении обратной передачи. Параметры включают ежедневное, еженедельное или ежемесячное. | Настройте этот параметр, чтобы включить сравнимый размер данных в базовый план. | Нет | 
| Функции | Список функций, которые будут анализироваться за смещение данных с течением времени. | Задайте функции вывода модели для измерения смещения концепции. Не включайте функции, которые естественным образом отменяют время (месяц, год, индекс и т. д.). После настройки списка функций можно выполнить обратную засыпку и существующий монитор рассмещения данных. | Да | 
| Целевой объект вычисления | Целевой объект Машинное обучение Azure вычислений для запуска заданий монитора набора данных. | | Да | 

### <a name="monitor-settings"></a>Параметры монитора

Эти параметры предназначены для конвейера монитора запланированного набора данных, который будет создан. 

| Параметр | Описание | "Советы" | Изменяемый | 
| ------- | ----------- | ---- | ------- |
| Включите параметр | Включение или отключение расписания в конвейере монитора набора данных | Отключите расписание для анализа исторических данных с помощью параметра «обратная засыпку». Его можно включить после создания монитора набора данных. | Да | 
| Задержка | Время в часах, затраченное на получение данных в наборе данных. Например, если для получения данных в базе данных SQL, которую инкапсулирует данные, требуется три дня, установите задержку 72. | Невозможно изменить после создания монитора набора данных | Нет | 
| Адреса электронной почты. | Адреса электронной почты для оповещений на основе нарушения порогового значения процента смещения данных. | Сообщения электронной почты отправляются через Azure Monitor. | Да | 
| Порог | Пороговое значение смещения данных в процентах для предупреждений электронной почты. | Дополнительные оповещения и события можно задать во многих других метриках в связанном Application Insights ресурсе рабочей области. | Да | 

### <a name="backfill-settings"></a>Параметры обратной засыпку

Эти параметры предназначены для выполнения обратной обратной засыпку для метрик смещения данных.

| Параметр | Описание | "Советы" |
| ------- | ----------- | ---- |
| Дата начала | Дата начала задания на обратную засыпку. | | 
| Дата окончания | Дата окончания задания на обратную засыпку. | Дата окончания не может быть больше 31 * частота единиц времени с даты начала. В существующем мониторе набора данных метрики могут быть заполнены, чтобы анализировать исторические данные или заменять метрики обновленными параметрами. |

## <a name="create-dataset-monitors"></a>Создание мониторов набора данных 

Создание мониторов набора данных для обнаружения и оповещения о смещении данных в новом наборе данных с помощью Машинное обучение Azure Studio или пакета SDK Python. 

### <a name="azure-machine-learning-studio"></a>Студия машинного обучения Azure.
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

Чтобы настроить оповещения в мониторе набора данных, Рабочая область, содержащая набор данных, для которого необходимо создать монитор, должен иметь возможности выпуска Enterprise Edition. 

После подтверждения функциональности рабочей области перейдите на домашнюю страницу Studio и выберите вкладку Наборы данных слева. Выберите Мониторы набора данных.

![Список мониторов](./media/how-to-monitor-datasets/monitor-list.png)

Нажмите кнопку **+ Создать монитор** и продолжайте работу с мастером, нажав кнопку **Далее**.

![Мастер](./media/how-to-monitor-datasets/wizard.png)

Результирующий монитор набора данных появится в списке. Выберите его, чтобы открыть страницу сведений этого монитора.

### <a name="from-python-sdk"></a>Из пакета SDK для Python

Подробные сведения см. в [справочной документации по пакету SDK для Python о смещении данных](/python/api/azureml-datadrift/azureml.datadrift) . 

В следующем примере показано, как создать монитор набора данных с помощью пакета SDK для Python.

```python
from azureml.core import Workspace, Dataset
from azureml.datadrift import DataDriftDetector
from datetime import datetime

# get the workspace object
ws = Workspace.from_config()

# get the target dataset
dset = Dataset.get_by_name(ws, 'target')

# set the baseline dataset
baseline = target.time_before(datetime(2019, 2, 1))

# set up feature list
features = ['latitude', 'longitude', 'elevation', 'windAngle', 'windSpeed', 'temperature', 'snowDepth', 'stationName', 'countryOrRegion']

# set up data drift detector
monitor = DataDriftDetector.create_from_datasets(ws, 'drift-monitor', baseline, target, 
                                                      compute_target='cpu-cluster', 
                                                      frequency='Week', 
                                                      feature_list=None, 
                                                      drift_threshold=.6, 
                                                      latency=24)

# get data drift detector by name
monitor = DataDriftDetector.get_by_name(ws, 'drift-monitor')

# update data drift detector
monitor = monitor.update(feature_list=features)

# run a backfill for January through May
backfill1 = monitor.backfill(datetime(2019, 1, 1), datetime(2019, 5, 1))

# run a backfill for May through today
backfill1 = monitor.backfill(datetime(2019, 5, 1), datetime.today())

# disable the pipeline schedule for the data drift detector
monitor = monitor.disable_schedule()

# enable the pipeline schedule for the data drift detector
monitor = monitor.enable_schedule()
```

Полный пример настройки `timeseries` набора данных и средства обнаружения смещения данных см. в нашем [примере записной книжки](https://aka.ms/datadrift-notebook).

## <a name="understanding-data-drift-results"></a>Основные сведения о результатах смещения данных

Монитор данных создает две группы результатов: Общие сведения о смещении и сведения о характеристиках. Следующая анимация показывает доступные диаграммы монитора смещения на основе выбранной функции и метрики. 

![Демонстрационное видео](./media/how-to-monitor-datasets/video.gif)

### <a name="drift-overview"></a>Общие сведения о смещении

Раздел Обзор отклонения содержит подробные **сведения о** величине смещения данных, а также о возможностях дальнейшего анализа. 

| Метрика | Описание | "Советы" | 
| ------ | ----------- | ---- | 
| Величина смещения данных | Задается в процентах между базовым и целевым наборами данных с течением времени. В диапазоне от 0 до 100, где 0 указывает на идентичные наборы данных, а 100 указывает, что функция Машинное обучение Azureного смещения может полностью сообщать два набора данных. | Шум в выражении точного процента ожидается из-за методик машинного обучения, используемых для создания этой величины. | 
| Отклонения от вклада в функцию | Вклад каждого компонента в целевом наборе данных в измеренную величину смещения. |  Из-за ковариаций сдвига базовое распределение компонента не обязательно должно быть изменено, чтобы иметь относительно высокую важность функций. | 

На следующем рисунке показан пример диаграмм, отображаемых в результатах « **Обзор** отклонения» в машинное обучение Azure Studio, что приводит к получению [данных из интегрированной области NOAA](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/). Данные были `stationName contains 'FLORIDA'`вычислены с 2019 января в качестве базового набора данных и всех данных 2019, используемых в качестве целевого.
 
![Общие сведения о смещении](./media/how-to-monitor-datasets/drift-overview.png)

### <a name="feature-details"></a>Сведения о функции

В разделе **сведения о функциях** содержится информация об изменениях в распределении выбранного компонента, а также другие статистические данные со временем. 

Целевой набор данных также профилирован с течением времени. Статистическое расстояние между базовым распределением каждого компонента сравнивается со временем целевого набора данных, что концептуально напоминает величину смещения данных за исключением того, что это статистическое расстояние предназначено для отдельной функции. Также доступны значения min, Max и среднее. 

В Машинное обучение Azure Studio, если щелкнуть точку данных на графе, распределение отображаемого компонента будет соответствующим образом скорректировано. По умолчанию он показывает распределение базового набора данных и самое последнее распределение выполнения для одной и той же функции. 

Эти метрики также можно получить в пакете SDK для Python с помощью `get_metrics()` метода для `DataDriftDetector` объекта. 

#### <a name="numeric-features"></a>Числовые функции 

Числовые возможности профилирования выполняются в каждом мониторе набора данных. В Машинное обучение Azure Studio доступны следующие. Для распределения отображается плотность вероятности.

| Метрика | Описание |  
| ------ | ----------- |  
| Dresdner расстояние | Минимальный объем работы для преобразования базового распределения в целевое распределение. |
| Среднее значение | Среднее значение функции. |
| Минимальное значение | Минимальное значение функции. |
| Максимальное значение | Максимальное значение функции. |

![Числовой сведения о компоненте](./media/how-to-monitor-datasets/feature-details.png)

#### <a name="categorical-features"></a>Функции категорий 

Числовые возможности профилирования выполняются в каждом мониторе набора данных. В Машинное обучение Azure Studio доступны следующие. Для распределения отображается гистограмма.

| Метрика | Описание |  
| ------ | ----------- |  
| Евклидовой расстояние | Геометрическое расстояние между базовым и целевым распределениями. |
| Уникальные значения | Число уникальных значений функции (количество элементов). |


![Подробные сведения о характеристиках](./media/how-to-monitor-datasets/feature-details2.png)

## <a name="metrics-alerts-and-events"></a>Метрики, оповещения и события

Вы можете запросить метрики в ресурсе [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) , связанном с рабочей областью машинного обучения. Который предоставляет доступ ко всем функциям Application Insights, включая настройку для настраиваемых правил оповещений и групп действий, чтобы активировать такие действия, как, электронная почта, SMS, Push/Voice или функция Azure. Дополнительные сведения см. в полной Application Insights документации. 

Чтобы приступить к работе, перейдите к портал Azure и выберите страницу **обзора** вашей рабочей области.  Связанный Application Insights ресурс находится в крайнем правом углу:

[![Обзор портала Azure](./media/how-to-monitor-datasets/ap-overview.png)](media/how-to-monitor-datasets/ap-overview-expanded.png)

Выберите журналы (аналитика) в разделе Мониторинг на левой панели:

![Обзор Application Insights](./media/how-to-monitor-datasets/ai-overview.png)

Метрики монитора набора данных хранятся в виде `customMetrics`. После настройки монитора набора данных для просмотра можно написать и выполнить запрос:

[![Запрос log Analytics](./media/how-to-monitor-datasets/simple-query.png)](media/how-to-monitor-datasets/simple-query-expanded.png)

Определив метрики, чтобы настроить правила оповещений, создайте новое правило генерации оповещений:

![Новое правило генерации оповещений](./media/how-to-monitor-datasets/alert-rule.png)

Можно использовать существующую группу действий или создать новую, чтобы определить действие, выполняемое при выполнении условий набора.

![Новая группа действий](./media/how-to-monitor-datasets/action-group.png)

## <a name="troubleshooting"></a>Устранение неполадок

Ограничения и известные проблемы:

* Интервал времени заданий на обратную засыпку ограничен 31 интервалами для параметра частоты монитора. 
* Ограничение функций 200, если не указан список функций (все используемые компоненты).
* Размер вычислений должен быть достаточно большим, чтобы обрабатывать данные. 
* Убедитесь, что набор данных содержит данные в пределах начальной и конечной даты для данного монитора.
* Мониторы наборов данных будут работать только на наборах данных, содержащих 50 строк или более. 

Столбцы или функции в наборе данных классифицируются по категориям или числу в зависимости от условий, приведенных в следующей таблице. Если функция не соответствует этим условиям (например, столбец строкового типа с >100 уникальными значениями), то функция удаляется из алгоритма смещения данных, но по-прежнему выполняется в процессе профилирования. 

| Тип компонента | Тип данных | Условие | Ограничения | 
| ------------ | --------- | --------- | ----------- |
| категориальные; | String, bool, int, float | Число уникальных значений в компоненте меньше 100 и меньше 5% от количества строк. | Значение NULL считается собственной категорией. | 
| числовые; | int, float | Значения в функции имеют числовой тип данных и не соответствуют условию для функции категоризации. | Функция удалена, если >15% значений равны NULL. | 

## <a name="next-steps"></a>Дальнейшие шаги

* Перейдите в [машинное обучение Azure Studio](https://ml.azure.com) или в [записную книжку Python](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datadrift-tutorial/datadrift-tutorial.ipynb) , чтобы настроить монитор набора данных.
* См. раздел Настройка смещения данных для [моделей, развернутых в службе Azure Kubernetes](how-to-monitor-data-drift.md).
* Настройка мониторов смещения набора данных с помощью [сетки событий](how-to-use-event-grid.md). 
