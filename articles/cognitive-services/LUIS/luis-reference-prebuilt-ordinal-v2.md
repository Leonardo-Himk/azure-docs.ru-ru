---
title: Порядковый номер V2 предварительно созданная сущность — LUIS
titleSuffix: Azure Cognitive Services
description: В этой статье содержатся сведения о предварительно построенных сущностях с порядковым номером v2 в Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 5e852313db75e598da647ea0f985e2ee18af16de
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "78270487"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Порядковый номер V2 предварительно созданной сущности для приложения LUIS
Порядковый номер V2 расширяет [порядковый](luis-reference-prebuilt-ordinal.md) номер для предоставления относительных `last`ссылок, `previous`таких как `next`, и. Они не извлекаются с использованием порядкового номера предварительно построенной сущности.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Разрешение для предварительно построенной сущности с порядковым номером v2

Для запроса возвращаются следующие объекты сущности:

`what is the second to last choice in the list`

#### <a name="v3-response"></a>[V3 ответ](#tab/V3)

Следующий код JSON имеет `verbose` параметр со значением: `false`

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[V3 подробный ответ](#tab/V3-verbose)

Следующий код JSON имеет `verbose` параметр со значением: `true`

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ],
    "$instance": {
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2.relative",
                "text": "the second to last",
                "startIndex": 8,
                "length": 18,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[Ответ v2](#tab/V2)

В следующем примере показано разрешение **встроенной сущности. ordinalV2** .

```json
"entities": [
    {
        "entity": "the second to last",
        "type": "builtin.ordinalV2.relative",
        "startIndex": 8,
        "endIndex": 25,
        "resolution": {
            "offset": "-1",
            "relativeTo": "end"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о [конечной точке прогнозирования V3](luis-migration-api-v3.md).

Сведения о [процентах](luis-reference-prebuilt-percentage.md), [номере телефона](luis-reference-prebuilt-phonenumber.md)и [температуре](luis-reference-prebuilt-temperature.md) .
