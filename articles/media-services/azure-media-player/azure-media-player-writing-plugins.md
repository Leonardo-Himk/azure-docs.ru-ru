---
title: Создание подключаемых модулей для Проигрыватель мультимедиа Azure
description: Узнайте, как написать подключаемый модуль с помощью Проигрыватель мультимедиа Azure JavaScript
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 04/20/2020
ms.openlocfilehash: 7902dfdf81d8e44921a5218d56effc90f433f02d
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857409"
---
# <a name="writing-plugins-for-azure-media-player"></a>Создание подключаемых модулей для Проигрыватель мультимедиа Azure #

Подключаемый модуль — это JavaScript, предназначенный для расширения или улучшения проигрывателя. Вы можете написать подключаемые модули, которые изменяют внешний вид Проигрыватель мультимедиа Azure, его функциональность или даже могут взаимодействовать с другими службами. Это можно сделать двумя простыми шагами:

## <a name="step-1"></a>Шаг 1 ##

Напишите JavaScript в функцию следующим образом:

```javascript

    (function () {
        amp.plugin('yourPluginName', function (options) {
        var myPlayer = this;
           myPlayer.addEventListener(amp.eventName.ready, function () {
        console.log("player is ready!");
            });
        });
    }).call(this);
```

Код можно писать непосредственно на HTML-странице внутри `<script>` тегов или во внешнем файле JavaScript. Если вы выполняете Последнее действие, не забудьте включить файл JavaScript в `<head>` страницу HTML *после* скрипта amp.

Пример

```javascript
    <!--*****START OF Azure Media Player Scripts*****-->
    <script src="//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <!--*****END OF Azure Media Player Scripts*****-->
    <!--Add Plugins-->
    <script src="yourPluginName.js"></script>
```

## <a name="step-2"></a>Шаг 2 ##
Инициализируйте подключаемый модуль с помощью JavaScript одним из двух способов:

Метод 1.

```javascript
    var myOptions = {
        autoplay: true,
        controls: true,
        width: "640",
        height: "400",
        poster: "",
        plugins: {
            yourPluginName: {
                [your plugin options]: [example options]
           }
        }
    };     
    var myPlayer = amp([videotag id], myOptions);
```

Метод 2.

```javascript
    var video = amp([videotag id]);
    video.yourPluginName({[your plugins option]: [example option]});
```

Параметры подключаемого модуля не являются обязательными, в том числе они позволяют разработчикам, использующим подключаемый модуль, настраивать его поведение без необходимости изменения исходного кода.

Для создания и получения дополнительных примеров по созданию подключаемого модуля Ознакомьтесь с нашей [галереей](azure-media-player-plugin-gallery.md)

>[!NOTE]
> Код подключаемого модуля динамически изменяет элементы в модели DOM в течение времени существования проигрывателя средства просмотра, но никогда не вносит постоянные изменения в исходный код проигрывателя. Именно здесь удобно понимать средства разработчика, представленные в браузере. Например, если вы хотите изменить внешний вид элемента в проигрывателе, его элемент HTML можно найти по имени класса, а затем добавить или изменить атрибуты. Ниже приведен отличный ресурс по [изменению АТРИБУТОВ HTML.](http://www.w3schools.com/js/js_htmldom_html.asp)

### <a name="integrated-plugins"></a>Интегрированные подключаемые модули ###

 В настоящее время два подключаемых модуля помогут в AMP: [Подсказка времени](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/timetip/example.html) и [сочетания клавиш](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/hotkeys/example.html). Эти подключаемые модули изначально разрабатывались как модульные подключаемые модули для проигрывателя, но теперь включены в исходный код проигрывателя.

### <a name="plugin-gallery"></a>Коллекция подключаемых модулей ###

[Коллекция подключаемых](https://aka.ms/ampplugins) модулей содержит несколько подключаемых модулей, которые сообщество уже участвовало в таких функциях, как маркеры времени, масштаб, аналитика и многое другое. Эта страница предоставляет доступ к подключаемым модулям и инструкциям по их настройке, а также демонстрации, которое показывает подключаемый модуль в действии. Если вы создаете замечательный подключаемый модуль, который вы считаете в нашей коллекции, вы можете отправить его, чтобы мы могли извлечь его.

## <a name="next-steps"></a>Дальнейшие действия ##

- [Краткое руководство. Проигрыватель мультимедиа Azure](azure-media-player-quickstart.md)
