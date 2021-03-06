---
title: Запрос общедоступных транзитных данных в реальном времени | Карты Microsoft Azure
description: Запросите общедоступные транзитные данные в реальном времени с помощью службы мобильности Microsoft Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 4743fbe84f5d41b4659e13d96868d2f64a473e4b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82086083"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-service"></a>Запрос общедоступных транзитных данных в реальном времени с помощью службы Azure Maps Mobility Service

В этой статье показано, как использовать [службу Azure Maps Mobility Service](https://aka.ms/AzureMapsMobilityService) для запроса общедоступных транзитных данных в режиме реального времени.

В этой статье вы узнаете, как запросить следующие поступления в реальном времени для всех строк, поступающих на заданную точку.

## <a name="prerequisites"></a>Предварительные условия

Сначала необходимо иметь учетную запись Azure Maps и ключ подписки, чтобы выполнять вызовы Azure Maps общедоступных транзитных API. Для получения сведений следуйте инструкциям в разделе [Создание учетной записи](quick-demo-map-app.md#create-an-account-with-azure-maps) для создания учетной записи Azure Maps. Выполните действия, описанные в разделе [Получение первичного ключа](quick-demo-map-app.md#get-the-primary-key-for-your-account) , чтобы получить первичный ключ для вашей учетной записи. Дополнительные сведения о проверке подлинности в Azure Maps см. в [этой статье](./how-to-manage-authentication.md).

В этой статье для создания вызовов REST используется [приложение Postman](https://www.getpostman.com/apps). Вы можете использовать любую среду разработки API.

## <a name="request-real-time-arrivals-for-a-stop"></a>Запрос прибытия в режиме реального времени для завершения

Чтобы запросить данные о поступлении конкретного открытого транзитного пути в режиме реального времени, необходимо выполнить запрос к [API прибытия в реальном времени](https://aka.ms/AzureMapsMobilityRealTimeArrivals) для [службы Azure Maps Mobility Service](https://aka.ms/AzureMapsMobilityService). Для выполнения запроса потребуются **Метроид** и **стопид** . Чтобы узнать больше о том, как запросить эти параметры, ознакомьтесь с нашим руководством по [запросу общих транзитных маршрутов](https://aka.ms/AMapsHowToGuidePublicTransitRouting).

Давайте будем использовать "522" в качестве идентификатора Metro, который является ИДЕНТИФИКАТОРом Metro для области "Сиэтл – Tacoma – Бельвью, WA". В качестве идентификатора завершения используйте "522---2060603". Эта шина останавливается в "NE 24 St & 162nd Ave NE, Бельвью WA". Чтобы запросить следующие пять данных о поступлении в реальном времени, для следующих прямых появления при этой ошибке выполните следующие действия.

1. Откройте приложение POST и создадим коллекцию для хранения запросов. В верхней части поступающего приложения выберите **создать**. В окне **создать новое** выберите **коллекция**.  Присвойте имя коллекции и нажмите кнопку **создать** .

2. Чтобы создать запрос, нажмите кнопку **создать** еще раз. В окне **создать новое** выберите **запрос**. Введите **имя запроса** для запроса. Выберите коллекцию, созданную на предыдущем шаге, в качестве расположения для сохранения запроса. Затем выберите **сохранить**.

    ![Создание запроса в POST](./media/how-to-request-transit-data/postman-new.png)

3. Выберите метод **получения** HTTP на вкладке Построитель и введите следующий URL-адрес для создания запроса GET. Замените `{subscription-key}`на Azure Maps первичный ключ.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. После успешного выполнения запроса вы получите следующий ответ.  Обратите внимание, что параметр "scheduleType" определяет, основывается ли предполагаемое время прибытия на основе данных в режиме реального времени или статических.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте, как запросить транзитные данные с помощью службы Mobility Service:

> [!div class="nextstepaction"]
> [Запрос передачи данных](how-to-request-transit-data.md)

Изучите документацию по API службы Mobility Service Azure Maps.

> [!div class="nextstepaction"]
> [Документация по API службы Mobility Service](https://aka.ms/AzureMapsMobilityService)
