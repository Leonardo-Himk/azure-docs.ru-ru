---
title: Балансировка метрик подкластеров
description: Воздействие ограничений на размещение при балансировке и способе ее решения
author: nipavlo
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: nipavlo
ms.openlocfilehash: 7f571a851e4da147240c524b742bcd652bc54181
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82183128"
---
# <a name="balancing-of-subclustered-metrics"></a>Балансировка метрик подкластеров

## <a name="what-is-subclustering"></a>Что такое подкластеризация

Подсистема кластеризации происходит, когда службы с разными ограничениями на размещение имеют общую метрику, и они сообщают о загрузке. Если нагрузка, сообщаемая службами, значительно различается, Общая нагрузка на узлах будет иметь большое стандартное отклонение, и оно будет выглядеть как несбалансированный кластер, даже если у него достигнут лучший баланс.

## <a name="how-subclustering-affects-load-balancing"></a>Влияние подкластеризации на балансировку нагрузки

Если нагрузка, сообщаемая службами на разных узлах, существенно различается, может показаться, что существует большой дисбаланс, где нет. Кроме того, если ложное дисбаланс, вызванное подкластером, больше фактического дисбаланса, он может запутать алгоритм балансировки диспетчер ресурсов и создать неоптимальный баланс в кластере.

Например, предположим, что у нас есть четыре службы, и все они сообщают о нагрузке для Метрика1 метрики:

* Служба A — имеет ограничение на размещение "NodeType = = интерфейсный", сообщает о загрузке 10
* Служба б имеет ограничение на размещение "NodeType = = интерфейсный", сообщает о загрузке 10
* Служба C — имеет ограничение на размещение "NodeType = = Серверная часть", сообщает о загрузке 100
* Служба D — имеет ограничение на размещение "NodeType = = Серверная часть", сообщает о загрузке 100
* И у нас есть четыре узла. Для двух из них задан параметр NodeType "интерфейсная часть", а другие два — "Серверная".

И у нас есть следующее размещение:

<center>

![Пример размещения в подкластере][Image1]
</center>

Кластер может выглядеть несбалансированным, у нас есть большая нагрузка на узлы 3 и 4, но в этом случае в этой ситуации будет создано наилучшее возможное сальдо.

Диспетчер ресурсов могут распознать ситуации с подкластеризацией и почти во всех случаях могут получить оптимальный баланс для данной ситуации.

В некоторых исключительных ситуациях, когда диспетчер ресурсов не может оптимально сбалансировать подкластерную метрику, он по-прежнему будет определять подкластеры и создаст отчет о работоспособности, чтобы порекомендовать устранить проблему.

## <a name="types-of-subclustering-and-how-they-are-handled"></a>Типы подкластеризации и их обработка

Ситуации подкластеризации можно классифицировать по трем различным категориям. Категория конкретной ситуации с подкластеризацией определяет, как она будет обрабатываться диспетчер ресурсов.

### <a name="first-category--flat-subclustering-with-disjoint-node-groups"></a>Первая категория — плоская подкластеризация с несвязанными группами узлов

Эта категория содержит простейшую форму создания подкластеров, где узлы можно разделять на разные группы, и каждая служба может быть размещена только на узлах одной из этих групп. Каждый узел принадлежит к одной группе и только к одной группе. Описанная выше ситуация относится к этой категории как к большинству ситуаций с подкластеризацией. 

Для ситуаций в этой категории диспетчер ресурсов может получить оптимальный баланс, и дальнейшие действия не требуются.

### <a name="second-category--subclustering-with-hierarchical-node-groups"></a>Вторая категория — подкластеризация с иерархическими группами узлов

Такая ситуация возникает, когда группа узлов, разрешенная для одной службы, является подмножеством групп узлов, разрешенных для другой службы. Наиболее распространенный пример такой ситуации — когда для некоторой службы определено ограничение на размещение, а другая служба не имеет ограничения на размещение и может быть размещена на любом узле.

Пример.

