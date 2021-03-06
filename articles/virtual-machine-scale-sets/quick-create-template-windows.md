---
title: Краткое руководство. Создание масштабируемого набора виртуальных машин Windows с помощью шаблона Azure
description: Узнайте, как быстро создать масштабируемый набор виртуальных машин Windows с помощью шаблона Azure Resource Manager, который позволяет развернуть пример приложения и настроить правила автоматического масштабирования.
author: ju-shim
ms.author: jushiman
ms.topic: quickstart
ms.service: virtual-machine-scale-sets
ms.subservice: windows
ms.date: 03/27/2020
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 8bf75dc08a033c254152cb6e5b6f3af6389d8380
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198207"
---
# <a name="quickstart-create-a-windows-virtual-machine-scale-set-with-an-azure-template"></a>Краткое руководство. Создание масштабируемого набора виртуальных машин Windows с помощью шаблона Azure

Масштабируемый набор виртуальных машин обеспечивает развертывание и администрирование набора идентичных автомасштабируемых виртуальных машин. Вы можете вручную изменить число виртуальных машин в масштабируемом наборе или определить правила для автоматического масштабирования в зависимости от использования ЦП, объема памяти или сетевого трафика. После этого Azure Load Balancer будет распределять трафик между экземплярами виртуальных машин в масштабируемом наборе. С помощью этого краткого руководства вы создадите масштабируемый набор виртуальных машин и развернете пример приложения с помощью шаблона Azure Resource Manager.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Нет.

## <a name="create-a-scale-set"></a>Создание масштабируемого набора

Шаблоны Azure Resource Manager позволяют развертывать группы связанных ресурсов. С помощью одного шаблона можно создать масштабируемый набор виртуальных машин, установить приложения и настроить правила автоматического масштабирования. Вы можете повторно использовать этот шаблон, применив переменные и параметры, чтобы обновить существующие или создать дополнительные масштабируемые наборы. Шаблоны можно развернуть с помощью портала Azure, Azure CLI, Azure PowerShell либо же на основе конвейеров непрерывной интеграции и непрерывной поставки (CI/CD).

### <a name="review-the-template"></a>Изучение шаблона

Шаблон, используемый в этом кратком руководстве, взят из [шаблонов быстрого запуска Azure](https://azure.microsoft.com/resources/templates/201-vmss-windows-webapp-dsc-autoscale/).

:::code language="json" source="~/quickstart-templates/201-vmss-windows-webapp-dsc-autoscale/azuredeploy.json" range="1-397" highlight="236-325":::

В этих шаблонах определены следующие ресурсы:

- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks);
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadbalancers);
- [**Microsoft.Compute/virtualMachineScaleSets**](/azure/templates/microsoft.compute/virtualmachinescalesets);
- [**Microsoft.Insights/autoscaleSettings**](/azure/templates/microsoft.insights/autoscalesettings).

#### <a name="define-a-scale-set"></a>Определение масштабируемого набора

Выделенная часть — это определение ресурса масштабируемого набора. Чтобы создать масштабируемый набор с помощью шаблона, определите необходимые ресурсы. Ниже перечислены основные элементы и типы ресурсов для масштабируемого набора виртуальных машин.

| Свойство                     | Описание свойства                                  | Пример значения в шаблоне                    |
|------------------------------|----------------------------------------------------------|-------------------------------------------|
| type                         | Создаваемый тип ресурса Azure                            | Microsoft.Compute/virtualMachineScaleSets; |
| name                         | Имя масштабируемого набора                                       | myScaleSet                                |
| location                     | Расположение для создания масштабируемого набора                     | Восточная часть США                                   |
| sku.name                     | Размер виртуальной машины для каждого экземпляра в масштабируемом наборе                  | Standard_A1                               |
| sku.capacity                 | Количество экземпляров виртуальных машин, которое требуется изначально создать           | 2                                         |
| upgradePolicy.mode           | Режим обновления экземпляра виртуальной машины при внесении изменений              | Автоматически                                 |
| imageReference               | Платформа или пользовательский образ, используемые для экземпляров виртуальных машин | Microsoft Windows Server 2016 Datacenter  |
| osProfile.computerNamePrefix | Префикс имени для каждого экземпляра виртуальной машины                     | myvmss                                    |
| osProfile.adminUsername      | Имя пользователя для каждого экземпляра виртуальной машины                        | azureuser                                 |
| osProfile.adminPassword      | Пароль для каждого экземпляра виртуальной машины                        | P@ssw0rd!                                 |

