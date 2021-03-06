---
title: Перемещение внутренних Load Balancer Azure в другой регион Azure с помощью Azure PowerShell
description: Использование шаблона Azure Resource Manager для перемещения внутренних Load Balancer Azure из одного региона Azure в другой с помощью Azure PowerShell
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: f8e431124155fe23853fe61e985fe4db522c3f77
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75644279"
---
# <a name="move-azure-internal-load-balancer-to-another-region-using-powershell"></a>Перемещение внутренних Load Balancer Azure в другой регион с помощью PowerShell

Существуют различные сценарии, в которых необходимо переместить имеющуюся внутреннюю подсистему балансировки нагрузки из одного региона в другой. Например, может потребоваться создать внутреннюю подсистему балансировки нагрузки с той же конфигурацией для тестирования. Также может потребоваться переместить внутреннюю подсистему балансировки нагрузки в другой регион в рамках планирования аварийного восстановления.

Внутренние подсистемы балансировки нагрузки Azure невозможно переместить из одного региона в другой. Однако можно использовать шаблон Azure Resource Manager для экспорта существующей конфигурации и виртуальной сети внутренней подсистемы балансировки нагрузки.  Затем можно разместить ресурс в другом регионе, экспортировав подсистему балансировки нагрузки и виртуальную сеть в шаблон, изменив параметры в соответствии с регионом назначения, а затем развернув шаблоны в новом регионе.  Дополнительные сведения о диспетчер ресурсов и шаблонах см. [в статье экспорт групп ресурсов в шаблоны](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates) .


## <a name="prerequisites"></a>Предварительные требования

- Убедитесь, что внутренний балансировщик нагрузки Azure находится в регионе Azure, из которого вы хотите переместиться.

- Внутренние подсистемы балансировки нагрузки Azure нельзя перемещать между регионами.  Необходимо связать новую подсистему балансировки нагрузки с ресурсами в целевом регионе.

- Чтобы экспортировать конфигурацию внутренней подсистемы балансировки нагрузки и развернуть шаблон для создания внутренней подсистемы балансировки нагрузки в другом регионе, вам потребуется роль "участник сети" или выше.
   
- Определите структуру сети в исходном регионе и все ресурсы, которые вы сейчас используете. Этот макет включает, но не ограничен подсистемами балансировки нагрузки, группами безопасности сети, виртуальными машинами и виртуальными сетями.

- Убедитесь, что подписка Azure позволяет создавать внутренние подсистемы балансировки нагрузки в используемом целевом регионе. Свяжитесь со службой поддержки, чтобы включить необходимые квоты.

- Убедитесь, что у вашей подписки достаточно ресурсов для поддержки добавления подсистем балансировки нагрузки для этого процесса.  См. раздел [Подписка Azure, пределы, квоты и ограничения службы](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits) .


## <a name="prepare-and-move"></a>Подготовка и перемещение
Ниже показано, как подготовить внутреннюю подсистему балансировки нагрузки для перемещения с помощью шаблона диспетчер ресурсов и переместить конфигурацию внутренней подсистемы балансировки нагрузки в целевую область с помощью Azure PowerShell.  В рамках этого процесса необходимо добавить конфигурацию виртуальной сети для внутреннего балансировщика нагрузки и выполнить ее сначала перед перемещением внутренней подсистемы балансировки нагрузки.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-virtual-network-template-and-deploy-from-azure-powershell"></a>Экспорт шаблона виртуальной сети и развертывание из Azure PowerShell

1. Войдите в подписку Azure с помощью команды [Connect-азаккаунт](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) и следуйте инструкциям на экране.
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```
2.  Получите идентификатор ресурса виртуальной сети, которую необходимо переместить в целевой регион, и поместите ее в переменную с помощью команды [Get-азвиртуалнетворк](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourceVNETID = (Get-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Экспортируйте исходную виртуальную сеть в файл JSON в каталог, в котором выполняется команда [Export-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0):
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. Скачанный файл будет называться именем группы ресурсов, из которой был экспортирован ресурс.  Выберите файл, который был экспортирован из команды с именем ** \<Resource-Group-Name>. JSON** , и откройте ее в любом редакторе:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Чтобы изменить параметр имени виртуальной сети, измените значение **DefaultValue** для свойства имя исходной виртуальной сети на имя целевой виртуальной сети, а затем убедитесь, что оно находится в кавычках:
    
    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentmyResourceGroupVNET.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_myVNET1_name": {
        "defaultValue": "<target-virtual-network-name>",
        "type": "String"
        }
    ```

6.  Чтобы изменить целевой регион, в который будет перемещена виртуальная сеть, измените свойство **Location** в разделе ресурсы:

    ```json
    "resources": [
                {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region>",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                        "addressSpace": {
                            "addressPrefixes": [
                                "10.0.0.0/16"
                            ]
                        },

    ```
  
