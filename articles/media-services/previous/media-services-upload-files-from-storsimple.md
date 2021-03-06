---
title: Отправка файлов в учетную запись служб мультимедиа Azure из Azure StorSimple | Документация Майкрософт
description: В этой статье рассматривается использование диспетчера данных Azure StorSimple. Она также содержит ссылки на руководства, в которых описывается, как извлечь данные из StorSimple и передать их в качестве ресурсов в учетную запись служб мультимедиа Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: c77b700cab4afd411c3a2df824ee8335cb394cda
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "64868311"
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a>Отправка файлов в учетную запись служб мультимедиа Azure из Azure StorSimple  

> [!NOTE]
> В Cлужбы мультимедиа версии 2 больше не добавляются новые компоненты или функциональные возможности. <br/>Ознакомьтесь с последней версией [служб мультимедиа v3](https://docs.microsoft.com/azure/media-services/latest/). См. также [руководство по миграции из v2 в версии 3](../latest/migrate-from-v2-to-v3.md) .
>
> 
> В настоящее время диспетчер данных Azure StorSimple доступен в качестве закрытой предварительной версии. 
> 

## <a name="overview"></a>Обзор

В службах мультимедиа цифровые файлы отправляются в актив. Ресурс может содержать видео, аудио, изображения, коллекции эскизов, текстовые дорожки и файлы скрытых субтитров (а также метаданные этих файлов). После передачи файлов содержимое будет безопасно сохранено в облаке для дальнейшей обработки и потоковой передачи.

[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) использует облачное хранилище в качестве расширения локального решения и автоматически связывает данные локального хранилища с данными облачного хранилища. Перед отправкой данных в облако устройство StorSimple дублирует и сжимает их. Таким образом, в облако можно эффективно отправлять большие файлы. Служба [диспетчера данных StorSimple](../../storsimple/storsimple-data-manager-overview.md) предоставляет интерфейсы API, позволяющие извлекать данные из StorSimple и представлять их в качестве ресурсов AMS.

## <a name="get-started"></a>Начало работы

1. [Создайте учетную запись служб мультимедиа](media-services-portal-create-account.md), в которую вы хотите перенести ресурсы.
2. Зарегистрируйтесь для получения предварительной версии диспетчера данных, как описано в статье [Общие сведения о диспетчере данных StorSimple (закрытая предварительная версия)](../../storsimple/storsimple-data-manager-overview.md).
3. Создайте учетную запись диспетчера данных StorSimple.
4. Создайте задание для преобразования данных, которое извлекает данные из устройства StorSimple, а затем переносит их в учетную запись AMS в качестве ресурсов. 

    При запуске задания создается очередь хранилища. Эта очередь по мере готовности заполняется сообщениями о преобразованных больших двоичных объектах. Имя этой очереди совпадает с именем определения задания. Вы можете использовать эту очередь для определения готовности ресурсов, а затем вызвать необходимую операцию служб мультимедиа для этих ресурсов. Например, вы можете использовать эту очередь, чтобы активировать функции Azure, в которых содержится нужный код служб мультимедиа.

## <a name="see-also"></a>См. также

[Использование пакета SDK для .NET для активации заданий в Диспетчер данных](../../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Предоставление отзыва
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Дальнейшие действия

Теперь можно закодировать отправленные ресурсы. Дополнительную информацию см. в статье, посвященной [кодированию ресурсов](media-services-portal-encode.md).
