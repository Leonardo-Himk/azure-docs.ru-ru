---
title: Машинное обучение Azure в качестве источника сетки событий
description: Описание свойств, предоставляемых для событий рабочей области Машинное обучение с помощью службы "Сетка событий Azure"
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: spelluru
ms.openlocfilehash: f77a76d6acb67c739e0adf186d23e9b16ff7e2ee
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82928879"
---
# <a name="azure-machine-learning-as-an-event-grid-source"></a>Машинное обучение Azure в качестве источника сетки событий

Эта статья содержит свойства и схему для событий рабочей области машинного обучения. Общие сведения о схемах событий см. в статье [Схема событий службы "Сетка событий Azure"](event-schema.md).

## <a name="event-grid-event-schema"></a>Схема событий службы "Сетка событий Azure"

### <a name="available-event-types"></a>Доступные типы событий

Машинное обучение Azure выдает следующие типы событий:

| Тип события. | Описание |
| ---------- | ----------- |
| Microsoft. Мачинелеарнингсервицес. Моделрегистеред | Возникает при успешной регистрации новой модели или версии модели. |
| Microsoft. Мачинелеарнингсервицес. Моделдеплойед | Возникает при успешном развертывании моделей в конечной точке. |
| Microsoft. Мачинелеарнингсервицес. Рункомплетед | Возникает после успешного завершения выполнения. |
| Microsoft. Мачинелеарнингсервицес. Датасетдрифтдетектед | Возникает, когда монитор смещения набора данных обнаруживает смещение. |
| Microsoft. Мачинелеарнингсервицес. Рунстатусчанжед | Возникает при изменении состояния выполнения на "Failed". |

### <a name="the-contents-of-an-event-response"></a>Содержимое ответа на событие

При активации события служба службы "Сетка событий" отправляет данные об этом событии в подписку на конечную точку.

В этом разделе содержится пример того, как будут выглядеть данные для каждого события.

### <a name="microsoftmachinelearningservicesmodelregistered-event"></a>Событие Microsoft. Мачинелеарнингсервицес. Моделрегистеред

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "models/sklearn_regression_model:20",
  "eventType": "Microsoft.MachineLearningServices.ModelRegistered",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ModelName": "sklearn_regression_model",
    "ModelVersion": 20,
    "ModelTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ModelProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesmodeldeployed-event"></a>Событие Microsoft. Мачинелеарнингсервицес. Моделдеплойед

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "endpoints/my-sklearn-service",
  "eventType": "Microsoft.MachineLearningServices.ModelDeployed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ServiceName": "my-sklearn-service",
    "ServiceComputeType": "ACI",
    "ModelIds": "sklearn_regression_model:1,sklearn_regression_model:2",
    "ServiceTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ServiceProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesruncompleted-event"></a>Событие Microsoft. Мачинелеарнингсервицес. Рункомплетед

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesdatasetdriftdetected-event"></a>Событие Microsoft. Мачинелеарнингсервицес. Датасетдрифтдетектед

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "datadrifts/{}/runs/{}",
  "eventType": "Microsoft.MachineLearningServices.DatasetDriftDetected",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "DataDriftId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef",
    "DataDriftName": "myDriftMonitor",
    "RunId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef_1571590300380",
    "BaseDatasetId": "3c56d136-0f64-4657-a0e8-5162089a88a3",
    "TargetDatasetId": "d7e74d2e-c972-4266-b5fb-6c9c182d2a74",
    "DriftCoefficient": 0.83503490684792081,
    "StartTime": "2019-07-04T00:00:00+00:00",
    "EndTime": "2019-07-05T00:00:00+00:00"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesrunstatuschanged-event"></a>Событие Microsoft. Мачинелеарнингсервицес. Рунстатусчанжед

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    },
   "RunStatus": "failed"
   },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Свойства события

Событие содержит следующие высокоуровневые данные:

| Свойство | Type | Описание |
| -------- | ---- | ----------- |
| Раздел | string | Полный путь к ресурсу для источника событий. Это поле защищено от записи. Это значение предоставляет служба "Сетка событий". |
| subject | string | Определенный издателем путь к субъекту событий. |
| eventType | string | Один из зарегистрированных типов событий для этого источника событий. |
| eventTime | string | Время создания события с учетом времени поставщика в формате UTC. |
| id | string | Уникальный идентификатор события. |
| . | object | Данные события хранилища BLOB-объектов. |
| dataVersion | string | Версия схемы для объекта данных. Версию схемы определяет издатель. |
| metadataVersion | string | Версия схемы для метаданных события. Служба "Сетка событий" определяет схему свойств верхнего уровня. Это значение предоставляет служба "Сетка событий". |

