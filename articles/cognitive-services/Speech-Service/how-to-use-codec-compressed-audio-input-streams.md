---
title: Потоковая передача сжатого аудио-кодека с помощью речевого пакета SDK — служба речи
titleSuffix: Azure Cognitive Services
description: Узнайте, как выполнять потоковую передачу сжатых аудио в службу речи с помощью речевого пакета SDK. Доступно для C++, C# и Java для Linux, Java в Android и цели-C в iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
zone_pivot_groups: programming-languages-set-twelve
ms.openlocfilehash: 13cb35ffa650661da2855787279c4bdc37126ac9
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585031"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Использование сжатых звуковых данных кодека с помощью пакета SDK для распознавания речи

**Сжатый потоковый вход** пакета SDK службы речевого ввода предоставляет способ потоковой передачи сжатого звука в службу распознавания речи с помощью `PullStream` или `PushStream` .

В настоящее время поддерживается потоковая передача сжатых аудио-данных в C#, C++, Java в Windows (приложения UWP не поддерживаются) и Linux (Ubuntu 16,04, Ubuntu 18,04, Debian 9, RHEL 7/8, CentOS 7/8). Она также поддерживается для Java в Android и цели-C на платформе iOS.
* Требуется пакет SDK для распознавания речи версии 1.10.0 или более поздней для RHEL 8 и CentOS 8
* Для Windows требуется пакет SDK для распознавания речи версии 1.11.0 или более поздней.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="prerequisites"></a>Предварительные требования

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Пример кода, использующий сжатые звуковые входные кодеки

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/examples.md)]
::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Узнайте, как распознать речь](quickstarts/speech-to-text-from-microphone.md)
