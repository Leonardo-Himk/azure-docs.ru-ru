---
title: Запуск контейнера распознавателя форм в службе "экземпляры контейнеров Azure"
titleSuffix: Azure Cognitive Services
description: Разверните контейнер распознавателя форм в экземпляре контейнера Azure и протестируйте его в веб-браузере.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: acba4d9fab784181fda5728c30831c8c1838b91f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80879536"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Развертывание контейнера распознавателя форм в службе "экземпляры контейнеров Azure"

Узнайте, как развернуть контейнер [распознавателя форм](form-recognizer-container-howto.md) Cognitive Services в службе " [экземпляры контейнеров](https://docs.microsoft.com/azure/container-instances/)Azure". Эта процедура демонстрирует создание ресурса распознавателя форм Azure. Затем мы обсудим извлечение связанного образа контейнера. Наконец, мы выделим возможность выполнить согласование двух из браузера. Использование контейнеров может повлечь за собой внимание разработчиков от управления инфраструктурой до того, чтобы сосредоточиться на разработке приложений.

> [!IMPORTANT]
> Контейнеры распознавателя форм в настоящее время используют API распознавателя форм версии 1,0. Вы можете получить доступ к последней версии API, используя управляемую службу.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Запрос доступа к реестру контейнеров

Прежде всего необходимо заполнить и отправить [форму запроса доступа к контейнерам Распознавателя документов Cognitive Services](https://aka.ms/FormRecognizerContainerRequestAccess), чтобы запросить доступ к контейнеру. При этом вы зарегистрируетесь для использования API Компьютерного зрения. Вам не нужно специально выполнять вход в систему, чтобы заполнить форму запроса на API Компьютерного зрения. 

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
