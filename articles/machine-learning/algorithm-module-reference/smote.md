---
title: SMOTE
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль СМОТЕ в Машинное обучение Azure для увеличения числа недорогих примеров в наборе данных с помощью избыточной выборки.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: ed6d9e86143c3a5d6c97c4bd92a07c258bbd1bbc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79477465"
---
# <a name="smote"></a>SMOTE

В этой статье описывается использование модуля СМОТЕ в конструкторе Машинное обучение Azure (Предварительная версия) для увеличения числа недопустимых вариантов в наборе данных, используемом для машинного обучения. СМОТЕ — лучший способ увеличения количества редких случаев, чем просто дублирование существующих вариантов.  

Модуль СМОТЕ подключается к *несбалансированному*набору данных. Существует множество причин, по которым набор данных может быть несбалансированным. Например, категория, для которой вы нацеливание, может быть редкой в совокупности, или данные могут быть трудно собраны. Как правило, СМОТЕ используется, когда *класс* , который нужно проанализировать, является недопустимым. 
  
Модуль возвращает набор данных, содержащий исходные образцы. Он также возвращает ряд искусственных долей, в зависимости от указанного процента.  
  
## <a name="more-about-smote"></a>Дополнительные сведения о СМОТЕ

Искусственный метод избыточной доли миноритария (СМОТЕ) — это статистический метод для равномерного увеличения количества вариантов в наборе данных. Модуль работает путем создания новых экземпляров на основе существующих миноритарных вариантов, которые указываются в качестве входных данных. Эта реализация СМОТЕ *не* изменяет число большинства случаев.

Новые экземпляры являются не только копиями существующих вариантов миноритария. Вместо этого алгоритм принимает выборки *пространства функций* для каждого целевого класса и его ближайших соседей. Затем алгоритм создает новые примеры, объединяющие функции целевого варианта с функциями его соседей. Такой подход увеличивает функции, доступные для каждого класса и обобщает выборку.
  
СМОТЕ принимает весь набор данных как входные данные, но увеличивает процентное соотношение только доли миноритария. Например, предположим, что у вас есть несбалансированный набор данных, где только 1 процент вариантов имеют целевое значение A (класс миноритария) и 99 процентов вариантов имеют значение B. Чтобы увеличить процент вариантов доли в два раза больше предыдущего процентного значения, введите **200** для **смоте** в свойствах модуля.  
  
## <a name="examples"></a>Примеры  

Рекомендуем попробовать использовать модуль SMOTE с меньшим набором данных, чтобы увидеть, как он работает. В следующем примере используется набор данных о пожертвовании крови, доступный в Машинное обучение Azure Designer.
  
Если вы добавите набор данных в конвейер и выберете **визуализацию** на выходе набора данных, можно увидеть, что из 748 строк или вариантов в наборе данных, 570 вариантов (76 процентов) имеют класс 0, а 178 варианта (24 процента) имеют класс 1. Хотя этот результат не является несбалансированным, класс 1 представляет пользователей, которые поддержали кровь, поэтому эти строки содержат *пространство функций* , которое необходимо смоделировать.
 
Чтобы увеличить количество вариантов, можно задать значение **смоте в процентах**, используя кратные 100, как показано ниже.

||Класс 0|Класс 1|total|  
|-|-------------|-------------|-----------|  
|Исходный набор данных<br /><br /> (эквивалентно **смоте проценту** = **0**)|570<br /><br /> 76%|178<br /><br /> круглосуточно|748|  
|**Смоте в процентах** = **100**|570<br /><br /> 62%|356<br /><br /> 38%|926|  
|**Смоте в процентах** = **200**|570<br /><br /> 52%|534<br /><br /> 48 %|1104|  
|**Смоте в процентах** = **300**|570<br /><br /> 44%|712<br /><br /> 56%|1 282|  
  
> [!WARNING]
> Увеличение количества вариантов с помощью СМОТЕ не гарантирует получения более точных моделей. Попробуйте выполнить конвейер с различными процентами, различными наборами функций и различными числами ближайших соседей, чтобы увидеть, как добавление вариантов влияет на модель.  
  
## <a name="how-to-configure-smote"></a>Настройка СМОТЕ
  
1.  Добавьте модуль СМОТЕ в конвейер. Модуль можно найти в разделе **модули преобразования данных**в категории **Управление** .