* Служба а: нет ограничения на размещение
* Служба б. ограничение размещения "NodeType = = интерфейсный"
* Служба C: ограничение размещения "NodeType = = Серверная часть"

Эта конфигурация создает отношение подмножества-надмножество между группами узлов для различных служб.

<center>

![Подмножество подмножеств подмножества][Image2]
</center>

В этом случае существует вероятность того, что будет сделана Неоптимальная балансировка.

Диспетчер ресурсов распознает эту ситуацию и создаст отчет о работоспособности, предложим разделить службу а на две службы — службу a1, которую можно разместить на интерфейсных узлах и службе a2, которые можно разместить на внутренних узлах. В результате мы вернемся к ситуации с первой категорией, которая может быть оптимальной балансировкой.

### <a name="third-category--subclustering-with-partial-overlap-between-node-sets"></a>Третья категория — подкластеризация с частичным перекрытием между наборами узлов

Такая ситуация возникает при частичном наложении между наборами узлов, на которых можно разместить некоторые службы.

Например, если у нас есть свойство Node с именем NodeColor и у нас есть три узла:

* Узел 1: NodeColor = Red
* Узел 2: NodeColor = Blue
* Узел 2: NodeColor = зеленый

И у нас есть две службы:

* Служба A: с ограничением на размещение "Color = = Red | | Color = = синий»
* Служба б. с ограничением на размещение "Color = = голубой | | Color = = зеленый»

По этой причине служба A может быть размещена на узлах 1 и 2, а служба б может быть размещена на узлах 2 и 3.

В этом случае существует вероятность того, что будет сделана Неоптимальная балансировка.

Диспетчер ресурсов распознает эту ситуацию и создаст отчет о работоспособности, предложим вам разделить некоторые службы.

В этой ситуации диспетчер ресурсов не может предоставить предложение по разделению служб, так как можно выполнить несколько разбиений и определить, какой способ будет оптимальным для разделения служб.

## <a name="configuring-subclustering"></a>Настройка подкластеризации

Поведение диспетчер ресурсов о подкластере можно изменить, изменив следующие параметры конфигурации:
* Субклустеринженаблед — параметр определяет, будет ли диспетчер ресурсов при выполнении балансировки нагрузки подкластеризация учитывается. Если этот параметр выключен, диспетчер ресурсов будет игнорировать подкластеры и попытаться достичь оптимального баланса на глобальном уровне. Значение этого параметра по умолчанию равно false.
* Субклустерингрепортингполици — определяет, как диспетчер ресурсов будет выдавать отчеты о работоспособности для иерархической кластеризации и подсистемы частичного перекрытия. Нулевое значение означает, что отчеты о работоспособности для подкластеризации отключены, "1" означает, что отчеты о работоспособности будут создаваться для неоптимальных ситуаций подкластеризации, а значение "2" приведет к созданию отчетов о работоспособности "ОК". Значение по умолчанию для этого параметра — 1.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="SubclusteringEnabled" Value="true" />
            <Parameter Name="SubclusteringReportingPolicy" Value="1" />
        </Section>
```

Для автономных развертываний используется ClusterConfig.json, а для размещенных в Azure кластеров — Template.json.

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "SubclusteringEnabled",
          "value": "true"
      },
      {
          "name": "SubclusteringReportingPolicy",
          "value": "1"
      },
    ]
  }
]
```

## <a name="next-steps"></a>Дальнейшие действия
* Чтобы узнать, как диспетчер кластерных ресурсов управляет нагрузкой кластера и балансирует ее, ознакомьтесь со статьей о [балансировке нагрузки](service-fabric-cluster-resource-manager-balancing.md)
* Чтобы узнать, как можно ограничить доступ к службам только на определенных узлах, см. раздел [Свойства узла и ограничения на размещение](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints) .

[Image1]:./media/cluster-resource-manager-subclustering/subclustered-placement.png
[Image2]:./media/cluster-resource-manager-subclustering/subset-superset-nodes.png
