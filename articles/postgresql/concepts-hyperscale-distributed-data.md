---
title: Распределенные данные — масштабирование (Цитус) — база данных Azure для PostgreSQL
description: Сведения о распределенных таблицах, ссылочных таблицах, локальных таблицах и сегментах в базе данных Azure для PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: ade7632dc042741a07bdb59e34e30b3fb464e0e9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79243657"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus"></a>Распределенные данные в базе данных Azure для PostgreSQL — масштабирование (Цитус)

В этой статье описаны три типа таблиц в базе данных Azure для PostgreSQL — масштабирование (Цитус).
В нем показано, как распределенные таблицы хранятся в виде сегментов и как сегменты размещаются на узлах.

## <a name="table-types"></a>типы таблиц

Существует три типа таблиц в группе серверов с горизонтальным масштабированием (Цитус), которые используются для разных целей.

### <a name="type-1-distributed-tables"></a>Тип 1: распределенные таблицы

Первый и наиболее распространенный тип — это распределенные таблицы. Они выглядят как обычные таблицы для инструкций SQL, но они горизонтально секционированы между рабочими узлами. Это означает, что строки таблицы хранятся на разных узлах в таблицах фрагментов, называемых сегментами.

В процессе масштабирования (Цитус) выполняется не только SQL, но и инструкции DDL в кластере.
Изменение схемы распределенной таблицы каскадом для обновления всех сегментов таблицы между рабочими процессами.

#### <a name="distribution-column"></a>Столбец распределения

Функция "масштабирование" (Цитус) использует алгоритм сегментирования для назначения строк сегментам. Присваивание выполняется детерминированно на основе значения столбца таблицы, называемого столбцом распределения. Администратор кластера должен назначить этот столбец при распространении таблицы.
Правильный вариант важен для производительности и функциональности.

### <a name="type-2-reference-tables"></a>Тип 2: ссылочные таблицы

Ссылочная таблица — это тип распределенной таблицы, все содержимое которого Concentrated в один сегмент. Сегмент реплицируется на каждом рабочем процессе. Запросы к любой рабочей роли могут обращаться к справочным сведениям локально, не требуя от сети дополнительных расходов на запрос строк из другого узла. Ссылочные таблицы не имеют столбца распределения, поскольку нет необходимости различать отдельные сегменты в строке.

Ссылочные таблицы обычно невелики и используются для хранения данных, относящихся к запросам, выполняемым на любом рабочем узле. Примером являются перечислимые значения, такие как состояния заказов или категории продуктов.

### <a name="type-3-local-tables"></a>Тип 3: локальные таблицы

При использовании масштабирования (Цитус) узел координатора, к которому выполняется подключение, является обычной базой данных PostgreSQL. Вы можете создать обычные таблицы в координаторе и отказаться от их сегментирования.

Хорошим кандидатом для локальных таблиц являются небольшие административные таблицы, которые не участвуют в запросах JOIN. Примером является таблица Users для входа и проверки подлинности приложения.

## <a name="shards"></a>Сегментов

В предыдущем разделе описано, как распределенные таблицы хранятся в качестве сегментов на рабочих узлах. В этом разделе рассматриваются дополнительные технические подробности.

Таблица `pg_dist_shard` метаданных в координаторе содержит по одной строке для каждого сегмента распределенной таблицы в системе. Строка соответствует ИДЕНТИФИКАТОРу сегмента с диапазоном целых чисел в пространстве хэша (шардминвалуе, шардмаксвалуе).

```sql
SELECT * from pg_dist_shard;
 logicalrelid  | shardid | shardstorage | shardminvalue | shardmaxvalue 
---------------+---------+--------------+---------------+---------------
 github_events |  102026 | t            | 268435456     | 402653183
 github_events |  102027 | t            | 402653184     | 536870911
 github_events |  102028 | t            | 536870912     | 671088639
 github_events |  102029 | t            | 671088640     | 805306367
 (4 rows)
```

Если узел координатор хочет определить, какой сегмент содержит строку `github_events`, он хэширует значение столбца распределения в строке. Затем узел проверяет, какой диапазон\'сегментов содержит хэшированное значение. Диапазоны определяются таким образом, чтобы изображение хэш-функции было несвязанным объединением.

### <a name="shard-placements"></a>Размещения сегментов

Предположим, что сегмент 102027 связан с рассматриваемой строкой. Строка считывается или записывается в таблицу, которая `github_events_102027` называется одним из рабочих ролей. Какой работник? Это полностью определено таблицами метаданных. Сопоставление сегмента и рабочей роли называется размещением сегментов.

Узел координатор переписывает запросы в фрагменты, которые ссылаются на конкретные таблицы, например `github_events_102027` , и выполняет эти фрагменты на соответствующих рабочих процессах. Ниже приведен пример запроса, выполняемого в фоновом режиме для поиска узла с ИДЕНТИФИКАТОРом сегмента 102027.

```sql
SELECT
    shardid,
    node.nodename,
    node.nodeport
FROM pg_dist_placement placement
JOIN pg_dist_node node
  ON placement.groupid = node.groupid
 AND node.noderole = 'primary'::noderole
WHERE shardid = 102027;
```

    ┌─────────┬───────────┬──────────┐
    │ shardid │ nodename  │ nodeport │
    ├─────────┼───────────┼──────────┤
    │  102027 │ localhost │     5433 │
    └─────────┴───────────┴──────────┘

## <a name="next-steps"></a>Дальнейшие шаги
- Узнайте, как [выбрать столбец распределения](concepts-hyperscale-choose-distribution-column.md) для распределенных таблиц.
