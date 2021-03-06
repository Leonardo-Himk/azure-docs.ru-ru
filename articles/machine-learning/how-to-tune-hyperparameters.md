---
title: Настройка гиперпараметров модели
titleSuffix: Azure Machine Learning
description: Эффективная настройка параметров для модели глубокого обучения и машинного обучения с помощью Машинное обучение Azure. Вы узнаете, как определить область поиска параметров, указать основную метрику для оптимизации и раннего завершения плохо выполняемых запусков.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/30/2020
ms.custom: seodec18
ms.openlocfilehash: a58ea58ebf6fdc7d8521d204ac42fcbadeca39a4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82189306"
---
# <a name="tune-hyperparameters-for-your-model-with-azure-machine-learning"></a>Настройка параметров для модели с помощью Машинное обучение Azure
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Эффективная настройка параметров для модели с помощью Машинное обучение Azure.  Настройка гиперпараметров включает следующие шаги:

* определение пространства поиска параметров;
* Указание основной метрики для оптимизации  
* установка критерия досрочного завершения для запусков с низкой эффективностью;
* Выделение ресурсов для настройки гиперпараметров
* Запустить эксперимент с использованием представленной выше конфигурации.
* визуализация учебных запусков;
* выбор наиболее эффективной конфигурации для модели.

## <a name="what-are-hyperparameters"></a>Сведения о гиперпараметрах

Гиперпараметры — это настраиваемые параметры для обучения модели, которая самостоятельно регулирует процесс обучения. Например, перед началом обучения модели в глубокой нейронной сети вы можете выбрать количество скрытых уровней в сети и количество узлов на каждом уровне. Эти значения обычно не изменяются во время процесса обучения.

В сценариях глубокого и машинного обучения эффективность модели в значительной степени зависит от выбранных значений гиперпараметров. Цель изучения гиперпараметров — найти наиболее эффективную конфигурацию гиперпараметров среди множества вариантов. Как правило, процесс изучения гиперпараметров часто выполняется вручную, учитывая, что пространство поиска очень широкое и оценка каждой конфигурации может быть дорогостоящей.

Служба "Машинное обучение Azure" позволяет эффективно автоматизировать исследование гиперпараметров, обеспечивая существенную экономию времени и ресурсов. Вы просто указываете диапазон значений гиперпараметров и максимальное количество учебных запусков. Система автоматически начинает несколько одновременных запусков с разными конфигурациями параметров и выбирает из них наиболее эффективную по указанным вами метрикам. Обучающие прогоны с низкой эффективностью автоматически заканчиваются досрочно, снижая потери вычислительных ресурсов. Вместо этого эти ресурсы используются для изучения других конфигураций гиперпараметров.


## <a name="define-search-space"></a>Определение пространства поиска

Гиперпараметры настраиваются автоматически путем анализа по диапазону значений, определенному для каждого из гиперпараметров.

### <a name="types-of-hyperparameters"></a>Типы гиперпараметров

Каждый параметр может быть дискретным или непрерывным и иметь распределение значений, описываемых [выражением параметра](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.parameter_expressions?view=azure-ml-py).

#### <a name="discrete-hyperparameters"></a>Дискретные гиперпараметры 

Дискретные гиперпараметры определяются как `choice` в дискретных значениях. Параметр `choice` может принимать следующие значения:

* одно или несколько значений, разделенных запятыми;
* объект `range`;
* любой произвольный объект `list`.


