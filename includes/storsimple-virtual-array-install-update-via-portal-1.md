---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: b4c3fcb86fb098263840accc561785a40b767952
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "67185475"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Установка обновлений с помощью портала Azure

1. Откройте диспетчер устройств StorSimple и выберите **Устройства**. Из списка устройств, подключенных к службе, выберите то, которое требуется обновить.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate1m.png) 

2. В колонке **Параметры** щелкните **Обновления устройства**.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate2m.png)  

3. При наличии доступных обновлений вы увидите сообщение об этом. Чтобы проверить наличие обновлений, можно щелкнуть элемент **Проверить**. Запишите версию программного обеспечения, которое выполняется в вашей системе. 

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate3m1.png)

    Вы получите уведомление о начале и успешном завершении проверки.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate5m.png)

4. После проверки наличия обновлений щелкните **Скачать обновления**.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate6m.png)

5. В колонке **новых обновлений** просмотрите заметки о выпуске. Обратите внимание, что после скачивания обновлений необходимо подтвердить установку. Нажмите кнопку **ОК**.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate7m.png)

6. Вы получите уведомления о начале и успешном завершении загрузки.

     ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate8m.png)

5. В колонке **Обновления устройства** щелкните **Установить**.

     ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate11m1.png)

6. В колонке **Свежие обновления** вы увидите предупреждение о том, что обновление нарушит работу. Так как виртуальный массив является устройством с одним узлом, после обновления он будет перезагружен. Все текущие операции ввода-вывода будут прерваны. Щелкните **ОК**, чтобы установить обновления.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate12m.png)

7. Вы получите уведомление о запуске задания установки.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate13m.png)

8.  Когда задание установки успешно завершится, выберите ссылку **Просмотреть задание** в колонке **Обновление устройства**, чтобы проверить выполнение установки. 

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate15m1.png)

    Вы перейдете к колонке **Install Updates** (Установка обновлений). Здесь вы увидите подробные сведения о выполненном задании.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate16m1.png)

9. Если в начале у вас был виртуальный массив с установленным обновлением 0.6 (10.0.10293.0) для программного обеспечения, сейчас у вас установлено обновление 1. Оставшиеся действия можно пропустить. Если же у вас был виртуальный массив с более ранней версией программного обеспечения, чем 0.6 (10.0.10293.0), то теперь у вас установлена версия 0.6. Появится еще одно сообщение о том, что обновления доступны. Повторите шаги 4–8, чтобы установить обновление 1.

    ![обновление устройства](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate17.png)

