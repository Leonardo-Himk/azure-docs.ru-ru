---
author: kumudD
ms.service: load-balancer
ms.topic: include
ms.date: 11/09/2018
ms.author: kumud
ms.openlocfilehash: 459c99d1b45af9c98cc1a6d0d7dd7a9a04c824ec
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185709"
---
Azure Load Balancer является балансировщиком нагрузки 4-го уровня (TCP, UDP). Балансировщик нагрузки обеспечивает высокий уровень доступности, распределяя входящий трафик между работоспособными экземплярами службы в облачных службах или виртуальных машинах, определенных в наборе балансировщика нагрузки. Azure Load Balancer может также представить данные службы на нескольких портах, нескольких IP-адресах или обоими этими способами.

Балансировщик нагрузки можно настроить для выполнения следующих задач.

* Балансировка нагрузки входящего интернет-трафика виртуальных машин. В этом сценарии балансировщика нагрузки называют [балансировщиком нагрузки для Интернета](../articles/load-balancer/load-balancer-internet-overview.md).
* Балансировка нагрузки трафика между виртуальными машинами в виртуальной сети, между виртуальными машинами в облачных службах или между локальными компьютерами и виртуальными машинами в распределенной виртуальной сети. В этом сценарии балансировщика нагрузки называют [внутренним балансировщиком нагрузки](../articles/load-balancer/load-balancer-internal-overview.md).
* Перенаправление внешнего трафика к определенному экземпляру виртуальной машины.
