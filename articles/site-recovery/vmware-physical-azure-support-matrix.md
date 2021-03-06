---
title: Матрица поддержки для VMware или физического аварийного восстановления в Azure Site Recovery
description: Содержит сводку по поддержке аварийного восстановления виртуальных машин VMware и физического сервера в Azure с помощью Azure Site Recovery.
ms.topic: conceptual
ms.date: 2/24/2020
ms.openlocfilehash: d8e7b2f8f6483d462f781d95011ef7b972e83b87
ms.sourcegitcommit: c8a0fbfa74ef7d1fd4d5b2f88521c5b619eb25f8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82801796"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>Таблица поддержки аварийного восстановления виртуальных машин VMware и физических серверов в Azure

В этой статье перечислены поддерживаемые компоненты и параметры для аварийного восстановления виртуальных машин VMware и физических серверов в Azure с помощью [Azure Site Recovery](site-recovery-overview.md).

- [Узнайте больше](vmware-azure-architecture.md) об архитектуре аварийного восстановления виртуальных машин VMware или физических серверов.
- Следуйте нашим [руководствам](tutorial-prepare-azure.md) , чтобы испытать аварийное восстановление.

## <a name="deployment-scenarios"></a>Сценарии развертывания

**Сценарий** | **Сведения**
--- | ---
Аварийное восстановление виртуальных машин VMware | Репликация локальных виртуальных машин VMware в Azure. Этот сценарий можно развернуть в портал Azure или с помощью [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Аварийное восстановление физических серверов | Репликация локальных физических серверов Windows или Linux в Azure. Этот сценарий можно развернуть с помощью портала Azure.

## <a name="on-premises-virtualization-servers"></a>Локальные серверы виртуализации

**Сервер** | **Требования** | **Сведения**
--- | --- | ---
с сервером vCenter | Версии 6,7, 6,5, 6,0 или 5,5 | Рекомендуется использовать vCenter Server в развертывании аварийного восстановления.
Узлы vSphere | Версии 6,7, 6,5, 6,0 или 5,5 | Мы рекомендуем размещать узлы vSphere и серверы vCenter в той же сети, в которой находится сервер обработки. По умолчанию сервер обработки запускается на сервере конфигурации. [Дополнительные сведения](vmware-physical-azure-config-process-server-overview.md).


## <a name="site-recovery-configuration-server"></a>Сервер конфигурации Site Recovery

Сервер конфигурации размещается на локальном компьютере, где выполняются компоненты Site Recovery, включая сервер конфигурации, сервер обработки и главный целевой сервер.

- Для виртуальных машин VMware необходимо настроить сервер конфигурации, загрузив шаблон OVF, чтобы создать виртуальную машину VMware.
- Для физических серверов Настройка компьютера сервера конфигурации выполняется вручную.

**Компонент** | **Требования**
--- |---
Ядра ЦП | 8
ОЗУ | 16 ГБ
Количество дисков | 3 диска<br/><br/> В состав дисков входят: диск ОС, диск кэша сервера обработки, диск хранения (для восстановления размещения).
Свободное место на диске | 600 ГБ пространства для кэша сервера обработки.
Свободное место на диске | 600 ГБ пространства для диска хранения.
Операционная система  | Windows Server 2012 R2 или Windows Server 2016 с возможностями рабочего стола <br/><br> Если вы планируете использовать встроенный главный целевой сервер для восстановления размещения, убедитесь, что версия ОС совпадает с версией реплицированных элементов или выше нее.|
Язык операционной системы | Английский (en-us)
[PowerCLI](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) | Не требуется для сервера конфигурации [9,14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) или более поздней версии.
Роли Windows Server | Не включайте службы домен Active Directory; Службы IIS (IIS) или Hyper-V.
Групповые политики| — запрет на использование командной строки; <br/> — запрет на использование инструментов редактирования реестра; <br/> — логика доверия для вложенных файлов; <br/> — включение выполнения скриптов. <br/> - [Подробнее](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Не забудьте выполнить следующие действия.<br/><br/> -У вас нет предварительно существующего веб-сайта по умолчанию <br/> — включите [анонимную аутентификацию](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx); <br/> — включите параметр [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx);  <br/> — убедитесь, что отсутствует предварительно созданный веб-сайт или приложение, ожидающее передачи данных через порт 443.<br/>
Тип сетевой карты | VMXNET3 (при развертывании в качестве виртуальной машины VMware)
Тип IP-адреса | Статические
Порты | 443 используется для оркестрации канала управления<br/>9443 для транспорта данных

## <a name="replicated-machines"></a>Реплицируемые компьютеры

Site Recovery поддерживает репликацию любой рабочей нагрузки, выполняемой на поддерживаемом компьютере.

> [!Note]
> В следующей таблице перечислены поддерживаемые компьютеры с загрузкой BIOS. Сведения о поддержке на компьютерах на основе UEFI см. в разделе " [хранилище](#storage) ".

**Компонент** | **Сведения**
--- | ---
Параметры компьютера | Компьютеры, которые реплицируются в Azure, должны соответствовать [требованиям Azure](#azure-vm-requirements).
Рабочая нагрузка компьютера | Site Recovery поддерживает репликацию любой рабочей нагрузки, выполняемой на поддерживаемом компьютере. [Дополнительные сведения](https://aka.ms/asr_workload).
Имя компьютера | Убедитесь, что отображаемое имя компьютера не попадает в [зарезервированные имена ресурсов Azure](https://docs.microsoft.com/azure/azure-resource-manager/templates/error-reserved-resource-name) .<br/><br/> В именах логических томов регистр не учитывается. Убедитесь, что имена двух томов на устройстве не совпадают. Пример: тома с именами "voLUME1", "voLUME1" не могут быть защищены с помощью Azure Site Recovery.
Windows Server 2019 | Поддерживается из [накопительного пакета обновления 34](https://support.microsoft.com/help/4490016) (версия 9,22 службы Mobility Service), начиная с версии.
Windows Server 2016 64-bit | Поддерживается для Server Core, Server с возможностями рабочего стола.
Windows Server 2012 R2 или Windows Server 2012 | Поддерживается.
Windows Server 2008 R2 с пакетом обновления 1 (SP1) — назад. | Поддерживается.<br/><br/> В версии [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) агента службы Mobility Service требуется [Обновление стека обслуживания (SSU)](https://support.microsoft.com/help/4490628) и [Обновление SHA-2](https://support.microsoft.com/help/4474419) , установленные на компьютерах под управлением Windows 2008 R2 с пакетом обновления 1 (SP1) или более поздней версии. SHA-1 не поддерживается с 2019 сентября, а если подпись кода SHA-2 не включена, расширение агента не будет устанавливаться и обновляться должным образом. Дополнительные сведения об [обновлении и требованиях SHA-2](https://aka.ms/SHA-2KB).
Windows Server 2008 с пакетом обновления 2 (SP2) или более поздней версии (64-или 32-разрядная версия) |  Поддерживается только для миграции. [Дополнительные сведения](migrate-tutorial-windows-server-2008.md).<br/><br/> В версии [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) агента службы Mobility Service требуется [Обновление стека обслуживания (SSU)](https://support.microsoft.com/help/4493730) и [Обновление SHA-2](https://support.microsoft.com/help/4474419) , установленные на компьютерах под управлением Windows 2008 с пакетом обновления 2 (SP2). ИША-1 не поддерживается в сентябре 2019, и если не включено подписывание кода SHA-2, расширение агента не будет устанавливаться и обновляться должным образом. Дополнительные сведения об [обновлении и требованиях SHA-2](https://support.microsoft.com/en-us/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus).
Windows 10, Windows 8.1, Windows 8 | Поддерживается.
Windows 7 с пакетом обновления 1 (SP1) 64-bit | Поддерживается из [накопительного пакета обновления 36](https://support.microsoft.com/help/4503156) (версия 9,22 службы Mobility Service), начиная с версии. </br></br> С [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) агента службы Mobility Service требуется [Обновление стека обслуживания (SSU)](https://support.microsoft.com/help/4490628) и [Обновление SHA-2](https://support.microsoft.com/help/4474419) на компьютерах под управлением Windows 7 с пакетом обновления 1 (SP1).  SHA-1 не поддерживается с 2019 сентября, а если подпись кода SHA-2 не включена, расширение агента не будет устанавливаться и обновляться должным образом. Дополнительные сведения об [обновлении и требованиях SHA-2](https://support.microsoft.com/en-us/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus).
Linux | Поддерживается только 64-разрядная система. 32-разрядная система не поддерживается.<br/><br/>На каждом сервере Linux должны быть установлены [компоненты Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) . После тестовой отработки отказа или отработки отказа необходимо загрузить сервер в Azure. Если в системе отсутствуют встроенные компоненты LIS, установите [их перед](https://www.microsoft.com/download/details.aspx?id=55106) включением репликации для загрузки виртуальных машин в Azure. <br/><br/> Site Recovery координирует отработку отказа для запуска серверов Linux в Azure. Но поставщики Linux могут поддерживать только те версии дистрибутивов, срок поддержки которых еще не окончился.<br/><br/> Поддерживаются только дистрибутивы Linux на основе номенклатурных ядер, которые являются частью выпуска или обновления дополнительной версии.<br/><br/> Обновление защищенных виртуальных машин с основной версией дистрибутивов Linux не поддерживается. Для обновления отключите репликацию, обновите операционную систему и включите репликацию снова.<br/><br/> Дополнительные [сведения](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) о поддержке Linux и технологии с открытым кодом в Azure.
Linux Red Hat Enterprise | от 5,2 до 5,11</b><br/> от 6,1 до 6,10</b> </br> 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, 7,6, [7,7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [8,0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), 8,1 <br/> Несколько старых ядер на серверах, на которых работает Red Hat Enterprise Linux 5.2 — 5.11 & 6.1-6.10, не имеют предварительно установленных [компонентов Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) . Если в системе отсутствуют встроенные компоненты LIS, установите [их перед](https://www.microsoft.com/download/details.aspx?id=55106) включением репликации для загрузки виртуальных машин в Azure.
Linux: CentOS | от 5,2 до 5,11</b><br/> от 6,1 до 6,10</b><br/> от 7,0 до 7,6<br/> <br/> от 8,0 до 8,1<br/><br/> Несколько старых ядер на серверах с CentOS 5.2 — 5.11 & 6.1-6.10 не имеют предварительно установленных [компонентов Integration Services Linux (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) . Если в системе отсутствуют встроенные компоненты LIS, установите [их перед](https://www.microsoft.com/download/details.aspx?id=55106) включением репликации для загрузки виртуальных машин в Azure.
Ubuntu | Ubuntu 14,04 LTS Server [(Обзор поддерживаемых версий ядра)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16,04 LTS Server [(Обзор поддерживаемых версий ядра)](#ubuntu-kernel-versions) </br> Ubuntu 18,04 LTS Server [(Обзор поддерживаемых версий ядра)](#ubuntu-kernel-versions)
Debian | Debian 7/Debian 8 [(Обзор поддерживаемых версий ядра)](#debian-kernel-versions)
SUSE Linux | SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4 [(Обзор поддерживаемых версий ядра)](#suse-linux-enterprise-server-12-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 15, 15 с пакетом обновления 1 [(SP1) (Обзор поддерживаемых версий ядра)](#suse-linux-enterprise-server-15-supported-kernel-versions)<br/> SUSE Linux Enterprise Server 11 с пакетом обновления 3 или SUSE Linux Enterprise Server 11 с пакетом обновления 4<br/> Обновление реплицированных виртуальных машин с SUSE Linux Enterprise Server 11 SP3 до SP4 не поддерживается. Чтобы выполнить обновление, отключите репликацию и снова включите ее после обновления.
Oracle Linux | 6,4, 6,5, 6,6, 6,7, 6,8, 6,9, 6,10, 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, 7,6, [7,7](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery)<br/><br/> Запуск ядра, совместимого с Red Hat, или неразрывный выпуск Enterprise ядра 3, 4 & 5 (UEK3, UEK4, UEK5)

> [!Note]
> Для каждой версии Windows Azure Site Recovery поддерживает только [долгосрочные сборки канала обслуживания (LTSC)](/windows-server/get-started-19/servicing-channels-19#long-term-servicing-channel-ltsc) .  В настоящее время не поддерживается [полугодовые](/windows-server/get-started-19/servicing-channels-19#semi-annual-channel) выпуски канала.

### <a name="ubuntu-kernel-versions"></a>Версии ядра Ubuntu

**Поддерживаемый выпуск** | **Версия службы Mobility Service** | **Версия ядра** |
--- | --- | --- |
14.04 LTS | [9,32][9.32 UR] | 3.13.0-24-Generic для 3.13.0-170-generic,<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>4.4.0-21-Generic для 4.4.0-148-generic,<br/>4.15.0-1023 — Azure to 4.15.0-1045-Azure |
14.04 LTS | [9,31][9.31 UR] | 3.13.0-24-Generic для 3.13.0-170-generic,<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>4.4.0-21-Generic для 4.4.0-148-generic,<br/>4.15.0-1023 — Azure to 4.15.0-1045-Azure |
14.04 LTS | [9,30][9.30 UR] | 3.13.0-24-Generic для 3.13.0-170-generic,<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>4.4.0-21-Generic для 4.4.0-148-generic,<br/>4.15.0-1023 — Azure to 4.15.0-1045-Azure |
14.04 LTS | [9,29][9.29 UR]| 3.13.0-24-Generic для 3.13.0-170-generic,<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>4.4.0-21-Generic для 4.4.0-148-generic,<br/>4.15.0-1023 — Azure to 4.15.0-1045-Azure |
|||
16.04 LTS | [9,32][9.32 UR] | 4.4.0-21-Generic для 4.4.0-171-generic,<br/>с 4.8.0-34-generic по 4.8.0-58-generic<br/>с 4.10.0-14-generic по 4.10.0-42-generic,<br/>с 4.11.0-13-generic по 4.11.0-14-generic,<br/>с 4.13.0-16-generic по 4.13.0-45-generic,<br/>4.15.0-13-Generic для 4.15.0-74-Generic<br/>с 4.11.0-1009-azure по 4.11.0-1016-azure,<br/>с 4.13.0-1005-azure по 4.13.0-1018-azure. <br/>4.15.0-1012-Azure в 4.15.0-1066-Azure|
16.04 LTS | [9,31][9.31 UR] | 4.4.0-21-Generic — 4.4.0-170-generic,<br/>с 4.8.0-34-generic по 4.8.0-58-generic<br/>с 4.10.0-14-generic по 4.10.0-42-generic,<br/>с 4.11.0-13-generic по 4.11.0-14-generic,<br/>с 4.13.0-16-generic по 4.13.0-45-generic,<br/>4.15.0-13-Generic — 4.15.0-72-Generic<br/>с 4.11.0-1009-azure по 4.11.0-1016-azure,<br/>с 4.13.0-1005-azure по 4.13.0-1018-azure. <br/>4.15.0-1012 — Azure — 4.15.0 — 1063 — Azure|
16.04 LTS | [9,30][9.30 UR] | 4.4.0-21-Generic для 4.4.0-166-generic,<br/>с 4.8.0-34-generic по 4.8.0-58-generic<br/>с 4.10.0-14-generic по 4.10.0-42-generic,<br/>с 4.11.0-13-generic по 4.11.0-14-generic,<br/>с 4.13.0-16-generic по 4.13.0-45-generic,<br/>4.15.0-13-Generic для 4.15.0-66-Generic<br/>с 4.11.0-1009-azure по 4.11.0-1016-azure,<br/>с 4.13.0-1005-azure по 4.13.0-1018-azure. <br/>4.15.0-1012 — Azure — 4.15.0 — 1061 — Azure|
16.04 LTS | [9,29][9.29 UR] | 4.4.0-21-Generic для 4.4.0-164-generic,<br/>с 4.8.0-34-generic по 4.8.0-58-generic<br/>с 4.10.0-14-generic по 4.10.0-42-generic,<br/>с 4.11.0-13-generic по 4.11.0-14-generic,<br/>с 4.13.0-16-generic по 4.13.0-45-generic,<br/>4.15.0-13-Generic — 4.15.0-64-Generic<br/>с 4.11.0-1009-azure по 4.11.0-1016-azure,<br/>с 4.13.0-1005-azure по 4.13.0-1018-azure. <br/>4.15.0-1012 — Azure — 4.15.0 — 1059 — Azure|
|||
18,04 LTS | [9,32][9.32 UR]| 4.15.0-20-Generic для 4.15.0-74-Generic </br> 4.18.0-13-Generic для 4.18.0-25-Generic </br> 5.0.0-15-Generic до 5.0.0-37-Generic </br> 5.3.0-19-Generic для 5.3.0-24-Generic </br> 4.15.0-1009-Azure в 4.15.0-1037-Azure </br> 4.18.0-1006 — Azure — 4.18.0-1025 — Azure </br> 5.0.0-1012 — Azure — 5.0.0-1028 — Azure </br> 5.3.0-1007-Azure в 5.3.0-1009-Azure|
18,04 LTS | [9,31][9.31 UR]| 4.15.0-20-Generic для 4.15.0-72-Generic </br> 4.18.0-13-Generic для 4.18.0-25-Generic </br> 5.0.0-15-Generic до 5.0.0-37-Generic </br> 5.3.0-19-Generic для 5.3.0-24-Generic </br> 4.15.0-1009-Azure в 4.15.0-1037-Azure </br> 4.18.0-1006 — Azure — 4.18.0-1025 — Azure </br> 5.0.0-1012 — Azure — 5.0.0-1025 — Azure </br> 5.3.0-1007 — Azure|
18,04 LTS | [9,30][9.30 UR] | 4.15.0-20-Generic для 4.15.0-66-Generic </br> 4.18.0-13-Generic для 4.18.0-25-Generic </br> 5.0.0-15-Generic до 5.0.0-32-Generic </br> 4.15.0-1009-Azure в 4.15.0-1037-Azure </br> 4.18.0-1006 — Azure — 4.18.0-1025 — Azure </br> 5.0.0-1012 — Azure — 5.0.0-1023 — Azure|
18,04 LTS | [9,29][9.29 UR] | 4.15.0-20-Generic — 4.15.0-62-Generic </br> 4.18.0-13-Generic для 4.18.0-25-Generic </br> 5.0.0-15-Generic до 5.0.0-27-Generic </br> 4.15.0-1009-Azure в 4.15.0-1037-Azure </br> 4.18.0-1006 — Azure — 4.18.0-1025 — Azure </br> 5.0.0-1012-Azure – 5.0.0-1018-Azure|

### <a name="debian-kernel-versions"></a>Версии ядра Debian


**Поддерживаемый выпуск** | **Версия службы Mobility Service** | **Версия ядра** |
--- | --- | --- |
Debian 7 | [9,29][9.29 UR], [9,30][9.30 UR], [9,31][9.31 UR], [9,32][9.32 UR]| С 3.2.0-4-amd64 по 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9,30][9.30 UR], [9,31][9.31 UR], [9,32][9.32 UR] | 3.16.0-4-AMD64 – 3.16.0-10-AMD64, 4.9.0 -0. BPO. 4-AMD64 до 4.9.0 -0. BPO. 11 — AMD64 |
Debian 8 | [9,29][9.29 UR] | 3.16.0-4-AMD64 – 3.16.0-10-AMD64, 4.9.0 -0. BPO. 4-AMD64 до 4.9.0 -0. BPO. 9 — AMD64 |

### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>Поддерживаемые версии ядра SUSE Linux Enterprise Server 12

**Release** | **Версия службы Mobility Service** | **Версия ядра** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | [9,28][9.28 UR] | С SP1 3.12.49-11-default по 3.12.74-60.64.40-default</br></br> SP1 (ЛТСС) 3.12.74-60.64.45-по умолчанию — 3.12.74-60.64.118 — по умолчанию</br></br> С SP2 4.4.21-69-default по 4.4.120-92.70-default</br></br>SP2 (ЛТСС) 4.4.121-92.73-по умолчанию — 4.4.121-92.117 — по умолчанию</br></br>SP3 4.4.73-5-по умолчанию используется 4.4.180-94.100-Default</br></br>SP3 4.4.138-4.7-Azure в 4.4.180-4.31-Azure</br></br>SP4 4.12.14-94.41-Default для 4.12.14-95.29-Default</br>SP4 4.12.14-6.3-Azure в 4.12.14-6.23-Azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | [9,27][9.27 UR] | С SP1 3.12.49-11-default по 3.12.74-60.64.40-default</br></br> SP1 (ЛТСС) 3.12.74-60.64.45-по умолчанию — 3.12.74-60.64.115 — по умолчанию</br></br> С SP2 4.4.21-69-default по 4.4.120-92.70-default</br></br>SP2 (ЛТСС) 4.4.121-92.73-по умолчанию — 4.4.121-92.114 — по умолчанию</br></br>SP3 4.4.73-5-по умолчанию используется 4.4.180-94.97-Default</br></br>SP3 4.4.138-4.7-Azure в 4.4.180-4.31-Azure</br></br>SP4 4.12.14-94.41-Default для 4.12.14-95.19-Default</br>SP4 4.12.14-6.3-Azure в 4.12.14-6.15-Azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | [9,26][9.26 UR] | С SP1 3.12.49-11-default по 3.12.74-60.64.40-default</br></br> SP1 (ЛТСС) 3.12.74-60.64.45-по умолчанию — 3.12.74-60.64.110 — по умолчанию</br></br> С SP2 4.4.21-69-default по 4.4.120-92.70-default</br></br>SP2 (ЛТСС) 4.4.121-92.73-по умолчанию — 4.4.121-92.109 — по умолчанию</br></br>SP3 4.4.73-5-по умолчанию используется 4.4.178-94.91-Default</br></br>SP3 4.4.138-4.7-Azure в 4.4.178-4,28-Azure</br></br>SP4 4.12.14-94.41-Default для 4.12.14-95.16-Default</br>SP4 4.12.14-6.3-Azure до 4.12.14-6,9-Azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | [9,25][9.25 UR] | С SP1 3.12.49-11-default по 3.12.74-60.64.40-default</br></br> С SP1(LTSS) 3.12.74-60.64.45-default по 3.12.74-60.64.107-default</br></br> С SP2 4.4.21-69-default по 4.4.120-92.70-default</br></br>SP2 (ЛТСС) 4.4.121-92.73-по умолчанию — 4.4.121-92.104 — по умолчанию</br></br>SP3 4.4.73-5-по умолчанию используется 4.4.176-94.88-Default</br></br>SP3 4.4.138-4.7-Azure в 4.4.176-4.25-Azure</br></br>SP4 4.12.14-94.41-Default для 4.12.14-95.13-Default</br>SP4 4.12.14-6.3-Azure до 4.12.14-6,9-Azure |

### <a name="suse-linux-enterprise-server-15-supported-kernel-versions"></a>SUSE Linux Enterprise Server 15 поддерживаемых версий ядра

**Release** | **Версия службы Mobility Service** | **Версия ядра** |
--- | --- | --- |
SUSE Linux Enterprise Server 15 и 15 с пакетом обновления 1 | [9,32](https://support.microsoft.com/help/4550047/) | По умолчанию поддерживаются все [ядра для SuSE 15 и 15 ядер](https://www.suse.com/support/kb/doc/?id=000019587) . </br></br> 4.12.14-5.5 — Azure — 4.12.14 — 8.22 — Azure



## <a name="linux-file-systemsguest-storage"></a>Файловые системы Linux и гостевое хранилище

**Компонент** | **Поддерживается**
--- | ---
Файловые системы | ext3, ext4, XFS, БТРФС (условия, применимые в этой таблице)
Подготовка управления логическими томами (LVM)| Толстая инициализация — да <br></br> Тонкая инициализация — нет
Диспетчер томов | -LVM поддерживается.<br/> параметр-/Boot в LVM поддерживается начиная с [накопительного пакета обновления 31](https://support.microsoft.com/help/4478871/) (версия 9,20 службы Mobility Service). Она не поддерживается в предыдущих версиях службы Mobility Service.<br/> -Несколько дисков ОС не поддерживаются.
Паравиртуализированные запоминающие устройства | Устройства, экспортированные с помощью паравиртуализированных драйверов, не поддерживаются.
Блочные устройства ввода-вывода с несколькими очередями | Не поддерживается.
Физические серверы с контроллером хранилища HP CCISS | Не поддерживается.
Соглашение об именовании точки устройства/подключения | Имя устройства или точки подключения должно быть уникальным.<br/> Убедитесь, что имена двух устройств и точек подключения не содержат учет регистра. Например, имена устройств для одной виртуальной машины с *device1* и *device1* не поддерживаются.
Каталоги | Если вы используете версию службы Mobility Service, предшествующую версии 9,20 (выпущенную в [накопительном пакете обновления 31](https://support.microsoft.com/help/4478871/)), то действуют следующие ограничения.<br/><br/> — Эти каталоги (если они настроены как отдельные разделы и файловые системы) должны находиться на одном диске ОС на исходном сервере:/(root),/Boot,/usr,/usr/local,/var,/etc.</br> — Каталог/boot должен находиться в разделе диска, а не в томе LVM.<br/><br/> Начиная с версии 9,20 эти ограничения не применяются. 
Каталог загрузок | — Загрузочные диски не должны должны быть в формате раздела GPT. Это ограничение архитектуры Azure. Диски GPT поддерживаются в качестве дисков данных.<br/><br/> Несколько загрузочных дисков на виртуальной машине не поддерживаются.<br/><br/> -/Boot на томе LVM на нескольких дисках не поддерживается.<br/> — Невозможно реплицировать компьютер без загрузочного диска.
Требования к свободному месту| 2 ГБ в разделе /root <br/><br/> 250 МБ в папке установки
XFSv5 | Поддерживаются функции XFSv5 в файловых системах XFS, таких как контрольная сумма метаданных (служба Mobility Service версии 9,10).<br/> Используйте служебную программу xfs_info, чтобы проверить системный блок XFS для раздела. Если `ftype` параметр имеет значение 1, то используются XFSv5 компоненты.
бтрфс | БТРФС поддерживается из [накопительного пакета обновления 34](https://support.microsoft.com/help/4490016) (версия 9,22 службы Mobility Service). БТРФС не поддерживается, если:<br/><br/> — БТРФС файловая система файловой системы изменяется после включения защиты.</br> — Файловая система БТРФС распределена по нескольким дискам.</br> — Файловая система БТРФС поддерживает RAID.

## <a name="vmdisk-management"></a>Управление виртуальной машиной и диском

**Действие** | **Сведения**
--- | ---
Изменение размера диска на реплицируемой виртуальной машине | Поддерживается на исходной виртуальной машине перед отработкой отказа непосредственно в свойствах виртуальной машины. Отключение и повторное включение репликации не требуется.<br/><br/> При изменении исходной виртуальной машины после отработки отказа изменения не фиксируются.<br/><br/> При изменении размера диска на виртуальной машине Azure после отработки отказа при восстановлении после сбоя Site Recovery создает новую виртуальную машину с обновлениями.
Добавление диска к реплицируемой виртуальной машине | Не поддерживается.<br/> Отключите репликацию для виртуальной машины, добавьте диск, а затем снова включите репликацию.

## <a name="network"></a>Сеть

**Компонент** | **Поддерживается**
--- | ---
Объединение сетевых карт в сети узла | Поддерживается для виртуальных машин VMware. <br/><br/>Не поддерживается для репликации физического компьютера.
Виртуальная локальная сеть узла | Да.
IPv4-адрес сети узла | Да.
IPv6-адрес сети узла | Нет.
Объединение гостевых сетевых карт в сети гостевой системы или сервера | Нет.
IPv4-адрес cети клиента или сервера | Да.
IPv6-адрес cети клиента или сервера | Нет.
Статический IP-адрес (Windows) сети клиента или сервера | Да.
Статический IP-адрес (Linux) cети клиента или сервера | Да. <br/><br/>Виртуальные машины настроены для использования DHCP при восстановлении размещения.
Несколько сетевых адаптеров cети клиента или сервера | Да.


## <a name="azure-vm-network-after-failover"></a>Сеть виртуальной машины Azure (после отработки отказа)

**Компонент** | **Поддерживается**
--- | ---
Azure ExpressRoute | Да
Внутренний балансировщик нагрузки | Да
Внешний балансировщик нагрузки | Да
Диспетчер трафика Azure | Да
Несколько сетевых адаптеров | Да
Зарезервированный IP-адрес | Да
IPv4 | Да
Сохранение исходного IP-адреса | Да
Конечные точки служб для виртуальной сети Azure<br/> | Да
Ускорение работы в сети | нет

## <a name="storage"></a>Память
**Компонент** | **Поддерживается**
--- | ---
Динамический диск | Диск ОС должен быть базовым диском. <br/><br/>Диски данных могут быть динамическими дисками.
Конфигурация дисков Docker | нет
NFS узла | Да для VMware<br/><br/> Нет для физических серверов
SAN узла (ISCSI или FC) | Да
vSAN узла | Да для VMware<br/><br/> Недоступно для физических серверов.
Операции многопутевого ввода-вывода (MPIO) для узла | Да — протестировано с помощью Microsoft DSM, EMC PowerPath 5.7 SP4 и EMC PowerPath DSM для CLARiiON
Виртуальные тома узла (VVol) | Да для VMware<br/><br/> Недоступно для физических серверов.
VMDK клиента или сервера | Да
Общий диск кластера клиента или сервера | нет
Зашифрованный диск клиента или сервера | нет
NFS клиента или сервера | нет
ISCSI гостя или сервера | Для миграции — да<br/>Для аварийного восстановления — нет, iSCSI выполнит восстановление размещения в качестве подключенного диска для виртуальной машины.
SMB 3.0 клиента или сервера | нет
RDM клиента или сервера | Да<br/><br/> Недоступно для физических серверов.
Диск размером более 1 ТБ для клиента или сервера | Да, диск должен быть больше 1024 МБ<br/><br/>До 8 192 ГБ при репликации на управляемые диски (9,26 версии)<br></br> До 4 095 ГБ при репликации в учетные записи хранения
Диск клиента или сервера с физическими и логическими секторами с размером в 4 КБ | нет
Диск гостя или сервера с логическим размером 4 КБ и 512-байтами физический сектор | нет
Том с чередующимся диском размером более 4 ТБ для гостевой системы или сервера | Да
Управление логическими томами (LVM)| Толстая подготовка — да <br></br> Тонкая подготовка — нет
Дисковые пространства для гостя или сервера | нет
"Горячее" добавление и удаление дисков для гостя или сервера | нет
Исключение диска для гостя или сервера | Да
Поддержка операций многопутевого ввода-вывода (MPIO) для гостевой системы или сервера | нет
Разделы GPT для гостевых и серверных систем | Из [накопительного пакета обновления 37](https://support.microsoft.com/help/4508614/) (версия 9,25 службы Mobility Service) поддерживаются пять секций. Поддерживались четыре ранее.
ReFS | Отказоустойчивая файловая система поддерживается в службе Mobility Service версии 9,23 или более поздней
Загрузка из виртуальной машины/сервера EFI/UEFI | — Поддерживается для Windows Server 2012 или более поздней версии, SLES 12 SP4 и RHEL 8,0 с агентом Mobility Service версии 9,30 и выше<br/> -Безопасный тип загрузки UEFI не поддерживается.

## <a name="replication-channels"></a>Каналы репликации

|**Тип репликации**   |**Поддерживается**  |
|---------|---------|
|Разгрузка передачи данных (ODX)    |       нет  |
|Автономное заполнение        |   нет      |
| Azure Data Box | нет

## <a name="azure-storage"></a>Хранилище Azure

**Компонент** | **Поддерживается**
--- | ---
Локально избыточное хранилище | Да
Геоизбыточное хранилище | Да
Геоизбыточное хранилище с доступом для чтения | Да
"Холодное" хранилище | нет
"Горячее" хранилище| нет
Blob-блоки | нет
Шифрование неактивных (SSE)| Да
Шифрование неактивных (CMK)| Да (с помощью PowerShell AZ 3.3.0 Module)
Хранилище уровня "Премиум" | Да
Служба импорта и экспорта | нет
Брандмауэры службы хранилища Azure для виртуальных сетей | Да.<br/> Настроена для целевой учетной записи хранения или кэша (используется для хранения данных репликации).
Учетные записи хранения общего назначения версии 2 (горячий и холодно-уровни) | Да (затраты на транзакции значительно выше для версии 2 по сравнению с v1)

## <a name="azure-compute"></a>Служба вычислений Azure

**Компонент** | **Поддерживается**
--- | ---
Группы доступности | Да
Зоны доступности | нет
Концентратор | Да
Управляемые диски | Да

## <a name="azure-vm-requirements"></a>Требования для виртуальных машин Azure

Локальные виртуальные машины, реплицируемые в Azure, должны соответствовать требованиям к виртуальным машинам Azure, приведенным в этой таблице. Если Site Recovery выполняет проверку предварительных требований для репликации, проверка завершится ошибкой, если некоторые требования не будут выполнены.

**Компонент** | **Требования** | **Сведения**
--- | --- | ---
Операционная система на виртуальной машине | [Поддерживаемые операционные системы для реплицируемых виртуальных машин](#replicated-machines) | Если не поддерживается, проверка завершается ошибкой.
Архитектура операционной системы на виртуальной машине | 64-разрядная | Если не поддерживается, проверка завершается ошибкой.
Размер диска операционной системы | До 2048 ГБ | Если не поддерживается, проверка завершается ошибкой.
Число дисков операционной системы | 1 | Если не поддерживается, проверка завершается ошибкой.
Число дисков с данными | 64 ГБ и меньше | Если не поддерживается, проверка завершается ошибкой.
Размер диска данных | До 8 192 ГБ при репликации на управляемый диск (9,26 версии)<br></br>До 4 095 ГБ при репликации в учетную запись хранения| Если не поддерживается, проверка завершается ошибкой.
Сетевые адаптеры | Поддерживаются несколько адаптеров. |
Общий виртуальный жесткий диск | Не поддерживается. | Если не поддерживается, проверка завершается ошибкой.
Диск FC | Не поддерживается. | Если не поддерживается, проверка завершается ошибкой.
BitLocker | Не поддерживается. | Прежде чем включать репликацию для компьютера, необходимо отключить BitLocker. |
имя виртуальной машины; | От 1 до 63 символов.<br/><br/> при этом допустимы только буквы, цифры и дефисы<br/><br/> Имя компьютера должно начинаться и заканчиваться буквой или цифрой. |  Обновите значение в свойствах компьютера в службе Site Recovery.

## <a name="resource-group-limits"></a>Ограничения группы ресурсов

Сведения о количестве виртуальных машин, которые можно защитить в одной группе ресурсов, см. в статье [ограничения и квоты подписки](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits).

## <a name="churn-limits"></a>Ограничения на обновление

В таблице ниже приведены ограничения Azure Site Recovery.
- Эти ограничения основаны на наших тестах, но не охватывают все возможные сочетания операций ввода-вывода в приложении.
- Фактические результаты зависят от сочетания операций ввода-вывода приложения.
- Для получения наилучших результатов настоятельно рекомендуется запустить [средство планировщик развертывания](site-recovery-deployment-planner.md)и выполнить расширенное тестирование приложений с помощью тестовой отработки отказа, чтобы получить истинную картину производительности приложения.

**Целевой объект репликации** | **Средний размер ввода-вывода исходного диска** |**Средняя скорость обработки данных исходного диска** | **Общее число обработанных данных с диска источника в день**
---|---|---|---
Хранилище уровня "Стандартный" | 8 КБ    | 2 МБ/с | 168 ГБ на диск
Диск P10 или P15 класса Premium | 8 КБ    | 2 МБ/с | 168 ГБ на диск
Диск P10 или P15 класса Premium | 16 КБ | 4 МБ/с |    336 ГБ на диск
Диск P10 или P15 класса Premium | 32 КБ или выше | 8 МБ/с | 672 ГБ на диск
Диск P20, P30, P40 или P50 класса Premium | 8 КБ    | 5 МБ/с | 421 ГБ на диск
Диск P20, P30, P40 или P50 класса Premium | 16 КБ или выше |20 МБ/с | 1684 ГБ на диск


**Исходная скорость обработки данных** | **Максимальный предел**
---|---
Пиковая скорость обработки данных на всех дисках виртуальной машины | 54 МБ/с
Максимальный объем обработки данных, поддерживаемый сервером обработки | 2 ТБ

- В указанных средних значениях предполагается 30-процентное перекрытие операций ввода-вывода.
- В зависимости от коэффициента перекрытия, размера записи и фактической рабочей нагрузки ввода-вывода Site Recovery может обрабатывать более высокую пропускную способность.
- Эти числа предполагают типичную невыполненную работу примерно через пять минут. то есть после передачи выполняется обработка данных, а созданная точка восстановления составляет 5 минут.

## <a name="vault-tasks"></a>Задачи хранилища

**Действие** | **Поддерживается**
--- | ---
Перемещение хранилища между группами ресурсов | нет
Перемещение хранилища между подписками и между ними | нет
Перемещение хранилищ, сетей, виртуальных машин Azure между группами ресурсов | нет
Перемещение хранилища, сети, виртуальных машин Azure в рамках и между подписками. | нет


## <a name="obtain-latest-components"></a>Получение последних компонентов

**Имя** | **Описание** | **Сведения**
--- | --- | ---
Сервер конфигурации | Установлен локально.<br/> Координирует обмен данными между локальными серверами VMware или физическими компьютерами и Azure. | - [Сведения о](vmware-physical-azure-config-process-server-overview.md) сервере конфигурации.<br/> - [Сведения об](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) обновлении до последней версии.<br/> - [Сведения о](vmware-azure-deploy-configuration-server.md) настройке сервера конфигурации.
Сервер обработки | По умолчанию устанавливается на сервере конфигурации.<br/> Получает данные репликации, оптимизирует их с помощью кэширования, сжатия и шифрования и отправляет в Azure.<br/> По мере роста развертывания можно добавить дополнительные серверы обработки для обработки больших объемов трафика репликации. | - [Сведения о](vmware-physical-azure-config-process-server-overview.md) сервере обработки.<br/> - [Сведения об](vmware-azure-manage-process-server.md#upgrade-a-process-server) обновлении до последней версии.<br/> - [Дополнительные сведения о](vmware-physical-large-deployment.md#set-up-a-process-server) настройке серверов обработки масштабирования.
Служба Mobility Service | Устанавливается на виртуальных машинах VMware или физических серверах, которые необходимо реплицировать.<br/> Координирует репликацию между локальными серверами VMware или физическими серверами и Azure.| - [Сведения о](vmware-physical-mobility-service-overview.md) службе Mobility Service.<br/> - [Сведения об](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal) обновлении до последней версии.<br/>



## <a name="next-steps"></a>Следующие шаги
[Узнайте, как](tutorial-prepare-azure.md) подготовить Azure для аварийного восстановления виртуальных машин VMware.

[9.32 UR]: https://support.microsoft.com/en-in/help/4538187/update-rollup-44-for-azure-site-recovery
[9.31 UR]: https://support.microsoft.com/en-in/help/4537047/update-rollup-43-for-azure-site-recovery
[9.30 UR]: https://support.microsoft.com/en-in/help/4531426/update-rollup-42-for-azure-site-recovery
[9.29 UR]: https://support.microsoft.com/en-in/help/4528026/update-rollup-41-for-azure-site-recovery
[9.28 UR]: https://support.microsoft.com/en-in/help/4521530/update-rollup-40-for-azure-site-recovery
[9.27 UR]: https://support.microsoft.com/en-in/help/4517283/update-rollup-39-for-azure-site-recovery
[9.26 UR]: https://support.microsoft.com/en-in/help/4513507/update-rollup-38-for-azure-site-recovery
[9.25 UR]: https://support.microsoft.com/en-in/help/4508614/update-rollup-37-for-azure-site-recovery
[9.24 UR]: https://support.microsoft.com/en-in/help/4503156
[9.23 UR]: https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
