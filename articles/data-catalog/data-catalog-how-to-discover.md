---
title: Обнаружение источников данных в каталоге данных Azure
description: Эта статья описывает обнаружение зарегистрированных ресурсов данных с помощью каталога данных Azure, в том числе возможности поиска, фильтрации и выделения совпадений, предоставляемые порталом каталога данных Azure.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: b12cb94832a1ea977fb13f5f2271984dc8780cee
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "68736374"
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Обнаружение источников данных в каталоге данных Azure

## <a name="introduction"></a>Введение

Каталог данных Azure — это полностью управляемая облачная служба, выполняющая функции системы регистрации и обнаружения корпоративных источников данных. Иными словами, каталог данных помогает пользователям обнаруживать, анализировать и использовать источники данных. Она помогает организациям получать больше ценности из существующих данных. После того как источник данных зарегистрирован в каталоге данных, служба индексирует его метаданные, чтобы можно было легко найти необходимые данные.

## <a name="searching-and-filtering"></a>Поиск и фильтрация

Для обнаружения информации в каталоге данных используется два основных механизма: поиск и фильтрация.

Поиск должен быть одновременно понятным и эффективным. Условия поиска по умолчанию должны соответствовать всем свойствам в каталоге, включая пользовательские заметки.

Фильтрация дополняет поиск. Вы можете выбирать определенные характеристики, например экспертов, тип источника данных, тип объекта и теги. Можно просматривать только соответствующие ресурсы данных, а также ограничивать результаты поиска соответствующими ресурсами.

Используя сочетание поиска и фильтрации, вы можете быстро проходить по источникам данных, зарегистрированным в каталоге данных, в поиске нужных источников.

## <a name="search-syntax"></a>Синтаксис поиска

Хотя поиск произвольного текста по умолчанию является простым и интуитивно понятным, можно также воспользоваться синтаксисом поиска каталога данных для уточнения результатов. Каталог данных поддерживает следующие методы:

| Метод | Использование | Пример |
| --- | --- | --- |
| Обычный поиск |Обычный поиск с использованием одного или нескольких условий поиска. Результаты представлены ресурсами, которые соответствуют любому свойству с одним или несколькими указанными терминами. |`sales data` |
| Ограничение по свойству |Возвращение только тех источников данных, в которых искомое слово совпадает с указанным свойством. |`name:finance` |
| логические операторы |Расширение или сужение поиска с помощью логических операций. |`finance NOT corporate` |
| Обособление с помощью скобок |Скобки используются для обособления частей запросов, позволяя добиться логической изоляции. Это особенно удобно при использовании логических операторов. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Операторы сравнения |Операторы сравнения, в отличие от операторов равенства, используются для свойств с числовым типом данных и типом данных даты. |`modifiedTime > "11/05/2014"` |

Дополнительные сведения о поиске в каталоге данных см. в разделе [Каталог данных Azure](/rest/api/datacatalog/#search-syntax-reference).

## <a name="hit-highlighting"></a>Выделение совпадений

При просмотре результатов поиска выделяются все отображаемые свойства, соответствующие указанным условиям поиска: имя ресурса данных, описание и теги. Эти выделенные свойства помогают понять, почему среди результатов поиска появляется тот или иной ресурс данных.

> [!NOTE]
> Чтобы отключать выделение совпадений, используйте параметр **Выделение** на портале каталога данных.

При просмотре результатов поиска может быть не всегда очевидно, почему включен ресурс данных, даже если включено выделение попаданий. Так как поиск по умолчанию выполняется по всем свойствам, ресурс данных может быть включен в результаты из-за совпадения свойств на уровне столбца. И так как несколько пользователей могут снабжать зарегистрированные ресурсы данных своими тегами и описаниями, не все метаданные отображаются в списке результатов поиска.

В представлении плиток по умолчанию каждая плитка, отображенная в результатах поиска, будет включать значок **Просмотреть совпадения поиска терминов**. Это позволяет быстро просматривать все совпадения и их расположение, переходя к ним при необходимости.

 ![Выделение совпадений и результатов поиска на портале каталога данных Azure](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Сводка

Благодаря тому, что при регистрации в каталоге данных из источника данных копируются структурные и описательные метаданные, упрощается обнаружение и оценка такого источника. Зарегистрировав источник данных, вы можете обнаружить его на портале каталога данных Azure с помощью функций фильтрации и поиска.

## <a name="next-steps"></a>Дальнейшие действия

* Пошаговые инструкции по обнаружению источников данных см. в разделе [Начало работы с каталогом данных Azure](data-catalog-get-started.md).
