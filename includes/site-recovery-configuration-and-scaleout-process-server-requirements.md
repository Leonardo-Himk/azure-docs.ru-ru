---
title: Включить имя файла
description: включить файл
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 1aaec104e9130eeef723c6505e04e3317271566b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80234250"
---
**Требования к конфигурации и серверу обработки**


## <a name="hardware-requirements"></a>Требования к оборудованию

**Компонент** | **Требование** 
--- | ---
Ядра ЦП | 8 
ОЗУ | 16 ГБ
Количество дисков | 3 (диск ОС, диск кэша сервера обработки, диск хранения (для восстановления размещения)) 
Свободное место на диске (кэш сервера обработки) | 600 ГБ
Свободное место на диске (диск хранения) | 600 ГБ
 | 

## <a name="software-requirements"></a>Требования к программному обеспечению

**Компонент** | **Требование** 
--- | ---
Операционная система | Windows Server 2012 R2 <br> Windows Server 2016
Язык операционной системы | Английский (EN-*)
Роли Windows Server | Не включайте эти роли: <br> — доменные службы Active Directory; <br>— службы IIS; <br> — Hyper-V. 
Групповые политики | Не включать эти групповые политики: <br> — запрет на использование командной строки; <br> — запрет на использование инструментов редактирования реестра; <br> — логика доверия для вложенных файлов; <br> — включение выполнения скриптов. <br> [Дополнительные сведения](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | — Должен отсутствовать предварительно созданный веб-сайт по умолчанию. <br> — Должен отсутствовать предварительно созданный веб-сайт или приложение, ожидающее передачи данных на порте 443. <br>— включите [анонимную аутентификацию](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx); <br> — включите параметр [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx); 
FIPS (федеральные стандарты обработки информации) | Не включать режим FIPS
|

## <a name="network-requirements"></a>Требования к сети

**Компонент** | **Требование** 
--- | --- 
Тип IP-адреса | Статические 
Порты | 443 (оркестрация канала управления)<br>9443 (передача данных) 
Тип сетевой карты | VMXNET3 (если сервер конфигурации является виртуальной машиной VMware)
 |
**Доступ к Интернету** (серверу требуется доступ к следующим URL-адресам, напрямую или через прокси-сервер):|
\*.backup.windowsazure.com | Используется для передачи и координации реплицированных данных
\*.store.core.windows.net | Используется для передачи и координации реплицированных данных
\*.blob.core.windows.net | Используется для получения доступа к учетной записи хранения, в которой хранятся реплицируемые данные
\*.hypervrecoverymanager.windowsazure.com | Используется для операций управления репликацией и координации
https:\//management.azure.com | Используется для операций управления репликацией и координации 
*.services.visualstudio.com | Используется в целях телеметрии (необязательно)
time.nist.gov | Используется для проверки синхронизации времени между системой и глобальным временем
time.windows.com | Используется для проверки синхронизации времени между системой и глобальным временем
| <ul> <li> https:\//login.microsoftonline.com </li><li> https:\//secure.aadcdn.microsoftonline-p.com </li><li> HTTPS:\//Login.Live.com </li><li> HTTPS:\//Graph.Windows.NET </li><li> https:\//login.windows.net </li><li> HTTPS:\//www.Live.com </li><li> HTTPS:\//www.Microsoft.com </li></ul> | Программе установки OVF требуется доступ к этим URL-адресам. Они используются для управления доступом и управления удостоверениями с Azure Active Directory.
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi  | Для завершения загрузки MySQL. </br> В нескольких регионах загрузка может быть перенаправлена на URL-адрес CDN. При необходимости убедитесь, что URL-адрес CDN также список разрешений.
|

## <a name="required-software"></a>Необходимое программное обеспечение

**Компонент** | **Требование** 
--- | ---
VMware vSphere PowerCLI | Если сервер конфигурации работает на виртуальной машине VMware, должен быть установлен компонент [PowerCLI версии 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).
MySQL | Должен быть установлен компонент MySQL. Его можно установить вручную, или его может установить Site Recovery. (Дополнительные сведения см. в разделе, посвященном [настройке параметров](../articles/site-recovery/vmware-azure-deploy-configuration-server.md#configure-settings)).
|

## <a name="sizing-and-capacity-requirements"></a>Требования к размеру и емкости

В следующей таблице приведены требования к емкости для сервера конфигурации. При репликации нескольких виртуальных машин VMware ознакомьтесь с [вопросами планирования емкости](../articles/site-recovery/site-recovery-plan-capacity-vmware.md) и запустите [средство планировщик развертывания Azure Site Recovery](../articles/site-recovery/site-recovery-deployment-planner.md).


**ЗАГРУЗКИ** | **Память** | **Диск кэша** | **Скорость изменения данных** | **Реплицируемые компьютеры**
--- | --- | --- | --- | ---
8 виртуальных ЦП<br/><br/> 2 сокета по 4 ядра с частотой \@ 2,5 ГГц | 16 ГБ | 300 ГБ | 500 ГБ или менее | < 100 компьютеров
12 виртуальных ЦП<br/><br/> 2 сокета по 6 ядер с частотой \@ 2,5 ГГц | 18 ГБ | 600 ГБ | 500 ГБ — 1 ТБ | От 100 до 150 компьютеров
16 виртуальных ЦП<br/><br/> 2 сокета по 8 ядер с частотой \@ 2,5 ГГц | 32 ГБ | 1 TБ | 1-2 TБ | От 150 до 200 компьютеров
|

