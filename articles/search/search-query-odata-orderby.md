---
title: Порядок сортировки по Справочнику OData
titleSuffix: Azure Cognitive Search
description: Справочная документация по синтаксису и языку для использования предложения ORDER-BY в запросах Когнитивный поиск Azure.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 99ec639b88f3334530243242aadfa0ab52a40df0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74113147"
---
# <a name="odata-orderby-syntax-in-azure-cognitive-search"></a>Синтаксис $orderby OData в Azure Когнитивный поиск

 С помощью [параметра **$OrderBy** OData](query-odata-filter-orderby-syntax.md) можно применить пользовательский порядок сортировки для результатов поиска в Azure когнитивный Поиск. В этой статье подробно описывается синтаксис **$OrderBy** . Дополнительные общие сведения об использовании **$OrderBy** при представлении результатов поиска см. [в статье работа с результатами поиска в Azure когнитивный Поиск](search-pagination-page-layout.md).

## <a name="syntax"></a>Синтаксис

Параметр **$OrderBy** принимает разделенный запятыми список из 32 **предложений упорядочения**. Синтаксис предложения ORDER-BY описывается следующим EBNF ([Расширенная форма Backus-Наура](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)):

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

Доступна также интерактивная схема синтаксиса:

> [!div class="nextstepaction"]
> [Схема синтаксиса OData для Когнитивный поиск Azure](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> Полный EBNF см. в [справочнике по синтаксису выражений OData для Azure когнитивный Поиск](search-query-odata-syntax-reference.md) .

Каждое предложение имеет критерий сортировки, при необходимости за которым следует направление сортировки`asc` (для по возрастанию или `desc` по убыванию). Если не указать направление, по умолчанию используется значение по возрастанию. Критерий сортировки может быть либо путем к `sortable` полю, либо вызовом либо [`geo.distance`](search-query-odata-geo-spatial-functions.md) [`search.score`](search-query-odata-search-score-function.md) функций, либо.

Если несколько документов имеют одинаковые условия сортировки и `search.score` функция не используется (например, если сортировка выполняется по числовому `Rating` полю, а все три документа имеют оценку 4), то в убывающем порядке они будут разорваны. Если оценки документа одинаковы (например, если в запросе не указан запрос полнотекстового поиска), относительный порядок связанных документов является неопределенным.

Вы можете определить несколько условий сортировки. Порядок выражений определяет окончательный порядок сортировки. Например, для сортировки по убыванию по показателям, за которыми следует оценка, синтаксис будет выглядеть `$orderby=search.score() desc,Rating desc`так:.

Синтаксис для `geo.distance` в **$orderby** такой же, как и в **$filter**. При использовании функции `geo.distance` в **$orderby** поле, к которому она применяется, должно быть сортируемым (`sortable`) и иметь тип `Edm.GeographyPoint`.

Синтаксис для `search.score` в **$orderby** — `search.score()`. Функция `search.score` не принимает никаких параметров.

## <a name="examples"></a>Примеры

Сортировать отели по возрастанию базового тарифа:

    $orderby=BaseRate asc

Сортировать отели по убыванию рейтинга, а затем по возрастанию базового тарифа (помните, что по умолчанию сортировка происходит по возрастанию):

    $orderby=Rating desc,BaseRate

Сортировка гостиниц по убыванию по рейтингу, затем по возрастанию по расстоянию от заданных координат:

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

Сортировка гостиниц в порядке убывания по поиску. Оценка и оценка, а затем в порядке возрастания по расстоянию от заданных координат. Между двумя гостиницами с одинаковыми показателями релевантности и рейтингами в первую очередь указывается ближайшее значение:

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Дальнейшие действия  

- [Работа с результатами поиска в Azure Когнитивный поиск](search-pagination-page-layout.md)
- [Общие сведения о языке выражений OData для Azure Когнитивный поиск](query-odata-filter-orderby-syntax.md)
- [Справочник по синтаксису выражений OData для Azure Когнитивный поиск](search-query-odata-syntax-reference.md)
- [Поиск документов &#40;Azure Когнитивный поиск REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
