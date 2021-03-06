---
title: Общие сведения о встроенной конечной точке Центра Интернета вещей Azure | Документация Майкрософт
description: В этом руководстве разработчика описано, как читать сообщения, отправляемые с устройства в облако, с помощью встроенной конечной точки, совместимой с концентратором событий.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2019
ms.custom: amqp
ms.openlocfilehash: bf7c4118e17727c6c8141570ab146026d5383059
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2020
ms.locfileid: "82996933"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Чтение сообщений, пересылаемых с устройства в облако, из встроенной конечной точки

По умолчанию сообщения направляются во встроенную конечную точку, доступную для службы (**/messages/events**), которая совместима с [Центрами событий](https://azure.microsoft.com/documentation/services/event-hubs/). Сейчас эта конечная точка предоставляется только по протоколу [AMQP](https://www.amqp.org/) на порте 5671. Центр Интернета вещей позволяет управлять встроенной конечной точкой обмена сообщениями **messages/events**, совместимой с концентраторами событий, с помощью приведенных ниже свойств.

| Свойство            | Описание |
| ------------------- | ----------- |
| **Число секций** | Это свойство задается во время создания, чтобы определить количество [разделов](../event-hubs/event-hubs-features.md#partitions) для приема событий, отправляемых с устройства в облако. |
| **Время хранения**  | Это свойство задает время в днях, в течение которого сообщения хранятся в Центре Интернета вещей. Значение по умолчанию — один день, но это значение можно увеличить до семи дней. |

Центр Интернета вещей допускает хранение данных в встроенных концентраторах событий не более 7 дней. Время хранения можно задать во время создания центра Интернета вещей. Время хранения данных в центре Интернета вещей зависит от уровня центра Интернета вещей и типа устройства. С точки зрения размера встроенные концентраторы событий могут хранить сообщения с максимальным размером не менее 24 часов квот. Например, для 1 центра Интернета вещей типа S1 предоставляет достаточно места для хранения по крайней мере 400 тыс. сообщений размером в 4 КБ. Если устройства отправляют небольшие сообщения, они могут храниться дольше (до 7 дней) в зависимости от объема используемого хранилища. Мы гарантируем сохранение данных за указанное время хранения как минимум. Срок действия сообщений истечет и будет недоступен после окончания срока хранения. 

Центр Интернета вещей также предоставляет возможность управлять группами потребителей во встроенной конечной точке, которая получает сообщения, отправляемые с устройства в облако. Для каждого центра Интернета вещей можно использовать до 20 групп потребителей.

Если вы используете [маршрутизацию сообщений](iot-hub-devguide-messages-d2c.md) и включен [резервный маршрут](iot-hub-devguide-messages-d2c.md#fallback-route) , все сообщения, которые не соответствуют запросу по маршруту, переходят во встроенную конечную точку. Если отключить этот резервный маршрут, то сообщения, не соответствующие ни одному из запросов, будут удалены.

Период хранения можно изменить на [портале Azure](https://portal.azure.com) или программно (с помощью [интерфейсов REST API поставщика ресурсов Центра Интернета вещей](/rest/api/iothub/iothubresource)).

Центр Интернета вещей предоставляет встроенную конечную точку **messages/events**, с помощью которой внутренние службы считывают сообщения, отправляемые в Центр с устройства в облако. Эта конечная точка совместима с концентраторами событий, поэтому можно использовать любой из механизмов для чтения сообщений, который поддерживает служба "Центры событий".

## <a name="read-from-the-built-in-endpoint"></a>Считывание данных из встроенной конечной точки

Некоторые интеграции продукта и пакеты SDK концентраторов событий знают о центре Интернета вещей и позволяют использовать строку подключения службы центра Интернета вещей для подключения к встроенной конечной точке.

При использовании пакетов SDK концентраторов событий или интеграции продуктов, которые не знаются с центром Интернета вещей, необходимо иметь совместимую с концентратором событий конечную точку и имя, совместимое с концентратором событий. Эти значения можно получить на портале следующим образом:

1. Войдите в [портал Azure](https://portal.azure.com) и перейдите в центр Интернета вещей.

2. Щелкните **Встроенные конечные точки**.

3. В разделе **событий** содержатся следующие значения: **Секции**, **имя, совместимое с концентратором событий**, **Конечная точка, совместимая с концентратором событий**, **время хранения**и **группы потребителей**.

    ![Параметры отправки сообщений с устройства в облако](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

На портале поле Конечная точка, совместимое с концентратором событий, содержит полную строку подключения концентраторов событий, которая выглядит следующим образом: **Endpoint = SB://abcd1234namespace.servicebus.Windows.NET/; SharedAccessKeyName = iothubowner; SharedAccessKey = кэйкэйкэйкэйкэйкэй =; EntityPath = iothub-ехуб-ABCD-1234-123456**. Если для пакета SDK, который вы используете, требуются другие значения, они будут выглядеть следующим образом:

| Имя | Значение |
| ---- | ----- |
| Конечная точка | sb://abcd1234namespace.servicebus.windows.net/ |
| Имя узла | abcd1234namespace.servicebus.windows.net |
| Пространство имен | abcd1234namespace |

Затем можно использовать любую политику общего доступа с разрешениями **ServiceConnect**, которые позволяют подключаться к указанному концентратору событий.

Пакеты SDK, которые можно использовать для подключения к встроенной конечной точке, совместимой с концентратором событий, которые предоставляет центр Интернета вещей:

| Язык | SDK | Пример |
| -------- | --- | ------ |
| .NET | https://github.com/Azure/azure-event-hubs-dotnet | [Краткое руководство](quickstart-send-telemetry-dotnet.md) |
 Java | https://github.com/Azure/azure-event-hubs-java | [Краткое руководство](quickstart-send-telemetry-java.md) |
| Node.js | https://www.npmjs.com/package/@azure/event-hubs | [Краткое руководство](quickstart-send-telemetry-node.md) |
| Python | https://pypi.org/project/azure-eventhub/ | https://github.com/Azure-Samples/azure-iot-samples-python/tree/master/iot-hub/Quickstarts/read-d2c-messages |

Интеграции продукта, которые можно использовать со встроенной конечной точкой, совместимой с концентраторами событий, включают в себя:

* [Функции Azure](https://docs.microsoft.com/azure/azure-functions/). См. статью [обработка данных из центра Интернета вещей с помощью функций Azure](https://azure.microsoft.com/resources/samples/functions-js-iot-hub-processing/).
* [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/). См. раздел [потоковые данные в качестве входных данных в Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-iot-hub).
* [Аналитика временных рядов](https://docs.microsoft.com/azure/time-series-insights/). См. раздел [Добавление источника событий центра Интернета вещей в среду "аналитика временных рядов](../time-series-insights/time-series-insights-how-to-add-an-event-source-iothub.md)".
* [Воронка Apache Storm](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). Вы можете просмотреть [источник воронки](https://github.com/apache/storm/tree/master/external/storm-eventhubs) на портале GitHub.
* [интеграция Apache Spark.](../hdinsight/spark/apache-spark-eventhub-streaming.md)
* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/).

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о конечных точках Центра Интернета вещей см. в статье [Руководство. Конечные точки Центра Интернета вещей](iot-hub-devguide-endpoints.md).

* В [кратких руководствах](quickstart-send-telemetry-node.md) рассказывается, как отправлять сообщения с имитированных устройств в облако и читать их из встроенной конечной точки. 

Дополнительные сведения см. в руководстве [Настройка маршрутизации сообщений с Центром Интернета вещей](tutorial-routing.md).

* Сведения о перенаправлении сообщений, отправляемых с устройства в облако, на пользовательские конечные точки см. в статье [Использование правил маршрутизации и пользовательских конечных точек для сообщений, отправляемых с устройства в облако](iot-hub-devguide-messages-read-custom.md).
