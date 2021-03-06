---
title: НИЖЕ Azure Cosmos DB язык запросов
description: Сведения о более РАННЕй системной функции SQL в Azure Cosmos DB для возврата строкового выражения после преобразования символьных данных в нижний регистр
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 80dba57d4fe05630eb5ae4f8fc96bd0aa214c6d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78302276"
---
# <a name="lower-azure-cosmos-db"></a>НИЖЕ (Azure Cosmos DB)
 Возвращает строковое выражение после преобразования символов верхнего регистра в нижний.  

Нижняя системная функция не использует индекс. Если планируется регулярное сравнение без учета регистра, то более низкая системная функция может потреблять значительное количество единиц запросов. В этом случае вместо использования нижней системной функции для нормализации данных каждый раз при сравнении можно нормализовать регистр при вставке. Затем запрос, например SELECT * FROM c, где ниже (c. Name) = ' Bob ', просто превращается в SELECT * FROM c, где c.name = ' Bob '.

## <a name="syntax"></a>Синтаксис
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Аргументы
  
*str_expr*  
   Является строковым выражением.  
  
## <a name="return-types"></a>Типы возвращаемых данных
  
  Возвращает строковое выражение.  
  
## <a name="examples"></a>Примеры
  
  В следующем примере показано, как использовать `LOWER` в запросе.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 Результирующий набор:  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="remarks"></a>Remarks

Эта системная функция не будет использовать индекс.

## <a name="next-steps"></a>Дальнейшие шаги

- [Строковые функции Azure Cosmos DB](sql-query-string-functions.md)
- [Системные функции Azure Cosmos DB](sql-query-system-functions.md)
- [Знакомство с Azure Cosmos DB](introduction.md)
