---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/11/2020
ms.author: glenga
ms.openlocfilehash: dab5f0f24fa1f36b611eb79329336832d8a4b3cb
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2020
ms.locfileid: "78191090"
---
Добавьте код, который использует объект выходной привязки `msg` в `context.bindings` для создания сообщения очереди. Добавьте этот код перед инструкцией `context.res`.

:::code language="typescript" range="10" source="~/functions-docs-typescript/functions-add-output-binding-storage-queue-cli/HttpExample/index.ts":::

На этом этапе ваша функция должна выглядеть следующим образом:

:::code language="typescript" source="~/functions-docs-typescript/functions-add-output-binding-storage-queue-cli/HttpExample/index.ts":::