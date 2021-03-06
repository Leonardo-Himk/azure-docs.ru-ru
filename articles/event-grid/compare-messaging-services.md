---
title: Сравнение служб обмена сообщениями Azure
description: 'Описаны три службы обмена сообщениями Azure: Сетка событий, Центры событий и Служебная шина Azure. Рекомендации по выбору служб для различных сценариев.'
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: overview
ms.date: 10/22/2019
ms.author: spelluru
ms.custom: seodec18
ms.openlocfilehash: 6122f17637e76f42cc4fbcc87ac9f48da3cdca36
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2020
ms.locfileid: "76122208"
---
# <a name="choose-between-azure-messaging-services---event-grid-event-hubs-and-service-bus"></a>Сравнение трех служб обмена сообщениями Azure: Сетка событий, Центры событий и Служебная шина

Azure предлагает три службы, с помощью которых реализуется доставка сообщений о событиях в решениях. Такими службами являются:

* [Сетка событий](/azure/event-grid/)
* [Центры событий](/azure/event-hubs/)
* [Служебная шина](/azure/service-bus-messaging/)

Несмотря на то что они имеют некоторые сходства, каждая служба предназначена для определенного сценария. С помощью этой статьи вы узнаете о различиях между этими службами, а также определите, какую из них следует выбрать для своего приложения. Во многих случаях службы обмена сообщениями дополняют друг друга и могут использоваться совместно.

## <a name="event-vs-message-services"></a>Сравнение служб доставки сообщений и событий

Следует обратить внимание на важное различие между службами доставки событий и службами доставки сообщений.

### <a name="event"></a>Событие

Событие — это упрощенное уведомление об условии или изменении состояния. У издателя события нет ожиданий относительно его обработки. Решение о последующих действиях, связанных с уведомлением, принимает потребитель события. Событие может быть дискретным элементом или частью последовательности.

