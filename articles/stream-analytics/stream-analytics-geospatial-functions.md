---
title: Сведения о геопространственных функциях Azure Stream Analytics
description: В этой статье описываются геопространственные функции, которые используются в заданиях Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: f47f34b60c858bb9a0feafd25176e4a811046630
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75426223"
---
# <a name="introduction-to-stream-analytics-geospatial-functions"></a>Сведения о геопространственных функциях Azure Stream Analytics

Геопространственные функции в Azure Stream Analytics позволяют анализировать в реальном времени потоковые геопространственные данные. Имея всего несколько строк кода, можно разработать решение для производственного уровня сложных сценариев. 

Ниже приведены примеры сценариев, которые могут использовать преимущества геопространственных функций.

* Совместное использование в поездке
* Управление автотранспортным парком
* Отслеживание данных ресурсов
* Установка геозон
* Отслеживание телефона на сотовых веб-сайтах

Язык запросов Stream Analytics содержит семь встроенных геопространственных функций: **CreateLineString**, **CreatePoint**, **CreatePolygon**, **ST_DISTANCE**, **ST_OVERLAPS**, **ST_INTERSECTS** и **ST_WITHIN**.

## <a name="createlinestring"></a>CreateLineString

Функция `CreateLineString` принимает точки и возвращает объект GeoJSON LineString, который может отображаться в виде линии на карте. Для создания экземпляра LineString необходимо иметь по крайней мере две точки. Точки LineString будут соединены по порядку.

В следующем запросе используется `CreateLineString` для создания LineString по трех точках. Первая точка создается на основе потоковых входных данных, а две других — вручную.

```SQL 
SELECT  
     CreateLineString(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5))  
FROM input  
```  

### <a name="input-example"></a>Пример входных данных  
  
|широта|долгота|  
|--------------|---------------|  
|3.0|-10,2|  
|-87,33|20,2321|  
  
### <a name="output-example"></a>Пример выходных данных  

 {"type": "LineString", "coordinates": [ [-10,2; 3,0], [10,0; 10,0], [10,5; 10,5] ]}

 {"type": "LineString", "coordinates": [ [20,2321; -87,33], [10,0; 10,0], [10,5; 10,5] ]}

