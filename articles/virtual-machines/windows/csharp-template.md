---
title: Развертывание виртуальной машины с помощью C# и шаблона диспетчер ресурсов
description: Узнайте, как использовать C# и шаблон Resource Manager для развертывания виртуальной машины Azure.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 07/14/2017
ms.author: cynthn
ms.openlocfilehash: dfcc0c550af9df6c884c8cd864ed90daf5f78e2f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82082923"
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Развертывание виртуальной машины Azure с помощью C# и шаблона Resource Manager

В этой статье описывается развертывание шаблона Azure Resource Manager с помощью C#. Созданный шаблон развертывает одну виртуальную машину под управлением Windows Server в новой виртуальной сети с одной подсетью.

Подробное описание ресурса виртуальной машины см. в статье [Виртуальные машины в шаблоне Azure Resource Manager](template-description.md). Дополнительные сведения обо всех ресурсах в шаблоне см. в статье [Пошаговое руководство по созданию шаблона Resource Manager](../../azure-resource-manager/resource-manager-template-walkthrough.md).

На выполнение этих действий требуется примерно 10 минут.

## <a name="create-a-visual-studio-project"></a>Создание проекта Visual Studio

На этом шаге нужно убедиться, что экземпляр Visual Studio установлен, и создать консольное приложение, используемое для развертывания шаблона.

1. Если вы этого еще не сделали, установите [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). На странице рабочих нагрузок выберите **Разработка классических приложений .NET** и нажмите кнопку **Установить**. В сводке вы увидите, что **средства разработки .NET Framework 4–4.6** выберутся автоматически. Если вы уже установили Visual Studio, можно добавить рабочую нагрузку .NET с помощью средства запуска Visual Studio.
2. В Visual Studio щелкните **файл** > **создать** > **проект**.
3. В меню **шаблоны** > **Visual C#** выберите **консольное приложение (.NET Framework)**, введите *мидотнетпрожект* в поле имя проекта, выберите расположение проекта и нажмите кнопку **ОК**.

## <a name="install-the-packages"></a>Установка пакетов

Самый простой способ установить библиотеки, необходимые для выполнения этих инструкций, — это использовать пакеты NuGet. Чтобы получить эти библиотеки в Visual Studio, выполните следующие действия.

1. Щелкните **инструменты** > **Диспетчер пакетов NuGet**, а затем — **консоль диспетчера пакетов**.
2. В консоли введите следующие команды:

    ```powershell
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-the-files"></a>Создание файлов

На этом шаге вы создадите файл шаблона, который развертывает ресурсы, и файл параметров, который предоставляет значения для параметров шаблона. Кроме того, вы создадите файл авторизации для выполнения операций Azure Resource Manager.

### <a name="create-the-template-file"></a>Создание файла шаблона

1. В Обозреватель решений щелкните правой кнопкой мыши *мидотнетпрожект* > **добавить** > **новый элемент**, а затем выберите **текстовый файл** в *элементах Visual C#*. Назовите файл *CreateVMTemplate.json*, а затем щелкните **Добавить**.
2. Добавьте следующий код JSON в созданный файл:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

3. Сохраните файл CreateVMTemplate.json.

### <a name="create-the-parameters-file"></a>Создание файла параметров

Чтобы указать значения для параметров ресурсов в шаблоне, необходимо создать файл параметров, содержащий значения.

1. В Обозреватель решений щелкните правой кнопкой мыши *мидотнетпрожект* > **добавить** > **новый элемент**, а затем выберите **текстовый файл** в *элементах Visual C#*. Назовите файл *Parameters.json*, а затем щелкните **Добавить**.
2. Добавьте следующий код JSON в созданный файл:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

4. Сохраните файл Parameters.json.

### <a name="create-the-authorization-file"></a>Создание файла авторизации

Перед развертыванием шаблона убедитесь в наличии доступа к [субъекту-службе Active Directory](../../active-directory/develop/howto-authenticate-service-principal-powershell.md). Субъект-служба предоставляет маркер для аутентификации запросов к Azure Resource Manager. Кроме того, необходимо записать идентификатор приложения, ключ проверки подлинности и идентификатор клиента, которые понадобятся в файле авторизации.

1. В Обозреватель решений щелкните правой кнопкой мыши *мидотнетпрожект* > **добавить** > **новый элемент**, а затем выберите **текстовый файл** в *элементах Visual C#*. Назовите файл *azureauth.properties* и щелкните **Добавить**.
2. Добавьте следующие свойства авторизации:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.microsoft.com/
    ```

    Замените ** &lt;Subscription-ID&gt; ** идентификатором подписки, ** &lt;Application-ID&gt; ** — идентификатором приложения Active Directory, ** &lt;ключом&gt; проверки подлинности** с ключом приложения, а ** &lt;идентификатором&gt; клиента** — идентификатором клиента.

