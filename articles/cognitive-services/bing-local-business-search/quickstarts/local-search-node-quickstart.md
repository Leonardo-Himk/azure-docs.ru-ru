---
title: Краткое руководство. Отправка запроса в API Поиска местных компаний Bing с помощью Node.js
titleSuffix: Azure Cognitive Services
description: С помощью этого краткого руководства вы сможете начать отправку запросов в Bing Local Business Search API в Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: d366195f9cd72e6baa88c17203ae93cbbc6cbe6a
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80475538"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Краткое руководство. Отправка запроса в API Bing Local Business Search с помощью Node.js

С помощью этого краткого руководства вы сможете начать отправку запросов в Bing Local Business Search API в Cognitive Services. Хотя это простое приложение написано на Node.js, API является веб-службой RESTful, совместимой с любым языком программирования, который может выполнять HTTP-запросы и анализировать JSON.

В этом примере приложения из API извлекаются сведения о местных компаниях по поисковому запросу `hotel in Bellevue`.

## <a name="prerequisites"></a>предварительные требования

* Последняя версия [Node.js](https://nodejs.org/en/download/).

* [Библиотека запросов JavaScript](https://github.com/request/request)

Необходима [учетная запись API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с API Bing. Для этого краткого руководства достаточно [бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api). Используйте ключ доступа, предоставляемый в бесплатной пробной версии.  См. также [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="code-scenario"></a>Сценарий кода

Ниже приведенный код получает, определяет и отправляет запрос. Он реализуется с помощью следующих действий:

1. Объявите переменные для указания конечной точки с помощью узла и пути.
2. Укажите запрос и добавьте параметр запроса.
3. Создайте функцию обработчика для ответа.
4. Определите функцию поиска, которая создает запрос и добавляет заголовок "Ocp-Apim-Subscription-Key".
5. Запустите функцию поиска.

Полный код этого примера выглядит так:

```javascript
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'your-access-key';

let host = 'api.cognitive.microsoft.com/bing';
let path = '/v7.0/localbusinesses/search';

let mkt = 'en-US';
let q = 'hotel in Bellevue';

let params = '?q=' + encodeURI(q) + "&mkt=" + mkt;

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

```

## <a name="next-steps"></a>Дальнейшие действия

* [Quickstart: Send a query to the Bing Local Business Search API in C#](local-quickstart.md) (Краткое руководство. Отправка запроса в API Bing Local Business Search с помощью C#)
* [Краткое руководство. Отправка запроса в API Bing для поиска местных компаний с помощью Java](local-search-java-quickstart.md)
* [Краткое руководство. Отправка запроса в API Bing Local Business Search с помощью Python](local-search-python-quickstart.md)
