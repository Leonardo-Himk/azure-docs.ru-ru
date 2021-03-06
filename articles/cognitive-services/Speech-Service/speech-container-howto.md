---
title: Установка речевых контейнеров — служба речи
titleSuffix: Azure Cognitive Services
description: Установите и запустите речевые контейнеры. Преобразование речи в текст позволяет расшифровывать аудиопотоки в режиме реального времени и сохранять их в текстовом формате, который ваши приложения, инструменты или устройства могут использовать или отображать. Преобразование текста в речь преобразует вводимый текст в синтезированную речь, похожую на человеческую.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: aahi
ms.openlocfilehash: 1d4fde8dd21911b70d5a1c0f3b23304a3468a2a6
ms.sourcegitcommit: fc0431755effdc4da9a716f908298e34530b1238
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/24/2020
ms.locfileid: "83816239"
---
# <a name="install-and-run-speech-service-containers-preview"></a>Установка и запуск контейнеров речевых служб (Предварительная версия)

Контейнеры позволяют запускать некоторые API речевых служб в собственной среде. Контейнеры отлично подходят для конкретных требований к безопасности и управлению данными. В этой статье вы узнаете, как скачать, установить и запустить речевой контейнер.

Речевые контейнеры позволяют клиентам создавать архитектуру приложений для распознавания речи, оптимизированных как для надежных облачных возможностей, так и для пограничных локализации. Доступно четыре разных контейнера. Два стандартных контейнера — преобразование **речи в текст** и преобразование **текста в речь**. Два пользовательских контейнера — это **пользовательское распознавание речи** и **Пользовательский текст в речь**. В речевых контейнерах действуют те же [цены](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) , что и в облачных службах Azure для распознавания речи.

