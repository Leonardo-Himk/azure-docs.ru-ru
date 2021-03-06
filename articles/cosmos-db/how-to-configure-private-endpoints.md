---
title: Настройка частной ссылки Azure для учетной записи Azure Cosmos
description: Узнайте, как настроить личную ссылку Azure для доступа к учетной записи Azure Cosmos с помощью частного IP-адреса в виртуальной сети.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: thweiss
ms.openlocfilehash: 4b49d2aa61587d0156755bdd5c47b3eeb90090a5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81270695"
---
# <a name="configure-azure-private-link-for-an-azure-cosmos-account"></a>Настройка частной ссылки Azure для учетной записи Azure Cosmos

С помощью функции "Частная связь Azure" можно подключиться к учетной записи Azure Cosmos с помощью частной конечной точки. Частная конечная точка — это набор частных IP-адресов в подсети в виртуальной сети. Затем можно ограничить доступ к учетной записи Azure Cosmos через частные IP-адреса. Если частная ссылка сочетается с ограниченными политиками NSG, она помогает снизить риск утечка данных. Дополнительные сведения о частных конечных точках см. в статье о [частной ссылке Azure](../private-link/private-link-overview.md) .

Частная ссылка позволяет пользователям получить доступ к учетной записи Azure Cosmos из виртуальной сети или из любой одноранговой виртуальной сети. Ресурсы, сопоставленные с частной ссылкой, также доступны локально через частный пиринг через VPN или Azure ExpressRoute. 

Вы можете подключиться к учетной записи Azure Cosmos, настроенной с помощью закрытой ссылки, используя метод автоматического или ручного утверждения. Дополнительные сведения см. в разделе " [Рабочий процесс утверждения](../private-link/private-endpoint-overview.md#access-to-a-private-link-resource-using-approval-workflow) " в документации по частному каналу. 

В этой статье описываются действия по созданию частной конечной точки. Предполагается, что вы используете метод автоматического утверждения.

> [!NOTE]
> Частная конечная точка в настоящее время доступна только для режима подключения шлюза. В режиме прямого доступа он доступен в виде предварительной версии функции.

## <a name="create-a-private-endpoint-by-using-the-azure-portal"></a>Создайте закрытую конечную точку с помощью портал Azure

Чтобы создать частную конечную точку для существующей учетной записи Azure Cosmos с помощью портал Azure, выполните следующие действия.

1. В области **все ресурсы** выберите учетную запись Azure Cosmos.

1. Выберите **подключения частной конечной точки** в списке параметров, а затем выберите **Частная конечная точка**.

   ![Параметры для создания частной конечной точки в портал Azure](./media/how-to-configure-private-endpoints/create-private-endpoint-portal.png)

1. В области **Создание частной конечной точки — основы** введите или выберите следующие сведения.

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** | |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите группу ресурсов.|
    | **Сведения об экземпляре** |  |
    | Имя | Введите любое имя для частной конечной точки. Если это имя используется, создайте уникальное. |
    |Регион| Выберите регион, в котором требуется развернуть закрытую ссылку. Создайте частную конечную точку в том же расположении, где находится виртуальная сеть.|
    |||
1. По завершении выберите **Next: Ресурс**.
1. В окне **Создание частной конечной точки — Ресурс** введите или выберите следующую информацию:

    | Параметр | Значение |
    | ------- | ----- |
    |Метод подключения  | Выберите **подключиться к ресурсу Azure в моем каталоге**. <br/><br/> Затем можно выбрать один из ресурсов, чтобы настроить частную ссылку. Вы также можете подключиться к ресурсу другого пользователя с помощью идентификатора ресурса или псевдонима, к которому вам предоставили доступ.|
    | Подписка| Выберите свою подписку. |
    | Тип ресурса | Выберите **Microsoft. азурекосмосдб/databaseAccounts**. |
    | Ресурс |Выберите учетную запись Azure Cosmos. |
    |Целевой подресурс |Выберите тип API Azure Cosmos DB, который нужно сопоставлять. По умолчанию используется только один вариант для API-интерфейсов SQL, MongoDB и Cassandra. Для API-интерфейсов Gremlin и Table можно также выбрать **SQL** , так как эти API-интерфейсы совместимы с API SQL. |
    |||

