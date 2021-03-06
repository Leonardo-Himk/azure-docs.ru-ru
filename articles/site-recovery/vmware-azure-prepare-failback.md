---
title: Подготовка виртуальных машин VMware для повторного включения защиты и восстановления размещения с помощью Azure Site Recovery
description: Подготовка к восстановлению после сбоя виртуальных машин VMware после отработки отказа с Azure Site Recovery
ms.topic: conceptual
ms.date: 12/24/2019
ms.openlocfilehash: 5a330f8cba31640d0116ca3d5ccab352ce5b3509
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79257190"
---
# <a name="prepare-for-reprotection-and-failback-of-vmware-vms"></a>Подготовка к повторному включению защиты и восстановлению размещения виртуальных машин VMware

После [отработки отказа](site-recovery-failover.md) локальных виртуальных машин VMware или физических серверов в Azure вы повторно включите защиту виртуальных машин Azure, созданных после отработки отказа, чтобы они реплицировались на локальный сайт. После репликации из Azure в локальную среду вы можете восстановить размещение, запустив отработку отказа из Azure в локальную среду, когда будете готовы.

Прежде чем продолжить, получите краткий обзор с этим видео о том, как восстановить диск из Azure на локальный сайт.<br /><br />
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

## <a name="reprotectionfailback-components"></a>Компоненты повторной защиты и восстановления размещения

Необходимо установить ряд компонентов и параметров, прежде чем можно будет повторно включить защиту и восстановить размещение из Azure.

**Компонент**| **Сведения**
--- | ---
**Локальный сервер конфигурации** | Локальный сервер конфигурации должен быть установлен и подключен к Azure.<br/><br/> Виртуальная машина, на которую выполняется возврат, должна существовать в базе данных сервера конфигурации. Если аварии влияет на сервер конфигурации, восстановите его с тем же IP-адресом, чтобы обеспечить работу восстановления размещения.<br/><br/>  Если IP-адреса реплицированных компьютеров были сохранены при отработке отказа, необходимо установить подключение типа "сеть — сеть" (или подключение ExpressRoute) между виртуальными машинами Azure и сетевым адаптером восстановления размещения сервера конфигурации. Для сохраняемых IP-адресов серверу конфигурации требуются два сетевых адаптера — один для подключения к исходному компьютеру, а другой — для подключения к серверу восстановления размещения Azure. Это позволяет избежать перекрытия диапазонов адресов подсети для исходных и отработки отказа виртуальных машин.
**Сервер обработки в Azure** | Чтобы восстановить работоспособность на локальном сайте, необходим сервер обработки в Azure.<br/><br/> Сервер обработки получает данные из защищенной виртуальной машины Azure и отправляет их на локальный сайт.<br/><br/> Требуется сеть с низкой задержкой между сервером обработки и защищенной виртуальной машиной, поэтому рекомендуется развернуть сервер обработки в Azure для повышения производительности репликации.<br/><br/> Для подтверждения концепции можно использовать локальный сервер обработки и ExpressRoute с частными пиринга.<br/><br/> Сервер обработки должен находиться в сети Azure, в которой находится виртуальная машина, для которой выполнена отработка отказа. Сервер обработки также должен иметь возможность взаимодействия с локальным сервером конфигурации и главным целевым сервером.
**Отдельный главный целевой сервер** | Главный целевой сервер получает данные восстановления размещения, и по умолчанию главный целевой сервер Windows работает на локальном сервере конфигурации.<br/><br/> К главному целевому серверу может быть подключено до 60 дисков. Виртуальные машины, для которых выполняется сбой, имеют больше общего количества 60 дисков или если вы не выполняете резервное копирование больших объемов трафика, создайте отдельный главный целевой сервер для восстановления размещения.<br/><br/> Если компьютеры собираются в группу репликации для согласованности с несколькими виртуальными машинами, виртуальные машины должны быть Windows или все они должны быть Linux. Почему? Так как все виртуальные машины в группе репликации должны использовать один и тот же главный целевой сервер, а на главном целевом сервере должна быть установлена одна и та же операционная система (с той же или более поздней версией), чем на реплицированных компьютерах.<br/><br/> Главный целевой сервер не должен иметь моментальных снимков на своих дисках, в противном случае повторное включение защиты и восстановление размещения не будет работать.<br/><br/> На главном целевом сервере не может находиться контроллер Paravirtual SCSI. На нем может быть только контроллер LSI Logic. Если контроллер LSI Logic отсутствует, при повторном включении защиты происходит сбой.
**Политика репликации восстановления размещения** | Для повторной репликации на локальный сайт требуется политика восстановления размещения. Эта политика создается автоматически при создании политики репликации в Azure.<br/><br/> Политика автоматически сопоставляется с сервером конфигурации. Для него задано пороговое значение RPO, равное 15 минутам, срок хранения точки восстановления — 24 часа, а периодичность моментальных снимков с учетом приложений — 60 минут. Невозможно изменить политику. 
**Частный пиринг VPN типа "сеть — сеть" или ExpressRoute** | Для повторного включения защиты и восстановления размещения требуется VPN-подключение типа "сеть — сеть" или частный пиринг ExpressRoute для репликации данных. 


## <a name="ports-for-reprotectionfailback"></a>Порты для повторного включения защиты или восстановления размещения

Для повторного включения защиты или восстановления размещения необходимо открыть несколько портов. На следующем рисунке показаны порты и повторная защита и восстановление размещения.

