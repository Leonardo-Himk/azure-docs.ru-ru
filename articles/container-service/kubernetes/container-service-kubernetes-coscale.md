---
title: Мониторинг кластера Kubernetes в Azure с помощью CoScale (не рекомендуется)
description: Мониторинг кластера Kubernetes в Службе контейнеров Azure с помощью CoScale
author: fryckbos
ms.service: container-service
ms.topic: conceptual
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f195a5c05c6c95dac898b2d471747952a3446d52
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681715"
---
# <a name="deprecated-monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Мониторинг кластера Kubernetes в Службе контейнеров Azure с помощью CoScale (не рекомендуется)

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

В этой статье показано, как развернуть агент [CoScale](https://web.archive.org/web/20180317071550/https://www.coscale.com/) для отслеживания всех узлов и контейнеров кластера Kubernetes в Службе контейнеров Azure. Для работы с этой конфигурацией вам понадобится учетная запись с CoScale. 


## <a name="about-coscale"></a>Сведения о CoScale 

CoScale представляет собой платформу для мониторинга, собирающую метрики и события из всех контейнеров на нескольких платформах оркестрации. CoScale обеспечивает комплексный мониторинг для сред Kubernetes. Она предоставляет средства визуализации и аналитики на всех слоях стека: ОС, Kubernetes, Docker и приложения, выполняющиеся в контейнерах. CoScale содержит несколько встроенных панелей мониторинга, а также средства обнаружения аномалий, которые позволяют операторам и разработчикам быстро обнаруживать проблемы в инфраструктуре и приложении.

![Пользовательский интерфейс CoScale](./media/container-service-kubernetes-coscale/coscale.png)

Как показано в этой статье, вы можете установить агенты в кластер Kubernetes для выполнения CoScale в качестве решения SaaS. CoScale также можно установить локально, если необходимо хранить данные на месте.


## <a name="prerequisites"></a>Предварительные условия

Сначала потребуется [создать учетную запись CoScale](https://web.archive.org/web/20170507123133/https://www.coscale.com/free-trial).

В этом пошаговом руководстве предполагается, что вы [создали кластер Kubernetes с помощью службы контейнеров Azure](container-service-kubernetes-walkthrough.md).

Кроме того, предполагается, что у вас установлен Azure CLI `az` и средства `kubectl`.

Чтобы проверить наличие средства `az`, выполните такую команду:

```azurecli
az --version
```

Если средство `az` не установлено, следуйте инструкциям, приведенным [здесь](/cli/azure/install-azure-cli).

Чтобы проверить наличие средства `kubectl`, выполните такую команду:

```bash
kubectl version
```

Если средство `kubectl` не установлено, выполните команду:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>Установка агента CoScale с помощью DaemonSet
[Daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) используются Kubernetes для запуска одного экземпляра контейнера на каждом узле в кластере.
Они идеально подходят для выполнения агентов мониторинга, таких как CoScale.

После входа в CoScale перейдите на [страницу агента](https://developer.newrelic.com/), чтобы установить в кластер агенты CoScale с помощью DaemonSet. Пользовательский интерфейс CoScale содержит пошаговые инструкции настройки по созданию агента и запуску мониторинга всего кластера Kubernetes.

![Конфигурация агента CoScale](./media/container-service-kubernetes-coscale/installation.png)

Чтобы запустить агент в кластере, выполните указанную команду.

![Запуск агента CoScale](./media/container-service-kubernetes-coscale/agent_script.png)

Вот и все! Через несколько минут после того, как агенты будут запущены и начнут работать, вы увидите данные в консоли. Посетите [страницу агента](https://developer.newrelic.com/), чтобы просмотреть сводку по кластеру, выполнить дополнительные действия по настройке и увидеть панели мониторинга, например панель **основных сведений о кластере Kubernetes**.

![Основные сведения о кластере Kubernetes](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

На новых компьютерах в кластере агент CoScale устанавливается автоматически. Он автоматически обновляется при выпуске новой версии.


## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о решениях CoScale для мониторинга см. в [документации по CoScale](https://web.archive.org/web/20180415164304/http://docs.coscale.com:80/) и в [блоге](https://web.archive.org/web/20170501021344/http://www.coscale.com:80/blog). 

