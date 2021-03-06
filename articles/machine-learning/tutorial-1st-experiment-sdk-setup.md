---
title: Руководство по Создание первого эксперимента ML
titleSuffix: Azure Machine Learning
description: В этом учебнике описано начало работы с пакетом SDK Машинного обучения Azure для Python, запущенным в Записных книжках Jupyter.  В части 1 вы создадите рабочую область, в которой вы будете управлять экспериментами и моделями машинного обучения.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 02/10/2020
ms.openlocfilehash: 3177de6816dd690514620098e79db844077fbaf6
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655447"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Руководство по Начало работы по созданию эксперимента Машинного обучения с помощью пакета SDK для Python
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

В этом учебнике описан пошаговый стандартный процесс начала работы с пакетом SDK Машинного обучения Azure для Python, запущенным в Записных книжках Jupyter. Этот учебник является **первой частью цикла из двух частей**. В этой части рассматривается установка и настройка среды Python, а также создание рабочей области для управления экспериментами и моделями машинного обучения. [**Вторая часть**](tutorial-1st-experiment-sdk-train.md) основана на этом учебнике. В ней показано, как обучить несколько моделей машинного обучения и управлять моделями с использованием студии машинного обучения Azure и пакета SDK.

Изучив это руководство, вы:

> [!div class="checklist"]
> * создадите [рабочую область машинного обучения Azure](concept-workspace.md) для работы со следующим руководством;
> * выполните клонирование записной книжки с учебниками в папку в рабочей области;
> * создадите облачный экземпляр для вычислений с предварительно установленным и настроенным пакетом SDK Машинного обучения Azure для Python.


Если у вас еще нет подписки Azure, создайте бесплатную учетную запись Azure, прежде чем начинать работу. Опробуйте [бесплатную или платную версию Машинного обучения Azure](https://aka.ms/AMLFree) уже сегодня.

## <a name="create-a-workspace"></a>Создание рабочей области

Рабочая область машинного обучения Azure — это основной ресурс в облаке для экспериментов, обучения и развертывания моделей машинного обучения. Она связывает подписку и группу ресурсов Azure с легко используемым объектом в службе. 

Вы создаете рабочую область с помощью портала Azure, веб-консоли для управления ресурсами Azure. 

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> Запишите **рабочую область** и **подписку**. Они понадобятся вам для того, чтобы создать эксперимент в нужном месте. 

## <a name="run-notebook-in-your-workspace"></a><a name="azure"></a>Запуск записной книжки в рабочей области

В этом учебнике в качестве предварительно настроенного интерфейса, не требующего установки, используется облачный сервер записных книжек в рабочей области. Используйте [собственную среду](how-to-configure-environment.md#local), если вы предпочитаете контролировать среду, пакеты и зависимости.

 Просмотрите следующее видео или выполните указанные ниже подробные инструкции, чтобы клонировать и запустить пример записной книжки из рабочей области. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4mTUr]

### <a name="clone-a-notebook-folder"></a>Клонирование папки записной книжки

Выполните следующие действия по настройке и выполнению эксперимента в студии машинного обучения Azure — объединенном интерфейсе, включающем в себя средства машинного обучения для выполнения сценариев обработки и анализа данных, основанных на всех уровнях навыков.

1. Войдите в [Студию машинного обучения Azure](https://ml.azure.com/).

1. Выберите свою подписку и рабочую область, которую создали.

1. Выберите **Записные книжки**слева.

1. Откройте вкладку **Примеры** вверху экрана.

1. Откройте папку **Python**.

1. Откройте папку с номером версии.  Это число представляет текущий выпуск пакета SDK для Python.

1. Выберите папку **"..."** справа от папки **Учебники**, а затем выберите **Клонировать**.

    :::image type="content" source="media/tutorial-1st-experiment-sdk-setup/clone-tutorials.png" alt-text="Клонирование папки tutorials":::

1. В списке папок отображается каждый пользователь, обращающийся к рабочей области.  Выберите свою папку для клонирования папки **Учебник**.

### <a name="open-the-cloned-notebook"></a><a name="open"></a>Открытие клонированной записной книжки

1. Откройте папку **tutorials**, которая была только что закрыта в разделе **Файлы пользователей**.

    > [!IMPORTANT]
    > Вы можете просматривать записные книжки в папке **Примеры**, но запускать их оттуда нельзя.  Для запуска записной книжки обязательно откройте клонированную версию записной книжки в разделе **Пользовательские файлы**.
    
1. Выберите файл **tutorial-1st-experiment-sdk-train.ipynb** в папке **tutorials/create-first-ml-experiment**.

    :::image type="content" source="media/tutorial-1st-experiment-sdk-setup/expand-user-folder.png" alt-text="Открытие папки tutorials":::


1. На верхней панели выберите экземпляр для вычислений для запуска записной книжки. Эти виртуальные машины предварительно настроены со [всем необходимым для запуска Машинного обучения Azure](concept-compute-instance.md#contents). 

1. Если виртуальные машины не найдены, выберите **+ Добавить**, чтобы создать виртуальную машину для вычислительных операций. 

    1. При создании виртуальной машины следуйте правилам:  
        + поле имени является обязательным и не может быть пустым;
        + имя должно быть уникальным (без учета регистра) во всех существующих вычислительных экземплярах в регионе Azure экземпляра рабочей области или вычислительной среды; если выбранное имя не является уникальным, вы получите оповещение;
        + допускаются прописные и строчные буквы, цифры (от 0 до 9) и символ "-";
        + длина имени должна составлять от 3 до 24 символов;
        + имя должно начинаться с буквы (не цифры или тире);
        + если в имени используется символ "-", за ним должна следовать хотя бы одна буква. Пример Test-, test-0, test-01 являются недопустимыми, а test-a0, test-0a — допустимыми экземплярами.

    1.  Из доступных вариантов выберите размер виртуальной машины.

    1. Щелкните **Создать**. Настройка виртуальной машины может занять около 5 минут.

1. После того как виртуальная машина будет доступна, она отобразится на верхней панели инструментов.  Теперь записную книжку можно запустить с помощью значка **Run all** (Запустить все) на панели инструментов или с помощью **Shift+Enter** в ячейках кода записной книжки.

Если у вас есть пользовательские мини-приложения или вы предпочитаете использовать Jupyter и JupyterLab, в раскрывающемся списке справа выберите **Jupyter**, а затем — **Jupyter** или **JupyterLab**. Откроется новое окно браузера.

## <a name="next-steps"></a>Дальнейшие действия

В рамках этого учебника вы выполнили такие задачи:

* создали рабочую область Машинного обучения Azure;
* создали и настроили облачный сервер записных книжек в рабочей области.

Во **второй части** руководства вы выполните код в `tutorial-1st-experiment-sdk-train.ipynb` для обучения модели машинного обучения. 

> [!div class="nextstepaction"]
> [Руководство. Обучение первой модели](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Если вы не планируете выполнять инструкции из второй части этого руководства или из других руководств, [остановите работу неиспользуемой виртуальной машины сервера облачной записной книжки](tutorial-1st-experiment-sdk-train.md#clean-up-resources), чтобы снизить затраты.


