---
title: Преобразование "Статистическая обработка" в потоке данных сопоставления
description: Узнайте, как агрегировать данные в фабрике данных Azure с помощью преобразования «Статистическая обработка потока данных сопоставления».
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 871f2b49e2dce9d762ef8a54923da04b0f24e4be
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606539"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Преобразование "Статистическая обработка" в потоке данных сопоставления

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Преобразование «Статистическая обработка» определяет статистические выражения столбцов в потоках данных. С помощью построителя выражений можно определить различные типы агрегатов, такие как SUM, MIN, MAX и COUNT, сгруппированные по существующим или вычисляемым столбцам.

## <a name="group-by"></a>Group by

Выберите существующий столбец или создайте новый вычисляемый столбец для использования в качестве предложения GROUP BY для статистической обработки. Чтобы использовать существующий столбец, выберите его из раскрывающегося списка. Чтобы создать новый вычисляемый столбец, наведите указатель мыши на предложение и щелкните **вычисляемый столбец**. Откроется [Построитель выражений потока данных](concepts-data-flow-expression-builder.md). После создания вычисляемого столбца введите имя выходного столбца в поле **name as (имя** ). Если вы хотите добавить дополнительное предложение GROUP BY, наведите указатель мыши на существующее предложение и щелкните значок "плюс".

![Статистическая обработка группы преобразования по параметрам](media/data-flow/agg.png "Статистическая обработка группы преобразования по параметрам")

Предложение GROUP BY является необязательным в преобразовании «Статистическая обработка».

## <a name="aggregate-column"></a>Статистический столбец 

Перейдите на вкладку **статистические** выражения для построения статистических выражений. Можно либо перезаписать существующий столбец агрегатом, либо создать новое поле с новым именем. Выражение статистической обработки указывается в правом поле рядом с селектором имени столбца. Чтобы изменить выражение, щелкните текстовое поле, чтобы открыть построитель выражений. Чтобы добавить дополнительные агрегаты, наведите указатель мыши на существующее выражение и щелкните значок плюса, чтобы создать новый столбец статистической обработки или [шаблон столбца](concepts-data-flow-column-pattern.md).

Каждое статистическое выражение должно содержать по крайней мере одну агрегатную функцию.

![Агрегатные параметры преобразования "Статистическая обработка"](media/data-flow/agg2.png "Агрегатные параметры преобразования "Статистическая обработка"")


> [!NOTE]
> В режиме отладки построитель выражений не может создавать предварительные представления данных с агрегатными функциями. Чтобы просмотреть предварительные данные для преобразований «Статистическая обработка», закройте построитель выражений и просмотрите данные на вкладке «Предварительный просмотр данных».

## <a name="reconnect-rows-and-columns"></a>Повторное соединение строк и столбцов

Агрегатные преобразования похожи на статистические запросы SELECT SQL. Столбцы, не входящие в предложение GROUP BY или агрегатные функции, не будут передаваться в выходные данные преобразования «Статистическая обработка». Если вы хотите включить другие столбцы в агрегированные выходные данные, выполните один из следующих методов.

* Используйте агрегатную функцию, например `last()` или `first()` , чтобы включить этот дополнительный столбец.
* Повторно присоедините столбцы к выходному потоку, используя [шаблон самосоединения](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/).

## <a name="removing-duplicate-rows"></a>Удаление повторяющихся строк

Обычно преобразование «Статистическая обработка» используется для удаления или идентификации повторяющихся записей в исходных данных. Этот процесс называется дедупликацией. На основе набора ключей Group By используйте эвристический алгоритм выбора, чтобы определить, какую из строк следует удерживать. Распространенная эвристика `first()`: `last()`, `max()`, и `min()`. Используйте [шаблоны столбцов](concepts-data-flow-column-pattern.md) , чтобы применить правило к каждому столбцу, за исключением столбцов Group By.

![Deduplication](media/data-flow/agg-dedupe.png "Дедупликация") (Дедупликация)

В приведенном выше примере столбцы `ProductID` и `Name` используются для группирования. Если две строки имеют одинаковые значения для этих двух столбцов, они считаются дубликатами. В этом преобразовании «Статистическая обработка» значения, соответствующие первой строке, будут сохранены, а все остальные будут удалены. При использовании синтаксиса шаблона столбцов все столбцы, имена `ProductID` которых `Name` не совпадают и, сопоставляются с существующим именем столбца и получают значения первых совпадающих строк. Выходная схема совпадает с входной схемой.

Для сценариев проверки данных `count()` функция может использоваться для подсчета количества дубликатов.

## <a name="data-flow-script"></a>Скрипт потока данных

### <a name="syntax"></a>Синтаксис

```
<incomingStream>
    aggregate(
           groupBy(
                <groupByColumnName> = <groupByExpression1>,
                <groupByExpression2>
               ),
           <aggregateColumn1> = <aggregateExpression1>,
           <aggregateColumn2> = <aggregateExpression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <aggregateTransformationName>
```

### <a name="example"></a>Пример

В приведенном ниже примере принимается `MoviesYear` входящий поток и группируются `year`строки по столбцу. Преобразование создает статистический столбец `avgrating` , результатом которого является среднее значение столбца. `Rating` Это преобразование «Статистическая `AvgComedyRatingsByYear`обработка» имеет имя.

В интерфейсе фабрики данных это преобразование выглядит как на изображении ниже:

![Пример группировки](media/data-flow/agg-script1.png "Пример группировки")

![Пример агрегирования](media/data-flow/agg-script2.png "Пример агрегирования")

Скрипт потока данных для этого преобразования находится в следующем фрагменте кода.

```
MoviesYear aggregate(
                groupBy(year),
                avgrating = avg(toInteger(Rating))
            ) ~> AvgComedyRatingByYear
```

![Статистический сценарий потока данных](media/data-flow/aggdfs1.png "Статистический сценарий потока данных")

```MoviesYear```: Производный столбец, определяющий столбцы ```AvgComedyRatingByYear```year и Title: преобразование "Статистическая обработка" для средней ```avgrating```оценки комедиес, сгруппированных по годам: имя создаваемого столбца для хранения агрегированного значения

```
MoviesYear aggregate(groupBy(year),
    avgrating = avg(toInteger(Rating))) ~> AvgComedyRatingByYear
```

## <a name="next-steps"></a>Дальнейшие шаги

* Определение агрегирования на основе окон с помощью [преобразования «окно»](data-flow-window.md)
