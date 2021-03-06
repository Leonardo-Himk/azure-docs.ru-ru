---
title: Добавление автозаполнения и предложений в поле поиска
titleSuffix: Azure Cognitive Search
description: Включите действия запроса "Поиск по мере использования" в Когнитивный поиск Azure, создав средства подбора и составления запросы, которые Автозаполнение поля поиска с завершенными условиями или фразами. Вы также можете вернуть предложенные совпадения.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 60e9a435d705ee0fee6509e92cdcb056ac7ab609
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81758111"
---
# <a name="add-autocomplete-and-suggestions-to-client-apps"></a>Добавление автозаполнения и предложений в клиентские приложения

Поиск по мере использования — это распространенный способ повышения производительности запросов, инициированных пользователем. В Когнитивный поиск Azure этот интерфейс поддерживается через *Автозаполнение*, которое завершает термин или фразу на основе частичного входа (завершение "Micro" с "Microsoft"). Другая форма — это *рекомендации*: краткий список соответствующих документов (возврат названий книг с идентификатором, чтобы можно было ссылаться на страницу со сведениями). Как автозаполнение, так и предложения зависят от соответствия в индексе. Служба не будет предлагать запросы, возвращающие нулевые результаты.

Для реализации этих возможностей в Azure Когнитивный поиск потребуется:

