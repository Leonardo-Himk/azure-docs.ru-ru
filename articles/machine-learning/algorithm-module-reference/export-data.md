---
title: 'Экспорт данных: ссылка на модуль'
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль Export Data (экспорт данных) в Машинное обучение Azure для сохранения результатов, промежуточных данных и рабочих данных из конвейеров в конечные точки облачного хранилища, находящиеся за пределами Машинное обучение Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 807771fd4018c9666f059c965370ebc36d0105df
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79456307"
---
# <a name="export-data-module"></a>Экспорт модуля данных

В этой статье описывается модуль в Машинное обучение Azure Designer (Предварительная версия).

Используйте этот модуль для сохранения результатов, промежуточных данных и рабочих данных из конвейеров в конечных объектах облачного хранилища. 

Этот модуль поддерживает экспорт данных в следующие облачные службы данных:

- Контейнер больших двоичных объектов Azure
- Общая папка Azure
- Azure Data Lake
- Azure Data Lake 2-го поколения

Прежде чем экспортировать данные, необходимо сначала зарегистрировать хранилище данных в рабочей области Машинное обучение Azure. Дополнительные сведения см. [в статье доступ к данным в службах хранилища Azure](../how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Настройка экспорта данных

1. Добавьте модуль **Export Data (экспорт данных** ) в конвейер в конструкторе. Этот модуль можно найти в категории **входные и выходные данные** .

1. Подключите **Экспорт данных** к модулю, содержащему данные, которые необходимо экспортировать.

1. Выберите **Экспорт данных** , чтобы открыть панель **свойств** .

1. В поле **хранилище данных**выберите существующее хранилище данных из раскрывающегося списка. Можно также создать новое хранилище данных. Узнайте, как [открыть доступ к данным в службах хранилища Azure](../how-to-access-data.md).

1. Флажок **повторно создать выходные данные**определяет, следует ли выполнять модуль для повторного создания выходных данных во время выполнения. 

    По умолчанию он не выбран. Это означает, что если модуль выполнялся с теми же параметрами ранее, система повторно использует выходные данные последнего запуска для сокращения времени выполнения. 

    Если он установлен, система снова выполнит модуль для повторного создания выходных данных.

1. Определите путь в хранилище данных, где находятся данные. Путь представляет собой относительный путь. Пустые пути или URL-пути не допускаются.


1. В поле **Формат файла**выберите формат, в котором должны храниться данные.
 
1. Отправьте конвейер.

## <a name="next-steps"></a>Дальнейшие шаги

См. [набор модулей, доступных](module-reference.md) для машинное обучение Azure. 
