---
title: Назначение переменных с помощью синапсе SQL
description: В этой статье вы найдете советы по назначению переменных T-SQL с помощью синапсе SQL.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: b2a596b71ee7e5f58e01d5bc10b330f6f54a69d2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81428671"
---
# <a name="assigning-variables-with-synapse-sql"></a>Назначение переменных с помощью синапсе SQL

В этой статье вы найдете советы по назначению переменных T-SQL с помощью синапсе SQL.

## <a name="setting-variables-with-declare"></a>Задание переменных с помощью DECLARE

Переменные в синапсе SQL задаются с `DECLARE` помощью инструкции или `SET` инструкции. Инициализация переменных с помощью DECLARE является одним из наиболее гибких способов установки значения переменной в синапсе SQL.

```sql
DECLARE @v  int = 0
;
```

С помощью DECLARE можно задать одновременно несколько переменных. Нельзя использовать SELECT или UPDATE для следующих задач:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Нельзя инициализировать и использовать переменную в одной и той же инструкции DECLARE. Для иллюстрации следующий пример не допускается, так как *@p1* инициализируется и используется в одной и той же инструкции DECLARE. Следующий пример приводит к возникновению ошибки.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Задание значений с помощью SET

SET — это очень распространенный метод задания одной переменной.

Ниже перечислены допустимые способы задания значения с помощью инструкции SET:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

С помощью SET можно одновременно задать только одну переменную. Тем не менее допускаются составные операторы.

## <a name="limitations"></a>Ограничения

Нельзя использовать UPDATE для назначения переменных.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные советы по разработке см. в статье [Общие сведения о разработке СИНАПСЕ SQL](develop-overview.md) .