---
title: Создание частной конечной точки Azure с помощью Azure PowerShell | Документация Майкрософт
description: Сведения о частной ссылке Azure
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 8af33e95c92cf51bdabe3325bd9249b4662b7d28
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583768"
---
# <a name="create-a-private-endpoint-using-azure-powershell"></a>Создание частной конечной точки с помощью Azure PowerShell
Частная конечная точка — ключевой компонент для построения частной ссылки в Azure. Это позволяет ресурсам Azure, таким как виртуальные машины (VM), обмениваться данными в частном порядке с ресурсами частной ссылки. 

В этом кратком руководстве вы узнаете, как создать виртуальную машину в виртуальной сети Azure и сервер базы данных SQL с частной конечной точкой Azure, используя Azure PowerShell. Затем вы сможете безопасно получить доступ к серверу базы данных SQL с виртуальной машины.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Перед созданием ресурсов необходимо создать группу ресурсов, в которой размещается виртуальная сеть и частная конечная точка, с помощью [New-азресаурцеграуп](/powershell/module/az.resources/new-azresourcegroup). В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *WestUS* :

```azurepowershell

New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus
```

## <a name="create-a-virtual-network"></a>Создайте виртуальную сеть
В этом разделе вы создадите виртуальную сеть и подсеть. Затем подсеть связывается с виртуальной сетью.

### <a name="create-a-virtual-network"></a>Создайте виртуальную сеть

Создайте виртуальную сеть для частной конечной точки с помощью [New-азвиртуалнетворк](/powershell/module/az.network/new-azvirtualnetwork). В следующем примере создается виртуальная сеть с именем *MyVirtualNetwork*:
 
```azurepowershell

$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Добавление подсети

Azure развертывает ресурсы в подсеть в виртуальной сети, поэтому необходимо создать подсеть. Создайте конфигурацию подсети с именем *mySubnet* с помощью [Add-азвиртуалнетворксубнетконфиг](/powershell/module/az.network/add-azvirtualnetworksubnetconfig). В следующем примере создается подсеть с именем *mySubnet* с флагом политики сети частной конечной точки, установленным в значение **Disabled (отключено**).

```azurepowershell
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 10.0.0.0/24 `
  -PrivateEndpointNetworkPoliciesFlag "Disabled" `
  -VirtualNetwork $virtualNetwork
```

> [!CAUTION]
> `PrivateEndpointNetworkPoliciesFlag` Параметр можно легко запутать с другим доступным флагом, `PrivateLinkServiceNetworkPoliciesFlag`так как оба слова являются длинными словами и имеют одинаковый внешний вид.  Убедитесь, что используется правильный, `PrivateEndpointNetworkPoliciesFlag`.

### <a name="associate-the-subnet-to-the-virtual-network"></a>Связывание подсети с виртуальной сетью

Вы можете записать конфигурацию подсети в виртуальную сеть с помощью [Set-азвиртуалнетворк](/powershell/module/az.network/Set-azVirtualNetwork). Эта команда создает подсеть:

```azurepowershell
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-a-virtual-machine"></a>Создание виртуальной машины

Создайте виртуальную машину в виртуальной сети с помощью [New-AzVM](/powershell/module/az.compute/new-azvm). При запуске приведенной ниже команды запрашиваются учетные данные. Введите имя пользователя и пароль виртуальной машины:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "westcentralus" `
    -VirtualNetworkName "MyVirtualNetwork" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389 `
    -AsJob  
```

Параметр `-AsJob` создает виртуальную машину в фоновом режиме. Перейдите к следующему шагу.

Когда Azure начнет создание виртуальной машины в фоновом режиме, вы получите результат, аналогичный следующему:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

## <a name="create-a-sql-database-server"></a>Создание сервера базы данных SQL 

Создайте сервер базы данных SQL с помощью команды New-Азсклсервер. Помните, что имя сервера базы данных SQL должно быть уникальным в пределах Azure, поэтому замените значение заполнителя в квадратных скобках своим уникальным значением:

```azurepowershell-interactive
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"

$server = New-AzSqlServer -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver" `
    -Location "WestCentralUS" `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

New-AzSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver"`
    -DatabaseName "myda"`
    -RequestedServiceObjectiveName "S0" `
    -SampleName "AdventureWorksLT"
