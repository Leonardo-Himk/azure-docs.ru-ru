---
title: Размеры виртуальных машин Azure — GPU | Документация Майкрософт
description: Список различных размеров GPU, доступных для виртуальных машин в Azure. Сведения о количестве виртуальных ЦП, дисков данных и сетевых адаптеров, а также о пропускной способности хранилища и сети для размеров виртуальных машин этой серии.
services: virtual-machines
documentationcenter: ''
author: vikancha
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 5d36ba05d2138a06ebb2ef4e49aadb6032b62b92
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82627047"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Размеры виртуальных машин, оптимизированных для GPU

Размер ВИРТУАЛЬНЫХ машин, оптимизированных для GPU, — это специализированные виртуальные машины, доступные с помощью одного, нескольких и дробных GPU. Эти размеры предназначены для рабочих нагрузок с большим объемом вычислений, графической обработки и визуализаций. В этой статье содержатся сведения о количестве и типе GPU, виртуальных ЦП, дисков данных и сетевых адаптеров. Кроме того, для каждого размера виртуальной машины этой группы учитывается пропускная способность хранилища и сети.

- Размеры серий [NC](nc-series.md), [NCv2](ncv2-series.md)и [NCv3](ncv3-series.md) оптимизированы для ресурсоемких приложений и алгоритмов с большим объемом вычислительных ресурсов и сети. Примерами могут быть CUDA и OpenCL приложения и моделирование, AI и глубокое обучение. Серия NCv3 предназначена для высокопроизводительных вычислительных рабочих нагрузок на базе графического процессора NVIDIA Tesla V100. В серии NC используется процессор Intel Xeon 3-2690 v3 с частотой до ГГц v3 (Haswell), а виртуальные машины серии NCv2 и NCv3 используют процессор Intel Xeon-2690 V4 (Broadwell).

- Размеры серий [ND](nd-series.md)и [NDv2](ndv2-series.md) ориентированы на сценарии обучения и вывода для глубокого обучения. Они используют процессор NVIDIA Tesla P40 GPU и Intel Xeon 2690 V4 (Broadwell). В серии NDv2 используется процессор Intel Xeon Platinum 8168 (Skylake).

- Размеры серий [NV](nv-series.md) и [NVv3](nvv3-series.md) оптимизированы и предназначены для удаленной визуализации, потоковой передачи, игр, кодирования и сценариев VDI с использованием таких платформ, как OpenGL и DirectX. Эти виртуальные машины работают на базе графического процессора NVIDIA Tesla M60.

- [Серия NVv4](nvv4-series.md) Размеры виртуальных машин оптимизированы и предназначены для VDI и удаленной визуализации. При использовании секционированных GPU NVv4 предлагает правильный размер для рабочих нагрузок, для которых требуются небольшие ресурсы GPU. Эти виртуальные машины поддерживаются графическим процессором AMD Radeon порывом MI25. В настоящее время виртуальные машины NVv4 поддерживают только гостевую операционную систему Windows.

## <a name="supported-operating-systems-and-drivers"></a>Поддерживаемые операционные системы и драйверы

Чтобы воспользоваться преимуществами возможностей GPU виртуальных машин Azure серии N, необходимо установить драйверы NVIDIA или AMD GPU.

- Для виртуальных машин, которые поддерживаются графическими процессорами NVIDIA, [расширение драйвера GPU NVIDIA](/azure/virtual-machines/extensions/hpccompute-gpu-windows) устанавливает соответствующие драйверы NVIDIA CUDA или Grid. Для установки расширения и управления им можно использовать портал Azure или такие инструменты, как Azure PowerShell и шаблоны Azure Resource Manager. Сведения о поддерживаемых операционных системах и этапах развертывания см. в [документации по расширению драйвера GPU NVIDIA](/azure/virtual-machines/extensions/hpccompute-gpu-windows). Общие сведения о расширениях виртуальных машин см. в статье [Расширения и компоненты виртуальных машин Azure](/azure/virtual-machines/extensions/overview).

   Кроме того, драйверы NVIDIA GPU можно установить вручную. См. статью [Установка драйверов NVIDIA GPU на виртуальных машинах серии n под управлением Windows](/azure/virtual-machines/windows/n-series-driver-setup) или [Установка драйверов NVIDIA GPU на виртуальных машинах серии n под управлением Linux](/azure/virtual-machines/linux/n-series-driver-setup) для поддерживаемых операционных систем, драйверов, установки и шагов проверки.

- Для виртуальных машин, поддерживающих процессоры AMD GPU, см. статью [Установка драйверов GPU AMD на виртуальных машинах серии N под управлением Windows](/azure/virtual-machines/windows/n-series-amd-driver-setup) для поддерживаемых операционных систем, драйверов, установок и шагов проверки.

## <a name="deployment-considerations"></a>Рекомендации по развертыванию

- Сведения о доступности виртуальных машин серии N по регионам см. на [этой странице](https://azure.microsoft.com/regions/services/).

- Виртуальные машины серии N поддерживают только модель развертывания с помощью Resource Manager.

- Виртуальные машины серии N отличаются типом хранилища Azure, которое поддерживается для их дисков. Виртуальные машины серий NC и NV поддерживают только диски на основе хранилища дисков уровня "Стандартный" (HDD). Виртуальные машины серий NCv2, NCv3, ND, NDv2 и NVv2 поддерживают только диски на основе хранилища дисков ценовой категории "Премиум" (SSD).

- Если вы хотите развернуть большое количество виртуальных машин серии N, мы рекомендуем подписку с оплатой по мере использования или другие варианты покупки. Если вы используете [бесплатную учетную запись Azure](https://azure.microsoft.com/free/), вам доступно ограниченное количество вычислительных ядер Azure.

- Возможно, вам потребуется увеличить квоту на использование ядер (для каждого региона) в подписке Azure и отдельную квоту для ядер серии NC, NCv2, NCv3, ND, NDv2, NV или NVv2. Чтобы запросить увеличение квоты, отправьте [запрос в службу поддержки клиентов](../azure-portal/supportability/how-to-create-azure-support-request.md) бесплатно. Ограничения по умолчанию отличаются в зависимости от категории подписки.

## <a name="other-sizes"></a>Остальные размеры

- [Универсальные](sizes-general.md)
- [Оптимизированные для вычислений](sizes-compute.md)
- [Для высокопроизводительных вычислений](sizes-hpc.md)
- [Оптимизированные для памяти](sizes-memory.md)
- [Оптимизированные для хранилища](sizes-storage.md)
- [Предыдущие поколения](sizes-previous-gen.md)

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.
