---
title: Фильтровать по языку в индексе поиска
titleSuffix: Azure Cognitive Search
description: Условия фильтра для поддержки многоязыкового поиска, области выполнения запросов до полей, зависящих от языка.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: b0ebbbb64e173e1501f08f8385b14c365759a804
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82116287"
---
# <a name="how-to-filter-by-language-in-azure-cognitive-search"></a>Как фильтровать по языку в Azure Когнитивный поиск 

Основным требованием к многоязычному приложению поиска является возможность выполнять поиск и извлекать результаты на языке пользователя. В Когнитивный поиск Azure одним из способов удовлетворения языковых требований многоязычного приложения является создание набора полей, предназначенных для хранения строк на определенном языке, а затем ограничение полнотекстового поиска только этими полями во время запроса.

Параметры запроса используются для ограничения операции поиска, а затем обрезки результатов полей, которые не предоставили содержимого, совместимого с возможностями поиска, которые необходимо доставить.

| Параметры | Назначение |
|-----------|--------------|
| **searchFields** | Ограничивает полнотекстовый поиск до списка именованных полей. |
| **$select** | Обрезает ответ, включая только указанные поля. По умолчанию возвращаются все извлекаемые поля. Параметр **$Select** позволяет выбрать, какие из них следует вернуть. |

Успех этого метода зависит от целостности содержимого поля. Когнитивный поиск Azure не преобразует строки или не выполняет определение языка. Вы должны убедиться, что поля содержат ожидаемые строки.

## <a name="define-fields-for-content-in-different-languages"></a>Определение полей для содержимого на разных языках

В Когнитивный поиск Azure запросы предназначены для одного индекса. Разработчики, желающие предоставить языковые строки в одном поисковом интерфейсе, обычно задают выделенные поля для хранения значений: одно поле для строк на английском, одно для строк на французском и т. д. 

Следующий пример относится к примеру с [реальным пространством](search-get-started-portal.md) , в котором есть несколько строковых полей, содержащих содержимое на разных языках. Обратите внимание на назначения анализатора языка для полей в этом индексе. Поля, содержащие строки, работают быстрее в полнотекстовом поиске, когда они объединены с анализатором, спроектированным для обработки лингвистических правил целевого языка.

  ![](./media/search-filters-language/lang-fields.png)

> [!Note]
> Примеры кода, показывающие определения полей с помощью анализаторов языков, см. в разделе [Определение индекса службы поиска Azure](https://docs.microsoft.com/azure/search/search-create-index-dotnet) и [Определение индекса службы поиска Azure с помощью JSON-содержимого правильного формата](search-create-index-rest-api.md).

## <a name="build-and-load-an-index"></a>Построение и загрузка индекса

Промежуточным (и, возможно, очевидным) шагом является [сборка и заполнение индекса](https://docs.microsoft.com/azure/search/search-create-index-dotnet) перед формулировкой запроса. Здесь мы упомянем этот шаг для полноты картины. Один из способов определить доступность индекса — это проверить список индексов на [портале](https://portal.azure.com).

## <a name="constrain-the-query-and-trim-results"></a>Ограничение запроса и усечение результатов

Параметры запроса используются для ограничения поиска конкретных полей, а затем усечения результатов всех полей, которые не полезны для сценария. Учитывая цель ограничения поиска по полям, содержащим французские строки, нужно использовать **searchFields** для целевого запроса в полях, содержащих строки на этом языке. 

По умолчанию поиск возвращает все поля, которые помечены как извлекаемые. Таким образом может потребоваться исключить поля, которые не соответствуют языковому интерфейсу поиска, который необходимо предоставить. В частности при ограничении поиска до полей со строками на французском языке, может потребоваться исключить из результатов поля со строками на английском языке. С помощью параметра запроса **$select** можно контролировать какие поля возвращаются вызывающему приложению.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Хотя в запросе нет $filter аргумента, этот вариант использования строго связан с концепциями фильтров, поэтому он представлен как сценарий фильтрации.

## <a name="see-also"></a>См. также

+ [Фильтры в Когнитивный поиск Azure](search-filters.md)
+ [Языковые анализаторы](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [How full text search works in Azure Cognitive Search](search-lucene-query-architecture.md) (Как выполняется полнотекстовый поиск в Когнитивном поиске Azure)
+ [Поиск документов REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents)

