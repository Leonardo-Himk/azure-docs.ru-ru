---
title: Планирование Машинное обучение Azure конвейеров
titleSuffix: Azure Machine Learning
description: Планирование Машинное обучение Azure конвейеров с помощью пакета SDK для Машинное обучение Azure для Python. Запланированные Конвейеры позволяют автоматизировать обычные и трудоемкие задачи, такие как обработка данных, обучение и мониторинг.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 11/12/2019
ms.openlocfilehash: 8e1e718fa4e6660d72203ac98bb6d427cdba2059
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82024563"
---
# <a name="schedule-machine-learning-pipelines-with-azure-machine-learning-sdk-for-python"></a>Планирование конвейеров машинного обучения с помощью пакета SDK для Машинное обучение Azure для Python

В этой статье вы узнаете, как программным образом запланировать запуск конвейера в Azure. Расписание можно создать на основе истекшего времени или изменения файловой системы. Расписания на основе времени можно использовать для выполнения регулярных задач, таких как мониторинг смещения данных. Расписания на основе изменений можно использовать для реагирования на нестандартные или непредсказуемые изменения, такие как отправка новых данных или изменение старых данных. После изучения способов создания расписаний вы узнаете, как извлекать и деактивировать их.

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас нет подписки Azure, создайте [бесплатную учетную запись](https://aka.ms/AMLFree).

* Среда Python, в которой установлен пакет SDK для Машинное обучение Azure для Python. Дополнительные сведения см [. в статьях создание повторно используемых сред для обучения и развертывания с помощью машинное обучение Azure и управление ими.](how-to-use-environments.md)

* Рабочая область Машинное обучение с опубликованным конвейером. Вы можете использовать встроенную [программу создания и запуска конвейеров машинного обучения с помощью пакета SDK для машинное обучение Azure](how-to-create-your-first-pipeline.md).

## <a name="initialize-the-workspace--get-data"></a>Инициализация рабочей области & получения данных

Чтобы запланировать конвейер, вам потребуется ссылка на рабочую область, идентификатор опубликованного конвейера и имя эксперимента, в котором вы хотите создать расписание. Эти значения можно получить с помощью следующего кода:

```Python
import azureml.core
from azureml.core import Workspace
from azureml.pipeline.core import Pipeline, PublishedPipeline
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

experiments = Experiment.list(ws)
for experiment in experiments:
    print(experiment.name)

published_pipelines = PublishedPipeline.list(ws)
for published_pipeline in  published_pipelines:
    print(f"{published_pipeline.name},'{published_pipeline.id}'")

experiment_name = "MyExperiment" 
pipeline_id = "aaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" 
```

## <a name="create-a-schedule"></a>Создание расписания

Чтобы выполнить конвейер на повторяющейся основе, создайте расписание. Объект `Schedule` связывает конвейер, эксперимент и триггер. Триггер может иметь значение`ScheduleRecurrence` , описывающее время ожидания между запусками или путь к хранилищу данных, указывающий каталог для отслеживания изменений. В любом случае вам потребуется идентификатор конвейера и имя эксперимента, в котором создается расписание.

В верхней части файла Python импортируйте классы `Schedule` и: `ScheduleRecurrence`

```python

from azureml.pipeline.core.schedule import ScheduleRecurrence, Schedule
```

### <a name="create-a-time-based-schedule"></a>Создание расписания на основе времени

`ScheduleRecurrence` Конструктор имеет обязательный `frequency` аргумент, который должен быть одной из следующих строк: "Minute", "Hour", "Day", "Week" или "month". Кроме того, требуется целочисленный `interval` аргумент, указывающий, `frequency` сколько единиц должно пройти между запусками расписания. Необязательные аргументы позволяют более подробно узнать о времени запуска, как описано в [документации по пакету SDK для счедулерекурренце](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedulerecurrence?view=azure-ml-py).

Создайте `Schedule` , который начинает выполнение каждые 15 минут:

```python
recurrence = ScheduleRecurrence(frequency="Minute", interval=15)
recurring_schedule = Schedule.create(ws, name="MyRecurringSchedule", 
                            description="Based on time",
                            pipeline_id=pipeline_id, 
                            experiment_name=experiment_name, 
                            recurrence=recurrence)
```

### <a name="create-a-change-based-schedule"></a>Создание расписания на основе изменений

Конвейеры, активируемые изменениями файлов, могут оказаться более эффективными по сравнению с расписаниями, основанными на времени. Например, может потребоваться выполнить шаг предварительной обработки при изменении файла или при добавлении нового файла в каталог данных. Вы можете отслеживать любые изменения в хранилище данных или изменения в определенном каталоге в хранилище данных. При мониторинге определенного каталога изменения в подкаталогах этого каталога _не_ активируют запуск.

Для создания реактивного `Schedule`файла необходимо задать `datastore` параметр в вызове [Schedule. Create](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedule?view=azure-ml-py#create-workspace--name--pipeline-id--experiment-name--recurrence-none--description-none--pipeline-parameters-none--wait-for-provisioning-false--wait-timeout-3600--datastore-none--polling-interval-5--data-path-parameter-name-none--continue-on-step-failure-none--path-on-datastore-none---workflow-provider-none---service-endpoint-none-). Для наблюдения за папкой задайте `path_on_datastore` аргумент.

`polling_interval` Аргумент позволяет указать в минутах частоту проверки хранилища данных на наличие изменений.

Если конвейер был создан с помощью [пути](https://docs.microsoft.com/python/api/azureml-core/azureml.data.datapath.datapath?view=azure-ml-py) [пипелинепараметер](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelineparameter?view=azure-ml-py), можно присвоить этой переменной имя измененного файла, задав `data_path_parameter_name` аргумент.

```python
datastore = Datastore(workspace=ws, name="workspaceblobstore")

reactive_schedule = Schedule.create(ws, name="MyReactiveSchedule", description="Based on input file change.",
                            pipeline_id=pipeline_id, experiment_name=experiment_name, datastore=datastore, data_path_parameter_name="input_data")
```

### <a name="optional-arguments-when-creating-a-schedule"></a>Необязательные аргументы при создании расписания

В дополнение к описанным выше аргументам можно задать для `status` аргумента значение `"Disabled"` , чтобы создать неактивное расписание. Наконец, `continue_on_step_failure` позволяет передать логическое значение, которое будет переопределять поведение по умолчанию при сбоях конвейера.

### <a name="use-azure-logic-apps-for-more-complex-workflows"></a>Использование Azure Logic Apps для более сложных рабочих процессов

Azure Logic Apps поддерживает более сложные рабочие процессы и гораздо более широкую интеграцию, чем конвейеры Машинное обучение Azure. Дополнительные сведения см. в разделе [Активация запуска конвейера машинное обучение из приложения логики](how-to-trigger-published-pipeline.md) .

## <a name="view-your-scheduled-pipelines"></a>Просмотр запланированных конвейеров

В веб-браузере перейдите по адресу Машинное обучение Azure. В разделе **конечные точки** панели навигации выберите пункт **конечные точки конвейера**. Откроется список конвейеров, опубликованных в рабочей области.

![Страница конвейеров AML](./media/how-to-schedule-pipelines/scheduled-pipelines.png)

На этой странице можно просмотреть сводную информацию обо всех конвейерах в рабочей области: имена, описания, состояние и т. д. Выполните детализацию, щелкнув конвейер. На полученной странице имеются дополнительные сведения о конвейере, и вы можете выполнить детализацию для отдельных запусков.

## <a name="deactivate-the-pipeline"></a>Деактивация конвейера

Если у вас есть `Pipeline` публикация, но она не запланирована, ее можно отключить с помощью:

```python
pipeline = PublishedPipeline.get(ws, id=pipeline_id)
pipeline.disable()
```

Если расписание конвейера запланировано, сначала необходимо отменить его. Получите идентификатор расписания на портале или запустив:

```python
ss = Schedule.list(ws)
for s in ss:
    print(s)
```

После того, как `schedule_id` вы хотите отключить, выполните:

```python
def stop_by_schedule_id(ws, schedule_id):
    s = next(s for s in Schedule.list(ws) if s.id == schedule_id)
    s.disable()
    return s

stop_by_schedule_id(ws, schedule_id)
```

При повторном запуске `Schedule.list(ws)` необходимо получить пустой список.

## <a name="next-steps"></a>Дальнейшие шаги

В этой статье вы использовали пакет SDK для Машинное обучение Azure для Python, чтобы запланировать конвейер двумя разными способами. Одно расписание повторяется на основе затраченного времени. Другое расписание выполняется при изменении файла на указанном `Datastore` или в каталоге в этом хранилище. Вы узнали, как использовать портал для проверки конвейера и отдельных запусков. Наконец, вы узнали, как отключить расписание, чтобы остановить выполнение конвейера.

Дополнительные сведения можно найти в разделе

> [!div class="nextstepaction"]
> [Использование конвейеров Машинного обучения Azure для пакетной оценки](tutorial-pipeline-batch-scoring-classification.md)

* Дополнительные сведения о [конвейерах](concept-ml-pipelines.md)
* Дополнительные сведения об [исследовании машинное обучение Azure с помощью Jupyter](samples-notebooks.md)

