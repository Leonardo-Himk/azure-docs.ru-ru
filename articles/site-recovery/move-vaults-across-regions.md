---
title: Перемещение хранилища Azure Site Recovery в другой регион
description: Описание того, как переместить хранилище Служб восстановления (Azure Site Recovery) в другой регион Azure
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 32dff9a165125ab1949560ce36438ae266cd3036
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "74090296"
---
# <a name="move-a-recovery-services-vault-and-azure-site-recovery-configuration-to-another-azure-region"></a>Перемещение хранилища Служб восстановления и конфигурации Azure Site Recovery в другой регион Azure

Существуют разные причины, по которым может потребоваться перенос существующих ресурсов Azure из одного региона в другой. Например, он может выполняться, чтобы упростить управление или вследствие слияний и приобретений компаний. При перемещении виртуальных машин Azure, возможно, также понадобиться перенести конфигурацию аварийного восстановления. 

Совершенного способа для перемещения имеющейся конфигурации аварийного восстановления из одного региона в другой не существует. Это связано с тем, что целевой регион настраивается на основе исходного региона виртуальной машины. После изменения исходного региона имеющиеся конфигурации целевого региона станут недоступными для повторного использования, поэтому их необходимо сбросить. В этой статье описан пошаговый процесс повторной настройки аварийного восстановления и перемещения в другой регион.

В рамках этого документа вы будете выполнять следующие операции:

> [!div class="checklist"]
> * проверка предварительных требований для переноса;
> * определение ресурсов, которые использовались службой Azure Site Recovery;
> * отключение репликации;
> * удаление ресурсов;
> * настройка Site Recovery на основе нового исходного региона для виртуальных машин.

> [!IMPORTANT]
> В настоящее время не существует совершенного способа для перемещения хранилища Служб восстановления и конфигурации аварийного восстановления в другой регион. В этой статье описывается процесс отключения репликации и ее настройки в новом регионе.

## <a name="prerequisites"></a>предварительные требования

- Удалите конфигурацию аварийного восстановления, прежде чем пытаться переместить виртуальные машины Azure в другой регион. 

  > [!NOTE]
  > Если ваш целевой регион для виртуальной машины Azure совпадает с целевым регионом аварийного восстановления, вы можете использовать существующую конфигурацию репликации и выполнить перемещение. Выполните действия, указанные в статье [Перенос виртуальных машин Azure в другой регион](azure-to-azure-tutorial-migrate.md).

- Убедитесь, что вы принимаете обоснованное решение и что заинтересованные лица уведомлены об этом. Виртуальная машина не будет защищена от аварий, пока не завершится ее перемещение.

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>Определение ресурсов, которые использовались службой Azure Site Recovery.
Мы рекомендуем выполнить этот шаг, прежде чем переходить к следующему. При репликации виртуальных машин идентифицировать важные ресурсы проще.

В каждой реплицируемой виртуальной машине Azure выберите **Защищенные элементы** > **Реплицированные элементы** > **Свойства** и найдите следующие ресурсы:

- Целевая группа ресурсов
- Учетная запись хранения кэша.
- Целевая учетная запись хранения (в случае виртуальной машины Azure на основе неуправляемого диска). 
- Целевая сеть.


## <a name="disable-the-existing-disaster-recovery-configuration"></a>Отключение имеющейся конфигурации аварийного восстановления

1. Перейдите к хранилищу Служб восстановления.
2. Выберите **Защищенные элементы** > **Реплицированные элементы**, щелкните имя виртуальной машины правой кнопкой мыши и выберите **Отключить репликацию**.
3. Сделайте это для всех виртуальных машин, которые необходимо переместить.

> [!NOTE]
> Служба мобильности не будет удалена с защищенных серверов, поэтому ее необходимо удалить вручную. Если вы планируете повторно защитить сервер, то удаление службы мобильности можно пропустить.

## <a name="delete-the-resources"></a>Удаление ресурсов

1. Перейдите к хранилищу Служб восстановления.
2. Выберите команду **Удалить**.
3. Удалите все другие ресурсы, [идентифицированные ранее](#identify-the-resources-that-were-used-by-azure-site-recovery).
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Перенос виртуальных машин Azure в целевой регион

Выполните действия, приведенные в этих статьях. Действия будут зависеть от ваших требований к переносу виртуальных машин Azure в целевой регион.

- [Перенос виртуальных машин Azure в другой регион](azure-to-azure-tutorial-migrate.md)
- [Перенос виртуальных машин Azure в Зоны доступности](move-azure-VMs-AVset-Azone.md)

## <a name="set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>Настройка Site Recovery на основе нового исходного региона для виртуальных машин

Настройте аварийное восстановление для виртуальных машин Azure, которые были перемещены в новый регион, выполнив действия, приведенные в статье [Настройка аварийного восстановления для виртуальных машин Azure](azure-to-azure-tutorial-enable-replication.md).
