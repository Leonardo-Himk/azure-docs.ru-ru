---
title: Обзор API ретранслятора Azure | Документация Майкрософт
description: В этой статье представлен обзор доступных Azure Relay интерфейсов API (.NET Standard, .NET Framework, Node. js и т. д.).
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: ad7226b5c5badfddf9f436a1229a48f729485821
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83210597"
---
# <a name="available-relay-apis"></a>Доступные интерфейсы API ретранслятора

## <a name="runtime-apis"></a>API среды выполнения

В таблице ниже приведен список всех доступных клиентских библиотек среды выполнения ретранслятора.

Ознакомьтесь с разделом [Дополнительная информация](#additional-information), чтобы узнать больше о состоянии каждой библиотеки среды выполнения.

| Язык или платформа | Доступная функция | Пакет клиента | Хранилище |
| --- | --- | --- | --- |
| .NET Standard | Гибридные подключения | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | Ретранслятор WCF | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Н/Д |
| Узел | Гибридные подключения | [WebSockets`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[WebSockets`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[HTTP-запросы:`hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Дополнительные сведения

#### <a name="net"></a>.NET

В экосистеме .NET существует несколько сред выполнения, поэтому для ретранслятора используется несколько библиотек .NET. Библиотеку .NET Standard можно запустить с помощью .NET Core или .NET Framework, а библиотеку .NET Framework можно запустить только в среде .NET Framework. Дополнительные сведения о платформах .NET Framework см. в разделе [версии платформы](/dotnet/articles/standard/frameworks).

Библиотека .NET Framework поддерживает только модель программирования WCF и использует собственный двоичный протокол на основе транспорта WCF `net.tcp`. Этот протокол и библиотека предоставляются для обеспечения обратной совместимости с существующими приложениями.

Библиотека .NET Standard основана на определении открытого протокола для ретранслятора гибридных подключений, который основан на протоколе HTTP и подключениях WebSocket. Библиотека поддерживает абстракцию потока через WebSockets и простой жест API типа "запрос-ответ" для ответа на HTTP-запросы. В примере [Веб-API](https://github.com/Azure/azure-relay-dotnet) показано, как интегрировать гибридные подключения с веб-службами ASP.NET Core.

#### <a name="nodejs"></a>Node.js

Модули гибридных подключений в приведенной выше таблице заменяют или изменяют существующие модули Node.js с помощью альтернативных реализаций, которые ожидают передачи данных из службы ретрансляции Azure, а не локального сетевого стека.

`hyco-https`Модуль изменяет и частично переопределяет основные модули Node. js `http` и `https` предоставляет реализацию прослушивателя HTTPS, совместимую с множеством существующих модулей Node. js и приложений, которые используют эти основные модули.

Модули `hyco-ws` и `hyco-websocket` изменяют популярные модули `ws` и `websocket` для Node.js, предоставляя альтернативные реализации прослушивателя, позволяющие модулям и приложениям использовать любой из этих модулей в зоне действия ретранслятора гибридных подключений.

Сведения об этих модулях можно найти в репозитории GitHub [azure-relay-node](https://github.com/Azure/azure-relay-node).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о ретрансляторе Azure доступны по следующим ссылкам:
* [Что такое ретранслятор Azure?](relay-what-is-it.md)
* [Вопросы и ответы по ретранслятору](relay-faq.md)