```

## <a name="create-a-private-endpoint"></a>Создание частной конечной точки

Частная конечная точка для сервера базы данных SQL в виртуальной сети с помощью [New-азпривателинксервицеконнектион](/powershell/module/az.network/New-AzPrivateLinkServiceConnection): 

```azurepowershell

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnection" `
  -PrivateLinkServiceId $server.ResourceId `
  -GroupId "sqlServer" 
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  "myResourceGroup" -Name "MyVirtualNetwork"  
 
$subnet = $virtualNetwork `
  | Select -ExpandProperty subnets `
  | Where-Object  {$_.Name -eq 'mysubnet'}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName "myResourceGroup" `
  -Name "myPrivateEndpoint" `
  -Location "westcentralus" `
  -Subnet  $subnet `
  -PrivateLinkServiceConnection $privateEndpointConnection
``` 

## <a name="configure-the-private-dns-zone"></a>Настройка частной зоны DNS 
Создайте частную зону DNS для домена сервера базы данных SQL и создайте связь связи с виртуальной сетью: 

```azurepowershell

$zone = New-AzPrivateDnsZone -ResourceGroupName "myResourceGroup" `
  -Name "privatelink.database.windows.net" 
 
$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName "myResourceGroup" `
  -ZoneName "privatelink.database.windows.net"`
  -Name "mylink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$networkInterface = Get-AzResource -ResourceId $privateEndpoint.NetworkInterfaces[0].Id -ApiVersion "2019-04-01" 
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName -RecordType A -ZoneName "privatelink.database.windows.net"  `
-ResourceGroupName "myResourceGroup" -Ttl 600 `
-PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address $ipconfig.properties.privateIPAddress)  
} 
} 
``` 
  
## <a name="connect-to-a-vm-from-the-internet"></a>Подключение к виртуальной машине из Интернета

Чтобы получить общедоступный IP-адрес виртуальной машины, выполните командлет [Get-AzPublicIpAddress](/powershell/module/az.network/Get-AzPublicIpAddress). В этом примере возвращается общедоступный IP-адрес виртуальной машины *myVM* :

```azurepowershell
Get-AzPublicIpAddress `
  -Name myPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress 
```  
Откройте командную строку на локальном компьютере. Выполните команду mstsc. Замените <publicIpAddress> общедоступным IP-адресом, полученным на последнем шаге: 


> [!NOTE]
> Если вы выполняли эти команды в командной строке PowerShell на локальном компьютере и используете модуль Az PowerShell 1.0 или более поздней версии, можете продолжить работу в этом интерфейсе.
```
mstsc /v:<publicIpAddress>
```

1. При появлении запроса выберите **Подключиться**. 
2. Введите имя пользователя и пароль, указанные при создании виртуальной машины.
  > [!NOTE]
  > Вам может потребоваться выбрать дополнительные варианты > использовать другую учетную запись, чтобы указать учетные данные, введенные при создании виртуальной машины. 
  
3. Щелкните **ОК**. 
4. Вы можете получить предупреждение о сертификате. В таком случае выберите **Да** или **Продолжить**. 

## <a name="access-sql-database-server-privately-from-the-vm"></a>Доступ с виртуальной машины к серверу базы данных SQL в частном порядке

1. Откройте PowerShell на удаленном рабочем столе myVm.
2. Введите `nslookup myserver.database.windows.net`. Не забудьте `myserver` заменить на имя SQL Server.

    Должно появиться сообщение следующего вида:
    
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
    ```
    
3. Установите SQL Server Management Studio.
4. В окне **Подключение к серверу** введите или выберите следующую информацию:

    | Параметр | Значение |
    | --- | --- |
    | Тип сервера | Компонент Database Engine |
    | Имя сервера | myserver.database.windows.net |
    | Имя пользователя | Введите имя пользователя, указанное при создании |
    | Пароль | Введите пароль, указанный при создании |
    | Запомнить пароль | Да |
    
5. Выберите **Подключиться**.
6. Просмотрите **базы данных** в меню слева. 
7. (Дополнительно) Создание или запрос информации из базы данных mydatabase.
8. Закройте подключение к удаленному рабочему столу *myVM*. 

## <a name="clean-up-resources"></a>Очистка ресурсов 
Завершив использование частной конечной точки, сервера базы данных SQL и виртуальной машины, удалите группу ресурсов и все ресурсы, используя команду [Remove-азресаурцеграуп](/powershell/module/az.resources/remove-azresourcegroup) .

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения о службе [Приватный канал Azure](private-link-overview.md)
