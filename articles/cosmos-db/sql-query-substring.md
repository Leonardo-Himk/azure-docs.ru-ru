---
title: Подстрока в языке запросов Azure Cosmos DB
description: Сведения о подстроке системной функции SQL в Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d4462fc407093b23510bddfae4d9f55d68f8c0fa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78303704"
---
# <a name="substring-azure-cosmos-db"></a>Подстрока (Azure Cosmos DB)
 Возвращает часть строкового выражения, начиная с указанной позиции (отсчет начинается с нуля) и до достижения указанной длины (или до конца строки).  
  
## <a name="syntax"></a>Синтаксис
  
```sql
SUBSTRING(<str_expr>, <num_expr1>, <num_expr2>)  
```  
  
## <a name="arguments"></a>Аргументы
  
*str_expr*  
   Является строковым выражением.
  
*num_expr1*  
   Числовое выражение для обозначения начального символа. Значение 0 является первым символом *str_expr*.
  
*num_expr2*  
   Числовое выражение для обозначения максимального числа символов, возвращаемых *str_expr* . Значение 0 или меньше приводит к пустой строке.

## <a name="return-types"></a>Типы возвращаемых данных
  
  Возвращает строковое выражение.  
  
## <a name="examples"></a>Примеры
  
  Пример ниже возвращает подстроку "abc", которая начинается с 1 и имеет длину в 1 знак.  
  
```sql
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Результирующий набор:  
  
```json
[{"substring": "b"}]  
```

## <a name="remarks"></a>Remarks

Эта системная функция будет использовать [индекс диапазона](index-policy.md#includeexclude-strategy) , если начальная позиция — `0`.

## <a name="next-steps"></a>Дальнейшие шаги

- [Строковые функции Azure Cosmos DB](sql-query-string-functions.md)
- [Системные функции Azure Cosmos DB](sql-query-system-functions.md)
- [Знакомство с Azure Cosmos DB](introduction.md)
