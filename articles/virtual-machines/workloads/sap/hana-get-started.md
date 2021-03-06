---
title: Установка SAP HANA на виртуальных машинах Azure | Документация Майкрософт "
description: Руководства по установке SAP HANA на виртуальных машинах Azure
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/19/2020
ms.author: juergent
ms.openlocfilehash: e017e082472e7a4a2fab6a2845e52d3dc7acc460
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80123347"
---
# <a name="installation-of-sap-hana-on-azure-virtual-machines"></a>Установка SAP HANA на виртуальных машинах Azure
## <a name="introduction"></a>Введение
Это руководством поможет вам указать нужные ресурсы для успешного развертывания HANA на виртуальных машинах Azure. В этом руководстве содержатся сведения о ресурсах, которые необходимо проверить перед установкой SAP HANA на виртуальной машине Azure. Итак, вы можете выполнить необходимые действия, чтобы закончиться с помощью поддерживаемой конфигурации SAP HANA на виртуальных машинах Azure.  

> [!NOTE]
> Это руководство описывает развертывание SAP HANA на виртуальных машинах Azure. Сведения о развертывании SAP HANA в крупных экземплярах HANA см. в [статье Установка и настройка SAP HANA (крупные экземпляры) в Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation).
 
## <a name="prerequisites"></a>Предварительные условия
Кроме того, в этом руководство предполагается, что вы знакомы с:
* SAP HANA и SAP NetWeaver и их локальная установка.
* Установка и работа SAP HANA и экземпляров приложений SAP в Azure.
* Основные понятия и процедуры, описанные в:
   * Планирование развертывания SAP в Azure, включающее Планирование виртуальных сетей Azure и использование хранилища Azure. См. раздел [SAP NetWeaver на виртуальных машинах Azure — руководством по планированию и внедрению](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
   * Принципы и способы развертывания виртуальных машин в Azure. См. раздел [Развертывание виртуальных машин Azure для SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide) .
   * Основные понятия высокого уровня доступности для SAP HANA, как описано в статье [SAP HANA высокий уровень доступности для виртуальных машин Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview) .

## <a name="step-by-step-before-deploying"></a>Пошаговое выполнение до развертывания
В этом разделе перечислены различные шаги, которые необходимо выполнить перед началом установки SAP HANA на виртуальной машине Azure. Порядок является перечислимым, и, как это необходимо, следует выполнить перечисленные ниже действия.

1. В Azure поддерживаются не все возможные сценарии развертывания. Таким образом, следует проверить документ [о рабочей нагрузке SAP в поддерживаемых виртуальных машинах Azure сценариях](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) для сценария, с которым вы задумались при развертывании SAP HANA. Если сценарий отсутствует в списке, необходимо предположить, что он не тестировался и, следовательно, не поддерживается.
2. Если у вас есть грубое представление о требованиях к памяти для развертывания SAP HANA, необходимо найти подгонку виртуальных машин Azure. Не все виртуальные машины, сертифицированные для SAP NetWeaver, задокументированы в [заметке о поддержке sap #1928533](https://launchpad.support.sap.com/#/notes/1928533), SAP HANA сертифицированы. Источником истинности SAP HANA сертифицированных виртуальных машин Azure является веб-сайт [SAP HANA Каталог оборудования](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Единицы, начинающиеся с **S** , являются единицами [крупных экземпляров Hana](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) , а не виртуальными машинами Azure.
3. Разные типы виртуальных машин Azure имеют разные минимальные выпуски операционных систем для SUSE Linux или Red Hat Linux. На [SAP HANA каталоге оборудования](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)веб-сайта необходимо щелкнуть запись в списке SAP HANA сертифицированные единицы, чтобы получить подробные данные об этой единице. Помимо поддерживаемой рабочей нагрузки HANA, выпуски ОС, поддерживаемые с этими единицами для SAP HANA, перечислены
4. По мере выпуска операционных систем необходимо учитывать некоторые минимальные выпуски ядра. Эти минимальные выпуски описаны в следующих заметках о поддержке SAP:
    - [Примечание о поддержке SAP #2814271 сбой резервного копирования SAP HANA в Azure с ошибкой контрольной суммы](https://launchpad.support.sap.com/#/notes/2814271)
    - [Примечание о поддержке SAP #2753418 потенциальное снижение производительности из-за резервного таймера](https://launchpad.support.sap.com/#/notes/2753418)
    - [Примечание о поддержке SAP #2791572 снижение производительности из-за отсутствия поддержки ВДСО для Hyper-V в Azure](https://launchpad.support.sap.com/#/notes/2791572)
4. В зависимости от выпуска ОС, который поддерживается для выбранного типа виртуальной машины, необходимо проверить, поддерживается ли необходимый выпуск SAP HANA в этой операционной системе. Прочитайте [Примечание о поддержке SAP #2235581](https://launchpad.support.sap.com/#/notes/2235581) для матрицы поддержки SAP HANA выпусков с различными выпусками операционной системы.
5. Как вы могли найти допустимую комбинацию типа виртуальной машины Azure, выпуска операционной системы и SAP HANA, необходимо проверить таблицу доступности продуктов SAP. В матрице доступности SAP можно узнать, поддерживается ли продукт SAP, который будет выполняться для базы данных SAP HANA.


## <a name="step-by-step-vm-deployment-and-guest-os-considerations"></a>Пошаговое развертывание виртуальной машины и требования к гостевой ОС
На этом этапе необходимо выполнить шаги по развертыванию виртуальных машин для установки HANA и постепенно оптимизировать выбранную операционную систему после установки.

1. Выбрать базовый образ из коллекции Azure. Если вы хотите создать собственный образ операционной системы для SAP HANA, необходимо знать все пакеты, необходимые для успешной установки SAP HANA. В противном случае рекомендуется использовать образы SUSE и Red Hat для SAP или SAP HANA из коллекции образов Azure. Эти образы содержат пакеты, необходимые для успешной установки HANA. В зависимости от контракта на поддержку с поставщиком операционной системы необходимо выбрать образ, в котором вы хотите использовать собственную лицензию. Или вы выбираете образ ОС, который включает поддержку
2. Если вы выбрали образ гостевой ОС, требующий использования собственной лицензии, необходимо зарегистрировать образ ОС в подписке, чтобы загрузить и применить последние исправления. Этот шаг требует общедоступного доступа к Интернету. Если вы не настроите частный экземпляр, например сервер SMT в Azure.
3. Определите конфигурацию сети виртуальной машины. Дополнительные сведения см. в документе [SAP HANA конфигурации инфраструктуры и операции в Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations). Помните, что отсутствуют квоты пропускной способности сети, которые можно назначить виртуальным сетевым картам в Azure. В результате, единственная цель направления трафика через разные vNIC основана на вопросах безопасности. Мы доверяю вам возможность найти компромисс между сложностью маршрутизации трафика через несколько vNIC и требованиями, которые применяются аспектами безопасности.
3. После развертывания и регистрации виртуальной машины примените последние исправления к операционной системе. Зарегистрировано в собственной подписке. Если вы выбрали образ, который включает поддержку операционной системы, у виртуальной машины уже должен быть доступ к исправлениям. 
4. Примените настройки, необходимые для SAP HANA. Эти настройки перечислены в следующих заметках о поддержке SAP:

    - [Примечание о поддержке SAP #2694118-Red Hat Enterprise Linux надстройки высокой доступности в Azure](https://launchpad.support.sap.com/#/notes/2694118)
    - [Примечание о поддержке SAP #1984787-SUSE LINUX Enterprise Server 12: замечания по установке](https://launchpad.support.sap.com/#/notes/1984787) 
    - [Примечание о поддержке SAP #2578899-SUSE Linux Enterprise Server 15: Примечание по установке](https://launchpad.support.sap.com/#/notes/2578899)
    - [Примечание о поддержке SAP #2002167-Red Hat Enterprise Linux 7. x: Установка и обновление](https://launchpad.support.sap.com/#/notes/0002002167)
    - [SAP Support Note #2292690 — SAP HANA DB: Recommended OS settings for RHEL 7](https://launchpad.support.sap.com/#/notes/0002292690) (Примечание по поддержке SAP №2292690. База данных SAP HANA: рекомендуемые параметры операционной системы для RHEL 7). 
    -  [Примечание о поддержке SAP #2772999-Red Hat Enterprise Linux 8. x: Установка и настройка](https://launchpad.support.sap.com/#/notes/2772999) 
    -  [Примечание о поддержке SAP #2777782-SAP HANA DB. Рекомендуемые параметры ОС для RHEL 8](https://launchpad.support.sap.com/#/notes/2777782)
    -  [Примечание о поддержке SAP #2455582-Linux: запуск приложений SAP, скомпилированных с помощью GCC 6. x](https://launchpad.support.sap.com/#/notes/0002455582)
    -  [Примечание о поддержке SAP #2382421 — оптимизация конфигурации сети на уровне HANA и ОС](https://launchpad.support.sap.com/#/notes/2382421)

1. Выберите тип хранилища Azure для SAP HANA. На этом шаге необходимо выбрать структуру хранилища для SAP HANA установки. Вы будете использовать подключенные диски Azure или собственные общие ресурсы Azure NFS. Типы хранилища Azure, которые можно использовать, и комбинации различных типов хранилища Azure, которые могут использоваться, описаны в [SAP HANA конфигурациях хранилища виртуальных машин Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage). Задавайте конфигурации, задокументированные в качестве отправной точки. Для непроизводственных систем можно настроить более низкую пропускную способность или операции ввода-вывода. В производственных целях может потребоваться дополнительная настройка пропускной способности и операций ввода-вывода в секунду.
2. Убедитесь, что вы настроили [ускоритель записи Azure](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) для томов, которые содержат журналы транзакций СУБД, или журналы повторяемых операций при использовании виртуальных машин серии M или Mv2. Помните об ограничениях Ускоритель записи, как описано в статье.
2. Проверьте, включена ли поддержка [ускорения в Azure](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) для виртуальных машин, которые развернуты.

> [!NOTE]
> Не все команды в различных профилях SAP-настройки или, как описано в примечаниях, могут успешно выполняться в Azure. Команды, управляющие режимом питания виртуальных машин, обычно возвращают ошибку, так как режим питания базового оборудования узла Azure не может управляться.

## <a name="step-by-step-preparations-specific-to-azure-virtual-machines"></a>Пошаговые подготовительные действия, относящиеся к виртуальным машинам Azure
Одна из особенностей Azure — это установка расширения виртуальной машины Azure, которая предоставляет данные мониторинга для агента узла SAP. Сведения об установке этого расширения мониторинга описаны в следующих руководствах:

-  [Примечание SAP 2191498](https://launchpad.support.sap.com/#/notes/2191498/E) обсуждает Улучшенное наблюдение SAP с виртуальными машинами Linux в Azure. 
-  [Примечание SAP 1102124](https://launchpad.support.sap.com/#/notes/1102124/E) обсуждаются сведения о SAPOSCOL в Linux 
-  [Примечание SAP 2178632](https://launchpad.support.sap.com/#/notes/2178632/E) обсуждаются ключевые показатели мониторинга для SAP на Microsoft Azure
-  [Развертывание виртуальных машин Azure для SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide#d98edcd3-f2a1-49f7-b26a-07448ceb60ca)

## <a name="sap-hana-installation"></a>Установка SAP HANA
После развертывания виртуальных машин Azure и зарегистрированных и настроенных операционных систем можно установить SAP HANA в соответствии с установкой SAP. Чтобы перейти к этой документации, начните с ресурсов SAP Web [Hana](https://www.sap.com/products/hana/implementation/resources.html)

Для SAP HANA конфигураций горизонтального масштабирования с использованием непосредственно подключенных дисков хранилища Azure класса Premium или Ultra Disk ознакомьтесь с конкретными сведениями в документе [SAP HANA конфигурациях и операциях инфраструктуры в Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations#configuring-azure-infrastructure-for-sap-hana-scale-out) .


## <a name="additional-resources-for-sap-hana-backup"></a>Дополнительные ресурсы для резервного копирования SAP HANA
Сведения о резервном копировании баз данных SAP HANA на виртуальных машинах Azure см. в следующих статьях:
* [Руководство по резервному копированию SAP HANA на виртуальных машинах Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [Резервное копирование SAP HANA в Azure на уровне файлов](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)

## <a name="next-steps"></a>Дальнейшие шаги
Ознакомьтесь с документацией:

- [Конфигурации инфраструктуры SAP HANA и работа с ней в Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Конфигурации хранилища виртуальных машин SAP HANA в Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)





