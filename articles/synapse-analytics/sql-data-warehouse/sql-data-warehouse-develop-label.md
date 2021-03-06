---
title: Использование меток для инструментирования запросов
description: Советы по использованию меток для инструментирования запросов в пуле синапсе SQL для разработки решений.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 5e2cd03ae878e80139a7f7a8ba67cef15b24d571
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80633488"
---
# <a name="using-labels-to-instrument-queries-in-synapse-sql-pool"></a>Использование меток для инструментирования запросов в пуле SQL синапсе

В эту статью включены советы по разработке решений с помощью меток для инструментирования запросов в пуле SQL.

Рекомендации по использованию меток для инструментирования запросов в хранилище данных SQL Azure для разработки решений.

## <a name="what-are-labels"></a>Что такое метки

Пул SQL поддерживает концепцию, называемую метками запросов. Прежде чем углубляться в это понятие, рассмотрим пример.

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Последняя строка добавляет к запросу тег "Метка моего запроса". Этот тег полезен, так как метка может выполнять запросы через динамические административные представления.

Использование меток в запросах позволяет находить проблемные запросы и помогает определять ход выполнения посредством извлечения, преобразования и загрузки.

Хорошо продуманный способ именования упрощает работу с метками. Например, запуск метки с помощью проекта, процедуры, инструкции или комментария однозначно определяет запрос для всего кода в системе управления версиями.

Следующий запрос использует динамическое административное представление для поиска по метке:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Слово label в запросе необходимо заключать в квадратные скобки или двойные кавычки. Label — зарезервированное слово. Его использование без разделителя приводит к ошибке.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные советы по разработке см. в статье [Проектные решения и методики программирования для хранилища данных SQL](sql-data-warehouse-overview-develop.md).
