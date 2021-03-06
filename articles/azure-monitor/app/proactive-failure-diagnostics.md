---
title: Интеллектуальное обнаружение аномальных сбоев в Application Insights | Документация Майкрософт
description: Оповещает о необычных изменениях в частоте неудачных запросов для веб-приложения, а также выполняет диагностический анализ. Настройка не требуется.
ms.topic: conceptual
ms.date: 12/18/2018
ms.reviewer: yalavi
ms.openlocfilehash: a1bce3ab86748d8247a72da3bd70e0f2e8155dbf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536817"
---
# <a name="smart-detection---failure-anomalies"></a>Интеллектуальное обнаружение аномальных сбоев
[Application Insights](../../azure-monitor/app/app-insights-overview.md) автоматически предупреждает вас в режиме реального времени, если веб-приложение испытывает ненормальное увеличение частоты неудачных запросов. Эта служба обнаруживает необычное увеличение числа невыполненных HTTP-запросов или вызовов зависимостей. Для запросов неудачные запросы обычно имеют коды ответов 400 или выше. Чтобы помочь вам в рассмотрении и диагностике проблемы, в подробных сведениях о предупреждении приводится анализ характеристик сбоев и связанных с ними данных приложений. Кроме того, даются ссылки на портал Application Insights для дальнейшей диагностики. Эта функция не требует настройки, так как использует алгоритмы машинного обучения для прогнозирования нормальной частоты невыполненных запросов.