> [!IMPORTANT]
> Все контейнеры речи в настоящее время предоставляются как часть [общедоступного "условного" предварительного просмотра](../cognitive-services-container-support.md#public-gated-preview-container-registry-containerpreviewazurecrio). Объявление будет создано, когда речевые контейнеры переводятся в общедоступную версию.

| Компонент | Компоненты | Последняя |
|--|--|--|
| Преобразование речи в текст | Анализирует тональности и расшифровывает непрерывную голосовую или пакетную звукозапись в реальном времени с промежуточными результатами.  | 2.2.0 |
| Пользовательское распознавание речи к тексту | Используя настраиваемую модель на [портале пользовательское распознавание речи](https://speech.microsoft.com/customspeech), расшифровывает непрерывную голосовую или пакетную звукозапись в режиме реального времени в текст с промежуточными результатами. | 2.2.0 |
| Преобразование текста в речь | Преобразует текст в голосовую речь с помощью обычного текстового ввода или языка разметки речи (SSML). | 1.4.0 |
| Пользовательский текст в речь | С помощью настраиваемой модели [пользовательского голосового портала](https://aka.ms/custom-voice-portal)преобразует текст в голосовую речь с помощью обычного текстового ввода или языка разметки речи (SSML). | 1.4.0 |

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Перед использованием речевых контейнеров выполните следующие предварительные требования.

| Обязательно | Назначение |
|--|--|
| Модуль Docker | На [главном компьютере](#the-host-computer) должен быть установлен модуль Docker. Docker предоставляет пакеты, которые настраивают среду с Docker для [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) и [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Ознакомьтесь с [общими сведениями о Docker и контейнерах](https://docs.docker.com/engine/docker-overview/).<br><br> Docker нужно настроить таким образом, чтобы контейнеры могли подключать и отправлять данные о выставлении счетов в Azure. <br><br> **В ОС Windows** для Docker нужно также настроить поддержку контейнеров Linux.<br><br> |
| Опыт работы с Docker | Требуется базовое представление о понятиях Docker, включая реестры, репозитории, контейнеры и образы контейнеров, а также знание основных команд `docker`. |
| Речевой ресурс | Для использования контейнеров необходимо следующее:<br><br>Ресурс _речи_ Azure для получения связанного ключа API и URI конечной точки. Оба значения доступны на страницах "Обзор **речи** " и "ключи" портал Azure. Они необходимы для запуска контейнера.<br><br>**{API_KEY}**: один из двух доступных ключей ресурсов на странице " **ключи** "<br><br>**{ENDPOINT_URI}**: конечная точка, указанная на странице **обзора** |

## <a name="request-access-to-the-container-registry"></a>Запрос доступа к реестру контейнеров

Заполните [форму запроса](https://aka.ms/cognitivegate) и отправьте ее, чтобы запросить доступ к контейнеру. 


[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Главный компьютер

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Поддержка расширенного векторного расширения

**Главным** является компьютер, на котором выполняется контейнер Docker. Узел *должен поддерживать* [Расширенные векторные расширения](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2). Вы можете проверить поддержку AVX2 на узлах Linux с помощью следующей команды:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```
> [!WARNING]
> Главный компьютер *необходим* для поддержки AVX2. Контейнер *не будет* работать правильно без поддержки AVX2.

### <a name="container-requirements-and-recommendations"></a>Требования к контейнеру и рекомендации

В следующей таблице описаны минимальное и рекомендуемое выделение ресурсов для каждого контейнера речи.

# <a name="speech-to-text"></a>[Преобразование речи в текст](#tab/stt)

| Контейнер | Минимальные | Рекомендуется |
|-----------|---------|-------------|
| Преобразование речи в текст | 2 ядра, 2 ГБ памяти | 4 ядра, 4 ГБ памяти |

# <a name="custom-speech-to-text"></a>[Пользовательское распознавание речи к тексту](#tab/cstt)

| Контейнер | Минимальные | Рекомендуется |
|-----------|---------|-------------|
| Пользовательское распознавание речи к тексту | 2 ядра, 2 ГБ памяти | 4 ядра, 4 ГБ памяти |

# <a name="text-to-speech"></a>[Преобразование текста в речь](#tab/tts)

| Контейнер | Минимальные | Рекомендуется |
|-----------|---------|-------------|
| Преобразование текста в речь | 1 ядро, 2 ГБ памяти | 2 ядра, 3 ГБ памяти |

# <a name="custom-text-to-speech"></a>[Пользовательский текст в речь](#tab/ctts)

| Контейнер | Минимальные | Рекомендуется |
|-----------|---------|-------------|
| Пользовательский текст в речь | 1 ядро, 2 ГБ памяти | 2 ядра, 3 ГБ памяти |

***

* Частота каждого ядра должна быть минимум 2,6 ГГц.

Ядро и память соответствуют параметрам `--cpus` и `--memory`, которые используются как часть команды `docker run`.

> [!NOTE]
> Минимальное и рекомендуемое значение основано на ограничениях DOCKER, а *не* на ресурсах компьютера размещения. Например, контейнеры преобразования речи в текст отображают части модели больших языков, и *рекомендуется* , чтобы весь файл поместился в память, что является дополнительным 4-6 ГБ. Кроме того, первый запуск любого контейнера может занять больше времени, так как модели разбиваются на страницы в памяти.

## <a name="get-the-container-image-with-docker-pull"></a>Получение образа контейнера с помощью `docker pull`

Образы контейнеров для речи доступны в следующем реестре контейнеров.

# <a name="speech-to-text"></a>[Преобразование речи в текст](#tab/stt)

| Контейнер | Хранилище |
|-----------|------------|
| Преобразование речи в текст | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |

# <a name="custom-speech-to-text"></a>[Пользовательское распознавание речи к тексту](#tab/cstt)

| Контейнер | Хранилище |
|-----------|------------|
| Пользовательское распознавание речи к тексту | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest` |

# <a name="text-to-speech"></a>[Преобразование текста в речь](#tab/tts)

| Контейнер | Хранилище |
|-----------|------------|
| Преобразование текста в речь | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

# <a name="custom-text-to-speech"></a>[Пользовательский текст в речь](#tab/ctts)

| Контейнер | Хранилище |
|-----------|------------|
| Пользовательский текст в речь | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest` |

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-speech-containers"></a>Извлечение DOCKER для контейнеров распознавания речи

# <a name="speech-to-text"></a>[Преобразование речи в текст](#tab/stt)

#### <a name="docker-pull-for-the-speech-to-text-container"></a>Опрашивающий запрос DOCKER для контейнера преобразования речи в текст

Используйте команду [DOCKER Pull](https://docs.docker.com/engine/reference/commandline/pull/) , чтобы скачать образ контейнера из реестра предварительного просмотра контейнера.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

> [!IMPORTANT]
> `latest`Тег извлекает `en-US` языковой стандарт. Дополнительные языковые стандарты см. [в разделе языки и речь](#speech-to-text-locales).

#### <a name="speech-to-text-locales"></a>Языки перевода речи в текст

Все теги, кроме, `latest` имеют следующий формат и учитывают регистр:

```
<major>.<minor>.<patch>-<platform>-<locale>-<prerelease>
```

Следующий тег является примером формата:

```
2.2.0-amd64-en-us-preview
```

Сведения о всех поддерживаемых языковых стандартах контейнера **для преобразования речи** в текст см. в статье [теги изображений](../containers/container-image-tags.md#speech-to-text).

# <a name="custom-speech-to-text"></a>[Пользовательское распознавание речи к тексту](#tab/cstt)

#### <a name="docker-pull-for-the-custom-speech-to-text-container"></a>Опрашивающий запрос DOCKER для контейнера Пользовательское распознавание речи в текст

Используйте команду [DOCKER Pull](https://docs.docker.com/engine/reference/commandline/pull/) , чтобы скачать образ контейнера из реестра предварительного просмотра контейнера.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest
```

> [!NOTE]
> `locale` `voice` Пользовательские контейнеры и для пользовательских речевых контейнеров определяются настраиваемой моделью, принимаемой контейнером.

# <a name="text-to-speech"></a>[Преобразование текста в речь](#tab/tts)

#### <a name="docker-pull-for-the-text-to-speech-container"></a>Извлечение DOCKER для контейнера текста в речь

Используйте команду [DOCKER Pull](https://docs.docker.com/engine/reference/commandline/pull/) , чтобы скачать образ контейнера из реестра предварительного просмотра контейнера.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

> [!IMPORTANT]
> `latest`Тег извлекает `en-US` языковой стандарт и `jessarus` голосовое значение. Дополнительные языковые стандарты см. [в разделе Языки перевода текста в речь](#text-to-speech-locales).

#### <a name="text-to-speech-locales"></a>Языки перевода текста в речь

Все теги, кроме, `latest` имеют следующий формат и учитывают регистр:

```
<major>.<minor>.<patch>-<platform>-<locale>-<voice>-<prerelease>
```

Следующий тег является примером формата:

```
1.3.0-amd64-en-us-jessarus-preview
```

Все поддерживаемые языковые стандарты и соответствующие **голоса контейнера преобразования текста в речь см** . в разделе [теги изображений текста в речь](../containers/container-image-tags.md#text-to-speech).

> [!IMPORTANT]
> При создании стандартного HTTP-запроса POST *для преобразования текста в речь* в сообщении о [языке разметки речи (SSML)](speech-synthesis-markup.md) требуется `voice` элемент с `name` атрибутом. Значение представляет собой соответствующий языковой стандарт контейнера и голоса, также называемый ["коротким именем"](language-support.md#standard-voices). Например, `latest` тег будет иметь название Voice `en-US-JessaRUS` .

# <a name="custom-text-to-speech"></a>[Пользовательский текст в речь](#tab/ctts)

#### <a name="docker-pull-for-the-custom-text-to-speech-container"></a>Опрашивающий запрос DOCKER для пользовательского контейнера преобразования текста в речь

Используйте команду [DOCKER Pull](https://docs.docker.com/engine/reference/commandline/pull/) , чтобы скачать образ контейнера из реестра предварительного просмотра контейнера.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest
```

> [!NOTE]
> `locale` `voice` Пользовательские контейнеры и для пользовательских речевых контейнеров определяются настраиваемой моделью, принимаемой контейнером.

***

## <a name="how-to-use-the-container"></a>Использование контейнера

После размещения контейнера на [главном компьютере](#the-host-computer) воспользуйтесь следующей процедурой для работы с ним.

1. [Запустите контейнер](#run-the-container-with-docker-run) с необходимыми настройками выставления счетов. Доступны дополнительные [примеры](speech-container-configuration.md#example-docker-run-commands) команды `docker run`.
1. [Запросите конечную точку прогнозирования контейнера](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Запуск контейнера с помощью команды `docker run`

Воспользуйтесь командой [docker run](https://docs.docker.com/engine/reference/commandline/run/) для запуска контейнера. Дополнительные сведения о том, как получить значения и, см. в разделе [сбор обязательных параметров](#gathering-required-parameters) `{Endpoint_URI}` `{API_Key}` . Также [examples](speech-container-configuration.md#example-docker-run-commands) доступны дополнительные примеры `docker run` команды.

# <a name="speech-to-text"></a>[Преобразование речи в текст](#tab/stt)

Чтобы запустить контейнер преобразования *речи в текст* , выполните следующую `docker run` команду.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Эта команда:

* Запускает контейнер преобразования *речи в текст* из образа контейнера.
* Выделяет 4 ядра ЦП и 4 гигабайта (ГБ) памяти.
* предоставляет TCP-порт 5000 и выделяет псевдотелетайп для контейнера;
* автоматически удаляет контейнер после завершения его работы. Образ контейнера остается доступным на главном компьютере.


#### <a name="analyze-sentiment-on-the-speech-to-text-output"></a>Анализ тональности на выходе из речи в текст 

Начиная с версии v 2.2.0 контейнера для преобразования речи в текст можно вызвать [API тональности Analysis v3](../text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md) на выходе. Для вызова анализа тональности требуется конечная точка ресурса API анализа текста. Пример: 
* `https://westus2.api.cognitive.microsoft.com/text/analytics/v3.0-preview.1/sentiment`
* `https://localhost:5000/text/analytics/v3.0-preview.1/sentiment`

Если вы обращаетесь к конечной точке текстовой аналитики в облаке, потребуется ключ. Если вы работаете Анализ текста локально, вам может не потребоваться его указывать.

Ключ и конечная точка передаются в контейнер речи в качестве аргументов, как показано в следующем примере.

```bash
docker run -it --rm -p 5000:5000 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
CloudAI:SentimentAnalysisSettings:TextAnalyticsHost={TEXT_ANALYTICS_HOST} \
CloudAI:SentimentAnalysisSettings:SentimentAnalysisApiKey={SENTIMENT_APIKEY}
```

Эта команда:

* Выполняет те же действия, что и приведенная выше команда.
* Хранит API анализа текстаную конечную точку и ключ для отправки запросов анализа тональности. 


# <a name="custom-speech-to-text"></a>[Пользовательское распознавание речи к тексту](#tab/cstt)

Пользовательское распознавание речи контейнер полагается на пользовательскую модель речевого *ввода* . Пользовательская модель должна быть [обучена](how-to-custom-speech-train-model.md) с помощью [пользовательского портала речевого](https://speech.microsoft.com/customspeech)ввода.

> [!IMPORTANT]
> Модель Пользовательское распознавание речи должна быть обучена одной из следующих версий модели:
> * **20181201 (унифицированная версия 3.3)**
> * **20190520 (версия 4.14 Unified)**
> * **20190701 (версия 4.17 Unified)**<br>
> ![Пользовательское распознавание речи обучение модели контейнера](media/custom-speech/custom-speech-train-model-container-scoped.png)

Для запуска контейнера требуется **идентификатор пользовательской модели** распознавания речи. Его можно найти на странице " **обучение** " пользовательского речевого портала. На настраиваемом портале речевого ввода перейдите на страницу **обучения** и выберите модель.
<br>

![Страница обучения пользовательской речи](media/custom-speech/custom-speech-model-training.png)

Получите **идентификатор модели** , который будет использоваться в качестве аргумента для `ModelId` параметра `docker run` команды.
<br>

![Сведения о пользовательской модели речи](media/custom-speech/custom-speech-model-details.png)

В следующей таблице представлены различные `docker run` Параметры и соответствующие им описания.

| Параметр | Описание |
|---------|---------|
| `{VOLUME_MOUNT}` | Узел [тома](https://docs.docker.com/storage/volumes/)главного компьютера, который DOCKER использует для сохранения настраиваемой модели. Например, *к:\кустомспич* , где *диск C* находится на хост-компьютере. |
| `{MODEL_ID}` | **Идентификатор модели** пользовательское распознавание речи на странице **обучения** пользовательского речевого портала. |
| `{ENDPOINT_URI}` | Для оценки и выставления счетов требуется конечная точка. Дополнительные сведения см. в разделе [сбор обязательных параметров](#gathering-required-parameters). |
| `{API_KEY}` | Требуется ключ API. Дополнительные сведения см. в разделе [сбор обязательных параметров](#gathering-required-parameters). |

Чтобы запустить контейнер *пользовательское распознавание речи в текст* , выполните следующую `docker run` команду:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Эта команда:

* Выполняет *пользовательское распознавание речиный* контейнер из образа контейнера.
* Выделяет 4 ядра ЦП и 4 гигабайта (ГБ) памяти.
* Загружает *пользовательское распознавание речиную* модель из подключения ввода тома, например *к:\кустомспич*.
* предоставляет TCP-порт 5000 и выделяет псевдотелетайп для контейнера;
* Загружает модель, заданную `ModelId` (если она не найдена в подключении тома).
* Если пользовательская модель была ранее скачана, параметр `ModelId` игнорируется.
* автоматически удаляет контейнер после завершения его работы. Образ контейнера остается доступным на главном компьютере.

# <a name="text-to-speech"></a>[Преобразование текста в речь](#tab/tts)

Чтобы запустить контейнер преобразования *текста в речь* , выполните следующую `docker run` команду.

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Эта команда:

* Выполняет контейнер преобразования *текста в речь* из образа контейнера.
* Выделяет 2 ядра ЦП и один гигабайт (ГБ) памяти.
* предоставляет TCP-порт 5000 и выделяет псевдотелетайп для контейнера;
* автоматически удаляет контейнер после завершения его работы. Образ контейнера остается доступным на главном компьютере.

# <a name="custom-text-to-speech"></a>[Пользовательский текст в речь](#tab/ctts)

Пользовательский контейнер преобразования *текста в речь* использует пользовательскую голосовую модель. Пользовательская модель должна быть [обучена](how-to-custom-voice-create-voice.md) с помощью [пользовательского Voice](https://aka.ms/custom-voice-portal). Для запуска контейнера требуется идентификатор настраиваемой **модели** голоса. Его можно найти на странице **обучение** на настраиваемом голосовом портале. На портале настраиваемого голоса перейдите на страницу **обучения** и выберите модель.
<br>

![Страница настраиваемого речевого обучения](media/custom-voice/custom-voice-model-training.png)

Получите **идентификатор модели** , который будет использоваться в качестве аргумента `ModelId` параметра команды DOCKER Run.
<br>

![Сведения о настраиваемой модели голоса](media/custom-voice/custom-voice-model-details.png)

В следующей таблице представлены различные `docker run` Параметры и соответствующие им описания.

| Параметр | Описание |
|---------|---------|
| `{VOLUME_MOUNT}` | Узел [тома](https://docs.docker.com/storage/volumes/)главного компьютера, который DOCKER использует для сохранения настраиваемой модели. Например, *к:\кустомспич* , где *диск C* находится на хост-компьютере. |
| `{MODEL_ID}` | **Идентификатор модели** пользовательское распознавание речи на странице " **обучение** " настраиваемого голоса Portal. |
| `{ENDPOINT_URI}` | Для оценки и выставления счетов требуется конечная точка. Дополнительные сведения см. в разделе [сбор обязательных параметров](#gathering-required-parameters). |
| `{API_KEY}` | Требуется ключ API. Дополнительные сведения см. в разделе [сбор обязательных параметров](#gathering-required-parameters). |

Чтобы запустить пользовательский контейнер преобразования *текста в речь* , выполните следующую `docker run` команду:

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Эта команда:

* Выполняет пользовательский контейнер преобразования *текста в речь* из образа контейнера.
* Выделяет 2 ядра ЦП и один гигабайт (ГБ) памяти.
* Загружает *настраиваемую модель преобразования текста в речь* из подключения ввода тома, например *к:\кустомвоице*.
* предоставляет TCP-порт 5000 и выделяет псевдотелетайп для контейнера;
* Загружает модель, заданную `ModelId` (если она не найдена в подключении тома).
* Если пользовательская модель была ранее скачана, параметр `ModelId` игнорируется.
* автоматически удаляет контейнер после завершения его работы. Образ контейнера остается доступным на главном компьютере.

***

> [!IMPORTANT]
> Для запуска контейнера необходимо указать параметры `Eula`, `Billing` и `ApiKey`. В противном случае контейнер не запустится.  Дополнительные сведения см. в [разделе о выставлении счетов](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Запрос конечной точки прогнозирования контейнера

> [!NOTE]
> Если вы используете несколько контейнеров, используйте уникальный номер порта.

| Контейнеры | URL-адрес узла SDK | Протокол |
|--|--|--|
| Преобразование речи в текст и Пользовательское распознавание речи в текст | `ws://localhost:5000` | WS |
| Преобразование текста в речь и пользовательский текст в речь | `http://localhost:5000` | HTTP |

Дополнительные сведения об использовании протоколов WSS и HTTPS см. в разделе [Безопасность контейнера](../cognitive-services-container-support.md#azure-cognitive-services-container-security).

[!INCLUDE [Query Speech-to-text container endpoint](includes/speech-to-text-container-query-endpoint.md)]

#### <a name="analyze-sentiment"></a>Анализ тональности

Если вы указали учетные данные API анализа текста в [контейнере](#analyze-sentiment-on-the-speech-to-text-output), вы можете использовать РЕЧЕВОЙ пакет SDK для отправки запросов распознавания речи с помощью анализа тональности. Можно настроить Ответы API для использования *простого* или *подробного* формата.

# <a name="simple-format"></a>[Простой формат](#tab/simple-format)

Чтобы настроить речевой клиент для использования простого формата, добавьте `"Sentiment"` в качестве значения `Simple.Extensions` . Если вы хотите выбрать конкретную версию модели Анализ текста, замените `'latest'` в `speechcontext-phraseDetection.sentimentAnalysis.modelversion` конфигурации свойства.

```python
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Simple.Extensions',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentAnalysis.modelversion',
    value='latest',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

`Simple.Extensions`вернет результат тональности в корневом слое ответа.

```json
{
   "DisplayText":"What's the weather like?",
   "Duration":13000000,
   "Id":"6098574b79434bd4849fee7e0a50f22e",
   "Offset":4700000,
   "RecognitionStatus":"Success",
   "Sentiment":{
      "Negative":0.03,
      "Neutral":0.79,
      "Positive":0.18
   }
}
```

# <a name="detailed-format"></a>[Подробный формат](#tab/detailed-format)

Чтобы настроить речевой клиент для использования подробного формата, добавьте `"Sentiment"` в качестве значения для параметров `Detailed.Extensions` , `Detailed.Options` или. Если вы хотите выбрать конкретную версию модели Анализ текста, замените `'latest'` в `speechcontext-phraseDetection.sentimentAnalysis.modelversion` конфигурации свойства.

```python
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Detailed.Options',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Detailed.Extensions',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentAnalysis.modelversion',
    value='latest',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

`Detailed.Extensions`предоставляет тональности результат в корневом слое ответа. `Detailed.Options`предоставляет результат в `NBest` слое ответа. Их можно использовать отдельно или вместе.

```json
{
   "DisplayText":"What's the weather like?",
   "Duration":13000000,
   "Id":"6a2aac009b9743d8a47794f3e81f7963",
   "NBest":[
      {
         "Confidence":0.973695,
         "Display":"What's the weather like?",
         "ITN":"what's the weather like",
         "Lexical":"what's the weather like",
         "MaskedITN":"What's the weather like",
         "Sentiment":{
            "Negative":0.03,
            "Neutral":0.79,
            "Positive":0.18
         }
      },
      {
         "Confidence":0.9164971,
         "Display":"What is the weather like?",
         "ITN":"what is the weather like",
         "Lexical":"what is the weather like",
         "MaskedITN":"What is the weather like",
         "Sentiment":{
            "Negative":0.02,
            "Neutral":0.88,
            "Positive":0.1
         }
      }
   ],
   "Offset":4700000,
   "RecognitionStatus":"Success",
   "Sentiment":{
      "Negative":0.03,
      "Neutral":0.79,
      "Positive":0.18
   }
}
```

---

Если вы хотите полностью отключить анализ тональности, добавьте `false` значение в `sentimentanalysis.enabled` .

```python
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentanalysis.enabled',
    value='false',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

### <a name="text-to-speech-or-custom-text-to-speech"></a>Преобразование текста в речь или пользовательский текст в речь

[!INCLUDE [Query Text-to-speech container endpoint](includes/text-to-speech-container-query-endpoint.md)]

### <a name="run-multiple-containers-on-the-same-host"></a>Запуск нескольких контейнеров на одном узле

Если вы планируете запускать несколько контейнеров при открытых портах, обязательно назначьте каждому контейнеру отдельный открытый порт. Например, запускайте первый контейнер на порте 5000, а второй — на порте 5001.

Этот контейнер и другой контейнер Azure Cognitive Services могут одновременно работать на одном и том же узле. Также одному запущенному контейнеру Cognitive Services могут соответствовать сразу несколько контейнеров.

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Остановка контейнера

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Устранение неполадок

При запуске или запуске контейнера могут возникнуть проблемы. Используйте [Подключение](speech-container-configuration.md#mount-settings) к выходным данным и включите ведение журнала. Это позволит контейнеру создавать файлы журналов, которые полезны при устранении неполадок.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Выставление счетов

Контейнеры распознавания речи отправляют сведения о выставлении счетов в Azure с помощью *речевого* ресурса в учетной записи Azure.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Дополнительные сведения об этих параметрах см. в статье [Настройка контейнеров](speech-container-configuration.md).

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Сводка

В этой статье вы узнали основные понятия и рабочий процесс по скачиванию, установке и запуску речевых контейнеров. В разделе "Сводка" сделайте следующее.

* Речь предоставляет четыре контейнера Linux для DOCKER, включая различные возможности:
  * *Преобразование речи в текст*
  * *Пользовательское распознавание речи к тексту*
  * *Преобразование текста в речь*
  * *Пользовательский текст в речь*
* Образы контейнеров загружаются из реестра контейнеров в Azure.
* Образы контейнеров выполняются в Docker.
* Независимо от того, используете ли вы REST API (только преобразование текста в речь) или пакет SDK (преобразование речи в текст или преобразование текста в речь), укажите URI узла контейнера. 
* При создании экземпляра контейнера необходимо указать сведения о выставлении счетов.

> [!IMPORTANT]
>  Контейнеры Cognitive Services не лицензируются для запуска без подключения к Azure для отслеживания использования. Клиенты должны разрешить контейнерам непрерывную передачу данных для выставления счетов в службу контроля потребления. Контейнеры Cognitive Services не отправляют в корпорацию Майкрософт данные клиента (например, анализируемые изображения или тексты).

## <a name="next-steps"></a>Дальнейшие действия

* Проверьте настройки [контейнеров](speech-container-configuration.md) на наличие параметров конфигурации.
* Узнайте, как [использовать контейнеры службы речи с Kubernetes и Helm](speech-container-howto-on-premises.md)
* Использование большего числа [контейнеров Cognitive Services](../cognitive-services-container-support.md)
