---
title: Краткое руководство. Создание веб-приложения, которое запускает Иммерсивное средство чтения, используя Node.js
titleSuffix: Azure Cognitive Services
description: В рамках этого краткого руководства вы создадите веб-приложения с нуля и добавите функции API "Иммерсивное средство чтения".
author: pasta
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 01/14/2020
ms.author: pasta
ms.openlocfilehash: 749e75fed409632c613713a49154e4cd8dc265b3
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75945970"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-nodejs"></a>Краткое руководство. Создание веб-приложения, которое запускает Иммерсивное средство чтения (Node.js)

[Иммерсивное средство чтения](https://www.onenote.com/learningtools) — это включительно разработанное решение, в котором реализованы проверенные методы, улучшающие понимание при чтении.

В рамках этого краткого руководства вы создадите веб-приложение с нуля и интегрируете иммерсивное средство чтения с помощью пакета SDK иммерсивного средства чтения. Полностью рабочий пример этого краткого руководства доступен [здесь](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-nodejs).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

* Ресурс "Иммерсивное средство чтения", настроенный для проверки подлинности Azure Active Directory. Инструкции по настройке см. [здесь](./how-to-create-immersive-reader.md). Вам потребуются некоторые значения, созданные здесь при настройке свойств среды. Сохраните результаты своего сеанса в текстовом файле для использования в будущем.
* [Node.js](https://nodejs.org/) и [Yarn](https://yarnpkg.com).
* Интегрированная среда разработки, такая как [Visual Studio Code](https://code.visualstudio.com/).

## <a name="create-a-nodejs-web-app-with-express"></a>Создание веб-приложения Node.js с помощью средства Express

Создайте веб-приложение Node.js с помощью средства `express-generator`.

```bash
npm install express-generator -g
express --view=pug quickstart-nodejs
cd quickstart-nodejs
```

Установите зависимости yarn и добавьте зависимости `request` и `dotenv`, которые будут использоваться в этом кратком руководстве позже.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="set-up-authentication"></a>Настройка проверки подлинности

### <a name="configure-authentication-values"></a>Настройка значений проверки подлинности

Создайте новый файл с именем _.env_ в корневом каталоге проекта. Вставьте в него следующий код, указав значения, заданные при создании ресурса "Иммерсивное средство чтения".
Не включайте кавычки или символы "{" и "}".

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN={YOUR_SUBDOMAIN}
```

Не забудьте зафиксировать этот файл в системе управления версиями, так как он содержит секреты, для которых не следует предоставлять открытый доступ.

Затем откройте файл _app.js_ и добавьте следующее в его начало. В Node загрузятся свойства, указанные в файле .env в качестве переменных среды.

```javascript
require('dotenv').config();
```

### <a name="update-the-router-to-acquire-the-token"></a>Обновление маршрутизатора для получения токена
Откройте файл _routes\index.js_ и замените автоматически созданный код приведенным ниже кодом.

Этот код создает конечную точку API, которая получает токен проверки подлинности Azure AD, используя пароль субъект-службы. Кроме того, этот код возвращает дочерний домен, а также объект, содержащий токен и поддомен.

```javascript
var express = require('express');
var router = express.Router();
var request = require('request');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/GetTokenAndSubdomain', function(req, res) {
    try {
        request.post({
            headers: {
                'content-type': 'application/x-www-form-urlencoded'
            },
            url: `https://login.windows.net/${process.env.TENANT_ID}/oauth2/token`,
            form: {
                grant_type: 'client_credentials',
                client_id: process.env.CLIENT_ID,
                client_secret: process.env.CLIENT_SECRET,
                resource: 'https://cognitiveservices.azure.com/'
            }
        },
        function(err, resp, tokenResult) {
            if (err) {
                console.log(err);
                return res.status(500).send('CogSvcs IssueToken error');
            }

            var tokenResultParsed = JSON.parse(tokenResult);

            if (tokenResultParsed.error) {
                console.log(tokenResult);
                return res.send({error :  "Unable to acquire Azure AD token. Check the debugger for more information."})
            }

            var token = tokenResultParsed.access_token;
            var subdomain = process.env.SUBDOMAIN;
            return res.send({token, subdomain});
        });
    } catch (err) {
        console.log(err);
        return res.status(500).send('CogSvcs IssueToken error');
    }
});

module.exports = router;
```

Конечная точка API **GetTokenAndSubdomain** должна быть защищена какой-либо формой аутентификации (например, [OAuth](https://oauth.net/2/)), чтобы предотвратить получение неавторизованными пользователями токенов для использования в службе Иммерсивного средства чтения и выставлении счетов. Эти сведения выходят за рамки этого руководства.

## <a name="add-sample-content"></a>Добавление примеров содержимого

Теперь мы добавим пример содержимого для этого веб-приложения. Откройте файл _views\index.pug_ и замените автоматически созданный код приведенным ниже примером.

```pug
doctype html
html
   head
      title Immersive Reader Quickstart Node.js

      link(rel='stylesheet', href='https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css')

      // A polyfill for Promise is needed for IE11 support.
      script(src='https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js')

      script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.1.0.0.js')
      script(src='https://code.jquery.com/jquery-3.3.1.min.js')

      style(type="text/css").
        .immersive-reader-button {
          background-color: white;
          margin-top: 5px;
          border: 1px solid black;
          float: right;
        }
   body
      div(class="container")
        button(class="immersive-reader-button" data-button-style="iconAndText" data-locale="en")

        h1(id="ir-title") About Immersive Reader
        div(id="ir-content" lang="en-us")
          p Immersive Reader is a tool that implements proven techniques to improve reading comprehension for emerging readers, language learners, and people with learning differences. The Immersive Reader is designed to make reading more accessible for everyone. The Immersive Reader

            ul
                li Shows content in a minimal reading view
                li Displays pictures of commonly used words
                li Highlights nouns, verbs, adjectives, and adverbs
                li Reads your content out loud to you
                li Translates your content into another language
                li Breaks down words into syllables

          h3 The Immersive Reader is available in many languages.

          p(lang="es-es") El Lector inmersivo está disponible en varios idiomas.
          p(lang="zh-cn") 沉浸式阅读器支持许多语言
          p(lang="de-de") Der plastische Reader ist in vielen Sprachen verfügbar.
          p(lang="ar-eg" dir="rtl" style="text-align:right") يتوفر \"القارئ الشامل\" في العديد من اللغات.

script(type="text/javascript").
  function getTokenAndSubdomainAsync() {
        return new Promise(function (resolve, reject) {
            $.ajax({
                url: "/GetTokenAndSubdomain",
                type: "GET",
                success: function (data) {
                    if (data.error) {
                        reject(data.error);
                    } else {
                        resolve(data);
                    }
                },
                error: function (err) {
                    reject(err);
                }
            });
        });
    }

    $(".immersive-reader-button").click(function () {
        handleLaunchImmersiveReader();
    });

    function handleLaunchImmersiveReader() {
        getTokenAndSubdomainAsync()
            .then(function (response) {
                const token = response["token"];
                const subdomain = response["subdomain"];
                // Learn more about chunk usage and supported MIME types https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#chunk
                const data = {
                    title: $("#ir-title").text(),
                    chunks: [{
                        content: $("#ir-content").html(),
                        mimeType: "text/html"
                    }]
                };
                // Learn more about options https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#options
                const options = {
                    "onExit": exitCallback,
                    "uiZIndex": 2000
                };
                ImmersiveReader.launchAsync(token, subdomain, data, options)
                    .catch(function (error) {
                        alert("Error in launching the Immersive Reader. Check the console.");
                        console.log(error);
                    });
            })
            .catch(function (error) {
                alert("Error in getting the Immersive Reader token and subdomain. Check the console.");
                console.log(error);
            });
    }

    function exitCallback() {
        console.log("This is the callback function. It is executed when the Immersive Reader closes.");
    }
```


Обратите внимание, что весь текст имеет атрибут **lang**, который описывает языки текста. Этот атрибут помогает Иммерсивному средству чтения предоставить соответствующие функции языка и грамматики.

## <a name="build-and-run-the-app"></a>Создание и запуск приложения

Теперь наше веб-приложение готово. Запустите приложение, выполнив следующую команду:

```bash
npm start
```

Откройте веб-браузер и перейдите по адресу _http://localhost:3000_ . Вы увидите следующее:

![Пример приложения](./media/quickstart-nodejs/1-buildapp.png)

## <a name="launch-the-immersive-reader"></a>Запуск Иммерсивного средства чтения

При нажатии кнопки "иммерсивное средство чтения" появится запущенное иммерсивное средство чтения с содержимым на странице.

![Иммерсивное средство чтения](./media/quickstart-nodejs/2-viewimmersivereader.png)

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь с разделом о [пакете SDK для иммерсивного средства чтения](https://github.com/microsoft/immersive-reader-sdk) и [справочнике по этому пакету](./reference.md).