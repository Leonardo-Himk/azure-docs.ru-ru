---
title: Создание диаграммы производительности с помощью службы Azure Monitor для виртуальных машин
description: "\"Производительность\" — это функция службы Azure Monitor для виртуальных машин, которая автоматически обнаруживает компоненты приложений в системах Windows и Linux и отображает связи между службами. В этой статье рассказывается о том, как использовать эту функцию в различных сценариях."
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/15/2019
ms.openlocfilehash: a50ba39777e6a9d3d609e584c0c7d872f2a65f35
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80283724"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>Создание диаграммы производительности с помощью службы Azure Monitor для виртуальных машин

Служба Azure Monitor для виртуальных машин включает в себя набор диаграмм производительности, ориентированных на несколько ключевых показателей эффективности (KPI), которые помогают определить, насколько эффективно работает виртуальная машина. На диаграммах отображается использование ресурсов за определенный период времени, что позволяет выявлять узкие места, аномалии, а также переключаться на перспективное представление каждой виртуальной машины для просмотра использования ресурсов на основе выбранной метрики. Хотя при анализе производительности следует учитывать множество элементов, Azure Monitor для виртуальных машин отслеживает ключевые показатели производительности операционной системы, связанные с работой процессора, памяти, сетевого адаптера и диска. Функция "Производительность" дополняет функцию мониторинга работоспособности и помогает выявлять проблемы, которые указывают на возможный отказ компонентов системы, поддерживает настройку и оптимизацию для повышения эффективности, а также планирование ресурсов.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Перспективное представление нескольких виртуальных машин в Azure Monitor

Начиная с Azure Monitor, функция производительности предоставляет представление всех наблюдаемых виртуальных машин, развернутых в рабочих группах в ваших подписках или в вашей среде. Чтобы получить доступ к Azure Monitor, выполните указанные ниже действия. 

1. В портал Azure выберите **мониторинг**. 
2. В разделе **решения** выберите **виртуальные машины** .
3. Выберите вкладку **Производительность**.

![Представление списка первых N диаграмм производительности в аналитике виртуальных машин](media/vminsights-performance/vminsights-performance-aggview-01.png)

На вкладке **Top N Charts** (Первые N диаграмм), если у вас несколько рабочих областей Log Analytics, выберите рабочую область, которая включена в решении, в селекторе **Рабочая область** вверху страницы. Селектор **Группа** возвращает подписки, группы ресурсов, [группы компьютеров](../platform/computer-groups.md) и масштабируемые наборы виртуальных машин с компьютеров, связанных с выбранной рабочей областью, которые вы можете использовать для дальнейшей фильтрации результатов, представленных на диаграммах этой и других страниц. Выбор применяется только к функции производительности и не применяется к работоспособности и сопоставлению.  

По умолчанию на диаграммах отображаются последние 24 часа. С помощью селектора **Временной интервал** можно запросить временной интервал длительностью до 30 дней, чтобы отобразить производительность за прошедший период.

На странице отображаются пять диаграмм использования ресурсов:

* "Загрузка ЦП (%)" — показывает первые пять компьютеров с наибольшей средней загрузкой процессоров. 
* "Доступная память" — показывает первые пять компьютеров с наименьшим средним объемом доступной памяти. 
* Logical Disk Space Used % (% используемого места на логических дисках) — показывает первые пять компьютеров с наибольшим средним процентом использования места на всех томах дисков. 
* Bytes Sent Rate (Скорость отправки байт) — показывает первые пять компьютеров с наибольшим средним количеством отправленных байт. 
* Скорость получения данных в байтах — показывает пять основных компьютеров с наибольшим средним значением полученных байтов 

Щелкнув значок закрепления в правом верхнем углу любой из пяти диаграмм, вы закрепите выбранную диаграмму до последней просмотренной панели мониторинга Azure.  На панели мониторинга можно изменить размер и расположение диаграммы. Выбор диаграммы на панели мониторинга приведет к перенаправлению Azure Monitor для виртуальных машин и загрузке правильной области и представления.  