Дополнительные сведения см. по ссылке [CreateLineString](https://docs.microsoft.com/stream-analytics-query/createlinestring).

## <a name="createpoint"></a>CreatePoint

Функция `CreatePoint` принимает широту и долготу и возвращает точки GeoJSON, которые можно отображать на карте. Широта и долгота должны быть типом данных **float**.

Следующий запрос использует `CreatePoint`, чтобы создать точку используя широту и долготу из потоковой передачи входных данных.

```SQL 
SELECT  
     CreatePoint(input.latitude, input.longitude)  
FROM input 
```  

### <a name="input-example"></a>Пример входных данных  
  
|широта|долгота|  
|--------------|---------------|  
|3.0|-10,2|  
|-87,33|20,2321|  
  
### <a name="output-example"></a>Пример выходных данных
  
 {"type": "Point", "coordinates": [-10,2; 3,0]}  
  
 {"type": "Point", "coordinates": [20,2321; -87,33]}  

Дополнительные сведения см. по ссылке [CreatePoint](https://docs.microsoft.com/stream-analytics-query/createpoint).

## <a name="createpolygon"></a>CreatePolygon

Функция `CreatePolygon` принимает точки и возвращает запись многоугольника GeoJSON. Порядок точек должен соответствовать ориентации справа налево или против часовой стрелки. Представьте, что вы двигаетесь от одной точки к другой по порядку, в котором они были объявлены. Центр многоугольника все время будет находиться слева.

Следующий запрос использует `CreatePolygon`, чтобы создать многоугольник на основе трех точек. Первые два пункта создаются вручную, а последняя точка — из входных данных.

```SQL 
SELECT  
     CreatePolygon(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5), CreatePoint(input.latitude, input.longitude))  
FROM input  
```  

### <a name="input-example"></a>Пример входных данных  
  
|широта|долгота|  
|--------------|---------------|  
|3.0|-10,2|  
|-87,33|20,2321|  
  
### <a name="output-example"></a>Пример выходных данных  

 {"type": "Polygon", "coordinates": [[ [-10,2; 3,0], [10,0; 10,0], [10,5; 10,5], [-10,2; 3,0] ]]}
 
 {"type": "Polygon", "coordinates": [[ [20,2321; -87,33], [10,0; 10,0], [10,5; 10,5], [20,2321; -87,33] ]]}

Дополнительные сведения см. по ссылке [CreatePolygon](https://docs.microsoft.com/stream-analytics-query/createpolygon).


## <a name="st_distance"></a>ST_DISTANCE
Функция `ST_DISTANCE` возвращает расстояние между двумя точками в метрах. 

В следующем запросе используется `ST_DISTANCE` для создания события о заправке, которая находится меньше 10 км от автомобиля.

```SQL
SELECT Cars.Location, Station.Location 
FROM Cars c  
JOIN Station s ON ST_DISTANCE(c.Location, s.Location) < 10 * 1000
```

Дополнительные сведения см. по ссылке [ST_DISTANCE](https://docs.microsoft.com/stream-analytics-query/st-distance).

## <a name="st_overlaps"></a>ST_OVERLAPS
Функция `ST_OVERLAPS` сравнивает два многоугольника. Если многоугольники перекрываются, функция возвращает значение 1. Если многоугольники не перекрываются, функция возвращает значение 0. 

В следующем запросе используется `ST_OVERLAPS` для создания события, когда здание находится в пределах возможной зоны затопления.

```SQL
SELECT Building.Polygon, Building.Polygon 
FROM Building b 
JOIN Flooding f ON ST_OVERLAPS(b.Polygon, b.Polygon) 
```

Следующий запрос создает событие, когда шторм приближается к автомобилю.

```SQL
SELECT Cars.Location, Storm.Course
FROM Cars c, Storm s
JOIN Storm s ON ST_OVERLAPS(c.Location, s.Course)
```

Дополнительные сведения см. по ссылке [ST_OVERLAPS](https://docs.microsoft.com/stream-analytics-query/st-overlaps).

## <a name="st_intersects"></a>ST_INTERSECTS
Функция `ST_INTERSECTS` сравнивает два LineString. Если LineString пересекаются, функция возвращает 1. Если LineString не пересекаются, функция возвращает 0.

Следующий пример запроса использует `ST_INTERSECTS`, чтобы определить, пересекает ли асфальтированная дорога грунтовую.

```SQL 
SELECT  
     ST_INTERSECTS(input.pavedRoad, input.dirtRoad)  
FROM input  
```  

### <a name="input-example"></a>Пример входных данных  
  
|datacenterArea|stormArea|  
|--------------------|---------------|  
|{"Type": "LineString", "координаты": [[-10,0, 0,0], [0,0, 0,0], [10,0, 0,0]]}|{"Type": "LineString", "координаты": [[0,0, 10,0], [0,0, 0,0], [0,0,-10,0]]}|  
|{"Type": "LineString", "координаты": [[-10,0, 0,0], [0,0, 0,0], [10,0, 0,0]]}|{"Type": "LineString", "координаты": [[-10,0, 10,0], [0,0, 10,0], [10,0, 10,0]]}|  
  
### <a name="output-example"></a>Пример выходных данных  

 1  
  
 0  

Дополнительные сведения см. по ссылке [ST_INTERSECTS](https://docs.microsoft.com/stream-analytics-query/st-intersects).

## <a name="st_within"></a>ST_WITHIN
Функция `ST_WITHIN` определяет есть ли точки или многоугольник внутри многоугольника. Если многоугольник содержит точки или многоугольник, функция возвращает значение 1. Функция возвращает значение 0, если точки или многоугольник не находятся в пределах объявленного многоугольника.

В следующем примере используется запрос `ST_WITHIN`, чтобы определить, находится ли точка назначения доставки внутри данного складского многоугольника.

```SQL 
SELECT  
     ST_WITHIN(input.deliveryDestination, input.warehouse)  
FROM input 
```  

### <a name="input-example"></a>Пример входных данных  
  
|deliveryDestination|Хранилище данных|  
|-------------------------|---------------|  
|{"Type": "Point", "координаты": [76,6, 10,1]}|{"Type": "Polygon", "координаты": [[0,0, 0,0], [10,0, 0,0], [10,0, 10,0], [0,0, 10,0], [0,0, 0,0]]}|  
|{"Type": "Point", "координаты": [15,0, 15,0]}|{"Type": "Polygon", "координаты": [[10,0, 10,0], [20,0, 10,0], [20,0, 20,0], [10,0, 20,0], [10,0, 10,0]]}|  
  
### <a name="output-example"></a>Пример выходных данных  

 0  
  
 1  

Дополнительные сведения см. по ссылке [ST_WITHIN](https://docs.microsoft.com/stream-analytics-query/st-within).

## <a name="next-steps"></a>Дальнейшие действия

* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