3. Сохраните файл azureauth.properties.
4. Задайте переменную среды в Windows с именем AZURE_AUTH_LOCATION, указав полный путь к созданному файлу авторизации. Например, можно использовать следующую команду PowerShell:

    ```powershell
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2019\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

    

## <a name="create-the-management-client"></a>Создание клиента управления

1. Откройте файл Program.cs для созданного проекта. Затем добавьте эти операторы using в существующие инструкции в начале файла:

    ```csharp
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. Чтобы создать клиент управления, добавьте следующий код в метод Main:

    ```csharp
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Чтобы задать значения для приложения, добавьте код в метод Main:

```csharp
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>Создание учетной записи хранения

Шаблон и параметры развертываются из учетной записи хранения в Azure. На этом шаге создается учетная запись и отправляются файлы. 

Чтобы создать учетную запись, добавьте следующий код в метод Main:

```csharp
string storageAccountName = SdkContext.RandomResourceName("st", 10);

Console.WriteLine("Creating storage account...");
var storage = azure.StorageAccounts.Define(storageAccountName)
    .WithRegion(Region.USWest)
    .WithExistingResourceGroup(resourceGroup)
    .Create();

var storageKeys = storage.GetKeys();
string storageConnectionString = "DefaultEndpointsProtocol=https;"
    + "AccountName=" + storage.Name
    + ";AccountKey=" + storageKeys[0].Value
    + ";EndpointSuffix=core.windows.net";

var account = CloudStorageAccount.Parse(storageConnectionString);
var serviceClient = account.CreateCloudBlobClient();

Console.WriteLine("Creating container...");
var container = serviceClient.GetContainerReference("templates");
container.CreateIfNotExistsAsync().Wait();
var containerPermissions = new BlobContainerPermissions()
    { PublicAccess = BlobContainerPublicAccessType.Container };
container.SetPermissionsAsync(containerPermissions).Wait();

Console.WriteLine("Uploading template file...");
var templateblob = container.GetBlockBlobReference("CreateVMTemplate.json");
templateblob.UploadFromFileAsync("..\\..\\CreateVMTemplate.json").Result();

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFileAsync("..\\..\\Parameters.json").Result();
```

## <a name="deploy-the-template"></a>Развертывание шаблона

Разверните шаблон и параметры из созданной учетной записи хранения. 

Для развертывания шаблона добавьте следующий код в метод Main.

```csharp
var templatePath = "https://" + storageAccountName + ".blob.core.windows.net/templates/CreateVMTemplate.json";
var paramPath = "https://" + storageAccountName + ".blob.core.windows.net/templates/Parameters.json";
var deployment = azure.Deployments.Define("myDeployment")
    .WithExistingResourceGroup(groupName)
    .WithTemplateLink(templatePath, "1.0.0.0")
    .WithParametersLink(paramPath, "1.0.0.0")
    .WithMode(Microsoft.Azure.Management.ResourceManager.Fluent.Models.DeploymentMode.Incremental)
    .Create();
Console.WriteLine("Press enter to delete the resource group...");
Console.ReadLine();
```

## <a name="delete-the-resources"></a>Удаление ресурсов

Так как за использование ресурсов Azure взимается плата, рекомендуется всегда удалять ресурсы, которые больше не нужны. Не нужно отдельно удалять каждый ресурс из группы ресурсов. Удалите группу ресурсов, и все ее ресурсы будут автоматически удалены. 

Чтобы удалить группу ресурсов, добавьте следующий код в метод Main:

```csharp
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>Выполнение приложения

На полное выполнение этого консольного приложения потребуется примерно 5 минут. 

1. Чтобы запустить консольное приложение, щелкните **Запустить**.

2. Прежде чем нажать клавишу **ВВОД** и начать удаление ресурсов, потратьте несколько минут и проверьте на портале Azure, созданы ли эти ресурсы. Щелкните состояние развертывания, чтобы просмотреть сведения о развертывании.

## <a name="next-steps"></a>Дальнейшие шаги

* Если возникли проблемы с развертыванием, следующим шагом будет рассмотреть [Устранение распространенных ошибок развертывания Azure с Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
* Узнайте, как развернуть виртуальную машину и ее вспомогательные ресурсы, ознакомившись с разделом [Развертывание ресурсов Azure с помощью языка C#](csharp.md).