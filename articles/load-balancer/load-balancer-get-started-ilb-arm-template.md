---
title: Создание внутренней подсистемы балансировки нагрузки на основе шаблона Azure
titleSuffix: Azure Load Balancer
description: Узнайте, как создать внутренний балансировщик нагрузки в диспетчере ресурсов с помощью шаблона.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 0d7cc4d571ddeb0b57fd4f025b8cbf7b204f61e6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79456970"
---
# <a name="create-an-internal-load-balancer-using-a-template"></a>Создайте внутренний балансировщик нагрузки с помощью шаблона

> [!div class="op_single_selector"]
> * [Портал Azure](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Шаблон](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Развертывание шаблона с помощью кнопки развертывания

Образец шаблона, который находится в общедоступном репозитории, использует файл параметров, содержащий значения по умолчанию для создания описанного выше сценария. Чтобы развернуть этот шаблон, перейдите по [данной ссылке](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), нажмите **Deploy to Azure**(Развернуть в Azure), при необходимости замените значения параметров по умолчанию и следуйте указаниям на портале.

## <a name="deploy-the-template-by-using-powershell"></a>Развертывание шаблона с помощью PowerShell

Чтобы развернуть шаблон, загруженный с помощью PowerShell, выполните описанные ниже действия.

1. Если вы ранее не использовали Azure PowerShell, следуйте инструкциям в статье [Установка и настройка Azure PowerShell](/powershell/azure/overview) , чтобы выполнить вход в Azure и выбрать подписку.
2. Скачайте файл параметров на локальный диск.
3. Измените и сохраните файл.
4. Выполните командлет **New-азресаурцеграупдеплоймент** , чтобы создать группу ресурсов с помощью шаблона.

    ```azurepowershell-interactive
    New-AzResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Развертывание шаблона с помощью интерфейса командной строки Azure

Чтобы развернуть шаблон с помощью интерфейса командной строки Azure, выполните следующие действия.

1. Если вы никогда не использовали Azure CLI, см. статью [Установка и настройка Azure CLI](../cli-install-nodejs.md) и следуйте инструкциям вплоть до точки, в которой вы выбрали учетную запись Azure и подписку.
2. Перейдите по адресу [https://shell.azure.com](https://shell.azure.com), чтобы открыть Cloud Shell в браузере. Выполните команду **azure config mode** , чтобы переключиться в режим диспетчера ресурсов, как показано ниже.

    ```console
    azure config mode arm
    ```

    Вот результат, ожидаемый для указанной выше команды:

        info:    New mode is arm

3. Откройте файл параметров, выберите его содержимое и сохраните его в файл на своем компьютере. В этом примере мы сохранили файл параметров в файл *parameters.json*.
4. Выполните команду **azure group deployment create** , чтобы развернуть новый внутренний балансировщик нагрузки с помощью данного шаблона и файлов параметров, которые вы скачали и изменили ранее. В списке, который откроется после выполнения команды, будут указаны используемые параметры.

    ```console
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Дальнейшие шаги

[Настройка режима распределения балансировщика нагрузки с помощью соответствия исходному IP-адресу](load-balancer-distribution-mode.md)

[Настройка параметров времени ожидания простоя TCP для подсистемы балансировки нагрузки](load-balancer-tcp-idle-timeout.md)

Синтаксис JSON и свойства подсистемы балансировки нагрузки в шаблоне см. в справочнике по шаблону для ресурса [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers).