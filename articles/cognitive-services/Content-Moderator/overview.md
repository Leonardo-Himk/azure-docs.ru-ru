---
title: Что такое Azure Content Moderator?
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать Content Moderator для отслеживания, пометки, оценки и фильтрации неподходящего материала, создаваемого пользователями.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: f28f2bcf5d04c9a6354b8135bd39546b9d8b9bf3
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81404300"
---
# <a name="what-is-azure-content-moderator"></a>Что такое Azure Content Moderator?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Content Moderator — это когнитивная служба, которая проверяет текст, изображения и видео на наличие потенциально оскорбительных, представляющих риск или нежелательных по иным причинам материалов. При обнаружении таких материалов служба применяет к содержимому соответствующие метки (флаги). Затем приложение может обрабатывать помеченное содержимое для обеспечения соответствия нормативным требованиям или оставлять его как предполагаемую среду для пользователей. Сведения о том, что означают различные флаги содержимого, см. в разделе об [API модерации](#moderation-apis).

## <a name="where-its-used"></a>Сценарии использовании

Ниже приведены несколько сценариев, в которых разработчик программного обеспечения или команда используют Content Moderator:

- интернет-магазины с модерацией каталогов продукции и создаваемого пользователями содержимого;
- компании-разработчики игр, которые модерируют создаваемые пользователями игровые артефакты и чаты;
- социальные платформы для обмена сообщениями с модерацией изображений, текста и видео, добавляемых пользователями;
- корпоративные медиакомпании, внедряющие централизованную модерацию своего содержимого;
- поставщики образовательных решений K-12, фильтрующие неуместное содержимое для учащихся и преподавателей.

> [!NOTE]
> Вы не можете использовать Content Moderator для проверки изображений на наличие сцен противозаконной эксплуатации детей. При этом уполномоченные организации могут использовать для этих целей [облачную службу PhotoDNA](https://www.microsoft.com/photodna "Облачная служба Microsoft PhotoDNA").

## <a name="what-it-includes"></a>Компоненты

Служба Content Moderator состоит из нескольких API веб-службы, доступных через вызовы REST и пакет SDK для .NET. Она также содержит средство проверки, с помощью которого человек может помочь службе и улучшить или оптимизировать ее функцию модерации.

## <a name="moderation-apis"></a>API модерации

В службу Content Moderator входят API модерации, которые проверяют содержимое на наличие потенциально неприемлемых или нежелательных материалов.

![Блок-схема API модерации в Content Moderator](images/content-moderator-mod-api.png)

Различные типы API модерации описаны в следующей таблице.

| Группа API | Description |
| ------ | ----------- |
|[**Модерация текста**](text-moderation-api.md)| Просматривает текст на наличие оскорбительного содержимого, содержимого сексуального или непристойного характера, содержимого с ненормативной лексикой и персональными данными.|
|[**Пользовательские списки терминов**](try-terms-list-api.md)| Проверяет текст с использованием настраиваемого списка терминов наряду со встроенным списком. Используйте пользовательские списки для блокировки или разрешения содержимого в соответствии с собственными политиками содержимого.|  
|[**Модерация изображений**](image-moderation-api.md)| Проверяет изображения на наличие непристойного содержимого или содержимого для взрослых, выявляет в изображениях текст с помощью функции распознавания текста, а также распознает лица.|
|[**Пользовательские списки изображений**](try-image-list-api.md)| Проверяет изображения с использованием пользовательского списка изображений. Используйте пользовательский список изображений для фильтрации экземпляров часто встречающегося содержимого, которое вы не хотите классифицировать еще раз.|
|[**Модерация видео**](video-moderation-api.md)| Проверяет видео на наличие непристойного содержимого или содержимого для взрослых и возвращает метки времени для указанного содержимого.|

## <a name="review-apis"></a>Просмотр API-интерфейсов

API проверки позволяют интегрировать работу конвейера модерации и модераторов-людей. Используйте операции [Задания](review-api.md#jobs), [Проверки](review-api.md#reviews) и [Рабочий процесс](review-api.md#workflows) для создания и автоматизации рабочих процессов с участием человека при помощи [средства проверки](#review-tool) (см. ниже).

> [!NOTE]
> API рабочих процессов еще недоступен в пакете SDK для .NET, но его уже можно использовать с конечной точкой REST.

![Блок-схема API проверки в Content Moderator](images/content-moderator-rev-api.png)

## <a name="review-tool"></a>Средство проверки

Служба Content Moderator также имеет [средство проверки](Review-Tool-User-Guide/human-in-the-loop.md), в котором находятся результаты проверки для обработки людьми. Вводимые людьми данные не обучают службу, но совместное использование службы и команд настраиваемой проверки позволяет разработчикам найти баланс между эффективностью и точностью. Средство проверки также имеет удобный интерфейс, в котором доступны разные ресурсы Content Moderator.

![Домашняя страница средства проверки Content Moderator](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Конфиденциальность и безопасность данных

Как и в случае со всеми другими Cognitive Services, разработчикам, использующим службу Content Moderator, следует учитывать политику корпорации Майкрософт касательно клиентских данных. Дополнительные сведения см. на [странице о Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) Центра управления безопасностью Майкрософт.

## <a name="next-steps"></a>Дальнейшие действия

Инструкции по началу работы со службой Content Moderator см. в статье [Краткое руководство. Знакомство с Content Moderator](quick-start.md).
