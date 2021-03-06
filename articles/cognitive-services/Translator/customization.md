---
title: Настройка перевода — переводчик
titleSuffix: Azure Cognitive Services
description: Используйте Microsoft Translator Hub для создания собственной системы машинного перевода с нужными вам терминами и стилем.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 6db43300632ec5b2c4f6c18848442901a40561b0
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2020
ms.locfileid: "83997004"
---
# <a name="customize-your-text-translations"></a>Настройка переводов текста

Пользовательский Переводчик — это функция службы переводчиков, которая позволяет пользователям настраивать расширенный перевод нейронных компьютеров Microsoft Translator при преобразовании текста с помощью переводчика (только версия 3).

Этот компонент также можно использовать для настройки перевода речи с использованием [службы "Речь" в Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Пользовательский переводчик

Custom Translator позволяет создавать нейронные системы перевода, которые понимают особую терминологию вашего бизнеса и отрасли. Настраиваемые системы перевода могут интегрироваться с существующими приложениями, рабочими процессами и веб-сайтами.

### <a name="how-does-it-work"></a>Принципы работы

Используйте ранее переведенные документы (леафлетс, веб-страницы, документацию и т. д.), чтобы создать систему перевода, отражающую терминологию и стиль, характерные для конкретного домена, лучше, чем стандартная система перевода. Пользователи могут отправлять документы TMX, XLIFF, TXT, DOCX и XLSX.  

Эта система также принимает данные, которые параллельны на уровне документов, но еще не согласованы на уровне предложений. Если пользователи имеют доступ к разным версиям одного содержимого на нескольких языках, представленным в отдельных документах, Custom Translator сможет автоматически сопоставить предложения между этими документами.  Система также может использовать данные на одном языке (на любом из языков пары или на обеих сразу) в дополнение к параллельным обучающим данным для улучшения переводов.

Затем настроенная система становится доступной через обычный вызов переводчика с помощью параметра Category.

При наличии нужного объема и правильного типа данных для обучения, совершенно нормальным можно считать улучшение качества перевода на 5–10 позиций или даже увеличения показателя BLEU благодаря Custom Translator.

Дополнительные сведения о различных уровнях настройки на основе доступных данных можно найти в [руководстве пользователя по Custom Translator](https://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Translator Hub

> [!NOTE]
> Устаревший центр Microsoft Translator будет снят с 17 мая 2019 г. [Просмотр важных сведений о миграции и дат](https://www.microsoft.com/translator/business/hub/).  

## <a name="custom-translator-versus-hub"></a>Сравнение Custom Translator и центра

|   | **Концентратор** | **Пользовательский переводчик**|
|:-----|:----:|:----:|
|Состояние компонента настройки    | Общедоступная версия    | Общедоступная версия |
| Версия API перевода текстов    | Только версия 2    | Только версия 3 |
| Настройка SMT    | Да    | Нет |
| Настройка NMT    | Нет    | Да |
| Настройка новых единых служб распознавания речи    | Нет    | Да |
| [Без трассировки](https://www.aka.ms/notrace) | Да    | Да |

## <a name="collaborative-translations-framework"></a>Платформа совместной работы над переводами

> [!NOTE]
> По состоянию на 1 февраля 2018 г. Аддтранслатион () и Аддтранслатионаррай () больше не доступны для использования с Translator v 2.0. Эти методы возвращают ошибку и ничего не записывают. Переводчик версии 3.0 не поддерживает эти методы.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Настройка системы с учетом особенностей языка с помощью Custom Translator](https://aka.ms/CustomTranslatorDocs)
