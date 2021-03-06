---
title: Документация по пакету SDK для речевых устройств
titleSuffix: Azure Cognitive Services
description: Заметки о выпуске содержат журналы обновлений, улучшения, исправления ошибок и изменения в пакете SDK для речевых устройств. Эта статья обновлена в каждом выпуске пакета SDK для речевых устройств.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: wellsi
ms.openlocfilehash: 93bedbf4275dacc3ec84e6df9c55e7e9a7b9d632
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780918"
---
# <a name="release-notes-speech-devices-sdk"></a>Заметки о выпуске: пакет SDK для речевых устройств

В следующих разделах перечислены изменения в последних выпусках.

## <a name="speech-devices-sdk-1110"></a>1.11.0 пакета SDK для речевых устройств:

- Поддержка [произвольных геометрических массивов микрофона](how-to-devices-microphone-array-configuration.md) и задание рабочего угла с помощью [файла конфигурации](https://aka.ms/sdsdk-micarray-json).
- Поддержка [УРБЕТТЕР DDK](http://www.urbetter.com/products_56/278.html).
- Освобожденные двоичные файлы для [динамика гжек](https://aka.ms/sdsdk-download-speaker) , используемого в нашем [примере голосового помощника](https://aka.ms/sdsdk-speaker).
- Выпущены двоичные файлы для [Linux ARM32](https://aka.ms/sdsdk-download-linux-arm32) и [Linux ARM 64](https://aka.ms/sdsdk-download-linux-arm64) для Raspberry Pi и аналогичных устройств.
- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.11.0. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-190"></a>1.9.0 пакета SDK для речевых устройств:

- Предоставляются исходные двоичные файлы для [DDK урбеттер](https://aka.ms/sdsdk-download-urbetter) (Linux ARM64).
- Теперь в Рубо v1 используется Maven для речевого пакета SDK
- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.9.0. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-170"></a>1.7.0 пакета SDK для речевых устройств:

- Поддержка ARM для Linux теперь поддерживается.
- Предоставляются исходные двоичные файлы для [Рубо v2 DDK](https://aka.ms/sdsdk-download-roobov2) (Linux ARM64).
- Пользователи Windows могут использовать `AudioConfig.fromDefaultMicrophoneInput()` или `AudioConfig.fromMicrophoneInput(deviceName)` для указания микрофона, который будет использоваться.
- Размер библиотеки оптимизирован.
- Поддержка многофакторного распознавания с помощью одного объекта распознавателя речи и намерения.
- Устранение случайного зависания, которое может произойти при прекращении распознавания.
- Примеры приложений теперь содержат пример файла участников. Properties для демонстрации формата файла.
- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.7.0. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-160"></a>1.6.0 пакета SDK для речевых устройств:

- Поддержка [Azure KINECT DK](https://azure.microsoft.com/services/kinect-dk/) в Windows и Linux с общим [примером приложения](https://aka.ms/sdsdk-download)
- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.6.0. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-151"></a>1.5.1 пакета SDK для речевых устройств:

- Включить [транскрипцию беседы](conversation-transcription-service.md) в примере приложения.
- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.5.1. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-150-2019-may-release"></a>Пакет SDK для речевых устройств 1.5.0:2019 — может выпустить

- Пакет SDK для речевых устройств теперь является общедоступным и больше не является предварительным.
- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.5.0. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).
- Новая ключевая технология обеспечивает значительные улучшения качества, см. раздел критические изменения.
- Новый конвейер обработки звука для улучшенного распознавания полей.

**Критические изменения**

- В связи с новым ключевым словом технология на улучшенном портале ключевых слов необходимо создать повторно все ключевые слова. Чтобы полностью удалить старые ключевые слова с устройства, удалите старое приложение.
  - ADB удаление com. Microsoft. cognitiveservices. Speech. Samples. сдсдкстартерапп

## <a name="speech-devices-sdk-140-2019-apr-release"></a>Пакет SDK для речевых устройств 1.4.0:2019-Апр

- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.4.0. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-131-2019-mar-release"></a>Пакет SDK для речевых устройств 1.3.1:2019-Мар

- Обновлен компонент [речевого пакета SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.3.1. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).
- Обновлена обработка ключевых слов, см. раздел критические изменения.
- Пример приложения добавляет выбор языка для распознавания речи и перевода.

**Критические изменения**

- [Установка ключевого слова](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) упрощена, теперь она является частью приложения и не требует отдельной установки на устройстве.
- Распознавание ключевых слов изменилось, и поддерживаются два события.
  - `RecognizingKeyword,`Указывает, что в речевом результате содержится текст ключевого слова (не проверено).
  - `RecognizedKeyword`Указывает, что распознавание ключевого слова завершено для распознавания данного ключевого слова.

## <a name="speech-devices-sdk-110-2018-nov-release"></a>Пакет SDK для речевых устройств 1.1.0:2018-Ноя

- Обновленный компонент [пакета SDK для распознавания речи](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.1.0. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).
- Повышена точность распознавания речи из дальней зоны за счет улучшенного алгоритма обработки аудио.
- В пример приложения добавлена поддержка распознавания речи на китайском языке.

## <a name="speech-devices-sdk-101-2018-oct-release"></a>Пакет SDK для речевых устройств 1.0.1:2018 — выпуск Oct

- Обновленный компонент [пакета SDK для распознавания речи](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 1.0.1. Дополнительные сведения см. в [заметках о выпуске](https://aka.ms/csspeech/whatsnew).
- Повышение точности распознавания речи за счет улучшенного алгоритма обработки аудио
- Исправлена одна ошибка для звукового сеанса непрерывного распознавания.

**Критические изменения**

- В этом выпуске представлен ряд критических изменений. Дополнительные сведения о соответствующих API приведены на [этой странице](https://aka.ms/csspeech/breakingchanges_1_0_0).
- Файлы модели KWS несовместимы с пакетом SDK для речевых устройств версии 1.0.1. Существующие файлы ключевых слов будут удалены после того, как на устройство будет записано новое ключевое слово.

## <a name="speech-devices-sdk-050-2018-aug-release"></a>Пакет SDK для речевых устройств 0.5.0:2018-Авг

- Повышенная точность распознавания речи благодаря исправлению ошибки в коде обработки звука.
- Обновленный компонент [пакета SDK для распознавания речи](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) до версии 0.5.0. Дополнительные сведения см. в [заметках о выпуске](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="speech-devices-sdk-0212733-2018-may-release"></a>Пакет SDK для речевых устройств 0.2.12733:2018 — может выпустить

Первый выпуск общедоступной предварительной версии пакета SDK для речевых устройств в Cognitive Services.
