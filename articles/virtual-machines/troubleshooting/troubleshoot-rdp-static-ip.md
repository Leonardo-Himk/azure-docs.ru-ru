---
title: Сбой подключения удаленного рабочего стола к Виртуальным машинам Azure из-за статического IP-адреса | Документация Майкрософт
description: Способы устранения сбоев подключения удаленного рабочего стола из-за статического IP-адреса в Microsoft Azure | Документация Майкрософт
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/08/2018
ms.author: genli
ms.openlocfilehash: 92ad33fbc759605ae901c3bcf09283c8e0b1c4b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77918195"
---
#  <a name="cannot-remote-desktop-to-azure-virtual-machines-because-of-static-ip"></a>Сбой подключения удаленного рабочего стола к Виртуальным машинам Azure из-за статического IP-адреса

В этой статье описывается проблема, при которой не удается установить подключение удаленного рабочего стола к виртуальным машинам Azure под управлением Windows после настройки на виртуальной машины статического IP-адреса.


## <a name="symptoms"></a>Симптомы

При подключении RDP к виртуальной машине в Azure может появиться сообщение об ошибке:

**Удаленному рабочему столу не удается подключиться к удаленному компьютеру по одной из следующих причин.**

1. **Удаленный доступ к серверу запрещен**.

2. **Удаленный компьютер выключен**.

3. **К удаленному компьютеру нет доступа в сети**.

**Убедитесь, что удаленный компьютер включен и подключен к сети, и удаленный доступ включен.**

При проверке снимка экрана в разделе [Диагностика загрузки](../troubleshooting/boot-diagnostics.md) на портале Azure вы увидите, что виртуальная машина загружается нормально и ожидает учетных данных на экране входа.

## <a name="cause"></a>Причина

У виртуальной машины есть статический IP-адрес, определенный для сетевого интерфейса в Windows. Этот IP-адрес отличается от адреса, который определен на портале Azure.

## <a name="solution"></a>Решение

Прежде чем выполнять какие-либо действия, сделайте моментальный снимок диска ОС затронутой виртуальной машины в качестве резервной копии. Дополнительные сведения см. в статье [Создание моментального снимка](../windows/snapshot-copy-managed-disk.md).

Для решения этой проблемы используйте последовательную консоль, чтобы включить DHCP, или [сбросьте сетевой интерфейс](reset-network-interface.md) виртуальной машины.

### <a name="use-serial-control"></a>Использование последовательной консоли

1. Подключитесь к [последовательной консоли и откройте экземпляр командной строки](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Если последовательная консоль на нужной виртуальной машине не включена, ознакомьтесь со сведениями о том, как [сбросить сетевой интерфейс](reset-network-interface.md).
2. Проверьте, отключен ли протокол DHCP в сетевом интерфейсе:

        netsh interface ip show config
3. Если протокол DHCP отключен, измените конфигурацию сетевого интерфейса так, чтобы использовался протокол DHCP:

        netsh interface ip set address name="<NIC Name>" source=dhc

    Например, если имя интерфейса взаимодействия — Ethernet 2, выполните команду ниже:

        netsh interface ip set address name="Ethernet 2" source=dhc

4. Запросите конфигурацию IP-адреса еще раз, чтобы убедиться, что сетевой интерфейс правильно настроен. Новый IP-адрес должен соответствовать предоставленному Azure.

        netsh interface ip show config

    На этом этапе не нужно перезапускать виртуальную машину. Она снова станет доступна.

После этого, если вы хотите настроить статический IP-адрес для виртуальной машины, ознакомьтесь с [этой статьей](../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md).