```Python
    {
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

В этом случае `batch_size` принимает значение из списка [16, 32, 64, 128], а `number_of_hidden_layers` — из списка [1, 2, 3, 4].

Расширенные дискретные гиперпараметры также могут быть заданы с использованием распределения. Поддерживаются следующие распределения:

* `quniform(low, high, q)` — возвращает значение типа round(uniform(low, high) / q) * q.
* `qloguniform(low, high, q)` — возвращает значение типа round(exp(uniform(low, high)) / q) * q.
* `qnormal(mu, sigma, q)` — возвращает значение типа round(normal(mu, sigma) / q) * q.
* `qlognormal(mu, sigma, q)` — возвращает значение типа round(exp(normal(mu, sigma)) / q) * q.

#### <a name="continuous-hyperparameters"></a>Непрерывные гиперпараметры 

Непрерывные гиперпараметры определяются как распределение по непрерывному диапазону значений. Поддерживаются такие распределения:

* `uniform(low, high)` — возвращает значение, равномерно распределенное между верхней и нижней границами.
* `loguniform(low, high)` — возвращает значение, полученное в соответствии с exp(uniform(low, high)), так что логарифм возвращаемого значения равномерно распределен.
* `normal(mu, sigma)` — возвращает реальное значение, которое обычно распределяется со средним значением mu и стандартным отклонением sigma.
* `lognormal(mu, sigma)` — возвращает значение, полученное в соответствии с exp(normal(mu, sigma)), так что логарифм возвращаемого значения нормально распределен.

Ниже приведен пример определения пространства параметров:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Этот код определяет пространство поиска с двумя параметрами: `learning_rate` и `keep_probability`. `learning_rate` имеет нормальное распределение со средним значением 10 и стандартным отклонением 3. `keep_probability` имеет равномерное распределение с минимальным значением 0,05 и максимальным значением 0,1.

### <a name="sampling-the-hyperparameter-space"></a>Выборка пространства гиперпараметров

Вы также можете указать метод выборки параметров для определения пространства гиперпараметров. Машинное обучение Azure поддерживает случайную выборку, выборку по сетке и Байеса выборку.

#### <a name="picking-a-sampling-method"></a>Подбор метода выборки

* Выборка по сетке может использоваться, если пространство с параметрами может быть определено как выборка среди дискретных значений и если имеется достаточный бюджет для поиска всех значений в заданной области поиска. Кроме того, можно использовать автоматическое преждевременное завершение непроизводительных запусков, что сокращает расход ресурсов.
* Случайная выборка позволяет пространству параметров включать как дискретные, так и непрерывные параметры. На практике это дает хорошие результаты в большинстве случаев, а также позволяет использовать автоматическое раннее завершение плохо выполняющихся запусков. Некоторые пользователи выполняют исходный Поиск с использованием случайной выборки, а затем итеративно обвышают область поиска для улучшения результатов.
* Выборка Байеса использует знания предыдущих выборок при выборе значений параметров, что эффективно попытается улучшить сообщаемую основную метрику. Выборка Байеса рекомендуется при наличии достаточного бюджета для изучения места в параметрах. для получения наилучших результатов при использовании Байеса выборки рекомендуется использовать максимальное число запусков, большее или равное 20, чем число настраиваемых параметров. Обратите внимание, что выборка Байеса в настоящее время не поддерживает политику раннего завершения.

#### <a name="random-sampling"></a>Случайная выборка

При случайной выборке значения гиперпараметров выбираются случайным образом из определенного пространства поиска. [Случайная выборка](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.randomparametersampling?view=azure-ml-py) позволяет области поиска включать как дискретные, так и непрерывные параметры.

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Решетчатая выборка

[Выборка по сетке](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.gridparametersampling?view=azure-ml-py) выполняет простой поиск по всем возможным значениям в заданной области поиска. Ее можно использовать только с гиперпараметрами, указанными с помощью `choice`. Например, следующее пространство имеет в общей сложности шесть образцов:

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Байесовская выборка

[Выборка Байеса](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.bayesianparametersampling?view=azure-ml-py) основана на алгоритме оптимизации Байеса и делает интеллектуальный выбор значений параметров в примере ниже. Образец выбирается на основе результатов для предыдущих образцов так, чтобы новый образец улучшал основную оцениваемую метрику.

При использовании байесовской выборки количество одновременных запусков влияет на эффективность процесса настройки. Обычно меньшее число параллельных запусков улучшает конвергенцию выборки, так как низкая степень параллелизма увеличивает число запусков, оптимизированных по результатам прошлых запусков.

Выборка Байеса поддерживает `choice` `uniform`только `quniform` распределения по области поиска.

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Байесовская выборка сейчас не поддерживает политику досрочного завершения (см. раздел[Указание политики досрочного завершения](#specify-early-termination-policy)). При использовании байесовской выборки параметров установите `early_termination_policy = None` или оставьте отключенным параметр `early_termination_policy`.

<a name='specify-primary-metric-to-optimize'/>

## <a name="specify-primary-metric"></a>Настройка основной метрики

Укажите [основную метрику](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.primarymetricgoal?view=azure-ml-py) , которую необходимо оптимизировать для эксперимента настройки параметров. Для основной метрики оценивается каждый учебный запуск. Запуски с низкой эффективностью (у которых основная метрика не соответствует критериям, установленным политикой досрочного завершения) прекращаются. Помимо названия основной метрики следует указать цель оптимизации — максимизация или минимизация основной метрики.

* `primary_metric_name`: имя основной метрики для оптимизации. Имя основной метрики должно точно соответствовать имени метрики, зарегистрированной в сценарии обучения. См. сведения в разделе [Ведение журнала метрик для настройки гиперпараметров](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: это может быть `PrimaryMetricGoal.MAXIMIZE` или `PrimaryMetricGoal.MINIMIZE`. Это свойство определяет, что будет выполняться при оценке прогонов: максимизация или минимизация основной метрики. 

```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```

Запуски оптимизируются для максимизации точности.  Не забудьте, что это значение нужно сохранять в скрипте обучения.

<a name='log-metrics-for-hyperparameter-tuning'/>

### <a name="log-metrics-for-hyperparameter-tuning"></a>Ведение журнала метрик для настройки гиперпараметров

Скрипт обучения для модели должен сохранять значения соответствующих метрик в процессе обучения модели. В конфигурации для настройки гиперпараметров вы указываете основную метрику, по которой будет оцениваться производительность выполнения. (См. раздел [Указание основной метрики для оптимизации](#specify-primary-metric-to-optimize).)  В сценарии обучения необходимо зарегистрировать эту метрику, чтобы она была доступна процессу настройки параметров.

Чтобы сохранять значения метрики из скрипта обучения, примените следующий фрагмент кода:

```Python
from azureml.core.run import Run
run_logger = Run.get_context()
run_logger.log("accuracy", float(val_accuracy))
```

Скрипт обучения вычисляет значение `val_accuracy` и сохраняет его как значение "точности", которая настроена в качестве основной метрики. Каждое записанное значение метрики поступает на обработку в службу настройки гиперпараметров. Разработчик модели должен определить, как часто сообщать об этой метрике.

<a name='specify-early-termination-policy'/>

## <a name="specify-early-termination-policy"></a><a name="early-termination"></a>Укажите политику раннего завершения

Политика досрочного завершения позволяет автоматически прекращать запуски с низкой эффективностью. Прекращение запусков позволяет снизить потери ресурсов и перераспределить их для изучения других конфигураций параметров.

Для политики досрочного завершения можно настроить следующие параметры, которые управляют применением этой политики.

* `evaluation_interval`: частота применения политики. Каждый раз, когда сценарий обучения регистрирует основную метрику, это считается одним интервалом. Таким образом, `evaluation_interval` со значением 1 будет применять политику каждый раз, когда сценарий обучения сообщает основную метрику. Таким образом, `evaluation_interval` со значением 2 будет применять политику через раз, когда сценарий обучения сообщает основную метрику. Если значение для `evaluation_interval` не указано, по умолчанию используется 1.
* `delay_evaluation`: задерживает первую оценку политики для определенного количества интервалов. Это необязательный параметр. Он позволяет настроить минимальное количество интервалов для проверки каждой конфигурации, чтобы избежать преждевременного завершения учебных запусков. Если этот параметр указан, политика применяет каждое кратное значение evaluation_interval, которое больше или равно delay_evaluation.

Машинное обучение Azure поддерживает следующие политики раннего завершения.

### <a name="bandit-policy"></a>Политика Bandit

[Бандит](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.banditpolicy?view=azure-ml-py#definition) — это политика завершения, основанная на значении коэффициента резервного времени и времени временного резерва и интервале оценки. Эта политика досрочно завершает все запуски, где основная метрика не находится в пределах указанного коэффициента или количества резерва времени в отношении учебного запуска с самой высокой эффективностью. Эта политика принимает следующие параметры конфигурации:

* `slack_factor` или `slack_amount`: резерв времени, допустимый в отношении обучающего прогона с самой высокой эффективностью. `slack_factor` задает допустимый резерв времени как коэффициент. `slack_amount` указывает допустимый резерв времени как абсолютную величину, а не коэффициент.

    Например, рассмотрим политику Bandit, применяемую в интервале 10. Предположим, что наиболее эффективный прогон в интервале 10 сообщил основную метрику 0,8 с целью ее максимизации. Если политика была указана с использованием `slack_factor` 0,2, любые обучающие прогоны, лучшая метрика которых в интервале 10 меньше 0,66 (0,8/(1+`slack_factor`)), будут завершены. Если вместо этого политика была указана с использованием `slack_amount` 0,2, любые обучающие прогоны, лучшая метрика которых в интервале 10 меньше 0,6 (0,8 – `slack_amount`), будут завершены.
* `evaluation_interval`: частота применения политики (необязательный параметр).
* `delay_evaluation`: задерживает первую оценку политики для определенного количества интервалов (необязательный параметр).


```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

