---
title: Начало работы с двойниками устройств Центра Интернета вещей (Node) | Документация Майкрософт
description: Добавление тегов и последующее использование запроса Центра Интернета вещей с помощью двойников устройств Центра Интернета вещей. Используйте пакеты SDK для Центра Интернета вещей Azure для Node.js, чтобы реализовать приложение имитации устройства и приложение службы, которое добавит теги, и выполнить запрос Центра Интернета вещей.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: elioda
ms.custom: mqtt
ms.openlocfilehash: e65c781bd5cb62bdaa693b854caafd5f91fd497e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732278"
---
# <a name="get-started-with-device-twins-nodejs"></a>Начало работы с двойниковами устройств (Node. js)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

По завершении работы с этим руководством у вас будет два консольных приложения Node.js:

* **AddTagsAndQuery.js** — это внутреннее приложение Node.js, которое добавляет теги и выполняет запросы к двойникам устройств.

* **TwinSimulatedDevice. js**— это приложение Node. js, которое имитирует устройство, которое подключается к центру Интернета вещей с удостоверением устройства, созданным ранее, и сообщает о его состоянии подключения.

> [!NOTE]
> Статья [IoT Hub SDKs](iot-hub-devguide-sdks.md) (Пакеты SDK для Центра Интернета вещей) содержит сведения о разных пакетах SDK для Azure IoT, с помощью которых можно создать приложения для устройств и внутренние приложения.
>

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим учебником необходимы указанные ниже компоненты.

* Node. js версии 10.0. x или более поздней.

* Активная учетная запись Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/) всего за несколько минут.

