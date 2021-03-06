---
title: Создание индекса поиска в портал Azure
titleSuffix: Azure Cognitive Search
description: Узнайте, как создать индекс для Когнитивный поиск Azure с помощью встроенного конструктора индексов портала.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f2e875c625431867e6e83cfd1e0b2c6d7a2781f7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74112850"
---
# <a name="create-an-azure-cognitive-search-index-in-the-portal"></a>по созданию индекса службы "Когнитивный поиск Azure" на портале

Когнитивный поиск Azure включает встроенный конструктор индексов на портале, который полезен для прототипов или создания [индекса поиска](search-what-is-an-index.md) , размещенного в службе когнитивный Поиск Azure. Этот инструмент позволяет создавать схемы. При сохранении определения пустой индекс будет полностью выражен в Когнитивный поиск Azure. Способ загрузки содержимого с возможностью поиска — это ваша информация.

Конструктор индексов — это лишь один из возможных способов создания индекса. Кроме того, можно создать и загрузить индекс с помощью [мастера импорта данных](search-get-started-portal.md). Мастер работает только с индексами, которые он создает. Вы можете выполнить эти задачу программным способом через API [.NET](search-create-index-dotnet.md) или API [REST](search-create-index-rest-api.md).

## <a name="start-index-designer"></a>Запуск конструктора индекса