Объект данных имеет следующие свойства для каждого типа событий:

### <a name="microsoftmachinelearningservicesmodelregistered"></a>Microsoft. Мачинелеарнингсервицес. Моделрегистеред

| Свойство | Type | Описание |
| -------- | ---- | ----------- |
| ModelName | string | Имя зарегистрированной модели. |
| моделверсион | string | Версия зарегистрированной модели. |
| моделтагс | object | Теги зарегистрированной модели. |
| моделпропертиес | object | Свойства зарегистрированной модели. |

### <a name="microsoftmachinelearningservicesmodeldeployed"></a>Microsoft. Мачинелеарнингсервицес. Моделдеплойед

| Свойство | Type | Описание |
| -------- | ---- | ----------- |
| ServiceName | string | Имя развернутой службы. |
| сервицекомпутетипе | string | Тип вычислений (например, ACI, AKS) развернутой службы. |
  | моделидс | string | Разделенный запятыми список идентификаторов моделей. Идентификаторы моделей, развернутых в службе. |
| сервицетагс | object | Теги развернутой службы. |
| сервицепропертиес | object | Свойства развернутой службы. |

### <a name="microsoftmachinelearningservicesruncompleted"></a>Microsoft. Мачинелеарнингсервицес. Рункомплетед

| Свойство | Type | Описание |
| -------- | ---- | ----------- |
| експериментид | string | ИДЕНТИФИКАТОР эксперимента, к которому относится выполнение. |
| експериментнаме | string | Имя эксперимента, которому принадлежит выполнение. |
| RunId | string | Идентификатор завершенного запуска. |
| рунтипе | string | Тип запуска завершенного выполнения. |
| рунтагс | object | Теги завершенного выполнения. |
| рунпропертиес | object | Свойства завершенного выполнения. |

### <a name="microsoftmachinelearningservicesdatasetdriftdetected"></a>Microsoft. Мачинелеарнингсервицес. Датасетдрифтдетектед

| Свойство | Type | Описание |
| -------- | ---- | ----------- |
| датадрифтид | string | ИДЕНТИФИКАТОР монитора смещения данных, вызвавшего событие. |
| датадрифтнаме | string | Имя монитора смещения данных, вызвавшего событие. |
| RunId | string | Идентификатор запуска, который обнаружил отклонение данных. |
| баседатасетид | string | Идентификатор базового набора данных, используемый для обнаружения смещения. |
| таржетдатасетид | string | Идентификатор целевого набора данных, используемый для обнаружения смещения. |
| дрифткоеффиЦиент | double | Результат коэффициента, вызвавшего событие. |
| StartTime | DATETIME | Время начала временного ряда целевого набора данных, которое привело к обнаружению отклонения.  |
| EndTime | DATETIME | Время окончания временного ряда набора данных, которое привело к обнаружению рассмещений. |

### <a name="microsoftmachinelearningservicesrunstatuschanged"></a>Microsoft. Мачинелеарнингсервицес. Рунстатусчанжед

| Свойство | Type | Описание |
| -------- | ---- | ----------- |
| експериментид | string | ИДЕНТИФИКАТОР эксперимента, к которому относится выполнение. |
| експериментнаме | string | Имя эксперимента, которому принадлежит выполнение. |
| RunId | string | Идентификатор завершенного запуска. |
| рунтипе | string | Тип запуска завершенного выполнения. |
| рунтагс | object | Теги завершенного выполнения. |
| рунпропертиес | object | Свойства завершенного выполнения. |
| RunStatus | string | Состояние выполнения. |

## <a name="tutorials-and-how-tos"></a>Руководства и инструкции
| Заголовок | Описание |
| ----- | ----- |
| [Использование событий Машинное обучение Azure](../machine-learning/concept-event-grid-integration.md) | Общие сведения об интеграции Машинное обучение Azure со службой "Сетка событий". |

## <a name="next-steps"></a>Дальнейшие действия

* См. общие сведения о [службе "Сетка событий Azure"](overview.md).
* Дополнительные сведения о создании подписки на службу "Сетка событий Azure" см. в разделе [Схема подписки службы "Сетка событий](subscription-creation-schema.md) ".
* Общие сведения об использовании службы "Сетка событий Azure" с Машинное обучение Azure см. в статье Использование [событий машинное обучение Azure](/azure/machine-learning/concept-event-grid-integration) .
* Пример использования службы "Сетка событий Azure" с Машинное обучение Azure см. в статье [создание рабочих процессов машинного обучения на основе событий](/azure/machine-learning/how-to-use-event-grid) .
