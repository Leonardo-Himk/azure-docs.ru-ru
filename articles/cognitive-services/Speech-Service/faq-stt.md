---
title: Часто задаваемые вопросы о преобразовании речи в текст
titleSuffix: Azure Cognitive Services
description: Получите ответы на часто задаваемые вопросы о службе распознавания речи в тексте.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/4/2019
ms.author: panosper
ms.openlocfilehash: a279aebdd19ebd3a41ddad0c1c279937e00838c2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "77168467"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Часто задаваемые вопросы о преобразовании речи в текст

Если в этой статье вы не нашли ответы на свои вопросы, ознакомьтесь с [другими вариантами поддержки](support.md).

## <a name="general"></a>Общие

**Вопрос. Чем отличаются базовая и пользовательская модели преобразования речи в текст?**

**Ответ**. Базовая модель заранее обучена по данным, принадлежащим корпорации Майкрософт, и уже развернута в облаке. Вы можете создать пользовательскую модель под конкретную среду с известным уровнем шума и (или) особенностями языка. Например, адаптированная акустическая модель будет полезна в заводских цехах, в автомобиле или на шумной улице. Для таких тем, как биология, физика, радиология, а также при частом использовании названий продуктов и (или) нетипичных сокращений, следует создать адаптированную языковую модель.

**Вопрос. С чего лучше начинать работу с базовой моделью?**

**Ответ**. Прежде всего получите [ключ подписки](get-started.md). Если вы хотите использовать вызовы REST к уже развернутым базовым моделям, изучите инструкции [по REST API](rest-apis.md). Если вы планируете использовать WebSocket, скачайте [этот пакет SDK](speech-sdk.md).

**Вопрос. Всегда ли мне нужна пользовательская модель речи?**

Ответ **. нет**. Если ваше приложение использует типичную повседневную речь, вам не обязательно персонализировать модель. Если приложение будет использоваться в среде без значительного фонового шума, вам не обязательно персонализировать модель.

Вы можете развертывать базовые и пользовательские модели на портале, а затем проверять их точность. Такая возможность позволяет сравнить точность базовой и пользовательской моделей.

**Вопрос. Как мне узнать, когда завершится обработка набора данных и (или) модели?**

**Ответ**. Сейчас это можно проверить только по состоянию модели или набора данных, которое отображается в таблице. Когда обработка завершится, там будет отображено состояние **Успешно**.

**Вопрос. Можно ли создать больше одной модели?**

**Ответ**. Не существует ограничений на число моделей, размещенных в коллекции.

**Вопрос. я понял, что я сделал ошибку. Разделы справки отменить импорт данных или создание модели?**

**Ответ**. Сейчас возможность отмены для процессов акустической или языковой адаптации не поддерживается. Вы сможете удалить импортированные данные и модели после достижения конечного состояния.

**Вопрос. Каковы различия между моделью поиска и диктовки и разговорной моделью?**

**Ответ**. В службе "Речь" вы можете выбрать несколько базовых моделей. Разговорная модель полезна для распознавания речи при обычном общении. Она идеально подходит для расшифровки телефонных звонков. Модель поиска и диктовки идеально подходит для приложений с голосовым управлением. Новая "Универсальная" модель должна поддерживать сразу оба этих сценария. "Универсальная" модель находится на уровне или выше уровня качества разговорной модели в большинстве регионов.

**Вопрос. Можно ли обновить имеющуюся модель (распределение модели)?**

**Ответ**. Вы не сможете обновить существующую модель. Но вы можете объединить старый набор данных с новым и выполнить повторную адаптацию.

Старый и новый наборы данных следует объединить в один ZIP-файл (для акустических данных) или TXT-файл (для языковых данных). Когда адаптация завершится, обновленную модель следует заново развернуть, чтобы получить новую конечную точку.

**Вопрос. когда доступна новая версия базового плана, автоматически ли обновляется мое развертывание?**

**Ответ**. Развертывания НЕ обновляются автоматически.

Если вы адаптировали и развернули модель с базовым планом версии 1.0, такое развертывание не будет изменяться. Клиенты могут списать развернутую модель, повторно адаптировать ее с помощью новой версии базовых показателей и повторного развертывания.

**Вопрос. Как мне скачать модель для локального выполнения?**

