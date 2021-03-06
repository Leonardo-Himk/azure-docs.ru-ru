---
title: Общие сведения о выделенных узлах Azure для виртуальных машин
description: Узнайте больше о том, как можно использовать выделенные узлы Azure для развертывания виртуальных машин.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.date: 01/09/2020
ms.author: cynthn
ms.openlocfilehash: c19b3410e106aaf5fed53aba45d06eac6dd9d601
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78970725"
---
# <a name="azure-dedicated-hosts"></a>Выделенные узлы Azure

Выделенный узел Azure — это служба, которая предоставляет физические серверы — возможность размещения одной или нескольких виртуальных машин, выделенных для одной подписки Azure. Выделенные узлы — это те же физические серверы, которые используются в наших центрах обработки данных, предоставляемые в качестве ресурса. Вы можете подготавливать выделенные узлы в регионе, зоне доступности и домене сбоя. Затем можно разместить виртуальные машины непосредственно в подготовленных узлах в любой конфигурации, которая лучше всего соответствует вашим потребностям.



[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]


## <a name="next-steps"></a>Дальнейшие шаги

- Выделенный узел можно развернуть с помощью [Azure CLI](dedicated-hosts-cli.md), [портала](dedicated-hosts-portal.md)и [PowerShell](../windows/dedicated-hosts-powershell.md).

- Дополнительные сведения см. в разделе Общие сведения о [выделенных узлах](dedicated-hosts.md) .

- [Здесь](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)приведен пример шаблона, который использует зоны и домены сбоя для максимальной устойчивости в регионе.

- Вы также можете сэкономить на затратах с помощью [зарезервированного экземпляра выделенных узлов Azure](../prepay-dedicated-hosts-reserved-instances.md).
