---
title: Автоматическое восстановление узлов Azure Kubernetes Service (AKS)
description: Сведения о функции автоматического восстановления узла и о том, как AKS устраняет неисправные рабочие узлы.
services: container-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 9bf9df69a0a6bfa4d9f4029278d2a146811980c8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80284846"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Автоматическое восстановление узла службы Azure Kubernetes Service (AKS)

AKS постоянно проверяет состояние работоспособности рабочих узлов и выполняет автоматическое восстановление узлов, если они становятся неработоспособными. В этой документации описывается, как служба Azure Kubernetes Service (AKS) наблюдает за рабочими узлами и исправляет неработоспособные рабочие узлы.  Документация предназначена для информирования операторов AKS о поведении функции восстановления узла. Также важно отметить, что Платформа Azure [выполняет обслуживание на виртуальных машинах, на][vm-updates] которых возникают проблемы. AKS и Azure работают вместе, чтобы минимизировать перерывы в работе служб для кластеров.

> [!Important]
> В настоящее время функция автоматического восстановления узла не поддерживается для пулов узлов Windows Server.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Как AKS проверяет наличие неработоспособных узлов

> [!Note]
> AKS выполняет действия по восстановлению узлов с учетной записью пользователя **AKS-ремедиатор**.

AKS использует правила, чтобы определить, находится ли узел в неработоспособном состоянии и требуется ли его восстановление. AKS использует следующие правила, чтобы определить, требуется ли автоматическое восстановление.

* Узел сообщает о состоянии **NotReady** при последовательных проверках в течение 10 минут.
* Узел не сообщает о состоянии в течение 10 минут.

Состояние работоспособности узлов можно проверить вручную с помощью kubectl. 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Как работает автоматическое восстановление

> [!Note]
> AKS выполняет действия по восстановлению узлов с учетной записью пользователя **AKS-ремедиатор**.

Это поведение предназначено для **масштабируемых наборов виртуальных машин**.  Автоматическое восстановление выполняет несколько шагов для восстановления поврежденного узла.  Если узел определен как неработоспособный, AKS попытается выполнить несколько действий по исправлению.  Шаги выполняются в следующем порядке:

1. Когда среда выполнения контейнера перестает отвечать на запросы в течение 10 минут, на узле перезапускаются неудачные службы среды выполнения.
2. Если узел не готов в течение 10 минут, узел перезагружается.
3. Если узел не готов в течение 30 минут, узел будет создан повторно.

> [!Note]
> Если несколько узлов неработоспособны, они восстанавливаются по одному.

## <a name="next-steps"></a>Дальнейшие шаги

Используйте [зоны доступности][availability-zones] , чтобы повысить уровень доступности для рабочих нагрузок кластера AKS.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
