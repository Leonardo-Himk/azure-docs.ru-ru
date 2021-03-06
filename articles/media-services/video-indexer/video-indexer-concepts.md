---
title: Основные понятия, связанные с Индексатором видео
titleSuffix: Azure Media Services
description: В этой статье описываются некоторые концепции службы индексатора видео служб мультимедиа Azure.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 73dad1db4f44134f871c9f3d6e7edcdd3bd1e2ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74900671"
---
# <a name="video-indexer-concepts"></a>Основные понятия, связанные с Индексатором видео
 
В этой статье описаны некоторые основные понятия, связанные со службой "Индексатор видео".
    
## <a name="summarized-insights"></a>Сводные аналитические сведения

Сводные аналитические сведения включают агрегированное представление данных: лица, темы, эмоции. Например, вместо того, чтобы обрабатывать каждый из нескольких тысяч диапазонов времени и лица в нем, можно использовать сводные аналитические сведения. Они содержат данные обо всех лицах с указанием диапазонов времени, в течение которых отображается каждое лицо, и значений времени отображения в процентах.

## <a name="time-range-vs-adjusted-time-range"></a>Диапазон времени по сравнению со скорректированным диапазоном времени

TimeRange — диапазон времени в исходном видео. Параметр adjustedTimeRange — это диапазон времени, который относится к текущему списку воспроизведения. Можно создать список воспроизведения с помощью разных строк из разных видео, поэтому вы можете взять видео продолжительностью один час и использовать только одну строку из него, например в диапазоне времени 10:00–10:15. В этом случае вы получите список воспроизведения, содержащий 1 строку, где диапазон времени составляет 10:00–10:15, но adjustedTimeRange будет 00:00–00:15.
 
## <a name="blocks"></a>Блоки

Блоки предназначены для упрощения восприятия данных. Например, разделение на блоки происходит, когда меняется докладчик или возникает длинная пауза.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в руководстве по [регистрации и отправке видео](video-indexer-get-started.md).

## <a name="see-also"></a>См. также

[Общие сведения об Индексаторе видео](video-indexer-overview.md)
