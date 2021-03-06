---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: f0761847c3677b324ef16c5987eb9a1561dbcbe0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75379303"
---
Изучите значения `"confidence"` для каждого результата "ключ — значение" в узле `"pageResults"`. Следует также обратить внимание на оценки достоверности в узле `"readResults"`, который соответствует операции чтения текста. Достоверность результатов чтения не влияет на достоверность результатов извлечения пар "ключ — значение", поэтому следует проверять оба значения.
* Если оценка достоверности для операции чтения текста низкая, попробуйте улучшить качество входных документов (см. [требования к входным данным](../overview.md#input-requirements)).
* Если оценки достоверности для операции извлечения ключей и значений низкие, убедитесь, что у анализируемых документов и документов в наборе для обучения одинаковый тип. Если внешний вид документов в обучающем наборе отличается, попробуйте рассортировать их по разным папкам и обучить разные модели соответствующим образом.

Целевые показатели достоверности будут зависеть от варианта использования, но обычно рекомендуется ориентироваться на оценку 80 % или выше. Для более конфиденциальных ситуаций, таких как чтение медицинских записей или выставление счетов, рекомендуемая оценка — 100 %.