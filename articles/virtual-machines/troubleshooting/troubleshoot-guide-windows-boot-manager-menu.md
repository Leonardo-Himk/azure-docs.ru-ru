---
title: Не удается загрузить виртуальную машину Windows из-за диспетчера загрузки Windows
description: В этой статье приведены инструкции по устранению проблем, при которых диспетчер загрузки Windows предотвращает загрузку виртуальной машины Azure.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b3598
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 5d2fb62870e2c41af635627f5d692f08c67f8394
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80373353"
---
# <a name="windows-vm-cannot-boot-due-to-windows-boot-manager"></a>Не удается загрузить виртуальную машину Windows из-за диспетчера загрузки Windows

В этой статье приведены инструкции по устранению проблем, при которых диспетчер загрузки Windows предотвращает загрузку виртуальной машины Azure.

## <a name="symptom"></a>Симптом

Виртуальная машина зависает при ожидании запроса пользователя и не загружается, если не указать вручную.

При использовании [диагностики загрузки](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) для просмотра снимка экрана виртуальной машины вы увидите, что на снимке экрана отображается диспетчер загрузки Windows с сообщением *выберите операционную систему для запуска или нажмите клавишу TAB, чтобы выбрать инструмент:*.

На рисунке 1
 
![Диспетчер загрузки Windows, указывающий «выберите операционную систему для запуска» или нажмите клавишу TAB, чтобы выбрать средство:](media/troubleshoot-guide-windows-boot-manager-menu/1.jpg)

## <a name="cause"></a>Причина

Ошибка возникает из-за флага BCD *дисплайбутмену* в диспетчере загрузки Windows. Если флаг включен, диспетчер загрузки Windows запрашивает пользователя во время загрузки, чтобы выбрать загрузчик, который требуется запустить, что приведет к задержке загрузки. В Azure эта функция может добавлять время, необходимое для загрузки виртуальной машины.

## <a name="solution"></a>Решение

Обзор процесса:

1. Настройте для ускорения времени загрузки с помощью последовательной консоли.
2. Создайте виртуальную машину восстановления и получите к ней доступ.
3. Настройка для ускорения времени загрузки на виртуальной машине восстановления.
4. **Рекомендуется**. перед перестроением виртуальной машины включите сбор последовательной консоли и дампа памяти.
5. Перестройте виртуальную машину.

### <a name="configure-for-faster-boot-time-using-serial-console"></a>Настройка для ускорения времени загрузки с помощью последовательной консоли

Если у вас есть доступ к последовательной консоли, существует два способа достижения более быстрого времени загрузки. Сократите время ожидания *дисплайбутмену* или полностью удалите флаг.

1. Следуйте указаниям по доступу к [последовательной консоли Azure для Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows) , чтобы получить доступ к текстовой консоли.

   > [!NOTE]
   > Если вы не можете получить доступ к последовательной консоли, перейдите к [созданию виртуальной машины восстановления и доступа к ней](#create-and-access-a-repair-vm).

2. **Вариант а**. сокращение времени ожидания

   a. Время ожидания по умолчанию устанавливается в 30 секунд, но может быть изменено на более быстрое (например, 5 секунд).

   b. Чтобы настроить значение времени ожидания, используйте следующую команду в последовательной консоли:

      `bcdedit /set {bootmgr} timeout 5`

3. **Вариант б**. Удаление ФЛАГа BCD

   a. Чтобы не допустить полного вывода меню загрузки, введите следующую команду:

      `bcdedit /deletevalue {bootmgr} displaybootmenu`

      > [!NOTE]
      > Если вы не смогли использовать последовательную консоль для настройки более быстрого времени загрузки в описанных выше действиях, можно продолжить выполнение следующих действий. Чтобы устранить эту проблему, вы можете устранять неполадки в автономном режиме.

### <a name="create-and-access-a-repair-vm"></a>Создание виртуальной машины для восстановления и доступ к ней

1. Выполните [шаги 1-3 команды восстановления виртуальной машины](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) , чтобы подготовить виртуальную машину восстановления.
2. Используйте подключение к удаленному рабочему столу подключиться к виртуальной машине восстановления.

### <a name="configure-for-faster-boot-time-on-a-repair-vm"></a>Настройка для ускорения времени загрузки на виртуальной машине восстановления

1. Откройте командную строку с повышенными привилегиями.
2. Введите следующую команду, чтобы включить Дисплайбутмену:

   Используйте эту команду для **виртуальных машин поколения 1**:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} displaybootmenu yes`

   Используйте эту команду для **виртуальных машин поколения 2**:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} displaybootmenu yes`

   Замените символы "больше" или "меньше", а также текст внутри них, например "< текст здесь >".

3. Измените значение времени ожидания на 5 секунд:

   Используйте эту команду для **виртуальных машин поколения 1**:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} timeout 5`

   Используйте эту команду для **виртуальных машин поколения 2**:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} timeout 5`

   Замените символы "больше" или "меньше", а также текст внутри них, например "< текст здесь >".

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Рекомендуется: перед перестроением виртуальной машины включите сбор последовательной консоли и дампа памяти.

Чтобы включить сбор дампов памяти и последовательную консоль, выполните следующий скрипт:

1. Откройте сеанс командной строки с повышенными привилегиями (Запуск от имени администратора).
2. Выполните следующие команды:

   Включить последовательную консоль

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

   Замените символы "больше" или "меньше", а также текст внутри них, например "< текст здесь >".

3. Убедитесь, что объем свободного места на диске ОС совпадает с объемом памяти (ОЗУ) виртуальной машины.

   Если на диске операционной системы недостаточно места, следует изменить расположение файла дампа памяти и сослаться на любой диск данных, подключенный к виртуальной машине, имеющей достаточно свободного места. Чтобы изменить расположение, замените "% SystemRoot%" буквой диска (например, "F:") диска данных в приведенных ниже командах.

#### <a name="suggested-configuration-to-enable-os-dump"></a>Рекомендуемая конфигурация для включения дампа ОС

**Загрузка неработающего диска ОС**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**Включить в ControlSet001:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Включить в ControlSet002:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Выгрузка неработающего диска ОС:**

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Перестроение исходной виртуальной машины

Чтобы заново собрать виртуальную машину, используйте [Шаг 5 команд восстановления виртуальной машины](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) .