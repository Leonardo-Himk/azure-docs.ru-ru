---
title: Что такое целевые показатели вычислений
titleSuffix: Azure Machine Learning
description: Определите, где вы хотите обучить или развернуть модель с помощью Машинное обучение Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 03/30/2020
ms.openlocfilehash: ed65d69c18f2dbcd53324fe3cc18af8c51c546b2
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780119"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>Что такое целевые показатели вычислений в Машинное обучение Azure? 

**Целевой объект вычислений** — это назначенный ресурс или среда вычислений, где выполняется сценарий обучения или размещается развертывание службы. Это может быть локальный компьютер или облачный ресурс вычислений. С помощью целевых объектов вычислений можно легко изменить среду вычислений, не изменяя код.  

В типичном цикле разработки модели вы можете:
1. Начните с разработки и экспериментирования с небольшим объемом данных. На этом этапе мы рекомендуем использовать локальную среду (локальный компьютер или облачную виртуальную машину) в качестве целевого объекта вычислений. 
2. Увеличьте масштаб до больших данных или выполните распределенное обучение с помощью одной из этих [целей для обучения](#train).  
3. Когда модель будет готова, разверните ее в среде веб-размещения или на устройстве IoT, используя один из этих [целевых объектов для вычислений развертывания](#deploy).

Ресурсы вычислений, используемые для целевых целей вычислений, присоединяются к [рабочей области](concept-workspace.md). Пользователи рабочей области совместно используют ресурсы для вычислений, отличных от локального компьютера.

## <a name="training-compute-targets"></a><a name="train"></a>Цели обучающих вычислений

Машинное обучение Azure поддерживает различные ресурсы вычислений.  Можно также присоединить собственные ресурсы вычислений, хотя поддержка различных сценариев может быть различной.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

Дополнительные сведения о [настройке и использовании целевого объекта вычислений для обучения модели](how-to-set-up-training-targets.md).

## <a name="deployment-targets"></a><a name="deploy"></a>Цели развертывания

Для размещения развертывания модели можно использовать следующие ресурсы вычислений.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

Узнайте [, где и как развертывать модель в целевом объекте вычислений](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Машинное обучение Azure вычислений (управляемый)

Управляемый ресурс вычислений создается и управляется с помощью Машинное обучение Azure. Это вычисление оптимизировано для рабочих нагрузок машинного обучения. Машинное обучение Azureными кластерами вычислений и [экземплярами вычислений](concept-compute-instance.md) являются только управляемые вычислений. Дополнительные управляемые ресурсы вычислений могут быть добавлены в будущем.

Вы можете создать Машинное обучение Azureные экземпляры (Предварительная версия) или COMPUTE Clusters из:
* Студия машинного обучения Azure.
* Портал Azure
* Классы [компутеинстанце](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py) и [амлкомпуте](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute(class)?view=azure-ml-py) пакета SDK для Python
* [Пакет SDK для R](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets)
* Шаблон Resource Manager

Вы также можете создавать кластеры вычислений с помощью [расширения машинного обучения для Azure CLI](tutorial-train-deploy-model-cli.md#create-the-compute-target-for-training).

При создании эти ресурсы вычислений автоматически являются частью рабочей области, в отличие от других типов целевых объектов вычислений.

### <a name="compute-clusters"></a>Вычислительные кластеры

Вы можете использовать Машинное обучение Azureные кластерные ресурсы для обучения и пакетной обработки результатов (Предварительная версия).  С этим ресурсом вычислений у вас есть:

* Кластер с одним или несколькими узлами
* Автомасштабирование при каждой отправке выполнения 
* Автоматическое управление кластерами и планирование заданий 
* поддерживает ресурсы ЦП и GPU;

### <a name="supported-vm-series-and-sizes"></a>Поддерживаемые ряды и размеры виртуальных машин

При выборе размера узла для управляемого ресурса вычислений в Машинное обучение Azure вы можете выбрать один из доступных размеров виртуальной машины в Azure. Azure предлагает ряд размеров для Linux и Windows для различных рабочих нагрузок. Дополнительные сведения о различных [типах и размерах виртуальных машин](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)см. здесь.

При выборе размера виртуальной машины существует несколько исключений и ограничений.
* Некоторые серии виртуальных машин не поддерживаются в Машинное обучение Azure.
* Некоторые серии виртуальных машин ограничены. Чтобы использовать ограниченные ряды, обратитесь в службу поддержки и запросите увеличение квоты для ряда. Сведения о связи со службой поддержки см. в статье [варианты поддержки Azure](https://azure.microsoft.com/support/options/) .

Дополнительные сведения о поддерживаемых рядах и ограничениях см. в следующей таблице. 

| **Поддерживаемые серии виртуальных машин**  | **Ограничения** |
|------------|------------|
| D | Отсутствуют |
| Dv2 | Отсутствуют |  
| DSv2 | Отсутствуют |  
| Серия fsv2 | Отсутствуют |  
| M | Запрос утверждения |
| NC | Отсутствуют |    
| NCsv2 | Запрос утверждения |
| NCsv3 | Запрос утверждения |  
| Структура | Запрос утверждения |
| NDv2 | Запрос утверждения |
| NV | Отсутствуют |
| NVv3 | Запрос утверждения | 


Хотя Машинное обучение Azure поддерживает эти серии виртуальных машин, они могут быть недоступны во всех регионах Azure. Проверить наличие серии виртуальных машин можно здесь: [продукты, доступные по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

## <a name="unmanaged-compute"></a>Неуправляемое вычисление

Неуправляемый целевой объект вычислений *не* управляется машинное обучение Azure. Вы создаете этот тип целевого объекта вычислений вне Машинное обучение Azure, а затем подключаете его к рабочей области. Неуправляемые ресурсы вычислений могут потребовать дополнительных действий для поддержки или повышения производительности рабочих нагрузок машинного обучения.

## <a name="next-steps"></a>Дальнейшие действия

Вы узнаете, как выполнять следующие задачи:
* [Настройка целевого объекта вычислений для обучения модели](how-to-set-up-training-targets.md)
* [Развертывание модели в целевом объекте вычислений](how-to-deploy-and-where.md)
