---
title: 'Назначение данных кластеру: ссылка на модуль'
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль "назначение данных в кластере" в Машинное обучение Azure для оценки модели кластеризации.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: 207172f10277589af2b22ae2f41b07234a0925b3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79477720"
---
# <a name="module-assign-data-to-clusters"></a>Модуль: назначение данных кластерам

В этой статье описывается использование модуля *Assign Data to Clusters* в машинное обучение Azure Designer (Предварительная версия). Модуль создает прогнозы с помощью модели кластеризации, обученной с помощью алгоритма *кластеризации K-средних* .

Модуль назначение данных в кластеры возвращает набор данных, который содержит возможные назначения для каждой новой точки данных. 

## <a name="how-to-use-assign-data-to-clusters"></a>Использование назначения данных кластерам
  
1. В конструкторе Машинное обучение Azure выберите ранее обученную модель кластеризации. Создать и обучить модель кластеризации можно одним из следующих способов.  
  
    - Настройте алгоритм кластеризации K-средних с помощью модуля [кластеризации k-средних](k-means-clustering.md) и обучить модель с помощью набора данных и модуля обучение модели кластеризации (в этой статье).  
  
    - Можно также добавить существующую обученную модель кластеризации из **сохраненной группы моделей** в рабочую область.

2. Присоедините обученную модель к левому порту ввода для **назначения данных кластерам**.  

3. Присоединение нового набора данных в качестве входных данных. 

   В этом наборе данных метки необязательны. Как правило, кластеризация — это неконтролируемый метод обучения. Вы не должны заранее узнавать категории. Однако входные столбцы должны быть теми же, что и столбцы, использованные при обучении модели кластеризации, иначе возникает ошибка.

    > [!TIP]
    > Чтобы уменьшить количество столбцов, записываемых в конструктор из прогнозов кластера, используйте [Выбор столбцов в наборе данных](select-columns-in-dataset.md)и выберите подмножество столбцов. 
    
4. Если требуется, чтобы результаты содержали полный входной набор данных, включая столбец, в котором отображаются результаты (назначения кластера), оставьте флажок **флажок Добавить или снять флажок только для результата** установлен.
  
    Если снять этот флажок, возвращаются только результаты. Этот параметр может быть полезен при создании прогнозов как части веб-службы.
  
5.  Отправьте конвейер.  
  
### <a name="results"></a>Результаты

+  Чтобы просмотреть значения в наборе данных, щелкните правой кнопкой мыши модуль и выберите **визуализировать**. Или выберите модуль и перейдите на вкладку **выходные данные** на правой панели, щелкните значок гистограммы в **выходных данных порта** , чтобы визуализировать результат.