![Порты для отработки отказа и восстановления размещения](./media/vmware-azure-reprotect/failover-failback.png)


## <a name="deploy-a-process-server-in-azure"></a>Развертывание сервера обработки в Azure

1. [Настройте сервер обработки](vmware-azure-set-up-process-server-azure.md) в Azure для восстановления размещения.
2. Убедитесь, что виртуальные машины Azure могут подключиться к серверу обработки. 
3. Убедитесь, что VPN-подключение типа "сеть — сеть" или сеть частного пиринга ExpressRoute имеет достаточно пропускную способность для отправки данных с сервера обработки на локальный сайт.

## <a name="deploy-a-separate-master-target-server"></a>Развертывание главного целевого сервера

1. Обратите внимание на требования к главному целевому серверу [и ограничения](#reprotectionfailback-components).
2. Создайте главный целевой сервер [Windows](site-recovery-plan-capacity-vmware.md#deploy-additional-master-target-servers) или [Linux](vmware-azure-install-linux-master-target.md) в соответствии с операционной системой виртуальных машин, для которых необходимо повторно включить защиту и восстановить размещение.
3. Убедитесь, что для главного целевого сервера не используется хранилище vMotion, или восстановление размещения может завершиться сбоем. Не удается запустить компьютер виртуальной машины из-за недоступности дисков.
    - Чтобы избежать этого, исключите главный целевой сервер из списка vMotion.
    - Если после повторного включения защиты главный целевой сервер выполняет задачу vMotion хранилища, защищенные диски виртуальной машины, подключенные к главному целевому серверу, переходят на целевой объект задачи vMotion. При попытке восстановить диск после этого произойдет сбой отключения диска, так как диски не найдены. После этого трудно найти диски в учетных записях хранения. Если это происходит, найдите их вручную и подключите их к виртуальной машине. После этого можно загрузить локальную виртуальную машину.

4. Добавьте диск хранения к существующему главному целевому серверу Windows. Добавьте новый диск и отформатируйте его. Диск хранения используется для остановки точек во времени, когда виртуальная машина реплицируется обратно на локальный сайт. Обратите внимание на эти критерии. Если они не соблюдаются, значит, диск отсутствует в списке для главного целевого сервера:
    - Том не используется для других целей, таких как целевой объект репликации, и не находится в режиме блокировки.
    - Том не является томом кэша. Пользовательский том установки сервера обработки и главного целевого сервера не может использоваться как том хранения. Если на этом томе установлен сервер обработки и главный целевой сервер, он используется в качестве тома кэша главного целевого сервера.
    - Типом файловой системы тома не является FAT или FAT32.
    - Размер тома не равен нулю.
    - Том хранения по умолчанию для Windows — это том R.
    - Том хранения по умолчанию для Linux — /mnt/retention.

5. Добавьте диск, если вы используете существующий сервер обработки. Новый диск должен соответствовать требованиям на последнем шаге. Если диск хранения отсутствует, он не отображается в раскрывающемся списке на портале. После добавления диска на локальный главный целевой сервер для его появления на портале может потребоваться до 15 минут. Если диск не отображается через 15 минут, можно обновить сервер конфигурации.
6. Установите инструменты VMware или open-vm-tools на главном целевом сервере. Без этих инструментов не удастся обнаружить хранилища данных на узле ESXi главного целевого сервера.
7. Задайте диск. EnableUUID = true параметр в параметрах конфигурации главной целевой виртуальной машины в VMware. Если этой строки нет, добавьте ее. Этот параметр необходим для обеспечения согласованного UUID для правильного подключения VMDK.
8. Проверьте vCenter Server требования к доступу:
    - Если виртуальная машина, на которую выполняется возврат, находится на ESXi узле, управляемом VMware vCenter Server, главному целевому серверу требуется доступ к локальному виртуальному диску виртуальной машины (VMDK) для записи реплицированных данных на диски виртуальной машины. Убедитесь, что локальное хранилище виртуальных машин подключено к главному целевому узлу с доступом на чтение и запись.
    - Если виртуальная машина не находится на узле ESXi, управляемом VMware vCenter Server, Site Recovery создает новую виртуальную машину во время повторного включения защиты. Эта виртуальная машина создается на узле ESXi, на котором создается виртуальная машина главного целевого сервера. Тщательно выберите узел ESXi, чтобы создать виртуальную машину на нужном узле. Жесткий диск виртуальной машины должен находиться в хранилище данных, доступном для узла, на котором работает главный целевой сервер.
    - Или при наличии локальной виртуальной машины для восстановления размещения удалите ее, прежде чем выполнить восстановление размещения. Затем восстановление размещения создает новую виртуальную машину на том же узле, что и главный целевой узел ESXi. При восстановлении размещения в альтернативном расположении данные восстанавливаются в то же хранилище данных и на том же узле ESXi, что и на локальном главном целевом сервере.
9. Для физических компьютеров, которые не возвращаются на виртуальные машины VMware, следует завершить обнаружение узла, на котором работает главный целевой сервер, прежде чем можно будет повторно защитить компьютер.
10. Убедитесь, что к узлу ESXi, на котором Главная Целевая виртуальная машина, подключено по крайней мере одно хранилище данных файловой системы виртуальной машины (VMFS). Если ни одно хранилище данных VMFS не присоединено, то входные данные в параметрах повторного включения защиты будут пустыми, и вы не сможете продолжать работу.


## <a name="next-steps"></a>Дальнейшие шаги

Повторно [включите защиту](vmware-azure-reprotect.md) виртуальной машины.
