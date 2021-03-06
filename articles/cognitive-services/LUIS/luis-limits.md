---
title: Ограничения — LUIS
description: Это статья об известных ограничениях Интеллектуальной службы распознавания речи Azure (LUIS). LUIS имеет несколько ограничивающих областей. Ограничение модели управляет объектами, сущностями и функциями в LUIS. Предел квот основывается на типе ключа. Сочетание клавиш управляет веб-сайтом LUIS.
ms.topic: reference
ms.date: 05/06/2020
ms.openlocfilehash: d4a6162758fab7e5c9592b98974620bbf06ba978
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684603"
---
# <a name="limits-for-your-luis-model-and-keys"></a>Ограничения для модели и ключей LUIS
LUIS имеет несколько ограничивающих областей. Первый — это [ограничение модели](#model-limits), которое управляет особенностями, сущностями и ФУНКЦИЯМИ в Luis. Вторая область — [пределы квот](#key-limits) на основе типа ключа. Третья область ограничений — [сочетание клавиш](#keyboard-controls) для управления веб-сайтом Luis. Четвертая область — [сопоставление региона мира](luis-reference-regions.md) между веб-сайтом разработки LUIS и API-интерфейсами [конечной точки](luis-glossary.md#endpoint) LUIS.

<a name="model-boundaries"></a>

## <a name="model-limits"></a>Ограничения модели

Если размер приложения превышает ограничения модели LUIS, рекомендуется использовать приложение [диспетчеризации Luis](luis-concept-enterprise.md#dispatch-tool-and-model) или [контейнер Luis](luis-container-howto.md).

|С областями|Ограничение|
|--|:--|
| [Имя приложения][luis-get-started-create-app] | * Макс. кол-во символов по умолчанию |
| Приложения| 500. приложения на ресурс, разработав Azure |
| [Пакетное тестирование][batch-testing]| 10 наборов данных, 1000 высказываний для каждого набора данных|
| Явный список | 50 для каждого приложения|
| Внешние сущности | без ограничений |
| [Намерения][intents]|500 на приложение: 499 пользовательских целей и обязательное назначение _None_ .<br>Приложение, [основанное на диспетчеризации](https://aka.ms/dispatch-tool) , имеет соответствующие 500 источники диспетчеризации.|
| [Отображение сущностей](./luis-concept-entity-types.md) | Родитель: 50, потомок: 20 000 элементов. Каноническое имя — *макс. кол-во символов по умолчанию. Значения синонимов не имеют ограничений по длине. |
| [сущности и роли машинного обучения](./luis-concept-entity-types.md):<br> составного<br>простого<br>роль сущности|Ограничение в 100 родительских сущностей или 330 сущностей, в зависимости от того, какое ограничение будет достигнуто первым. Роль считается сущностью с целью этого ограничения. Примером является составной объект с простой сущностью, имеющий 2 роли: 1 составная + 1 простая + 2 роли = 4 из сущностей 330.<br>Вложенные сущности можно вкладывать до 5 уровней.|
|Модель как функция| Максимальное количество моделей, которое можно использовать в качестве функции для конкретной модели, чтобы иметь 10 моделей. Максимальное число списков фраз, используемых в качестве компонента для конкретной модели, в 10 списков фраз.|
| [Предварительный просмотр — сущности динамического списка](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|2 списка запросов к конечной точке прогноза ~ 1000 на запрос|
| [Шаблоны](luis-concept-patterns.md)|500 шаблонов для каждого приложения.<br>Максимальная длина шаблона — 400 символов.<br>3 сущности Pattern.any для каждого шаблона<br>Максимум 2 вложенных необязательных текста в шаблоне|
| [Pattern.Any](./luis-concept-entity-types.md)|100 для каждого приложения, 3 сущности pattern.any для каждого шаблона |
| [Список фраз][phrase-list]|500. списки фраз. 10 списков глобальных фраз из-за модели в качестве ограничения возможностей. Список неизменяемых фраз содержит максимум 5 000 фраз. Список взаимозаменяемых фраз содержит максимум 50 000 фраз. Максимальное число фраз для каждого приложения 500 000 словосочетаний.|
| [Предварительно созданные сущности](./luis-prebuilt-entities.md) | без ограничений|
| [Сущности регулярного выражения](./luis-concept-entity-types.md)|20 сущностей<br>макс. 500 символов для каждого шаблона сущности регулярного выражения|
| [Роли](luis-concept-roles.md)|300 ролей для каждого приложения, 10 ролей для каждой сущности|
| [Фраза][utterances] | 500 символов|
| [Высказывания][utterances] | 15 000 на приложение-нет ограничений на количество фразы продолжительностью на каждую цель|
| [Операционных](luis-concept-version.md)| 100 версий на приложение |
| [Имя версии][luis-how-to-manage-versions] | 128 символов |

* Макс. кол-во символов по умолчанию — 50.

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Уникальность имени

Имена объектов должны быть уникальными по сравнению с другими объектами того же уровня.

|Объекты|Ограничения|
|--|--|
|Намерение, сущность|Все имена намерений и сущностей должны быть уникальными в версии приложения.|
|Компоненты сущности ML|Все компоненты сущности для машинного обучения (дочерние сущности) должны быть уникальными в пределах этой сущности для компонентов на одном уровне.|
|Компоненты | Все именованные функции, такие как списки фраз, должны быть уникальными в пределах версии приложения.|
|Роли сущности|Все роли сущности или компонента сущности должны быть уникальными, если они находятся на одном уровне сущности (родительский, дочерний, внучатый и т. д.).|

## <a name="object-naming"></a>Именование объектов

Не используйте следующие символы в следующих именах.

|Объект|Исключить символы|
|--|--|
|Назначение, сущность и имена ролей|`:`<br>`$` <br> `&`|
|Имя версии|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="resource-usage-and-limits"></a>Использование ресурсов и ограничения

Понимание языка имеет отдельные ресурсы, один тип для создания и один тип для запроса к конечной точке прогнозирования. Дополнительные сведения о различиях между основными типами ключей см. в статье [Ключи разработки и запрашивания конечной точки прогнозирования в LUIS](luis-concept-keys.md).

<a name="key-limits"></a>

### <a name="authoring-resource-limits"></a>Ограничения на создание ресурсов

_kind_ `LUIS.Authoring` При фильтрации ресурсов в портал Azure используйте тип. LUIS ограничивает число приложений 500 на ресурс, созданный Azure.

|Создание ресурса|Создание технической спецификации|
|--|--|
|Ключ для начала работы|1 млн/месяц, 5/с|
|F0 — уровень Free |1 млн/месяц, 5/с|

* TPS = количество транзакций в секунду

[Дополнительные сведения о ценах.][pricing]

### <a name="query-prediction-resource-limits"></a>Ограничения ресурсов прогнозирования запросов

_kind_ `LUIS` При фильтрации ресурсов в портал Azure используйте тип. Ресурс конечной точки прогнозирования запросов LUIS, используемый в среде выполнения, допустим только для запросов к конечной точке.

|Ресурс прогнозирования запросов|Запрос технической спецификации|
|--|--|
|F0 — уровень Free |10 тыс./месяц, 5/с|
|S0 — уровень "Стандартный"|50/с|

### <a name="sentiment-analysis"></a>Анализ тональности

[Интеграция анализа тональности](luis-how-to-publish-app.md#enable-sentiment-analysis), которая предоставляет сведения о тональности, предоставляется без использования другого ресурса Azure.

### <a name="speech-integration"></a>Интеграция речи

[Интеграция с речью](../speech-service/how-to-recognize-intents-from-speech-csharp.md) обеспечивает 1000 запросов к конечной точке за единицу.

[Дополнительные сведения о ценах.][pricing]

## <a name="keyboard-controls"></a>Элементы управления клавиатуры

|Ввод с клавиатуры | Описание |
|--|--|
|CTRL+E|Переключение между токенами и сущностями в списке высказываний|

## <a name="website-sign-in-time-period"></a>Период времени входа веб-сайта

Выполнять вход можно в течение **60 минут**. По истечении этого времени возникнет ошибка. Необходимо снова выполнить вход.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
