---
title: Получение событий ресурсов в службе приложений Azure
description: Узнайте, как получать события ресурсов с помощью журналов действий и сетки событий в приложении службы приложений.
ms.topic: article
ms.date: 04/24/2020
ms.author: msangapu
ms.openlocfilehash: 1fd283f95823a67319dc467a3a1d6251193182da
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124743"
---
# <a name="get-resource-events-in-azure-app-service"></a>Получение событий ресурсов в службе приложений Azure

Служба приложений Azure предоставляет встроенные средства для мониторинга состояния и работоспособности ресурсов. События ресурсов помогают понять все изменения, внесенные в базовые ресурсы веб-приложения, и предпринимать необходимые действия. К примерам событий относятся: масштабирование экземпляров, обновления параметров приложения, перезапуск веб-приложения и многое другое. В этой статье вы узнаете, как просматривать [журналы действий Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) и включать службу " [Сетка событий](https://docs.microsoft.com/azure/event-grid/) " для мониторинга событий ресурсов, связанных с веб-приложением службы приложений.

> [!NOTE]
> Интеграция службы приложений с сеткой событий доступна в **предварительной версии**. [Дополнительные сведения см. в объявлении.](https://aka.ms/app-service-event-grid-announcement)
>

## <a name="view-azure-activity-logs"></a>Просмотр журналов действий Azure
Журналы действий Azure содержат события ресурсов, созданные операциями с ресурсами в подписке. Действия пользователя в шаблонах портал Azure и Azure Resource Manager влияют на события, захваченные журналом действий. 

Журналы действий Azure для сведений о службе приложений, например:
- какие операции выполнялись над ресурсами (например, планы службы приложений).
- кто запустил операцию;
- когда была выполнена операция;
- состояние операции;
- значения свойств, помогающие в исследовании операции

### <a name="what-can-you-do-with-azure-activity-logs"></a>Что можно сделать с помощью журналов действий Azure?

Журналы действий Azure можно запрашивать с помощью портал Azure, PowerShell, REST API или CLI. Журналы можно отправить в учетную запись хранения, концентратор событий и Log Analytics. Можно также проанализировать их в Power BI или создать оповещения, которые будут обновляться на мероприятиях ресурсов.

[Просмотр и получение событий журнала действий Azure.](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

## <a name="ship-activity-logs-to-event-grid"></a>Отправить журналы действий в сетку событий

Хотя журналы действий основаны на пользователях, существует новая интеграция службы " [Сетка событий](https://docs.microsoft.com/azure/event-grid/) " со службой приложений (Предварительная версия), которая регистрирует как действия пользователя, так и автоматические события. С помощью сетки событий можно настроить обработчик для реагирования на события, которые говорят. Например, служба "Сетка событий Azure" может активировать функции для бессерверной обработки данных, запуская анализ изображений при каждом добавлении новой фотографии в контейнер больших двоичных объектов.

Кроме того, службу "Сетка событий Azure" можно использовать с Logic Apps для обработки данных в любой точке мира без необходимости писать код. Служба "Сетка событий Azure" связывает источники данных и обработчики событий. Например, служба "Сетка событий Azure" может активировать функции для бессерверной обработки данных, запуская анализ изображений при каждом добавлении новой фотографии в контейнер больших двоичных объектов.

### <a name="supported-event-types"></a>Поддерживаемые типы событий
| Тип события |Описание|
| -----------| ------------- |
| Microsoft.web/sites | Webapp |
| баккупоператионкомплетед |Резервное копирование webapp успешно завершено|
| баккупоператионфаилед | Не удалось выполнить резервное копирование webapp|
| рестореоператионстартед |Начато восстановление из резервной копии|
| рестореоператионкомплетед |Восстановление из резервной копии успешно завершено|
| рестореоператионфаилед |Сбой восстановления из резервной копии|
| слотсвапстартед |Переключение слотов начато|
| слотсвапкомплетед |Переключение слотов выполнено успешно|
| слотсвапфаилед |Сбой переключения слотов|
| слотсвапвиспревиевстартед |Начато переключение слотов с предварительным просмотром|
| слотсвапвиспревиевканцеллед |Сбой переключения слотов с предварительным просмотром|
| аппупдатед | |
| Перезапустить | Webapp был перезапущен |
| Остановлена | Webapp остановлена |
| чанжедаппсеттингс | Параметры приложения на webapp были изменены |
| - | - |
| Microsoft. Web/serverfarms | (План службы приложений) |
| аспупдатед | План службы приложений был обновлен. Объект события содержит подробные сведения о свойствах, которые были изменены. |
| увеличение или уменьшение масштаба; | План службы приложений масштабируется вверх или вниз. Объект события содержит число экземпляров.|


## <a name="next-steps"></a><a name="nextsteps"></a> Дальнейшие действия
* [Журналы запросов с Azure Monitor](../azure-monitor/log-query/log-query-overview.md)
* [Мониторинг приложений в Службе приложений Azure](web-sites-monitor.md)
* [Troubleshoot an app in Azure App Service using Visual Studio](troubleshoot-dotnet-visual-studio.md) (Устранение неполадок приложения в Службе приложений Azure с помощью Visual Studio)
* [Анализ журналов приложения в HDInsight](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
