---
title: Краткое руководство. Отправка запроса для прогнозирования с помощью браузера — LUIS
description: Из этого краткого руководства вы узнаете, как использовать общедоступное приложение LUIS для определения намерения пользователя на основе текста разговора в браузере.
ms.topic: quickstart
ms.date: 04/21/2020
ms.openlocfilehash: 5ba86882ebf3cb538ad6b865382342fcbd43d27c
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769971"
---
# <a name="quickstart-query-prediction-runtime-with-user-text"></a>Краткое руководство. Среда выполнения прогнозирующих запросов с поддержкой пользовательского текста

Чтобы понять, какие данные возвращает конечная точка прогнозирования LUIS, просмотрите результат прогнозирования в веб-браузере.

## <a name="prerequisites"></a>Предварительные требования

Чтобы запросить общедоступное приложение, вам потребуется перечисленные ниже средства.

* Сведения о ресурсах Распознавания речи (LUIS):
    * **Ключ прогнозирования** — можно получить на портале [LUIS](https://www.luis.ai/). Если у вас еще нет подписки для создания ключа, вы можете зарегистрироваться, чтобы получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
    * **Поддомен конечной точки прогнозирования** — дочерний домен также является **именем** ресурса LUIS.
* Идентификатор приложения LUIS. Используйте общедоступный идентификатор приложения для Интернета вещей `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. Пользовательский запрос, используемый в коде из этого краткого руководства, относится только к этому приложению.

## <a name="use-the-browser-to-see-predictions"></a>Использование браузера для просмотра прогнозирования

1. Откройте веб-браузер.
1. Используйте полные URL-адреса, приведенные ниже, заменив `YOUR-KEY` своим ключом прогнозирования LUIS. Запросы — это запросы GET, которые включает авторизацию с ключом прогнозирования LUIS в качестве параметра строки запроса.

    #### <a name="v3-prediction-request"></a>[Запрос на прогнозирование V3](#tab/V3-1-1)


    Формат URL-адреса V3 для запроса **GET** к конечной точке:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY
    `

    #### <a name="v2-prediction-request"></a>[Запрос на прогнозирование V2](#tab/V2-1-2)

    Формат URL-адреса V2 для запроса **GET** к конечной точке:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=YOUR-LUIS-PREDICTION-KEY&q=turn on all lights
    `

1. Вставьте URL-адрес в окне браузера и нажмите клавишу ВВОД. В браузере отображается результат JSON, который указывает, что LUIS определяет намерение `HomeAutomation.TurnOn` как основное и сущность `HomeAutomation.Operation` со значением `on`.

    #### <a name="v3-prediction-response"></a>[Ответ на прогнозирование V3](#tab/V3-2-1)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-response"></a>[Ответ на прогнозирование V2](#tab/V2-2-2)

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    * * *

1. Чтобы увидеть все намерения, добавьте соответствующий параметр строки запроса.

    #### <a name="v3-prediction-endpoint"></a>[Конечная точка прогнозирования V3](#tab/V3-3-1)

    Добавьте `show-all-intents=true` в конец строки запроса, чтобы **показать все намерения**:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/predict/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-endpoint"></a>[Конечная точка прогнозирования V2](#tab/V2)

    Добавьте `verbose=true` в конец строки запроса, чтобы **показать все намерения**:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY&verbose=true
    `

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "intents": [
            {
                "intent": "HomeAutomation.TurnOn",
                "score": 0.5375382
            },
            {
                "intent": "None",
                "score": 0.08687421
            },
            {
                "intent": "HomeAutomation.TurnOff",
                "score": 0.0207554
            }
        ],
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

## <a name="next-steps"></a>Дальнейшие действия

См. также:
* [Конечная точка прогнозирования V3](luis-migration-api-v3.md)
* [Пользовательские поддомены](../cognitive-services-custom-subdomains.md)

> [!div class="nextstepaction"]
> [Создание приложения на портале LUIS](get-started-portal-build-app.md)