В этом примере политика раннего завершения применяется в каждом интервале, когда указываются метрики, начиная с оценочного интервала 5. Любой прогон, лучшая метрика которого меньше (1/(1+0,1)) или соответствует 91 % от наилучшего прогона, будет завершен.

### <a name="median-stopping-policy"></a>Политика медианной остановки

[Средняя остановка](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.medianstoppingpolicy?view=azure-ml-py) — это политика раннего завершения, основанная на средних показателях основных метрик, сообщаемых выполнениями. Эта политика вычисляет средние значения среди всех обучающих прогонов и завершает прогоны, эффективность которых хуже медианы средних показателей. Она принимает следующие параметры конфигурации:
* `evaluation_interval`: частота применения политики (необязательный параметр).
* `delay_evaluation`: задерживает первую оценку политики для определенного количества интервалов (необязательный параметр).


```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

В этом примере политика раннего завершения применяется в каждом интервале, начиная с оценочного интервала 5. Прогон будет завершен на интервале 5, если его лучшая основная метрика хуже, чем медиана средних показателей за интервалы 1:5 во всех обучающих прогонах.

### <a name="truncation-selection-policy"></a>Политика выбора усечения

[Выбор усечения](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.truncationselectionpolicy?view=azure-ml-py) отменяет заданный процент наименьших выполнений в каждом интервале оценки. Прогоны сравниваются в зависимости от их эффективности по основной метрике, и X % прогонов с наименьшей эффектностью завершаются. Эта политика принимает следующие параметры конфигурации:

* `truncation_percentage`: процент прогонов с наименьшей эффективностью, которые будут завершены в каждом интервале оценки. Укажите целое число от 1 до 99.
* `evaluation_interval`: частота применения политики (необязательный параметр).
* `delay_evaluation`: задерживает первую оценку политики для определенного количества интервалов (необязательный параметр).


```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