1. По завершении выберите **Next: Конфигурация**.
1. В окне **Создание частной конечной точки — конфигурация**введите или выберите следующие сведения:

    | Параметр | Значение |
    | ------- | ----- |
    |**Сетевое взаимодействие**| |
    | Виртуальная сеть| Выберите виртуальную сеть. |
    | Подсеть | Выберите подсеть. |
    |**Интеграция Частная зона DNS**||
    |Интеграция с частной зоной DNS |Выберите **Да**. <br><br/> Для частного подключения к частной конечной точке требуется запись DNS. Рекомендуется интегрировать закрытую конечную точку с частной зоной DNS. Вы также можете использовать собственные DNS-серверы или создать записи DNS с помощью файлов узлов на виртуальных машинах. |
    |Частная зона DNS |Выберите **privatelink.Documents.Azure.com**. <br><br/> Частная зона DNS определяется автоматически. Его нельзя изменить с помощью портал Azure.|
    |||

1. Выберите **Review + create** (Просмотреть и создать). На странице " **Проверка и создание** " Azure проверяет конфигурацию.
1. При появлении сообщения **Проверка пройдена** нажмите кнопку **Создать**.

Если вы утвердили закрытую ссылку для учетной записи Azure Cosmos, в портал Azure, параметр **все сети** в области **брандмауэр и виртуальные сети** недоступен.

В следующей таблице показано сопоставление между различными типами API учетной записи Azure Cosmos, поддерживаемыми подресурсами и соответствующими именами частных зон. Вы также можете получить доступ к учетным записям Gremlin и API таблиц через API SQL, поэтому для этих API-интерфейсов существует две записи.

|Тип API учетной записи Azure Cosmos  |Поддерживаемые подресурсы (или идентификаторы групп) |Имя частной зоны  |
|---------|---------|---------|
|SQL    |   SQL      | privatelink.documents.azure.com   |
|Cassandra    | Cassandra        |  privatelink.cassandra.cosmos.azure.com    |
|Mongo   |  MongoDB       |  privatelink.mongo.cosmos.azure.com    |
|Gremlin     | Gremlin        |  privatelink.gremlin.cosmos.azure.com   |
|Gremlin     |  SQL       |  privatelink.documents.azure.com    |
|Таблица    |    Таблица     |   privatelink.table.cosmos.azure.com    |
|Таблица     |   SQL      |  privatelink.documents.azure.com    |

### <a name="fetch-the-private-ip-addresses"></a>Получение частных IP-адресов

После подготовки частной конечной точки можно запросить IP-адреса. Чтобы просмотреть IP-адреса из портал Azure:

1. Щелкните **Все ресурсы**.
1. Найдите закрытую конечную точку, созданную ранее. В данном случае это **cdbPrivateEndpoint3**.
1. Перейдите на вкладку **Обзор** , чтобы просмотреть параметры DNS и IP-адреса.

![Частные IP-адреса в портал Azure](./media/how-to-configure-private-endpoints/private-ip-addresses-portal.png)

Для каждой частной конечной точки создается несколько IP-адресов:

* Один для глобальной (независимого от региона) конечной точки учетной записи Azure Cosmos.
* По одному для каждого региона, в котором развернута учетная запись Azure Cosmos.

## <a name="create-a-private-endpoint-by-using-azure-powershell"></a>Создание частной конечной точки с помощью Azure PowerShell

Выполните следующий сценарий PowerShell, чтобы создать частную конечную точку с именем "Миприватиндпоинт" для существующей учетной записи Azure Cosmos. Замените значения переменных на сведения о вашей среде.

