---
title: Развертывание моделей в рабочей среде — командный процесс обработки и анализа данных
description: Способы развертывания моделей в рабочей среде для их использовании при принятии бизнес-решений.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: e3e2ef3340ca836f56176c21cf3d221f0bf172b9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76722243"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>Развертывание моделей в рабочей среде должно играть активную роль в принятии бизнес-решений

Развертывание в рабочей среде позволяет использовать модель как важный элемент в работе организации. Прогнозы, полученные от развернутой модели можно использовать для принятия бизнес-решений.

## <a name="production-platforms"></a>Рабочие платформы

Поместить модели в рабочую среду можно с использованием разных подходов и платформ. Вот некоторые из них:

- [Где развертываются модели с Машинное обучение Azure](../how-to-deploy-and-where.md)
- [Развертывание модели на сервере SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Перед развертыванием необходимо убедиться, что показатели задержки модели достаточно малы для использования в рабочей среде.
>

>[!NOTE]
>Разработка с помощью Студии машинного обучения Azure описывается в разделе [Развертывание веб-службы машинного обучения Azure](../studio/deploy-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>Тестирование А/Б

При работе с несколькими моделями в рабочей среде для сравнения производительности модели можно использовать [тестирование A/B](https://en.wikipedia.org/wiki/A/B_testing) . 
 
## <a name="next-steps"></a>Дальнейшие шаги

Также доступны пошаговые руководства, которые демонстрируют все этапы процесса для **конкретных сценариев**. Эти этапы с иллюстрациями и краткими описаниями перечислены в статье [Пошаговые руководства по процессу обработки и анализа данных группы](walkthroughs.md). В них показано, как объединить облачные и локальные средства и службы в единый рабочий процесс или конвейер, чтобы создать интеллектуальное приложение. 
