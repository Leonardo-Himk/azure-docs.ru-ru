---
title: Коды ошибок динамических событий служб мультимедиа Azure | Документация Майкрософт
description: В этой статье перечислены коды динамических событий.
author: IngridAtMicrosoft
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/4/2020
ms.author: inhenkel
ms.openlocfilehash: e039a240465478e86ce816890219e5dfaa07dfbf
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791057"
---
# <a name="media-services-live-event-error-codes"></a>Коды ошибок динамических событий служб мультимедиа

В таблицах этого раздела перечислены коды [динамических событий](live-events-outputs-concept.md) .

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected;

При оформлении подписки на события [сетки событий](https://docs.microsoft.com/azure/event-grid/) для события в реальном времени может появиться одна из следующих ошибок события [лививентконнектионрежектед](media-services-event-schemas.md#liveeventconnectionrejected) .

| Код результата | Описание |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | Неправильный URL-адрес приема. |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | IP-адрес кодировщика отсутствует в настроенном списке разрешенных IP-адресов. |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | Кодировщику RTMP не удалось отправить команду Сетдатафраме. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Указанный кодек не поддерживается. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED |Сведения о описании носителя не были получены до доставки фактических данных мультимедиа.|
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED |Число качеств для типа звука или видео превысило максимально допустимое значение.|
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED |Общая скорость входящей скорости в интерактивной службе событий или канала превысила максимально допустимый предел.|
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | Метка времени из кодировщика RTMP для FLVTag видео или аудио недействительна. |
| MPE_INGEST_FRAMERATE_EXCEEDED | Входящий кодировщик, принимающий потоки с фрамератес, превысил максимально допустимый 30fps для кодирования динамических событий и каналов.|
| MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED | Поток входящих потоков, принимающих входящие кодировщики, превысил следующие допустимые разрешения: 1920x1088 для кодирования динамических событий и каналов и 4096 x 2160 для сквозных динамических событий и каналов.|
| MPE_INGEST_RTMP_TOO_LARGE_UNPROCESSED_FLV | Интерактивное событие получило большой объем звуковых данных за один раз или большой объем видеоданных без ключевых кадров. Мы отключили кодировщик, чтобы дать ему возможность повторить попытку с правильными данными. |

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDisconnected.

Может появиться одна из следующих ошибок события [лививентенкодердисконнектед](media-services-event-schemas.md#liveeventencoderdisconnected) .

|Код результата|Описание|
|---|---|
|MPE_RTMP_SESSION_IDLE_TIMEOUT|Превышено время ожидания сеанса RTMP после простоя в течение допустимого ограничения времени.|
|MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID|Метка времени из кодировщика RTMP для FLVTag видео или аудио недействительна.|
|MPE_CAPACITY_LIMIT_REACHED|Кодировщик отправляет данные слишком быстро.|
|Неизвестные коды ошибок|Эти коды ошибок могут варьироваться от ошибки памяти до дублирования записей в карте хэшей.|


## <a name="see-also"></a>См. также раздел

[Коды ошибок конечной точки потоковой передачи (исходного кода)](streaming-endpoint-error-codes.md)

## <a name="next-steps"></a>Дальнейшие действия

[Руководство. потоковая передача в реальном времени с помощью служб мультимедиа](stream-live-tutorial-with-api.md)