```azurepowershell-interactive
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"

# API type of the Azure Cosmos account: Sql, MongoDB, Cassandra, Gremlin, or Table
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "MyPrivateEndpoint"
# Location where the private endpoint can be created. The private endpoint should be created in the same location where your subnet or the virtual network exists
$Location = "westcentralus"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnectionPS" -PrivateLinkServiceId $cosmosDbResourceId -GroupId $CosmosDbApiType
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $ResourceGroupName -Name $VNetName  
 
$subnet = $virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq $SubnetName}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $ResourceGroupName -Name $PrivateEndpointName -Location "westcentralus" -Subnet  $subnet -PrivateLinkServiceConnection $privateEndpointConnection
```

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>Интеграция частной конечной точки с частной зоной DNS

После создания закрытой конечной точки ее можно интегрировать с частной зоной DNS с помощью следующего сценария PowerShell:

```azurepowershell-interactive
Import-Module Az.PrivateDns
$zoneName = "privatelink.documents.azure.com"
$zone = New-AzPrivateDnsZone -ResourceGroupName $ResourceGroupName `
  -Name $zoneName

$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName $ResourceGroupName `
  -ZoneName $zoneName `
  -Name "myzonelink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$pe = Get-AzPrivateEndpoint -Name $PrivateEndpointName `
  -ResourceGroupName $ResourceGroupName

$networkInterface = Get-AzResource -ResourceId $pe.NetworkInterfaces[0].Id `
  -ApiVersion "2019-04-01"
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName `
  -RecordType A -ZoneName $zoneName  `
  -ResourceGroupName $ResourceGroupName -Ttl 600 `
  -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
  -IPv4Address $ipconfig.properties.privateIPAddress)  
}
}
```

### <a name="fetch-the-private-ip-addresses"></a>Получение частных IP-адресов

После подготовки частной конечной точки можно запросить IP-адреса и сопоставление полного доменного имени, используя следующий сценарий PowerShell:

```azurepowershell-interactive
$pe = Get-AzPrivateEndpoint -Name MyPrivateEndpoint -ResourceGroupName myResourceGroup
$networkInterface = Get-AzNetworkInterface -ResourceId $pe.NetworkInterfaces[0].Id
foreach ($IPConfiguration in $networkInterface.IpConfigurations)
{
    Write-Host $IPConfiguration.PrivateIpAddress ":" $IPConfiguration.PrivateLinkConnectionProperties.Fqdns
}
```

## <a name="create-a-private-endpoint-by-using-azure-cli"></a>Создание частной конечной точки с помощью Azure CLI

Выполните следующий сценарий Azure CLI, чтобы создать частную конечную точку с именем "Миприватиндпоинт" для существующей учетной записи Azure Cosmos. Замените значения переменных на сведения о вашей среде.

```azurecli-interactive
# Resource group where the Azure Cosmos account and virtual network resources are located
ResourceGroupName="myResourceGroup"

# Subscription ID where the Azure Cosmos account and virtual network resources are located
SubscriptionId="<your Azure subscription ID>"

# Name of the existing Azure Cosmos account
CosmosDbAccountName="mycosmosaccount"

# API type of your Azure Cosmos account: Sql, MongoDB, Cassandra, Gremlin, or Table
CosmosDbApiType="Sql"

# Name of the virtual network to create
VNetName="myVnet"

# Name of the subnet to create
SubnetName="mySubnet"

# Name of the private endpoint to create
PrivateEndpointName="myPrivateEndpoint"

# Name of the private endpoint connection to create
PrivateConnectionName="myConnection"

az network vnet create \
 --name $VNetName \
 --resource-group $ResourceGroupName \
 --subnet-name $SubnetName

az network vnet subnet update \
 --name $SubnetName \
 --resource-group $ResourceGroupName \
 --vnet-name $VNetName \
 --disable-private-endpoint-network-policies true

az network private-endpoint create \
    --name $PrivateEndpointName \
    --resource-group $ResourceGroupName \
    --vnet-name $VNetName  \
    --subnet $SubnetName \
    --private-connection-resource-id "/subscriptions/$SubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.DocumentDB/databaseAccounts/$CosmosDbAccountName" \
    --group-ids $CosmosDbApiType \
    --connection-name $PrivateConnectionName