В этом примере политика раннего завершения применяется в каждом интервале, начиная с оценочного интервала 5. Выполнение будет прервано с интервала 5, если его производительность в интервале 5 превышает 20% производительности всех запусков с интервалом 5.

### <a name="no-termination-policy"></a>Политика без завершения

Если вы хотите, чтобы все обучающие прогоны выполнялись полностью, установите для политики значение "Нет". Политика досрочного завершения выполняться не будет.

```Python
policy=None
```

### <a name="default-policy"></a>Политика по умолчанию

Если политика не указана, служба настройки параметров может разрешить выполнение всех обучающих запусков до завершения.

### <a name="picking-an-early-termination-policy"></a>Выбор политики раннего завершения

* Если требуется консервативная политика, которая обеспечит экономию без прерывания запланированных заданий, можно воспользоваться политикой медианной остановки (Median Stopping Policy) со значениями `evaluation_interval` 1 и `delay_evaluation` 5. Это консервативные настройки, которые могут обеспечить экономию приблизительно 25–35 % без потерь по основной метрике (на основе наших оценочных данных).
* Если вы ищете более агрессивную экономию от раннего завершения, вы можете использовать бандит политику с более строгой (меньшей) допустимой временной резервной политикой или политики выбора усечения с более крупным процентом усечения.

## <a name="allocate-resources"></a>Выделение ресурсов

