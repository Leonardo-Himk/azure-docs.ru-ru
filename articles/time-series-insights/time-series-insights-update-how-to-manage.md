---
title: Создание среды предварительного просмотра и управление ею с помощью временных рядов Azure | Документация Майкрософт
description: Узнайте, как подготавливать среду предварительной версии службы "аналитика временных рядов Azure" и управлять ею.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 1ec0d9c7ecf16c60c32abdf08b358268f460edb0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77087217"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Подготовка к работе службы "Аналитика временных рядов Azure" (предварительная версия) и управление ею

В этой статье описывается создание среды "Аналитика временных рядов Azure" (предварительная версия) и управление ею с помощью [портала Azure](https://portal.azure.com/).

## <a name="overview"></a>Обзор

Среды предварительной версии службы "аналитика временных рядов Azure" являются средами с *оплатой по мере* использования (PAYG).

При подготовке среды предварительного просмотра службы "аналитика временных рядов Azure" вы создадите следующие ресурсы Azure:

* Среда "Аналитика временных рядов Azure (предварительная версия)"  
* Учетная запись службы хранилища Azure общего назначения версии 1
* Дополнительное горячее хранение для более быстрых и неограниченных запросов

> [!TIP]
> * Узнайте [, как спланировать среду](./time-series-insights-update-plan.md).
> * Узнайте, как [Добавить источник концентратора событий](./time-series-insights-how-to-add-an-event-source-eventhub.md) или [Добавить источник центра Интернета вещей](./time-series-insights-how-to-add-an-event-source-iothub.md).

Вы научитесь:

1. **(Необязательно)** Свяжите каждую среду предварительного просмотра службы "аналитика временных рядов Azure" с источником событий. Кроме того, вы будете предоставлять свойство идентификатора метки времени и уникальную группу потребителей, чтобы обеспечить доступ к соответствующим событиям в среде.

   > [!NOTE]
   > Предыдущий шаг не является обязательным при подготовке среды. Если пропустить этот шаг, необходимо присоединить источник событий к среде позже, чтобы получить доступ к данным в среде.

1. Когда процесс подготовки будет завершен, вы можете изменять политику доступа и другие атрибуты среды в соответствии со своими бизнес-требованиями.

## <a name="create-the-environment"></a>Создание среды

Создание среды предварительной версии службы "аналитика временных рядов Azure"

1. Выберите **PAYG** в качестве **уровня**. Укажите имя среды и выберите группу подписок и группу ресурсов для использования. Затем выберите поддерживаемое расположение для размещения среды.

   [![Создайте экземпляр службы "Аналитика временных рядов Azure".](media/v2-update-manage/create-and-manage-configuration.png)](media/v2-update-manage/create-and-manage-configuration.png#lightbox)

1. Введите идентификатор временного ряда.

    > [!NOTE]
    > * Идентификатор временного ряда учитывает *регистр* и *неизменяемый*. (Его нельзя изменить после установки.)
    > * Идентификаторы временных рядов могут состоять не более чем из *трех* ключей.
    > * Узнайте больше о [том, как выбрать идентификатор временного ряда](time-series-insights-update-how-to-id.md)

1. Создайте учетную запись хранения Azure, выбрав имя учетной записи хранения и указав вариант репликации. При этом будет автоматически создана учетная запись хранения Azure общего назначения версии 1. Учетная запись создается в том же регионе, что и ранее выбранная среда предварительной версии службы "аналитика временных рядов Azure".

    [![Конфигурация холодного хранилища](media/v2-update-manage/create-and-manage-cold-store.png)](media/v2-update-manage/create-and-manage-cold-store.png#lightbox)

1. **(Необязательно)** Включите горячее хранение для вашей среды, если вам нужны более быстрые и неограниченные запросы к последним данным в среде. Вы также можете создать или удалить горячее хранилище с помощью параметра **конфигурации хранилища** в левой области навигации после создания среды предварительной версии службы "аналитика временных рядов".

    [![Конфигурация "горячего" хранилища](media/v2-update-manage/create-and-manage-warm-storage.png)](media/v2-update-manage/create-and-manage-warm-storage.png#lightbox)

1. **(Необязательно)** Теперь можно добавить источник событий. Кроме того, можно подождать, пока экземпляр не будет подготовлен.

   * Служба "аналитика временных рядов" поддерживает [центр Интернета вещей Azure](./time-series-insights-how-to-add-an-event-source-iothub.md) и [концентраторы событий Azure](./time-series-insights-how-to-add-an-event-source-eventhub.md) в качестве параметров источника событий. Хотя при создании среды можно добавить только один источник событий, можно позднее добавить другой источник событий. 
   
     При добавлении источника событий можно выбрать существующую группу потребителей или создать новую. Лучше всего создать уникальную группу потребителей, чтобы убедиться, что все события видны в среде предварительной версии службы "аналитика временных рядов Azure".

   * Выберите соответствующее свойство timestamp. По умолчанию служба "аналитика временных рядов Azure" использует время очереди сообщений для каждого источника событий.

     > [!TIP]
     > Время в очереди сообщений может быть не лучшим параметром, настроенным для использования в сценариях пакетных событий или в сценариях передачи исторических данных. В таких случаях обязательно убедитесь, что ваше решение использует или не использует свойство timestamp.

     [![Вкладка «Настройка источника событий»](media/v2-update-manage/create-and-manage-event-source.png)](media/v2-update-manage/create-and-manage-event-source.png#lightbox)

1. Убедитесь, что ваша среда подготовлена и настроена нужным образом.

    [![Вкладка "Обзор и создание"](media/v2-update-manage/create-and-manage-review-and-confirm.png)](media/v2-update-manage/create-and-manage-review-and-confirm.png#lightbox)

## <a name="manage-the-environment"></a>Управление средой

Вы можете управлять своей средой службы "Аналитика временных рядов Azure" (предварительная версия) с помощью портала Azure. Существует несколько ключевых различий между средой предварительного просмотра службы "аналитика временных рядов Azure PAYG" и общедоступными средами S1 или S2 при управлении средой с помощью портал Azure:

* В колонке **обзор** портал Azure Preview имеются следующие изменения.

  * Емкость удалена, так как она не применяется к средам PAYG.
  * Добавляется свойство **идентификатора временных рядов** . Оно определяет способ секционирования данных.
  * Удалены наборы эталонных данных.
  * Отображаемый URL-адрес указывает на [проводник службы "Аналитика временных рядов Azure (предварительная версия)"](./time-series-insights-update-explorer.md).
  * Указано имя вашей учетной записи хранения Azure.

* Колонка **настройки** портал Azure будет удалена в предварительной версии службы "аналитика временных рядов Azure", так как среды PAYG не настраиваются. Однако можно использовать **конфигурацию хранилища** для настройки вновь появившегося горячего хранилища.

* Колонка **ссылочных данных** портал Azure удаляется в предварительной версии службы "аналитика временных рядов Azure", так как справочные данные не входят в PAYG среды.

[![Среда "Аналитика временных рядов" (предварительная версия) на портале Azure](media/v2-update-manage/create-and-manage-overview-confirm.png)](media/v2-update-manage/create-and-manage-overview-confirm.png#lightbox)

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о доступности сред и предварительных версий для аналитики временных рядов см. в статье [Планирование среды](./time-series-insights-update-plan.md).

- Узнайте, как [Добавить источник концентратора событий](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Настройка [источника центра Интернета вещей](./time-series-insights-how-to-add-an-event-source-iothub.md).