```

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>Интеграция частной конечной точки с частной зоной DNS

После создания закрытой конечной точки ее можно интегрировать с частной зоной DNS с помощью следующего скрипта Azure CLI:

```azurecli-interactive
zoneName="privatelink.documents.azure.com"

az network private-dns zone create --resource-group $ResourceGroupName \
   --name  $zoneName

az network private-dns link vnet create --resource-group $ResourceGroupName \
   --zone-name  $zoneName\
   --name myzonelink \
   --virtual-network $VNetName \
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name $PrivateEndpointName --resource-group $ResourceGroupName --query 'networkInterfaces[0].id' -o tsv)
 
# Copy the content for privateIPAddress and FQDN matching the Azure Cosmos account 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
 
#Create DNS records 
az network private-dns record-set a create --name recordSet1 --zone-name privatelink.documents.azure.com --resource-group $ResourceGroupName
az network private-dns record-set a add-record --record-set-name recordSet2 --zone-name privatelink.documents.azure.com --resource-group $ResourceGroupName -a <Private IP Address>
```

## <a name="create-a-private-endpoint-by-using-a-resource-manager-template"></a>Создание частной конечной точки с помощью шаблона диспетчер ресурсов

Вы можете настроить частную связь, создав закрытую конечную точку в подсети виртуальной сети. Это достигается с помощью шаблона Azure Resource Manager.

Используйте следующий код, чтобы создать шаблон диспетчер ресурсов с именем "PrivateEndpoint_template. JSON". Этот шаблон создает закрытую конечную точку для существующей учетной записи API Azure Cosmos SQL в существующей виртуальной сети.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "[resourceGroup().location]",
          "metadata": {
            "description": "Location for all resources."
          }
        },
        "privateEndpointName": {
            "type": "string"
        },
        "resourceId": {
            "type": "string"
        },
        "groupId": {
            "type": "string"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('privateEndpointName')]",
            "type": "Microsoft.Network/privateEndpoints",
            "apiVersion": "2019-04-01",
            "location": "[parameters('location')]",
            "properties": {
                "subnet": {
                    "id": "[parameters('subnetId')]"
                },
                "privateLinkServiceConnections": [
                    {
                        "name": "MyConnection",
                        "properties": {
                            "privateLinkServiceId": "[parameters('resourceId')]",
                            "groupIds": [ "[parameters('groupId')]" ],
                            "requestMessage": ""
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {
        "privateEndpointNetworkInterface": {
          "type": "string",
          "value": "[reference(concat('Microsoft.Network/privateEndpoints/', parameters('privateEndpointName'))).networkInterfaces[0].id]"
        }
    }
}
```

**Определение файла параметров для шаблона**

Создайте файл параметров для шаблона и назовите его «PrivateEndpoint_parameters. JSON». Добавьте следующий код в файл параметров:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateEndpointName": {
            "value": ""
        },
        "resourceId": {
            "value": ""
        },
        "groupId": {
            "value": ""
        },
        "subnetId": {
            "value": ""
        }
    }
}
```

**Развертывание шаблона с помощью сценария PowerShell**

Создайте скрипт PowerShell с помощью следующего кода. Перед выполнением скрипта замените идентификатор подписки, имя группы ресурсов и другие значения переменных на сведения о вашей среде.

```azurepowershell-interactive
### This script creates a private endpoint for an existing Azure Cosmos account in an existing virtual network

## Step 1: Fill in these details. Replace the variable values with the details for your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Azure Cosmos account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Sign in to your Azure account and select the target subscription.
Login-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet.
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private endpoint.
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName

