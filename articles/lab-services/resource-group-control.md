---
title: Выбор группы ресурсов для виртуальных машин в Azure DevTest Labs | Документация Майкрософт
description: Узнайте, как указать группу ресурсов для виртуальных машин в лаборатории Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2019
ms.author: spelluru
ms.openlocfilehash: 29816d158cf1428727b7ff17bcc2c347f402dedf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77134533"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Выбор группы ресурсов для виртуальных машин лаборатории в Azure DevTest Labs

В определенной группе ресурсов вы можете настроить виртуальные лаборатории в качестве владельца. Эта функция поможет вам в следующих случаях.

- Нужно меньше групп ресурсов, созданных заданиями в вашей подписке.
- Работа с лабораториями выполняется в фиксированном наборе настроенных групп ресурсов.
- Нужно обойти необходимые ограничения и одобрения для создания групп ресурсов в вашей подписке Azure.
- Объедините все лабораторные ресурсы в одной группе ресурсов, чтобы упростить отслеживание этих ресурсов и применение [политик](../governance/policy/overview.md) для управления ресурсами на уровне группы ресурсов.

С помощью этой функции можно использовать сценарий, чтобы указать новую или существующую группу ресурсов в подписке Azure для всех виртуальных машин лаборатории. В настоящее время Azure DevTest Labs поддерживает эту функцию через API.

> [!NOTE]
> Все ограничения подписки применяются при создании лабораторий в DevTest Labs. Представьте себе лабораторию в качестве любого другого ресурса в вашей подписке. В случае с группами ресурсов ограничение составляет [980 групп ресурсов на подписку](../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits). 

## <a name="use-azure-portal"></a>Использование портала Azure
Выполните следующие действия, чтобы указать группу ресурсов для всех виртуальных машин, созданных в лаборатории. 

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню навигации слева выберите **все службы** . 
3. Выберите **DevTest Labs** из списка.
4. В списке лабораторий выберите свою **лабораторию**.  
5. Выберите **Конфигурация и политики** в разделе **Параметры** в меню слева. 
6. Выберите **Параметры лаборатории** в меню слева. 
7. Выберите **все виртуальные машины в одной группе ресурсов**. 
8. Выберите существующую группу ресурсов в раскрывающемся списке (или) выберите **создать**, введите **имя** группы ресурсов и нажмите кнопку **ОК**. 

    ![Выберите группу ресурсов для всех виртуальных машин лаборатории](./media/resource-group-control/select-resource-group.png)

## <a name="use-powershell"></a>Использование PowerShell 
В следующем примере показано, как использовать скрипт PowerShell для создания всех виртуальных машин лаборатории в новой группе ресурсов.

```powershell
[CmdletBinding()]
Param(
    $subId,
    $labRg,
    $labName,
    $vmRg
)

az login | out-null

az account set --subscription $subId | out-null

$rgId = "/subscriptions/"+$subId+"/resourceGroups/"+$vmRg

"Updating lab '$labName' with vm rg '$rgId'..."

az resource update -g $labRg -n $labName --resource-type "Microsoft.DevTestLab/labs" --api-version 2018-10-15-preview --set properties.vmCreationResourceGroupId=$rgId

"Done. New virtual machines will now be created in the resource group '$vmRg'."
```

Вызовите скрипт с помощью следующей команды. ResourceGroup. ps1 — файл, содержащий предыдущий скрипт:

```powershell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-an-azure-resource-manager-template"></a>Использование шаблона Azure Resource Manager
Если вы используете шаблон Azure Resource Manager для создания лаборатории, используйте свойство **вмкреатионресаурцеграупид** в разделе "свойства лаборатории" шаблона, как показано в следующем примере:

```json
        {
            "type": "microsoft.devtestlab/labs",
            "name": "[parameters('lab_name')]",
            "apiVersion": "2018_10_15_preview",
            "location": "eastus",
            "tags": {},
            "scale": null,
            "properties": {
                "vmCreationResourceGroupId": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>",
                "labStorageType": "Premium",
                "premiumDataDisks": "Disabled",
                "provisioningState": "Succeeded",
                "uniqueIdentifier": "000000000f-0000-0000-0000-00000000000000"
            },
            "dependsOn": []
        },
```


## <a name="api-to-configure-a-resource-group-for-lab-vms"></a>API для настройки группы ресурсов для виртуальных машин лаборатории
При использовании этого API в качестве владельца лаборатории доступны следующие варианты:

- Выберите **группу ресурсов лаборатории** для всех виртуальных машин.
- Выберите **существующую группу ресурсов** , отличную от группы ресурсов лаборатории для всех виртуальных машин.
- Введите имя **новой группы ресурсов** для всех виртуальных машин.
- Продолжайте использовать существующее поведение, при котором группа ресурсов создается для каждой виртуальной машины в лаборатории.
 
Этот параметр применяется к новым виртуальным машинам, созданным в лаборатории. Старые виртуальные машины в лаборатории, созданные в собственных группах ресурсов, остаются без изменений. Среды, созданные в лаборатории, продолжают оставаться в собственных группах ресурсов.

Использование этого API.
- Используйте API версии **2018_10_15_preview**.
- При указании новой группы ресурсов убедитесь, что у вас есть **разрешения на запись в группы ресурсов** в вашей подписке. Если у вас отсутствуют разрешения на запись, создание новых виртуальных машин в указанной группе ресурсов завершится ошибкой.
- При использовании API передайте **полный идентификатор группы ресурсов**. Например: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Убедитесь, что группа ресурсов находится в той же подписке, что и лаборатория. 


## <a name="next-steps"></a>Дальнейшие действия
См. следующие статьи: 

- [Управление базовыми политиками лаборатории в Azure DevTest Labs](devtest-lab-get-started-with-lab-policies.md)
- [Вопросы и ответы](devtest-lab-faq.md)
