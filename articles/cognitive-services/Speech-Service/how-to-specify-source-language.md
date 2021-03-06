---
title: Указание языка исходного текста для перевода речи в текст
titleSuffix: Azure Cognitive Services
description: Пакет SDK для распознавания речи позволяет указать исходный язык при преобразовании речи в текст. В этой статье описывается, как использовать методы Фромконфиг и Саурцелангуажеконфиг, чтобы служба распознавания речи знала исходный язык и предоставил настраиваемый целевой объект модели.
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 32c08af129172fb1dbebf1679ea01694e8bd3d1a
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653276"
---
# <a name="specify-source-language-for-speech-to-text"></a>Укажите исходный язык для перевода речи в текст

В этой статье вы узнаете, как указать исходный язык для входных данных, передаваемых в речевой пакет SDK для распознавания речи. Кроме того, приведен пример кода для указания пользовательской модели речи для улучшения распознавания.

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>Как указать исходный язык на языке C #

В этом примере исходный язык предоставляется явно в качестве параметра с помощью `SpeechRecognizer` конструкции.

```csharp
var recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

В этом примере исходный язык предоставляется с помощью `SourceLanguageConfig` . Затем объект `sourceLanguageConfig` передается в качестве параметра для `SpeechRecognizer` создания.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

В этом примере исходный язык и Пользовательская конечная точка предоставляются с помощью `SourceLanguageConfig` . Затем объект `sourceLanguageConfig` передается в качестве параметра для `SpeechRecognizer` создания.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE", "The Endpoint ID for your custom model.");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SpeechRecognitionLanguage`и `EndpointId` методы Set являются устаревшими из `SpeechConfig` класса в C#. Использовать эти методы не рекомендуется, и их не следует использовать при создании `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>Как указать исходный язык в C++

В этом примере исходный язык явно указывается в качестве параметра с помощью `FromConfig` метода.

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

В этом примере исходный язык предоставляется с помощью `SourceLanguageConfig` . Затем объект `sourceLanguageConfig` передается в качестве параметра `FromConfig` при создании `recognizer` .

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

В этом примере исходный язык и Пользовательская конечная точка предоставляются с помощью `SourceLanguageConfig` . `sourceLanguageConfig`Передается в качестве параметра `FromConfig` при создании `recognizer` .

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SetSpeechRecognitionLanguage`и `SetEndpointId` являются устаревшими методами из `SpeechConfig` класса в C++ и Java. Использовать эти методы не рекомендуется, и их не следует использовать при создании `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>Как указать исходный язык в Java

В этом примере исходный язык предоставляется явно при создании нового `SpeechRecognizer` .

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

В этом примере исходный язык предоставляется с помощью `SourceLanguageConfig` . Затем объект `sourceLanguageConfig` передается в качестве параметра при создании нового `SpeechRecognizer` .

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

В этом примере исходный язык и Пользовательская конечная точка предоставляются с помощью `SourceLanguageConfig` . Затем объект `sourceLanguageConfig` передается в качестве параметра при создании нового `SpeechRecognizer` .

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `setSpeechRecognitionLanguage`и `setEndpointId` являются устаревшими методами из `SpeechConfig` класса в C++ и Java. Использовать эти методы не рекомендуется, и их не следует использовать при создании `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>Как указать исходный язык в Python

В этом примере исходный язык предоставляется явно в качестве параметра с помощью `SpeechRecognizer` конструкции.

```Python
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, language="de-DE", audio_config=audio_config)
```

В этом примере исходный язык предоставляется с помощью `SourceLanguageConfig` . Затем объект `SourceLanguageConfig` передается в качестве параметра для `SpeechRecognizer` создания.

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

В этом примере исходный язык и Пользовательская конечная точка предоставляются с помощью `SourceLanguageConfig` . Затем объект `SourceLanguageConfig` передается в качестве параметра для `SpeechRecognizer` создания.

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE", "The Endpoint ID for your custom model.")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

>[!Note]
> `speech_recognition_language``endpoint_id`Свойства и являются устаревшими из `SpeechConfig` класса в Python. Использовать эти свойства не рекомендуется, и их не следует использовать при создании `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>Как указать исходный язык в JavaScript

Первым шагом является создание `SpeechConfig` :

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

Затем укажите исходный язык для звука `speechRecognitionLanguage` :

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

Если вы используете пользовательскую модель для распознавания, конечную точку можно указать `endpointId` следующим образом:

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>Как указать исходный язык в цели-C

В этом примере исходный язык предоставляется явно в качестве параметра с помощью `SPXSpeechRecognizer` конструкции.

```Objective-C
SPXSpeechRecognizer* speechRecognizer = \
    [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig language:@"de-DE" audioConfiguration:audioConfig];
```

В этом примере исходный язык предоставляется с помощью `SPXSourceLanguageConfiguration` . Затем объект `SPXSourceLanguageConfiguration` передается в качестве параметра для `SPXSpeechRecognizer` создания.

```Objective-C
SPXSourceLanguageConfiguration* sourceLanguageConfig = [[SPXSourceLanguageConfiguration alloc]init:@"de-DE"];
SPXSpeechRecognizer* speechRecognizer = [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                                                     sourceLanguageConfiguration:sourceLanguageConfig
                                                                              audioConfiguration:audioConfig];
```

В этом примере исходный язык и Пользовательская конечная точка предоставляются с помощью `SPXSourceLanguageConfiguration` . Затем объект `SPXSourceLanguageConfiguration` передается в качестве параметра для `SPXSpeechRecognizer` создания.

```Objective-C
SPXSourceLanguageConfiguration* sourceLanguageConfig = \
        [[SPXSourceLanguageConfiguration alloc]initWithLanguage:@"de-DE"
                                                     endpointId:@"The Endpoint ID for your custom model."];
SPXSpeechRecognizer* speechRecognizer = [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                                                     sourceLanguageConfiguration:sourceLanguageConfig
                                                                              audioConfiguration:audioConfig];
```

>[!Note]
> `speechRecognitionLanguage``endpointId`Свойства и являются устаревшими из `SPXSpeechConfiguration` класса в цели-C. Использовать эти свойства не рекомендуется, и их не следует использовать при создании `SPXSpeechRecognizer` .

::: zone-end

## <a name="see-also"></a>См. также раздел

* Список поддерживаемых языков и языковых стандартов для распознавания речи в тексте см. в разделе [Поддержка языков](language-support.md).

## <a name="next-steps"></a>Следующие шаги

* [Справочная документация по пакету SDK для распознавания речи](speech-sdk.md)