$deploymentOutput
```

В скрипте PowerShell `GroupId` переменная может содержать только одно значение. Это значение является типом API учетной записи. Допустимые значения: `Sql`, `MongoDB`, `Cassandra`, `Gremlin`и `Table`. Некоторые типы учетных записей Azure Cosmos доступны через несколько интерфейсов API. Пример:

* Доступ к учетной записи API Gremlin можно получить из учетных записей Gremlin и API SQL.
* Доступ к учетной записи API таблиц можно получить из учетных записей таблиц и API SQL.

Для этих учетных записей необходимо создать одну частную конечную точку для каждого типа API. Соответствующий тип API указан в `GroupId` массиве.

После успешного развертывания шаблона можно увидеть выходные данные, аналогичные показанным на следующем рисунке. `provisioningState` Значение равно `Succeeded` , если частные конечные точки настроены правильно.

![Выходные данные развертывания для шаблона диспетчер ресурсов](./media/how-to-configure-private-endpoints/resource-manager-template-deployment-output.png)

После развертывания шаблона частные IP-адреса зарезервированы в подсети. Правило брандмауэра учетной записи Azure Cosmos настроено для приема подключений только от частной конечной точки.

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>Интеграция частной конечной точки с зоной Частная зона DNS

Используйте следующий код, чтобы создать шаблон диспетчер ресурсов с именем "PrivateZone_template. JSON". Этот шаблон создает закрытую зону DNS для существующей учетной записи API Azure Cosmos SQL в существующей виртуальной сети.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateZoneName": {
            "type": "string"
        },
        "VNetId": {
            "type": "string"
        }        
    },
    "resources": [
        {
            "name": "[parameters('privateZoneName')]",
            "type": "Microsoft.Network/privateDnsZones",
            "apiVersion": "2018-09-01",
            "location": "global",
            "properties": {                
            }
        },
        {
            "type": "Microsoft.Network/privateDnsZones/virtualNetworkLinks",
            "apiVersion": "2018-09-01",
            "name": "[concat(parameters('privateZoneName'), '/myvnetlink')]",
            "location": "global",
            "dependsOn": [
                "[resourceId('Microsoft.Network/privateDnsZones', parameters('privateZoneName'))]"
            ],
            "properties": {
                "registrationEnabled": false,
                "virtualNetwork": {
                    "id": "[parameters('VNetId')]"
                }
            }
        }        
    ]
}
```

Используйте следующий код, чтобы создать шаблон диспетчер ресурсов с именем "PrivateZoneRecords_template. JSON".

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "DNSRecordName": {
            "type": "string"
        },
        "IPAddress": {
            "type":"string"
        }        
    },
    "resources": [
         {
            "type": "Microsoft.Network/privateDnsZones/A",
            "apiVersion": "2018-09-01",
            "name": "[parameters('DNSRecordName')]",
            "properties": {
                "ttl": 300,
                "aRecords": [
                    {
                        "ipv4Address": "[parameters('IPAddress')]"
                    }
                ]
            }
        }    
    ]
}
```

**Определение файла параметров для шаблона**

Создайте для шаблона следующий файл параметров. Создайте "PrivateZone_parameters. JSON". на новый код:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateZoneName": {
            "value": ""
        },
        "VNetId": {
            "value": ""
        }
    }
}
```

Создайте "PrivateZoneRecords_parameters. JSON". на новый код:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "DNSRecordName": {
            "value": ""
        },
        "IPAddress": {
            "type":"object"
        }
    }
}
```

**Развертывание шаблона с помощью сценария PowerShell**

Создайте скрипт PowerShell с помощью следующего кода. Перед выполнением скрипта замените идентификатор подписки, имя группы ресурсов и другие значения переменных на сведения о вашей среде.

```azurepowershell-interactive
### This script:
### - creates a private zone
### - creates a private endpoint for an existing Cosmos DB account in an existing VNet
### - maps the private endpoint to the private zone

## Step 1: Fill in these details. Replace the variable values with the details for your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Azure Cosmos account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private zone to create
$PrivateZoneName = "myPrivateZone.documents.azure.com"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateZoneTemplateFilePath = "PrivateZone_template.json"
$PrivateZoneParametersFilePath = "PrivateZone_parameters.json"
$PrivateZoneRecordsTemplateFilePath = "PrivateZoneRecords_template.json"
$PrivateZoneRecordsParametersFilePath = "PrivateZoneRecords_parameters.json"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Login your Azure account and select the target subscription
Login-AzAccount 
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private zone
New-AzResourceGroupDeployment -Name "PrivateZoneDeployment" `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $PrivateZoneTemplateFilePath `
    -TemplateParameterFile $PrivateZoneParametersFilePath `
    -PrivateZoneName $PrivateZoneName `
    -VNetId $VNetResourceId