Эта функция работает для любого веб-приложения, размещенного в облаке или на собственных серверах, которые создают запрос приложения или данные зависимостей. Например, если у вас есть рабочая роль, которая вызывает [TrackRequest ()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) или [TrackDependency ()](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

После настройки [Application Insights для проекта](../../azure-monitor/app/app-insights-overview.md), если приложение создает определенный минимальный объем данных, интеллектуальное обнаружение аномалий сбоев занимает 24 часа, чтобы узнать о нормальном поведении приложения, прежде чем оно будет включено и сможет отсылать предупреждения.

Вот пример оповещения:

[![](./media/proactive-failure-diagnostics/013.png "Sample smart detection alert showing cluster analysis around failure")](./media/proactive-failure-diagnostics/013.png#lightbox)

Сведения о предупреждении помогут вам:

* Количество сбоев по сравнению с обычным поведением приложения.
* Количество затронутых пользователей, чтобы вы знали, насколько все серьезно.
* Шаблон характеристик, связанный с ошибками. В этом примере присутствует определенный код ответа, имя запроса (операция) и версия приложения. Это позволяет сразу же определить фрагмент кода, с которого следует начать поиск. К другим возможностям может относиться определенный тип браузера или клиентской операционной системы.
* Исключения, трассировки журналов и сбои в зависимостях (базах данных или других внешних компонентах), которые могут быть связаны с описанными ошибками.
* Прямые ссылки на соответствующие поисковые запросы к данным в Application Insights.

## <a name="benefits-of-smart-detection"></a>Преимущества интеллектуального обнаружения
Обычные [оповещения метрик](../../azure-monitor/app/alerts.md) сообщают о возможном наличии проблемы. Но интеллектуальное обнаружение запускает диагностику за вас, выполняя большую часть анализа, которую в противном случае придется выполнить самостоятельно. Вы получаете тщательно подобранные результаты, что помогает быстро выявить первопричину проблемы.

## <a name="how-it-works"></a>Принцип работы
Интеллектуальное обнаружение отслеживает данные, полученные из вашего приложения, и в частности, на определенные уровни сбоев. Это правило подсчитывает число запросов, для которых свойство `Successful request` имеет значение false, и количество вызовов зависимостей, для которых свойство `Successful call` имеет значение false. По умолчанию для запросов `Successful request == (resultCode < 400)` (если вы не написали пользовательский код для [фильтрации](../../azure-monitor/app/api-filtering-sampling.md#filtering) или создания собственных вызовов [TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)). 

Производительность вашего приложения демонстрирует типичный шаблон поведения. Одни запросы или вызовы зависимостей будут более подвержены сбоям, чем другие, а общий процент сбоев может возрастать при увеличении нагрузки. Для выявления таких аномалий система интеллектуального обнаружения использует машинное обучение.

По мере того, как данные поступают в Application Insights из веб-приложения, интеллектуальное обнаружение сравнивает текущее поведение с шаблонами, наблюдаемыми за последние несколько дней. Если по сравнению с предыдущими показателями производительности наблюдается чрезмерное увеличение частоты отказов, запускается анализ.

При запуске анализа служба анализирует кластер по неудачному запросу, чтобы попытаться определить шаблон значений, который характеризует отказы. 

В приведенном выше примере анализ обнаружил, что большинство сбоев связаны с конкретным кодом ошибки, именем запроса, URL-адресом сервера и экземпляром роли. 

Когда служба оснащена этими вызовами, анализатор ищет исключение и ошибку зависимости, связанные с запросами в обнаруженном кластере, а также примеры журналов трассировки, связанных с этими запросами.

Результат анализа отправляется в виде оповещения, если в настройках не указан иной способ.

Как и [оповещения, заданные вручную](../../azure-monitor/app/alerts.md), можно проверить состояние запущенного предупреждения, которое можно устранить, если проблема устранена. Настройте правила генерации оповещений на странице "оповещения" Application Insights ресурса. В отличие от других оповещений, здесь не требуется настройка интеллектуального обнаружения. При желании адреса электронной почты для отправки оповещений можно отключить или изменить.

### <a name="alert-logic-details"></a>Сведения о логике оповещений

Оповещения запускаются нашим собственным алгоритмом машинного обучения, поэтому мы не можем предоставить точные сведения о реализации. Однако мы понимаем, что иногда пользователям требуется более глубокое представление о работе базовой логики. Ниже приводятся основные факторы, учитываемые при определении необходимости запуска оповещения. 

* Анализ процента неудачных запросов и зависимостей в скользящем временном окне, составляющем 20 минут.
* Сравнение процента сбоев за последние 20 минут с коэффициентом за последние 40 минут и последние семь дней и определение существенных отклонений, которые в несколько раз превышают стандартное отклонение.
* Использование адаптивного ограничения для минимального процента сбоев, который зависит от объема запросов и зависимостей приложения.
* Существует логика, которая может автоматически разрешить запущенное условие монитора предупреждений, если проблема больше не обнаруживается в течение 8-24 часов.

## <a name="configure-alerts"></a>Настройка оповещений

Правило оповещения интеллектуального обнаружения можно отключить на портале или с помощью Azure Resource Manager ([см. Пример шаблона](https://docs.microsoft.com/azure/azure-monitor/app/proactive-arm-config)).

Это правило генерации оповещений создается с помощью связанной [группы действий](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) с именем "Application Insights интеллектуального обнаружения", которая содержит действия электронной почты и веб-перехватчика, и может быть расширена для активации дополнительных действий при срабатывании оповещения.

> [!NOTE]
> Уведомления по электронной почте, отправленные из этого правила оповещения, теперь по умолчанию отправляются пользователям, связанным с ролями "читатель мониторинга" и "Мониторинг участников подписки". Дополнительные сведения об этом можно найти [здесь](https://docs.microsoft.com/azure/azure-monitor/app/proactive-email-notification).
> Уведомления, отправленные из этого правила оповещения, соответствуют [общей схеме предупреждений](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema).
>

Откройте страницу "Оповещения". Правила генерации оповещений об ошибках включаются вместе со всеми оповещениями, настроенными вручную, и можно проверить, находится ли он в состоянии оповещения.

[![](./media/proactive-failure-diagnostics/021.png "On the Application Insights resource page, click 'Alerts' tile, then 'Manage alert rules'")](./media/proactive-failure-diagnostics/021.png#lightbox)

Щелкните оповещение, чтобы его настроить.

[![](./media/proactive-failure-diagnostics/032.png "Rule configuration screen")](./media/proactive-failure-diagnostics/032.png#lightbox)

Обратите внимание, что вы можете отключить или удалить правило генерации оповещений об ошибках, но не можете создать другое на том же ресурсе Application Insights.

## <a name="example-of-failure-anomalies-alert-webhook-payload"></a>Пример полезных данных веб-перехватчика предупреждения об аномалиях ошибок

```json
{
    "properties": {
        "essentials": {
            "severity": "Sev3",
            "signalType": "Log",
            "alertState": "New",
            "monitorCondition": "Resolved",
            "monitorService": "Smart Detector",
            "targetResource": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourcegroups/test-group/providers/microsoft.insights/components/test-rule",
            "targetResourceName": "test-rule",
            "targetResourceGroup": "test-group",
            "targetResourceType": "microsoft.insights/components",
            "sourceCreatedId": "1a0a5b6436a9b2a13377f5c89a3477855276f8208982e0f167697a2b45fcbb3e",
            "alertRule": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourcegroups/test-group/providers/microsoft.alertsmanagement/smartdetectoralertrules/failure anomalies - test-rule",
            "startDateTime": "2019-10-30T17:52:32.5802978Z",
            "lastModifiedDateTime": "2019-10-30T18:25:23.1072443Z",
            "monitorConditionResolvedDateTime": "2019-10-30T18:25:26.4440603Z",
            "lastModifiedUserName": "System",
            "actionStatus": {
                "isSuppressed": false
            },
            "description": "Failure Anomalies notifies you of an unusual rise in the rate of failed HTTP requests or dependency calls."
        },
        "context": {
            "DetectionSummary": "An abnormal rise in failed request rate",
            "FormattedOccurenceTime": "2019-10-30T17:50:00Z",
            "DetectedFailureRate": "50.0% (200/400 requests)",
            "NormalFailureRate": "0.0% (over the last 30 minutes)",
            "FailureRateChart": [["2019-10-30T05:20:00Z",
            0],
            ["2019-10-30T05:40:00Z",
            100],
            ["2019-10-30T06:00:00Z",
            0],
            ["2019-10-30T06:20:00Z",
            0],
            ["2019-10-30T06:40:00Z",
            100],
            ["2019-10-30T07:00:00Z",
            0],
            ["2019-10-30T07:20:00Z",
            0],
            ["2019-10-30T07:40:00Z",
            100],
            ["2019-10-30T08:00:00Z",
            0],
            ["2019-10-30T08:20:00Z",
            0],
            ["2019-10-30T08:40:00Z",
            100],
            ["2019-10-30T17:00:00Z",
            0],
            ["2019-10-30T17:20:00Z",
            0],
            ["2019-10-30T09:00:00Z",
            0],
            ["2019-10-30T09:20:00Z",
            0],
            ["2019-10-30T09:40:00Z",
            100],
            ["2019-10-30T10:00:00Z",
            0],
            ["2019-10-30T10:20:00Z",
            0],
            ["2019-10-30T10:40:00Z",
            100],
            ["2019-10-30T11:00:00Z",
            0],
            ["2019-10-30T11:20:00Z",
            0],
            ["2019-10-30T11:40:00Z",
            100],
            ["2019-10-30T12:00:00Z",
            0],
            ["2019-10-30T12:20:00Z",
            0],
            ["2019-10-30T12:40:00Z",
            100],
            ["2019-10-30T13:00:00Z",
            0],
            ["2019-10-30T13:20:00Z",
            0],
            ["2019-10-30T13:40:00Z",
            100],
            ["2019-10-30T14:00:00Z",
            0],
            ["2019-10-30T14:20:00Z",
            0],
            ["2019-10-30T14:40:00Z",
            100],
            ["2019-10-30T15:00:00Z",
            0],
            ["2019-10-30T15:20:00Z",
            0],
            ["2019-10-30T15:40:00Z",
            100],
            ["2019-10-30T16:00:00Z",
            0],
            ["2019-10-30T16:20:00Z",
            0],
            ["2019-10-30T16:40:00Z",
            100],
            ["2019-10-30T17:30:00Z",
            50]],
            "ArmSystemEventsRequest": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourceGroups/test-group/providers/microsoft.insights/components/test-rule/query?query=%0d%0a++++++++++++++++systemEvents%0d%0a++++++++++++++++%7c+where+timestamp+%3e%3d+datetime(%272019-10-30T17%3a20%3a00.0000000Z%27)+%0d%0a++++++++++++++++%7c+where+itemType+%3d%3d+%27systemEvent%27+and+name+%3d%3d+%27ProactiveDetectionInsight%27+%0d%0a++++++++++++++++%7c+where+dimensions.InsightType+in+(%275%27%2c+%277%27)+%0d%0a++++++++++++++++%7c+where+dimensions.InsightDocumentId+%3d%3d+%27718fb0c3-425b-4185-be33-4311dfb4deeb%27+%0d%0a++++++++++++++++%7c+project+dimensions.InsightOneClassTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightExceptionCorrelationTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightDependencyCorrelationTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightRequestCorrelationTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightTraceCorrelationTable%0d%0a++++++++++++&api-version=2018-04-20",
            "LinksTable": [{
                "Link": "<a href=\"https://portal.azure.com/#blade/AppInsightsExtension/ProactiveDetectionFeedBlade/ComponentId/{\"SubscriptionId\":\"4f9b81be-fa32-4f96-aeb3-fc5c3f678df9\",\"ResourceGroup\":\"test-group\",\"Name\":\"test-rule\"}/SelectedItemGroup/718fb0c3-425b-4185-be33-4311dfb4deeb/SelectedItemTime/2019-10-30T17:50:00Z/InsightType/5\" target=\"_blank\">View full details in Application Insights</a>"
            }],
            "SmartDetectorId": "FailureAnomaliesDetector",
            "SmartDetectorName": "Failure Anomalies",
            "AnalysisTimestamp": "2019-10-30T17:52:32.5802978Z"
        },
        "egressConfig": {
            "displayConfig": [{
                "rootJsonNode": null,
                "sectionName": null,
                "displayControls": [{
                    "property": "DetectionSummary",
                    "displayName": "What was detected?",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "property": "FormattedOccurenceTime",
                    "displayName": "When did this occur?",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "property": "DetectedFailureRate",
                    "displayName": "Detected failure rate",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "property": "NormalFailureRate",
                    "displayName": "Normal failure rate",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "chartType": "Line",
                    "xAxisType": "Date",
                    "yAxisType": "Percentage",
                    "xAxisName": "",
                    "yAxisName": "",
                    "property": "FailureRateChart",
                    "displayName": "Failure rate over last 12 hours",
                    "type": "Chart",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "defaultLoad": true,
                    "displayConfig": [{
                        "rootJsonNode": null,
                        "sectionName": null,
                        "displayControls": [{
                            "showHeader": false,
                            "columns": [{
                                "property": "Name",
                                "displayName": "Name"
                            },
                            {
                                "property": "Value",
                                "displayName": "Value"
                            }],
                            "property": "tables[0].rows[0][0]",
                            "displayName": "All of the failed requests had these characteristics:",
                            "type": "Table",
                            "isOptional": false,
                            "isPropertySerialized": true
                        }]
                    }],
                    "property": "ArmSystemEventsRequest",
                    "displayName": "",
                    "type": "ARMRequest",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "showHeader": false,
                    "columns": [{
                        "property": "Link",
                        "displayName": "Link"
                    }],
                    "property": "LinksTable",
                    "displayName": "Links",
                    "type": "Table",
                    "isOptional": false,
                    "isPropertySerialized": false
                }]
            }]
        }
    },
    "id": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourcegroups/test-group/providers/microsoft.insights/components/test-rule/providers/Microsoft.AlertsManagement/alerts/7daf8739-ca8a-4562-b69a-ff28db4ba0a5",
    "type": "Microsoft.AlertsManagement/alerts",
    "name": "Failure Anomalies - test-rule"
}
```

## <a name="triage-and-diagnose-an-alert"></a>Рассмотрение и диагностика оповещений

Оповещение означает, что обнаружен чрезмерный рост частоты невыполненных запросов. Скорее всего, проблема возникла в приложении или его среде.

Для дальнейшего изучения щелкните "просмотреть полные сведения в Application Insights" ссылки на этой странице помогут перейти на [страницу поиска](../../azure-monitor/app/diagnostic-search.md) , отфильтрованную по соответствующим запросам, исключениям, зависимостям или трассировкам. 

Можно также открыть [портал Azure](https://portal.azure.com), перейти к ресурсу Application Insights приложения и открыть страницу сбои.

Щелкнув "Диагностика сбоев", вы сможете получить дополнительные сведения и устранить проблему.

[![](./media/proactive-failure-diagnostics/051.png "Diagnostic search")](./media/proactive-failure-diagnostics/051.png#lightbox)

В зависимости от процента запросов и числа вовлеченных пользователей вы можете решить, насколько срочно необходимо решить проблему. В приведенном выше примере частота сбоев 78,5% сравнивается со стандартной частотой 2,2%, что указывает на то, что происходит неправильное выполнение. С другой стороны, затронуты только 46 пользователей. Если бы это было ваше приложение, вы сможете оценить степень его серьезности.

Во многих случаях вы сможете быстро диагностировать проблему по имени запроса, исключению, сбою зависимости и данным трассировки.

В этом примере произошло исключение из базы данных SQL из-за достижения лимита запросов.

[![](./media/proactive-failure-diagnostics/052.png "Failed request details")](./media/proactive-failure-diagnostics/052.png#lightbox)

## <a name="review-recent-alerts"></a>Просмотр последних оповещений

Щелкните **оповещения** на странице ресурсов Application Insights, чтобы получить последние инициированные оповещения.

[![](./media/proactive-failure-diagnostics/070.png "Alerts summary")](./media/proactive-failure-diagnostics/070.png#lightbox)

## <a name="whats-the-difference-"></a>В чем разница?
Интеллектуальное обнаружение аномальных сбоев дополняет другие похожие компоненты Application Insights.

* [Оповещения метрик](../../azure-monitor/app/alerts.md) задаются пользователем и позволяют отслеживать самые разные метрики, включая нагрузку на ЦП, частоту запросов, время загрузки страниц и т. д. Их можно использовать, например, для того, чтобы добавить ресурсы. С другой стороны, интеллектуальное обнаружение аномалий сбоев охватывает небольшой диапазон критически важных метрик (в настоящее время только частота неудачных запросов), предназначенный для уведомления пользователя практически в реальном времени после того, как частота неудачных запросов веб-приложения увеличится по сравнению с нормальным поведением веб-приложения. В отличие от оповещений метрик, интеллектуальное обнаружение автоматически устанавливает и обновляет пороговые значения в ответ на изменения в работе. Интеллектуальное обнаружение также запускает диагностическую работу, экономя время на устранение проблем.

* [Система интеллектуального обнаружение аномалий производительности](proactive-performance-diagnostics.md) использует искусственный интеллект для обнаружения необычного поведения ваших метрик без необходимости в настройке. В отличие от интеллектуального обнаружения аномальных сбоев, эта функция предназначена для поиска плохо обслуживаемых сегментов вашей системы (например, определенных страниц в определенном типе браузера). Анализ выполняется ежедневно, и обнаруженные результаты обычно не требуют таких срочных действий, как оповещения. В отличие от этого, анализ аномалий сбоев выполняется непрерывно для входящих данных приложения, и вы будете получать уведомления в течение нескольких минут, если частота сбоев сервера превышает ожидаемую.

## <a name="if-you-receive-a-smart-detection-alert"></a>При возникновении оповещения интеллектуального обнаружения
*Почему мне направлено это предупреждение?*

* Мы обнаружили аномальное увеличение количества неудачных запросов по сравнению с обычными показателями за предыдущий период. После анализа сбоев и связанных данных приложений мы думаем, что есть проблема, с которой следует обратить внимание.

*Уведомление означает, что определенно возникла какая-то неполадка?*

* Мы оповещать о нарушении или ухудшении работы приложения, однако только вы имеете полное представление о семантике и влиянии на приложение или пользователей.

*Итак, вы просматриваете мои данные приложения?*

* Нет. Служба работает полностью в автоматическом режиме. Уведомления получаете только вы. Ваши данные остаются [конфиденциальными](../../azure-monitor/app/data-retention-privacy.md).

*Нужно ли мне подписаться на это оповещение?*

* Нет. Каждое приложение, отправляющее данные запроса, имеет правило оповещения Smart Detection.

*Можно отменить подписку или настроить пересылку уведомлений моим коллегам?*

* Да, в области "Правила генерации оповещений" щелкните правило интеллектуального обнаружения, чтобы настроить его. Вы можете отключить это оповещение или изменить его получателей.

*Не могу найти письмо. Где найти уведомления на портале?*

* В журналах действий. В Azure откройте ресурс Application Insights для своего приложения и выберите "Журналы действий".

*Некоторые оповещения относятся к известным проблемам, и я не хочу получать их.*

* Можно использовать функцию подавления [правил предупреждений](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-action-rules) .

## <a name="next-steps"></a>Дальнейшие шаги
Эти средства диагностики помогают проверить данные приложения:

* [Обозреватель метрик](../../azure-monitor/platform/metrics-charts.md)
* [Обозреватель поиска](../../azure-monitor/app/diagnostic-search.md)
* [Аналитика, мощный язык запросов](../../azure-monitor/log-query/get-started-portal.md)

Интеллектуальные обнаружения являются автоматическими. но, возможно, вам потребуется настроить некоторые дополнительные оповещения.

* [Настройка оповещений в Application Insights](../../azure-monitor/app/alerts.md)
* [Доступность веб-тестов](../../azure-monitor/app/monitor-web-app-availability.md)