+ *Предложение* в серверной части.
+ *Запрос* , указывающий API [автозаполнения](https://docs.microsoft.com/rest/api/searchservice/autocomplete) или [предложения](https://docs.microsoft.com/rest/api/searchservice/suggestions) для запроса.
+ *Элемент управления пользовательского интерфейса* , позволяющий управлять взаимодействием поиска по мере ввода в клиентском приложении. Для этой цели рекомендуется использовать существующую библиотеку JavaScript.

В Когнитивный поиск Azure автозавершенные запросы и предлагаемые результаты извлекаются из индекса поиска из выбранных полей, зарегистрированных с помощью предложения. Предложение является частью индекса и указывает, какие поля будут предоставлять содержимое, которое либо завершает запрос, либо предлагает результат, либо оба. При создании и загрузке индекса структура данных средства подбора создается внутренним образом для хранения префиксов, используемых для сопоставления в частичных запросах. Для удобства можно выбрать подходящие поля, которые являются уникальными или по крайней мере не повторяться. Дополнительные сведения см. [в разделе Создание](index-add-suggesters.md)средства подбора.

Оставшаяся часть этой статьи посвящена запросам и клиентскому коду. Для иллюстрации ключевых моментов используется JavaScript и C#. REST API примеры используются для краткого представления каждой операции. Ссылки на готовые примеры кода см. в разделе [дальнейшие действия](#next-steps).

## <a name="set-up-a-request"></a>Настройка запроса

Элементы запроса включают один из интерфейсов API поиска по мере использования, частичный запрос и средство подбора. Следующий скрипт иллюстрирует компоненты запроса, используя REST API автозаполнения в качестве примера.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2019-05-06
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

**Сугжестернаме** предоставляет поля, поддерживающие предложения, которые используются для завершения условий и предложений. Для предложений в частности, список полей должен состоять из тех, которые предлагают четкие варианты по сравнению с результатами. На сайте, который продает компьютерные игры, поле может быть названием игры.

Параметр **поиска** предоставляет частичный запрос, где символы направляются в запрос запроса через элемент управления автозаполнения jQuery. В приведенном выше примере «минекраф» — это статическая иллюстрация того, что может быть передано элемент управления.

API не накладывают требования к минимальной длине для частичного запроса; оно может быть небольшим одним символом. Однако Автозаполнение jQuery обеспечивает минимальную длину. Обычно используется как минимум два или три символа.

Соответствия находятся в начале термина в любом месте входной строки. Учитывая «быструю отметку Fox», как автозаполнение, так и предложения будут соответствовать частичным версиям «The», «Quick», «Иванов» или «Fox», но не для частично инфиксныеных терминов, таких как «ровн» или «OX». Кроме того, каждое соответствие задает область для нисходящих расширений. Частичный запрос "Quick BR" будет соответствовать "быстрому коричневый" или "краткому представлению", но ни "Иванов", ни "операция" не будут соответствовать друг другу, если не предшествует "Quick".

### <a name="apis-for-search-as-you-type"></a>Интерфейсы API для поиска по мере ввода

Перейдите по следующим ссылкам на справочные страницы по ОСТАВШИМся и .NET SDK:

+ [Предложения REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [Автозаполнение REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [Метод Сугжествисхттпмессажесасинк](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [Метод Аутокомплетевисхттпмессажесасинк](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

## <a name="structure-a-response"></a>Структурирование ответа

Ответы для автозаполнения и предложений — это то, что можно ожидать при использовании шаблона: [функция автозаполнения](https://docs.microsoft.com/rest/api/searchservice/autocomplete#response) возвращает список терминов, а [предложения](https://docs.microsoft.com/rest/api/searchservice/suggestions#response) возвращают термины и идентификатор документа, чтобы можно было получить документ (используйте API-интерфейс [поиска документов](https://docs.microsoft.com/rest/api/searchservice/lookup-document) для получения определенного документа для страницы сведений).

Ответы обрабатываются параметрами запроса. Для автозаполнения задайте [**autocompleteMode**](https://docs.microsoft.com/rest/api/searchservice/autocomplete#autocomplete-modes) , чтобы определить, выполняется ли завершение текста на одном или двух терминах. Для предложений это поле определяет содержимое ответа.

Для получения предложений необходимо уточнить ответ, чтобы избежать повторений, или то, что кажется несвязанным. Чтобы управлять результатами, включите в запрос дополнительные параметры. Следующие параметры применяются как к автозаполнению, так и к предложениям, но, возможно, больше нужны для предложений, особенно если средство подбора содержит несколько полей.

| Параметр | Использование |
|-----------|-------|
| **$select** | Если в предложении есть несколько **саурцефиелдс** , используйте **$SELECT** , чтобы выбрать, какое поле вносит значения (`$select=GameTitle`). |
| **searchFields** | Ограничьте запрос конкретными полями. |
| **$filter** | Примените критерии соответствия для результирующего набора`$filter=Category eq 'ActionAdventure'`(). |
| **$top** | Ограничьте результаты до определенного числа (`$top=5`).|

## <a name="add-user-interaction-code"></a>Добавление кода взаимодействия с пользователем

Для автоматического заполнения термина запроса или удаления списка соответствующих ссылок требуется код взаимодействия с пользователем (обычно это JavaScript), который может использовать запросы из внешних источников, например Автозаполнение или запросы предложения к указателю поиска Azure.

Несмотря на то, что этот код можно написать в собственном коде, гораздо проще использовать функции из существующей библиотеки JavaScript. В этой статье демонстрируются две, одна для предложений и другая для автозаполнения. 

+ В примере предложения используется [мини-приложение автозаполнения (пользовательский интерфейс JQuery)](https://jqueryui.com/autocomplete/) . Можно создать поле поиска, а затем ссылаться на него в функции JavaScript, которая использует мини-приложение автозаполнения. В свойствах мини-приложения устанавливаются источник (функция автозаполнения или предложения), минимальная длина входных символов перед выполнением действия и позиционирование.

+ [Подключаемый модуль автозаполнения ксдсофт](https://xdsoft.net/jqplugins/autocomplete/) используется в качестве примера автозаполнения.

С помощью этих библиотек мы создаем поле поиска, которое поддерживает обе функции — предложения и автозаполнение. Входные данные, собранные в поле поиска, связаны с предложениями и действиями автозаполнения.

## <a name="suggestions"></a>Предложения

В этом разделе описывается реализация предлагаемых результатов, начиная с определения поля поиска. Здесь также показано, как и скрипт, который вызывает первую библиотеку автозавершения JavaScript, указанную в этой статье.

### <a name="create-a-search-box"></a>Создание поля поиска

Предполагая, что [Библиотека автозаполнения ПОЛЬЗОВАТЕЛЬСКОГО интерфейса jQuery](https://jqueryui.com/autocomplete/) и проект MVC в C#, вы можете определить поле поиска с помощью JavaScript в файле **index. cshtml** . Библиотека добавляет взаимодействие "Поиск как вы" в поле поиска, делая асинхронные вызовы контроллера MVC для получения предложений.

В **index. cshtml** в папке \виевс\хоме строка для создания поля поиска может выглядеть следующим образом:

```html
<input class="searchBox" type="text" id="searchbox1" placeholder="search">
```

В этом примере создается простое текстовое поле ввода с классом для определения стиля, идентификатором для ссылок на JavaScript и замещающим текстом.  

В том же файле внедрите JavaScript, который ссылается на поле поиска. Следующая функция вызывает API предложения, который запрашивает документы, соответствующие предложению, на основе частичных входных данных термина:

```javascript
$(function () {
    $("#searchbox1").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

`source` Сообщает функции автозаполнения интерфейса jQuery, где можно получить список элементов, отображаемых под полем поиска. Поскольку этот проект является проектом MVC, он вызывает функцию **предлагаю** в **HomeController.CS** , которая содержит логику для возврата предложений запросов. Также эта функция передает несколько параметров для управления выделением, нечетким соответствием и терминами. API JavaScript автозаполнения добавляет параметр термина.

Параметр `minLength: 3` гарантирует, что рекомендации будут отображаться только в том случае, если в поле поиска есть по крайней мере три символа.

### <a name="enable-fuzzy-matching"></a>Включить нечеткое сопоставление

Поиск нечетких соответствий позволяет получить результаты, даже если пользователь неверно напишет слово в поле поиска. Расстояние правки равно 1. Это означает, что может быть достигнуто максимальное расхождение одного символа между входными данными пользователя и совпадением. 

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

### <a name="enable-highlighting"></a>Включить выделение

При выделении применяется стиль шрифта к символам в результатах, которые соответствуют входным данным. Например, если часть входных данных имеет значение Micro, результат будет выглядеть как **Micro**Soft, **Micro**Scope и т. д. Выделение основано на параметрах Хигхлигхтпретаг и Хигхлигхтпосттаг, определенных встроенной функцией предложения.

```javascript
source: "/home/suggest?highlights=true&fuzzy=true&",
```

### <a name="suggest-function"></a>Предложение Function

Если вы используете C# и приложение MVC, файл **HomeController.CS** в каталоге Controllers — это место, где можно создать класс для предлагаемых результатов. В .NET функция предлагаю основана на [методе документсоператионсекстенсионс. предлагаю](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet).

`InitSearch` Метод создает клиент ИНДЕКСов HTTP с проверкой подлинности в службе когнитивный Поиск Azure. Дополнительные сведения о пакете SDK для .NET см. в статье [использование когнитивный Поиск Azure из приложения .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        Select = HotelName,
        SearchFields = HotelName,
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

Функция предложений использует два параметра, которые определяют, возвращается ли четкое совпадение или используется нечеткое соответствие в дополнение к вводу слова для поиска. Метод создает [объект сугжестпараметерс](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggestparameters?view=azure-dotnet), который затем передается в API предложения. Затем результат преобразуется в JSON, чтобы его можно было передать клиенту.

## <a name="autocomplete"></a>Автозавершение

До сих пор код UX поиска был выровнен по центру предложений. Следующий блок кода показывает Автозаполнение с помощью функции автозаполнения пользовательского интерфейса Ксдсофт jQuery, передавая запрос на Автозаполнение Azure Когнитивный поиск. Как и в случае с предложениями, в приложении C# код, который поддерживает взаимодействие с пользователем, рассматривается в **index. cshtml**.

```javascript
$(function () {
    // using modified jQuery Autocomplete plugin v1.2.6 https://xdsoft.net/jqplugins/autocomplete/
    // $.autocomplete -> $.autocompleteInline
    $("#searchbox1").autocompleteInline({
        appendMethod: "replace",
        source: [
            function (text, add) {
                if (!text) {
                    return;
                }

                $.getJSON("/home/autocomplete?term=" + text, function (data) {
                    if (data && data.length > 0) {
                        currentSuggestion2 = data[0];
                        add(data);
                    }
                });
            }
        ]
    });

    // complete on TAB and clear on ESC
    $("#searchbox1").keydown(function (evt) {
        if (evt.keyCode === 9 /* TAB */ && currentSuggestion2) {
            $("#searchbox1").val(currentSuggestion2);
            return false;
        } else if (evt.keyCode === 27 /* ESC */) {
            currentSuggestion2 = "";
            $("#searchbox1").val("");
        }
    });
});
```

### <a name="autocomplete-function"></a>Функция автозаполнения

Автозаполнение основано на [методе документсоператионсекстенсионс. AutoComplete](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet). Как и в случае с предложениями, этот блок кода пойдет в файл **HomeController.CS** .

```csharp
public ActionResult AutoComplete(string term)
{
    InitSearch();
    //Call autocomplete API and return results
    AutocompleteParameters ap = new AutocompleteParameters()
    {
        AutocompleteMode = AutocompleteMode.OneTermWithContext,
        UseFuzzyMatching = false,
        Top = 5
    };
    AutocompleteResult autocompleteResult = _indexClient.Documents.Autocomplete(term, "sg", ap);

    // Convert the Suggest results to a list that can be displayed in the client.
    List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = autocomplete
    };
}
```

Эта функция автозавершения принимает входные данные с термином для поиска. Затем этот метод создает объект [AutoCompleteParameters](https://docs.microsoft.com/rest/api/searchservice/autocomplete). Затем результат преобразуется в JSON, чтобы его можно было передать клиенту.

## <a name="next-steps"></a>Дальнейшие шаги

Используйте эти ссылки для получения комплексных инструкций или кода, демонстрирующих возможности поиска как типов. Оба примера кода включают гибридные реализации предложений и автозаполнения вместе.

+ [Учебник. Создание первого приложения на C# (урок 3)](tutorial-csharp-type-ahead-and-suggestions.md)
+ [Пример кода C#: Azure-Search-DotNet-Samples/Create-First-App/3 — Add-typeahead/](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/3-add-typeahead)
+ [Пример кода на C# и JavaScript с использованием параллельной программы RESTFUL](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete)