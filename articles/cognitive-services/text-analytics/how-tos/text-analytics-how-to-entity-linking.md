---
title: Использование распознавания сущностей с помощью API анализа текста
titleSuffix: Azure Cognitive Services
description: Узнайте, как идентифицировать и устранить неоднозначность удостоверения сущности, найденной в тексте с помощью REST API Анализ текста.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 05/13/2020
ms.author: aahi
ms.openlocfilehash: 457be5ac014fda6b4984ed7af3dcc89780b16379
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84141623"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Как использовать распознавание именованных сущностей в Анализ текста

API анализа текста позволяет получить неструктурированный текст и возвращает список неоднозначного количества сущностей со ссылками на дополнительные сведения в Интернете. API поддерживает как распознавание сущностей (NER), так и связывание сущностей.

### <a name="entity-linking"></a>Связывание сущностей

Связывание сущностей — это возможность определить и устранить неоднозначность идентификации сущности, найденной в тексте (например, определить, относится ли вхождение слова «Mars» к планетам или Roman божественные War). Для этого процесса требуется наличие базы знаний на соответствующем языке, чтобы связать распознанные сущности в тексте. При связывании сущностей в качестве этой базы знаний используется [Википедии](https://www.wikipedia.org/) .


### <a name="named-entity-recognition-ner"></a>Распознавание именованных сущностей (NER)

Распознавание имен сущностей (NER) — это возможность определения различных сущностей в тексте и категоризация их по заранее определенным классам или типам, таким как "пользователь", "расположение", "событие", "продукт" и "Организация".  

## <a name="named-entity-recognition-versions-and-features"></a>Версии и функции распознавания именованных сущностей

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

| Компонент                                                         | NER версии 3.0 | NER версии 3.1 – Предварительная версия. 1 |
|-----------------------------------------------------------------|--------|----------|
| Методы для одиночных и пакетных запросов                          | X      | X        |
| Расширенное распознавание сущностей в нескольких категориях           | X      | X        |
| Отдельные конечные точки для отправки запросов на связывание сущностей и NER. | X      | X        |
| Распознавание личных `PII` объектов () и сведений о работоспособности ( `PHI` )        |        | X        |

Дополнительные сведения см. в разделе [Поддержка языков](../language-support.md) .

### <a name="entity-types"></a>Типы сущностей

Распознавание именованных сущностей v3 обеспечивает Расширенное обнаружение для нескольких типов. В настоящее время NER v 3.0 может распознавать сущности в [категории общих сущностей](../named-entity-types.md).

Распознавание имен сущностей версии 3.1 – Preview. 1 включает возможности обнаружения версий 3.0 и возможность обнаружения персональных данных ( `PII` ) с помощью `v3.1-preview.1/entities/recognition/pii` конечной точки. Необязательный параметр можно использовать `domain=phi` для обнаружения конфиденциальных сведений о работоспособности ( `PHI` ). Дополнительные сведения см. в разделе статьи о [категориях сущностей](../named-entity-types.md) и [запрос конечных точек](#request-endpoints) ниже.


## <a name="sending-a-rest-api-request"></a>Отправка запроса к REST API

### <a name="preparation"></a>Подготовка

Необходимо иметь документы JSON в следующем формате: ID, Text, Language.

Длина каждого документа не должна превышать 5 120 символов. в каждой коллекции может быть до 1 000 элементов (идентификаторов). Коллекция передается в тексте запроса.

### <a name="structure-the-request"></a>Структурирование запроса

Создайте запрос POST. Вы можете [использовать POST](text-analytics-how-to-call-api.md) или **консоль тестирования API** в следующих ссылках, чтобы быстро структурировать и отправить ее. 

> [!NOTE]
> Ключ и конечную точку для ресурса Анализа текста можно найти на портале Azure. Они находятся на странице ресурса **Быстрый запуск** в разделе **Управление ресурсами**. 


### <a name="request-endpoints"></a>Конечные точки запроса

#### <a name="version-30"></a>[Версия 3.0](#tab/version-3)

В примере распознавания сущностей v3 используются отдельные конечные точки для запросов NER и связывания сущностей. Используйте следующий формат URL-адреса в зависимости от вашего запроса:

Связывание сущностей
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

NER
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

#### <a name="version-31-preview1"></a>[Версия 3,1-Preview. 1](#tab/version-3-preview)

При распознавании именованных сущностей `v3.1-preview.1` используются отдельные конечные точки для запросов NER и связывания сущностей. Используйте следующий формат URL-адреса в зависимости от вашего запроса:

Связывание сущностей
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/entities/linking`

NER
* Общие сущности —`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/entities/recognition/general`

* Личные `PII` сведения () —`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/entities/recognition/pii`

Кроме того, можно использовать необязательный `domain=phi` параметр для обнаружения `PHI` сведений о работоспособности () в тексте. 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/entities/recognition/pii?domain=phi`

---

Включите ключ API Анализа текста в заголовок запроса. В тексте запроса укажите подготовленные документы JSON.

### <a name="example-ner-request"></a>Пример запроса NER 

Ниже приведен пример содержимого, которое можно отправить в API. Формат запроса одинаков для обеих версий API.

```json
{
  "documents": [
    {
        "id": "1",
        "language": "en",
        "text": "Our tour guide took us up the Space Needle during our trip to Seattle last week."
    }
  ]
}

```

## <a name="post-the-request"></a>Передача запроса

Анализ выполняется при получении запроса. Сведения о размере и числе запросов, которые можно отправлять в минуту и секунду, см. в разделе об [ограничениях данных](../overview.md#data-limits).

API Анализа текста не учитывает состояние. Никакие данные в учетной записи не сохраняются, а все результаты немедленно возвращаются в ответе.

## <a name="view-results"></a>Просмотр результатов

Все запросы POST возвращают отформатированный ответ JSON с идентификаторами и обнаруженными свойствами сущности.

Вывод возвращается немедленно. Можно выполнить потоковую передачу результатов в приложение, которое принимает JSON, или сохранить выходные данные в файле локальной системы, а затем импортировать их в приложение, которое дает возможность сортировки, поиска данных и управления ими. В связи с поддержкой многоязычности и эмодзи ответ может содержать смещения текста. Дополнительные сведения см. [в разделе Обработка смещения текста](../concepts/text-offsets.md) .

### <a name="example-v3-responses"></a>Примеры ответов v3

Версия 3 предоставляет отдельные конечные точки для NER и связывания сущностей. Ниже приведены ответы для обеих операций. 

#### <a name="example-ner-response"></a>Пример ответа NER

```json
{
  "documents": [
    {
      "id": "1",
      "entities": [
        {
          "text": "tour guide",
          "category": "PersonType",
          "offset": 4,
          "length": 10,
          "confidenceScore": 0.45
        },
        {
          "text": "Space Needle",
          "category": "Location",
          "offset": 30,
          "length": 12,
          "confidenceScore": 0.38
        },
        {
          "text": "trip",
          "category": "Event",
          "offset": 54,
          "length": 4,
          "confidenceScore": 0.78
        },
        {
          "text": "Seattle",
          "category": "Location",
          "subcategory": "GPE",
          "offset": 62,
          "length": 7,
          "confidenceScore": 0.78
        },
        {
          "text": "last week",
          "category": "DateTime",
          "subcategory": "DateRange",
          "offset": 70,
          "length": 9,
          "confidenceScore": 0.8
        }
      ],
      "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-04-01"
}
```


#### <a name="example-entity-linking-response"></a>Пример ответа на связывание сущности

```json
{
  "documents": [
    {
      "id": "1",
      "entities": [
        {
          "name": "Space Needle",
          "matches": [
            {
              "text": "Space Needle",
              "offset": 30,
              "length": 12,
              "confidenceScore": 0.4
            }
          ],
          "language": "en",
          "id": "Space Needle",
          "url": "https://en.wikipedia.org/wiki/Space_Needle",
          "dataSource": "Wikipedia"
        },
        {
          "name": "Seattle",
          "matches": [
            {
              "text": "Seattle",
              "offset": 62,
              "length": 7,
              "confidenceScore": 0.25
            }
          ],
          "language": "en",
          "id": "Seattle",
          "url": "https://en.wikipedia.org/wiki/Seattle",
          "dataSource": "Wikipedia"
        }
      ],
      "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-02-01"
}
```


## <a name="summary"></a>Сводка

В этой статье рассматриваются основные понятия и рабочий процесс связывания сущностей в службе Текстовой аналитики Cognitive Services. В разделе "Сводка" сделайте следующее.

* В тексте запроса передаются документы JSON, которые содержат идентификатор, текст и код языка.
* Запросы POST отправляются на одну или несколько конечных точек, используя персонализированный [ключ доступа и конечную точку](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) , допустимую для вашей подписки.
* Ответ будет содержать выходные данные, содержащие связанные сущности (включая оценки уверенности, смещения и веб-ссылки для каждого идентификатора документа), которые можно использовать в любом приложении

## <a name="next-steps"></a>Дальнейшие шаги

* [Text Analytics overview](../overview.md) (Общие сведения об анализе текста)
* [Использование клиентской библиотеки Анализа текста](../quickstarts/text-analytics-sdk.md)
* [Новые возможности](../whats-new.md)
