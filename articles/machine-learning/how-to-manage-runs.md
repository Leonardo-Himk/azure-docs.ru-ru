---
title: Запуск, отслеживание и отмена обучающих запусков в Python
titleSuffix: Azure Machine Learning
description: Узнайте, как начать, задать состояние, отмечать и упорядочивать эксперименты машинного обучения.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 01/09/2020
ms.openlocfilehash: d6dc2eeb572eeed17281677945c93067bbadee94
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628577"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Запуск, отслеживание и отмена обучающих запусков в Python
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

[Пакет SDK машинное обучение Azure для Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), [Машинное обучение CLI](reference-azure-machine-learning-cli.md)и [машинное обучение Azure Studio](https://ml.azure.com) предоставляют различные методы для мониторинга, Организации и управления запусками для обучения и экспериментирования.

В этой статье приведены примеры следующих задач.

* Мониторинг производительности выполнения.
* Отмена или сбой выполнения.
* Создание дочерних запусков.
* Теги и поиск запусков.

## <a name="prerequisites"></a>Предварительные условия

Вам потребуются следующие элементы:

* Подписка Azure. Если у вас еще нет подписки Azure, создайте бесплатную учетную запись, прежде чем начинать работу. Опробуйте [бесплатную или платную версию Машинного обучения Azure](https://aka.ms/AMLFree) уже сегодня.

* [Рабочая область машинное обучение Azure](how-to-manage-workspace.md).

* Машинное обучение Azure SDK для Python (версия 1.0.21 или более поздняя). Чтобы установить или обновить последнюю версию пакета SDK, см. статью [Установка или обновление пакета SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Чтобы проверить версию пакета SDK для Машинное обучение Azure, используйте следующий код:

    ```python
    print(azureml.core.VERSION)
    ```

* Расширение [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) и [CLI для машинное обучение Azure](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Запуск запуска и процесса ведения журнала

### <a name="using-the-sdk"></a>Использование пакета SDK

Настройте эксперимент, импортировав классы [рабочей области](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [эксперимент](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)и [скриптрунконфиг](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) из пакета [azureml. Core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) .

```python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Запустите запуск и процесс ведения журнала с помощью [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) метода.

```python
notebook_run = exp.start_logging()
notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>Использование интерфейса командной строки

Чтобы начать выполнение эксперимента, выполните следующие действия.

1. В оболочке или командной строке используйте Azure CLI для проверки подлинности в подписке Azure:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

1. Подключите конфигурацию рабочей области к папке, содержащей сценарий обучения. Замените `myworkspace` рабочей областью машинное обучение Azure. Замените `myresourcegroup` на группу ресурсов Azure, содержащую рабочую область:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Эта команда создает `.azureml` подкаталог, содержащий примеры файлов среды runconfig и conda. Он также содержит `config.json` файл, который используется для взаимодействия с рабочей областью машинное обучение Azure.

    Дополнительные сведения см. в разделе [AZ ML Folder Attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Чтобы начать выполнение, используйте следующую команду. При использовании этой команды укажите имя файла runconfig (текст до \*. runconfig, если вы ищете файловую систему) в параметре-c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > `az ml folder attach` Команда создала `.azureml` подкаталог, который содержит два примера файлов runconfig.
    >
    > При наличии скрипта Python, который создает объект конфигурации запуска программно, можно использовать [RunConfig. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) , чтобы сохранить его как файл RunConfig.
    >
    > Дополнительные примеры файлов runconfig см. в [https://github.com/MicrosoftDocs/pipelines-azureml/](https://github.com/MicrosoftDocs/pipelines-azureml/)разделе.

    Дополнительные сведения см. в разделе [AZ ML Run Submit-Script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

### <a name="using-azure-machine-learning-studio"></a>Использование Машинное обучение Azure Studio

Чтобы запустить отправку конвейера в конструкторе (Предварительная версия), выполните следующие действия.

1. Задайте целевой объект вычислений по умолчанию для конвейера.

1. Выберите **выполнить** в верхней части холста конвейера.

1. Выберите эксперимент, чтобы сгруппировать запуски конвейера.

## <a name="monitor-the-status-of-a-run"></a>Наблюдение за состоянием запуска

### <a name="using-the-sdk"></a>Использование пакета SDK

Получение состояния выполнения с помощью [`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) метода.

```python
print(notebook_run.get_status())
```

Чтобы получить идентификатор выполнения, время выполнения и дополнительные сведения о выполнении, используйте [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) метод.

```python
print(notebook_run.get_details())
```

Когда выполнение завершится успешно, используйте [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) метод, чтобы пометить его как завершенный.

```python
notebook_run.complete()
print(notebook_run.get_status())
```

Если вы используете шаблон `with...as` разработки Python, выполнение автоматически помечается как завершенное, если выполнение выходит за пределы области. Не нужно вручную помечать запуск как завершенный.

```python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print(notebook_run.get_status())

print(notebook_run.get_status())
```

### <a name="using-the-cli"></a>Использование интерфейса командной строки

1. Чтобы просмотреть список запусков для эксперимента, используйте следующую команду. Замените `experiment` на имя своего эксперимента:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Эта команда возвращает документ JSON, в котором перечисляются сведения о выполнении этого эксперимента.

    Дополнительные сведения см. в статье [AZ ML эксперимент List](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Чтобы просмотреть сведения о конкретном выполнении, используйте следующую команду. Замените `runid` на идентификатор запуска:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Эта команда возвращает документ JSON, в котором перечисляются сведения о выполнении.

    Дополнительные сведения см. в разделе [AZ ML Run показ](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).


### <a name="using-azure-machine-learning-studio"></a>Использование Машинное обучение Azure Studio

Для просмотра числа активных запусков для эксперимента в студии.

1. Перейдите к разделу **эксперименты** ... 

1. Выберите эксперимент.

    На странице эксперимента можно увидеть количество активных целевых объектов вычислений и длительность каждого запуска. 

1. Выберите конкретный номер запуска.

1. На вкладке **журналы** можно найти журналы диагностики и ошибок для выполнения конвейера.


## <a name="cancel-or-fail-runs"></a>Отмена или неудача выполнения

Если вы заметили ошибку или если выполнение занимает слишком много времени, можно отменить запуск.

### <a name="using-the-sdk"></a>Использование пакета SDK

Чтобы отменить запуск с помощью пакета SDK, используйте [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) метод:

```python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_script_run = exp.submit(run_config)
print(local_script_run.get_status())

local_script_run.cancel()
print(local_script_run.get_status())
```

Если выполнение завершается, но содержит ошибку (например, был использован неверный сценарий обучения), можно использовать [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) метод, чтобы пометить его как неудачный.

```python
local_script_run = exp.submit(run_config)
local_script_run.fail()
print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>Использование интерфейса командной строки

Чтобы отменить запуск с помощью интерфейса командной строки, используйте следующую команду. Замените `runid` на идентификатор запуска

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Дополнительные сведения см. в статье [AZ ML Run отмена](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

### <a name="using-azure-machine-learning-studio"></a>Использование Машинное обучение Azure Studio

Чтобы отменить запуск в студии, выполните следующие действия.

1. Перейдите к выполняющемуся конвейеру в разделе **эксперименты** или **конвейеры** . 

1. Выберите номер выполнения конвейера, который нужно отменить.

1. На панели инструментов нажмите **кнопку Отмена** .


## <a name="create-child-runs"></a>Создание дочерних запусков

Создайте дочерние выполнения для объединения связанных запусков, например для различных итераций настройки параметров.

> [!NOTE]
> Дочерние запуски можно создать только с помощью пакета SDK.

В этом примере кода используется `hello_with_children.py` скрипт для создания пакета из пяти дочерних запусков из отправленного запуска с помощью [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) метода:

```python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> При выходе из области действия дочерние тесты автоматически помечаются как завершенные.

Чтобы эффективно создать множество дочерних запусков, используйте [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) метод. Поскольку каждое создание приводит к сетевому вызову, создание пакета выполняется более эффективно, чем создание их по одному.

### <a name="submit-child-runs"></a>Отправить дочерние выполнения

Дочерние запуски также могут быть отправлены из родительского запуска. Это позволяет создавать иерархии родительских и дочерних запусков. 

Может потребоваться, чтобы ваш ребенок мог использовать конфигурацию запуска, отличную от конфигурации родительского запуска. Например, вы можете использовать менее мощную конфигурацию на основе ЦП для родительского элемента, а также использовать конфигурации на основе GPU для своих детей. Другим распространенным желанием является передача каждого дочернего различных аргументов и данных. Чтобы настроить дочерний запуск, передайте `RunConfiguration` объект в конструктор дочернего элемента. `ScriptRunConfig` Этот пример кода, который бы был частью скрипта родительского `ScriptRunConfig` объекта:

- Создает `RunConfiguration` извлечение именованного ресурса вычислений`"gpu-compute"`
- Выполняет перебор различных значений аргументов для передачи дочерним `ScriptRunConfig` объектам
- Создает и отправляет новый дочерний запуск с использованием настраиваемого ресурса вычислений и аргумента.
- Блокируется до завершения всех дочерних запусков

```python
# parent.py
# This script controls the launching of child scripts
from azureml.core import Run, ScriptRunConfig, RunConfiguration

run_config_for_aml_compute = RunConfiguration()
run_config_for_aml_compute.target = "gpu-compute"
run_config_for_aml_compute.environment.docker.enabled = True 

run = Run.get_context()

child_args = ['Apple', 'Banana', 'Orange']
for arg in child_args: 
    run.log('Status', f'Launching {arg}')
    child_config = ScriptRunConfig(source_directory=".", script='child.py', arguments=['--fruit', arg], run_config = run_config_for_aml_compute)
    # Starts the run asynchronously
    run.submit_child(child_config)

# Experiment will "complete" successfully at this point. 
# Instead of returning immediately, block until child runs complete

for child in run.get_children():
    child.wait_for_completion()
```

Чтобы создать множество дочерних запусков с одинаковыми конфигурациями, аргументами и входными [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) данными, используйте метод. Поскольку каждое создание приводит к сетевому вызову, создание пакета выполняется более эффективно, чем создание их по одному.

В дочернем выполнении можно просмотреть идентификатор родительского запуска:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Дочерние выполнения запросов

Чтобы запросить дочерние выполнения определенного родителя, используйте [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) метод. ``recursive = True`` Аргумент позволяет запрашивать вложенное дерево дочерних элементов и внуками.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Теги и поиск запусков

В Машинное обучение Azure можно использовать свойства и теги для упорядочения и запроса своих запусков для получения важной информации.

### <a name="add-properties-and-tags"></a>Добавление свойств и тегов

#### <a name="using-the-sdk"></a>Использование пакета SDK

Чтобы добавить метаданные для поиска в запуски, используйте [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) метод. Например, следующий код добавляет `"author"` свойство в Run:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Свойства являются неизменяемыми, поэтому они создают постоянную запись для целей аудита. В следующем примере кода возникает ошибка, так как мы уже добавили `"azureml-user"` в качестве значения `"author"` свойства в приведенном выше коде:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

В отличие от свойств, теги являются изменяемыми. Чтобы добавить доступную для поиска и осмысленную информацию для потребителей вашего эксперимента [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) , используйте метод.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Можно также добавить простые строковые Теги. Если эти теги отображаются в словаре тегов в качестве ключей, они имеют значение `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>Использование интерфейса командной строки

> [!NOTE]
> С помощью интерфейса командной строки можно добавлять или обновлять только теги.

Чтобы добавить или обновить тег, используйте следующую команду:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Дополнительные сведения см. в разделе [AZ ML Run Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Свойства и Теги запроса

Вы можете запросить запуски в эксперименте, чтобы получить список запусков, соответствующих указанным свойствам и тегам.

#### <a name="using-the-sdk"></a>Использование пакета SDK

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>Использование интерфейса командной строки

Azure CLI поддерживает запросы [JMESPath](http://jmespath.org) , которые можно использовать для фильтрации запусков на основе свойств и тегов. Чтобы использовать запрос JMESPath с Azure CLI, укажите его с помощью `--query` параметра. В следующих примерах показаны основные запросы, использующие свойства и теги.

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Дополнительные сведения о запросах Azure CLI результатов см. в разделе [запрос Azure CLI команды Output](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

### <a name="using-azure-machine-learning-studio"></a>Использование Машинное обучение Azure Studio

1. Перейдите в раздел **конвейеры** .

1. Используйте панель поиска для фильтрации конвейеров с помощью тегов, описаний, имен экспериментов и имени отправителя.

## <a name="example-notebooks"></a>Примеры записных книжек

В следующих записных книжках демонстрируются концепции, описанные в этой статье.

* Дополнительные сведения об API ведения журнала см. в [записной книжке API для ведения журнала](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Дополнительные сведения об управлении запусками с помощью пакета SDK для Машинное обучение Azure см. в разделе [Управление запуском записной книжки](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о том, как регистрировать метрики для экспериментов, см. в статье [метрики журнала во время учебных запусков](how-to-track-experiments.md).
* Сведения о мониторинге ресурсов и журналов с Машинное обучение Azure см. в разделе [мониторинг машинное обучение Azure](monitor-azure-machine-learning.md).