Чтобы настроить шаблон масштабируемого набора, вы можете изменить размер виртуальной машины или начальную емкость. Еще один вариант — использовать другую платформу или пользовательский образ.

#### <a name="add-a-sample-application"></a>Добавление примера приложения

Для проверки масштабируемого набора установите базовое веб-приложение. Когда вы развертываете масштабируемый набор, расширения виртуальных машин могут предусматривать задачи автоматизации и настройки после развертывания, например задачи установки приложения. Сценарии можно скачать из службы хранилища Azure или GitHub или передать на портал Azure во время выполнения расширения. Чтобы применить расширение к масштабируемому набору, добавьте раздел *extensionProfile* в предыдущий пример ресурса. Как правило, профиль расширения определяет следующие свойства:

- тип расширения;
- издатель расширения;
- версия расширения;
- расположение скриптов настройки или установки;
- команды для выполнения на экземплярах виртуальных машин.

В примере шаблона используется расширение PowerShell DSC для установки приложения ASP.NET MVC, выполняющегося в IIS.

Скрипт установки скачивается с сайта GitHub, как определено в свойстве *url*. Затем расширение запускает *InstallIIS* из скрипта *IISInstall.ps1*, как определено в свойствах *function* и *script*. Само приложение ASP.NET предоставляется в виде пакета веб-развертывания, который также скачивается с сайта GitHub, как определено в свойстве *WebDeployPackagePath*:

## <a name="deploy-the-template"></a>Развертывание шаблона

Вы можете развернуть шаблон, нажав кнопку **Развертывание в Azure**. После ее нажатия откроется портал Azure, загрузится весь шаблон и появится диалог для ввода параметров, таких как имя масштабируемого набора, число экземпляров и учетные данные администратора.

[![Развертывание шаблона в Azure](media/virtual-machine-scale-sets-create-template/deploy-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-vmss-windows-webapp-dsc-autoscale%2Fazuredeploy.json)

Вы также можете развернуть шаблон Resource Manager с помощью Azure PowerShell:

```azurepowershell-interactive
# Create a resource group
New-AzResourceGroup -Name myResourceGroup -Location EastUS

# Deploy template into resource group
New-AzResourceGroupDeployment `
    -ResourceGroupName myResourceGroup `
    -TemplateURI https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/azuredeploy.json

# Update the scale set and apply the extension
Update-AzVmss `
    -ResourceGroupName myResourceGroup `
    -VmScaleSetName myVMSS `
    -VirtualMachineScaleSet $vmssConfig
```

По запросу укажите имя масштабируемого набора и учетные данные администратора для экземпляров виртуальных машин. Чтобы создать масштабируемый набор и применить расширение для настройки приложения, может потребоваться 10–15 минут.

## <a name="test-the-deployment"></a>тестирование развертывания

Чтобы увидеть, как работает масштабируемый набор, откройте пример веб-приложения в браузере. Получите общедоступный IP-адрес своей подсистемы балансировки нагрузки с помощью командлета [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress), как показано ниже:

```azurepowershell-interactive
Get-AzPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Введите в браузере общедоступный IP-адрес своей подсистемы балансировки нагрузки в формате *http:\//publicIpAddress/MyApp*. Подсистема балансировки нагрузки передаст запрос на один из экземпляров виртуальной машины, как показано в следующем примере:

![Выполнение сайта IIS](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Вы можете удалить ненужную группу ресурсов и масштабируемый набор с помощью командлета [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup). Параметр `-Force` подтверждает, что вы хотите удалить ресурсы без дополнительного запроса. При использовании параметра `-AsJob` управление возвращается в командную строку без ожидания завершения операции.

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup" -Force -AsJob
```

## <a name="next-steps"></a>Дальнейшие действия

С помощью этого краткого руководства вы создали масштабируемый набор Windows с помощью шаблона Azure, а также установили простое приложение ASP.NET на экземплярах виртуальных машин с помощью расширения PowerShell DSC. Чтобы получить дополнительные сведения, продолжите роботу с руководством по созданию масштабируемых наборов виртуальных машин Azure и управлению ими.

> [!div class="nextstepaction"]
> [Создание масштабируемых наборов виртуальных машин Azure и управление ими](tutorial-create-and-manage-powershell.md)
