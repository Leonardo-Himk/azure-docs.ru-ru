---
title: Настройка коллекции общих образов в Azure DevTest Labs | Документация Майкрософт
description: Узнайте, как настроить общую коллекцию образов в Azure DevTest Labs
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2019
ms.author: spelluru
ms.openlocfilehash: 7591f22286f9ac451a15dd926adab0212adb190e
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691289"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Настройка коллекции общих образов в Azure DevTest Labs
DevTest Labs теперь поддерживает функцию [коллекции общих образов](../virtual-machines/windows/shared-image-galleries.md) . Он позволяет пользователям лаборатории получать доступ к изображениям из общего расположения при создании лабораторных ресурсов. Кроме того, он позволяет создавать структуру и организацию для пользовательских управляемых образов виртуальных машин. Коллекция общих образов поддерживает следующие возможности.

- Управляемая Глобальная репликация образов
- Управление версиями и группирование образов для упрощения управления
- Обеспечьте высокую доступность образов с учетными записями избыточного хранилища зоны (ZRS) в регионах, поддерживающих зоны доступности. ZRS обеспечивает лучшую устойчивость к сбоям зональные.
- Совместное использование в подписках и даже между клиентами с помощью управления доступом на основе ролей (RBAC).

Дополнительные сведения см. в [документации по общей коллекции образов](../virtual-machines/windows/shared-image-galleries.md). 
 
Если у вас есть большое количество управляемых образов, которые нужно сохранить и сделать доступными для всей компании, можно использовать коллекцию общих образов в качестве репозитория, что позволит легко обновлять и передавать образы. Как владелец лаборатории вы можете подключить к лаборатории существующую коллекцию общих образов. После присоединения этой коллекции пользователи лаборатории могут создавать компьютеры на основе этих последних образов. Основное преимущество этой функции заключается в том, что DevTest Labs теперь может использовать преимущества совместного использования образов в лабораториях, в подписках и в разных регионах. 

> [!NOTE]
> Дополнительные сведения о затратах, связанных со службой коллекции общих образов, см. в разделе [выставление счетов для общей коллекции образов](../virtual-machines/windows/shared-image-galleries.md#billing).

## <a name="considerations"></a>Рекомендации
- В лабораторию можно одновременно подключить только одну коллекцию общих образов. Если вы хотите подключить другую галерею, необходимо отключить существующую и присоединить другую. 
- В настоящее время DevTest Labs поддерживает только обобщенные образы коллекции образов.
- В настоящее время DevTest Labs не поддерживает передачу изображений в коллекцию через лабораторию. 
- При создании виртуальной машины с помощью общего образа коллекции образов DevTest Labs всегда использует последнюю опубликованную версию этого образа. Однако если образ имеет несколько версий, пользователь может выбрать создание компьютера из более ранней версии, перейдя на вкладку Дополнительные параметры во время создания виртуальной машины.  
- Несмотря на то что DevTest Labs автоматически делает наилучшую попытку обеспечить репликацию образов с помощью общей коллекции образов в регион, в котором существует Лаборатория, это не всегда возможно. Чтобы избежать проблем с созданием виртуальных машин на основе этих образов, убедитесь, что образы уже реплицированы в регион лаборатории.

## <a name="use-azure-portal"></a>Использование портала Azure
1. Войдите на [портал Azure](https://portal.azure.com).
1. В меню навигации слева выберите **все службы** .
1. Выберите **DevTest Labs** из списка.
1. В списке лабораторий выберите свою **лабораторию**.
1. Выберите **Конфигурация и политики** в разделе **Параметры** в меню слева.
1. В меню слева выберите **коллекции общих образов** в разделе **базовые виртуальные машины** .

    ![Меню галереи общих образов](./media/configure-shared-image-gallery/shared-image-galleries-menu.png)
1. Подключите имеющуюся коллекцию общих образов к своей лаборатории, нажав кнопку **присоединить** и выбрав коллекцию в раскрывающемся списке.

    ![Attach](./media/configure-shared-image-gallery/attach-options.png)
1. Перейдите в присоединенную коллекцию и настройте коллекцию, чтобы **включить или отключить** общие образы для создания виртуальной машины. Выберите коллекцию образов из списка, чтобы настроить ее. 

    По умолчанию параметр **Разрешить использование всех образов в качестве баз данных виртуальных машин** имеет значение **Да**. Это означает, что все образы, доступные в коллекции подключенных общих образов, будут доступны пользователю лаборатории при создании виртуальной машины лаборатории. Если доступ к определенным изображениям необходимо ограничить, измените параметр **Разрешить использование всех образов в качестве баз виртуальных машин** **без**изменений и выберите образы, которые нужно разрешить при создании виртуальных машин, а затем нажмите кнопку **сохранить** .

    ![Включить или отключить](./media/configure-shared-image-gallery/enable-disable.png)
1. Пользователи лаборатории могут создать виртуальную машину с помощью включенных образов. для этого щелкните **+ Добавить** и найдите образ на странице **Выбор базовой** страницы.

    ![Пользователи лаборатории](./media/configure-shared-image-gallery/lab-users.png)
## <a name="use-azure-resource-manager-template"></a>Использование шаблона Azure Resource Manager

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Подключение коллекции общих образов к лаборатории
Если вы используете шаблон Azure Resource Manager для подключения коллекции общих образов к лаборатории, необходимо добавить ее в раздел ресурсов шаблона диспетчер ресурсов, как показано в следующем примере:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "mylab",
    "location": "eastus",
    "resources": [
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/mySharedGalleryRg/providers/Microsoft.Compute/galleries/mySharedGallery",
            "allowAllImages": "Enabled"
        }
    }
    ]
}
```

Полный пример шаблона диспетчер ресурсов см. в разделе примеры шаблонов диспетчер ресурсов в нашем общедоступном репозитории GitHub: [Настройка общей коллекции образов при создании лаборатории](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured).

## <a name="use-rest-api"></a>Использование REST API

### <a name="get-a-list-of-labs"></a>Получение списка лабораторий 

```rest
GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs?api-version= 2018-10-15-preview
```

### <a name="get-the-list-of-shared-image-galleries-associated-with-a-lab"></a>Получение списка общих коллекций изображений, связанных с лабораторией

```rest
GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries?api-version= 2018-10-15-preview
   ```

### <a name="create-or-update-shared-image-gallery"></a>Создание или обновление коллекции общих образов

```rest
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}?api-version= 2018-10-15-preview
Body: 
{
    "properties":{
        "galleryId": "[Shared Image Gallery resource Id]",
        "allowAllImages": "Enabled"
    }
}

```

### <a name="list-images-in-a-shared-image-gallery"></a>Вывод списка образов в общей коллекции образов

```rest
GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
```



## <a name="next-steps"></a>Дальнейшие действия
См. следующие статьи о создании виртуальной машины с помощью образа из коллекции подключенных общих образов: [Создание виртуальной машины с помощью общего образа из коллекции](add-vm-use-shared-image.md) .