2. Подключите набор данных, который требуется увеличить. Если необходимо указать пространство функций для создания новых вариантов, либо с помощью только определенных столбцов, либо путем исключения некоторых, используйте модуль [Выбор столбцов в наборе данных](select-columns-in-dataset.md) . Затем можно изолировать столбцы, которые вы хотите использовать, прежде чем использовать СМОТЕ.
  
    В противном случае создание новых вариантов через СМОТЕ будет основано на *всех* столбцах, предоставленных в качестве входных данных. По крайней мере один столбец в столбцах компонента является числовым.
  
3.  Убедитесь, что выбран столбец, содержащий метку или целевой класс. СМОТЕ принимает только двоичные метки.
  
4.  Модуль СМОТЕ автоматически определяет класс миноритария в столбце Метка, а затем получает все примеры для класса миноритария. Все столбцы не могут иметь значений NaN.
  
5.  В параметре **смоте в процентах** введите целое число, указывающее целевой процент вариантов доли миноритария в выходном наборе данных. Пример:  
  
    - Введите **0**. Модуль СМОТЕ возвращает тот же набор данных, который вы указали как входные. В этом случае не добавляются новые доли миноритария. В этом наборе данных класс пропорции не изменился.  
  
    - Вы вводите **100**. Модуль СМОТЕ создает новые доли миноритария. Он добавляет то же количество доли миноритария, которое было в исходном наборе данных. Так как СМОТЕ не увеличивает количество большинства случаев, изменилась часть вариантов каждого класса.  
  
    - Вы вводите **200**. Модуль удваивает долю доли миноритария по сравнению с исходным набором данных. Это *не* приводит к удвоению числа таких случаев, как раньше. Вместо этого размер набора данных увеличивается таким образом, что количество большинства вариантов остается неизменным. Количество случаев роста доли увеличивается до тех пор, пока оно не будет соответствовать требуемому процентному значению.  
  
    > [!NOTE]
    > Используйте только кратные 100 для СМОТЕ в процентах.

6.  Используйте параметр **число ближайших соседей** , чтобы определить размер пространства функций, используемого алгоритмом смоте при построении новых вариантов. Ближайший сосед — это строка данных (случай), похожая на целевой вариант. Расстояние между двумя случаями измеряется путем объединения взвешенных векторов всех признаков.  
  
    + Увеличивая число ближайших соседей, вы получаете функции в большинстве случаев.
    + Сохраняя число ближайших соседних соседей, вы используете функции, которые более близки к исходным примерам.  
  
7. Введите значение в поле **начальный случай** , если необходимо обеспечить одинаковые результаты при выполнении одного и того же конвейера с теми же данными. В противном случае модуль создает случайное начальное значение на основе значений тактовых частот процессора при развертывании конвейера. Создание случайного начального значения может привести к слегка разным результатам при выполнении.

8. Отправьте конвейер.  
  
   Выходные данные модуля — это набор данных, содержащий исходные строки плюс ряд добавленных строк с долями миноритария.  

## <a name="technical-notes"></a>Технические примечания

+ При публикации модели, использующей модуль **смоте** , удалите **смоте** из прогнозного конвейера перед его публикацией в качестве веб-службы. Причина заключается в том, что СМОТЕ предназначен для улучшения модели во время обучения, а не для оценки. Если опубликованный прогнозный конвейер содержит модуль СМОТЕ, может возникнуть ошибка.

+ Часто можно получить лучшие результаты, если очистить отсутствующие значения или применить другие преобразования для исправления данных перед применением СМОТЕ. 

+ Некоторые исследователи просматривают, эффективно ли СМОТЕ для больших или разреженных данных, таких как данные, используемые в классификации текста или наборы данных Genomics. В этом документе есть хорошая сводка по влиянию и теоретической допустимости применения СМОТЕ в таких случаях: [благус и Луса: смоте для больших данных несбалансированных классов](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-106).

+ Если СМОТЕ не действует в наборе данных, можно использовать и другие подходы:
  + Методы для избыточной выборки доли миноритария или недовыборки в большинстве случаев.
  + Ансамблей методики, которые облегчают обучение непосредственно с помощью кластеризации, баггинг или адаптивного повышения.


## <a name="next-steps"></a>Дальнейшие шаги

См. [набор модулей, доступных](module-reference.md) для машинное обучение Azure. 