* Убедитесь, что в брандмауэре открыт порт 8883. В примере для устройства в этой статье используется протокол MQTT, который обменивается данными через порт 8883. В некоторых корпоративных и академических сетях этот порт может быть заблокирован. Дополнительные сведения и способы устранения этой проблемы см. в разделе о [подключении к Центру Интернета вещей по протоколу MQTT](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Регистрация нового устройства в центре Интернета вещей

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="get-the-iot-hub-connection-string"></a>Получение строки подключения для центра Интернета вещей

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Создание приложения службы

В этом разделе вы создадите консольное приложение Node.js, которое добавляет метаданные расположения в двойник устройства, связанный с **myDeviceId**. После этого оно запрашивает данные двойников устройств, хранящихся в Центре Интернета вещей, выбирает устройства, находящиеся в США, а затем те, которые сообщили о подключении по сети мобильной связи.

1. Создайте пустую папку с именем **addtagsandqueryapp**. В папке **addtagsandqueryapp** создайте файл package.json, используя следующую команду в командной строке. `--yes` Параметр принимает все значения по умолчанию.

    ```cmd/sh
    npm init --yes
    ```

2. В командной строке в папке **addtagsandqueryapp** выполните следующую команду, чтобы установить пакет **azure-iothub**.

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. В текстовом редакторе создайте файл **AddTagsAndQuery.js** в папке **addtagsandqueryapp**.

4. Добавьте следующий код в файл **AddTagsAndQuery. js** . Замените `{iot hub connection string}` строкой подключения центра Интернета вещей, скопированной в поле [Получение строки подключения для центра Интернета вещей](#get-the-iot-hub-connection-string).

   ``` javascript
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };

                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   ```

    Объект **Registry** позволяет получить все методы, необходимые для взаимодействия с двойниками устройства из службы. Предыдущий код сначала инициализирует объект **Registry**, затем извлекает двойник устройства для **myDeviceId** и, наконец, обновляет его теги, используя сведения о требуемом расположении.

    После обновления тегов он вызывает функцию **queryTwins**.

5. Добавьте следующий код в конце файла **AddTagsAndQuery.js** для реализации функции **queryTwins**:

   ```javascript
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });

            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   ```

    Предыдущий код выполняет два запроса: первый выбирает только двойников устройства, расположенных на фабрике **Redmond43**, а второй уточняет условия первого запроса и выбирает устройства, подключенные по сети мобильной связи.

    Когда код создает объект **запроса** , он указывает максимальное число возвращаемых документов во втором параметре. Объект **query** содержит логическое свойство **hasMoreResults**, которое можно использовать для вызова методов **nextAsTwin** несколько раз, чтобы получить все результаты. Метод с именем **Next** доступен для результатов, которые не являются двойниковами устройств, например результатами запросов агрегирования.

6. Запустите приложение, выполнив следующую команду:

    ```cmd/sh
        node AddTagsAndQuery.js
    ```

   В результатах запроса на все устройства, расположенные на фабрике **Redmond43**, отобразится одно устройство, а для запроса на ограничение результатов устройствами, использующими сеть мобильной связи, не отобразится ни одного устройства.

   ![Просмотр одного устройства в результатах запроса](media/iot-hub-node-node-twin-getstarted/service1.png)

В следующем разделе рассказывается о том, как создать приложение устройства, которое сообщает сведения о подключении и изменяет результат запроса, описанного в предыдущем разделе.

## <a name="create-the-device-app"></a>Создание приложения устройства

В этом разделе вы создадите консольное приложение Node.js, которое подключается к центру как **myDeviceId** и обновляет сообщаемые свойства двойника устройства, добавив в них сведения о подключении по сети мобильной связи.

1. Создайте пустую папку с именем **reportconnectivity**. В папке **reportconnectivity** создайте файл package.json, используя следующую команду в командной строке. `--yes` Параметр принимает все значения по умолчанию.

    ```cmd/sh
    npm init --yes
    ```

2. В командной строке в папке **reportconnectivity** выполните следующую команду, чтобы установить пакеты **Azure-IOT-Device**и **Azure-IOT-Device-mqtt** :

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. В текстовом редакторе создайте файл **ReportConnectivity.js** в папке **reportconnectivity**.

4. Добавьте следующий код в файл **ReportConnectivity. js** . Замените `{device connection string}` строкой подключения устройства, скопированной при создании удостоверения устройства **myDeviceId** , в окне [Регистрация нового устройства в центре Интернета вещей](#register-a-new-device-in-the-iot-hub).

    ```javascript
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
    ```

    Объект **Client** позволяет получить все методы, необходимые для взаимодействия с двойниками устройства с устройства. После инициализации объекта **Client** предыдущий код извлекает двойник устройства для **myDeviceId** и обновляет его сообщаемое свойство, используя сведения о подключении.

5. Запуск приложения устройства

    ```cmd/sh
        node ReportConnectivity.js
    ```

    Отобразится сообщение `twin state reported`.

6. Теперь, когда устройство сообщило сведения о подключении, оно должно появиться в обоих запросах. Вернитесь к папке **addtagsandqueryapp** и выполните запросы снова:

    ```cmd/sh
        node AddTagsAndQuery.js
    ```

    На этот раз **myDeviceId** должен появиться в результатах обоих запросов.

    ![Показывать myDeviceId в обоих результатах запроса](media/iot-hub-node-node-twin-getstarted/service2.png)

## <a name="next-steps"></a>Дальнейшие шаги

В этом руководстве мы настроили новый Центр Интернета вещей на портале Azure и создали удостоверение устройства в реестре удостоверений Центра Интернета вещей. Вы добавили метаданные устройства в качестве тегов из внутреннего приложения и написали код приложения имитации устройства, чтобы сообщить сведения о подключении в двойнике устройства. Вы также узнали, как запрашивать эти сведения, используя похожий на SQL язык запросов Центра Интернета вещей.

Ознакомьтесь со следующими материалами, чтобы узнать как:

* Отправка данных телеметрии с устройств с помощью учебника [Приступая к работе с центром Интернета вещей](quickstart-send-telemetry-node.md)

* Настройка устройств с использованием требуемых свойств двойникаа устройства с помощью учебника [Использование требуемых свойств для настройки устройств](tutorial-device-twins.md) .

* Управление устройствами в интерактивном режиме (например, включение вентилятора из управляемого пользователем приложения) с помощью учебника [Использование прямых методов](quickstart-control-device-node.md) .