Если щелкнуть значок, расположенный слева от значка закрепления на любой из пяти диаграмм, откроется представление с **верхним N списка** .  Здесь можно просмотреть использование ресурсов для соответствующей метрики производительности по отдельным виртуальным машинам в виде списка и увидеть, для какой виртуальной машины характерны наиболее высокие показатели.  

![Вид списка первых N для выбранной метрики производительности](media/vminsights-performance/vminsights-performance-topnlist-01.png)

Щелкнув виртуальную машину, можно развернуть область **Свойства** справа, чтобы отобразить свойства выбранного элемента, такие как сведения о системе, сообщаемые операционной системой, свойства виртуальной машины Azure и т. д. Если щелкнуть один из параметров в разделе **быстрые ссылки** , вы сможете перенаправить вас на эту функцию непосредственно из выбранной виртуальной машины.  

![Область "Свойства" виртуальной машины](./media/vminsights-performance/vminsights-properties-pane-01.png)

Переключитесь на вкладку **Aggregated Charts** (Сводные диаграммы), чтобы просмотреть метрики производительности, отфильтрованные по средним показателям или процентилям.  

![Сводное представление производительности в аналитике виртуальных машин](./media/vminsights-performance/vminsights-performance-aggview-02.png)

Отображаются следующие диаграммы использования ресурсов:

* "Загрузка ЦП (%)" — по умолчанию отображаются среднее значение и верхний 95-й процентиль. 
* Доступная память — значения по умолчанию, отображающие средний, верхний 5-й и десятый процентиль 
* Logical Disk Space Used % (% используемого места на логических дисках) — по умолчанию отображаются среднее значение и 95-й процентиль. 
* Bytes Sent Rate (Скорость отправки байт) — по умолчанию отображается среднее количество отправленных байт. 
* Bytes Receive Rate (Скорость получения байт) — по умолчанию отображается среднее количество полученных байт.

Также в диапазоне времени можно изменить детализацию диаграмм, выбрав **Ср.**, **Мин.**, **Макс.**, **50-й**, **90-й** и **95-й** в селекторе процентилей.

Чтобы просмотреть сведения об использовании ресурсов отдельными виртуальными машинами в представлении списка и узнать, какой компьютер обработает с наибольшей степенью использования, перейдите на вкладку **верхняя N-список** .  На странице « **N лучших списков** » показаны 20 лучших компьютеров, отсортированных по наиболее интенсивности, чем 95 процентиль процентиль для метрик *использования ЦП*.  Чтобы просмотреть дополнительные машины, выберите **Загрузить больше**. Отобразятся результаты для первых 500 виртуальных машин. 

>[!NOTE]
>В списке нельзя отобразить более чем 500 машин одновременно.  
>

