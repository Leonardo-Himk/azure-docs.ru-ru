---
title: Что такое Azure Dev Spaces?
services: azure-dev-spaces
ms.date: 05/07/2019
ms.topic: overview
description: Сведения о предоставлении в Azure Dev Spaces интерфейса быстрой последовательной разработки Kubernetes для команд, которые работают с кластерами Службы Azure Kubernetes.
keywords: Docker, Kubernetes, Azure, AKS, Служба Azure Kubernetes, контейнеры, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 8b22181bcddda9e4156c0e0dbe61d7d813498d96
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80529728"
---
# <a name="what-is-azure-dev-spaces"></a>Что такое Azure Dev Spaces?

Azure Dev Spaces предоставляет интерфейс быстрой последовательной разработки Kubernetes для команд, работающих с кластерами Служба Azure Kubernetes (AKS). Azure Dev Spaces позволяет отлаживать и тестировать все компоненты приложения в AKS с минимальной настройкой компьютера для разработки без репликации или макетирования зависимостей.

![](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Как служба Azure Dev Spaces упрощает разработку Kubernetes

Azure Dev Spaces помогает командам сосредоточиться на разработке и быстрой последовательной доработке приложения для микрослужб, позволяя им работать непосредственно со всей архитектурой микрослужб или приложением в AKS. Azure Dev Spaces также дает возможность независимо обновлять части архитектуры микрослужб в режиме изоляции, не влияя на остальную часть кластера AKS или работу других разработчиков. Служба Azure Dev Spaces Azure предназначена для разработки и тестирования в средах для разработки и тестирования низкого уровня, а не для кластеров AKS в рабочей среде.

Так как команды могут работать с всем приложением и совместно работать непосредственно в AKS, служба Azure Dev Spaces:

* сводит к минимуму настройку локального компьютера;
* сокращает время настройки для новых разработчиков в команде;
* увеличивает скорость работы команды благодаря ускоренной итерации;
* уменьшает число избыточных сред разработки и интеграции, так как члены команды могут совместно использовать кластер;
* избавляет от необходимости реплицировать или макетировать зависимости;
* улучшает совместную работу команд разработчиков, а также команд, с которыми они сотрудничают, например команд DevOps.

Azure Dev Spaces предоставляет набор инструментов для создания ресурсов Docker и Kubernetes для ваших проектов. Этот набор инструментов позволяет легко добавлять новые и существующие приложения как в пространство разработки, так и в другие кластеры AKS.

Дополнительные сведения о том, как работает Azure Dev Spaces, см. в разделе [Принципы работы и настройки Azure Dev Spaces][how-dev-spaces-works].

## <a name="supported-regions-and-configurations"></a>Поддерживаемые регионы и конфигурации

Azure Dev Spaces поддерживается только кластерами AKS в [некоторых регионах][supported-regions]. Azure Dev Spaces поддерживает [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) и [Visual Studio Code](https://code.visualstudio.com/download) с [расширением Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) в Linux, macOS и Windows 8 или более поздней версии для создания и запуска приложений в AKS. Эта служба также поддерживает [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) в Windows 8 или более поздней версии. Чтобы использовать Visual Studio 2019, потребуется рабочая нагрузка для разработки в Azure. Чтобы использовать Visual Studio 2017, потребуется рабочая нагрузка для разработки веб-приложений и [Средства Visual Studio для Kubernetes](https://aka.ms/get-vsk8stools).

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о быстрой последовательной разработке для команд с помощью Azure Dev Spaces, ознакомившись с [кратким руководством по командной разработке][team-development-quickstart].

[how-dev-spaces-works]: how-dev-spaces-works.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-development-quickstart]: quickstart-team-development.md
