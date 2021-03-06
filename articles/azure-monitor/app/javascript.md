---
title: Application Insights Azure для веб-приложений JavaScript
description: Получение количества просмотров страниц и сеансов, данных веб-клиента, одностраничных приложений (SPA) и отслеживании закономерностей использования. Выявляйте исключения и проблемы с производительностью на веб-страницах JavaScript.
ms.topic: conceptual
author: Dawgfan
ms.author: mmcc
ms.date: 09/20/2019
ms.openlocfilehash: 50ce0d57ec7395c69bf65e41b67f0cb005a43cb8
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82854978"
---
# <a name="application-insights-for-web-pages"></a>Application Insights для веб-страниц

Узнайте о производительности и использовании своей веб-страницы или приложения. При добавлении [Application Insights](app-insights-overview.md) в скрипт страницы вы получаете время загрузки страниц и вызовов AJAX, количество и сведения об исключениях браузера и ошибках AJAX, а также о количестве пользователей и сеансов. Все эти данные можно разбить по страницам, версии клиентской ОС и браузера, географическому расположению и другим показателям. Можно также настроить оповещения для определенного количества сбоев или медленной загрузки страниц. Кроме того, вставив вызовы трассировки в код JavaScript, вы можете отслеживать использование различных функций приложения веб-страницы.

Application Insights можно использовать с любыми веб-страницами — просто добавьте небольшой фрагмент кода JavaScript. Если веб-служба — [Java](java-get-started.md) или [ASP.NET](asp-net.md), вы можете использовать пакеты SDK на стороне сервера в сочетании с клиентским пакетом JavaScript SDK, чтобы получить полное представление о производительности приложения.

## <a name="adding-the-javascript-sdk"></a>Добавление пакета SDK для JavaScript