![Пример страницы Top N List (Список первых N)](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Чтобы отфильтровать результаты по конкретной виртуальной машине в списке, введите ее имя компьютера в текстовое поле **Поиск по имени**.  

Если требуется просмотреть сведения об использовании по другой метрике производительности, в раскрывающемся списке **Метрика** выберите **Доступная память**, **Logical Disk Space Used %** (% используемого места на логических дисках), **Network Received Byte/s** (Получено байт по сети/с) или **Network Sent Byte/s** (Отправлено байт по сети/с), и список обновится, отображая показатели использования по соответствующей метрике.  

Если выбрать виртуальную машину из списка, откроется панель **Свойства** в правой части страницы, где можно выбрать **Сведения о производительности**.  Откроется страница **сведений о виртуальной машине**, ограниченных соответствующей виртуальной машиной, подобно доступу к функции "Производительность" аналитики виртуальных машин непосредственно с виртуальной машины Azure.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Просмотр производительности непосредственно с виртуальной машины Azure

Чтобы получить доступ непосредственно из виртуальной машины, выполните следующие действия.

1. На портале Azure выберите **Виртуальные машины**. 
2. В списке выберите виртуальную машину и в разделе **мониторинг** выберите **аналитика**.  
3. Выберите вкладку **Производительность**. 

Эта страница содержит не только диаграммы использования производительности, но и таблицу с отображением каждого обнаруженного логического диска, его емкости, использования и общего среднего значения по каждому показателю.  

Отображаются следующие диаграммы использования ресурсов:

* "Загрузка ЦП (%)" — по умолчанию отображаются среднее значение и верхний 95-й процентиль. 
* Доступная память — значения по умолчанию, отображающие средний, верхний 5-й и десятый процентиль 
* Logical Disk Space Used % (% используемого места на логических дисках) — по умолчанию отображаются среднее значение и 95-й процентиль. 
* Logical Disk IOPS (Операций ввода-вывода в секунду для логических дисков) — по умолчанию отображаются среднее значение и 95-й процентиль.
* Logical Disk MB/s (Мбит/с для логических дисков) — по умолчанию отображаются среднее значение и 95-й процентиль.
* Max Logical Disk Used % (Макс. % использования логических дисков) — по умолчанию отображаются среднее значение и 95-й процентиль.
* Bytes Sent Rate (Скорость отправки байт) — по умолчанию отображается среднее количество отправленных байт. 
* Bytes Receive Rate (Скорость получения байт) — по умолчанию отображается среднее количество полученных байт.

Щелкнув значок закрепления в правом верхнем углу любой из диаграмм, вы закрепляете выбранную диаграмму на последней просмотренной панели мониторинга Azure. На панели мониторинга можно изменить размер и расположение диаграммы. Выбор диаграммы на панели мониторинга перенаправляет вас на Azure Monitor для виртуальных машин и загружает представление сведений о производительности для виртуальной машины.  

![Представление производительности непосредственно с виртуальной машины в аналитике виртуальных машин](./media/vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="view-performance-directly-from-an-azure-virtual-machine-scale-set"></a>Просмотр производительности непосредственно из масштабируемого набора виртуальных машин Azure

Чтобы получить доступ непосредственно из масштабируемого набора виртуальных машин Azure, выполните следующие действия.

1. В портал Azure выберите **масштабируемые наборы виртуальных машин**.
2. В списке выберите виртуальную машину и в разделе **мониторинг** выберите **аналитика** , чтобы просмотреть вкладку **производительность** .

На этой странице загружается представление производительности Azure Monitor в области, выделенной для выбранного масштабируемого набора. Это позволяет видеть N верхних экземпляров в масштабируемом наборе по набору отслеживаемых метрик, просматривать статистическую производительность в масштабируемом наборе и просматривать тренды для выбранных метрик по отдельным экземплярам из масштабируемого набора. Выбор экземпляра из представления списка позволяет загрузить его карту или перейти в подробное представление производительности для этого экземпляра.

Щелкнув значок закрепления в правом верхнем углу любой из диаграмм, вы закрепляете выбранную диаграмму на последней просмотренной панели мониторинга Azure. На панели мониторинга можно изменить размер и расположение диаграммы. Выбор диаграммы на панели мониторинга перенаправляет вас на Azure Monitor для виртуальных машин и загружает представление сведений о производительности для виртуальной машины.  

![Производительность VM Insights непосредственно из представления масштабируемого набора виртуальных машин](./media/vminsights-performance/vminsights-performance-directvmss-01.png)

>[!NOTE]
>Вы также можете получить доступ к подробному представлению производительности для конкретного экземпляра из представления экземпляров для масштабируемого набора. Перейдите к **экземплярам** в разделе **Параметры** и выберите **аналитика**.



## <a name="next-steps"></a>Дальнейшие шаги

- Узнайте, как использовать [книги](vminsights-workbooks.md) , входящие в состав Azure Monitor для виртуальных машин, для дальнейшего анализа производительности и метрик сети.  

- Дополнительные сведения об обнаруженных зависимостях приложений см. в статье [просмотр Azure Monitor для виртуальных машин Map](vminsights-maps.md).
