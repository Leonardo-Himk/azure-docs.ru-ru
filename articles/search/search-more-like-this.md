---
title: функция запроса moreLikeThis (Предварительная версия)
titleSuffix: Azure Cognitive Search
description: Описание функции moreLikeThis (Предварительная версия), доступной в предварительных версиях REST API Когнитивный поиск Azure.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 95b9c76a2ff962cb2fa4bacbb1b1e9a953b7014f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74873817"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>moreLikeThis (Предварительная версия) в Azure Когнитивный поиск

> [!IMPORTANT] 
> Эта функция сейчас доступна в виде общедоступной предварительной версии. Для предварительной версии функции соглашение об уровне обслуживания не предусмотрено. Мы не рекомендуем использовать ее в рабочей среде. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Эта функция предоставляется в [версии REST API 2019-05-06-Preview](search-api-preview.md). В настоящее время нет поддержки портала или пакета SDK для .NET.

`moreLikeThis=[key]`параметр запроса в [API поиска документов](https://docs.microsoft.com/rest/api/searchservice/search-documents) , который находит документы, аналогичные документу, указанному ключом документа. При обработке поискового запроса с параметром `moreLikeThis` создается запрос с условиями поиска, извлеченными из заданного документа, которые лучше всего описывают этот документ. Затем созданный запрос используется для создания поискового запроса. По умолчанию учитываются содержимое всех полей, доступных для поиска, за вычетом полей с ограниченным доступом, заданных с помощью `searchFields` параметра. Параметр `moreLikeThis` нельзя использовать с параметром поиска `search=[string]`.

По умолчанию учитываются содержимое всех полей верхнего уровня, поддерживающих поиск. Если вместо этого нужно указать конкретные поля, можно использовать `searchFields` параметр. 

В `MoreLikeThis` [сложном типе](search-howto-complex-data-types.md)нельзя использовать подполя с возможностью поиска.

## <a name="examples"></a>Примеры

В следующих примерах используется пример использования гостиниц из [краткого руководства: создание индекса поиска в портал Azure](search-get-started-portal.md).

### <a name="simple-query"></a>Простой запрос

Следующий запрос находит документы, поля описания которых наиболее похожи на поля исходного документа, как указано в `moreLikeThis` параметре:

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=29&searchFields=Description&api-version=2019-05-06-Preview
```

В этом примере запрос выполняет поиск гостиниц, аналогичных одному, с `HotelId` 29.
Вместо использования HTTP GET можно также вызвать метод `MoreLikeThis` HTTP POST:

```
POST /indexes/hotels-sample-index/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "29",
      "searchFields": "Description"
    }
```

### <a name="apply-filters"></a>Применение фильтров

`MoreLikeThis`можно сочетать с другими общими параметрами запроса, такими `$filter`как. Например, запрос может быть ограничен только гостиницами, категория которых равна "бюджет" и где оценка выше 3,5:

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&api-version=2019-05-06-Preview
```

### <a name="select-fields-and-limit-results"></a>Выбор полей и ограничение результатов

`$top` Селектор можно использовать для ограничения количества результатов, `MoreLikeThis` возвращаемых в запросе. Кроме того, поля можно выбирать с `$select`помощью. Здесь выбираются три основных Гостиницы вместе с их ИДЕНТИФИКАТОРом, именем и оценкой: 

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&$top=3&$select=HotelId,HotelName,Rating&api-version=2019-05-06-Preview
```

## <a name="next-steps"></a>Дальнейшие действия

Для экспериментов с этой функцией можно использовать любое средство веб-тестирования.  Мы рекомендуем использовать POST в этом упражнении.

> [!div class="nextstepaction"]
> [Знакомство с Azure Когнитивный поиск API-интерфейсами RESTFUL с помощью POST](search-get-started-postman.md)
