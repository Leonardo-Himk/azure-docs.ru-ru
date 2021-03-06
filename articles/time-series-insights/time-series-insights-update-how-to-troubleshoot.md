---
title: Диагностика и устранение неполадок в среде предварительной версии — служба "аналитика временных рядов Azure" | Документация Майкрософт
description: Узнайте, как диагностировать и устранять неполадки в среде предварительной версии службы "аналитика временных рядов Azure".
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 667dee6365f38ae058e91c61c24838d8912df26a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80152670"
---
# <a name="diagnose-and-troubleshoot-a-preview-environment"></a>Диагностика и устранение неполадок в среде предварительной версии

В этой статье описаны распространенные проблемы, с которыми вы можете столкнуться при работе со средой службы "Аналитика временных рядов Azure" (предварительная версия). Здесь также описаны потенциальные причины и решения каждой проблемы.

## <a name="problem-i-cant-find-my-environment-in-the-preview-explorer"></a>Проблема. не удается найти окружение в обозревателе предварительной версии

Эта проблема может возникнуть, если у вас нет прав доступа к среде службы "Аналитика временных рядов". Для просмотра такой среды пользователям необходима роль доступа на уровне читателя. Чтобы проверить текущие уровни доступа и предоставить дополнительный доступ, перейдите к разделу **политики доступа к данным** в ресурсе "аналитика временных рядов" в [портал Azure](https://portal.azure.com/).

  [![Проверка политик доступа к данным.](media/preview-troubleshoot/verify-data-access-policies.png)](media/preview-troubleshoot/verify-data-access-policies.png#lightbox)

## <a name="problem-no-data-is-seen-in-the-preview-explorer"></a>Проблема. в обозревателе предварительной версии данные не отображаются

Существует несколько распространенных причин, по которым данные могут не отображаться в [обозревателе предварительного просмотра службы "аналитика временных рядов Azure](https://insights.timeseries.azure.com/preview)".

- Ваш источник события может не получать данные.

    Убедитесь, что источник события, представляющий собой концентратор событий или центр Интернета вещей, получает данные из ваших тегов или экземпляров. Для этого перейдите к странице обзора вашего ресурса на портале Azure.

    [![Обзор метрик панели мониторинга.](media/preview-troubleshoot/verify-dashboard-metrics.png)](media/preview-troubleshoot/verify-dashboard-metrics.png#lightbox)

- Источник события не содержит данные в формате JSON.

    Среда службы "Аналитика временных рядов" поддерживает только данные в формате JSON. Примеры JSON см. в статье [поддерживаемые фигуры JSON](./how-to-shape-query-json.md).

- Для вашего ключа источника события отсутствует необходимое разрешение.

  * Для Центра Интернета вещей необходимо указать ключ с разрешением на **подключение службы**.

    [![Проверьте разрешения центра Интернета вещей.](media/preview-troubleshoot/verify-correct-permissions.png)](media/preview-troubleshoot/verify-correct-permissions.png#lightbox)

    * Политики **iothubowner** и **Service** работают, так как у них есть разрешение на **Подключение к службе** .

  * Для концентратора событий необходимо указать ключ с разрешением на **прослушивание**.
  
    [![Проверьте разрешения концентратора событий.](media/preview-troubleshoot/verify-eh-permissions.png)](media/preview-troubleshoot/verify-eh-permissions.png#lightbox)

    * Политики **чтения** и **управления** работают, так как они имеют разрешение на **прослушивание** .

- Указанная группа объектов-получателей не является уникальной для службы "Аналитика временных рядов".

    Во время регистрации центра Интернета вещей или концентратора событий нужно указать группу объектов-получателей, которая используется для чтения данных. Эта группа потребителей должна быть уникальной для каждой среды. Иначе базовый концентратор событий автоматически отключит один из модулей чтения в случайном порядке. Укажите уникальную группу получателей, из которой должна считывать данные среда службы "Аналитика временных рядов".

- Ваше свойство идентификатора временного ряда, указанное во время подготовки, неверно, отсутствует или имеет значение NULL.

    Эта проблема может возникнуть, если во время подготовки среды свойство идентификатора временного ряда настроено неправильно. Дополнительные сведения см. [в статье рекомендации по выбору идентификатора временных рядов](./time-series-insights-update-how-to-id.md). В настоящее время вы не можете обновить имеющуюся среду службы "Аналитика временных рядов", чтобы использовать другой идентификатор временного ряда.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Проблема. Некоторые данные отображаются, но некоторые отсутствуют

Возможно, вы отправляете данные без идентификатора временного ряда.

- Эта проблема может возникнуть при отправке событий без поля идентификатора временного ряда в полезных данных. Дополнительные сведения см. в статье [поддерживаемые фигуры JSON](./how-to-shape-query-json.md).
- Эта проблема может возникнуть из-за регулирования вашей среды.

    > [!NOTE]
    > В настоящее время служба "Аналитика временных рядов" поддерживает максимальную скорость приема данных 6 Мбит/с.

## <a name="problem-data-was-showing-but-now-ingestion-has-stopped"></a>Проблема: данные были показаны, но теперь прием остановлен

- Возможно, ключ источника события был повторно создан, и в среде предварительного просмотра требуется новый ключ источника события.

Эта проблема возникает, когда ключ, указанный при создании источника события, больше не является допустимым. В концентраторе будут отображаться данные телеметрии, но не входящие сообщения, полученные в ходе анализа временных рядов. Если вы не уверены, был ли ключ создан повторно, можно выполнить поиск в журнале действий "Создание или обновление правил авторизации пространства имен" или выполнить поиск по запросу "Создание или обновление ресурса IotHub" для центра Интернета вещей. 

Чтобы обновить среду предварительного просмотра аналитики временных рядов с помощью нового ключа, откройте ресурс-концентратор в портал Azure и скопируйте новый ключ. Перейдите к ресурсу TSI и щелкните источники событий. 

   [![Обновление ключа.](media/preview-troubleshoot/update-hub-key-step-1.png)](media/preview-troubleshoot/update-hub-key-step-1.png#lightbox)

Выберите источники событий, которые были остановлены при приеме, вставьте новый ключ и нажмите кнопку Сохранить.

   [![Обновление ключа.](media/preview-troubleshoot/update-hub-key-step-2.png)](media/preview-troubleshoot/update-hub-key-step-2.png#lightbox)

## <a name="problem-my-event-sources-timestamp-property-name-doesnt-work"></a>Проблема. имя свойства метки времени источника событий не работает

Убедитесь, что имя и значение соответствуют следующим правилам:

* В имени свойства метки времени учитывается регистр.
* Значение свойства timestamp, которое берется из источника события в виде строки JSON, имеет формат `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Пример такой строки — `“2008-04-12T12:53Z”`.

Самый простой способ убедиться, что имя свойства метки времени зафиксировано и работает правильно, — использовать обозреватель службы "Аналитика временных рядов" (предварительная версия). В этом обозревателе с помощью схемы выберите период времени после предоставления имени свойства метки времени. Щелкните выделение правой кнопкой мыши и выберите параметр **обзора событий**. Заголовок первого столбца — это имя вашего свойства метки времени. Рядом со словом `Timestamp` должен находиться аргумент `($ts)` вместо приведенных ниже вариантов:

* `(abc)` указывает на то, что служба "Аналитика временных рядов" считывает значения данных в виде строк.
* Значок **календаря** , указывающий, что Time Series Insights считывает значение данных как DateTime.
* `#` указывает, что служба "Аналитика временных рядов" считывает значения данных как целое число.

Если свойство метки времени не указано явным образом, в качестве метки времени по умолчанию используется время постановки в очередь центра Интернета вещей или концентратора событий.

## <a name="problem-i-cant-view-data-from-my-warm-store-in-the-explorer"></a>Проблема. не удается просмотреть данные из "горячего" хранилища в проводнике

- Возможно, вы недавно подготовили горячее хранилище, и данные все еще передаются.
- Возможно, вы удалили горячее хранилище. в этом случае данные будут потеряны.

## <a name="problem-i-cant-view-or-edit-my-time-series-model"></a>Проблема. не удается просмотреть или изменить модель временных рядов

- Возможно, вы пытаетесь получить доступ к среде службы "Аналитика временных рядов" S1 или S2.

   Модели временных рядов поддерживаются только в средах с оплатой по мере использования. Дополнительные сведения о доступе к среде S1 или S2 из обозревателя предварительного просмотра аналитики временных рядов см. [в статье Визуализация данных в обозревателе](./time-series-insights-update-explorer.md).

   [![Нет событий в окружении.](media/preview-troubleshoot/troubleshoot-no-events.png)](media/preview-troubleshoot/troubleshoot-no-events.png#lightbox)

- Возможно, у вас нет прав на просмотр и изменение модели.

   Для изменения и просмотра модели временных рядов пользователям необходим доступ на уровне участника. Чтобы проверить текущие уровни доступа и предоставить дополнительный доступ, перейдите к разделу **политики доступа к данным** в ресурсе "аналитика временных рядов" в портал Azure.

## <a name="problem-all-my-instances-in-the-preview-explorer-lack-a-parent"></a>Проблема. у всех моих экземпляров в обозревателе предварительной версии нет родителя

Эта проблема может возникнуть, если в вашей среде не определена иерархия модели временных рядов. Дополнительные сведения см. [в статье работа с моделями временных рядов](./time-series-insights-update-how-to-tsm.md).

  [![Экземпляры, не являющиеся родительскими, будут отображать предупреждение.](media/preview-troubleshoot/unparented-instances.png)](media/preview-troubleshoot/unparented-instances.png#lightbox)

## <a name="next-steps"></a>Дальнейшие шаги

- [Data modeling in Azure Time Series Insights Preview](./time-series-insights-update-how-to-tsm.md) (Моделирование данных в службе "Аналитика временных рядов Azure" (предварительная версия))

- Дополнительные сведения о [поддерживаемых фигурах JSON](./how-to-shape-query-json.md).

- Ознакомьтесь [с планированием и ограничениями](./time-series-insights-update-plan.md) в предварительной версии службы "аналитика временных рядов Azure".
