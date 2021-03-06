---
title: Преобразование данных
titleSuffix: Azure Machine Learning
description: Сведения о преобразовании данных в конструкторе Машинное обучение Azure для создания собственных наборов данных.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 05/04/2020
ms.openlocfilehash: 5296ac54cab403ef78b3e8bd32fe5ebe6ea43119
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82842881"
---
# <a name="transform-data-in-azure-machine-learning-designer-preview"></a>Преобразование данных в конструкторе Машинное обучение Azure (Предварительная версия)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Из этой статьи вы узнаете, как преобразовать и сохранить наборы данных в конструкторе Машинное обучение Azure, чтобы вы могли подготовить собственные данные для машинного обучения.

Для подготовки двух наборов данных вы будете использовать набор данных о параметрах [двоичной классификации для взрослых](sample-designer-datasets.md) с невзрослыми переписями, который содержит только США и другой набор данных, включающий переписную информацию от взрослых.

Вы узнаете, как выполнять следующие задачи:

1. Преобразование набора данных для подготовки его к обучению.
1. Экспортируйте результирующие наборы данных в хранилище данных.
1. Просмотр результатов.

Это руководство является необходимым условием для статьи [как переучить модели конструктора](how-to-retrain-designer.md) . В этой статье вы узнаете, как использовать преобразованные наборы данных для обучения нескольких моделей с параметрами конвейера.

## <a name="transform-a-dataset"></a>Преобразование набора данных

В этом разделе вы узнаете, как импортировать образец набора данных и разделить данные на наборы данных US и non-US. Дополнительные сведения о том, как импортировать данные в конструктор, см. в разделе [Импорт данных](how-to-designer-import-data.md).

### <a name="import-data"></a>Импорт данных

Чтобы импортировать образец набора данных, выполните следующие действия.

1. Войдите на сайт <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a> и выберите рабочую область, с которой хотите работать.

1. Перейдите в конструктор. Выберите **простые для использования модули** , чтобы создать новый конвейер.

1. Выберите целевой объект вычислений по умолчанию для запуска конвейера.

1. Слева на холсте конвейера расположена палитра наборов данных и модулей. Выберите **наборы данных**. Затем просмотрите раздел **Samples** .

1. Перетащите на холст набор данных с **двоичной классификацией доходов для взрослых** .

1. Выберите модуль набора данных для **взрослых переписей доходов** .

1. В области сведений, расположенной справа от холста, выберите **выходные данные**.

1. Выберите значок "Визуализация" ![значок визуализации](media/how-to-designer-transform-data/visualize-icon.png).

1. Используйте окно Предварительный просмотр данных для просмотра набора данных. Обратите внимание на значения столбцов "собственная страна".

### <a name="split-the-data"></a>Разделение данных

В этом разделе вы используете [модуль Split Data (разделение данных](algorithm-module-reference/split-data.md) ) для обнаружения и разбиения строк, содержащих "Соединенные Штаты", в столбце "собственная страна". 

1. В палитре модулей слева от холста разверните раздел **Преобразование данных** и найдите модуль **Split Data (разделение данных** ).

1. Перетащите модуль **Split Data (разделение данных** ) на холст и удалите модуль под модулем набора данных.

1. Подключите модуль набора данных к модулю **Split Data (разделение данных** ).

1. Выберите модуль **Split Data** (Разделение данных).

1. В области сведения о модуле справа от холста установите **режим разделения** на **регулярное выражение**.

1. Введите **регулярное выражение**: `\"native-country" United-States`.

    Режим **регулярного выражения** проверяет отдельный столбец на наличие значения. Дополнительные сведения о модуле Split Data см. на [странице справочника по связанному модулю алгоритма](algorithm-module-reference/split-data.md).

Теперь конвейер будет выглядеть следующим образом:

![Снимок экрана, показывающий, как настроить конвейер и модуль Split Data](media/how-to-designer-transform-data/split-data.png).


## <a name="save-the-datasets"></a>Сохранение наборов данных

Теперь, когда ваш конвейер настроен на разделение данных, необходимо указать, где следует хранить наборы DataSet. В этом примере используйте модуль **Export Data (экспорт данных** ), чтобы сохранить набор данных в хранилище. Дополнительные сведения о хранилищах данных см. в статье [Подключение к службам хранилища Azure](how-to-access-data.md) .

1. В палитре модулей слева от холста разверните раздел **входные и выходные данные** и найдите модуль **Экспорт данных** .

1. Перетащите два модуля **экспорта данных** под модуль **Split Data (разделение данных** ).

1. Подключите каждый порт вывода модуля **Split Data (разделение данных** ) к другому модулю **экспорта данных** .

    Конвейер должен выглядеть примерно так:

    ![Снимок экрана, показывающий, как подключить модули экспорта данных](media/how-to-designer-transform-data/export-data-pipeline.png).

