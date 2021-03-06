---
title: Создание ресурса Azure Application Insights | Документация Майкрософт
description: Вручную настройте мониторинг Application Insights для нового работающего приложения.
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 4d8979469ca83dfd6b81aab10191e8fbf36104ff
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83200663"
---
# <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights

Azure Application Insights отображает данные о приложении в *ресурсе*Microsoft Azure. Таким образом, создание ресурса является частью [настройки Application Insights для мониторинга нового приложения][start]. После создания нового ресурса можно получить его ключ инструментирования и использовать его для настройки пакета SDK для Application Insights. Ключ инструментирования связывает данные телеметрии с ресурсом.

## <a name="sign-in-to-microsoft-azure"></a>Войдите в Microsoft Azure

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

## <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights

Войдите в [портал Azure](https://portal.azure.com)и создайте ресурс Application Insights:

![Щелкните знак "+" в левом верхнем углу. Выберите Средства для разработчиков, за которым следует Application Insights](./media/create-new-resource/new-app-insights.png)

   | "Настройки"        |  Значение           | Описание  |
   | ------------- |:-------------|:-----|
   | **имя**;      | `Unique value` | Имя, идентифицирующее отслеживаемое приложение. |
   | **Группа ресурсов**     | `myResourceGroup`      | Имя новой или существующей группы ресурсов для размещения данных App Insights. |
   | **Регион** | `East US` | Выберите расположение рядом с вами или рядом с ним, где размещено ваше приложение. |
   | **Режим ресурсов** | `Classic` или `Workspace-based` | Ресурсы на основе рабочей области в настоящее время доступны в общедоступной предварительной версии и позволяют отправить данные телеметрии Application Insights в общую рабочую область Log Analytics. Дополнительные сведения см. в [статье о ресурсах на основе рабочей области](create-workspace-resource.md).

> [!NOTE]
> Хотя одно и то же имя ресурса можно использовать в разных группах ресурсов, может быть полезно использовать глобально уникальное имя. Это может быть полезно, если планируется [выполнять запросы перекрестных запросов](https://docs.microsoft.com/azure/azure-monitor/log-query/cross-workspace-query#identifying-an-application) , так как это упрощает необходимый синтаксис.

Введите соответствующие значения в обязательные поля, а затем выберите **проверить и создать**.

![Введите значения в обязательные поля, а затем выберите "Проверка + создать".](./media/create-new-resource/review-create.png)

После создания приложения откроется новая область. В этой области отображаются данные о производительности и использовании отслеживаемого приложения. 

## <a name="copy-the-instrumentation-key"></a>Копирование ключа инструментирования

Ключ инструментирования определяет ресурс, с которым необходимо связать данные телеметрии. Необходимо скопировать ключ инструментирования и добавить его в код приложения.

![Щелкните и скопируйте ключ инструментирования](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>Установка пакета SDK в приложении

Установите пакет SDK Application Insights в приложении. Выполнение этого шага зависит от типа приложения.

Используйте ключ инструментирования для настройки [пакета SDK, который можно установить в приложении][start].

Пакет SDK включает стандартные модули, которые отправляют данные телеметрии без необходимости написания дополнительного кода. Для более подробного отслеживания действий пользователей или диагностики неполадок отправляйте собственные данные телеметрии, [используя API][api].

## <a name="creating-a-resource-automatically"></a>Автоматическое создание ресурса

### <a name="powershell"></a>PowerShell

Создание нового ресурса Application Insights

```powershell
New-AzApplicationInsights [-ResourceGroupName] <String> [-Name] <String> [-Location] <String> [-Kind <String>]
 [-Tag <Hashtable>] [-DefaultProfile <IAzureContextContainer>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="example"></a>Пример

```powershell
New-AzApplicationInsights -Kind java -ResourceGroupName testgroup -Name test1027 -location eastus
```
#### <a name="results"></a>Результаты

```powershell
Id                 : /subscriptions/{subid}/resourceGroups/testgroup/providers/microsoft.insights/components/test1027
ResourceGroupName  : testgroup
Name               : test1027
Kind               : web
Location           : eastus
Type               : microsoft.insights/components
AppId              : 8323fb13-32aa-46af-b467-8355cf4f8f98
ApplicationType    : web
Tags               : {}
CreationDate       : 10/27/2017 4:56:40 PM
FlowType           :
HockeyAppId        :
HockeyAppToken     :
InstrumentationKey : 00000000-aaaa-bbbb-cccc-dddddddddddd
ProvisioningState  : Succeeded
RequestSource      : AzurePowerShell
SamplingPercentage :
TenantId           : {subid}
```

Полную документацию по PowerShell для этого командлета и Узнайте, как получить ключ инструментирования, см. в [документации к Azure PowerShell](https://docs.microsoft.com/powershell/module/az.applicationinsights/new-azapplicationinsights?view=azps-2.5.0).

### <a name="azure-cli-preview"></a>Azure CLI (Предварительная версия)

Чтобы получить доступ к командам предварительной версии Application Insights Azure CLI, сначала необходимо выполнить следующие действия.

```azurecli
 az extension add -n application-insights
```

Если не выполнить `az extension add` команду, появится сообщение об ошибке, в котором будет указано следующее:`az : ERROR: az monitor: 'app-insights' is not in the 'az monitor' command group. See 'az monitor --help'.`

Теперь можно выполнить следующие действия, чтобы создать ресурс Application Insights:

```azurecli
az monitor app-insights component create --app
                                         --location
                                         --resource-group
                                         [--application-type]
                                         [--kind]
                                         [--tags]
```

#### <a name="example"></a>Пример

```azurecli
az monitor app-insights component create --app demoApp --location westus2 --kind web -g demoRg --application-type web
```

#### <a name="results"></a>Результаты

```azurecli
az monitor app-insights component create --app demoApp --location eastus --kind web -g demoApp  --application-type web
{
  "appId": "87ba512c-e8c9-48d7-b6eb-118d4aee2697",
  "applicationId": "demoApp",
  "applicationType": "web",
  "creationDate": "2019-08-16T18:15:59.740014+00:00",
  "etag": "\"0300edb9-0000-0100-0000-5d56f2e00000\"",
  "flowType": "Bluefield",
  "hockeyAppId": null,
  "hockeyAppToken": null,
  "id": "/subscriptions/{subid}/resourceGroups/demoApp/providers/microsoft.insights/components/demoApp",
  "instrumentationKey": "00000000-aaaa-bbbb-cccc-dddddddddddd",
  "kind": "web",
  "location": "eastus",
  "name": "demoApp",
  "provisioningState": "Succeeded",
  "requestSource": "rest",
  "resourceGroup": "demoApp",
  "samplingPercentage": null,
  "tags": {},
  "tenantId": {tenantID},
  "type": "microsoft.insights/components"
}
```

Полный Azure CLI документации по этой команде и сведения о получении ключа инструментирования см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest#ext-application-insights-az-monitor-app-insights-component-create).

## <a name="next-steps"></a>Дальнейшие действия
* [Поиск по журналу диагностики](../../azure-monitor/app/diagnostic-search.md)
* [Просмотр метрик](../../azure-monitor/platform/metrics-charts.md)
* [Написание запросов аналитики](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md
[start]: ../../azure-monitor/app/app-insights-overview.md
