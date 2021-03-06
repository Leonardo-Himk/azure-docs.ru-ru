---
title: Включение доменных служб Azure AD с помощью PowerShell | Документация Майкрософт
description: Узнайте, как настраивать и включать доменные службы Azure Active Directory с помощью Azure AD PowerShell и Azure PowerShell.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: d4bc5583-6537-4cd9-bc4b-7712fdd9272a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: sample
ms.date: 09/05/2019
ms.author: iainfou
ms.openlocfilehash: e99ad2d53bc26b4e13a34097baaec929058a61a0
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654805"
---
# <a name="enable-azure-active-directory-domain-services-using-powershell"></a>Включение доменных служб Azure Active Directory с помощью PowerShell

Доменные службы Azure Active Directory (Azure AD DS) предоставляют управляемые доменные службы, отвечающие за присоединение к домену, применение групповой политики, использование протокола LDAP, а также выполнение аутентификации Kerberos или NTLM (полностью поддерживается Windows Server Active Directory). Эти доменные службы можно использовать без необходимости развертывать, администрировать и обновлять контроллеры домена. Azure AD DS интегрируется с существующим клиентом Azure AD. Такая интеграция позволяет пользователям входить в систему с корпоративными учетными данными. При этом вы можете использовать существующие группы и учетные записи пользователей для защиты доступа к ресурсам.

В этой статье описано, как включить Azure AD DS с помощью PowerShell.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Предварительные требования

Для работы с этой статьей необходимо следующее:

* Установите и настройте Azure PowerShell.
    * При необходимости выполните инструкции по [установке модуля Azure PowerShell и его подключению к подписке Azure](/powershell/azure/install-az-ps).
    * Войдите в подписку Azure с помощью командлета [Connect-AzAccount][Connect-AzAccount].
* Установите и настройте Azure AD PowerShell.
    * При необходимости выполните инструкции по [установке модуля Azure AD PowerShell и подключению к Azure AD](/powershell/azure/active-directory/install-adv2).
    * Войдите в клиент Azure AD с помощью командлета [Connect-AzureAD][Connect-AzureAD].
* Привилегии *глобального администратора* для клиента Azure AD, чтобы включить доменные службы Azure AD.
* Для создания нужных ресурсов Azure AD DS требуются привилегии *участника* в подписке Azure.

## <a name="create-required-azure-ad-resources"></a>Создание необходимых ресурсов Azure AD

Для доменных служб Azure Active Directory требуется субъект-служба и группа Azure AD. Эти ресурсы позволяют управляемому домену доменной службы Azure Active Directory синхронизировать данные и определять, каким пользователям предоставлять административные разрешения в управляемом домене.

Сначала создайте субъект-службу Azure AD, чтобы обеспечить для Azure AD DS возможность взаимодействия и проверки подлинности. Используется определенный идентификатор приложения с названием *Domain Controller Services*(Службы контроллера домена) и номером идентификатора *2565bd9d-da50-47d4-8b85-4c97f669dc36*. Не изменяйте этот идентификатор приложения.

Создайте субъект-службу Azure AD с помощью командлета [New-AzureADServicePrincipal][New-AzureADServicePrincipal]:

```powershell
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

Создайте группу Azure AD с именем *Администраторы контроллера домена AAD*. Пользователи, добавленные в эту группу, получают разрешения на выполнение задач администрирования в управляемом домене доменных служб Azure AD.

Создайте группу *Администраторы контроллера домена AAD* с помощью командлета [New-AzureADGroup][New-AzureADGroup]:

```powershell
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

После создания группы *Администраторы контроллера домена AAD* добавьте в группу пользователя с помощью командлета [Add-AzureADGroupMember][Add-AzureADGroupMember]. Сначала получите идентификатор объекта группы *Администраторы контроллера домена AAD* с помощью командлета [Get-AzureADGroup][Get-AzureADGroup], затем получите идентификатор объекта нужного пользователя с помощью командлета [Get-AzureADUser][Get-AzureADUser].

В следующем примере это идентификатор объекта пользователя для учетной записи с именем участника-пользователя `admin@aaddscontoso.onmicrosoft.com`. Используйте вместо этой учетной записи пользователя имя участника-пользователя, которого нужно добавить в группу *Администраторы контроллера домена AAD*

```powershell
# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq 'admin@aaddscontoso.onmicrosoft.com'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId
```

