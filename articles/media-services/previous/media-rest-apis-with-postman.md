---
title: Настройка Postman для вызовов REST API служб мультимедиа Azure
description: В этой статье описывается, как настроить POST для вызовов служб мультимедиа REST API.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 11c9c26e7c0f36e1e3dba732e90a6aef95e6ee14
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76694996"
---
# <a name="configure-postman-for-media-services-v2-rest-api-calls"></a>Настройка POST для вызовов служб мультимедиа v2 REST API вызовы  

> [!NOTE]
> В Cлужбы мультимедиа версии 2 больше не добавляются новые компоненты или функциональные возможности. <br/>Ознакомьтесь с последней версией [служб мультимедиа v3](https://docs.microsoft.com/azure/media-services/latest/). См. также [руководство по миграции из v2 в версии 3](../latest/migrate-from-v2-to-v3.md) .

В этом руководстве описано, как настроить **Postman** для вызова REST API служб мультимедиа Azure (AMS). В руководстве также показано, как импортировать файлы среды и коллекции в **Postman**. Коллекция содержит сгруппированные определения HTTP-запросов, которые вызывают REST API служб мультимедиа Azure (AMS). Файл среды содержит переменные, которые используются коллекцией.

Среда и коллекция описаны в руководствах по выполнению различных задач с помощью REST API служб мультимедиа Azure.

## <a name="prerequisites"></a>Предварительные условия

- Установите клиент REST [Postman](https://www.getpostman.com/) для выполнения REST API, как показано в некоторых руководствах по REST AMS. 

    Мы используем **Postman**, но подойдет любое средство REST. Другие варианты: **Visual Studio Code** с подключаемым модулем RESTful или **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Настройка среды 

1. Создайте JSON-файл, содержащий переменные среды, используемые в руководствах по AMS. Назовите файл (например, **AzureMediaServices.postman_environment.json**). Откройте файл и вставьте код, который определяет среду Postman из [этого примера кода](postman-environment.md). 
2. Откройте **Postman**.
3. В правой части экрана выберите параметр **Управление средой**.

    ![Отправка файла](./media/media-services-rest-upload-files/postman-create-env.png)
4. В диалоговом окне **Управление средой** нажмите кнопку **Импорт**.
5. Найдите и выберите файл **AzureMediaServices.postman_environment.json**.
6. Будет добавлена среда **AzureMedia**.
7. Закройте диалоговое окно.
8. Выберите среду **AzureMedia**.

    ![Отправка файла](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Настройка коллекции

1. Создайте JSON-файл, содержащий коллекцию **Postman** со всеми операциями, которые необходимы для передачи файла в службы мультимедиа. Назовите файл (например, **AzureMediaServicesOperations.postman_collection.json**). Откройте файл и вставьте код, который определяет коллекцию **Postman** из этого [примера кода](postman-collection.md).
2. Нажмите кнопку **импорта**, чтобы импортировать файл коллекции.
3. Выберите файл **AzureMediaServicesOperations.postman_collection.json**.

    ![Отправка файла](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Дальнейшие шаги

См. руководство по [передаче файлов](media-services-rest-upload-files.md).  
