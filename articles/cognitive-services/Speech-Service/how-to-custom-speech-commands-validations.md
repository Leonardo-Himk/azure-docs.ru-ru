---
title: Как добавить проверки в пользовательские параметры команды
titleSuffix: Azure Cognitive Services
description: В этой статье объясняется, как добавить проверки в параметр в пользовательских командах.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: 2b7fd608156ab269cfc0c85c6c508fa9d5eebc83
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857191"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Как добавить проверки в пользовательские параметры команды (Предварительная версия)

В этой статье вы добавите проверки в параметры и запросите исправление.

## <a name="prerequisites"></a>Предварительные требования

Необходимо выполнить действия, описанные в следующих статьях:

> [!div class="checklist"]
> * [Краткое руководство. Создание настраиваемой команды](./quickstart-custom-speech-commands-create-new.md)
> * [Краткое руководство. Создание пользовательской команды с параметрами](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Создание команды SetTemperature

Чтобы продемонстрировать проверки, создадим новую команду, позволяющую пользователям устанавливать температуру.

1. Открытие созданного ранее приложения настраиваемых команд в [Speech Studio](https://speech.microsoft.com/)
1. Создание новой команды`SetTemperature`
1. Добавьте параметр для целевой температуры.

   | Конфигурация параметров           | Рекомендуемое значение    |Описание                 |                                    
   | ----------------- | ----------------------------------| -------------|
   | Имя              | температура;                       | Описательное имя параметра                                |
   | Обязательно          | checked                           | Флажок, указывающий, требуется ли значение этого параметра перед выполнением команды |
   | Ответ на обязательный параметр     | Простой редактор — >, какую температуру вам хотелось бы?  | Запрос на запрос значения этого параметра, если он неизвестен |
   | type              | Число                            | Тип параметра, например число, строка, Дата и время или география   |

1. Добавьте проверку для параметра температуры.

    - На странице Конфигурация **параметров** для `Temperature` параметра выберите `Add a validation` в разделе проверки.
    - Введите значения во всплывающем окне **Новая проверка** , как показано ниже, и выберите **создать**.

  
       | Конфигурация параметров         | Рекомендуемое значение                                          | Описание                                                                        |
       | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
       | Минимальное значение        | 60               | Для числовых параметров — минимальное значение, которое может предположить этот параметр. |
       | Максимальное значение        | 80               | Для числовых параметров — максимальное значение, которое может предположить этот параметр. |
       | Неудачный ответ — простой редактор| Первая разновидность, я могу установить только от 60 до 80 градусов.      | Запрашивать новое значение, если проверка не пройдена                                       |

       > [!div class="mx-imgBorder"]
       > ![Добавление проверки диапазона](media/custom-speech-commands/validations-add-temperature.png)

1. Добавление примеров предложений

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Добавление правила завершения для подтверждения результата

   | Параметр    | Рекомендуемое значение                                           |Описание                                     |
   | ---------- | --------------------------------------------------------- |-----|
   | Имя       | Сообщение с подтверждением                                      |Имя, описывающее назначение правила. |
   | Условия | Обязательные параметры-`Temperature`                       |Условия, определяющие, когда может выполняться правило    |   
   | Действия    | Отправка речевого ответа-`Ok, setting temperature to {Temperature} degrees` | Действие, выполняемое, если условие правила имеет значение true |

> [!TIP]
> В этом примере для подтверждения результата используется речевой ответ. Примеры выполнения команды с помощью действия клиента см. в разделе [как выполнять команды на клиенте с помощью пакета SDK для распознавания речи](./how-to-custom-speech-commands-fulfill-sdk.md) .


## <a name="try-it-out"></a>Попробуйте!
1. Значок `Train` выбора находится в верхней части правой панели.

1. После завершения обучения выберите `Test` и попробуйте несколько взаимодействий.

    - Входные данные: установите температуру в 72 градусов.
    - Выходные данные: ОК, установка температуры в 72 градусов
    - Входные данные: установите температуру в 45 градусов.
    - Выходные данные: приносим к сожалению, можно задать значение в диапазоне от 60 до 80 градусов
    - Входные данные: сделайте его 72 градусов
    - Выходные данные: ОК, установка температуры в 72 градусов

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Как добавить подтверждение в настраиваемую команду (Предварительная версия)](./how-to-custom-speech-commands-confirmations.md)
