---
title: Размеры виртуальных машин Azure — общее назначение | Документация Майкрософт
description: Список различных размеров общего назначения, доступных для виртуальных машин в Azure. Выводит сведения о количестве виртуальных ЦП, дисков данных и сетевых карт, а также пропускную способность хранилища и пропускную способность сети для размеров в этой серии.
services: virtual-machines
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/20/2020
ms.author: mimckitt
ms.openlocfilehash: fc263eb6fbe6c6402aaf529229bb7025f070b8d9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81269675"
---
# <a name="general-purpose-virtual-machine-sizes"></a>Размеры виртуальных машин общего назначения

В размерах виртуальных машин общего назначения предоставляется сбалансированное соотношение ресурсов ЦП и памяти. Идеальное решение для тестирования и разработки, небольших и средних баз данных, а также веб-серверов с небольшим или средним объемом трафика. В этой статье содержатся сведения о предложениях для вычислений общего назначения.

- Виртуальные машины [серии Av2](av2-series.md) можно развертывать на различных типах оборудования и процессорах. Виртуальные машины серии A обладают характеристиками производительности процессора и памяти, которые лучше всего подходят для рабочих нагрузок начального уровня, например для разработки и тестирования. Вы можете регулировать размер в зависимости от оборудования, чтобы обеспечить согласованные показатели производительности процессора для выполняемого экземпляра (независимо от устройства, на котором выполняется развертывание). Чтобы определить физическое оборудование, на котором развернут определенный размер, отправьте запрос к виртуальному оборудованию из виртуальной машины. Среди вариантов использования — серверы разработки и тестирования, веб-серверы с низким уровнем трафика, базы данных малого и среднего размера, экспериментальные решения и репозитории кода.

  > [!NOTE]
  > Виртуальные машины A8 – A11 планируется выпустить в 3/2021. Дополнительные сведения см. в разделе [руководством по миграции HPC](https://azure.microsoft.com/resources/hpc-migration-guide/).

- [Серия B — это пакетная](sizes-b-series-burstable.md) Виртуальные машины идеально подходят для рабочих нагрузок, которым не требуется непрерывная полная производительность ЦП, например веб-серверов, небольших баз данных и сред разработки и тестирования. Обычно для этих рабочих нагрузок требуется накапливаемая производительность. Серия B — это возможность для пользователей приобретать за приемлемую цену виртуальные машины размера, который обеспечивает базовый уровень производительности. Это позволяет экземпляру виртуальной машины накапливать кредиты, когда такая машина использует меньше ресурсов, чем предусмотрено базовым уровнем производительности. Когда виртуальная машина накапливает кредит, она может использовать больше ресурсов, чем предоставляет базовый план: до 100 % виртуальных процессоров, если приложению требуется более высокая производительность ЦП.

- [Серии Dav4 и Dasv4](dav4-dasv4-series.md) — это новые размеры, ИСПОЛЬЗУЮЩИЕ процессор AMD 2.35 ГГц епик<sup>TM</sup> 7452 в многопотоковой конфигурации с кэшем объемом до 256 МБ, выделяемый 8 МБ для этого кэша L3, до каждых 8 ядер, увеличивающих возможности клиента для выполнения рабочих нагрузок общего назначения. Серии Dav4 и Dasv4 имеют те же конфигурации памяти и диска, что и серия D & Dsv3-Series.

- [Серия DCv2](dcv2-series.md) помогает защитить конфиденциальность и целостность данных и кода во время их обработки в общедоступном облаке. Эти компьютеры поддерживаются последним поколением процессора Intel XEON E-2288G с технологией SGX. Благодаря технологии Intel Turbo Boost эти компьютеры могут проходить до 5,0 ГГц. Экземпляры серии DCv2 позволяют клиентам создавать безопасные приложения на основе анклава для защиты своего кода и данных, когда они используются.

- [Серии Dv2 и Dsv2](dv2-dsv2-series.md) Виртуальные машины, а затем к исходным сериям D, имеют более мощный процессор и оптимальную конфигурацию ЦП в памяти, что делает их пригодными для большинства рабочих нагрузок. Серия Dv2 составляет примерно 35% быстрее, чем серия D. Серия Dv2 работает на процессорах Intel® Xeon® с тактовой частотой 2.1 ГГц (Skylake), Intel® Xeon®, 2673 v4 2,3 ГГц (Broadwell) или Intel® Xeon® 2673 v3 2,4 ГГц (Haswell) с 2,0 технологией Intel Turbo Boost. Серия Dv2 имеет такие же конфигурации памяти и диска, как и серия D.

- [Серии Dv3 и Dsv3](dv3-dsv3-series.md) Виртуальные машины работают на процессорах Intel® Xeon® 8171M 2.1 ГГц (Skylake), Intel® Xeon® 2673 2,3 ГГц (Broadwell) или Intel® Xeon® 2673 v3 2,4 ГГц (Haswell) в конфигурации с технологией Hyper-Threading, предоставляя лучшее ценное предложение для большинства рабочих нагрузок общего назначения. Объем памяти увеличен с 3,5 ГиБ до 4 ГиБ на виртуальный ЦП, а дисковые и сетевые ограничения в связи с переходом на технологию Hyper Threading подстроены под каждое ядро. Dv3-Series больше не имеет размеры виртуальных машин высокой памяти для серии D/Dv2, которые были перемещены в оптимизированные для памяти [Ev3 и серии Esv3](ev3-esv3-series.md).

Примеры использования серии D включают приложения корпоративного класса, реляционные базы данных, кэширование в памяти и аналитику.

## <a name="other-sizes"></a>Остальные размеры

- [Оптимизированные для вычислений](sizes-compute.md)
- [Оптимизированные для памяти](sizes-memory.md)
- [Оптимизированные для хранилища](sizes-storage.md)
- [Оптимизированные для GPU](sizes-gpu.md)
- [Для высокопроизводительных вычислений](sizes-hpc.md)
- [Предыдущие поколения](sizes-previous-gen.md)

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.
