---
title: Обучение модели. Custom Translator
titleSuffix: Azure Cognitive Services
description: Обучение модели является важным шагом при создании модели перевода. Обучение происходит на основе выбранных для него документов.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: c29a0b8b429705bb0315c37fc6fe63eb8d77511f
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996709"
---
# <a name="train-a-model"></a>Обучение модели

Обучение модели — это важный шаг к созданию модели перевода, так как без обучения ее создать невозможно. Обучение происходит на основе выбранных для него документов.

Чтобы обучить модель:

1.  Выберите проект, где вы хотите создать модель.

2.  На вкладке "Данные" для проекта отображаются все соответствующие документы для языковой пары. Вручную выберите документы, которые вы хотите использовать для обучения модели. На этом экране можно выбрать данные для обучения, настройки и тестирования. Вы также просто выбираете набор для обучения, а Custom Translator создает для вас наборы для настройки и тестирования.

    -  Название документа. Имя документа.

    -  Связывание. Каким является этот документ: одноязычным или параллельным. Одноязычные документы в настоящее время не поддерживаются для обучения.

    -  Тип документа. Может быть обучением, настройкой, тестированием или словарем.

    -  Языковая пара. Исходный и целевой языки для проекта.

    -  Исходные предложения: показывает количество предложений, извлеченных из исходного файла.

    -  Целевые предложения: показывает количество предложений, извлеченных из целевого файла.

    ![Обучение модели](media/how-to/how-to-train-model.png)

3.  Нажмите кнопку "Обучение".

4.  В диалоговом окне укажите имя модели.

5.  Выберите "Обучение модели".

    ![Диалоговое окно "Обучение модели"](media/how-to/how-to-train-model-2.png)

6.  Custom Translator выполнит обучение и отобразит состояние обучения на вкладке моделей.

    ![Страница "Обучение модели"](media/how-to/how-to-train-model-3.png)

>[!Note]
>Пользовательский Переводчик поддерживает 10 одновременных обучения в рабочей области в любой момент времени.


## <a name="edit-a-model"></a>Изменение модели

Вы можете изменить модель, используя ссылку "Изменить" на странице сведений о модели.

1.  Щелкните значок карандаша.

    ![Изменение модели](media/how-to/how-to-edit-model.png)

2.  В диалоговом окне измените:

    1.  Поле "Имя модели" (обязательно). Присвойте своей модели понятное имя.

        ![Диалоговое окно изменения сведений о модели](media/how-to/how-to-edit-model-dialog.png)

3.  Нажмите кнопку «Сохранить».


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, [как просмотреть сведения о модели](how-to-view-model-details.md).