1. Войдите на [портал Azure](https://portal.azure.com) и откройте панель мониторинга службы. Щелкните **Все службы** на панели переходов, чтобы найти существующие службы поиска в текущей подписке. 

2. На панели команд в верхней части страницы нажмите ссылку **Добавить индекс**.

   ![Добавить ссылку на индекс на панели команд](media/search-create-index-portal/add-index.png "Добавить ссылку на индекс на панели команд")

3. Присвойте имя индексу Azure Когнитивный поиск. Имена индексов указываются в операциях индексирования и выполнения запросов. Имя индекса преобразуется в URL-адрес конечной точки, используемый для подключений к индексу и для отправки HTTP-запросов в REST API Когнитивный поиск Azure.

   * Оно должно начинаться с буквы.
   * Используйте только строчные буквы, цифры или дефисы ("-").
   * Ограничьте имя 60 символами.

## <a name="add-fields"></a>Добавить поля

Структура индекса включает *коллекцию полей*, которая определяет данные, доступные для поиска в индексе. Коллекция полей задает структуру документов, которые передаются отдельно. Коллекция полей включает обязательные и необязательные поля с указанием имен и типов, а также атрибутов индекса, которые описывают возможное использование каждого поля.

1. Добавьте поля, чтобы полностью определить документы, которые вы хотите отправить, задав для каждого из них [тип данных](https://docs.microsoft.com/rest/api/searchservice/supported-data-types). Например, если документы состоят из полей *hotel-id*, *hotel-name*, *address*, *city* и *region*, создайте в индексе соответствующие поля для каждого из них. Просмотрите раздел [Руководство по проектированию для настройки атрибутов](#design).

1. Если входящие данные являются иерархическими, схема должна включать [сложные типы](search-howto-complex-data-types.md) для представления вложенных структур. Встроенный набор данных в гостиницах иллюстрирует сложные типы с использованием адреса (содержит несколько вложенных полей), которые имеют связь "один к одному" с каждым Гостиницы, и сложную коллекцию комнат, в которой несколько комнат связаны с каждым гостиницы. 

1. Укажите поле *Ключ* типа Edm.String. Ключевое поле является обязательным для каждого индекса Azure Когнитивный поиск и должно быть строкой. Значения для этого поля должно однозначно определять каждый документ. По умолчанию поле называется *id*, но вы можете переименовать его. При этом строка должна соответствовать [правилам именования](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Если ваша коллекция полей содержит *hotel-id*, вы должны выбрать его для ключа. 

1. Установите атрибуты в каждом поле. Конструктор индекса исключает любые атрибуты, которые являются недопустимыми для типа данных, но не предлагает включаемых объектов. Просмотрите руководство в следующем разделе, чтобы понять, для чего нужны эти атрибуты.

    Документация по API Azure Когнитивный поиск содержит примеры кода с простым индексом *гостиниц* . На снимке экрана ниже вы можете увидеть определение индекса, включая анализатор французского языка, указанный во время определения индекса, который можно повторно создать на портале в качестве руководства.

    ![Демонстрационный индекс гостиниц](media/search-create-index-portal/field-definitions.png "Демонстрационный индекс гостиниц")

1. По завершении щелкните **Создать**, чтобы сохранить настройки и создать индекс.

<a name="design"></a>

## <a name="set-attributes"></a>Определение атрибутов

Несмотря на то что вы в любой момент можете добавить новые поля, имеющиеся определения полей блокируются на время существования индекса. По этой причине разработчики обычно используют портал для создания простых индексов, тестирования идей или используют страницы портала для поиска параметра. Частая итерация по структуре индекса более эффективна, если следовать подходу на основе кода для легкой перестройки индекса.

Анализаторы и средства подбора связываются с полями перед сохранением индекса. Не забудьте добавить анализаторы языка или средств подбора в определение индекса при его создании.

Строковые поля часто помечены как **Доступный для поиска** и **Доступный для получения**. Для сужения результатов поиска используются такие поля: **Сортируемый**, **Фильтруемый** и **Аспектируемый**.

Атрибуты поля определяют, как используется поле, например, используется ли полнотекстовый поиск, фасетная навигация, операции сортировки и т д. В следующей таблице описывается каждый атрибут.

|Атрибут|Описание|  
|---------------|-----------------|  
|**возможностью поиска**|Полнотекстовый поиск, подлежащий лексическому анализу, такому как разбиения на слова во время индексации. Если, например, задать для поля, поддерживающего поиск, значение sunny day (солнечный день), оно будет разделено на элементы sunny и day. Дополнительные сведения см. в статье [Как работает полнотекстовый поиск в службе поиска Azure](search-lucene-query-architecture.md).|  
|**Фильтруемые**|Указывается в запросах **$filter**. Для фильтруемых полей типа `Edm.String` и `Collection(Edm.String)` не выполняется разбиение на слова, поэтому они могут попасть в результаты поиска только по точному совпадению. Например, если для такого поля задать значение sunny day, запрос `$filter=f eq 'sunny'` не вернет совпадений, а запрос — `$filter=f eq 'sunny day'` вернет. |  
|**Сортируемый**|По умолчанию система сортирует результаты по их оценке, однако можно настроить сортировку на основе полей в документах. Поля типа `Collection(Edm.String)` не могут быть **сортируемыми**. |  
|**аспектируемый**|Обычно используется в представлении результатов поиска, включающих количество обращений по категориям (например, отелей в определенном городе). Этот параметр не предназначен для использования с полями типа `Edm.GeographyPoint`. Поля типа `Edm.String`, которые имеют атрибуты **фильтруемый**, **сортируемый** или **аспектируемый**, могут быть длинной до 32 КБ. Дополнительные сведения см. в статье [Create Index (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Создание индекса в REST API службы Поиска Azure).|  
|**key**|Уникальный идентификатор для документов в индексе. Только одно поле должно быть выбрано ключевым и оно должно иметь тип `Edm.String`.|  
|**извлекаемые**|Определяет, включается ли поле в возвращаемые поиском результаты. Этот атрибут полезен, когда поле (например, *показатель прибыльности*) нужно использовать для фильтрации, сортировки или оценки, но оно не должно отображаться конечному пользователю. У полей с установленным свойством `true` for `key` .|  

## <a name="next-steps"></a>Дальнейшие действия

После создания индекса Когнитивный поиск Azure можно перейти к следующему шагу: [Передача данных с возможностью поиска в индекс](search-what-is-data-import.md).

Кроме того, узнать больше об индексах можно [здесь](search-what-is-an-index.md). В дополнение к коллекции полей, индекс указывает анализаторы, средства подбора, профили оценки и параметры CORS. На портале есть вкладки для определения наиболее распространенных элементов: полей, анализаторов и средств подбора. Для создания или изменения других элементов можно использовать REST API или пакет SDK для .NET.

## <a name="see-also"></a>См. также

 [Принцип работы полнотекстового поиска](search-lucene-query-architecture.md)  
 [REST API службы поиска](https://docs.microsoft.com/rest/api/searchservice/) [Пакет SDK для .NET](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

