---
title: Решение VMware для Azure по Клаудсимпле — частные облака
description: Сведения о частных облаках и концепциях Клаудсимпле.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4fb930603455ed1a5df5d357fcab669f41a0c28c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77024960"
---
# <a name="cloudsimple-private-cloud-overview"></a>Общие сведения о частном облаке Клаудсимпле

Клаудсимпле преобразует и расширяет рабочие нагрузки VMware в общедоступные облака за считаные минуты. С помощью службы Клаудсимпле можно развертывать VMware изначально в инфраструктуре исходного состояния системы Azure. Развертывание находится в расположениях Azure и полностью интегрируется с остальными частями облака Azure.

Решение Клаудсимпле обеспечивает полную оперативную непрерывность работы VMware. Это решение предоставляет преимущества общедоступного облака:

* Эластичность
* Инновации
* Эффективность

С помощью Клаудсимпле можно воспользоваться моделью потребления в облаке, которая снижает общую стоимость владения. Он также предлагает оптимизацию по требованию, оплату по мере роста и оптимизировать ресурсы.

Клаудсимпле полностью совместима с:

* Существующие средства
* Навыки
* Процессы

Эта совместимость позволяет вашим командам управлять рабочими нагрузками в облаке Azure, не нарушая политики следующих типов:

* Сеть
* Безопасность  
* Защита данных  
* Аудит

Клаудсимпле управляет инфраструктурой и всеми необходимыми службами сети и управления. Служба Клаудсимпле позволяет команде сосредоточиться на следующих элементах:

* Ценность для бизнеса
* Подготовка приложений
* Непрерывность бизнес-процессов
* Поддержка
* Принудительное применение политики

## <a name="private-cloud-environment-overview"></a>Общие сведения о среде частного облака

Частное облако — это изолированный стек VMware, который поддерживает:

* Узлы ESXi
* vCenter
* vSAN
* нскс

Управление частными облаками осуществляется через портал Клаудсимпле. Они имеют собственный сервер vCenter в собственном домене управления.

Стек работает в:

* Специальные узлы
* Изолированные узлы оборудования без операционной системы

Пользователи используют стек с помощью собственных средств VMware, в том числе:

* vCenter
* НСКС Manager

Вы можете развернуть выделенные узлы в расположениях Azure. Затем вы можете управлять ими с помощью Azure и Клаудсимпле. Частное облако состоит из одного или нескольких кластеров vSphere, а каждый кластер содержит от 3 до 16 узлов.

Вы можете создать частное облако, используя приобретенные узлы с оплатой по мере использования или зарезервированные выделенные узлы.

Вы можете подключить частное облако к локальной среде и сети Azure, используя следующие подключения:

* Безопасность
* Частная VPN
* Azure ExpressRoute

Среда частного облака предназначена для устранения одиночных точек отказа:

* Кластеры ESXi настроены с высоким уровнем доступности vSphere и имеют по крайней мере один запасной узел для обеспечения устойчивости.
* vSAN предоставляет избыточное основное хранилище. для обеспечения защиты от одного сбоя vSan требуется по крайней мере три узла. Вы можете настроить vSAN, чтобы обеспечить более высокую устойчивость для больших кластеров.
* Вы можете настроить виртуальные машины vCenter, PSC и НСКС Manager с политикой хранения RAID-10, чтобы защититься от сбоев хранилища. vSphere HA защищает от сбоев узла и сети.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Сценарии развертывания частного облака

Ниже приведены некоторые примеры вариантов использования для развертывания частного облака.

### <a name="data-center-retirement-or-migration"></a>Прекращение использования или перенос центра обработки данных

* Получите дополнительную емкость при достижении ограничений существующего центра обработки данных или обновления оборудования.
* Добавьте необходимую емкость в облако и устраните проблемы управления обновлениями оборудования.
* Сократите риск и стоимость миграции в облако по сравнению с длительным преобразованием или реархитектурой.
* Используйте привычные средства и навыки VMware для ускорения миграции в облако. В облаке используйте службы Azure, чтобы модернизировать приложения в своем темпе.

### <a name="expand-on-demand"></a>Развернуть по запросу

* Расширение в облако для удовлетворения непредвиденных потребностей, таких как новые среды разработки или сезонные колебания емкости.
* Создайте новую емкость по запросу и не задерживайте ее, пока она нужна.
* Сократите свои инвестиции, повышайте скорость подготовки и упростите ту же архитектуру и политики в локальных и облачных средах.

### <a name="disaster-recovery-and-virtual-desktops-in-the-azure-cloud"></a>Аварийное восстановление и виртуальные рабочие столы в облаке Azure

* Установите удаленный доступ к данным, приложениям и рабочим столам в облаке Azure. При использовании подключений с высокой пропускной способностью вы отправляете или скачиваете данные быстро для восстановления после инцидентов. Сети с низкой задержкой обеспечивают быстрое время отклика, которое пользователи могут ждать от классического приложения.

* Репликация всех политик и сетевых подключений в облаке с помощью портала Клаудсимпле и знакомых средств VMware. Репликация сокращает усилия и риск создания и управления реализациями аварийного восстановления и VDI.

### <a name="high-performance-applications-and-databases"></a>Высокопроизводительные приложения и базы данных

* Запускайте самые ресурсоемкие рабочие нагрузки с помощью архитектуры гиперконвергентном, предоставляемой Клаудсимпле.
* Запустите Oracle, Microsoft SQL Server, промежуточные системы и высокопроизводительные базы данных без SQL.
* Оцените облако в качестве собственного центра обработки данных с высокоскоростными сетевыми подключениями 25 Гбит/с. Высокоскоростные подключения позволяют запускать гибридные приложения, охватывающие локальную среду, VMware в Azure и частные рабочие нагрузки Azure, не нарушая производительность.

### <a name="true-hybrid"></a>Истинное гибридное

* Объедините DevOps между VMware и службами Azure.
* Оптимизируйте администрирование VMware для служб и решений Azure, которые могут применяться ко всем рабочим нагрузкам.
* Доступ к общедоступным облачным службам без необходимости расширения центра обработки данных или перепроектирования приложений.
* Централизация удостоверений, политик контроля доступа, ведения журналов и мониторинга для приложений VMware в Azure.

## <a name="limits"></a>Ограничения

В следующей таблице перечислены ограничения узлов для ресурсов частного облака.

| Ресурс | Ограничение |
|----------|-------|
| Минимальное число узлов для создания частного облака | 3 |
| Максимальное число узлов в кластере в частном облаке | 16 |
| Максимальное число узлов в частном облаке | 64 |
| Минимальное число узлов в новом кластере | 3 |

## <a name="next-steps"></a>Дальнейшие шаги

* Узнайте, как [создать частное облако](create-private-cloud.md)
* Узнайте, как [настроить среду частного облака](quickstart-create-private-cloud.md)