Чтобы ограничить затраты на эксперимент по настройке гиперпараметров, укажите максимально допустимое общее число учебных запусков.  При желании укажите максимальный срок действия для результатов эксперимента по настройке гиперпараметров.

* `max_total_runs`: максимальное общее количество обучающих прогонов, которые будут созданы. Это верхняя граница. Количество запусков может оказаться меньше, например, если пространство гиперпараметров ограничено и не позволяет извлечь достаточное число образцов. Это значение должно быть в диапазоне от 1 до 1000.
* `max_duration_minutes`: максимальная продолжительность эксперимента по настройке гиперпараметров в минутах. Это необязательный параметр. Если он присутствует, автоматически отменяются любые запуски, которые еще не окончены по истечении этого времени.

>[!NOTE] 
>Если указаны одновременно параметры `max_total_runs` и `max_duration_minutes`, эксперимент по настройке гиперпараметров прекращается при достижении первого из двух пороговых значений.

Кроме того, укажите максимальное количество одновременно выполняемых учебных запусков при настройке гиперпараметров.

* `max_concurrent_runs`: максимальное количество запусков, выполняемых одновременно в любой момент. Если это значение не указано, все `max_total_runs` будут запущены параллельно. Если этот параметр указывается, его значение должно быть числом в диапазоне от 1 до 100.

>[!NOTE] 
>Количество параллельных прогонов зависит от ресурсов, доступных в заданном целевом объекте вычисления. Следовательно, вам нужно обеспечить в целевом объекте вычислений наличие ресурсов для требуемого уровня параллелизма.

Выделение ресурсов для настройки гиперпараметров:

```Python
max_total_runs=20,
max_concurrent_runs=4
```

Этот код настраивает эксперимент настройки параметров, чтобы использовать максимум 20 запусков, одновременно выполняющих четыре конфигурации.

## <a name="configure-experiment"></a>Настройка эксперимента

[Настройте эксперимент настройки параметров](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverunconfig?view=azure-ml-py) с помощью определенного пространства поиска параметров, политики раннего завершения, основной метрики и выделения ресурсов из приведенных выше разделов. Также нужно предоставить средство оценки (`estimator`), которое будет вызываться с отобранными гиперпараметрами. `estimator` описывает выполняемый сценарий обучения, ресурсы на каждое задание (один или несколько GPU) и целевой объект вычисления. Так как параллелизм для эксперимента по настройке гиперпараметров ограничивается объемом доступных ресурсов, обеспечьте в целевом объекте вычисления, который вы указали в `estimator`, достаточное количество ресурсов для желаемого уровня параллелизма. (Дополнительные сведения об инструментах оценки см. в статье [Как обучать модели машинного обучения с помощью Машинного обучения Azure](how-to-train-ml-models.md)).

Конфигурация эксперимента по настройке гиперпараметров:

```Python
from azureml.train.hyperdrive import HyperDriveConfig
hyperdrive_run_config = HyperDriveConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-experiment"></a>Отправка эксперимента

Определив конфигурацию настройки параметров, [отправьте эксперимент](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment%28class%29?view=azure-ml-py#submit-config--tags-none----kwargs-):

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

`experiment_name`— Это имя, которое назначается эксперименту настройки параметров и `workspace` является рабочей областью, в которой вы хотите создать эксперимент (Дополнительные сведения о экспериментах см. в разделе [как работает машинное обучение Azure?](concept-azure-machine-learning-architecture.md))

## <a name="warm-start-your-hyperparameter-tuning-experiment-optional"></a>Горячий запуск эксперимента по настройке параметров (необязательно)

Часто для поиска лучших значений параметров в модели может использоваться итеративный процесс, требующий нескольких запусков настройки, которые изучены в предыдущих запусках настройки параметров. Повторное использование набора знаний из этих предыдущих запусков ускоряет процесс настройки параметров, уменьшая затраты на настройку модели и потенциально улучшая основную метрику результирующей модели. При горячем запуске эксперимента по настройке параметров с Байеса выборки, пробные версии из предыдущего запуска будут использоваться в качестве предыдущих знаний для интеллектуального выбора новых примеров, чтобы улучшить основную метрику. Кроме того, при использовании случайной или выборки с сеткой все решения раннего завершения будут использовать метрики из предыдущих запусков для определения плохо выполняющихся обучающих курсов. 

Машинное обучение Azure позволяет горячий запуск настройки параметров, используя знания из до 5 ранее завершенных или отмененных родительских запусков настройки параметров. С помощью этого фрагмента кода можно указать список родительских запусков, для которых требуется горячий запуск.

```Python
from azureml.train.hyperdrive import HyperDriveRun

