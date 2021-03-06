---
title: Локализация Проигрыватель мультимедиа Azure
description: Поддержка нескольких языков для пользователей языковых стандартов, отличных от английского.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: ca4dc888af414ede270118eff72652f098d3306c
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2020
ms.locfileid: "82779048"
---
# <a name="localization"></a>Локализация #

Поддержка нескольких языков позволяет пользователям, не являющимся английским языком, взаимодействовать с проигрывателем. Проигрыватель мультимедиа Azure будет создавать экземпляр с глобальным словарем языков, который будет локализовать сообщения об ошибках на основе языка страницы. Разработчик также может создать экземпляр проигрывателя с принудительно заданным языком. По умолчанию используется английский язык (EN).

> [!NOTE]
> Эта функция все еще проходит проверку, поэтому она может быть связана с ошибками.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" data-setup='{"language":"es"}'>...</video>
```

В настоящее время Проигрыватель мультимедиа Azure поддерживает следующие языки с соответствующими кодами языков:

| Язык            | Код | Язык                | Код   | Язык                | Код         |
|---------------------|------|-------------------------|--------|-------------------------|--------------|
| Английский {по умолчанию}   | en   | Хорватский                | hr     | Румынский                | ro           |
| Арабский              | ar   | Венгерский               | hu     | Словацкий                  | sk           |
| Болгарский           | bg   | Индонезийский              | идентификатор     | Slovene                 | sl           |
| Каталонский             | Корнев   | Исландский               | — это     | Сербский — кириллица      | SR-Цирл-CS   |
| Чешский               | cs   | Итальянский                 | it     | Сербский — латиница         | sr-latn-rs   |
| Датский              | da   | Японский                | ja     | Русский                 | ru           |
| Немецкий              | de   | Казахский                  | kk     | Шведский                 | sv           |
| Греческий               | el   | Корейский                  | ko     | Тайский                    | th           |
| Испанский             | es   | Литовский              | lt     | Тагальский                 | TL           |
| Эстонский            | et   | Латышский                 | lv     | Турецкий                 | tr           |
| Баскский              | eu   | Малайзийский               | ms     | Украинский               | uk           |
| Фарси               | fa   | Норвежский — Бокмã ¥ l     | nb     | Урду                    | ur           |
| Финский             | fi   | Нидерландский                   | nl     | Вьетнамский              | vi           |
| Французский              | fr   | Норвежский — нюнорск     | nn     | Китайский (упрощенное письмо)    | zh-hans      |
| Галисийский            | gl   | Польский                  | pl     | Китайский (традиционное письмо)   | zh-hant      |
| Иврит              | he   | Португальский (Бразилия)     | pt-br  |                         |              |
| Hindi               | hi   | Португальский (Португалия)   | pt-pt  |                         |              |


> [!NOTE]
> Если не требуется выполнять локализацию, необходимо принудительно использовать английский язык

## <a name="next-steps"></a>Дальнейшие действия ##

- [Краткое руководство. Проигрыватель мультимедиа Azure](azure-media-player-quickstart.md)