Дискретные события указывают на изменение состояния. На их основе предпринимаются действия. Чтобы выполнить следующее действие, потребителю достаточно знать, что произошло какое-то событие. Данные события содержат сведения о том, что произошло, но не о причинах активации события. Например, потребители уведомляются о создании файла. Событие может содержать общие сведения о файле, но не сам файл. Дискретные события отлично подходят для [бессерверных](https://azure.com/serverless) которые нужно которых требуется масштабировать.

События, которые являются элементами последовательности, указывают на условие и используются для анализа. Эти события упорядочены по времени и взаимосвязаны. Последовательный ряд таких событий требуется потребителю для анализа ситуации.

### <a name="message"></a>Сообщение

Сообщение представляет собой необработанные данные, создаваемые службой для использования или хранения. Оно содержит данные, которые активировали конвейер сообщений. У издателя сообщения есть ожидания относительно того, как потребитель должен обработать это сообщение. Между двумя сторонами существует соглашение. Например, издатель отправляет сообщение с необработанными данными и ожидает, что потребитель создаст на их основе файл и отправит ответ по завершении задачи.

## <a name="comparison-of-services"></a>Сравнение служб

| Служба | Назначение | Тип | Назначение |
| ------- | ------- | ---- | ----------- |
| Сетка событий Azure | Реактивное программирование | Распространение событий (дискретные) | Ответ на изменение состояния |
| Центры событий | Конвейер больших данных | Потоковая передача событий (последовательные) | Потоковая передача данных телеметрии и распределенных данных |
| Служебная шина | Обмен важными сообщениями в организациях | Сообщение | Обработка заказов и финансовых транзакций |

### <a name="event-grid"></a>Сетка событий Azure

Служба "Сетка событий Azure" — это объединяющее решение для обработки событий, обеспечивающее реактивное программирование на основе событий. Она использует модель "публикация— подписка". Издатели генерируют события, не ожидая обработки конкретных событий. Подписчики решают, какие события обрабатывать.

Служба "Сетка событий" тесно интегрирована со службами Azure и может интегрироваться со сторонними службами. Она упрощает использование событий и снижает затраты, устраняя необходимость в постоянных опросах. Служба "Сетка событий" надежно и эффективно перенаправляет события из Azure и ресурсов, отличных от Azure. Она распределяет события к зарегистрированным конечным точкам подписчика. Сообщение о событии содержит сведения, необходимые для реагирования на изменения в службах и приложениях. Служба "Сетка событий" — это не конвейер данных. Она не выполняет доставку фактического объекта, который был обновлен.

Служба "Сетка событий" поддерживает сохранение недоставленных событий, которые не были доставлены в конечную точку.

Она отличается следующими характеристиками:

* динамически масштабируемая;
* низкая стоимость;
* независимая от сервера.
* выполняется минимум однократная доставка.

### <a name="event-hubs"></a>Центры событий

Центры событий Azure представляют собой конвейер больших данных. Он облегчает запись, хранение и воспроизведение потоковых данных телеметрии и событий. Данные могут поступать из многих параллельных источников. Благодаря Центрам событий данные телеметрии и событий доступны различным инфраструктурам потоковой обработки и службам аналитики. Они доступны как потоки данных или объединенные пакеты событий. Эта служба предоставляет единое решение, которое обеспечивает быстрое извлечение данных для обработки в режиме реального времени, а также повторное воспроизведение хранимых необработанных данных. Служба может записывать потоковые данные в файл для последующей обработки и анализа.

Она отличается следующими характеристиками:

* минимальная задержка;
* способность получать и обрабатывать миллионы событий в секунду.
* выполняется минимум однократная доставка.

### <a name="service-bus"></a>Служебная шина

Служебная шина предназначена для стандартных корпоративных приложений. Эти приложения требуют выполнения транзакций, упорядочивания, поиска повторяющихся сообщений и мгновенного обеспечения согласованности. Благодаря служебной шине [облачные](https://azure.microsoft.com/overview/cloudnative/) приложения способны обеспечить надежное управление переходами состояния для бизнес-процессов. Мы рекомендуем использовать служебную шину Azure при обработке сообщений высокой ценности, которые нельзя потерять или повторно отправить. Служебная шина также облегчает взаимодействие с высоким уровнем безопасности в гибридных облачных решениях, и с ее помощью можно подключить имеющиеся локальные системы к этим решениям.

Служебная шина — это система обмена сообщениями через брокер. Она хранит сообщения в "брокере" (например, в очереди), пока принимающая сторона не будет готова к приему.

Она отличается следующими характеристиками:

* надежная асинхронная доставка сообщений (корпоративный обмен сообщениями как услуга), требующая опросов;
* расширенные возможности обмена сообщениями, например обслуживание в порядке поступления (FIFO), пакетная обработка и сеансы, транзакции, перемещение в очередь недоставленных сообщений, временный контроль, маршрутизация и фильтрация, а также обнаружение повторяющихся сообщений.
* выполняется минимум однократная доставка.
* возможность выбора упорядоченной поставки;

## <a name="use-the-services-together"></a>Одновременное использование служб

В некоторых случаях для выполнения различных ролей службы могут использоваться параллельно. Например, сайт электронной коммерции может использовать служебную шину для обработки заказа, Центры событий — для сбора данных телеметрии сайта, а службу "Сетка событий" — для реагирования на такие события, как отправка товара.

В других случаях вы можете связать эти службы для формирования конвейера данных и событий. С помощью службы "Сетка событий" можно отвечать на события в других службах. Пример использования службы "Сетка событий" с Центрами событий для перемещения данных в хранилище данных см. в статье [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md). На следующем рисунке показан рабочий процесс потоковой передачи данных.

![Обзор процесса потоковой передачи данных](./media/compare-messaging-services/overview.png)

## <a name="next-steps"></a>Дальнейшие действия
См. следующие статьи: 
- [Asynchronous messaging options in Azure](/azure/architecture/guide/technology-choices/messaging) (Параметры асинхронного обмена сообщениями в Azure)
- [Events, Data Points, and Messages - Choosing the right Azure messaging service for your data](https://azure.microsoft.com/blog/events-data-points-and-messages-choosing-the-right-azure-messaging-service-for-your-data/) (События, точки данных, сообщения — выбор подходящей службы для ваших данных).
- [Очереди службы хранилища и очереди служебной шины: сходства и различия](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).
- Сведения о том, как приступить к использованию службы "Сетка событий", см. в статье [Создание и перенаправление пользовательских событий с помощью Azure CLI и службы "Сетка событий"](custom-event-quickstart.md).
- Сведения о том, как приступить к использованию Центров событий, см. в статье [Создание пространства имен Центров событий и концентратора событий с помощью портала Azure](../event-hubs/event-hubs-create.md).
- Сведения о том, как приступить к использованию служебной шины, см. в статье [Создание пространства имен служебной шины с помощью портала Azure](../service-bus-messaging/service-bus-create-namespace-portal.md).