**Ответ**. Модели нельзя скачивать и выполнять локально.

**Вопрос. Ведется ли журнал моих запросов?**

**Ответ**. При создании развертывания вы можете отключить трассировку. После этого данные аудио и (или) транскрипций сохраняться не будут. В противном случае все запросы обычно сохраняются в защищенное хранилище в Azure.

**Вопрос. Мои запросы регулируются?**

**Ответ**. REST API ограничивает количество запросов до 25 за 5 секунд. Подробные сведения можно найти на наших страницах о [преобразовании речи в текст](speech-to-text.md).

**Вопрос. как взимается система двустороннего аудио?**

Ответ **. при**отправке каждого канала по отдельности (каждый канал в отдельном файле) будет оплачено время каждого файла. Если отправить один файл со всеми коммутаторами, которые были объединены в один канал, будет выставляться счет за длительность одного файла. Дополнительные сведения о ценах см. на [странице цен на Cognitive Services Azure](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Если у вас есть требования к конфиденциальности, которые запрещают вам использовать пользовательскую службу распознавания речи, отправьте запрос по одному из каналов поддержки.

## <a name="increasing-concurrency"></a>Повышение параллелизма

**Вопрос. Что делать, если мне нужно большее число одновременно развернутых моделей, чем предлагается на портале?**

**Ответ**. Вы можете масштабировать модель, добавляя по 20 одновременно выполняемых запросов.

С необходимыми сведениями создайте запрос в службу поддержки на [портале поддержки Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). Не размещайте сведения на всех общедоступных каналах (GitHub, StackOverflow,...), указанных на [странице поддержки](support.md).

Чтобы увеличить параллелизм для ***пользовательской модели***, необходимы следующие сведения:

- Регион, в котором развернута модель,
- Идентификатор конечной точки развернутой модели:
  - Получен на [портал пользовательское распознавание речи](https://aka.ms/customspeech)
  - вход (при необходимости),
  - Выберите проект и развертывание,
  - Выберите конечную точку, для которой требуется увеличение параллелизма,
  - Скопируйте `Endpoint ID`.

Чтобы увеличить параллелизм для ***базовой модели***, необходимы следующие сведения:

- Регион службы,

и либо

- маркер доступа для подписки (см. [здесь](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text#how-to-get-an-access-token)),

или диспетчер конфигурации служб

- Идентификатор ресурса для вашей подписки:
  - Перейдите к [портал Azure](https://portal.azure.com)
  - в `Cognitive Services` поле поиска выберите
  - в отображаемых службах выберите службу распознавания речи, для которой требуется увеличить параллелизм.
  - отобразить `Properties` для этой службы
  - Скопируйте полный `Resource ID`.

## <a name="importing-data"></a>импорт данных

**Вопрос. Какое есть ограничение на размер набора данных, и чем оно обусловлено?**

**Ответ**. В настоящее время ограничение на размер набора данных составляет 2 ГБ. Оно объясняется ограничением на размер файла для передачи по HTTP.

**Вопрос. Могу ли я сжать текстовые файлы, чтобы загрузить больший размер?**

Ответ **. нет**. В настоящее время поддерживаются только текстовые файлы без сжатия.

**Вопрос. в отчете данных указано, что фразы продолжительностью не удалось. В чем заключается эта ошибка?**

**Ответ**. Загрузка менее чем 100 % фраз из файла не является проблемой. Если подавляющее большинство фраз в наборе акустических или языковых данных (например, более 95 %) успешно импортированы, набор данных можно использовать. Но мы советуем в любом случае выяснить, почему эти высказывания не удалось обработать, и устранить проблемы. Самые распространенные проблемы, например ошибки форматирования, исправляются легко.

## <a name="creating-an-acoustic-model"></a>Создание акустической модели

**Вопрос. Сколько мне нужно акустических данных?**

**Ответ**. Мы рекомендуем для начала подготовить от 30 минут до одного часа акустических данных.

**Вопрос. Какие данные следует собирать?**

**Ответ**. Собирайте такие данные, которые наиболее точно соответствуют сценарию использования приложения и реальной работы. Сбор данных должен соответствовать целевому приложению и пользователям в контексте устройств, сред и типов говорящих. В общем, следует собирать данные из как можно более широкого диапазона говорящих.

**Вопрос. Как следует собирать акустические данные?**

**Ответ**. Вы можете создать автономное приложение для сбора данных или использовать готовые программы для записи аудио. Также можно создать отдельную версию основного приложения, которая сохраняет аудиоданные и использует их.

**Вопрос. Нужно ли самостоятельно транскрибировать данные адаптации?**

Ответ **. Да**! Их можно транскрибировать самостоятельно или с помощью специальной службы для транскрибирования. Некоторые пользователи предпочитают поручить эту задачу профессионалам, а другие применяют краудсорсинг или делают все самостоятельно.

## <a name="accuracy-testing"></a>Тестирование точности

**Вопрос. Могу лия выполнить автономное тестирование пользовательской акустической модели с помощью пользовательской языковой модели?**

**Ответ**. Да, для этого достаточно выбрать пользовательскую языковую модель в раскрывающемся списке при настройке автономного тестирования.

**Вопрос. Могу лия выполнить автономное тестирование пользовательской языковой модели с помощью пользовательской акустической модели?**

**Ответ**. Да, для этого достаточно выбрать пользовательскую акустическую модель в раскрывающемся списке при настройке автономного тестирования.

**Вопрос. Что такое пословная вероятность ошибки (WER) и как она вычисляется?**

**Ответ**. Пословная вероятность ошибки — это метрика для оценки качества распознавания речи. WER подсчитывается как общее число ошибок, включая вставки, удаления и замены, деленное на общее число слов в референтной транскрипции. Подробнее этот вопрос раскрыт в [этом описании](https://en.wikipedia.org/wiki/Word_error_rate).

**Вопрос. Как мне оценить результаты теста на точность?**

**Ответ**. Эти результаты демонстрируют сравнение между базовой и пользовательской моделями. Чтобы персонализация имела смысл, нужен результат не хуже, чем у базовой модели.

**Вопрос. Как определить WER для базовой модели, чтобы оценить наличие улучшений?**

**Ответ**. Результаты изолированного тестирования демонстрируют точность базовой модели, точность пользовательской модели и улучшение по сравнению с базовой моделью.

## <a name="creating-a-language-model"></a>Создание языковой модели

**Вопрос. Сколько текстовых данных нужно отправить?**

**Ответ**. Это зависит от того, насколько словарный запас и фразы, используемые в приложении, отличаются от исходных языковых моделей. Для всех новых слов желательно предоставить как можно больше примеров использования. Для обычных фраз, которые используются в приложении, включая фразы в языковых данных, примеры также будут полезны — они сообщат системе, за чем еще нужно следить. Обычно наборы языковых данных содержат не менее 100 фраз, а еще лучше — несколько сотен. Если же ожидается, что некоторые типы запросов будут встречаться чаще других, добавьте в набор данных несколько копий этих "типичных" запросов.

**Вопрос. Могу ли я просто передать список слов?**

**Ответ**. Слова, отправленные простым списком, будут помещены в словарь, но это не даст системе информации об их типичном использовании. При наличии полных или частичных высказываний (предложений или фраз, которые обычно говорят пользователи) языковая модель сможет изучать новые слова и особенности их применения. Пользовательская языковая модель полезна не только для отправки в систему новых слов, но и для повышения вероятности узнавания в приложении уже известных слов. Система обучается лучше при использовании полных фраз.

## <a name="tenant-model-custom-speech-with-office-365-data"></a>Модель клиента (Пользовательское распознавание речи с данными Office 365)

**Вопрос. какие сведения включаются в модель клиента и как она создается?**

Ответ **.** Модель клиента создается с помощью [общедоступной группы](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2) сообщений электронной почты и документов, которые могут просматривать все пользователи в Организации.

**Вопрос. какие возможности распознавания речи улучшены в модели клиента?**

Ответ **.** Когда модель клиента включена, создается и публикуется, она используется для улучшения распознавания любых корпоративных приложений, созданных с помощью службы распознавания речи. Это также передает маркер AAD пользователя, который указывает на принадлежность к Организации.

Речевые функции, встроенные в Office 365, такие как Диктовка и субтитры PowerPoint, не изменяются при создании модели клиента для приложений службы распознавания речи.

## <a name="next-steps"></a>Следующие шаги

- [Устранение неполадок](troubleshooting.md)
- [Заметки о выпуске](releasenotes.md)