## <a name="create-supporting-azure-resources"></a>Создание вспомогательных ресурсов Azure

Сначала зарегистрируйте поставщика ресурсов доменных служб Azure AD с помощью командлета [Register-AzResourceProvider][Register-AzResourceProvider]:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD
```

Затем создайте группу ресурсов с помощью командлета [New-AzResourceGroup][New-AzResourceGroup]. В следующем примере создается группа ресурсов с именем *myResourceGroup* в регионе *westus*. Используйте желаемое имя и регион:

```powershell
$ResourceGroupName = "myResourceGroup"
$AzureLocation = "westus"

# Create the resource group.
New-AzResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation
```

Создайте виртуальную сеть и подсети для доменных служб Azure AD. Создаются две подсети — для доменных служб (*DomainServices*) и для рабочих нагрузок (*Workloads*). Службы Azure AD DS развертываются в выделенной подсети *DomainServices*. Не развертывайте другие приложения или рабочие нагрузки в этой подсети. Используйте отдельные подсети *Workloads* или другие подсети для остальных виртуальных машин.

Создайте подсети с помощью командлета [New-AzVirtualNetworkSubnetConfig][New-AzVirtualNetworkSubnetConfig], а затем создайте виртуальную сеть с помощью командлета [New-AzVirtualNetwork][New-AzVirtualNetwork].

```powershell
$VnetName = "myVnet"

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet= New-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location westus `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet
```

## <a name="create-an-azure-ad-ds-managed-domain"></a>Создание управляемого домена Azure AD DS

Теперь давайте создадим управляемый домен Azure AD DS. Задайте идентификатор подписки Azure, а затем укажите имя управляемого домена, например *aaddscontoso.com*. Вы можете получить идентификатор подписки с помощью командлета [Get-AzSubscription][Get-AzSubscription].

Если вы выбрали регион, который поддерживает зоны доступности, ресурсы Azure AD DS распределяются между зонами для дополнительной избыточности.

Зоны доступности — уникальные физические расположения в пределах одного региона Azure. Каждая зона состоит из одного или нескольких центров обработки данных, оснащенных независимыми системами электроснабжения, охлаждения и сетевого взаимодействия. Чтобы обеспечить устойчивость, во всех включенных областях используются минимум три отдельные зоны.

Вы не можете настроить распределение Azure AD DS между зонами. Платформа Azure автоматически обрабатывает распределение ресурсов зоны. См. [дополнительные сведения о зонах доступности и регионах][availability-zones].

```powershell
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "aaddscontoso.com"

# Enable Azure AD Domain Services for the directory.
New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -Force -Verbose
```

Создание ресурса и возврат управления в командную строку PowerShell занимает несколько минут. Подготовка к работе управляемого домена доменных служб Azure AD продолжается в фоновом режиме. На завершение развертывания может понадобиться до часа. На портале Azure на странице **Обзор** вашего управляемого домена Azure AD DS отображаются сведения о текущем состоянии на этом этапе развертывания.

Когда на портале Azure отобразятся сведения о том, что подготовка к работе управляемого домена Azure AD DS завершена, выполните следующие задачи:

* Обновите параметры DNS для виртуальной сети, чтобы виртуальные машины могли найти управляемый домен для присоединения к нему или для аутентификации.
    * Чтобы настроить DNS, выберите на портале управляемый домен Azure AD DS. В окне **Обзор** отобразится запрос на автоматическую настройку этих параметров DNS.
* Если вы создали управляемый домен Azure AD DS в регионе, который поддерживает зоны доступности, создайте группу безопасности сети. Это позволит ограничить трафик в виртуальной сети для управляемого домена Azure AD DS. Будет создана стандартная подсистема балансировки нагрузки Azure, для работы которой должны действовать эти правила. Эта группа безопасности сети защищает Azure AD DS и требуется для правильной работы управляемого домена.
    * Чтобы создать группу безопасности сети и необходимые правила, выберите на портале управляемый домен Azure AD DS. В окне **Обзор** отобразится запрос на автоматическое создание и настройку группы безопасности сети.
* [Включите синхронизацию паролей с доменными службами Azure AD](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds), чтобы пользователи могли входить в управляемый домен с рабочими учетными данными.

## <a name="complete-powershell-script"></a>Полный скрипт PowerShell