1. Сначала требуется ресурс Application Insights. Если у вас еще нет ресурса и ключа инструментирования, следуйте инструкциям в разделе [Создание новых ресурсов](create-new-resource.md).
2. Скопируйте ключ инструментирования из ресурса, в который необходимо отправить данные телеметрии JavaScript.
3. Добавьте Application Insights пакет SDK для JavaScript на веб-страницу или в приложение одним из следующих двух вариантов:
    * [Установка NPM](#npm-based-setup)
    * [Фрагмент кода JavaScript](#snippet-based-setup)

> [!IMPORTANT]
> Для добавления пакета SDK для JavaScript в приложение используйте только один метод. Если вы используете программу установки NPM, не используйте фрагмент и наоборот.

> [!NOTE]
> Программа установки NPM устанавливает пакет SDK для JavaScript как зависимость от проекта, позволяя использовать IntelliSense, в то время как фрагмент кода извлекает пакет SDK во время выполнения. Обе поддерживают одни и те же функции. Однако разработчики, которым требуется больше пользовательских событий и конфигурации, обычно заNPM настройку, тогда как пользователи хотят быстро включить готовый веб-анализ.

### <a name="npm-based-setup"></a>Установка на основе NPM

```js
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView(); // Manually call trackPageView to establish the current user/session/pageview
```

### <a name="snippet-based-setup"></a>Настройка на основе фрагментов кода

Если приложение не использует NPM, вы можете напрямую инструментировать веб-страницы с помощью Application Insights, вставляя этот фрагмент в верхней части каждой страницы. Желательно, чтобы он был первым сценарием в `<head>` разделе, чтобы он мог отслеживать все возможные проблемы со всеми зависимостями. Если вы используете серверное приложение Блазор, добавьте фрагмент в начало файла `_Host.cshtml` в `<head>` разделе.

```html
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(n){var o={config:n,initialize:!0},t=document,e=window,i="script";setTimeout(function(){var e=t.createElement(i);e.src=n.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",t.getElementsByTagName(i)[0].parentNode.appendChild(e)});try{o.cookie=t.cookie}catch(e){}function a(n){o[n]=function(){var e=arguments;o.queue.push(function(){o[n].apply(o,e)})}}o.queue=[],o.version=2;for(var s=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];s.length;)a("track"+s.pop());var r="Track",c=r+"Page";a("start"+c),a("stop"+c);var u=r+"Event";if(a("start"+u),a("stop"+u),a("addTelemetryInitializer"),a("setAuthenticatedUserContext"),a("clearAuthenticatedUserContext"),a("flush"),o.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===n.disableExceptionTracking||n.extensionConfig&&n.extensionConfig.ApplicationInsightsAnalytics&&!0===n.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){a("_"+(s="onerror"));var p=e[s];e[s]=function(e,n,t,i,a){var r=p&&p(e,n,t,i,a);return!0!==r&&o["_"+s]({message:e,url:n,lineNumber:t,columnNumber:i,error:a}),r},n.autoExceptionInstrumented=!0}return o}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);(window[aiName]=aisdk).queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="sending-telemetry-to-the-azure-portal"></a>Отправка данных телеметрии в портал Azure

По умолчанию в Application Insights SDK для JavaScript выполняется Автосбор нескольких элементов телеметрии, которые полезны для определения работоспособности приложения и базового интерфейса пользователя. Сюда входит следующее.

- **Неперехваченные исключения** в приложении, включая сведения о
    - Трассировка стека
    - Сведения об исключении и сообщение, сопровождающее ошибку
    - Номер строки & столбца с ошибкой
    - URL-адрес, где возникла ошибка
- **Запросы зависимостей сети** , выполняемые **XHR** и **извлечением** приложения (получение коллекции отключено по умолчанию), включение информации о
    - URL-адрес источника зависимостей
    - Метод & Command, используемый для запроса зависимости
    - Длительность запроса
    - Код результата и состояние успеха запроса
    - Идентификатор (если есть) пользователя, выполняющего запрос
    - Контекст корреляции (если есть), где выполняется запрос
- **Сведения о пользователе** (например, расположение, сеть, IP-адрес)
- **Сведения об устройстве** (например, "браузер", "ОС", "версия", "язык", "модель")
- **Сведения о сеансе**

### <a name="telemetry-initializers"></a>Инициализаторы телеметрии
Инициализаторы телеметрии используются для изменения содержимого собранных данных телеметрии перед их отправкой из браузера пользователя. Они также могут использоваться для предотвращения отправки определенных данных телеметрии путем возврата `false`. К экземпляру Application Insights можно добавить несколько инициализаторов телеметрии и они выполняются в порядке их добавления.

Входной аргумент `addTelemetryInitializer` для является обратным вызовом, который [`ITelemetryItem`](https://github.com/microsoft/ApplicationInsights-JS/blob/master/API-reference.md#addTelemetryInitializer) принимает в качестве аргумента и `boolean` возвращает `void`или. При возврате `false`элемент телеметрии не отправляется, в противном случае переходит к следующему инициализатору телеметрии, если таковой имеется или отправляется в конечную точку коллекции телеметрии.

Пример использования инициализаторов телеметрии:
```ts
var telemetryInitializer = (envelope) => {
  envelope.data.someField = 'This item passed through my telemetry initializer';
};
appInsights.addTelemetryInitializer(telemetryInitializer);
appInsights.trackTrace({message: 'This message will use a telemetry initializer'});

appInsights.addTelemetryInitializer(() => false); // Nothing is sent after this is executed
appInsights.trackTrace({message: 'this message will not be sent'}); // Not sent
```

## <a name="configuration"></a>Конфигурация
Большинство полей конфигурации называются так, чтобы их можно было по умолчанию иметь значение false. Все поля являются необязательными `instrumentationKey`, за исключением.

| Имя | По умолчанию | Описание |
|------|---------|-------------|
| instrumentationKey | null | **Обязательное**<br>Ключ инструментирования, полученный из портал Azure. |
| accountId | null | Необязательный идентификатор учетной записи, если приложение группирует пользователей в учетные записи. Без пробелов, запятых, точек с запятой, знаков равенства или вертикальных линий |
| сессионреневалмс | 1800000 | Сеанс регистрируется, если пользователь неактивен в течение этого периода времени в миллисекундах. Значение по умолчанию — 30 минут. |
| сессионекспиратионмс | 86400000 | Сеанс заносится в журнал, если он продолжался в течение этого времени (в миллисекундах). По умолчанию — 24 часа |
| максбатчсизеинбитес | 10000 | Максимальный размер пакета телеметрии. Если пакет превышает это ограничение, он немедленно отправляется и запускается новый пакет |
| максбатчинтервал | 15 000 | Время пакетной передачи данных перед отправкой (в миллисекундах) |
| дисабликсцептионтраккинг | false | Если значение — true, исключения не собираются. Значение по умолчанию — false. |
| дисаблетелеметри | false | Если значение — true, данные телеметрии не собираются и не отправляются. Значение по умолчанию — false. |
| енабледебуг | false | Если значение — true, **внутренние** данные отладки создаются как исключение, **а не** заносятся в журнал, независимо от параметров ведения журнала пакета SDK. Значение по умолчанию — false. <br>***Примечание.*** Включение этого параметра приведет к потере телеметрии при возникновении внутренней ошибки. Это может быть полезно для быстрого выявления проблем с конфигурацией или использованием пакета SDK. Если вы не хотите терять данные телеметрии во время отладки, попробуйте использовать `consoleLoggingLevel` или `telemetryLoggingLevel` вместо `enableDebug`. |
| логгинглевелконсоле | 0 | Записывает в консоль **внутренние** ошибки Application Insights. <br>0: выкл., <br>1: только критические ошибки, <br>2: все (ошибки & предупреждения) |
| логгинглевелтелеметри | 1 | Отправляет **внутренние** ошибки Application Insights как данные телеметрии. <br>0: выкл., <br>1: только критические ошибки, <br>2: все (ошибки & предупреждения) |
| диагностиклогинтервал | 10000 | внутрикластерных Интервал опроса (в мс) для внутренней очереди ведения журнала |
| самплингперцентаже | 100 | Процент событий, которые будут отправлены. Значение по умолчанию — 100, то есть отправляются все события. Установите этот параметр, если хотите сохранить ограничения на данные для крупномасштабных приложений. |
| аутотраккпажевиситтиме | false | Если значение равно true, то время просмотра предыдущей инструментированной страницы отсылается и отправляется в виде данных телеметрии, а для текущего pageview запускается новый таймер. Значение по умолчанию — false. |
| дисаблеажакстраккинг | false | Если значение равно true, вызовы AJAX не собираются. Значение по умолчанию — false. |
| дисаблефетчтраккинг | true | Если значение — true, запросы на выборку не собираются. Значение по умолчанию — true. |
| оверридепажевиевдуратион | false | Если значение — true, поведение по умолчанию trackPageView изменяется для записи окончания интервала длительности просмотра страницы при вызове trackPageView. Если для trackPageView не задано значение false и пользовательская длительность не предоставлена, то производительность просмотра страницы вычисляется с помощью API времени навигации. Значение по умолчанию — false. |
| максажакскаллспервиев | 500 | По умолчанию 500 — определяет, сколько вызовов AJAX будет отслеживаться каждым представлением страницы. Задайте значение-1, чтобы отслеживать все неограниченные вызовы AJAX на странице. |
| дисабледаталоссаналисис | true | Если значение равно false, внутренние буферы отправителя телеметрии будут проверяться при запуске для элементов, которые еще не были отправлены. |
| дисаблекоррелатионхеадерс | false | Если значение равно false, пакет SDK добавит два заголовка ("Request-ID" и "Request-context") во все запросы зависимости, чтобы сопоставить их с соответствующими запросами на стороне сервера. Значение по умолчанию — false. |
| коррелатионхеадерексклудеддомаинс |  | Отключение заголовков корреляции для конкретных доменов |
| коррелатионхеадердомаинс |  | Включение заголовков корреляции для конкретных доменов |
| дисаблефлушонбефореунлоад | false | Значение по умолчанию — false. Если значение равно true, метод Flush не будет вызываться при триггерах событий Онбефореунлоад |
| енаблесессионсторажебуффер | true | Значение по умолчанию — true. Если значение — true, буфер со всеми неотправленными данными телеметрии хранится в хранилище сеанса. Буфер восстанавливается при загрузке страницы |
| искукиеуседисаблед | false | Значение по умолчанию — false. Если значение — true, пакет SDK не будет хранить или считывать данные из файлов cookie.|
| кукиедомаин | null | Пользовательский домен cookie. Это полезно, если требуется предоставить общий доступ к Application Insights файлам cookie между поддоменами. |
| исретридисаблед | false | Значение по умолчанию — false. Если задано значение false, повторите попытку в 206 (частичное успешное завершение), 408 (timeout), 429 (слишком много запросов), 500 (внутренняя ошибка сервера), 503 (служба недоступна) и 0 (вне сети, только если обнаружено) |
| иссторажеуседисаблед | false | Если задано значение true, пакет SDK не будет хранить или считывать данные из локального хранилища и в хранилище сеанса. Значение по умолчанию — false. |
| исбеаконапидисаблед | true | Если значение равно false, пакет SDK будет отсылать все данные телеметрии с помощью [API маяка](https://www.w3.org/TR/beacon) . |
| онунлоаддисаблебеакон | false | Значение по умолчанию — false. Когда вкладка закрывается, пакет SDK будет передавать все оставшиеся данные телеметрии с помощью [API маяка](https://www.w3.org/TR/beacon) . |
| сдкекстенсион | null | Задает имя расширения пакета SDK. Допускаются только буквы. Имя расширения добавляется в качестве префикса к тегу "AI. internal. Сдкверсион" (например, "ext_javascript: 2.0.0"). Значение по умолчанию — null. |
| исбровсерлинктраккинженаблед | false | Значение по умолчанию — false. Если значение — true, пакет SDK будет отслеживанием всех запросов на [компоновку браузера](https://docs.microsoft.com/aspnet/core/client-side/using-browserlink) . |
| appId | null | AppId используется для корреляции между зависимостями AJAX, происходящими на стороне клиента, с запросами на стороне сервера. Если API маяка включен, он не может использоваться автоматически, но может быть задан вручную в конфигурации. Значение по умолчанию равно null |
| енаблекорскоррелатион | false | Если значение — true, пакет SDK добавит два заголовка ("Request-ID" и "Request-context") во все запросы CORS для корреляции исходящих зависимостей AJAX с соответствующими запросами на стороне сервера. Значение по умолчанию — false. |
| namePrefix | неопределенный | Необязательное значение, которое будет использоваться в качестве постфикса имени для localStorage и имени файла cookie.
| енаблеаутораутетраккинг | false | Автоматическая регистрация изменений маршрута в одностраничных приложениях (SPA). Если значение — true, каждое изменение маршрута будет отсылать новый pageview Application Insights. Изменения хэш-маршрута`example.com/foo#bar`() также записываются в виде новых просмотров страниц.
| енаблерекуессеадертраккинг | false | Если значение равно true, заголовки запроса AJAX & FETCH отписываются, по умолчанию — false.
| енаблереспонсехеадертраккинг | false | Если значение равно true, то заголовки ответа на запрос AJAX & FETCH отписываются, по умолчанию — false.
| дистрибутедтраЦингмоде | `DistributedTracingModes.AI` | Задает режим распределенной трассировки. Если задан режим AI_AND_W3C или W3C, заголовки контекста трассировки W3C (трацепарент/трацестате) будут формироваться и включаться во все исходящие запросы. AI_AND_W3C предоставляется для обеспечения обратной совместимости с любыми устаревшими службами, Application Insights инструментированные.

## <a name="single-page-applications"></a>Одностраничные приложения

По умолчанию этот пакет SDK **не** будет управлять изменением маршрута на основе состояния, которое происходит в одностраничных приложениях. Чтобы включить автоматическое отслеживание изменений маршрута для одностраничного приложения, можно добавить `enableAutoRouteTracking: true` в конфигурацию установки.

В настоящее время мы предлагаем отдельный [подключаемый модуль реагирования](#react-extensions), который можно инициализировать с помощью этого пакета SDK. Кроме того, будет осуществляться отслеживание изменений маршрута, а также собраны [другие данные телеметрии, связанные с реагированием](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md).

> [!NOTE]
> Используйте `enableAutoRouteTracking: true` , только если **не** используется подключаемый модуль "реагирующий". Оба варианта могут отправлять новые PageViews при изменении маршрута. Если оба этих флажка включены, может быть отправлен дубликат PageViews.

## <a name="configuration-autotrackpagevisittime"></a>Конфигурация: Аутотраккпажевиситтиме

С помощью `autoTrackPageVisitTime: true`параметра время, затрачиваемое пользователем на каждую страницу, будет отслеживаниь. На каждом новом PageView время, которое пользователь тратил на *предыдущей* странице, отправляется как [Пользовательская метрика](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-custom-overview) с `PageVisitTime`именем. Эта пользовательская метрика отображается в [Обозреватель метрик](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) как "Метрика на основе журнала".

## <a name="react-extensions"></a>Модули реагирования

| Расширения |
|---------------|
| [React](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)|
| [React Native](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-native/README.md)|

## <a name="explore-browserclient-side-data"></a>Изучение данных в браузере и на стороне клиента

Для просмотра данных в браузере и на стороне клиента можно перейти к **метрикам** и добавить отдельные метрики, которые вас интересуют:

![](./media/javascript/page-view-load-time.png)

Вы также можете просмотреть данные из пакета SDK для JavaScript, используя интерфейс браузера на портале.

Выберите **браузер** , а затем выберите **сбои** или **производительность**.

![](./media/javascript/browser.png)

### <a name="performance"></a>Производительность

![](./media/javascript/performance-operations.png)

### <a name="dependencies"></a>Зависимости

![](./media/javascript/performance-dependencies.png)

### <a name="analytics"></a>Analytics

Чтобы запросить данные телеметрии, собранные с помощью пакета SDK для JavaScript, нажмите кнопку **Просмотр в журналах (аналитика)** . Добавив `where` оператор `client_Type == "Browser"`, вы увидите только данные из пакета SDK для JavaScript, и все телеметрии на стороне сервера, собранные другими пакетами SDK, будут исключены.
 
```kusto
// average pageView duration by name
let timeGrain=5m;
let dataset=pageViews
// additional filters can be applied here
| where timestamp > ago(1d)
| where client_Type == "Browser" ;
// calculate average pageView duration for all pageViews
dataset
| summarize avg(duration) by bin(timestamp, timeGrain)
| extend pageView='Overall'
// render result in a chart
| render timechart
```

### <a name="source-map-support"></a>Поддержка исходной схемы

Стек вызовов минифицированные для телеметрии исключений может быть унминифиед в портал Azure. Все существующие интеграции на панели сведений об исключении будут работать с вновь унминифиед стека вызовов.

#### <a name="link-to-blob-storage-account"></a>Ссылка на учетную запись хранения BLOB-объектов

Вы можете связать ресурс Application Insights с контейнером хранилища BLOB-объектов Azure, чтобы автоматически деминификацию стеки вызовов. Чтобы приступить к работе, см. раздел [Автоматическая поддержка карт исходного кода](./source-map-support.md).

### <a name="drag-and-drop"></a>Перетаскивание

1. Выберите элемент телеметрии исключения в портал Azure, чтобы просмотреть сведения о сквозной транзакции.
2. Определяет, какие исходные карты соответствуют этому стеку вызовов. Исходная таблица должна соответствовать исходному файлу кадра стека, но с суффиксом`.map`
3. Перетащите карты источника в стек вызовов в портал Azure![](https://i.imgur.com/Efue9nU.gif)

### <a name="application-insights-web-basic"></a>Application Insights Web Basic

Для упрощения работы можно вместо этого установить базовую версию Application Insights
```
npm i --save @microsoft/applicationinsights-web-basic
```
Эта версия поставляется с минимальным числом функций и функциональных возможностей и зависит от того, как вы видите. Например, он не выполняет Автосбор (неперехваченные исключения, AJAX и т. д.). API-интерфейсы для отправки определенных типов телеметрии, `trackTrace`например `trackException`, и т. д., не включены в эту версию, поэтому необходимо предоставить собственную оболочку. Единственный доступный API — `track`. [Пример](https://github.com/Azure-Samples/applicationinsights-web-sample1/blob/master/testlightsku.html) находится здесь.

## <a name="examples"></a>Примеры

Примеры готовности к запуску см. в разделе [примеры пакетов SDK для Application Insights JavaScript](https://github.com/topics/applicationinsights-js-demo)

## <a name="upgrading-from-the-old-version-of-application-insights"></a>Обновление старой версии Application Insights

Критические изменения в версии пакета SDK v2:
- Чтобы обеспечить лучшую сигнатуру API, некоторые вызовы API, такие как trackPageView и trackException, были обновлены. Запуск в Internet Explorer 8 и более ранних версиях браузера не поддерживается.
- В связи с обновлением схемы данных в конверте телеметрии изменились имя и структура поля.
- `context.operation` Перемещено `context.telemetryTrace`в. Некоторые поля были также изменены (`operation.id` --> `telemetryTrace.traceID`).
  - Чтобы вручную обновить текущий идентификатор pageview (например, в приложениях SPA), используйте `appInsights.properties.context.telemetryTrace.traceID = Util.generateW3CId()`.
    > [!NOTE]
    > Чтобы идентификатор трассировки был уникальным, где ранее использовался `Util.newId()`, теперь используйте. `Util.generateW3CId()` Как и в конечном итоге, это идентификатор операции.

Если вы используете текущий пакет SDK Application Insights (1.0.20) и хотите узнать, работает ли новый пакет SDK в среде выполнения, обновите URL-адрес в зависимости от текущего сценария загрузки пакета SDK.

- Загрузка с помощью сценария CDN. Обновите фрагмент кода, который сейчас используется для указания следующего URL-адреса:
   ```
   "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
   ```

- сценарий NPM. вызовите `downloadAndSetup` , чтобы скачать полный скрипт APPLICATIONINSIGHTS из CDN и инициализировать его с помощью ключа инструментирования:

   ```ts
   appInsights.downloadAndSetup({
     instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx",
     url: "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
     });
   ```

Протестируйте во внутренней среде, чтобы убедиться, что данные телеметрии мониторинга работают должным образом. Если все работает, обновите сигнатуры API в соответствии с версией пакета SDK версии 2 и выполните развертывание в рабочих средах.

## <a name="sdk-performanceoverhead"></a>Производительность и затраты на пакет SDK

Всего за 25 КБ со сжатием gzip и для инициализации требуется только ~ 15 мс, Application Insights добавляет незначительное количество лоадтиме на веб-сайт. С помощью фрагмента кода можно быстро загрузить минимальные компоненты библиотеки. В то же время полный скрипт загружается в фоновом режиме.

Хотя скрипт скачивается из CDN, все отслеживание вашей страницы помещается в очередь. Когда загруженный скрипт завершает асинхронную инициализацию, отправляются все события, которые были поставлены в очередь. В результате все данные телеметрии не будут потеряны в течение всего жизненного цикла страницы. Этот процесс установки обеспечивает полную систему анализа, невидимую для пользователей.

> Сводка.
> - **25 КБ** со сжатием gzip
> - **15 мс** общее время инициализации
> - **Нулевое** отслеживание, пропущенное во время жизненного цикла страницы

## <a name="browser-support"></a>Поддержка браузеров

![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![FireFox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![IE](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)
--- | --- | --- | --- | --- |
Последняя ✔ Chrome |  Последние ✔ Firefox | Internet Explorer 9 + & пограничных ✔ | ✔ Opera последней версии | Самый последний ✔ Safari |

## <a name="open-source-sdk"></a>Пакет SDK с открытым исходным кодом

Application Insights SDK JavaScript является открытым кодом для просмотра исходного кода или для участия в проекте посетите [официальный репозиторий GitHub](https://github.com/Microsoft/ApplicationInsights-JS).

## <a name="next-steps"></a><a name="next"></a> Дальнейшие действия
* [Отслеживание использования](usage-overview.md)
* [Пользовательские события и метрики](api-custom-events-metrics.md)
* [Сборка, измерение и обучение](usage-overview.md)
