---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8cc5dbb907c342b766cebe6da36cf580ddac5e2c
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "67185319"
---
#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Установка обычных исправлений с помощью Windows PowerShell для StorSimple
1. Подключитесь к последовательной консоли устройства. Дополнительные сведения см. в разделе [Шаг 1. Подключение к последовательной консоли](../articles/storsimple/storsimple-update-device.md#step1).
2. В меню последовательной консоли выберите параметр 1 **Войти с полным доступом**. Задайте пароль. Пароль по умолчанию — **Password1**.
3. В командной строке введите:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Эта команда применяется только для обычных исправлений. Эта команда выполняется только на одном контроллере, но обновляться будут оба контроллера.
    > В процессе обновления может выполняться отработка отказа контроллера, но это не повлияет на доступность или работу системы.

4. При появлении запроса укажите путь к общей сетевой папке, содержащей файлы исправления.
5. После этого введите подтверждение для применения этих исправлений. Введите **Y**, чтобы продолжить установку исправления.

