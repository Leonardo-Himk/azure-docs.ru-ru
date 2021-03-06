---
title: Поддерживаемые языки в решении "Функции Azure"
description: Узнайте, какие языки поддерживаются (являются общедоступными), а какие используются в режиме экспериментальной или предварительной версии.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 029ea753439dca3093bf214a5adfb6d58a1fe567
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74942262"
---
# <a name="supported-languages-in-azure-functions"></a>Поддерживаемые языки в решении "Функции Azure"

В этой статье перечислены уровни поддержки языков, используемых с решением "Функции Azure".

## <a name="levels-of-support"></a>Уровни поддержки

Уровней поддержки три:

* **Общедоступные** — полностью поддерживаемые языки, утвержденные для использования в рабочей среде.
* **Предварительная версия** — языки, которые еще не поддерживаются, но в будущем станут общедоступными.
* **Экспериментальные** — языки, которые не поддерживаются и могут быть удалены в будущем. Нет гарантии, что они будут поддерживаться в качестве общедоступных.

## <a name="languages-by-runtime-version"></a>Языки по версии среды выполнения 

Доступны [три версии среды выполнения функций Azure](functions-versions.md) . В следующей таблице показаны поддерживаемые языки для каждой версии среды выполнения.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Языки, используемые в режиме экспериментальной версии

Языки, используемые как экспериментальные, в версии 1.x не масштабируются должным образом и поддерживают не все привязки.

Но не используйте экспериментальные функции для важных задач, так как эти функции не поддерживаются официально. Случаи поддержки не следует открывать для проблем, если применяются языки, используемые в качестве экспериментальных. 

Более поздние версии среды выполнения не поддерживают экспериментальные языки. Поддержка новых языков добавляется только в том случае, если язык может поддерживаться в рабочей среде. 

### <a name="language-extensibility"></a>Расширяемость языка

Начиная с версии 2. x среда выполнения разработана таким образом, чтобы обеспечить [расширяемость языка](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Языки JavaScript и Java в среде выполнения 2.x поддерживают эту расширяемость.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о разработке функций на поддерживаемых языках см. в следующих ресурсах:

+ [Справочник разработчика по библиотеке классов C#](functions-dotnet-class-library.md)
+ [Справочник разработчика по скриптам C#](functions-reference-csharp.md)
+ [Справочник разработчика Java](functions-reference-java.md)
+ [Справочник разработчика JavaScript](functions-reference-node.md)
+ [Справочник разработчика по PowerShell](functions-reference-powershell.md)
+ [Справочник разработчика Python](functions-reference-python.md)
+ [Справочник разработчика TypeScript](functions-reference-node.md#typescript)
