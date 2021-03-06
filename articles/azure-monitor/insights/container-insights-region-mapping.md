---
title: Azure Monitor для сопоставлений областей контейнеров
description: В этой статье описываются сопоставления регионов, поддерживаемые между Azure Monitor для контейнеров, Log Analytics рабочей области и настраиваемых метрик.
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: a058f9cac987bb5c7130019f50370c6a176b09ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75403420"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Сопоставления регионов, поддерживаемые Azure Monitor для контейнеров

 При включении Azure Monitor для контейнеров поддерживаются только определенные регионы для связывания Log Analytics рабочей области и кластера AKS и сбора пользовательских метрик, отправленных на Azure Monitor.

## <a name="log-analytics-workspace-supported-mappings"></a>Поддерживаемые сопоставления рабочей области Log Analytics

Ресурсы кластера AKS или Рабочая область Log Analytics могут находиться в других регионах, а в следующей таблице приведены сопоставления.

|**Регион кластера AKS** | **Регион Log Analytics рабочей области** |
|-----------------------|------------------------------------|
|**Африка** | |
|саусафриканорс |WestEurope |
|саусафрикавест |WestEurope |
|**Австралия** | |
|AustraliaEast |AustraliaEast |
|аустралиацентрал |аустралиацентрал |
|AustraliaCentral2 |аустралиацентрал |
|AustraliaEast |AustraliaEast |
|**Азиатско-Тихоокеанский регион** | |
|EastAsia |EastAsia |
|SoutheastAsia |SoutheastAsia |
|**Бразилия** | |
|бразилсаус | SouthCentralUS |
|**Canada** ||
|CanadaCentral |CanadaCentral |
|CanadaEast |CanadaCentral |
|**Европа** | |
|францецентрал |францецентрал |
|францесаус |францецентрал |
|норсеуропе |норсеуропе |
|UKSouth |UKSouth |
|уквест |UKSouth |
|WestEurope |WestEurope |
|**Индия** | |
|CentralIndia |CentralIndia |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Япония** | |
|JapanEast |JapanEast |
|жапанвест |JapanEast |
|**Корея** | |
|KoreaCentral |KoreaCentral |
|KoreaSouth |KoreaCentral |
|**СОВМЕСТ** | |
|CentralUS |CentralUS|
|EastUS |EastUS |
|EastUS2 |EastUS2 |
|WestUS |WestUS |
|WestUS2 |WestUS2 |
|WestCentralUS<sup>1</sup>|EastUS<sup>1</sup>|
|US Gov (Вирджиния) |US Gov (Вирджиния) |

<sup>1</sup> из-за емкости ограничений регион недоступен при создании новых ресурсов. Это относится к рабочей области Log Analytics. Однако уже существующие связанные ресурсы в регионе должны продолжать работать.

## <a name="custom-metrics-supported-regions"></a>Регионы, поддерживаемые пользовательскими метриками

Сбор метрик из узлов кластеров Azure Kubernetes Services (AKS) и модулей Pod поддерживается для публикации в качестве пользовательских метрик только в следующих [регионах Azure](../platform/metrics-custom-overview.md#supported-regions).

## <a name="next-steps"></a>Дальнейшие действия

Чтобы начать мониторинг кластера AKS, ознакомьтесь со сведениями о том, [как включить Azure Monitor для контейнеров](container-insights-onboard.md) , чтобы понять требования и доступные методы для включения мониторинга.  
