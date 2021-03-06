---
title: Использование параметров GROUP BY в синапсе SQL
description: Синапсе SQL позволяет разрабатывать решения путем реализации различных параметров GROUP BY.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 261f75344d250ae8a8d9687f4bcd80535d11716b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429048"
---
# <a name="group-by-options-in-synapse-sql"></a>ГРУППИРОВАНие по параметрам в синапсе SQL
Синапсе SQL позволяет разрабатывать решения путем реализации различных параметров GROUP BY. 

## <a name="what-does-group-by-do"></a>Что делает GROUP BY

Предложение T-SQL [GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) используется для объединения данных в сводный набор строк.

SQL по запросу поддерживает весь диапазон параметров GROUP BY. Пул SQL поддерживает ограниченное число вариантов GROUP BY.

## <a name="group-by-options-supported-in-sql-pool"></a>Параметры GROUP BY, поддерживаемые в пуле SQL

GROUP BY имеет некоторые параметры, которые не поддерживаются пулом SQL. Эти параметры имеют следующие обходные пути:

* GROUP BY с ROLLUP;
* GROUPING SETS
* GROUP BY с CUBE.

### <a name="rollup-and-grouping-sets-options"></a>Параметры Rollup и Grouping Sets

Самый простой вариант — использовать UNION ALL для выполнения свертки, а не полагаться на явный синтаксис. Результат будет точно таким же.

В следующем примере инструкция GROUP BY используется с параметром ROLLUP:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Используя ROLLUP, приведенный выше пример запрашивает следующие объединения.

* Страна и регион
* Country
* Grand Total

Чтобы получить те же результаты без ROLLUP, можно использовать оператор UNION ALL и явным образом указать требуемые объединения.

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Для замены GROUPING SETS применяется тот же принцип. Достаточно создать разделы с UNION ALL для требуемых уровней объединения.

### <a name="cube-options"></a>Параметры Cube

Можно создать группу с КУБОМ с помощью метода UNION ALL. Проблема в том, что в этом случае код может быстро разрастись и стать слишком громоздким. Для устранения этой проблемы можно использовать более сложный подход.

Для начала нужно задать «куб», определяющий все уровни группирования данных, которые нам нужно создать. Обратите внимание на перекрестное соединение двух производных таблиц при формировании всех уровней. Остальная часть кода содержит текст для форматирования.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

На следующем рисунке показаны результаты [CREATE TABLE как SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest):

![GROUP BY с CUBE](./media/develop-group-by-options/develop-group-by-cube.png)

Вторым шагом является указание целевой таблицы для хранения промежуточных результатов:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Третий шаг — цикл по Кубу столбцов, выполняющих статистическую обработку. Запрос будет выполнен один раз для каждой строки во временной таблице #Cube. Результаты хранятся во временной таблице #Results:

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Наконец, результаты можно вернуть, читая из #Results временной таблицы:

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Разбивая код на разделы и создавая циклическую конструкцию, код станет более управляемым и обслуживаемым.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные советы по разработке см. в статье [Проектные решения и методики программирования для хранилища данных SQL](develop-overview.md).
