---
title: Простой тип сущности — LUIS
titleSuffix: Azure Cognitive Services
description: Простая сущность описывает одну концепцию из контекста машинного обучения. Добавьте список фраз при использовании простой сущности для улучшения результатов.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 1b5754be3c9941101a53f332841ace93caf9acdd
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684556"
---
# <a name="simple-entity"></a>Простая сущность

Простая сущность — это универсальная сущность, которая описывает одну концепцию и изучена в контексте машинного обучения. Так как простые сущности обычно являются именами, например названиями компаний, продуктов или другими категориями имен, добавьте [список фраз](luis-concept-feature.md) для выделения используемых имен.

**Сущность оптимальна для данных в таких случаях.**

* У данных отсутствует согласованное форматирование, но они означают одно и то же.

![простая сущность](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Пример JSON

`Bob Jones wants 3 meatball pho`

В предыдущем высказывании `Bob Jones` помечена как простая сущность `Customer`.

Данные, возвращаемые из конечной точки, содержат имя сущности, обнаруженный текст из высказывания, расположение обнаруженного текста, а также оценку:

#### <a name="v2-prediction-endpoint-response"></a>[Отклик конечной точки прогнозирования V2](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[Отклик конечной точки прогнозирования V3](#tab/V3)

Это JSON, если `verbose=false` в строке запроса задано:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|Объект данных|Имя сущности|Значение|
|--|--|--|
|Простая сущность|`Customer`|`bob jones`|

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Изучение синтаксиса шаблона](reference-pattern-syntax.md)