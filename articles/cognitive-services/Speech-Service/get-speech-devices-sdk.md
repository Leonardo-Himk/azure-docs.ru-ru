---
title: Получение пакета SDK для устройств, подключаемых к службе "Речь"
titleSuffix: Azure Cognitive Services
description: Служба распознавания речи работает с самыми разнообразными устройствами и звуковыми источниками. Теперь ваши речевые приложения могут стать еще эффективнее благодаря соответствующему оборудованию и программному обеспечению. В этой статье вы узнаете, как получить доступ к пакету SDK для речевых устройств и приступить к разработке.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/14/2019
ms.author: erhopf
ms.openlocfilehash: 436e80ebed78043c9fdb102ca71d77fde65e5b45
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82928386"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Получение пакета SDK для речевых устройств в Cognitive Services

Пакет SDK для речевых устройств — это предварительно настроенная Библиотека, предназначенная для работы с готовыми комплектами разработки и различными конфигурациями массивов микрофона.

## <a name="choose-a-development-kit"></a>Выбор набора средств разработки

|Устройства|Спецификация|Описание:|Сценарии|
|--|--|--|--|
|[Пакет SDK для урбеттер пакета урбеттер Dev Kit](http://www.urbetter.com/products_56/278.html)![](media/speech-devices-sdk/device-urbetter.jpg)|7. массив MIC, ARM SOC, WIFI, Ethernet, HDMI, USB-камера. <br>Linux|Пакет SDK для речевых устройств, который адаптирует микрофонный массив Майкрософт и поддерживает расширенные операции ввода-вывода, такие как HDMI/Ethernet и другие периферийные устройства USB. <br> [Обратитесь в Урбеттер](http://www.urbetter.com/products_56/278.html)|Транскрипция беседы, образование, больницы, роботы, OTT Box, агент передачи голоса, диск|
|[Рубо Smart Audio Dev Kit](https://ddk.roobo.com)<br>[Setup](speech-devices-sdk-roobo-v1.md) / [Краткое](speech-devices-sdk-android-quickstart.md)руководство![по настройке Рубо Smart Audio Dev Kit](media/speech-devices-sdk/device-roobo-v1.jpg)|7. массив MIC, ARM SOC, WIFI, выход из аудиоподсистемы, IO. <br>[Android](speech-devices-sdk-android-quickstart.md)|Первый пакет SDK для речевых устройств, предназначенный для адаптации Microsoft MIC Array и Front Processing SDK, предназначен для разработки речевых сценариев и речи с высоким качеством.|Запись беседы, смарт-динамик, агент передачи голоса, носимого пользователем|
|[Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)<br>[Setup](https://docs.microsoft.com/azure/Kinect-dk/set-up-azure-kinect-dk) / [Краткое](speech-devices-sdk-windows-quickstart.md)руководство![по настройке Azure Kinect DK](media/speech-devices-sdk/device-azure-kinect-dk.jpg)|7 видеокамер и глубиной цвета массива MIC. <br>[Windows](speech-devices-sdk-windows-quickstart.md)/[Linux](speech-devices-sdk-linux-quickstart.md)|Комплект разработчика с усовершенствованными датчиками искусственного интеллекта (ии) для создания сложных моделей компьютерных концепций и речи. Он сочетает в себе высококлассный пространственный массив и фотокамеру с видеокамерой и датчиком ориентации — всего на одном маленьком устройстве с несколькими режимами, вариантами и пакетами SDK для размещения различных типов вычислений.|Транскрипция беседы, Robotics, интеллектуальное создание|
|Рубо Smart Audio Dev Kit 2<br>[Установка](speech-devices-sdk-roobo-v2.md)<br>![Рубо Smart Audio Dev Kit 2](media/speech-devices-sdk/device-roobo-v2.jpg)|7. массив MIC, ARM SOC, WIFI, Bluetooth, IO. <br>Linux|Пакет SDK для речевых устройств второго поколения, предоставляющий альтернативную ОС и дополнительные функции в экономичной справочной структуре.|Запись беседы, смарт-динамик, агент передачи голоса, носимого пользователем|


## <a name="download-the-speech-devices-sdk"></a>Загрузка пакета SDK для речевых устройств

Скачайте [пакет SDK для речевых устройств](https://aka.ms/sdsdk-download).

## <a name="next-steps"></a>Дальнейшие шаги

> [!div class="nextstepaction"]
> [Начало работы с пакетом SDK для речевых устройств](https://aka.ms/sdsdk-quickstart)