7. Чтобы получить коды расположения регионов, можно использовать командлет Azure PowerShell [Get-азлокатион](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) , выполнив следующую команду:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8.  Кроме того, можно изменить другие параметры в файле ** \<Resource-Group-Name>. JSON** , если выбрать, и они необязательны в зависимости от требований.

    * **Адресное пространство** . адресное пространство виртуальной сети можно изменить перед сохранением, изменив раздел **Resources** > **аддрессспаце** и изменив свойство **аддресспрефиксес** в файле ** \<Resource-Group-Name>. JSON** :

        ```json
                "resources": [
                    {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                    "addressSpace": {
                        "addressPrefixes": [
                        "10.0.0.0/16"
                        ]
                    },

        ```

    * **Подсеть** — имя подсети и адресное пространство подсети можно изменить или добавить в, изменив раздел **подсети** в ** \<файле Resource-Group-Name>. JSON** . Имя подсети можно изменить, изменив свойство **Name** . Адресное пространство подсети можно изменить, изменив свойство **addressPrefix** в файле ** \<Resource-Group-Name>. JSON** :

        ```json
                "subnets": [
                    {
                    "name": "subnet-1",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.0.0/24",
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    },
                    {
                    "name": "GatewaySubnet",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.1.0/29",
                    "serviceEndpoints": [],
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    }

                ]
        ```

         В файле ** \<Resource-Group-Name>. JSON** для изменения префикса адреса его необходимо изменить в двух местах: в разделе, приведенном выше, и в разделе **Type** , приведенном ниже.  Измените свойство **addressPrefix** , чтобы оно совпадало с указанным выше:

        ```json
         "type": "Microsoft.Network/virtualNetworks/subnets",
           "apiVersion": "2019-06-01",
           "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/GatewaySubnet')]",
              "dependsOn": [
                 "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                   ],
              "properties": {
                 "provisioningState": "Succeeded",
                 "addressPrefix": "10.0.1.0/29",
                 "serviceEndpoints": [],
                 "delegations": [],
                 "privateEndpointNetworkPolicies": "Enabled",
                 "privateLinkServiceNetworkPolicies": "Enabled"
                  }
                 },
                  {
                  "type": "Microsoft.Network/virtualNetworks/subnets",
                  "apiVersion": "2019-06-01",
                  "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/subnet-1')]",
                     "dependsOn": [
                        "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                          ],
                     "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "10.0.0.0/24",
                        "delegations": [],
                        "privateEndpointNetworkPolicies": "Enabled",
                        "privateLinkServiceNetworkPolicies": "Enabled"
                         }
                  }
         ]
        ```

9.  Сохраните файл ** \<Resource-Group-Name>. JSON** .

10. Создайте группу ресурсов в целевом регионе для развертывания целевой виртуальной сети с помощью [New-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0) .
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
11. Разверните измененный ** \<файл Resource-Group-Name>. JSON** в группе ресурсов, созданной на предыдущем шаге, с помощью команды [New-азресаурцеграупдеплоймент](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0).

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```
12. Чтобы проверить, созданы ли ресурсы в целевом регионе, используйте [Get-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) и [Get-азвиртуалнетворк](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzVirtualNetwork -Name <target-virtual-network-name> -ResourceGroupName <target-resource-group-name>

    ```
### <a name="export-the-internal-load-balancer-template-and-deploy-from-azure-powershell"></a>Экспорт шаблона внутренней подсистемы балансировки нагрузки и развертывание из Azure PowerShell

