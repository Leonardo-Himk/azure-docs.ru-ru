---
title: включить файл
description: включить файл
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 05/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e7a0424e4454639211c6494aab0700e75269361
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83721032"
---
Следующие ограничения применяются к пользовательским и системным разделам Сетки событий Azure, а *не* к доменам событий.

| Ресурс | Ограничение |
| --- | --- |
| Количество пользовательских разделов на подписку Azure | 100 |
| Количество подписок на события на раздел | 500 |
| Скорость публикации пользовательского раздела (входящий трафик) | 5000 событий на раздел в секунду |
| Размер события | 1 МБ, но они обрабатываются пакетами по 64 КБ, поэтому события объемом свыше 64 КБ обрабатываются как несколько событий. Так, событие объемом 130 КБ будет обработано как три отдельных события.  |
| Количество разделов на домен событий | 100 000 |
| Количество подписок на события на раздел в домене | 500 |
| Количество подписок на события области домена | 50 |
| Скорость публикации для домена событий (входящий трафик) | 5000 Событий за секунду |
| Количество доменов событий на подписку Azure | 100 |
| Подключения к частной конечной точке на раздел или домен | 64 | 
| Правила брандмауэра для IP-адресов на раздел или домен | 16 | 