---
title: Примеры PowerShell для виртуальной машины Azure.
description: Примеры PowerShell для виртуальной машины Azure.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 8d7db5fe88890b7f807263e50757e637ad808eb1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759323"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Примеры PowerShell для виртуальной машины Azure

Ниже приведена таблица с ссылками на примеры сценариев PowerShell, которые позволяют создавать виртуальные машины Linux и управлять ими.

| | |
|---|---|
|**Создание виртуальных машин**||
| [Создание полностью настроенной виртуальной машины](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает группу ресурсов, виртуальную машину и все связанные ресурсы.|
| [Создание виртуальной машины с помощью Docker](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает виртуальную машину, настраивает ее в качестве узла Docker и запускает контейнер NGINX. |
| [Создание виртуальной машины с помощью NGINX](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает виртуальную машину и использует расширение пользовательских скриптов Azure для установки NGINX. |
| [Создание виртуальной машины с помощью WordPress](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает виртуальную машину и использует расширение пользовательских скриптов Azure для установки WordPress. |
| [Создание виртуальной машины с помощью существующего управляемого диска ОС с использованием CLI](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает виртуальную машину путем подключения имеющегося управляемого диска как диска ОС. |
| [Создание виртуальной машины из моментального снимка с помощью PowerShell](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает виртуальную машину из моментального снимка, сначала создав из него управляемый диск, а затем подключив этот диск как диск ОС. |
|**Управление хранилищем**||
| [Создание управляемого диска на основе VHD-файла в учетной записи хранения в той же или другой подписке](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает управляемый диск из специализированного VHD в качестве диска ОС или из VHD данных в качестве диска данных в той же или в другой подписке.  |
| [Создание управляемого диска на основе моментального снимка](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создание управляемого диска из моментального снимка. |
| [Экспорт моментального снимка в виде VHD в учетную запись хранения](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Экспортирует управляемый моментальный снимок в качестве VHD в учетную запись хранения в другом регионе. |
| [Экспорт VHD управляемого диска в учетную запись хранения](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Экспортирует базовый VHD управляемого диска в учетную запись хранения в другом регионе. |
| [Создание моментального снимка на основе VHD](../scripts/virtual-machines-linux-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает моментальный снимок на основе VHD и использует его для быстрого создания нескольких идентичных управляемых дисков.  |
| [Копирование снимка экрана в ту же или другую подписку](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Копирует снимок экрана в ту же или в другую подписку, расположенную в том же регионе, что и родительский снимок экрана. |
|**Мониторинг виртуальных машин**||
| [Мониторинг виртуальной машины с помощью журналов Azure Monitor](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает виртуальную машину, устанавливает агент Log Analytics и регистрирует виртуальную машину в рабочей области Log Analytics.  |
| [Копирование управляемого диска в ту же или другую подписку](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Копирует управляемый диск в ту же или другую подписку, расположенную в том же регионе, что и родительский управляемый диск.
| [Получение сведений обо всех виртуальных машинах в подписке с помощью PowerShell](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Создает CSV-файл, содержащий имя виртуальной машины, имя группы ресурсов, регион, виртуальную сеть, подсеть, частный IP-адрес, тип ОС и общедоступный IP-адрес виртуальных машин в указанной подписке.
| | |