warmstart_parent_1 = HyperDriveRun(experiment, "warmstart_parent_run_ID_1")
warmstart_parent_2 = HyperDriveRun(experiment, "warmstart_parent_run_ID_2")
warmstart_parents_to_resume_from = [warmstart_parent_1, warmstart_parent_2]
```

Кроме того, могут возникнуть ситуации, когда отдельные обучающие циклы настройки параметров перезапускаются из-за ограничений бюджета или сбоев по другим причинам. Теперь можно возобновить такие отдельные учебные запуски из последней контрольной точки (при условии, что сценарий обучения обрабатывает контрольные точки). При возобновлении отдельного учебного запуска будет использоваться та же конфигурация параметров и для подключения к папке выходных данных, используемой для этого запуска. Сценарий обучения должен принять `resume-from` аргумент, который содержит файлы контрольной точки или модели, из которых будет возобновлена работа по обучению. Вы можете возобновить отдельные запуски обучения, используя следующий фрагмент кода:

```Python
from azureml.core.run import Run

resume_child_run_1 = Run(experiment, "resume_child_run_ID_1")
resume_child_run_2 = Run(experiment, "resume_child_run_ID_2")
child_runs_to_resume = [resume_child_run_1, resume_child_run_2]
```

Вы можете настроить эксперимент по настройке параметров в качестве горячего запуска из предыдущего эксперимента или возобновления отдельных запусков обучения с `resume_from` помощью `resume_child_runs` необязательных параметров и в конфигурации.

```Python
from azureml.train.hyperdrive import HyperDriveConfig

hyperdrive_run_config = HyperDriveConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          resume_from=warmstart_parents_to_resume_from, 
                          resume_child_runs=child_runs_to_resume,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="visualize-experiment"></a>Визуализация эксперимента

Пакет SDK для Машинное обучение Azure предоставляет [мини-приложение записной книжки](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets.rundetails?view=azure-ml-py) , которое визуализирует ход выполнения обучения. Следующий фрагмент кода визуализирует все запуски по настройке гиперпараметров в одном отображении, создаваемом в записной книжке Jupyter:

```Python
from azureml.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Этот код отображает таблицу со сведениями об учебных запусках для каждой из конфигураций гиперпараметров.

![Таблица настройки гиперпараметров](./media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Вы также можете визуализировать эффективность каждого прогона в ходе обучения. 

![График настройки гиперпараметров](./media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Кроме того, вы можете визуально оценить соотношение между эффективностью и значениями отдельных гиперпараметров, используя график параллельных координат. 

[![параллельные координаты для настройки гиперпараметров](./media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)](media/how-to-tune-hyperparameters/hyperparameter-tuning-parallel-coordinates-expanded.png)

Вы также можете визуализировать все запуски для настройки гиперпараметров на веб-портале Azure. Дополнительную информацию о просмотре сведений об эксперименте на веб-портале см. в [этой статье](how-to-track-experiments.md#view-the-experiment-in-the-web-portal).

## <a name="find-the-best-model"></a>Поиск наиболее эффективной модели

После завершения настройки параметров, [выполнив настройку, найдите наиболее подходящую конфигурацию](/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverun?view=azure-ml-py#get-best-run-by-primary-metric-include-failed-true--include-canceled-true--include-resume-from-runs-true-----typing-union-azureml-core-run-run--nonetype-) и соответствующие значения параметров.

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Пример записной книжки
См. Дополнительные сведения в записных книжках в этой папке:
* [how-to-use-azureml/training-with-deep-learning](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Дальнейшие действия
* [Руководство по отслеживанию эксперимента](how-to-track-experiments.md)
* [Развертывание обученной модели](how-to-deploy-and-where.md)
