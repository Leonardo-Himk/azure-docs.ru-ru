---
title: Виртуальные машины Azure серии Lsv2
description: Спецификации виртуальных машин серии Lsv2.
services: virtual-machines
author: sasha-melamed
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: bdb9e346b8deea71ef2af9f9f271ffa446be624e
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594344"
---
# <a name="lsv2-series"></a>Серия Lsv2

Серия Lsv2 включает напрямую сопоставляемое локальное хранилище NVMe с высокой пропускной способностью и низкой задержкой на базе процессора [AMD EPYC<sup>TM</sup> 7551](https://www.amd.com/en/products/epyc-7000-series) с ускоренной частотой всех ядер 2,55 ГГц и максимальным ускорением в 3,0 ГГц. Виртуальные машины серии Lsv2 могут иметь размеры от 8 до 80 виртуальных ЦП в одновременных многопоточных конфигурациях.  Отводится 8 ГиБ памяти на виртуальный ЦП, а также одно устройство NVMe SSD M.2 объемом 1,92 ТБ на 8 виртуальных ЦП, до 19,2 ТБ (10 x 1,92 ТБ) на L80s v2.

> [!NOTE]
> Виртуальные машины серии Lsv2 оптимизированы для использования локального диска на узле, подключенном непосредственно к виртуальной машине, вместо использования устойчивых дисков данных. Это позволяет повысить число операций ввода-вывода в секунду и пропускную способность для рабочих нагрузок. Серии Lsv2 и ls не поддерживают создание локального кэша для увеличения числа операций ввода-вывода в секунду, предоставляемых устойчивыми дисками данных.
>
> Высокая пропускная способность и операции ввода-вывода на локальном диске делают виртуальные машины серии Lsv2 идеальными для хранилищ NoSQL, таких как Apache Cassandra и MongoDB, которые реплицируют данные между несколькими виртуальными машинами для обеспечения сохранности в случае сбоя одной виртуальной машины.
>
> Дополнительные сведения см. в статье Оптимизация производительности на виртуальных машинах серии Lsv2 для [Windows](../virtual-machines/windows/storage-performance.md) или [Linux](../virtual-machines/linux/storage-performance.md).  

ACU: 150-175

Разбивка на пакеты: поддерживается

Хранилище класса Premium: поддерживается

Кэширование хранилища класса Premium: не поддерживается

Динамическая миграция: не поддерживается

Обновления с сохранением памяти: не поддерживается

| Размер | vCPU | Память, ГиБ | Временный диск <sup>1</sup> (ГиБ) | Диски NVMe <sup>2</sup> | Пропускная способность диска NVMe<sup>3</sup> (операций чтения в секунду/Мбит/с) | Пропускная способность диска с некэшированными данными (операций ввода-вывода в секунду)<sup>4</sup> | Максимальная пропускная способность некэшированного диска данных (операций ввода-вывода в секунду/Мбит/с)<sup>5</sup>| Максимальное число дисков данных | Максимальное количество сетевых адаптеров / ожидаемая пропускная способность сети (Мбит/с) |
|---|---|---|---|---|---|---|---|---|---|
| Standard_L8s_v2   |  8 |  64 |  80 |  1 x 1,92 ТБ  | 400000/2000  | 8000/160   | 8000/1280 | 16 | 2 / 3200   |
| Standard_L16s_v2  | 16 | 128 | 160 |  2 x 1,92 ТБ  | 800000/4000  | 16000/320  | 16000/1280 | 32 | 4 / 6400   |
| Standard_L32s_v2  | 32 | 256 | 320 |  4 x 1,92 ТБ  | 1,5 м/8000    | 32000/640  | 32000/1280 | 32 | 8 / 12800  |
| Standard_L48s_v2  | 48 | 384 | 480 |  6X 1.92 ТБ  | 2.2 м/14000   | 48000/960  | 48000/2000 | 32 | 8 и 16000 + |
| Standard_L64s_v2  | 64 | 512 | 640 |  8 x 1,92 ТБ  | 2.9 m/16000   | 64000/1280 | 64000/2000 | 32 | 8 и 16000 + |
| Standard_L80s_v2<sup>6</sup> | 80 | 640 | 800 | 10 x 1,92 ТБ | 3.8 м/20000 | 80000/1400 | 80000/2000 | 32 | 8 и 16000 + |

<sup>1</sup> Виртуальные машины серии Lsv2 используют стандартный временный диск на основе SCSI для работы с файлом подкачки ОС (D: на Windows, /dev/sdb в Linux). Этот диск предоставляет 80 ГБ хранилища, 4000 операций ввода-вывода и скорость передачи в 80 Мбит/с для каждых 8 виртуальных ЦП (например, Standard_L80s_v2 обеспечивает 800 ГиБ на 40 000 операций ввода-вывода и 800 Мбит/с). Это гарантирует, что диски NVMe могут быть полностью выделены для использования в приложениях. Данный диск является временным, все данные, размещенные на нем, будут потеряны во время остановки или освобождения.

<sup>2</sup> Локальные Диски NVMe являются временными, и если виртуальная машина будет остановлена или отменена, все данные, которые на них находятся, будут потеряны.

<sup>3</sup> Технология Hyper-V NVMe Direct предоставляет неограниченный доступ к локальным Дискам NVMe, безопасно подключенным в пространстве гостевой виртуальной машины.  Достижение максимальной производительности требует использования последней сборки WS2019 или Ubuntu 18.04 или 16.04 из Azure Marketplace.  Производительность записи меняется в зависимости от объема ввода-вывода, загрузки диска и использования емкости.

<sup>4</sup> Виртуальные машины серии Lsv2 не предоставляют кэш узла для диска данных, так как он не повышает производительность рабочих нагрузок Lsv2.  Тем не менее виртуальные машины Lsv2 позволяют использовать временный диск ОС виртуальной машины Azure (до 30 ГиБ).

<sup>пять</sup> виртуальных машин серии [Lsv2 могут](linux/disk-bursting.md) занимать свою производительность диска в течение 30 минут за один раз. 

<sup>6</sup> виртуальных машин с более чем 64 виртуальных ЦП требуется одна из поддерживаемых гостевых операционных систем:

- Windows Server 2016 или более поздней версии
- Ubuntu 16,04 LTS или более поздней версии с настроенным ядром Azure (ядро 4,15 или более поздней версии)
- SLES 12 SP2 или более поздней версии
- RHEL или CentOS версии 6,7 – 6,10 с установленным корпорацией Майкрософт пакетом LIS 4.3.1 (или более поздней версии)
- RHEL или CentOS версии 7,3 с установленным корпорацией Майкрософт пакетом LIS 4.2.1 (или более поздней версии)
- RHEL или CentOS версии 7,6 или более поздней
- Oracle Linux с UEK4 или более поздней версии
- Debian 9 с однопортным ядром, Debian 10 или более поздней версии.
- CoreOS с ядром 4,14 или более поздней версии

## <a name="size-table-definitions"></a>Определение размера

- Емкость хранилища отображается в единицах ГиБ (1 ГиБ = 1024^3 байтов). При сравнении емкости дисков в ГБ (1000^3 байтов) с емкостью дисков в ГиБ (1024^3 байтов) помните, что значения емкости в ГиБ могут казаться меньше, чем в ГБ. Например, 1023 ГиБ = 1098,4 ГБ.
- Пропускная способность дисков измеряется в операциях ввода-вывода в секунду (IOPS) и МБит/с, где 1 МБит/с = 10^6 байтов в секунду.
- Чтобы обеспечить наилучшую производительность для виртуальных машин, следует ограничить число дисков данных до двух на каждый виртуальный процессор.
- **Ожидаемая пропускная способность сети** — это максимальная совокупная [пропускная способность, выделенная на каждый тип виртуальной машины](../virtual-network/virtual-machine-network-throughput.md) по всем сетевым адаптерам для всех назначений. Верхние значения ограничений не всегда достигаются, но они позволяют определить и выбрать правильный тип виртуальной машины. Фактическая производительность сети зависит от многих факторов, в том числе загрузки сети и приложения, а также параметров сети. Сведения об оптимизации пропускной способности см. в [этой статье](../virtual-network/virtual-network-optimize-network-bandwidth.md). Чтобы обеспечить ожидаемую производительность сети на виртуальных машинах Linux или Windows, возможно, потребуется выбрать определенную версию виртуальной машины или оптимизировать ее. Дополнительные сведения см. в статье [Проверка пропускной способности (NTTTCP)](../virtual-network/virtual-network-bandwidth-testing.md).

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.