1. Выберите модуль **Export Data (экспорт данных** ), подключенный к *левому*порту модуля **Split Data (разделение данных** ).

    Порядок выходных портов для модуля **Split Data (разделение данных** ). Первый выходной порт содержит строки, в которых регулярное выражение имеет значение true. В этом случае первый порт содержит строки дохода на основе США, а второй порт содержит строки для дохода, отличного от США.

1. В области сведения о модуле справа от холста задайте следующие параметры.
    
    **Тип хранилища**данных: хранилище BLOB-объектов Azure

    **Хранилище данных**: выберите существующее хранилище данных или выберите "новое хранилище данных", чтобы создать его.

    **Путь**:`/data/us-income`

    **Формат файла**: CSV

    > [!NOTE]
    > В этой статье предполагается, что у вас есть доступ к хранилищу данных, зарегистрированному в текущей рабочей области Машинное обучение Azure. Инструкции по настройке хранилища данных см. в статье [Подключение к службам хранилища Azure](how-to-access-data.md#azure-machine-learning-studio).

    Если у вас нет хранилища данных, вы можете создать его сейчас. Например, в этой статье наборы данных будут сохранены в учетной записи хранения BLOB-объектов по умолчанию, связанной с рабочей областью. Наборы данных будут сохранены в `azureml` контейнере в новой папке с именем. `data`

1.  Выберите модуль **Export Data (экспорт данных** ), подключенный к крайнему *правому*порту модуля **Split Data (разделение данных** ).

1. В области сведения о модуле справа от холста задайте следующие параметры.
    
    **Тип хранилища**данных: хранилище BLOB-объектов Azure

    **Хранилище данных**. Выберите то же хранилище данных, которое было показано выше.

    **Путь**:`/data/non-us-income`

    **Формат файла**: CSV

1. Убедитесь, что модуль **Export Data (экспорт данных** ), подключенный к левому порту **разделенных данных** , имеет **путь** `/data/us-income`.

1. Убедитесь, что для модуля **Экспорт данных** , подключенного к нужному порту, указан **путь** `/data/non-us-income`.

    Ваш конвейер и параметры должны выглядеть следующим образом:
    
    ![Снимок экрана, показывающий, как настроить модули экспорта данных](media/how-to-designer-transform-data/us-income-export-data.png).

### <a name="submit-the-run"></a>Отправка запуска

Теперь, когда ваш конвейер настроен для разбиения и экспорта данных, отправьте выполнение конвейера.

1. В верхней части холста выберите **Отправить**.

1. В диалоговом окне **Настройка выполнения конвейера** выберите **создать** , чтобы создать эксперимент.

    Эксперименты логически группируют связанные запуски конвейера. При запуске этого конвейера в будущем следует использовать тот же эксперимент для ведения журнала и отслеживания.

1. Введите описательное имя эксперимента, например "Split-переписи данных".

1. Нажмите кнопку **Submit** (Отправить).

## <a name="view-results"></a>Просмотр результатов

После завершения выполнения конвейера можно просмотреть результаты, перейдя к хранилищу BLOB-объектов в портал Azure. Вы также можете просмотреть промежуточные результаты модуля **Split Data (разделение данных** ), чтобы убедиться в том, что данные были разделены правильно.

1. Выберите модуль **Split Data** (Разделение данных).

1. В области "Сведения о модуле" справа от холста щелкните элемент **Outputs + logs** (Выходные данные и журналы). 

1. Щелкните значок визуализировать ![значок визуализации](media/how-to-designer-transform-data/visualize-icon.png) рядом с полем **результаты DataSet1**. 

1. Убедитесь, что столбец "собственная-страна" содержит только значение "Соединенные Штаты".

1. Щелкните значок визуализировать ![значок визуализации](media/how-to-designer-transform-data/visualize-icon.png) рядом с пунктом **Results DataSet2 (результаты поиска**). 

1. Убедитесь в том, что столбец "собственная страна" не содержит значение "Соединенные Штаты".

## <a name="clean-up-resources"></a>Очистка ресурсов

Пропустите этот раздел, если вы хотите перейти к части 2 этого руководства, [переучить модели с помощью конструктора машинное обучение Azure](how-to-retrain-designer.md).

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали, как преобразовать набор данных и сохранить его в зарегистрированном хранилище.

Перейдите к следующей части статьи с инструкциями по повторному [обучению моделей с помощью машинное обучение Azure Designer](how-to-retrain-designer.md) , чтобы использовать преобразованные наборы данных и параметры конвейера для обучения моделей машинного обучения.
