---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "67185317"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Установка исправлений режима обслуживания с помощью Windows PowerShell для StorSimple
> [!IMPORTANT]
> В режиме обслуживания необходимо сначала применить исправление на одном контроллере, а затем на другом.
> 
> 

1. Переведите устройство в режим обслуживания. Инструкции по переходу в режим обслуживания см. в разделе [Шаг 2. Вход в режим обслуживания](../articles/storsimple/storsimple-update-device.md#step2).
2. Чтобы применить исправление, введите:
   
     `Start-HcsHotfix` 
3. При появлении запроса укажите путь к общей сетевой папке, содержащей файлы исправления.
4. После этого введите подтверждение для применения этих исправлений. Введите **Y**, чтобы продолжить установку исправления.
5. После применения исправления на одном контроллере войдите на другой контроллер. Установите исправление, как для предыдущего контроллера.
6. После применения исправления выйдите из режима обслуживания. Инструкции см. в разделе [Шаг 4. Выход из режима обслуживания](../articles/storsimple/storsimple-update-device.md#step4).

