---
title: Глоссарий терминов — Предварительная версия IoT Plug and Play | Документация Майкрософт
description: Концепции — Глоссарий общих терминов, связанных с Plug and Play предварительной версией IoT.
author: Philmea
ms.author: philmea
ms.date: 12/23/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: f0c21626c664f2d72b534ebae7f0a257620be07d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81767072"
---
# <a name="glossary-of-terms-for-iot-plug-and-play-preview"></a>Глоссарий терминов для предварительной версии Plug and Play IoT

Определения распространенных терминов, используемых в статьях IoT Plug and Play.

## <a name="azure-certified-for-iot-portal"></a>Сертификация Azure для портала IoT

Вы можете использовать веб-сайт на [портале сертификации Azure для Интернета вещей](https://aka.ms/ACFI) :

- Завершите [процесс сертификации](#device-certification) для [устройства IOT Plug and Play](#iot-plug-and-play-device).
- Поиск [моделей возможностей устройств](#device-capability-model).
- Опубликуйте модель возможностей устройства в [репозитории общедоступной модели](#public-model-repository).

## <a name="azure-cli"></a>Azure CLI

Azure CLI — это кросс-платформенный инструмент командной строки для управления ресурсами Azure. Расширение Интернета вещей Azure для Azure CLI — это средство командной строки для взаимодействия с и тестирования [устройств IoT Plug and Play](#iot-plug-and-play-device). Расширение можно использовать для:

- Подключитесь к устройству Plug and Play IoT.
- Просмотр данных [телеметрии](#telemetry) , отправляемых устройством.
- Работа со [свойствами](#properties)устройства.
- Вызов [команд](#commands)устройства.
- Управление [хранилищами моделей](#model-repository), [интерфейсами](#interface)и [моделями возможностей устройств](#device-capability-model).

## <a name="azure-iot-central"></a>Azure IoT Central

Azure IoT Central — это полностью управляемое решение "программное обеспечение как услуга", которое упрощает подключение, мониторинг и управление [устройствами IoT Plug and Play](#iot-plug-and-play-device). [Модели возможностей устройств](#device-capability-model) можно использовать для автоматической настройки IOT Centralного приложения для отслеживания устройств и управления ими.

## <a name="azure-iot-certification-service"></a>Служба сертификации Azure IoT

Служба сертификации Azure IoT выполняет набор тестов сертификации при отправке [устройства IoT Plug and Play](#iot-plug-and-play-device) для сертификации через [портал сертификации Azure для Интернета вещей](#azure-certified-for-iot-portal). Прежде чем можно будет добавить устройство в [Каталог сертифицированного для устройств IOT](#certified-for-iot-device-catalog), устройство должно быть сертифицировано.

## <a name="azure-iot-tools-extension"></a>Расширение средств Azure IoT

Средства Azure IoT — это набор расширений в [Visual Studio Code](#visual-studio-code) , которые помогают взаимодействовать с центром Интернета вещей и разрабатывать устройства IOT. При разработке Plug and Play для устройств IoT помогает:

- Создание моделей и [интерфейсов](#interface) [возможностей устройства](#device-capability-model) .
- Опубликовать в [репозиториях модели](#model-repository).
- Создание каркаса кода для реализации приложения устройства.

## <a name="azure-iot-explorer-tool"></a>Средство Azure IoT Explorer

Azure IoT Explorer — это графическое средство, которое можно использовать для взаимодействия и тестирования [устройств IoT Plug and Play](#iot-plug-and-play-device). После установки средства на локальном компьютере его можно использовать в следующих случаях:

- Просмотр устройств, подключенных к центру [Интернета вещей](#azure-iot-hub).
- Подключитесь к устройству Plug and Play IoT.
- Просмотр данных [телеметрии](#telemetry) , отправляемых устройством.
- Работа со [свойствами](#properties)устройства.
- Вызов [команд](#commands)устройства.

## <a name="azure-iot-hub"></a>Центр Интернета вещей Azure

Центр Интернета вещей — размещенная в облаке управляемая служба, которая действует в качестве центра сообщений для двусторонней связи между приложением Интернета вещей и устройствами, которыми оно управляет. [Устройства iot Plug and Play](#iot-plug-and-play-device) могут подключаться к центру Интернета вещей. Решение IoT использует центр Интернета вещей для включения следующих целей:

- Устройства для отправки данных телеметрии в облачное решение.
- Облачное решение для управления подключенными устройствами.

## <a name="azure-iot-device-sdk"></a>Пакет SDK для устройств Azure IoT

Существуют пакеты SDK для устройств для нескольких языков, которые можно использовать для создания клиентских приложений для устройств IoT Plug and Play. Одним из требований к [сертификации устройства](#device-certification) является то, что код клиента устройства использует один из пакетов SDK для устройств Azure IOT.

## <a name="certified-for-iot-device-catalog"></a>Сертифицировано для каталога устройств IoT

[Каталог "сертифицировано для устройств IOT](https://catalog.azureiotsolutions.com/) " содержит список [устройств IOT Plug and Play](#iot-plug-and-play-device) , которые прошли проверку на [сертификацию устройств](#device-certification) . [Модели возможностей устройства](#device-capability-model) для устройств IOT Plug and Play в каталоге и опубликованы в общедоступном репозитории модели.

## <a name="commands"></a>Команды

Команды, определенные в [интерфейсе](#interface) , представляют методы, которые можно выполнять в [цифровом двойника](#digital-twin). Например, команда для перезагрузки устройства.

## <a name="common-interface"></a>Общий интерфейс

Все [устройства IoT Plug and Play](#iot-plug-and-play-device) должны реализовывать некоторые общие [интерфейсы](#interface). Например, интерфейс сведений об устройстве определяет оборудование и сведения об операционной системе для устройства. Для [сертификации устройств](#device-certification) требуется, чтобы ваше устройство реализовало несколько распространенных интерфейсов. Общие определения интерфейса можно получить из репозитория общедоступной модели.

## <a name="company-model-repository"></a>Репозиторий модели компании

Организация может использовать [репозиторий модели](#model-repository) компании в качестве частного хранилища для [моделей возможностей устройств](#device-capability-model) и [интерфейсов](#interface).

## <a name="connection-string"></a>Строка подключения.

Строка подключения содержит сведения, необходимые для подключения к конечной точке. Строка подключения обычно содержит адрес конечной точки и сведения о безопасности, но ее формат отличается в зависимости от службы. Существует два типа строки подключения, связанной со службой Центра Интернета вещей:

- Строки подключения устройств позволяют [устройствам IoT Plug and Play](#iot-plug-and-play-device) подключаться к конечным точкам, подключенным к устройству, в центре Интернета вещей. Клиентский код на устройстве использует строку подключения для установления безопасного соединения с центром Интернета вещей.
- Строки подключения для центра Интернета вещей позволяют серверным решениям и средствам безопасно подключаться к конечным точкам, доступным для службы, в центре Интернета вещей. Эти решения и средства управляют центром Интернета вещей и подключенными к нему устройствами.
- Строки подключения к хранилищу моделей компании позволяют серверным решениям и средствам безопасно подключаться к [репозиторию модели компании](#company-model-repository). Эти решения и средства потребляют [модели возможностей устройств](#device-capability-model) и [интерфейсы](#interface) в репозитории или управляют ими.

## <a name="device-capability-model"></a>Модель возможностей устройства

Модель возможностей устройства описывает [устройство Plug and Play IOT](#iot-plug-and-play-device) и определяет набор [интерфейсов](#interface) , реализованных устройством. Модель возможностей устройства обычно соответствует физическому устройству, продукту или SKU. Для определения модели возможностей устройства используется [язык определения Digital двойника](#digital-twin-definition-language) .

## <a name="device-certification"></a>Сертификация устройства

Сертификация устройств — это процесс сертификации [устройства iot Plug and Play](#iot-plug-and-play-device) , чтобы его можно было добавить в [сертификат для каталога устройств IOT](#certified-for-iot-device-catalog) , а также [модель возможностей устройства](#device-capability-model) и [интерфейсы](#interface) , добавленные в [репозиторий общедоступной модели](#public-model-repository).

## <a name="device-developer"></a>Разработчик устройств

Разработчик устройства использует [модель возможностей устройства](#device-capability-model), [интерфейсы](#interface)и [пакет SDK для устройств Azure IOT](#azure-iot-device-sdk) , чтобы реализовать код для запуска на [устройстве IOT Plug and Play](#iot-plug-and-play-device).

## <a name="device-modeling"></a>Моделирование устройств

[Разработчик устройства](#device-developer) использует [язык определения Digital двойника](#digital-twin-definition-language) для моделирования возможностей [устройства IOT Plug and Play](#iot-plug-and-play-device). Модель может использоваться совместно с помощью репозитория модели. Разработчик устройства может создать каркас кода устройства из модели. [Разработчик решения](#solution-developer) может настроить решение IOT на основе модели.

## <a name="device-provisioning-service"></a>Служба подготовки устройств

[Azure IOT Central](#azure-iot-central) использует службу подготовки устройств для управления регистрацией и подключением устройств. Дополнительные сведения см. [в статье подключение устройств в Azure IOT Central](../iot-central/core/concepts-get-connected.md). Службу подготовки устройств также можно использовать для управления регистрацией устройств и подключения к решению IoT на основе центра Интернета вещей. Дополнительные сведения см. в статье [Подготовка устройств с помощью службы подготовки устройств для центра Интернета вещей Azure](../iot-dps/about-iot-dps.md).

## <a name="device-registration"></a>Регистрация устройства

Прежде чем [устройство iot Plug and Play](#iot-plug-and-play-device) сможет подключиться к решению IOT, оно должно быть зарегистрировано в решении. [Azure IOT Central](#azure-iot-central) использует [службу подготовки устройств](#device-provisioning-service) для управления регистрацией устройств. В пользовательском решении IoT можно регистрировать устройства в центре Интернета вещей портал Azure или программными средствами.

## <a name="device-first"></a>Устройство — сначала

[Azure IOT Central](#azure-iot-central) поддерживает сценарий регистрации и подключения для устройств в первую очередь. В этом сценарии [устройство IoT Plug and Play](#iot-plug-and-play-device) может подключаться к приложению IOT Central без регистрации заранее. Регистрация происходит автоматически при первом подключении устройства к приложению.

## <a name="digital-twin"></a>Цифровой двойника

Цифровой двойника — это модель [устройства IoT Plug and Play](#iot-plug-and-play-device). Цифровой двойника моделируется с помощью [языка определения цифровых двойника](#digital-twin-definition-language). Для взаимодействия с цифровым двойников во время выполнения можно использовать [пакеты SDK для устройств Azure IOT](#azure-iot-device-sdk) . Например, можно задать значение свойства в цифровом двойника на устройстве, а пакет SDK передает это изменение в ваше решение IoT в облаке.

## <a name="digital-twin-change-events"></a>События изменения цифровых двойника

Когда [устройство iot Plug and Play](#iot-plug-and-play-device) подключено к центру [Интернета вещей](#azure-iot-hub), центр может использовать возможности маршрутизации для отправки уведомлений об изменениях цифровых двойника. Например, при изменении значения [Свойства](#properties) на устройстве центр Интернета вещей может отправить уведомление в конечную точку, например в очередь служебной шины.

## <a name="digital-twin-definition-language"></a>Язык определения цифровых двойника

Язык для описания моделей и интерфейсов для [устройств IoT Plug and Play](#iot-plug-and-play-device). Используйте [язык определения Digital двойника](https://aka.ms/DTDL) , чтобы описать возможности [цифрового двойника](#digital-twin) и позволить платформе IoT и решениям IOT использовать семантику сущности.

## <a name="digital-twin-route"></a>Цифровой двойника маршрут

Маршрут, настроенный в [центре Интернета вещей](#azure-iot-hub) для доставки [событий изменения Digital двойника](#digital-twin-change-events) в конечную точку, например очередь служебной шины.

## <a name="interface"></a>Интерфейс

Интерфейс описывает связанные возможности, реализованные с помощью [устройства IoT Plug and Play](#iot-plug-and-play-device) или [Digital двойника](#digital-twin). Вы можете повторно использовать интерфейсы в различных [моделях возможностей устройств](#device-capability-model).

## <a name="iot-hub-query-language"></a>Язык запросов Центра Интернета вещей

Язык запросов центра Интернета вещей используется в нескольких целях. Например, можно использовать язык для поиска [устройств, зарегистрированных](#device-registration) в центре Интернета вещей, или уточнить поведение [маршрутизации Digital двойника](#digital-twin-route) .

## <a name="iot-plug-and-play-device"></a>Устройство IoT Plug and Play

Устройство IoT Plug and Play обычно является небольшим, автономным вычислительным устройством, собирающим данные или элементами управления для других устройств, которое запускает программное обеспечение или встроенное по, которое реализует [модель возможностей устройства](#device-capability-model).  Например, устройство IoT Plug and Play может быть устройством мониторинга среды или контроллером для системы Smart-сельское хозяйство охлаждения. Вы можете написать облачное решение IoT для команд, управления и получения данных с устройств IoT Plug and Play. Списки [каталога устройств, сертифицированные в Azure для IOT](#certified-for-iot-device-catalog) , доступны для Plug and Play устройств IOT. Каждое устройство Plug and Play IoT в каталоге проверено и имеет [модель возможностей устройства](#device-capability-model).

## <a name="microsoft-partner-center"></a>Центр партнеров Майкрософт

[Центр партнеров Майкрософт](https://docs.microsoft.com/partner-center/) — это место, где ваша организация управляет комплексной связью с корпорацией Майкрософт. Вам потребуется учетная запись центра партнеров Майкрософт, прежде чем вы сможете сертифицировать [устройство Plug and Play IOT](#iot-plug-and-play-device) на [портале сертификации Azure для IOT](#azure-certified-for-iot-portal).

## <a name="model-discovery"></a>Обнаружение модели

Когда [устройство iot Plug and Play](#iot-plug-and-play-device) подключается к решению IOT, решение может обнаружить возможности устройства, находя [модель возможностей устройства](#device-capability-model). Устройство может отправить свою модель возможностей в решение, или решение может найти модель возможностей устройства в [репозитории модели](#model-repository).

## <a name="model-repository"></a>Репозиторий модели

В репозитории модели хранятся модели и [интерфейсы](#interface) [возможностей устройств](#device-capability-model) . Существует один [общедоступный репозиторий модели](#public-model-repository). Организации могут создавать собственные репозитории организационной модели.

## <a name="model-repository-rest-api"></a>REST API репозитория модели

API для управления и взаимодействия с репозиториями модели. Например, можно использовать API для добавления [моделей возможностей устройства](#device-capability-model) и поиска моделей возможностей.

## <a name="properties"></a>Элемент Property

Свойства — это поля данных, определенные в [интерфейсе](#interface) , представляющем некоторое состояние цифрового двойника. Можно объявить свойства только для чтения или для записи. Свойства, доступные только для чтения, такие как серийный номер, задаются кодом, выполняемым на самом [устройстве IoT Plug and Play](#iot-plug-and-play-device) .  Свойства, доступные для записи, такие как порог оповещений, обычно устанавливаются из облачного решения IoT.

## <a name="public-model-repository"></a>Репозиторий общедоступной модели

Существует один общедоступный репозиторий модели, в котором хранятся [модели возможностей устройств](#device-capability-model) и [интерфейсы](#interface) для [сертифицированных устройств](#device-certification). В репозитории общедоступной модели также хранятся [Общие определения интерфейса](#common-interface) .

## <a name="registration-id"></a>Идентификатор регистрации

ИДЕНТИФИКАТОР регистрации уникальным образом идентифицирует устройство в [службе подготовки устройств](#device-provisioning-service). Этот идентификатор не совпадает с ИДЕНТИФИКАТОРом устройства, который является уникальным идентификатором устройства в [центре Интернета вещей](#azure-iot-hub).

## <a name="scope-id"></a>Идентификатор области

Область идентификатора области уникально идентифицирует экземпляр [службы подготовки устройств](#device-provisioning-service) .

## <a name="shared-access-signature"></a>Подписанный URL-адрес

Подписанные URL-адреса представляют собой механизм аутентификации на базе алгоритма безопасного хэширования SHA-256 или универсального кода ресурса. Проверка подлинности подписанного URL-доступа включает два компонента: политику общего доступа и подпись общего доступа (часто называется маркером). [Устройство iot Plug and Play](#iot-plug-and-play-device) использует подпись общего доступа для проверки подлинности в [центре Интернета вещей](#azure-iot-hub).

## <a name="solution-developer"></a>Разработчик решения

Разработчик решения создает серверную части решения. Разработчик решения обычно работает с ресурсами Azure, такими как [центр Интернета вещей](#azure-iot-hub) и [хранилища моделей](#model-repository), или работает с [IOT Central](#azure-iot-central).

## <a name="telemetry"></a>Телеметрия

Поля телеметрии, определенные в [интерфейсе](#interface) , представляют измерения. Эти измерения обычно являются такими значениями, как считывание датчиков, которые отправляются [устройством IoT Plug and Play](#iot-plug-and-play-device) в виде потока данных.

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code — это современный редактор кода, доступный для нескольких платформ. Расширения, например, в пакете [средств Azure IOT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) , позволяют настроить редактор для поддержки широкого спектра сценариев разработки.
