---
title: Установка обновлений на виртуальный массив StorSimple | Документация Майкрософт
description: В этой статье объясняется, как установить обновления на виртуальный массив StorSimple с помощью локального пользовательского веб-интерфейса и портала Azure.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: b67fcb82bdcc94d7faeceedb7420a869e6578cad
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "61436466"
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a>Установка обновления 0.4 на виртуальный массив StorSimple

## <a name="overview"></a>Обзор

В этой статье описываются шаги, необходимые для установки обновления версии 0.4 на виртуальный массив StorSimple с помощью локального пользовательского веб-интерфейса и портала Azure. Применение обновлений или исправлений программного обеспечения необходима, чтобы виртуальный массив StorSimple был актуальным. 

Помните, что при установке обновления или исправления происходит перезагрузка устройства. Если виртуальный массив StorSimple — это устройство с одним узлом, то любые выполняемые операции ввода-вывода будут прерваны, а само устройство будет простаивать в течение некоторого времени. 

Перед применением обновления рекомендуется перевести в автономный режим тома или общие папки на узле, а затем перевести в автономный режим само устройство. Это уменьшает возможность повреждения данных.

> [!IMPORTANT]
> Если у вас установлено обновление 0.1 или общедоступные версии программного обеспечения, то необходимо воспользоваться методом исправления через локальный пользовательский веб-интерфейс и установить обновление 0.3. Если у вас уже есть обновление 0.2, рекомендуется устанавливать обновления с помощью портала Azure.
 

## <a name="use-the-local-web-ui"></a>Использование локального веб-интерфейса

При использовании локального веб-интерфейса установка обновлений включает два этапа:

* Загрузка обновления или исправления
* Установка обновления или исправления

### <a name="download-the-update-or-the-hotfix"></a>Загрузка обновления или исправления

Для загрузки обновления программного обеспечения из каталога обновления Майкрософт выполните следующие действия.

#### <a name="to-download-the-update-or-the-hotfix"></a>Загрузка обновления или исправления

1. Запустите Internet Explorer и перейдите по [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com)адресу.

2. Если вы впервые используете каталог Центра обновления Майкрософт на этом компьютере, нажмите кнопку **Установить** , когда будет предложено установить надстройку каталога Центра обновления Майкрософт.

3. В поле поиска каталога Центра обновления Майкрософт введите номер необходимого исправления в базе знаний, которое нужно скачать. Введите **3216577** для обновления версии 0.4 и нажмите кнопку **Поиск**.
   
    Появится список исправлений, например **Обновление 0.4 для виртуального массива StorSimple**.
   
    ![Поиск в каталоге](./media/storsimple-virtual-array-install-update-04/download1.png)

4. Нажмите кнопку **Добавить**. Обновление будет добавлено в корзину.

5. Щелкните **Просмотреть корзину**.

6. Нажмите **Загрузить**. Укажите или **выберите** локальное расположение, в которое хотите скачать файлы. Обновления скачиваются в указанное расположение и помещаются во вложенную папку с тем же именем, что и имя обновления. Этот каталог также можно скопировать в сетевую папку, которая доступна с вашего устройства.

7. Откройте папку, в которую выполнено копирование. Вы увидите файл изолированного пакета Центра обновления Microsoft `WindowsTH-KB3011067-x64`. Этот файл используется для установки обновления или исправления.

### <a name="install-the-update-or-the-hotfix"></a>Установка обновления или исправления

Перед установкой обновления или исправления убедитесь, что обновление или исправление загружено локально на вашем узле или доступно в сетевой папке. 

Воспользуйтесь этим методом для установки обновлений на устройствах, на которых установлены общедоступные версии программного обеспечения или обновление 0.1. Эта процедура занимает менее двух минут. Для установки обновления или исправления выполните следующие действия.

#### <a name="to-install-the-update-or-the-hotfix"></a>Установка обновления или исправления

1. В локальном веб-интерфейсе перейдите в раздел **обслуживание** > **Обновление программного обеспечения**.
   
    ![обновление устройства](./media/storsimple-virtual-array-install-update/update1m.png)

2. В поле **Путь к файлу обновления**введите имя файла обновления или исправления. Также вы можете найти файл установки обновления или исправления, если он расположен в сетевой папке. Щелкните **Применить**.
   
    ![обновление устройства](./media/storsimple-virtual-array-install-update/update2m.png)

3. Отобразится предупреждение. Если это устройство с одним узлом, то после применения обновления оно перезагружается, а также возникает время простоя. Щелкните значок галочки.
   
   ![обновление устройства](./media/storsimple-virtual-array-install-update/update3m.png)

4. Начинается обновление. После успешного обновления устройство перезапускается. В течение этого времени локальный пользовательский интерфейс недоступен.
   
    ![обновление устройства](./media/storsimple-virtual-array-install-update/update5m.png)

5. После перезапуска открывается страница **входа** . Чтобы убедиться, что программное обеспечение устройства Обновлено, в локальном веб-интерфейсе перейдите в раздел **обслуживание** > **Обновление программного обеспечения**. Для обновления версии 0.4 должна отображаться версия программного обеспечения **10.0.0.0.0.10289.0**.
   
   > [!NOTE]
   > Версии программного обеспечения в локальном веб-интерфейсе и на портале Azure отображаются немного по-разному. Например, для одной и той же версии в локальном пользовательском веб-интерфейсе отображается номер **10.0.0.0.0.10289**, а на портале Azure — **10.0.10289.0**.
   
    ![обновление устройства](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a>Использование портала Azure

Если у вас запущено обновление версии 0.2 и выше, рекомендуется устанавливать обновления с помощью портала Azure. При использовании портала пользователю нужно проверить наличие обновлений, а затем скачать и установить обновления. Эта процедура занимает около 7 минут. Для установки обновления или исправления выполните следующие действия.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

После окончания установки (об этом свидетельствует состояние задания "100 %") перейдите к службе диспетчера устройств StorSimple. Перейдите к **устройствам** и в списке устройств, подключенных к службе, выберите то, которое требуется обновить. В колонке **Параметры** щелкните **Управление** и выберите **Обновления устройства**. Отображаемая версия программного обеспечения должна быть следующей: **10.0.10289.0**.


## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в статье [Использование пользовательского веб-интерфейса для администрирования виртуального массива StorSimple](storsimple-ova-web-ui-admin.md).

