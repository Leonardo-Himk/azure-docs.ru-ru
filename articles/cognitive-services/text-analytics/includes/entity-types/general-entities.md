---
title: Общие именованные сущности
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: aahi
ms.openlocfilehash: 32e80c50ff6f543679852cbd7e5ce9bda92d01e1
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140952"
---
При отправке запросов в конечную точку возвращаются следующие категории сущностей `/entities/recognition/general` .

| Категория   | Подкатегория | Описание:                          | Начальная версия модели                                                    | Примечания |
|------------|-------------|--------------------------------------|-------------------------------------------------------------|--------------------------------------|
| Модель Person     | Недоступно         | Имена людей.  | `2019-10-01`  | Также возвращается NER версии 2.1 |
| персонтипе | Недоступно         | Типы заданий или роли, удерживаемые человеком. | `2020-02-01` | |
|Расположение    | Недоступно         | Естественные и человеческие ориентиры, структуры, географические функции и геофункциональные объекты     |  `2019-10-01` | Также возвращается NER версии 2.1 |
|Расположение     | Геоадминистративная сущность (ГПЕ)        | Города, страны и регионы, Штаты.      | `2020-02-01` | |
|Расположение     | Структурное                       | Структуры Манмаде. | `2020-04-01` | |
|Расположение     | Географических       | Географические и естественные функции, такие как рек, океанские и собственные. |  `2020-04-01` | |
|Организация  | Недоступно | Компании, «неправительственные группы», «музыкальные зоны», «спорт», «государственные органы» и «общественные организации».  | `2019-10-01` | Национальные и религионсы не включаются в этот тип сущности. Также возвращается NER версии 2.1 |
|Организация | Медицина | Медицинские компании и группы. | `2020-04-01` |  |
|Организация | Обмен на фондовой бирже | Группы обмена фондовой биржи. | `2020-04-01` | |
| Организация | Спорт | Организации, связанные с спортивными делами. | `2020-04-01` |  |
| Событие  | Недоступно | Исторические, социальные и естественным образом происходящие события. | `2020-02-01` |  |
| Событие  | Различия | Культурные мероприятия и праздники. | `2020-04-01` | |
| Событие  | Natural | Естественное возникновение событий. | `2020-04-01` |  |
| Событие  | Спорт | Спортивные мероприятия.  | `2020-04-01` | |
| Продукт | Недоступно | Физические объекты различных категорий. | `2020-02-01` | |
| Продукт | Вычисление продуктов | Вычисление продуктов. |  `2020-02-01 ` | |
| Навык | Недоступно | Возможность, навык или опыт. | `2020-02-01` |  |
| Адрес | Недоступно | Полные адреса электронной почты.  | `2020-04-01` |  |
| PhoneNumber | Недоступно | Номера телефонов (только для телефонных номеров США и ЕС). | `2019-10-01` | Также возвращается NER версии 2.1 |
| Адрес электронной почты | Недоступно | Адреса электронной почты. | `2019-10-01` | Также возвращается NER версии 2.1 |
| URL-адрес | Недоступно | URL-адреса веб-сайтов. | `2019-10-01` | Также возвращается NER версии 2.1  |
| IP-адрес | Недоступно | Сетевые IP-адреса. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Дата и время | Недоступно | Даты и время суток. | `2019-10-01` | Также возвращается NER версии 2.1 | 
| Дата и время | Дата | Календарные даты. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Дата и время | Время | Время суток | `2019-10-01` | Также возвращается NER версии 2.1 |
| Дата и время | Диапазон дат | Диапазоны дат. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Дата и время | Диапазон времени | Диапазоны времени. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Дата и время | Длительность | Длительности. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Дата и время | Присвойте параметру | Набор, повторяющиеся значения времени. |  `2019-10-01` | Также возвращается NER версии 2.1 |
| Количество | Недоступно | Числа и числовые величины. | `2019-10-01` | Также возвращается NER версии 2.1  |
| Количество | число; | числа. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Количество | Процент | Процентов.| `2019-10-01` | Также возвращается NER версии 2.1 |
| Количество | Ordinal | Порядковые номера. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Количество | Возраст | Устаревают. | `2019-10-01` |  Также возвращается NER версии 2.1 |
| Количество | Валюта | Друг. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Количество | Измерение | Измерения и измерения. | `2019-10-01` | Также возвращается NER версии 2.1 |
| Количество | Температура | Измеряем. | `2019-10-01` | Также возвращается NER версии 2.1 |
