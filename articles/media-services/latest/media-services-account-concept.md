---
title: Управление учетными записями служб мультимедиа Azure v3 | Документация Майкрософт
description: Чтобы начать шифрование, кодирование, анализ, потоковую передачу мультимедийного содержимого и управление им в Azure, необходимо создать учетную запись Служб мультимедиа. В этой статье объясняется, как управлять учетными записями служб мультимедиа Azure версии 3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 08579f7ba952bb4ebcba1595508612affb852528
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75980384"
---
# <a name="manage-azure-media-services-v3-accounts"></a>Управление учетными записями служб мультимедиа Azure v3

Чтобы начать шифрование, кодирование, анализ, потоковую передачу мультимедийного содержимого и управление им в Azure, необходимо создать учетную запись Служб мультимедиа. При создании учетной записи Служб мультимедиа необходимо предоставить имя ресурса учетной записи хранения Azure. Указанная учетная запись хранения присоединена к учетной записи Служб мультимедиа. Учетная запись Служб мультимедиа и все связанные учетные записи хранения должны размещаться в одной подписке Azure. Дополнительные сведения см. в разделе [учетные записи хранения](storage-account-concept.md).

## <a name="moving-a-media-services-account-between-subscriptions"></a>Перемещение учетной записи служб мультимедиа между подписками 

Если необходимо переместить учетную запись служб мультимедиа в новую подписку, сначала необходимо переместить всю группу ресурсов, содержащую учетную запись служб мультимедиа, в новую подписку. Необходимо переместить все подключенные ресурсы: учетные записи хранения Azure, профили Azure CDN и т. д. Дополнительные сведения см. [в статье перемещение ресурсов в новую группу ресурсов или подписку](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Как и в случае с любыми ресурсами в Azure, перемещение группы ресурсов может занять некоторое время.

> [!NOTE]
> Службы мультимедиа v3 поддерживают модель с несколькими арендыми.

### <a name="considerations"></a>Рекомендации

* Перед миграцией в другую подписку создайте резервные копии всех данных в учетной записи.
* Необходимо отключить все конечные точки потоковой передачи и ресурсы потоковой трансляции. Пользователи не смогут получить доступ к вашему содержимому на время перемещения группы ресурсов. 

> [!IMPORTANT]
> Не запускайте конечную точку потоковой передачи, пока перемещение не завершится успешно.

### <a name="troubleshoot"></a>Устранение неполадок 

Если учетная запись служб мультимедиа или связанная учетная запись хранения Azure отключаются после перемещения группы ресурсов, попробуйте поворачивать ключи учетной записи хранения. Если при повороте ключей учетной записи хранения не разрешается состояние "отключено" для учетной записи служб мультимедиа, отправьте новый запрос в службу поддержки из меню "поддержка и устранение неполадок" в учетной записи служб мультимедиа.  

## <a name="next-steps"></a>Дальнейшие действия

[Создание учетной записи](create-account-cli-quickstart.md)
