---
title: Сведения о Виртуальной машине для обработки и анализа данных Azure
titleSuffix: Azure Data Science Virtual Machine
description: Обзор виртуальной машины Azure для обработки и анализа данных. Эту виртуальную машину легко создать и использовать на облачной платформе Azure с помощью предварительно установленных и настроенных инструментов и библиотек для обработки и анализа данных, а также для разработки интеллектуальных приложений.
keywords: средства анализа и обработки данных, виртуальная машина для анализа и обработки данных, средства для анализа и обработки данных, анализ и обработка данных Linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 04/02/2020
ms.openlocfilehash: 03bfee258fe96d90c32b6a305b99856a11d9a087
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754982"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>Что такое виртуальная машина для обработки и анализа данных Azure на Linux и Windows?

Виртуальная машина для обработки и анализа данных (DSVM) — это настроенный образ виртуальной машины на облачной платформе Azure, созданный специально для обработки и анализа данных. В ней предварительно установлено и настроено множество популярных средств для анализа и обработки данных, позволяющих быстро приступить к созданию интеллектуальных приложений для расширенной аналитики. 

DSVM можно использовать в следующих системах:

+ **Windows Server 2019**
+ **Ubuntu 18.04 LTS**
+ Windows Server 2016
+ Ubuntu 16.04 LTS

> [!NOTE]
> В виртуальную машину для обработки и анализа данных были добавлены все инструменты виртуальной машины для глубокого обучения. 

## <a name="why-choose-the-dsvm"></a>Почему DSVM (Виртуальная машина для обработки и анализа данных)?

Назначение Виртуальной машины для обработки и анализа данных — обеспечить для специалистов, обладающих любыми навыками и в любых отраслях, беспроблемную предварительно настроенную среду для обработки и анализа данных. Вместо развертывания сравнимой рабочей области можно подготавливать DSVM. Этот вариант позволит сэкономить дни или даже _недели_ при процессах установки, настройки и управления пакетами. Когда виртуальная машина DSVM будет выделена, можно немедленно приступать к проекту по обработке и анализу данных.

## <a name="sample-use-cases"></a>Примеры вариантов использования

Ниже показаны некоторые общие варианты использования для клиентов DSVM.

### <a name="moving-data-science-workloads-to-the-cloud"></a>Перемещение рабочих нагрузок обработки и анализа данных в облако

DSVM предоставляет базовую конфигурацию для команд обработки и анализа данных, которые хотят заменить свои локальные настольные компьютеры управляемым облачным рабочим столом, гарантируя, что все специалисты по обработке и анализу данных в группе имеют согласованную настройку для проверки экспериментов и повышения уровня совместной работы. Это также снижает расходы за счет сокращения нагрузки на системных администраторов. Это сокращение нагрузки позволяет сократить время, затрачиваемое на вычисление, установку и обслуживание программных пакетов для расширенной аналитики.

### <a name="data-science-training-and-education"></a>Обучение обработке и анализу данных

Корпоративные инструкторы и преподаватели, обучающие классы по обработке и анализу данных, обычно предоставляют образ виртуальной машины. Этот образ гарантирует, что учащиеся имеют одинаковые настройки, а примеры программ работают предсказуемо. 

DSVM создает требуемую среду с согласованными настройками, что упрощает задачи поддержки и устраняет проблемы несовместимости. Для случаев, когда необходимо часто создавать такие среды, особенно в рамках коротких учебных курсов, характерно больше преимуществ.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Эластичность по требованию для проектов большого масштаба

Интенсивная работа и соревнования по обработке и анализу данных, или моделирование и изучение данных большого масштаба обычно требуют использования мощного оборудования в течение короткого периода. DSVM помогает быстро и по требованию реплицировать среду обработки данных на развернутые серверы, что позволяет проводить эксперименты, требующие мощных вычислительных ресурсов для репликации.

### <a name="custom-compute-power-for-azure-notebooks"></a>Настройка вычислительной мощности для Записных книжек Azure

[Записные книжки Azure ](../../notebooks/azure-notebooks-overview.md)— это бесплатная размещенная служба для разработки и запуска записных книжек Jupyter, а также предоставления к ним общего доступа в облаке без их установки. Бесплатный уровень служб ограничен 4 ГБ памяти и 1 ГБ данных. 

Чтобы снять все ограничения, вы можете подключить проект "Записные книжки" к DSVM или к любой другой виртуальной машине, работающей на сервере Jupyter. Если вы вошли в "Записные книжки Azure" с помощью учетной записи, использующей Azure Active Directory (например, корпоративной учетной записи), в Записных книжках будут автоматически отображатся сведения о виртуальных машинах DSVM в любых подписках, связанных с этой учетной записью. Вы можете [подключить DSVM к Записным книжкам Azure](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier), чтобы расширить доступные вычислительные возможности.

### <a name="short-term-experimentation-and-evaluation"></a>Краткосрочные эксперименты и оценка

Вы можете использовать DSVM, чтобы оценить или изучить новые [инструменты](./tools-included.md) обработки и анализа данных, особенно путем перехода к нашим опубликованным [примерам и пошаговым руководствам](./dsvm-samples-and-walkthroughs.md).


### <a name="deep-learning-with-gpus"></a>Глубокое обучение с помощью графических процессоров

В DSVM модели обучения могут использовать алгоритмы глубокого обучения на оборудовании, основанном на графических процессорах (GPU). Используя преимущества масштабирования виртуальных машин платформы Azure, DSVM помогает использовать оборудование на основе GPU в облаке в соответствии с вашими потребностями. Вы можете переключиться на виртуальную машину на основе GPU при обучении больших моделей или выполняете высокоскоростные вычисления, не меняя диск операционной системы. Вы можете выбрать любой из SKU виртуальных машин серии N с поддержкой GPU и DSVM. Обратите внимание, что бесплатные учетные записи Azure не поддерживают номера SKU виртуальных машин с поддержкой GPU.

Выпуски DSVM для Windows содержат предварительно установленные драйверы GPU, платформы и версии GPU платформ глубокого обучения. В выпуске для Linux глубокое обучение на GPU включено во все версии DSVM. 

Вы также можете развернуть DSVM в версии для Ubuntu или Windows 2016 на виртуальной машине Azure, которая не основана на GPU. В этом случае все платформы глубокого обучения вернутся в режим ЦП.

[Дополнительные сведения о доступных платформах для глубокого обучения и искусственного интеллекта](dsvm-tools-deep-learning-frameworks.md).

<a name="included"></a>
## <a name="whats-included-on-the-dsvm"></a>Что входит в DSVM?

Полный список инструментов для DSVM на Windows и Linux [смотрите здесь](tools-included.md).

## <a name="next-steps"></a>Дальнейшие действия

См. сведения в следующих статьях:

+ Windows:
  + [Настройка Виртуальной машины для обработки и анализа данных (Windows)](provision-vm.md)
  + [Десять вещей, которые можно сделать с помощью Windows DSVM](vm-do-ten-things.md)

+ Linux:
  + [Настройка Linux DSVM (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Обработка и анализ данных в Linux DSVM](linux-dsvm-walkthrough.md)
