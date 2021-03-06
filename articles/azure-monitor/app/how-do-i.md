---
title: Выполнение заданий в Azure Application Insights | Документация Майкрософт
description: Вопросы и ответы об Application Insights
ms.topic: conceptual
ms.date: 04/04/2017
ms.openlocfilehash: 8d4b1e79c48b14ed7dce756468e4c48d633c3f04
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536868"
---
# <a name="how-do-i--in-application-insights"></a>Как работать с Application Insights
## <a name="get-an-email-when-"></a>Получать уведомление по электронной почте, если...
### <a name="email-if-my-site-goes-down"></a>Уведомлять меня по электронной почте, если сайт выходит из строя
Настройте [веб-тест доступности](../../azure-monitor/app/monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>Уведомлять меня по электронной почте, если сайт перегружен
Настройте [оповещение](../../azure-monitor/app/alerts.md) для **времени ответа от сервера**. Пороговое значение может быть в пределах от 1 до 2 секунд.

![](./media/how-do-i/030-server.png)

Приложение может также демонстрировать признаки нагрузки, выдавая коды ошибок. Настройте оповещение при **неудачных запросах**.

Если вы хотите настроить оповещение при **исключениях сервера**, для просмотра данных может потребоваться [дополнительная настройка](../../azure-monitor/app/asp-net-exceptions.md) .

### <a name="email-on-exceptions"></a>Получить уведомление по электронной почте при исключении
1. [Настройте мониторинг исключений](../../azure-monitor/app/asp-net-exceptions.md)
2. [Установите оповещение](../../azure-monitor/app/alerts.md) об исключении подсчета метрики

### <a name="email-on-an-event-in-my-app"></a>Уведомлять меня по электронной почте о событиях в приложении
Предположим, что вы хотите получать уведомления по электронной почте при возникновении определенных событий. Application Insights не предоставляет эту функцию напрямую, но позволяет [отправлять оповещение, если метрика превысит пороговое значение](../../azure-monitor/app/alerts.md).

Оповещения можно настроить для [пользовательских метрик](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric), но не для пользовательских событий. Напишите код, который будет увеличивать метрику при возникновении соответствующего события:

    telemetry.TrackMetric("Alarm", 10);

или:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Так как оповещения имеют два состояния, отправьте минимальное значение, при котором оповещение должно быть отменено:

    telemetry.TrackMetric("Alarm", 0.5);

Создайте диаграмму в [обозревателе метрик](../../azure-monitor/platform/metrics-charts.md) , чтобы увидеть оповещение:

![](./media/how-do-i/010-alarm.png)

Теперь настройте оповещение таким образом, чтобы оно отправлялось в случае, когда метрика превышает среднее значение за короткий период:

![](./media/how-do-i/020-threshold.png)

Установите минимальный период усреднения.

Вы будете получать уведомления по электронной почте, если метрика поднимется выше или упадет ниже порогового значения.

Учитывайте следующие факторы.

* Оповещение может находиться в двух состояниях: «оповещение» и «исправен». Состояние оценивается только при получении метрики.
* Электронное письмо отправляется только при изменении состояния. Вот почему необходимо отправлять как верхнюю, так и нижнюю метрику.
* Для оценки оповещениям берется среднее значений, полученных за предыдущий период. Это происходит при каждом получении метрики, поэтому сообщения электронной почты могут отправляться чаще установленной вами периодичности.
* Поскольку сообщения электронной почты отправляются и при состоянии «оповещение», и при состоянии «исправен», считайте это разовое событие условием с двумя состояниями. Например, вместо события «задание завершено» создайте условие «задание выполняется», при котором сообщения электронной почты будут отправляться при запуске и завершении задания.

### <a name="set-up-alerts-automatically"></a>Настройте автоматические оповещения
[Создание новых оповещений с помощью PowerShell](../../azure-monitor/app/alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>Использование PowerShell для управления Application Insights
* [Создание новых ресурсов](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#creating-a-resource-automatically)
* [Создание новых оповещений](../../azure-monitor/app/alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Разделение телеметрии разных версий

* Несколько ролей в приложении: используйте один ресурс Application Insights и выполните фильтрацию по [cloud_Rolename](../../azure-monitor/app/app-map.md).
* Отдельные стадии разработки, тестирования и выпуска версий. Используйте различные ресурсы Application Insights. Выберите ключи инструментирования из файла Web. config. Дополнительные [сведения](../../azure-monitor/app/separate-resources.md)
* Отчеты о версиях сборки. Добавьте свойство с помощью инициализатора телеметрии. [Дополнительные сведения](../../azure-monitor/app/separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Мониторинг внутренних серверов и классических приложений
[Используйте модуль пакета SDK для Windows Server](../../azure-monitor/app/windows-desktop.md).

## <a name="visualize-data"></a>Визуализируйте данные
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Панель мониторинга с метрикой для нескольких приложений
* В [обозревателе метрик](../../azure-monitor/platform/metrics-charts.md)настройте диаграмму и сохраните ее в списке избранного. Закрепите ее на панели мониторинга Azure.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Панель мониторинга с данными из других источников и Application Insights
* [Экспорт телеметрии в Power BI](../../azure-monitor/app/export-power-bi.md ).

Или

* Используйте SharePoint как панель мониторинга для отображения данных веб-компонентов SharePoint. [Используйте непрерывный экспорт и Stream Analytics для экспорта в SQL](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md).  Используйте PowerView для просмотра базы данных и создания веб-компонента SharePoint для PowerView.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Отфильтровывание анонимных или прошедших проверку подлинности пользователей
Если пользователи входят в систему, можно задать [идентификатор пользователя, прошедшего проверку подлинности](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users). (Это не происходит автоматически.)

Можно выполнить следующее.

* Поиск по указанным идентификаторам пользователей

![](./media/how-do-i/110-search.png)

* выполнить фильтрацию метрики для анонимных или прошедших проверку подлинности пользователей.

![](./media/how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Изменение имен и значений свойств
Создайте [фильтр](../../azure-monitor/app/api-filtering-sampling.md#filtering). Это позволяет изменять или фильтровать данные телеметрии перед их отправкой из приложения в Application Insights.

## <a name="list-specific-users-and-their-usage"></a>Вывод списка определенных пользователей и информации об их использовании
Если нужно просто [найти конкретных пользователей](#search-specific-users), можно задать [идентификатор пользователя, прошедшего проверку подлинности](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users).

Если вы хотите получить список пользователей с данными — например, на какие страницы заходят пользователи или как часто они входят в систему, — существует два варианта действий:

* [Укажите идентификатор пользователя, прошедшего проверку подлинности](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users), [экспортируйте его в базу данных](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md) и используйте подходящие средства для анализа данных пользователя.
* Если имеется небольшое количество пользователей, отправьте пользовательские события или метрики, используя интересующие данные в качестве значения метрики или имени события, и задавая идентификатор пользователя в качестве свойства. Для анализа просмотров страниц замените стандартный вызов JavaScript trackPageView. Чтобы проанализировать данные телеметрии на стороне сервера, используйте инициализатор телеметрии, чтобы добавить идентификатор пользователя во все данные телеметрии сервера. Затем можно отфильтровать и сегментировать метрики и поиск по ИДЕНТИФИКАТОРу пользователя.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Уменьшение трафика из вашего приложения в Application Insights
* В файле [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)отключите все неиспользуемые модули, например сборщик данных счетчиков производительности.
* Используйте [Выборка и фильтрация](../../azure-monitor/app/api-filtering-sampling.md) в пакете SDK.
* На своих веб-страницах ограничьте число вызовов Ajax для каждого представления страницы. Во фрагменте сценария после `instrumentationKey:...` вставьте `,maxAjaxCallsPerView:3` (или другое подходящее число).
* Если используется [TrackMetric](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric), вычисляйте агрегированное значение для пакетов значений метрики перед отправкой результата. Это можно сделать с помощью перегруженного метода TrackMetric().

Подробнее о [расценках и квотах](../../azure-monitor/app/pricing.md).

## <a name="disable-telemetry"></a>Отключение данных телеметрии
Чтобы **динамически остановить и запустить** сбор и передачу данных телеметрии с сервера:

### <a name="aspnet-classic-applications"></a>Классические приложения ASP.NET

```csharp
    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

### <a name="other-applications"></a>Другие приложения
Не рекомендуется использовать `TelemetryConfiguration.Active` Singleton в консольных и ASP.NET Core приложениях.
Если экземпляр создан `TelemetryConfiguration` самостоятельно, задайте для `DisableTelemetry` `true`значение.

Для ASP.NET Core приложений вы можете получить `TelemetryConfiguration` доступ к экземпляру с помощью [ASP.NET Core внедрения зависимостей](/aspnet/core/fundamentals/dependency-injection/). Дополнительные сведения см. в статье [ApplicationInsights for ASP.NET Core Applications](../../azure-monitor/app/asp-net-core.md) .

## <a name="disable-selected-standard-collectors"></a>Отключить выбранные стандартные собирающие
Можно отключить стандартные собирающие (например, счетчики производительности, HTTP-запросы или зависимости).

* **ASP.NET приложения** — удалите соответствующие строки в [файле ApplicationInsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) или закомментируйте их.
* **ASP.NET Core приложения** — используйте параметры конфигурации модулей телеметрии в [ApplicationInsights ASP.NET Core](../../azure-monitor/app/asp-net-core.md#configuring-or-removing-default-telemetrymodules)

## <a name="view-system-performance-counters"></a>Просмотр счетчиков производительности системы
В показатели метрики, которые можно отображать в обозревателе метрики, входит набор системных счетчиков производительности. В готовой колонке **Серверы** отображается несколько таких счетчиков.

![Откройте ресурс Application Insights и щелкните "Серверы".](./media/how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Если данные счетчика производительности не отображаются
* **Сервер IIS** на собственном компьютере или на виртуальной машине. [Установите монитор состояния](../../azure-monitor/app/monitor-performance-live-website-now.md).
* **Веб-сайт Azure** — мы еще не поддерживаем счетчики производительности. Существует несколько метрик, которые можно получить в составе стандартной панели управления веб-сайта Azure.
* **Unix server** - [Собранная установка](../../azure-monitor/app/java-collectd.md) сервера UNIX

### <a name="to-display-more-performance-counters"></a>Для отображения дополнительных счетчиков производительности
* Сначала [добавьте новую диаграмму](../../azure-monitor/platform/metrics-charts.md) , чтобы посмотреть, находится ли счетчик в базовом наборе предложения.
* Если его нет, [добавьте счетчик к набору, собранному модулем счетчика производительности](../../azure-monitor/app/performance-counters.md).