1. Войдите в подписку Azure с помощью команды [Connect-азаккаунт](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) и следуйте инструкциям на экране.
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Получите идентификатор ресурса внутренней подсистемы балансировки нагрузки, которую необходимо переместить в целевой регион, и поместите ее в переменную с помощью команды [Get-азлоадбаланцер](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourceIntLBID = (Get-AzLoadBalancer -Name <source-internal-lb-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Экспортируйте конфигурацию внутренней подсистемы балансировки нагрузки в файл JSON в каталог, в котором выполняется команда [Export-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0):
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceIntLBID -IncludeParameterDefaultValue
   ```
4. Скачанный файл будет называться именем группы ресурсов, из которой был экспортирован ресурс.  Выберите файл, который был экспортирован из команды с именем ** \<Resource-Group-Name>. JSON** , и откройте ее в любом редакторе:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Чтобы изменить параметр имени внутренней подсистемы балансировки нагрузки, измените значение **DefaultValue** для свойства "имя внутренней подсистемы балансировки нагрузки" на имя целевой внутренней подсистемы балансировки нагрузки и убедитесь, что имя находится в кавычках:

    ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-external-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_externalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
    ```
 
6. Чтобы изменить целевую виртуальную сеть, которая была перемещена выше, сначала необходимо получить идентификатор ресурса, а затем скопировать и вставить его в файл ** \<Resource-Group-Name>. JSON** .  Чтобы получить идентификатор, используйте команду [Get-азвиртуалнетворк](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0):
   
   ```azurepowershell-interactive
    $targetVNETID = (Get-AzVirtualNetwork -Name <target-vnet-name> -ResourceGroupName <target-resource-group-name>).Id
    ```
    Введите переменную и нажмите клавишу ВВОД, чтобы отобразить идентификатор ресурса.  Выделите путь к ИДЕНТИФИКАТОРу и скопируйте его в буфер обмена:

    ```powershell
    PS C:\> $targetVNETID
    /subscriptions/7668d659-17fc-4ffd-85ba-9de61fe977e8/resourceGroups/myResourceGroupVNET-Move/providers/Microsoft.Network/virtualNetworks/myVNET2-Move
    ```

7.  В файле ** \<Resource-Group-Name>. JSON** вставьте **идентификатор ресурса** из переменной вместо **DefaultValue** во втором параметре для целевого идентификатора виртуальной сети, убедитесь, что путь заключен в кавычки:
   
    ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-external-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_externalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
    ```

8. Чтобы изменить целевой регион, в который будет перемещена конфигурация внутренней подсистемы балансировки нагрузки, измените свойство **Location** в разделе **ресурсы** в файле ** \<Resource-Group-Name>. JSON** :

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-internal-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

11. Чтобы получить коды расположения регионов, можно использовать командлет Azure PowerShell [Get-азлокатион](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) , выполнив следующую команду:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
12. Кроме того, можно изменить другие параметры в шаблоне, если вы выбираете и являются необязательными в зависимости от ваших требований:
    
    * **SKU** . номер SKU внутренней подсистемы балансировки нагрузки можно изменить в конфигурации с Standard на Basic или Basic на Standard, изменив свойство**имя** **SKU** > в файле ** \<Resource-Group-Name>. JSON** .

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-internal-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Дополнительные сведения о различиях между подсистемами балансировки нагрузки уровня "базовый" и "Стандартный" см. в статье [обзор Load Balancer (цен. Категория "Стандартный") Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) .

    * **Правила балансировки нагрузки** . в конфигурацию можно добавить или удалить правила балансировки нагрузки, добавив или удалив записи в разделе **лоадбаланЦингрулес** файла ** \<Resource-Group-Name>. JSON** .

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Дополнительные сведения о правилах балансировки нагрузки см [. в разделе что такое Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Пробы** . Вы можете добавить или удалить пробу для балансировщика нагрузки в конфигурации, добавив или удалив записи в разделе **зонды** файла ** \<Resource-Group-Name>. JSON** :

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Дополнительные сведения о Azure Load Balancer зондах работоспособности см. в разделе [проверки работоспособности Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) .

    * **Правила NAT для входящего трафика** . Вы можете добавить или удалить правила NAT для входящего трафика для подсистемы балансировки нагрузки, добавив или удалив записи в разделе **inboundNatRules** файла ** \<Resource-Group-Name>. JSON** .

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Чтобы завершить добавление или удаление правила NAT для входящего трафика, оно должно присутствовать или удалено в качестве свойства **типа** в конце файла ** \<Resource-Group-Name>. JSON** :

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Дополнительные сведения о правилах NAT для входящего трафика см. в разделе [что такое Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)
    
13. Сохраните файл ** \<Resource-Group-Name>. JSON** .
    
10. Создайте или группу ресурсов в целевом регионе для развертывания целевой внутренней подсистемы балансировки нагрузки с помощью [New-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0). Существующую группу ресурсов, приведенную выше, можно также использовать повторно в рамках этого процесса:
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Разверните измененный ** \<файл Resource-Group-Name>. JSON** в группе ресурсов, созданной на предыдущем шаге, с помощью команды [New-азресаурцеграупдеплоймент](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0).

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Чтобы проверить, созданы ли ресурсы в целевом регионе, используйте [Get-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) и [Get-азлоадбаланцер](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzLoadBalancer -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>Игнорировать 

Если после развертывания вы хотите начать или удалить виртуальную сеть и подсистему балансировки нагрузки в целевом объекте, удалите группу ресурсов, созданную в целевом объекте, и перемещенную виртуальную сеть и подсистему балансировки нагрузки будут удалены.  Чтобы удалить группу ресурсов, используйте команду [Remove-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

## <a name="clean-up"></a>Очистка

Чтобы зафиксировать изменения и завершить перемещение NSG, удалите исходный NSG или группу ресурсов, используйте [Remove-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) или Remove- [Азвиртуалнетворк](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork?view=azps-2.6.0) и [Remove-азлоадбаланцер](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer?view=azps-2.6.0) .

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

``` azurepowershell-interactive

Remove-AzLoadBalancer -name <load-balancer> -ResourceGroupName <resource-group-name>

Remove-AzVirtualNetwork -Name <virtual-network-name> -ResourceGroupName <resource-group-name>


```

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы переместили внутренний балансировщик нагрузки Azure из одного региона в другой и очистили исходные ресурсы.  Дополнительные сведения о перемещении ресурсов между регионами и аварийным восстановлением в Azure см. по следующим ссылке:


- [Перемещение ресурсов в новую группу ресурсов или подписку](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Перенос виртуальных машин Azure в другой регион](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