## Step 5: Create the private endpoint
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName
$deploymentOutput

## Step 6: Map the private endpoint to the private zone
$networkInterface = Get-AzResource -ResourceId $deploymentOutput.Outputs.privateEndpointNetworkInterface.Value -ApiVersion "2019-04-01"
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) {
    foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) {
        $recordName = $fqdn.split('.',2)[0]
        $dnsZone = $fqdn.split('.',2)[1]
        Write-Output "Deploying PrivateEndpoint DNS Record $($PrivateZoneName)/$($recordName) Template on $($resourceGroupName)"
        New-AzResourceGroupDeployment -Name "PrivateEndpointDNSDeployment" `
            -ResourceGroupName $ResourceGroupName `
            -TemplateFile $PrivateZoneRecordsTemplateFilePath `
            -TemplateParameterFile $PrivateZoneRecordsParametersFilePath `
            -DNSRecordName "$($PrivateZoneName)/$($RecordName)" `
            -IPAddress $ipconfig.properties.privateIPAddress
    }
}
```

## <a name="configure-custom-dns"></a>Настройка пользовательской службы DNS

Следует использовать частную зону DNS в подсети, в которой была создана частная конечная точка. Настройте конечные точки таким образом, чтобы каждый частный IP-адрес был сопоставлен с записью DNS. (См. `fqdns` свойство в ответе, показанном выше.)

При создании частной конечной точки ее можно интегрировать с частной зоной DNS в Azure. Если вместо этого вы решили использовать пользовательскую зону DNS, необходимо настроить ее для добавления записей DNS для всех частных IP-адресов, зарезервированных для частной конечной точки.

## <a name="private-link-combined-with-firewall-rules"></a>Частная ссылка в сочетании с правилами брандмауэра

При использовании частной ссылки в сочетании с правилами брандмауэра возможны следующие ситуации и результаты.

* Если не настроить какие бы то ни было правила брандмауэра, по умолчанию весь трафик будет иметь доступ к учетной записи Azure Cosmos.

* Если вы настраиваете общедоступный трафик или конечную точку службы и создаете частные конечные точки, то соответствующий тип правила брандмауэра разрешается различными типами входящего трафика.

* Если вы не настраиваете общий трафик или конечную точку службы и не создаете частные конечные точки, учетная запись Azure Cosmos доступна только через частные конечные точки. Если не настроить общедоступный трафик или конечную точку службы, то после того, как все утвержденные частные конечные точки будут отклонены или удалены, учетная запись будет открыта для всей сети.

## <a name="blocking-public-network-access-during-account-creation"></a>Блокировка доступа к общедоступной сети во время создания учетной записи

Как описано в предыдущем разделе, и если не заданы конкретные правила брандмауэра, добавление частной конечной точки делает учетную запись Azure Cosmos доступной только для частных конечных точек. Это означает, что учетная запись Azure Cosmos может быть достигнута из общедоступного трафика после создания и перед добавлением закрытой конечной точки. Чтобы гарантировать, что доступ к общедоступной сети будет отключен даже перед созданием частных конечных точек, `publicNetworkAccess` можно установить `Disabled` флаг во время создания учетной записи. Пример использования этого флага см. в [этом шаблоне Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-cosmosdb-private-endpoint/) .

## <a name="update-a-private-endpoint-when-you-add-or-remove-a-region"></a>Обновление частной конечной точки при добавлении или удалении региона

Добавление или удаление регионов в учетной записи Azure Cosmos требует добавления или удаления записей DNS для этой учетной записи. После добавления или удаления регионов можно обновить частную зону DNS подсети, чтобы отразить добавленные или удаленные записи DNS и соответствующие им частные IP-адреса.

Например, предположим, что вы развертываете учетную запись Azure Cosmos в трех регионах: "Западная часть США", "Центральная часть США" и "Западная Европа". При создании частной конечной точки для вашей учетной записи в подсети зарезервированы четыре частных IP-адреса. Существует один IP-адрес для каждого из трех регионов, и существует один IP-адрес для конечной точки, не зависящей от глобальной или региона.

Позже вы можете добавить новый регион (например, "Восточная часть США") в учетную запись Azure Cosmos. После добавления нового региона необходимо добавить соответствующую запись DNS в вашу частную зону DNS или пользовательскую службу DNS.

Вы можете использовать те же действия при удалении региона. После удаления региона необходимо удалить соответствующую запись DNS из частной зоны DNS или из настраиваемой службы DNS.

## <a name="current-limitations"></a>Текущие ограничения

При использовании закрытой ссылки с учетной записью Azure Cosmos действуют следующие ограничения.

* Если вы используете закрытую связь с учетной записью Azure Cosmos с помощью подключения в прямом режиме, можно использовать только протокол TCP. Протокол HTTP пока не поддерживается.

* Частная конечная точка в настоящее время доступна только для режима подключения шлюза. В режиме прямого доступа он доступен в виде предварительной версии функции.

* Когда вы используете API Azure Cosmos DB для учетных записей MongoDB, Частная конечная точка поддерживается только для учетных записей на сервере версии 3,6 (то есть учетные записи, использующие конечную точку в формате `*.mongo.cosmos.azure.com`). Частная ссылка не поддерживается для учетных записей на сервере версии 3,2 (то есть учетные записи, использующие конечную `*.documents.azure.com`точку в формате). Чтобы использовать частную ссылку, необходимо перенести старые учетные записи в новую версию.

* Если вы используете API Azure Cosmos DB для учетных записей MongoDB с частной ссылкой, вы не можете использовать такие средства, как совместная работа 3T, Studio 3T и Mongoose. Конечная точка может иметь поддержку закрытых ссылок, `appName=<account name>` только если указан параметр. Например, `replicaSet=globaldb&appName=mydbaccountname`. Поскольку эти средства не передают имя приложения в строке подключения службе, нельзя использовать частную ссылку. Но вы по-прежнему можете получить доступ к этим учетным записям, используя драйверы пакета SDK с версией 3,6.

* Невозможно переместить или удалить виртуальную сеть, если она содержит закрытую ссылку.

* Вы не можете удалить учетную запись Azure Cosmos, если она подключена к частной конечной точке.

* Вы не можете выполнить отработку отказа учетной записи Azure Cosmos в регион, который не сопоставлен со всеми частными конечными точками, присоединенными к этой учетной записи.

* Чтобы создать автоматически утвержденные частные конечные точки, администратору сети необходимо предоставить по крайней мере разрешение "*/Приватиндпоинтконнектионсаппровал" в области учетной записи Azure Cosmos.

### <a name="limitations-to-private-dns-zone-integration"></a>Ограничения на интеграцию частной зоны DNS

Записи DNS в частной зоне DNS не удаляются автоматически при удалении частной конечной точки или при удалении региона из учетной записи Azure Cosmos. Необходимо вручную удалить записи DNS перед:

* Добавление новой частной конечной точки, связанной с этой частной зоной DNS.
* Добавление нового региона в любую учетную запись базы данных с закрытыми конечными точками, связанными с этой частной зоной DNS.

Если не очистить записи DNS, могут возникнуть непредвиденные проблемы с плоскостью данных. Эти проблемы включают сбой данных в регионах, добавленных после удаления частной конечной точки или удаления региона.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о Azure Cosmos DB средств безопасности см. в следующих статьях:

* Сведения о настройке брандмауэра для Azure Cosmos DB см. в разделе [Поддержка брандмауэра](firewall-support.md).

* Чтобы узнать, как настроить конечную точку службы виртуальной сети для учетной записи Azure Cosmos, см. раздел [Настройка доступа из виртуальных сетей](how-to-configure-vnet-service-endpoint.md).

* Дополнительные сведения о закрытой ссылке см. в документации по [частной ссылке Azure](../private-link/private-link-overview.md) .
