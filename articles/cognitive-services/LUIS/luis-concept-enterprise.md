---
title: Корпоративные концепции — LUIS
titleSuffix: Azure Cognitive Services
description: Принципы разработки больших приложений LUIS или нескольких приложений, использующих LUIS и QnA Maker.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: efef3faf3cc4ff04235254f0ff6538d92a831196
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "79221065"
---
# <a name="enterprise-strategies-for-a-luis-app"></a>Корпоративные стратегии для разработки приложения LUIS
Ознакомьтесь с этими стратегиями разработки корпоративных приложений.

## <a name="when-you-expect-luis-requests-beyond-the-quota"></a>Если ожидаются запросы LUIS, превышающие квоту

LUIS имеет месячную квоту, а также квоту за секунду в зависимости от ценовой категории ресурса Azure. 

Если частота запросов приложения LUIS превышает допустимую [частоту](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/), вы можете:

* Распределите нагрузку в более LUIS приложения с тем [же определением приложения](#use-multiple-apps-with-same-app-definition). Сюда входит, при необходимости, запуск LUIS из [контейнера](luis-container-howto.md). 
* Создание и [назначение нескольких ключей](#assign-multiple-luis-keys-to-same-app) для приложения. 

### <a name="use-multiple-apps-with-same-app-definition"></a>Использование нескольких приложений с одним определением приложения
Экспортируйте исходное приложение LUIS, а затем импортируйте его обратно в отдельные приложения. Каждое приложение имеет собственный идентификатор. При публикации не используйте один ключ для всех приложений, а создайте отдельный ключ для каждого приложения. Распределите нагрузку между всеми приложениями так, чтобы ни одно из них не было перегружено. Добавьте [Application Insights](luis-tutorial-bot-csharp-appinsights.md) для мониторинга использования. 

Чтобы получить одно и то же основное намерение во всех приложениях, прогнозирование намерения между первым и вторым намерением должно быть достаточно широким, чтобы служба LUIS не находилась в замешательстве, выдавая различные результаты в приложениях при незначительных изменениях в высказываниях. 

При обучении этих одноуровневых приложений необходимо провести [обучение со всеми данными](luis-how-to-train.md#train-with-all-data).

Назначьте одно приложение в качестве основного. Высказывания, предлагаемые для просмотра, следует добавить в основное приложение, а затем переместить обратно во все остальные приложения. Это можно сделать либо путем полного экспорта приложения, либо путем загрузки помеченных высказываний из основного приложения в дочерние. Загрузка выполняется с веб-сайта [LUIS](luis-reference-regions.md)или API разработки для [одного высказывания](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08) или для [пакета](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09). 

Запланируйте периодическую проверку (например, каждые две недели) [конечной точки фразы продолжительностью](luis-how-to-review-endpoint-utterances.md) для активного обучения, а затем выполните повторное обучение и повторную публикацию. 

### <a name="assign-multiple-luis-keys-to-same-app"></a>Назначение нескольких ключей LUIS одному приложению
Если приложение LUIS получает больше обращений к конечной точке, чем допускается квотой одного ключа, создайте и назначьте приложению дополнительные ключи LUIS. Чтобы управлять запросами конечной точки в ключах конечной точки, создайте диспетчер трафика или подсистему балансировки нагрузки. 

## <a name="when-your-monolithic-app-returns-wrong-intent"></a>Если монолитное приложение возвращает неправильное намерение
Если приложение предназначено для прогнозирования разнообразных высказываний пользователя, рекомендуется реализовать [модель диспетчеризации](#dispatch-tool-and-model). Разделение монолитного приложения позволит LUIS успешно обнаруживать намерения, а не путать намерения в родительском приложении и дочерних приложениях. 

Запланируйте периодическую [проверку высказываний конечной точки](luis-how-to-review-endpoint-utterances.md) для активного обучения, например каждые две недели, затем выполните повторное обучение и публикацию. 

## <a name="when-you-need-to-have-more-than-500-intents"></a>Если необходимо более 500 намерений
Предположим, что вы разрабатываете помощника Office, который имеет более 500 целей. Если 200 намерений относятся к планированию собраний, 200 связаны с напоминаниями, 200 предназначены для получения сведений о коллегах и 200 используются для отправки электронной почты, сгруппируйте намерения таким образом, чтобы каждая группа находилась в одном приложении, затем создайте приложение верхнего уровня, содержащее каждое намерение. Используйте [модель диспетчеризации](#dispatch-tool-and-model) для создания приложения верхнего уровня. Затем измените программу-робот для использования каскадного вызова, как показано в [руководстве по модели диспетчеризации](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs). 

## <a name="when-you-need-to-combine-several-luis-and-qna-maker-apps"></a>Если необходимо объединить несколько приложений LUIS и QnA Maker
Если у вас есть несколько приложений LUIS и QnA Maker, которые должны отвечать на робот, используйте [модель диспетчеризации](#dispatch-tool-and-model) для создания приложения верхнего уровня.  Затем измените программу-робот для использования каскадного вызова, как показано в [руководстве по модели диспетчеризации](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs). 

## <a name="dispatch-tool-and-model"></a>Средство и модель подготовки к отправке
Для объединения нескольких приложений LUIS и (или) приложений QnA Maker в родительское приложение LUIS используйте средство командной строки [Dispatch][dispatch-tool], которое можно найти в [BotBuilder-tools](https://github.com/Microsoft/botbuilder-tools). В этом случае у вас будет родительская предметная область, включая все темы и различные дочерние предметные области в отдельных приложениях. 

![Схематическое изображение архитектуры подготовки к отправке](./media/luis-concept-enterprise/dispatch-architecture.png)

Родительский домен указывается в LUIS с версией под именем `Dispatch` в списке приложений. 

Bot в чате получает utterance, а затем отправляет родительскому приложению LUIS для прогнозирования. Наиболее прогнозируемая цель из родительского приложения определяет, какое дочернее приложение LUIS вызывается далее. Программа-робот чата отправляет utterance в дочернее приложение для более конкретного прогноза.

Общие сведения о формировании этой иерархии вызовов из Bot Builder v4 см. в [этом](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs) руководстве.  

### <a name="intent-limits-in-dispatch-model"></a>Ограничения намерений в модели отправки
Приложение по отправке имеет максимум 500 источников отправки, эквивалентных 500 намерениям. 

## <a name="more-information"></a>Дополнительные сведения

* [Пакет SDK для Bot Framework](https://github.com/Microsoft/botframework)
* [Учебник по модели диспетчеризации](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs)
* [Диспетчер CLI](https://github.com/Microsoft/botbuilder-tools)
* Образец модели диспетчеризации — [.NET](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/csharp_dotnetcore/14.nlp-with-dispatch), [node. js](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/javascript_nodejs/14.nlp-with-dispatch)

## <a name="next-steps"></a>Следующие шаги

* Узнайте, как [выполнить пакетное тестирование](luis-how-to-batch-test.md)

[dispatcher-application-tutorial]: https://aka.ms/bot-dispatch
[dispatch-tool]: https://aka.ms/dispatch-tool
