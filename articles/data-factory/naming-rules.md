---
title: Правила именования сущностей фабрики данных Azure
description: Описывает правила именования для сущностей фабрик данных.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/16/2018
ms.openlocfilehash: f922ada663391cf65a61f4e18bba53668f9c4a1a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419414"
---
# <a name="azure-data-factory---naming-rules"></a>Фабрика данных Azure — правила именования

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

В следующей таблице приведены правила именования для артефактов фабрики данных.

| Имя | Уникальность имени | Проверки |
|:--- |:--- |:--- |
| Фабрика данных |Уникально в рамках Microsoft Azure. Регистр в именах не учитывается, т. е. `MyDF` и `mydf` ссылаются на одну и ту же фабрику данных. |<ul><li>Каждая фабрика данных привязывается ровно к одной подписке Azure.</li><li>Имена объектов должны начинаться с буквы или цифры и могут содержать только буквы, цифры и дефисы (-).</li><li>Перед каждым знаком тире (-) и после него должна стоять буква или цифра. Последовательные тире в именах контейнеров использовать нельзя.</li><li>Имя может содержать от 3 до 63 символов.</li></ul> |
| Связанные службы/наборы данных/конвейеры |Уникально в рамках фабрики данных. Регистр в именах не учитывается. |<ul><li>Имена объектов должны начинаться с буквы, цифры или символа подчеркивания (_).</li><li>Следующие символы не допускаются: ".", "+", "?", "/", "<", ">", "*", "%", "&", ":", "\\".</li><li>Дефисы ("-") запрещены только в именах связанных служб и наборов данных.</li></ul>  |
| Группа ресурсов |Уникально в рамках Microsoft Azure. Регистр в именах не учитывается. | Дополнительные сведения см. в разделе [Правила именования и ограничения](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming). |

## <a name="next-steps"></a>Дальнейшие шаги
Узнайте, как создавать фабрики данных, выполнив пошаговые инструкции из [краткого руководства](quickstart-create-data-factory-powershell.md). 
