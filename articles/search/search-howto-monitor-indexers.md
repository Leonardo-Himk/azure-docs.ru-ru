---
title: Мониторинг состояния индексатора и результатов
titleSuffix: Azure Cognitive Search
description: Отслеживайте состояние, ход выполнения и результаты работы индексаторов Azure Когнитивный поиск в портал Azure с помощью REST API или пакета SDK для .NET.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 699b5a4e5a7f10c883667ca5030dd971855467f5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74112976"
---
# <a name="how-to-monitor-azure-cognitive-search-indexer-status-and-results"></a>Как отслеживать состояние и результаты индексатора Azure Когнитивный поиск

Azure Когнитивный поиск предоставляет сведения о состоянии и мониторинге текущих и исторических запусков каждого индексатора.

Мониторинг индексатора полезен, если вы хотите:

* Отслеживать ход выполнения индексатора во время текущего выполнения.
* Проверьте результаты выполнения текущего или предыдущего индексатора.
* Выявление ошибок индексатора верхнего уровня, а также ошибок или предупреждений об отдельных индексируемых документах.

## <a name="get-status-and-history"></a>Получение состояния и журнала

Получить доступ к сведениям о мониторинге индексатора можно различными способами, в том числе:

* В [портал Azure](#portal)
* Использование [REST API](#restapi)
* Использование [пакета SDK для .NET](#dotnetsdk)

Доступные сведения мониторинга индексатора включают все следующие данные (хотя форматы данных отличаются в зависимости от используемого метода доступа):

* Сведения о состоянии самого индексатора
* Сведения о последнем запуске индексатора, включая его состояние, время начала и окончания, а также подробные ошибки и предупреждения.
* Список выполняемых индексаторов и их состояний, результатов, ошибок и предупреждений.

Индексаторы, обрабатывающие большие объемы данных, могут занимать много времени. Например, индексаторы, обрабатывающие миллионы исходных документов, могут выполняться в течение 24 часов, а затем сразу же перезапускаются. Состояние индексаторов с большим объемом громкости всегда может **говориться на** портале. Даже если выполняется индексатор, доступны подробные сведения о текущем ходе выполнения и предыдущих запусках.

<a name="portal"></a>

## <a name="monitor-using-the-portal"></a>Мониторинг с помощью портала

Текущее состояние всех индексаторов можно просмотреть в списке **индексаторов** на странице Обзор службы поиска.

   ![Список индексаторов](media/search-monitor-indexers/indexers-list.png "Список индексаторов")

При выполнении индексатора состояние в списке отображается как **выполняется**, а в поле **документы с успехом** отображается количество обработанных документов. Обновление значений состояния индексатора и количества документов на портале может занять несколько минут.

Индексатор, последний запуск которого был выполнен успешно, показывает **успешное**выполнение. Выполнение индексатора может быть успешным, даже если отдельные документы содержат ошибки, если количество ошибок меньше значения параметра **Максимальное число элементов, завершившихся** с помощью индексатора.

Если Последнее выполнение завершилось ошибкой, отобразится состояние **Ошибка**. Состояние **Reset** означает, что состояние отслеживания изменений в индексаторе было сброшено.

Щелкните индексатор в списке, чтобы просмотреть дополнительные сведения о текущих и последних запусках индексатора.

   ![Сводка индексатора и журнал выполнения](media/search-monitor-indexers/indexer-summary.png "Сводка индексатора и журнал выполнения")

На диаграмме **сводки индексатора** отображается график количества документов, обработанных в самых последних запусках.

В списке **сведения о выполнении** отображается до 50 последних результатов выполнения.

Щелкните результат выполнения в списке, чтобы просмотреть конкретные сведения об этом запуске. Сюда входит время начала и окончания, а также все возникшие ошибки и предупреждения.

   ![Сведения о выполнении индексатора](media/search-monitor-indexers/indexer-execution.png "Сведения о выполнении индексатора")

Если во время выполнения возникли проблемы, связанные с документами, они будут перечислены в полях ошибки и предупреждения.

   ![Сведения об индексаторе с ошибками](media/search-monitor-indexers/indexer-execution-error.png "Сведения об индексаторе с ошибками")

Предупреждения являются общими для некоторых типов индексаторов и не всегда указывают на проблему. Например, индексаторы, использующие службы, могут сообщать о предупреждениях, если изображения или PDF-файлы не содержат текст для обработки.

Дополнительные сведения об анализе ошибок и предупреждений индексатора см. [в статье Устранение распространенных неполадок индексатора в когнитивный Поиск Azure](search-indexer-troubleshooting.md).

<a name="restapi"></a>

## <a name="monitor-using-rest-apis"></a>Мониторинг с помощью API-интерфейсов RESTFUL

Состояние и журнал выполнения индексатора можно получить с помощью [команды получить состояние индексатора](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status):

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2019-05-06
    api-key: [Search service admin key]

Ответ содержит сведения об общем состоянии индексатора, последнем (или текущем) вызове индексатора, а также журнал последних вызовов индексатора.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Журнал выполнения содержит до 50 последних запусков, которые сортируются в обратный хронологический порядок (последний первый).

Обратите внимание, что существует два разных значения состояния. Состояние верхнего уровня — для самого индексатора. Состояние индексатора **выполняется** означает, что индексатор настроен правильно и доступен для выполнения, но не работает.

Каждый запуск индексатора также имеет собственное состояние, которое указывает, выполняется ли данное конкретное выполнение (**выполняется**) или уже завершено с состоянием **Success**, **трансиентфаилуре**или **персистентфаилуре** . 

Когда индексатор сбрасывается для обновления состояния отслеживания изменений, добавляется отдельная запись журнала выполнения с состоянием **сброса** .

Дополнительные сведения о кодах состояния и данных мониторинга индексатора см. в разделе [жетиндексерстатус](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status).

<a name="dotnetsdk"></a>

## <a name="monitor-using-the-net-sdk"></a>Мониторинг с помощью пакета SDK для .NET

Расписание для индексатора можно определить с помощью пакета SDK Azure Когнитивный поиск .NET. Для этого включите свойство **Schedule** при создании или обновлении индексатора.

В следующем примере C# в консоль записываются сведения о состоянии индексатора и результаты его последнего (или текущего) запуска.

```csharp
static void CheckIndexerStatus(Indexer indexer, SearchServiceClient searchService)
{
    try
    {
        IndexerExecutionInfo execInfo = searchService.Indexers.GetStatus(indexer.Name);

        Console.WriteLine("Indexer has run {0} times.", execInfo.ExecutionHistory.Count);
        Console.WriteLine("Indexer Status: " + execInfo.Status.ToString());

        IndexerExecutionResult result = execInfo.LastResult;

        Console.WriteLine("Latest run");
        Console.WriteLine("  Run Status: {0}", result.Status.ToString());
        Console.WriteLine("  Total Documents: {0}, Failed: {1}", result.ItemCount, result.FailedItemCount);

        TimeSpan elapsed = result.EndTime.Value - result.StartTime.Value;
        Console.WriteLine("  StartTime: {0:T}, EndTime: {1:T}, Elapsed: {2:t}", result.StartTime.Value, result.EndTime.Value, elapsed);

        string errorMsg = (result.ErrorMessage == null) ? "none" : result.ErrorMessage;
        Console.WriteLine("  ErrorMessage: {0}", errorMsg);
        Console.WriteLine("  Document Errors: {0}, Warnings: {1}\n", result.Errors.Count, result.Warnings.Count);
    }
    catch (Exception e)
    {
        // Handle exception
    }
}
```

Выходные данные в консоли будут выглядеть примерно так:

    Indexer has run 18 times.
    Indexer Status: Running
    Latest run
      Run Status: Success
      Total Documents: 7, Failed: 0
      StartTime: 10:02:46 PM, EndTime: 10:02:47 PM, Elapsed: 00:00:01.0990000
      ErrorMessage: none
      Document Errors: 0, Warnings: 0

Обратите внимание, что существует два разных значения состояния. Состояние верхнего уровня — это состояние самого индексатора. Состояние индексатора " **выполняется** " означает, что индексатор настроен правильно и доступен для выполнения, но не выполняется в данный момент.

Каждый запуск индексатора также имеет собственное состояние для того, является ли это конкретное выполнение текущим (**запущенным**) или уже завершенным с состоянием **Success** или **трансиентеррор** . 

Когда индексатор сбрасывается для обновления состояния отслеживания изменений, добавляется отдельная запись журнала с состоянием **сброса** .

Дополнительные сведения о кодах состояния и сведения о мониторинге индексаторов см. в разделе [жетиндексерстатус](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) в REST API.

Сведения об ошибках или предупреждениях, связанных с конкретным документом, можно получить `IndexerExecutionResult.Errors` , `IndexerExecutionResult.Warnings`перечисляя списки и.

Дополнительные сведения о классах пакета SDK для .NET, используемых для мониторинга индексаторов, см. в разделе [индексерексекутионинфо](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutioninfo?view=azure-dotnet) и [индексерексекутионресулт](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet).