Приведенный ниже полный скрипт PowerShell объединяет все задачи, описанные в этой статье. Скопируйте его и сохраните в текстовый файл с расширением `.ps1`. Запустите скрипт в локальной консоли PowerShell или [Azure Cloud Shell][cloud-shell].

> [!NOTE]
> Чтобы включить Azure AD DS, нужно быть глобальным администратором для клиента Azure AD. Кроме того, понадобятся разрешения не ниже уровня *Участник* в подписке Azure.

```powershell
# Change the following values to match your deployment.
$AaddsAdminUserUpn = "admin@aaddscontoso.onmicrosoft.com"
$ResourceGroupName = "myResourceGroup"
$VnetName = "myVnet"
$AzureLocation = "westus"
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "aaddscontoso.com"

# Connect to your Azure AD directory.
Connect-AzureAD

# Login to your Azure subscription.
Connect-AzAccount

# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"

# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"

# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq '$AaddsAdminUserUpn'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId

# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD

# Create the resource group.
New-AzResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $AzureLocation `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet

# Enable Azure AD Domain Services for the directory.
New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -Force -Verbose
```

Создание ресурса и возврат управления в командную строку PowerShell занимает несколько минут. Подготовка к работе управляемого домена доменных служб Azure AD продолжается в фоновом режиме. На завершение развертывания может понадобиться до часа. На портале Azure на странице **Обзор** вашего управляемого домена Azure AD DS отображаются сведения о текущем состоянии на этом этапе развертывания.

Когда на портале Azure отобразятся сведения о том, что подготовка к работе управляемого домена Azure AD DS завершена, выполните следующие задачи:

* Обновите параметры DNS для виртуальной сети, чтобы виртуальные машины могли найти управляемый домен для присоединения к нему или для аутентификации.
    * Чтобы настроить DNS, выберите на портале управляемый домен Azure AD DS. В окне **Обзор** отобразится запрос на автоматическую настройку этих параметров DNS.
* Если вы создали управляемый домен Azure AD DS в регионе, который поддерживает зоны доступности, создайте группу безопасности сети, чтобы ограничить трафик в виртуальной сети для управляемого домена Azure AD DS. Будет создана стандартная подсистема балансировки нагрузки Azure, для работы которой должны действовать эти правила. Эта группа безопасности сети защищает Azure AD DS и требуется для правильной работы управляемого домена.
    * Чтобы создать группу безопасности сети и необходимые правила, выберите на портале управляемый домен Azure AD DS. В окне **Обзор** отобразится запрос на автоматическое создание и настройку группы безопасности сети.
* [Включите синхронизацию паролей с доменными службами Azure AD](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds), чтобы пользователи могли входить в управляемый домен с рабочими учетными данными.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы оценить управляемый домен Azure AD DS в действии, [присоедините к нему виртуальную машину Windows][windows-join], а также настройте [защищенный протокол LDAP][tutorial-ldaps] и [синхронизацию хэша паролей][tutorial-phs].

<!-- INTERNAL LINKS -->
[windows-join]: join-windows-vm.md
[tutorial-ldaps]: tutorial-configure-ldaps.md
[tutorial-phs]: tutorial-configure-password-hash-sync.md

<!-- EXTERNAL LINKS -->
[Connect-AzAccount]: /powershell/module/Az.Accounts/Connect-AzAccount
[Connect-AzureAD]: /powershell/module/AzureAD/Connect-AzureAD
[New-AzureADServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
[New-AzureADGroup]: /powershell/module/AzureAD/New-AzureADGroup
[Add-AzureADGroupMember]: /powershell/module/AzureAD/Add-AzureADGroupMember
[Get-AzureADGroup]: /powershell/module/AzureAD/Get-AzureADGroup
[Get-AzureADUser]: /powershell/module/AzureAD/Get-AzureADUser
[Register-AzResourceProvider]: /powershell/module/Az.Resources/Register-AzResourceProvider
[New-AzResourceGroup]: /powershell/module/Az.Resources/New-AzResourceGroup
[New-AzVirtualNetworkSubnetConfig]: /powershell/module/Az.Network/New-AzVirtualNetworkSubnetConfig
[New-AzVirtualNetwork]: /powershell/module/Az.Network/New-AzVirtualNetwork
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
[cloud-shell]: /azure/cloud-shell/cloud-shell-windows-users
[availability-zones]: ../availability-zones/az-overview.md
