---
title: Язык разметки речи (SSML) — служба речи
titleSuffix: Azure Cognitive Services
description: С помощью языка Speech Synthesis Markup Language можно управлять произношением и интонацией при преобразовании текста в речь.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: trbye
ms.openlocfilehash: 855feaf9b5b47b7b725ee7927418a2b3a9e25393
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2020
ms.locfileid: "84017779"
---
# <a name="improve-synthesis-with-speech-synthesis-markup-language-ssml"></a>Улучшение синтеза с помощью языка разметки речи (SSML)

Язык разметки речи (SSML) — это язык разметки на основе XML, позволяющий разработчикам указать способ преобразования текста ввода в синтезированное речевое сообщение с помощью службы "преобразование текста в речь". По сравнению с обычным текстом, SSML позволяет разработчикам точно настраивать тон, произношение, скорость речи, громкость и другие выходные данные текста в речь. Обычные знаки препинания, такие как пауза после точки, или использование правильного интонатион, когда предложение заканчивается вопросительным знаком, автоматически обрабатывается.

Реализация службы распознавания речи SSML основана на [языке разметки для распознавания речи консорциум W3C версии 1,0](https://www.w3.org/TR/speech-synthesis).

> [!IMPORTANT]
> Символы китайского, японского и корейского языков считаются двумя символами для выставления счетов. Дополнительные сведения см. на странице [цен](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Стандартные, нейронные и пользовательские голоса

Выберите один из стандартных и нейронных голосов или создайте собственный пользовательский голос, уникальный для продукта или торговой марки. более 75 языков и национальных стандартов доступны на более чем 45 языках, а на четырех языках и национальных стандартах доступно 5 голосов нейронов. Полный список поддерживаемых языков, языковых стандартов и голосовых моделей (нейронных и стандартных) см. в разделе [языковой поддержки](language-support.md).

Дополнительные сведения о стандартном, нейронном и настраиваемом голосовом см. в статье [Обзор преобразования текста в речь](text-to-speech.md).

## <a name="special-characters"></a>Специальные символы

При использовании SSML следует помнить, что специальные символы, такие как кавычки, апострофы и квадратные скобки, должны быть экранированы. Дополнительные сведения см. в разделе [язык XML (XML) 1,0: приложение г](https://www.w3.org/TR/xml/#sec-entexpand).

## <a name="supported-ssml-elements"></a>Поддерживаемые элементы SSML

Каждый документ SSML создается с помощью элементов SSML (или тегов). Эти элементы используются для настройки высоты, Prosody, громкости и т. д. В следующих разделах подробно описано, как используется каждый элемент, а также когда элемент является обязательным или необязательным.  

> [!IMPORTANT]
> Не забывайте использовать двойные кавычки вокруг значений атрибутов. Стандарты для правильного формата, допустимые XML-данные, требуют, чтобы значения атрибутов заключались в двойные кавычки. Например, `<prosody volume="90">` является правильно сформированным, допустимым элементом, но `<prosody volume=90>` не имеет значение. SSML может не распознать значения атрибутов, которые не находятся в кавычках.

## <a name="create-an-ssml-document"></a>Создание документа SSML

`speak`является корневым элементом и является **обязательным** для всех документов SSML. `speak`Элемент содержит важную информацию, например версию, язык и определение словаря разметки.

**Синтаксис**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `version` | Указывает версию спецификации SSML, используемую для интерпретации разметки документа. Текущая версия — 1,0. | Обязательно |
| `xml:lang` | Указывает язык корневого документа. Значение может содержать символы в нижнем регистре, двухбуквенный код языка (например, `en` ), языковой код и страну или регион в верхнем регистре (например, `en-US` ). | Обязательно |
| `xmlns` | Задает универсальный код ресурса (URI) документа, который определяет словарь разметки (типы элементов и имена атрибутов) документа SSML. Текущий URI — http://www.w3.org/2001/10/synthesis . | Обязательно |

## <a name="choose-a-voice-for-text-to-speech"></a>Выбор голоса для преобразования текста в речь

`voice`Элемент является обязательным. Он используется для указания голоса, используемого для преобразования текста в речь.

**Синтаксис**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `name` | Определяет голос, используемый для вывода текста в речь. Полный список поддерживаемых голосов см. в разделе [Поддержка языков](language-support.md#text-to-speech). | Обязательный |

**Пример**

> [!NOTE]
> В этом примере используется `en-US-AriaRUS` речь. Полный список поддерживаемых голосов см. в разделе [Поддержка языков](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Использование нескольких голосов

В `speak` элементе можно указать несколько голосов для вывода текста в речь. Эти голоса могут быть на разных языках. Для каждого голоса текст должен быть заключен в оболочку `voice` элемента. 

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `name` | Определяет голос, используемый для вывода текста в речь. Полный список поддерживаемых голосов см. в разделе [Поддержка языков](language-support.md#text-to-speech). | Обязательно |

> [!IMPORTANT]
> Несколько голосов несовместимы с функцией границы слова. Чтобы использовать несколько голосов, необходимо отключить функцию границ слов.

### <a name="disable-word-boundary"></a>Отключить границу слова

В зависимости от языка речевого пакета SDK вы устанавливаете `"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"` свойство в значение `false` в экземпляре `SpeechConfig` объекта.

# <a name="c"></a>[C#](#tab/csharp)

Дополнительные сведения см. в <a href="https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-dotnet" target="_blank"> `SetProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>разделе.

```csharp
speechConfig.SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="c"></a>[C++](#tab/cpp)

Дополнительные сведения см. в <a href="https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setproperty" target="_blank"> `SetProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>разделе.

```cpp
speechConfig->SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="java"></a>[Java](#tab/java)

Дополнительные сведения см. в <a href="https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-java-stable#com_microsoft_cognitiveservices_speech_SpeechConfig_setProperty_String_String_" target="_blank"> `setProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>разделе.

```java
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="python"></a>[Python](#tab/python)

Дополнительные сведения см. в <a href="https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#set-property-by-name-property-name--str--value--str-" target="_blank"> `set_property_by_name` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>разделе.

```python
speech_config.set_property_by_name(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Дополнительные сведения см. в <a href="https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#setproperty-string--string-" target="_blank"> `setProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>разделе.

```javascript
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="objective-c"></a>[Objective-C](#tab/objectivec)

Дополнительные сведения см. в <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank"> `setPropertyTo` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>разделе.

```objectivec
[speechConfig setPropertyTo:@"false" byName:@"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"];
```

# <a name="swift"></a>[Swift](#tab/swift)

Дополнительные сведения см. в <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank"> `setPropertyTo` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>разделе.

```swift
speechConfig!.setPropertyTo(
    "false", byName: "SpeechServiceResponse_Synthesis_WordBoundaryEnabled")
```

---

**Пример**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        Good morning!
    </voice>
    <voice name="en-US-Guy24kRUS">
        Good morning to you too Aria!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Изменить стили речи

> [!IMPORTANT]
> Настройки стилей речи будут работать только с нейронными голосовыми данными.

По умолчанию в службе преобразования текста в речь используется нейтральный стиль для стандартных и нейронных голосов. С помощью нейронных голосов можно настроить стиль речи для выражения различных эмоции, таких как чирфулнесс, сопереживание и приходом, или оптимизировать голос для различных сценариев, таких как пользовательская служба, невскастинг и голосового помощника, используя элемент <мсттс: Express-AS>. Это необязательный элемент, уникальный для службы распознавания речи.

В настоящее время для этих нейронных голосов поддерживаются настройки стиля речи:
* `en-US-AriaNeural`
* `zh-CN-XiaoxiaoNeural`
* `zh-CN-YunyangNeural`

Изменения применяются на уровне предложений, а стиль зависит от голоса. Если стиль не поддерживается, служба вернет речь по нейтральному стилю речи по умолчанию.

**Синтаксис**

```xml
<mstts:express-as style="string"></mstts:express-as>
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `style` | Задает стиль речи. В настоящее время стиль речи зависит от голоса. | Требуется при настройке стиля речи для нейронной речи. Если используется `mstts:express-as` , необходимо указать стиль. Если указано недопустимое значение, этот элемент будет проигнорирован. |

Используйте эту таблицу, чтобы определить, какие стили речи поддерживаются для каждого нейронного голоса.

| Голосовая связь                   | Стиль                     | Описание                                                 |
|-------------------------|---------------------------|-------------------------------------------------------------|
| `en-US-AriaNeural`      | `style="newscast"`        | Выражает формальный и профессиональный тон для новостных закадровых новостей |
|                         | `style="customerservice"` | Выражает понятный и полезный тон поддержки клиентов  |
|                         | `style="chat"`            | Выражает обычный и нестрогий тон                         |
|                         | `style="cheerful"`        | Выражает положительный и счастливый тон                         |
|                         | `style="empathetic"`      | Выражает представление о волнует и понимании               |
| `zh-CN-XiaoxiaoNeural`  | `style="newscast"`        | Выражает формальный и профессиональный тон для новостных закадровых новостей |
|                         | `style="customerservice"` | Выражает понятный и полезный тон поддержки клиентов  |
|                         | `style="assistant"`       | Выражает горячий и неослабленный тон для цифровых помощников    |
|                         | `style="lyrical"`         | Выражает эмоции в мелодик и драгоценностям способом         |   
| `zh-CN-YunyangNeural`   | `style="customerservice"` | Выражает понятный и полезный тон поддержки клиентов  | 

**Пример**

В этом фрагменте SSML показано, как `<mstts:express-as>` элемент используется для изменения стиля речи на `cheerful` .

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        <mstts:express-as style="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Добавление или удаление перерыва или паузы

Используйте `break` элемент, чтобы вставить паузы (или перерывы) между словами или предотвратить автоматическое добавление приостанова службой преобразования текста в речь.

> [!NOTE]
> Используйте этот элемент, чтобы переопределить поведение по умолчанию преобразования текста в речь (TTS) для слова или фразы, если синтезированное речевое выражение для этого слова или фразы звучит неестественно. Задайте значение, `strength` `none` чтобы предотвратить разрыв интонационную, который автоматически вставляется службой преобразования текста в речь.

**Синтаксис**

```xml
<break strength="string" />
<break time="string" />
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `strength` | Задает относительную продолжительность паузы, используя одно из следующих значений:<ul><li>нет</li><li>x-слабый</li><li>безопасные</li><li>Средний (по умолчанию)</li><li>надежный</li><li>строгость x</li></ul> | Необязательно |
| `time` | Задает абсолютную длительность паузы в секундах или миллисекундах. Примеры допустимых значений — `2s` и.`500` | Необязательно |

| Сложность                      | Описание |
|-------------------------------|-------------|
| None или, если значение не указано | 0 мс        |
| x-слабый                        | 250 мс      |
| безопасные                          | 500 мс      |
| средняя                        | 750 мс      |
| надежный                        | 1000 мс     |
| строгость x                      | 1250 МС     |

**Пример**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Указание абзацев и предложений

`p``s`элементы and используются для обозначения абзацев и предложений соответственно. При отсутствии этих элементов служба преобразования текста в речь автоматически определяет структуру документа SSML.

`p`Элемент может содержать текст и следующие элементы: `audio` , `break` , `phoneme` , `prosody` , `say-as` , `sub` , `mstts:express-as` и `s` .

`s`Элемент может содержать текст и следующие элементы: `audio` , `break` , `phoneme` ,,, `prosody` `say-as` `mstts:express-as` и `sub` .

**Синтаксис**

```XML
<p></p>
<s></s>
```

**Пример**

```XML
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Использование фонемы для улучшения произношения

`ph`Элемент используется для фонетического произношения в документах SSML. `ph`Элемент может содержать только текст, но не другие элементы. Всегда предоставляющих удобное для восприятия речь в качестве резерва.

Фонетические алфавиты состоят из телефонов, которые состоят из букв, цифр или символов, иногда в сочетании. Каждый телефон описывает уникальный звук речи. Это отличается от латинского алфавита, где любая буква может представлять несколько речевых звуков. Рассмотрите разные произношения буквы "c" в словах "«а» и «прекращение» или другие произношение буквы «TH» в словах «вещь» и «эти».

**Синтаксис**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `alphabet` | Указывает фонетический алфавит, используемый при создании транскрипции строки в `ph` атрибуте. Строка, указывающая букву алфавита, должна быть указана в строчных буквах. Ниже приведены возможные алфавиты, которые можно указать.<ul><li>`ipa`&ndash; <a href="https://en.wikipedia.org/wiki/International_Phonetic_Alphabet" target="_blank">Международный фонетический <span class="docon docon-navigate-external x-hidden-focus"></span> алфавит</a></li><li>`sapi`&ndash; [Фонетический алфавит речевой службы](speech-ssml-phonetic-sets.md)</li><li>`ups`&ndash; <a href="https://documentation.help/Microsoft-Speech-Platform-SDK-11/17509a49-cae7-41f5-b61d-07beaae872ea.htm" target="_blank">Универсальный набор телефонов</a></li></ul><br>Алфавит применяется только к `phoneme` элементу в элементе.. | Необязательно |
| `ph` | Строка, содержащая телефоны, задающих произношение слова в `phoneme` элементе. Если указанная строка содержит нераспознаваемые телефоны, служба преобразования текста в речь (TTS) отклоняет весь документ SSML и не выдает ни одного из речевых выходных данных, указанных в документе. | Требуется при использовании фонемы. |

**Примеры**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <phoneme alphabet="sapi" ph="iy eh n y uw eh s"> en-US </phoneme>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

## <a name="use-custom-lexicon-to-improve-pronunciation"></a>Использование пользовательского лексикона для улучшения произношения

Иногда служба преобразования текста в речь не может правильно произношеновать слово. Например, название компании или медицинское условие. Разработчики могут определить, как отдельные сущности считываются в SSML с помощью `phoneme` `sub` тегов и. Однако если необходимо определить, как считываются несколько сущностей, можно создать пользовательский словарь с помощью `lexicon` тега.

> [!NOTE]
> Настраиваемый словарь в настоящее время поддерживает кодировку UTF-8. 

**Синтаксис**

```XML
<lexicon uri="string"/>
```

**Атрибуты**

| attribute | Описание                               | Обязательный или необязательный |
|-----------|-------------------------------------------|---------------------|
| `uri`     | Адрес внешнего документа областей. | Обязательный элемент.           |

**Использование**

Чтобы определить, как считываются несколько сущностей, можно создать пользовательский словарь, который хранится в формате XML или областей. Ниже приведен пример XML-файла.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<lexicon version="1.0" 
      xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
      xsi:schemaLocation="http://www.w3.org/2005/01/pronunciation-lexicon 
        http://www.w3.org/TR/2007/CR-pronunciation-lexicon-20071212/pls.xsd"
      alphabet="ipa" xml:lang="en-US">
  <lexeme>
    <grapheme>BTW</grapheme> 
    <alias>By the way</alias> 
  </lexeme>
  <lexeme>
    <grapheme> Benigni </grapheme> 
    <phoneme> bɛˈniːnji</phoneme>
  </lexeme>
</lexicon>
```

`lexicon`Элемент содержит по крайней мере один `lexeme` элемент. Каждый `lexeme` элемент содержит по крайней мере один `grapheme` элемент и один или несколько `grapheme` `alias` элементов, и `phoneme` . `grapheme`Элемент содержит текст, описывающий <a href="https://www.w3.org/TR/pronunciation-lexicon/#term-Orthography" target="_blank">орсографи <span class="docon docon-navigate-external x-hidden-focus"></span> </a>. `alias`Элементы используются для указания произношения акронима или сокращенного выражения. `phoneme`Элемент предоставляет текст, описывающий, как `lexeme` произносится.

Важно отметить, что нельзя напрямую задавать произношение слова с помощью пользовательского лексикона. Если необходимо задать произношение для, сначала укажите `alias` , а затем свяжите `phoneme` с ним `alias` . Пример:

```xml
  <lexeme>
    <grapheme>Scotland MV</grapheme> 
    <alias>ScotlandMV</alias> 
  </lexeme>
  <lexeme>
    <grapheme>ScotlandMV</grapheme> 
    <phoneme>ˈskɒtlənd.ˈmiːdiəm.weɪv</phoneme>
  </lexeme>
```

> [!IMPORTANT]
> `phoneme`Элемент не может содержать пробелы при использовании IPA.

Дополнительные сведения о настраиваемом файле лексикона см. в разделе [Спецификация лексикона произношения (областей) версии 1,0](https://www.w3.org/TR/pronunciation-lexicon/).

Затем опубликуйте пользовательский файл лексикона. Хотя у нас нет ограничений на место хранения этого файла, мы рекомендуем использовать [хранилище BLOB-объектов Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

После публикации пользовательского лексикона вы можете сослаться на него из SSML.

> [!NOTE]
> `lexicon`Элемент должен находиться внутри `voice` элемента.

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" 
          xmlns:mstts="http://www.w3.org/2001/mstts" 
          xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <lexicon uri="http://www.example.com/customlexicon.xml"/>
        BTW, we will be there probably at 8:00 tomorrow morning.
        Could you help leave a message to Robert Benigni for me?
    </voice>
</speak>
```

При использовании этого пользовательского лексикона "БТВ" будет считаться "следующим образом". "Бенигни" будет считан с помощью предоставленного IPA "бɛ ˈ Ni ː НЖИ".  

**Ограничения**
- Размер файла: максимальный размер файла словаря, 100 КБ, если он превышает этот размер, запрос синтеза завершится ошибкой.
- Обновление кэша словаря: Пользовательский словарь будет кэшироваться с URI в качестве ключа в службе TTS при первой загрузке. Словарь с одинаковым URI не будет перезагружен в течение 15 минут, поэтому для вступления в силу пользовательского лексикона необходимо подождать не более 15 минут.

**Фонетические наборы речевых служб**

В приведенном выше примере мы используем Международный фонетический алфавит, также известный как набор IPA Phone. Мы рекомендуем разработчикам использовать IPA, так как это Международный стандарт. Для некоторых IPA символов они имеют версию "уже сформированные" и "разложенные", если они представлены в Юникоде. Пользовательский словарь поддерживает только символы Юникода.

Учитывая, что IPA нелегко запомнить, служба распознавания речи определяет фонетический набор для семи языков (,, `en-US` ,,, `fr-FR` `de-DE` `es-ES` `ja-JP` `zh-CN` и `zh-TW` ).

Вы можете использовать в `sapi` качестве значение для `alphabet` атрибута с пользовательскими лексиконами, как показано ниже:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<lexicon version="1.0" 
      xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.w3.org/2005/01/pronunciation-lexicon
        http://www.w3.org/TR/2007/CR-pronunciation-lexicon-20071212/pls.xsd"
      alphabet="sapi" xml:lang="en-US">
  <lexeme>
    <grapheme>BTW</grapheme>
    <alias> By the way </alias>
  </lexeme>
  <lexeme>
    <grapheme> Benigni </grapheme>
    <phoneme> b eh 1 - n iy - n y iy </phoneme>
  </lexeme>
</lexicon>
```

Дополнительные сведения об фонетическом алфавите в службе распознавания речи см. в разделе [фонетические наборы речевых служб](speech-ssml-phonetic-sets.md).

## <a name="adjust-prosody"></a>Настройка Prosody

`prosody`Элемент используется для указания изменений для шага, профиля, диапазона, скорости, длительности и объема для вывода текста в речь. `prosody`Элемент может содержать текст и следующие элементы: `audio` , `break` , `p` , `phoneme` , `prosody` , `say-as` , `sub` и `s` .

Так как значения атрибутов интонационную могут различаться для широкого диапазона, распознаватель речи интерпретирует назначенные значения как предложение фактического значения интонационную выбранного голоса. Служба преобразования текста в речь ограничивает или заменяет значения, которые не поддерживаются. Примеры неподдерживаемых значений — шаг 1 МГц или объем 120.

**Синтаксис**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `pitch` | Указывает шаг по базовому плану для текста. Вы можете выразить тон следующим образом:<ul><li>Абсолютное значение, выраженное в виде числа, за которым следует "Гц" (герц). Например, 600 Гц.</li><li>Относительное значение, выраженное в виде числа, начинающегося с "+" или "-" и заканчивающееся на "Гц" или "St", которое указывает величину изменения высоты. Например: + 80 Гц или-2st. "St" означает, что единица изменения — полтона, то есть половина тона (половина шага) на стандартной диатоник шкале.</li><li>Значение константы:<ul><li>x-низкий</li><li>low</li><li>средняя</li><li>high</li><li>x-высокий</li><li>default</li></ul></li></ul>. | Необязательно |
| `contour` |Профиль теперь поддерживает как нейронные, так и стандартные голоса. Контур представляет изменения по шагам. Эти изменения представлены в виде массива целевых объектов в определенные позиции времени в выходных данных речи. Каждый целевой объект определяется набором пар параметров. Пример: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Первое значение в каждом наборе параметров указывает расположение изменения высоты в процентах от длительности текста. Второе значение задает величину, которую нужно увеличить или уменьшить, используя относительное значение или значение перечисления для высоты (см `pitch` .). | Необязательно |
| `range` | Значение, представляющее диапазон высоты для текста. `range`Для описания можно использовать те же абсолютные значения, относительные значения или значения перечисления `pitch` . | Необязательно |
| `rate` | Указывает интенсивность речи текста. Вы можете выразить `rate` следующим образом:<ul><li>Относительное значение, выраженное в виде числа, которое выступает в качестве множителя по умолчанию. Например, значение *1* приводит к отсутствию изменений в скорости. Значение *0,5* приводит к половине скорости. Значение *3* приводит к утраиваетсяу скорости.</li><li>Значение константы:<ul><li>x-замедляют</li><li>медленный</li><li>средняя</li><li>быстрая</li><li>x-Fast</li><li>default</li></ul></li></ul> | Необязательно |
| `duration` | Период времени, в течение которого служба синтеза речи (TTS) считывает текст в секундах или миллисекундах. Например, *2S* или *1800ms*. | Необязательно |
| `volume` | Указывает уровень громкости речи. Том можно выразить следующим образом:<ul><li>Абсолютное значение, выраженное в виде числа в *диапазоне от 0,0* до 100,0, от самого дальнего к *громкости*. Например, 75. Значение по умолчанию — 100,0.</li><li>Относительное значение, выраженное в виде числа с префиксом "+" или "-", которое указывает количество изменений в томе. Например, + 10 или-5,5.</li><li>Значение константы:<ul><li>silent</li><li>x — Soft</li><li>мягкий</li><li>средняя</li><li>сделать</li><li>x-вслух</li><li>default</li></ul></li></ul> | Необязательно |

### <a name="change-speaking-rate"></a>Изменение скорости речи

Скорость речи можно применять к нейронным голоса и стандартным голосовым данным на уровне слов или предложений. 

**Пример**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-GuyNeural">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Изменение громкости

Изменения тома можно применить к стандартным голосовым данным на уровне слов или предложений. Изменения тома могут применяться только к нейронным голосовым уровням на уровне предложений.

**Пример**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Изменение тона

Изменения высоты можно применить к стандартным голосовым параметрам на уровне слов или предложений. Изменения высоты могут применяться только к нейронным голосовым уровням на уровне предложений.

**Пример**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Изменение контура тона

> [!IMPORTANT]
> Изменения контура теперь поддерживаются с помощью нейронных голосов.

**Пример**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        <prosody contour="(60%,-60%) (100%,+80%)" >
            Were you the only person in the room? 
        </prosody>
    </voice>
</speak>
```
## <a name="say-as-element"></a>элемент "скажем как"

`say-as`— Необязательный элемент, который указывает тип содержимого (например, число или дату) текста элемента. Это дает рекомендации подсистеме синтеза речи о том, как произносится текст.

**Синтаксис**

```XML
<say-as interpret-as="string" format="digit string" detail="string"> <say-as>
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `interpret-as` | Указывает тип содержимого текста элемента. Список типов см. в таблице ниже. | Обязательно |
| `format` | Предоставляет дополнительные сведения о точном форматировании текста элемента для типов содержимого, которые могут иметь неоднозначные форматы. SSML определяет форматы для типов содержимого, которые их используют (см. таблицу ниже). | Необязательно |
| `detail` | Указывает уровень детализации для речи. Например, этот атрибут может запросить, чтобы механизм синтеза речи произносит знаки пунктуации. Для не определено ни одного стандартного значения `detail` . | Необязательно |

<!-- I don't understand the last sentence. Don't we know which one Cortana uses? -->

Ниже приведены поддерживаемые типы содержимого для `interpret-as` `format` атрибутов и. Включайте `format` атрибут только в том случае `interpret-as` , если для параметра задано значение Дата и время.

| интерпретировать как | format | Интерпретация |
|--------------|--------|----------------|
| `address` | | Текст обговорятся как адрес. Модуль синтеза речи произносит следующее:<br /><br />`I'm at <say-as interpret-as="address">150th CT NE, Redmond, WA</say-as>`<br /><br />Как «я в 150th суд Северная Восточная часть Редмонд Вашингтон». |
| `cardinal`, `number` | | Текст обназывается количеством элементов. Модуль синтеза речи произносит следующее:<br /><br />`There are <say-as interpret-as="cardinal">3</say-as> alternatives`<br /><br />Как «есть три варианта». |
| `characters`, `spell-out` | | Текст обносится как отдельные буквы (с буквами). Модуль синтеза речи произносит следующее:<br /><br />`<say-as interpret-as="characters">test</say-as>`<br /><br />Как "T E S". |
| `date` | dmy, mdy, ГМД, ГДМ, им, My, MD, DM, d, m, y | Текст обговорятся как дата. `format`Атрибут задает формат даты (*d = день, m = месяц и y = year*). Модуль синтеза речи произносит следующее:<br /><br />`Today is <say-as interpret-as="date" format="mdy">10-19-2016</say-as>`<br /><br />Как "Сегодня Октябрь девятнадцатого 2016". |
| `digits`, `number_digit` | | Текст обговорятся как последовательность отдельных цифр. Модуль синтеза речи произносит следующее:<br /><br />`<say-as interpret-as="number_digit">123456789</say-as>`<br /><br />Как "1 2 3 4 5 6 7 8 9". |
| `fraction` | | Текст обозначается как дробное число. Модуль синтеза речи произносит следующее:<br /><br /> `<say-as interpret-as="fraction">3/8</say-as> of an inch`<br /><br />Как «три восьмых дюйма». |
| `ordinal` | | Текст обговорятся как порядковый номер. Модуль синтеза речи произносит следующее:<br /><br />`Select the <say-as interpret-as="ordinal">3rd</say-as> option`<br /><br />Как "выберите третий вариант". |
| `telephone` | | Текст обговорятся как телефонный номер. `format`Атрибут может содержать цифры, представляющие код страны. Например, "1" для США или "39" для Италии. Подсистема синтеза речи может использовать эти сведения для указания произношения номера телефона. Номер телефона может также включать код страны и, если да, имеет приоритет над кодом страны в `format` . Модуль синтеза речи произносит следующее:<br /><br />`The number is <say-as interpret-as="telephone" format="1">(888) 555-1212</say-as>`<br /><br />Как «мой номер — код города 8 8 8 5 5 5 1 2 1 2». |
| `time` | hms12, hms24 | Текст обговорятся как время. `format`Атрибут указывает, задано ли время с помощью 12-часового времени (hms12) или 24-часового времени (hms24). Используйте двоеточие для разделения чисел, представляющих часы, минуты и секунды. Ниже приведены допустимые примеры времени: 12:35, 1:14:32, 08:15 и 02:50:45. Модуль синтеза речи произносит следующее:<br /><br />`The train departs at <say-as interpret-as="time" format="hms12">4:00am</say-as>`<br /><br />Как «обучение отчасти по четырем A M». |

**Использование**

`say-as`Элемент может содержать только текст.

**Пример**

Подсистема синтеза речи выступает в следующем примере, как «ваш первый запрос был для одной комнаты в октябре девятнадцатого 20 10 с ранним поступлением в 12 35 PM».
 
```XML
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <p>
        Your <say-as interpret-as="ordinal"> 1st </say-as> request was for <say-as interpret-as="cardinal"> 1 </say-as> room
        on <say-as interpret-as="date" format="mdy"> 10/19/2010 </say-as>, with early arrival at <say-as interpret-as="time" format="hms12"> 12:35pm </say-as>.
        </p>
    </voice>
</speak>
```

## <a name="add-recorded-audio"></a>Добавить записанный звук

`audio`— Это необязательный элемент, позволяющий вставлять звук MP3 в документ SSML. Тело элемента audio может содержать обычный текст или SSML разметку, которые говорят, если звуковой файл недоступен или воспроизводится. Кроме того, `audio` элемент может содержать текст и следующие элементы: `audio` , `break` , `p` , `s` , `phoneme` , `prosody` , `say-as` и `sub` .

Все аудио, входящие в документ SSML, должны соответствовать следующим требованиям:

* MP3 должен размещаться на доступной в Интернете конечной точке HTTPS. Требуется протокол HTTPS, а домен, на котором размещается MP3-файл, должен представлять действительный доверенный сертификат TLS/SSL.
* Формат MP3 должен быть допустимым MP3-файлом (MPEG v2).
* Скорость потока должна составлять 48 кбит/с.
* Частота дискретизации должна составлять 16 000 Гц.
* Суммарное общее время для всех текстовых и звуковых файлов в одном ответе не может превышать 90 (90) секунд.
* MP3 не должен содержать никаких конфиденциальных сведений, относящихся к заказчику.

**Синтаксис**

```xml
<audio src="string"/></audio>
```

**Атрибуты**

| attribute | Описание                                   | Обязательный или необязательный                                        |
|-----------|-----------------------------------------------|------------------------------------------------------------|
| `src`     | Указывает расположение или URL-адрес звукового файла. | Требуется при использовании элемента audio в документе SSML. |

**Пример**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <p>
            <audio src="https://contoso.com/opinionprompt.wav"/>
            Thanks for offering your opinion. Please begin speaking after the beep.
            <audio src="https://contoso.com/beep.wav">
                Could not play the beep, please voice your opinion now.
            </audio>
        </p>
    </voice>
</speak>
```

## <a name="add-background-audio"></a>Добавить фоновый звук

`mstts:backgroundaudio`Элемент позволяет добавить фоновый звук в документы SSML (или смешать звуковой файл с текстом в речь). С помощью можно `mstts:backgroundaudio` перебрать звуковой файл в фоновом режиме, появление в начале текста в речь и появление в конце текста в речь.

Если предоставленный фоновый звук короче, чем преобразование текста в речь или Исчезание, он будет циклическим. Если оно длиннее, чем преобразование текста в речь, оно будет остановлено по завершении затухания.

Для каждого документа SSML допускается только один фоновый звуковой файл. Тем не менее Теги можно `audio` `voice` добавлять в элемент, чтобы добавить дополнительный звук в документ SSML.

**Синтаксис**

```XML
<mstts:backgroundaudio src="string" volume="string" fadein="string" fadeout="string"/>
```

**Атрибуты**

| attribute | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| `src` | Указывает расположение или URL-адрес фонового звукового файла. | Требуется при использовании фонового звука в документе SSML. |
| `volume` | Указывает громкость фонового звукового файла. **Допустимые значения**: `0` в `100` включительно. Значение по умолчанию — `1`. | Необязательно |
| `fadein` | Задает время, в течение которого фоновый звук постепенно передается в миллисекундах. Значение по умолчанию — `0` , что эквивалентно отсутствию появления. **Допустимые значения**: `0` в `10000` включительно.  | Необязательно |
| `fadeout` | Указывает длительность фонового исчезновения в миллисекундах. Значение по умолчанию — `0` , что эквивалентно значению без затухания. **Допустимые значения**: `0` в `10000` включительно.  | Необязательно |

**Пример**

```xml
<speak version="1.0" xml:lang="en-US" xmlns:mstts="http://www.w3.org/2001/mstts">
    <mstts:backgroundaudio src="https://contoso.com/sample.wav" volume="0.7" fadein="3000" fadeout="4000"/>
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)">
        The text provided in this document will be spoken over the background audio.
    </voice>
</speak>
```

## <a name="next-steps"></a>Дальнейшие действия

* [Поддержка языков и регионов в API службы "Речь"](language-support.md)
