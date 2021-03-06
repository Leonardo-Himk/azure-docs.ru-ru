---
title: Обучение App-LUIS
titleSuffix: Azure Cognitive Services
description: Обучение версии приложения Интеллектуальной службы распознавания речи (LUIS) позволяет улучшить распознавание естественного языка. Следует обучать приложение LUIS после обновления модели, например добавления, изменения, пометки или удаления сущностей, намерений или фраз.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 1da8ab3015730c6b3e1962301a34b1ad43b1aad6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "79219866"
---
# <a name="train-your-active-version-of-the-luis-app"></a>Обучение активной версии приложения LUIS 

Обучение приложения службы "Распознавание речи" (LUIS) позволяет улучшить распознавание естественного языка. Следует обучать приложение LUIS после обновления модели, например добавления, изменения, пометки или удаления сущностей, намерений или фраз. 

Обучение и [тестирование](luis-concept-test.md) приложения — это итеративный процесс. Обученное приложение LUIS следует протестировать с использованием примеров фраз, чтобы проверить правильность распознавания намерений и сущностей. Если распознавание выполняется неправильно, внесите изменения в приложение, а затем обучите его и протестируйте еще раз. 

Обучение применяется к активной версии на портале LUIS. 

## <a name="how-to-train-interactively"></a>Обучение в интерактивном режиме

Чтобы начать итеративный процесс на [портале LUIS](https://www.luis.ai), необходимо сначала хотя бы один раз обучить приложение LUIS. Перед обучением убедитесь, что для каждого намерения имеется минимум одно высказывание.

1. Откройте приложение, выбрав его имя на странице **Мои приложения**. 

1. В приложении выберите **Обучить** на панели сверху. 

1. По завершении обучения в верхней части браузера появится уведомление.

## <a name="training-date-and-time"></a>Дата и время обучения

Дата и время обучения — GMT + 2. 

## <a name="train-with-all-data"></a>Обучение на основе всех данных

При обучении используется небольшой процент отрицательных выборок. Если вы хотите использовать все данные вместо небольшой отрицательной выборки, используйте [API](#version-settings-api-use-of-usealltrainingdata).

### <a name="version-settings-api-use-of-usealltrainingdata"></a>Параметры версии API использования Усеаллтраинингдата

Используйте [API параметров версии](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) с параметром `UseAllTrainingData` , имеющим значение true, чтобы отключить эту функцию. 

## <a name="unnecessary-training"></a>Необязательное обучение

Нет необходимости проводить обучение после каждого отдельного изменения. Обучение должно выполняться после группы изменений, применяемых к модели, а также когда следующим этапом будет тестирование или публикация. Если тестирование или публикация не требуются, проводить обучение необязательно. 

## <a name="training-with-the-rest-apis"></a>Обучение с помощью REST API

Обучение на портале LUIS выполняется в одно действие — нажатием кнопки **Обучить**. Обучение с помощью REST API — это процесс из двух этапов. Первый этап — [запрос обучения](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) с помощью HTTP POST. Второй этап — запрос [состояния обучения](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) с помощью HTTP GET. 

Чтобы узнать о завершении обучения, необходимо выполнять опрос состояния до тех пор, пока все модели не будут успешно обучены. 

## <a name="next-steps"></a>Следующие шаги

* [Интерактивное тестирование](luis-interactive-test.md)
* [Пакетное тестирование](luis-how-to-batch-test.md)
