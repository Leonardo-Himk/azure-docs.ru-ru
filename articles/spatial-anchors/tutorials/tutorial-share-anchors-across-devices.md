---
title: Руководство по совместному использованию пространственных привязок между сеансами и устройствами
description: Из этого руководства вы узнаете, как совместно использовать идентификаторы пространственных привязок Azure между устройствами Android и iOS в Unity с внутренней службой.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 3b377f87bdba40c90cb3af6caef2c089d7b7de49
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "77615494"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices"></a>Руководство по Совместное использование Пространственных привязок Azure между сеансами и устройствами

В этом руководстве описано, как использовать [Пространственные привязки Azure](../overview.md), чтобы создавать привязки во время одного сеанса, а затем обнаруживать их на том же или на другом устройстве. Те же самые привязки могут быть одновременно обнаружены несколькими устройствами в одном и том же расположении.

![Сохраняемость](./media/persistence.gif)

"Пространственные привязки Azure" — это кроссплатформенная служба разработчика, которая позволяет создавать среды смешанной реальности с применением объектов, не меняющих своего расположения на устройствах с течением времени. По завершении роботы с этим руководством у вас будет приложение, которое можно развернуть на двух или более устройствах. Пространственные привязки Azure, созданные одним экземпляром, могут совместно использоваться другими экземплярами.

Вы узнаете, как:

> [!div class="checklist"]
> * Развернуть веб-приложение ASP.NET Core в Azure, которое можно использовать для обмена привязками, храня их в памяти в течение некоторого времени.
> * Настроить сцену AzureSpatialAnchorsLocalSharedDemo в примере Unity из наших кратких руководств, чтобы воспользоваться преимуществами веб-приложения для совместного использования пространственных привязок.
> * развернуть и запустить привязку на одном или нескольких устройствах.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Хотя в этом руководстве рассматривается приложение Unity и веб-приложение ASP.NET Core, они представлены лишь в качестве примера того, как можно совместно использовать идентификаторы пространственных привязок Azure на других устройствах. Вы можете использовать другие языки и серверные технологии в тех же целях. Веб-приложение ASP.NET Core, используемое в этом руководстве, зависит от пакета SDK.NET Core 2.2. Оно отлично работает в обычных веб-приложениях Azure (для Windows), но в настоящее время не работает в веб-приложениях Azure для Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-the-sample-project"></a>Загрузка примера проекта

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Развертывание службы совместного использования привязок

## <a name="visual-studio"></a>[Visual Studio](#tab/VS)

Откройте Visual Studio, а затем откройте проект в папке `Sharing\SharingServiceSample`.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

## <a name="visual-studio-code"></a>[Visual Studio Code](#tab/VSC)

Перед развертыванием службы в VS Code необходимо создать группу ресурсов и план службы приложений.

### <a name="sign-in-to-azure"></a>Вход в Azure

Перейдите на <a href="https://portal.azure.com/" target="_blank">портал Azure</a> и войдите в свою подписку.

### <a name="create-a-resource-group"></a>Создание группы ресурсов

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Рядом с **группой ресурсов** выберите команду **Создать**.

Присвойте группе ресурсов имя **myResourceGroup** и нажмите кнопку **ОК**.

### <a name="create-an-app-service-plan"></a>Создание плана службы приложений

[!INCLUDE [app-service-plan](../../../includes/app-service-plan.md)]

Рядом с полем **План размещения** выберите **Создать**.

В диалоговом окне **Настройка плана размещения** используйте следующие параметры:

| Параметр | Рекомендуемое значение | Описание |
|-|-|-|
|План обслуживания приложения| MySharingServicePlan | Имя плана службы приложений. |
| Расположение | западная часть США | Центр обработки данных, где размещается веб-приложение. |
| Размер | Free | [Ценовая категория](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), которая определяет возможности размещения. |

Щелкните **ОК**.

Откройте Visual Studio Code, а затем откройте проект в папке `Sharing\SharingServiceSample`. Выполните действия в <a href="https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vscode?view=aspnetcore-2.2#open-it-with-visual-studio-code" target="_blank">этом руководстве</a>, чтобы развернуть службу общего доступа с помощью Visual Studio Code. Вы можете выполнить шаги, начиная с раздела "Открыть в Visual Studio Code". Не создавайте еще один проект MVC, как описано в предыдущем шаге, поскольку у вас уже есть проект, который нужно развернуть и опубликовать — SharingServiceSample.

---

## <a name="deploy-the-sample-app"></a>Развертывание примера приложения

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="unity-20193"></a>Unity 2019.3

Из-за критических изменений сейчас Unity 2019.3 не поддерживается. Используйте Unity 2019.1 или 2019.2.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы развернули веб-приложение ASP.NET Core в Azure, а затем настроили и развернули приложение Unity. Вы создали в приложении пространственные привязки и предоставили к ним доступ другим устройствам с помощью веб-приложения ASP.NET Core.

Вы можете улучшить веб-приложение ASP.NET Core, чтобы оно использовало Azure Cosmos DB для хранения хранилища общих идентификаторов пространственных привязок. Добавление поддержки Azure Cosmos DB позволит веб-приложению ASP.NET Core создавать привязку в один день, обнаружить ее снова спустя несколько дней с помощью идентификатора привязки, сохраненного в веб-приложении.

> [!div class="nextstepaction"]
> [Руководство по Совместном использовании Пространственных привязок Azure между сеансами и устройствами с помощью серверной части службы Azure Cosmos DB](./tutorial-use-cosmos-db-to-store-anchors.md)

