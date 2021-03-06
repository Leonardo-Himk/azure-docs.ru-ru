---
title: Известные проблемы и устранение неполадок
titleSuffix: Azure Machine Learning
description: Получите список известных проблем, обходных путей и устранение неполадок для Машинное обучение Azure.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 93015da810f163a48529704e69e1747ac1aec401
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2020
ms.locfileid: "82889393"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Известные проблемы и устранение неполадок Машинное обучение Azure

Эта статья поможет вам найти и исправить ошибки или сбои, которые могут возникнуть при использовании Машинное обучение Azure.

## <a name="diagnostic-logs"></a>Журналы диагностики

Иногда при обращении за помощью полезно предоставить диагностические сведения. Для просмотра некоторых журналов выполните следующие действия. 
1. Посетите [машинное обучение Azure Studio](https://ml.azure.com). 
1. В левой части выберите **эксперимент** . 
1. Выберите эксперимент.
1. Выберите Запуск.
1. В верхней части выберите **выходные данные и журналы**.

> [!NOTE]
> Машинное обучение Azure записывает сведения из различных источников во время обучения, например Аутомл или контейнер DOCKER, который запускает задание обучения. Многие из этих журналов не документированы. Если возникли проблемы и вы можете обратиться в службу поддержки Майкрософт, они смогут использовать эти журналы во время устранения неполадок.


## <a name="resource-quotas"></a>Квоты ресурсов

Дополнительные сведения о квотах ресурсов в службе "Машинное обучение Azure" см. [здесь](how-to-manage-quotas.md).

## <a name="installation-and-import"></a>Установка и импорт
                           
* **Установка PIP: не гарантируется совместимость зависимостей с однострочной установкой**: 

   Это известное ограничение PIP, так как в нем отсутствует действующий сопоставитель зависимостей при установке в виде одной строки. Первая уникальная зависимость — это единственная из них. 

   В следующем коде `azure-ml-datadrift` и `azureml-train-automl` устанавливаются с помощью одной строки. 
     ```
       pip install azure-ml-datadrift, azureml-train-automl
     ```
   Для этого примера предположим, что `azure-ml-datadrift` требуется версия > 1,0 и `azureml-train-automl` требуется версия < 1,2. Если последняя версия `azure-ml-datadrift` — 1,3, то оба `azureml-train-automl` пакета обновляются до 1,3, независимо от требований к пакету для более старой версии. 

   Чтобы обеспечить установку соответствующих версий для пакетов, установите с помощью нескольких строк, как в следующем коде. Order не является проблемой, так как PIP явным образом понижается в рамках вызова следующей строки. И поэтому применяются соответствующие зависимости версий.
    
     ```
        pip install azure-ml-datadrift
        pip install azureml-train-automl 
     ```
     
* **Объяснение пакет не гуаратид для установки при установке azureml-Training-аутомл-Client:** 
   
   При запуске удаленной аутомл с описанием модели Enabled вы увидите сообщение об ошибке "" установите пакет azureml-объяснить-Model для объяснения модели ". Это известная проблема. в качестве обходного решения выполните одно из следующих действий.
  
  1. Установка azureml-объяснить-Model на локальном компьютере.
   ```
      pip install azureml-explain-model
   ```
  2. Полностью отключите функцию объяснения, передав model_explainability = false в конфигурации аутомл.
   ```
      automl_config = AutoMLConfig(task = 'classification',
                             path = '.',
                             debug_log = 'automated_ml_errors.log',
                             compute_target = compute_target,
                             run_configuration = aml_run_config,
                             featurization = 'auto',
                             model_explainability=False,
                             training_data = prepped_data,
                             label_column_name = 'Survived',
                             **automl_settings)
    ``` 
    
* **Ошибки Panda: обычно встречаются во время Аутомл эксперимента:**
   
   При ручной настройке енвиронмнет с помощью PIP вы увидите ошибки атрибутов (особенно от PANDAS) из-за установки неподдерживаемых версий пакета. Чтобы предотвратить такие ошибки, [установите пакет SDK для аутомл с помощью automl_setup. cmd](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/README.md):
   
    1. Откройте запрос Anaconda и клонировать репозиторий GitHub для набора примеров записных книжек.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```
    
    2. Перейдите в папку с инструкциями azureml/автоматизированного машинного обучения, где были извлечены примеры записных книжек, а затем запустите:
    
    ```bash
    automl_setup
    ```
  
* **Сообщение об ошибке: не удается удалить "PyYAML"**

    Машинное обучение Azure SDK для Python: Пиямл является `distutils` установленным проектом. Поэтому невозможно точно определить, какие файлы принадлежат ему в случае частичного удаления. Чтобы продолжить установку SDK игнорируя эту ошибку, используйте:
    
    ```Python
    pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
    ```

* **Ошибка при установке пакетов в модулях**

    Сбой установки пакета SDK Машинное обучение Azure на Azure Databricks при установке дополнительных пакетов. Некоторые пакеты, такие как `psutil`, могут приводить к конфликтам. Чтобы избежать ошибок установки, установите пакеты, зафиксировать версию библиотеки. Эта проблема связана с модулями связи, а не с пакетом SDK для Машинное обучение Azure. Эта проблема также может возникнуть и в других библиотеках. Пример
    
    ```python
    psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
    ```

    Кроме того, можно использовать скрипты init, если возникают проблемы с установкой библиотек Python. Этот подход официально не поддерживается. Дополнительные сведения см. в разделе [сценарии инициализации с областью действия кластера](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

* **Ошибка импорта блоков кирпича: не удается импортировать имя "тимеделта" из "Pandas. _libs. тслибс"**: Если эта ошибка возникает при использовании автоматического машинного обучения, выполните в записной книжке две следующие строки:
    ```
    %sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
    %sh /databricks/python/bin/pip install pandas==0.23.4
    ```

* **Ошибка импорта блоков памяти: нет модуля с именем "Pandas. Core. indexes"**: Если при использовании автоматического машинного обучения возникает эта ошибка, выполните следующие действия.

    1. Выполните следующую команду, чтобы установить два пакета в кластере Azure Databricks:
    
       ```bash
       scikit-learn==0.19.1
       pandas==0.22.0
       ```
    
    1. Отсоедините и снова подключите кластер к записной книжке.
    
    Если эти действия не устранят проблему, попробуйте перезапустить кластер.

* **Фаилтосендфеасер**: Если при чтении данных в кластере Azure Databricks `FailToSendFeather` возникает ошибка, см. следующие решения.
    
    * Обновите `azureml-sdk[automl]` пакет до последней версии.
    * Добавьте `azureml-dataprep` версию 1.1.8 или более позднюю.
    * Добавьте `pyarrow` версию 0,11 или более позднюю.
    
## <a name="create-and-manage-workspaces"></a>Создание рабочих областей и управление ими

> [!WARNING]
> Перемещение рабочей области Машинное обучение Azure в другую подписку или перемещение ответственной подписки на новый клиент не поддерживается. Это может привести к ошибкам.

* **Портал Azure**. Если вы непосредственно переходите к просмотру рабочей области из ссылки на общий ресурс из пакета SDK или портала, вы не сможете просматривать обычную страницу **обзора** со сведениями о подписке в расширении. Кроме того, вы не сможете переключиться на другую рабочую область. Если вам нужно просмотреть другую рабочую область, перейдите непосредственно в [машинное обучение Azure Studio](https://ml.azure.com) и выполните поиск по имени рабочей области.

## <a name="set-up-your-environment"></a>Настройка среды

* **Проблемы при создании амлкомпуте**: существует редкий шанс, что некоторые пользователи, создавшие свою машинное обучение Azureную рабочую область из портал Azure до общедоступного выпуска, могут не иметь возможности создать амлкомпуте в этой рабочей области. Вы можете либо отправить запрос в службу поддержки, либо создать новую рабочую область на портале или с помощью пакета SDK, чтобы немедленно разблокировать пользователя.

## <a name="work-with-data"></a>Работа с данными

### <a name="overloaded-azurefile-storage"></a>Перегруженное хранилище Азурефиле

Если появляется сообщение об ошибке `Unable to upload project files to working directory in AzureFile because the storage is overloaded`, используйте следующие обходные пути.

Если вы используете общую папку для других рабочих нагрузок, таких как передача данных, рекомендуется использовать большие двоичные объекты, чтобы файловый ресурс можно было использовать для отправки запусков. Вы также можете разделить рабочую нагрузку между двумя разными рабочими областями.

### <a name="passing-data-as-input"></a>Передача данных в качестве входного

*  **Типиррор: FileNotFound: отсутствует такой файл или каталог**: Эта ошибка возникает, если указанный путь к файлу не находится там, где расположен файл. Необходимо убедиться в том, что путь к файлу соответствует месту подключения набора данных к целевому объекту вычислений. Чтобы обеспечить детерминированное состояние, рекомендуется использовать абстрактный путь при подключении набора данных к целевому объекту вычислений. Например, в следующем примере выполняется подключение набора данных в корне файловой системы целевого объекта вычислений `/tmp`. 
    
    ```python
    # Note the leading / in '/tmp/dataset'
    script_params = {
        '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
    } 
    ```

    Если не указать начальную косую черту ("/"), необходимо добавить к рабочему каталогу префикс `/mnt/batch/.../tmp/dataset` , например, в целевом объекте вычислений, чтобы указать, где нужно подключить набор данных.

### <a name="data-labeling-projects"></a>Проекты меток данных

|Проблема  |Решение  |
|---------|---------|
|Можно использовать только наборы данных, созданные в хранилищах BLOB-объектов.     |  Это известное ограничение текущего выпуска.       |
|После создания проект показывает "инициализацию" в течение длительного времени     | Обновите страницу вручную. Инициализация должна продолжаться примерно 20 точек на секунду. Отсутствие автообновления является известной проблемой.         |
|При просмотре изображений недавно помеченные изображения не отображаются     |   Чтобы загрузить все помеченные изображения, нажмите **первую** кнопку. **Первая** кнопка вернет вас к началу списка, но загрузит все помеченные данные.      |
|Нажатие клавиши ESC при пометке для обнаружения объектов создает метку нулевого размера в левом верхнем углу. Отправка меток в этом состоянии завершается ошибкой.     |   Удалите метку, щелкнув рядом с ней крестик.  |

## <a name="azure-machine-learning-designer"></a>Конструктор Машинного обучения Azure

Известные проблемы:

* **Длительное время подготовки вычислений**: это может быть несколько минут или даже больше при первом подключении или создании целевого объекта вычислений. 

## <a name="train-models"></a>Обучение моделей

* **Модулиррорс (без модуля с именем)**: Если вы используете в модулиррорс при отправке экспериментов в машинном обучении Azure, это означает, что обучающий сценарий ожидает установки пакета, но не добавляется. После предоставления имени пакета Azure ML установит пакет в среде, используемой для запуска обучения. 

    Если для отправки экспериментов используются средства [оценки](concept-azure-machine-learning-architecture.md#estimators) , можно указать имя пакета с помощью `pip_packages` параметра или `conda_packages` в средстве оценки на основе источника, на котором нужно установить пакет. Можно также указать файл yml со всеми зависимостями с помощью `conda_dependencies_file`или перечислить все требования к PIP в TXT-файле `pip_requirements_file` с помощью параметра. Если у вас есть собственный объект среды машинного обучения Azure, для которого необходимо переопределить образ по умолчанию, используемый средством оценки, можно указать эту среду с `environment` помощью параметра конструктора средства оценки.

    МАШИНное обучение Azure также предоставляет средства оценки, зависящие от платформы, для Tensorflow, PyTorch, Chain и SKLearn. Используя эти средства оценки, вы убедитесь, что зависимости основной платформы установлены от вашего имени в среде, используемой для обучения. Есть возможность указать дополнительные зависимости, как описано выше. 
 
    Образы DOCKER, поддерживаемые МАШИНным обучением Azure, и их содержимое можно просмотреть в [контейнерах AzureML](https://github.com/Azure/AzureML-Containers).
    Зависимости, зависящие от платформы, перечислены в соответствующей документации по платформе — [цепочке](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

    > [!Note]
    > Если вы считаете, что определенный пакет является достаточно распространенным для добавления в образы и среды машинного обучения Azure, повысьте вопрос GitHub в [контейнерах AzureML](https://github.com/Azure/AzureML-Containers). 
 
* **Намиррор (имя не определено), аттрибутиррор (объект не имеет атрибута)**: это исключение должно поступать из сценариев обучения. Чтобы получить дополнительные сведения о конкретном имени, которое не определено или об ошибке атрибута, можно просмотреть файлы журналов из портал Azure. Из пакета SDK можно использовать `run.get_details()` для просмотра сообщения об ошибке. Также будут перечислены все файлы журналов, созданные для выполнения. Обязательно ознакомьтесь со сценарием обучения и устраните ошибку перед повторной отправкой запуска. 

* **Работа хоровод**завершена: в большинстве случаев при возникновении ошибки "Абортедеррор: хоровод завершена" это исключение означает, что в одном из процессов, вызвавших хоровод, было вызвано исключение. Каждый рейтинг в задании MPI получает собственный выделенный файл журнала в МАШИНном обучении Azure. Эти журналы имеют имена `70_driver_logs`. В случае распределенного обучения имена журналов добавляются `_rank` к суффиксу, чтобы упростить различение журналов. Чтобы найти точную ошибку, которая привела к завершению работы хоровод, просмотрите все файлы журнала и найдите `Traceback` в конце файлов driver_log. Один из этих файлов предоставит фактическое базовое исключение. 

* **Запуск или экспериментирование удаление**. эксперименты можно архивировать с помощью метода [эксперимент. Archive](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--) или из представления "эксперимент" в машинное обучение Azure Studio Client с помощью кнопки "архивировать эксперимент". Это действие скрывает эксперимент из списка запросов и представлений, но не удаляет его.

    Постоянное удаление отдельных экспериментов или запусков в настоящее время не поддерживается. Дополнительные сведения об удалении ресурсов рабочей области см. в разделе [Экспорт или удаление данных рабочей области службы машинное обучение](how-to-export-delete-data.md).

* **Документ метрики слишком большой**: машинное обучение Azure имеет внутренние ограничения на размер объектов метрик, которые могут быть зарегистрированы одновременно в ходе обучающего запуска. При возникновении ошибки "документ метрики слишком велик" при записи в журнал метрики, возвращающей значение со списком, попробуйте разделить список на меньшие фрагменты, например:

    ```python
    run.log_list("my metric name", my_metric[:N])
    run.log_list("my metric name", my_metric[N:])
    ```

    На внутреннем уровне Azure ML объединяет блоки с одинаковым именем метрики в смежный список.

## <a name="automated-machine-learning"></a>Автоматическое машинное обучение

* **Тензорные Flow**: автоматизированное машинное обучение в настоящее время не поддерживает тензорные Flow версии 1,13. Установка этой версии приведет к прекращению работы зависимостей пакетов. Мы работаем над устранением этой проблемы в будущем выпуске.

* **Диаграммы экспериментов**. диаграммы двоичной классификации (точность-отзыв, Roc, прибыль и т. д.), показанные в автоматических итерациях эксперимента ml, неправильно отображаются в пользовательском интерфейсе с 4/12. Графики диаграммы в настоящее время показывают обратные результаты, в которых более эффективные модели отображаются с более низкими результатами. Решение находится в процессе исследования.

* Для **отмены автоматического запуска машинного обучения отменяются**: при использовании средств автоматического машинного обучения в Azure Databricks для отмены запуска и запуска нового эксперимента перезапустите кластер Azure Databricks.

* **Модуль данных >10 итераций для автоматического машинного обучения**: в автоматических параметрах машинного обучения при наличии более 10 итераций задайте значение `show_output` `False` при отправке выполнения.

* **Мини-приложение "кирпичи" для машинное обучение Azure пакета SDK и автоматизированного машинного обучения**. в записной книжке для модуля "приложения" не поддерживается приложение машинное обучение Azure SDK, так как записные книжки не могут анализировать элементы HTML. Вы можете просмотреть мини-приложение на портале с помощью этого кода Python в Azure Databricks записной книжке:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```

## <a name="deploy--serve-models"></a>Развертывание и обслуживание моделей

Выполните следующие действия для следующих ошибок:

|Error  | Решение  |
|---------|---------|
|Ошибка при создании образа при развертывании веб-службы     |  Добавьте "Пинакл = = 1.2.1" в качестве зависимости PIP к файлу Conda для конфигурации образа.       |
|`['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`     |   Измените номер SKU для виртуальных машин, используемых в развертывании, на один с большим объемом памяти. |
|Сбой FPGA     |  Вы не сможете развернуть модели на FPGA до тех пор, пока не будет запрошена и одобрена квота FPGA. Чтобы запросить доступ, заполните форму запроса квоты: https://aka.ms/aml-real-time-ai       |

### <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>Обновление компонентов Машинное обучение Azure в кластере AKS

Обновления компонентов Машинное обучение Azure, установленных в кластере службы Azure Kubernetes, необходимо применять вручную. 

Эти обновления можно применить, отсоединив кластер от рабочей области Машинное обучение Azure, а затем повторно присоединив кластер к рабочей области. Если в кластере включен протокол TLS, то при повторном присоединении кластера необходимо предоставить сертификат TLS/SSL и закрытый ключ. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Если у вас больше нет сертификата TLS/SSL и закрытого ключа или вы используете сертификат, созданный Машинное обучение Azure, вы можете получить файлы до отключения кластера, подключившись к кластеру с помощью `kubectl` и извлекая секретный код. `azuremlfessl`

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes сохраняет секреты в формате в кодировке Base-64. Необходимо сначала декодировать компоненты `cert.pem` и `key.pem` для этих секретов в Base-64, прежде чем предоставлять их `attach_config.enable_ssl`в. 

### <a name="webservices-in-azure-kubernetes-service-failures"></a>Ошибки WebService в службе Kubernetes Azure

Многие сбои WebService в службе Kubernetes Azure можно отлаживать, подключившись к кластеру с помощью `kubectl`. Чтобы получить `kubeconfig.json` для кластера службы Azure Kubernetes, запустите

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="authentication-errors"></a>Ошибки проверки подлинности

При выполнении операции управления на целевом объекте вычислений из удаленного задания вы получите одну из следующих ошибок:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Например, вы получите сообщение об ошибке, если попытаетесь создать или вложить целевой объект вычислений из конвейера Машинного обучения, который передается для удаленного выполнения.
