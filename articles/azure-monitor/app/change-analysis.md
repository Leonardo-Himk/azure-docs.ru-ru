---
title: Использование анализа изменений приложений в Azure Monitor для поиска проблем с веб-приложениями | Документация Майкрософт
description: Используйте анализ изменений приложений в Azure Monitor для устранения проблем с приложениями на активных веб-сайтах в службе приложений Azure.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/04/2020
ms.openlocfilehash: c287a2315f2b2319a6873ce84ee0e4e48bec8444
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836819"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Использование анализа изменений приложений (Предварительная версия) в Azure Monitor

При возникновении проблем на рабочем сайте или в случае сбоя быстро определить основную причину очень важно. Стандартные решения для мониторинга могут сообщить о проблеме. Они могут даже указывать, какой компонент не работает. Но это предупреждение не всегда немедленно объясняет причину сбоя. Вы уверены, что ваш сайт работал пять минут назад, и теперь он нарушен. Что изменилось за последние пять минут? Это вопрос, что анализ изменений приложения предназначен для ответа в Azure Monitor.

Основываясь на возможностях [графа ресурсов Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview), анализ изменений позволяет получить ценные сведения об изменениях в приложении Azure, чтобы увеличить наблюдаемость и сократить MTTR (среднее время ремонта).

> [!IMPORTANT]
> Анализ изменений сейчас находится на этапе предварительной версии. Эта предварительная версия предоставляется без соглашения об уровне обслуживания. Эта версия не рекомендуется для рабочих нагрузок в рабочей среде. Некоторые функции могут не поддерживаться или иметь ограниченные возможности. Дополнительные сведения см. в разделе Дополнительные [условия использования для предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Обзор

Средство анализа изменений обнаруживает различные типы изменений, от уровня инфраструктуры до развертывания приложений. Это поставщик ресурсов Azure уровня подписки, который проверяет изменения ресурсов в подписке. Анализ изменений предоставляет данные для различных диагностических средств, которые помогают пользователям понять, какие изменения могут вызвать проблемы.

На следующей схеме показана архитектура анализа изменений.

![Схема архитектуры, с которой средство анализа изменений получает данные об изменениях и предоставляет их клиентским средствам](./media/change-analysis/overview.png)

## <a name="data-sources"></a>Источники данных

Запросы на анализ изменений приложений для Azure Resource Manager отслеживанию свойств, прокси-конфигураций и изменений в гостевых приложениях. Кроме того, служба отслеживает изменения зависимостей ресурсов для диагностики и отслеживания комплексного приложения.

### <a name="azure-resource-manager-tracked-properties-changes"></a>Изменения Azure Resource Managerных отслеживаний свойств

С помощью [графа ресурсов Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview)анализ изменений позволяет вести историю того, как ресурсы Azure, на которых размещены приложения, изменились с течением времени. Могут быть обнаружены такие параметры, как управляемые удостоверения, обновление ОС платформы и имена узлов.

### <a name="azure-resource-manager-proxied-setting-changes"></a>Изменения параметров Azure Resource Manager прокси-сервера

Такие параметры, как правило IP-конфигурации, параметры TLS и версии расширений, пока недоступны в графе ресурсов Azure, поэтому запросы на анализ изменений и повторное вычисление этих изменений позволяют получить дополнительные сведения о том, что изменилось в приложении.

### <a name="changes-in-web-app-deployment-and-configuration-in-guest-changes"></a>Изменения в развертывании и настройке веб-приложения (изменения в гостевой системе)

Анализ изменений отслеживает развертывание и состояние конфигурации приложения каждые 4 часа. Это может обнаружить, например, изменения в переменных среды приложения. Средство вычислит различия и повлечет за собой изменения. В отличие от диспетчер ресурсов изменения, сведения об изменении развертывания кода могут быть недоступны сразу же в средстве. Чтобы просмотреть последние изменения в анализе изменений, выберите **Обновить**.

![Снимок экрана: кнопка "проверить изменения сейчас"](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Изменения зависимостей

Изменения зависимостей ресурсов также могут вызвать проблемы в веб-приложении. Например, если веб-приложение вызывает кэш Redis, номер SKU кэша Redis может повлиять на производительность веб-приложения. Для обнаружения изменений в зависимостях анализ изменений проверяет запись DNS веб-приложения. Таким образом, он определяет изменения во всех компонентах приложений, которые могут вызвать проблемы.
В настоящее время поддерживаются следующие зависимости:
- Веб-приложения
- Хранилище Azure
- Azure SQL

## <a name="application-change-analysis-service"></a>Служба анализа изменений приложений

Служба анализа изменений приложений вычисляет и объединяет данные изменений из источников данных, упомянутых выше. Он предоставляет набор аналитиков для пользователей, позволяющих легко перемещаться по всем изменениям ресурсов и определить, какие изменения важны в контексте устранения неполадок или мониторинга.
Поставщик ресурсов Microsoft. Чанжеаналисис должен быть зарегистрирован в подписке для Azure Resource Manager отслеживанию свойств и изменение параметров прокси-сервера для доступа к данным. При вводе средства "Диагностика и устранение неполадок" веб-приложения или на автономной вкладке "анализ изменений" Этот поставщик ресурсов регистрируется автоматически. В ней нет никаких реализаций производительности или затрат для вашей подписки. При включении анализа изменений для веб-приложений (или включении средства "Диагностика и устранение проблем") это приведет к незначительному снижению производительности веб-приложения и стоимости выставления счетов.
Для изменений в гостевом веб-приложении требуется отдельное включение для сканирования файлов кода в веб-приложении. Дополнительные сведения см. в разделе [анализ изменений в средстве "Диагностика и устранение проблем"](https://docs.microsoft.com/azure/azure-monitor/app/change-analysis#application-change-analysis-in-the-diagnose-and-solve-problems-tool) далее в этой статье.

## <a name="visualizations-for-application-change-analysis"></a>Визуализации для анализа изменений приложений

### <a name="standalone-ui"></a>Автономный пользовательский интерфейс

В Azure Monitor имеется изолированная панель для анализа изменений, позволяющая просматривать все изменения, содержащие сведения о зависимостях и ресурсах приложений.

Выполните поиск по запросу анализ изменений в строке поиска на портал Azure, чтобы запустить интерфейс.

![Снимок экрана: Поиск изменений в портал Azure](./media/change-analysis/search-change-analysis.png)

Все ресурсы в выбранной подписке отображаются с изменениями за последние 24 часа. Для оптимизации производительности загрузки страницы Служба отображает 10 ресурсов за раз. Щелкните следующие страницы, чтобы просмотреть дополнительные ресурсы. Мы работаем над удалением этого ограничения.

![Снимок экрана: колонка "анализ изменений" в портал Azure](./media/change-analysis/change-analysis-standalone-blade.png)

Щелкнув ресурс, можно просмотреть все его изменения. При необходимости выполните детализацию, чтобы просмотреть подробные сведения об изменениях в формате JSON и аналитику.

![Снимок экрана с сведениями об изменении](./media/change-analysis/change-details.png)

Для получения отзывов используйте кнопку Отправить отзыв в колонке или электронном письме changeanalysisteam@microsoft.com.

![Снимок экрана: кнопка "Отзывы" в колонке "анализ изменений"](./media/change-analysis/change-analysis-feedback.png)

### <a name="web-app-diagnose-and-solve-problems"></a>Диагностика и устранение проблем веб-приложений

В Azure Monitor анализ изменений также встроен в самостоятельную **диагностику и решение проблем** . Доступ к этому интерфейсу осуществляется со страницы **Обзор** приложения службы приложений.

![Снимок экрана с кнопкой "Обзор" и кнопкой "Диагностика и устранение проблем"](./media/change-analysis/change-analysis.png)

### <a name="application-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Анализ изменений приложений в средстве "Диагностика и устранение проблем"

Анализ изменений приложений — это автономное средство обнаружения в средствах диагностики и устранения неполадок веб-приложений. Кроме того, она обрабатывается в **сбоях приложений** и при **обнаружении веб-приложений**. При вводе средства "Диагностика и устранение проблем" поставщик ресурсов **Microsoft. чанжеаналисис** будет зарегистрирован автоматически. Выполните эти инструкции, чтобы включить отслеживание изменений в гостевом веб-приложении.

1. Выберите **доступность и производительность**.

    ![Снимок экрана с параметрами устранения неполадок "доступность и производительность"](./media/change-analysis/availability-and-performance.png)

2. Выберите **изменения приложения**. Эта функция также доступна в случае **сбоев приложений**.

   ![Снимок экрана: кнопка "сбои приложения"](./media/change-analysis/application-changes.png)

3. Чтобы включить анализ изменений, выберите **Включить сейчас**.

   ![Снимок экрана: параметры "сбои приложений"](./media/change-analysis/enable-changeanalysis.png)

4. Включите **анализ изменений** и нажмите кнопку **сохранить**. Средство отображает все веб-приложения в рамках плана службы приложений. Чтобы включить анализ изменений для всех веб-приложений в рамках плана, можно использовать параметр уровень плана.

    ![Снимок экрана: Пользовательский интерфейс "Включение анализа изменений"](./media/change-analysis/change-analysis-on.png)

5. Чтобы получить доступ к анализу изменений, выберите **Диагностика и устранение проблем** > **со** > **сбоями**и производительность приложения. Вы увидите граф, который суммирует тип изменений со временем, а также сведения об этих изменениях. По умолчанию за последние 24 часа отображаются изменения, которые помогут устранить немедленные проблемы.

     ![Снимок экрана представления "изменение инструмента сравнения"](./media/change-analysis/change-view.png)

### <a name="enable-change-analysis-at-scale"></a>Включить анализ изменений в масштабе

Если ваша подписка включает множество веб-приложений, включение службы на уровне веб-приложения будет неэффективным. Выполните следующий скрипт, чтобы включить все веб-приложения в подписке.

Предварительные требования:

- Модуль PowerShell AZ. Следуйте инструкциям по [установке модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.6.0)

Выполните следующий скрипт:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"

# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```

### <a name="virtual-machine-diagnose-and-solve-problems"></a>Диагностика и устранение проблем виртуальной машины

Перейдите к средству диагностика и устранение проблем для виртуальной машины.  Перейдите в раздел **средства устранения неполадок**, перейдите на страницу и выберите **Анализ недавних изменений** , чтобы просмотреть изменения на виртуальной машине.

![Снимок экрана: диагностика и устранение проблем виртуальной машины](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Снимок экрана: диагностика и устранение проблем виртуальной машины](./media/change-analysis/analyze-recent-changes.png)

## <a name="next-steps"></a>Дальнейшие действия

- Включите Application Insights для [приложений служб приложений Azure](azure-web-apps.md).
- Включите Application Insights для [виртуальных машин Azure и масштабируемых наборов виртуальной машины Azure, размещенных в IIS](azure-vm-vmss-apps.md).
- Узнайте больше о [графе ресурсов Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview), который помогает анализировать изменения в Power On.
