---
title: Поддерживаемые браузеры веб-пакета SDK | Карты Microsoft Azure
description: В этой статье вы узнаете о поддерживаемых браузерах для веб-пакета SDK для Microsoft Azure Maps и о том, как проверить, поддерживается ли браузер.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: e81b15b974469d319384a67b08512130b7876a30
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76988793"
---
# <a name="web-sdk-supported-browsers"></a>Браузеры, поддерживаемые в веб-пакетах SDK

Веб-пакет SDK Azure Maps предоставляет вспомогательную функцию с именем [Atlas. support](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-). Эта функция определяет, имеет ли веб-браузер минимальный набор функций WebGL, необходимых для поддержки загрузки и отрисовки элемента управления картой. Ниже приведен пример использования функции.

```JavaScript
if (!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if (!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    // Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Настольные

Веб-пакет SDK Azure Maps поддерживает следующие браузеры для настольных систем:

- Microsoft ребро (Текущая и Предыдущая версии)
- Google Chrome (Текущая и Предыдущая версии)
- Mozilla Firefox (Текущая и Предыдущая версии)
- Apple Safari (Mac OS X) (Текущая и Предыдущая версии)

См. также в разделе [устаревшие браузеры](#Target-Legacy-Browsers) ниже в этой статье.

## <a name="mobile"></a>Мобильный телефон

Веб-пакет SDK Azure Maps поддерживает следующие мобильные браузеры:

- Android
  - Текущая версия Chrome на Android 6,0 и более поздних версиях
  - Chrome WebView в Android 6,0 и более поздних версиях
- iOS
  - Mobile Safari в текущей и предыдущей основной версии iOS
  - Уивебвиев и Вквебвиев в текущей и предыдущей основной версии iOS
  - Текущая версия Chrome для iOS

> [!TIP]
> При внедрении схемы в мобильное приложение с помощью элемента управления WebView можно использовать [пакет NPM веб-пакета sdk Azure Maps](https://www.npmjs.com/package/azure-maps-control) вместо ссылки на версию пакета SDK, размещенного в сети доставки содержимого Azure. Такой подход сокращает время загрузки, так как пакет SDK уже находится на устройстве пользователя и не требуется загружать его во время выполнения.

## <a name="nodejs"></a>Node.js

В Node. js также поддерживаются следующие модули веб-пакета SDK:

- Модуль служб ([Документация](how-to-use-services-module.md) | [NPM Module](https://www.npmjs.com/package/azure-maps-rest))

## <a name="target-legacy-browsers"></a><a name="Target-Legacy-Browsers"></a>Целевые браузеры прежних версий

Вы можете использовать более старые браузеры, которые не поддерживают WebGL или имеют только ограниченную поддержку. В таких случаях рекомендуется использовать службы Azure Maps вместе с элементом управления картой с открытым кодом, например [леафлет](https://leafletjs.com/). Пример:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps + Леафлет" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
См. раздел "перо <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + леафлет</a> by<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () в <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о веб-пакете SDK для Azure Maps:

> [!div class="nextstepaction"]
> [Элемент управления картой](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Модуль служб](how-to-use-services-module.md)
