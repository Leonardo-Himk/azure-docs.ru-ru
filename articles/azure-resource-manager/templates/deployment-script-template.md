---
title: Использование скриптов развертывания в шаблонах | Документация Майкрософт
description: Используйте скрипты развертывания в шаблонах Azure Resource Manager.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: jgao
ms.openlocfilehash: 5b938e2072daec56261e529ab8a2a8b15b55d143
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872341"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>Использование скриптов развертывания в шаблонах (Предварительная версия)

Узнайте, как использовать скрипты развертывания в шаблонах ресурсов Azure. С помощью нового типа `Microsoft.Resources/deploymentScripts`ресурсов пользователи могут выполнять скрипты развертывания в развертываниях шаблонов и просматривать результаты выполнения. Эти скрипты можно использовать для выполнения пользовательских действий, таких как:

- Добавление пользователей в каталог
- выполнение операций с плоскостью данных, например копирование больших двоичных объектов или базы данных начального значения
- Поиск и проверка лицензионного ключа
- создать самозаверяющий сертификат;
- Создание объекта в Azure AD
- Поиск блоков IP-адресов из пользовательской системы

Преимущества скрипта развертывания:

- Простая в кодировании, использовании и отладка. Сценарии развертывания можно разрабатывать в избранных средах разработки. Скрипты можно внедрять в шаблоны или во внешние файлы скриптов.
- Можно указать язык скрипта и платформу. В настоящее время поддерживаются сценарии развертывания Azure PowerShell и Azure CLI в среде Linux.
- Разрешить указание удостоверений, используемых для выполнения скриптов. В настоящее время поддерживается только [управляемое пользователем удостоверение Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) .
- Разрешает передавать скрипту аргументы командной строки.
- Может указывать выходные данные скрипта и передавать их обратно в развертывание.

Ресурс скрипта развертывания доступен только в регионах, где доступен экземпляр контейнера Azure.  См. статью [доступность ресурсов для экземпляров контейнеров Azure в регионах Azure](../../container-instances/container-instances-region-availability.md).

> [!IMPORTANT]
> Учетная запись хранения и экземпляр контейнера необходимы для выполнения скриптов и устранения неполадок. Вы можете указать существующую учетную запись хранения, в противном случае учетная запись хранения вместе с экземпляром контейнера автоматически создается службой сценариев. Два автоматически создаваемых ресурса обычно удаляются службой сценариев, когда выполнение скрипта развертывания получает состояние терминала. Плата взимается за ресурсы, пока они не будут удалены. Дополнительные сведения см. в разделе [Очистка ресурсов скрипта развертывания](#clean-up-deployment-script-resources).

## <a name="prerequisites"></a>Предварительные условия

- **Назначаемое пользователем управляемое удостоверение с ролью участника в целевую группу ресурсов**. Это удостоверение используется для выполнения скриптов развертывания. Для выполнения операций вне группы ресурсов необходимо предоставить дополнительные разрешения. Например, назначьте удостоверению уровень подписки, если хотите создать новую группу ресурсов.

  > [!NOTE]
  > Служба скриптов создает учетную запись хранения (если не указана существующая учетная запись хранения) и экземпляр контейнера в фоновом режиме.  Назначаемое пользователем управляемое удостоверение с ролью участника на уровне подписки является обязательным, если подписка не зарегистрировала учетную запись хранения Azure (Microsoft. Storage) и поставщики ресурсов экземпляра контейнера Azure (Microsoft. Контаинеринстанце).

  Сведения о создании удостоверения см. в разделе [Создание назначаемого пользователем управляемого удостоверения с помощью портал Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)или с [помощью Azure CLI](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)или с [помощью Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md). Идентификатор удостоверения необходим при развертывании шаблона. Требуемый формат удостоверения:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Используйте следующий сценарий CLI или PowerShell, чтобы получить идентификатор, указав имя группы ресурсов и имя удостоверения.

  # <a name="cli"></a>[CLI](#tab/CLI)

  ```azurecli-interactive
  echo "Enter the Resource Group name:" &&
  read resourceGroupName &&
  echo "Enter the managed identity name:" &&
  read idName &&
  az identity show -g jgaoidentity1008rg -n jgaouami --query id
  ```

  # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name $idName).Id
  ```

  ---

- **Azure PowerShell** или **Azure CLI**. Список поддерживаемых версий Azure PowerShell см. [здесь](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list). Список поддерживаемых версий Azure CLI см. [здесь](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list).

    >[!IMPORTANT]
    > Скрипт развертывания использует доступные образы CLI из реестра контейнеров Майкрософт (мкр). Сертификация образа CLI для сценария развертывания занимает около одного месяца. Не используйте версии CLI, выпущенные в течение 30 дней. Сведения о датах выпуска для образов см. в разделе [Azure CLI заметки о выпуске](https://docs.microsoft.com/cli/azure/release-notes-azure-cli?view=azure-cli-latest). Если используется неподдерживаемая версия, в сообщении об ошибке выводится список поддерживаемых версий.

    Эти версии не требуются для развертывания шаблонов. Но эти версии необходимы для локального тестирования сценариев развертывания. См. статью [Установка модуля Azure PowerShell](/powershell/azure/install-az-ps). Вы можете использовать предварительно настроенный образ DOCKER.  См. раздел [Настройка среды разработки](#configure-development-environment).

## <a name="sample-templates"></a>Примеры шаблонов

Ниже приведен пример JSON.  Последнюю схему шаблона можно найти [здесь](/azure/templates/microsoft.resources/deploymentscripts).

```json
{
  "type": "Microsoft.Resources/deploymentScripts",
  "apiVersion": "2019-10-01-preview",
  "name": "runPowerShellInline",
  "location": "[resourceGroup().location]",
  "kind": "AzurePowerShell", // or "AzureCLI"
  "identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID": {}
    }
  },
  "properties": {
    "forceUpdateTag": 1,
    "containerSettings": {
      "containerGroupName": "mycustomaci"
    },
    "storageAccountSettings": {
      "storageAccountName": "myStorageAccount",
      "storageAccountKey": "myKey"
    },
    "azPowerShellVersion": "3.0",  // or "azCliVersion": "2.0.80"
    "arguments": "[concat('-name ', parameters('name'))]",
    "environmentVariables": [
      {
        "name": "someSecret",
        "secureValue": "if this is really a secret, don't put it here... in plain text..."
      }
    ],
    "scriptContent": "
      param([string] $name)
      $output = 'Hello {0}' -f $name
      Write-Output $output
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    ", // or "primaryScriptUri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
    "supportingScriptUris":[],
    "timeout": "PT30M",
    "cleanupPreference": "OnSuccess",
    "retentionInterval": "P1D"
  }
}
```

> [!NOTE]
> В этом примере используется демонстрационная цель.  **скриптконтент** и **примарискриптурис** не могут сосуществовать в шаблоне.

Сведения о значении свойства:

- **Identity**: служба скрипта развертывания использует управляемое пользователем удостоверение для выполнения скриптов. В настоящее время поддерживается только управляемое пользователем удостоверение.
- **kind**. Укажите тип скрипта. В настоящее время скрипты Azure PowerShell и Azure CLI поддерживаются. Значения — **азуреповершелл** и **Azure CLI**.
- **forceUpdateTag**: изменение этого значения между развертываниями шаблона приводит к повторному выполнению скрипта развертывания. Используйте функцию newGuid () или utcNow (), которая должна быть задана как defaultValue параметра. Дополнительные сведения о выполнении скрипта несколько раз см. [здесь](#run-script-more-than-once).
- **контаинерсеттингс**: укажите параметры для настройки экземпляра контейнера Azure.  **containerGroupName** — для указания имени группы контейнеров.  Если не указано, имя группы будет создано автоматически.
- **сторажеаккаунтсеттингс**: укажите параметры для использования существующей учетной записи хранения. Если не указано, учетная запись хранения создается автоматически. См. раздел [использование существующей учетной записи хранения](#use-an-existing-storage-account).
- **азповершеллверсион**/**азкливерсион**: Укажите версию модуля для использования. Список поддерживаемых версий PowerShell и CLI см. в разделе [Предварительные требования](#prerequisites).
- **arguments**. Укажите значения параметров. Значения разделяются пробелами.
- **environmentVariables**: укажите переменные среды для передачи в скрипт. Дополнительные сведения см. в разделе [Разработка скриптов развертывания](#develop-deployment-scripts).
- **scriptContent**. Укажите содержимое скрипта. Чтобы запустить внешний скрипт, используйте `primaryScriptUri` вместо него. Примеры см. в разделе [Использование встроенного скрипта](#use-inline-scripts) и [Использование внешнего скрипта](#use-external-scripts).
- **примарискриптури**: укажите общедоступный URL-адрес основного скрипта развертывания с поддерживаемыми расширениями файлов.
- **суппортингскриптурис**: укажите массив общедоступных URL-адресов для поддержки файлов, которые вызываются `ScriptContent` в `PrimaryScriptUri`либо.
- **timeout**. Укажите максимально допустимое время выполнения скрипта в [формате ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Значение по умолчанию — **P1D**.
- **клеануппреференце**. Укажите предпочтения по очистке ресурсов развертывания при выполнении скрипта в состоянии терминала. Значение по умолчанию — **всегда**, что означает удаление ресурсов, несмотря на состояние терминала (успешно, сбой, отменено). Дополнительные сведения в разделе об [очистке ресурсов скриптов развертывания](#clean-up-deployment-script-resources).
- **ретентионинтервал**: укажите интервал, в течение которого служба будет хранить ресурсы скрипта развертывания после того, как выполнение скрипта развертывания достигнет состояния терминала. Ресурсы скрипта развертывания будут удалены по истечении этого срока. Длительность основывается на [шаблоне ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Значение по умолчанию — **P1D**, то есть семь дней. Это свойство используется, если для параметра cleanupPreference установлено значение *OnExpiration*. Свойство *onexpir* в настоящее время не включено. Дополнительные сведения в разделе об [очистке ресурсов скриптов развертывания](#clean-up-deployment-script-resources).

### <a name="additional-samples"></a>Дополнительные примеры

- [Создание и назначение сертификата для хранилища ключей](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)

- [Создайте и назначьте назначаемое пользователем управляемое удостоверение группе ресурсов и запустите скрипт развертывания](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-mi.json).

> [!NOTE]
> Рекомендуется создать назначаемое пользователем удостоверение и предоставить разрешения заранее. При создании удостоверения и предоставлении разрешений в том же шаблоне, где выполняются скрипты развертывания, могут возникнуть ошибки входа и связанные с ними разрешения. На то, чтобы разрешения стали эффективными, требуется некоторое время.

## <a name="use-inline-scripts"></a>Использование встроенных скриптов

Следующий шаблон содержит один ресурс, определенный с помощью `Microsoft.Resources/deploymentScripts` типа. Выделенная часть является встроенным скриптом.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json" range="1-54" highlight="34-40":::

> [!NOTE]
> Так как встроенные скрипты развертывания заключены в двойные кавычки, строки внутри этих скриптов необходимо заключить в одинарные кавычки. Escape-символ для PowerShell — **&#92;** . Можно также использовать подстановку строк, как показано в предыдущем примере JSON. См. значение по умолчанию для параметра Name.

Скрипт принимает один параметр и выводит значение параметра. **Деплойментскриптаутпутс** используется для хранения выходных данных.  В разделе выходные данные в строке **значение** показано, как получить доступ к сохраненным значениям. `Write-Output`используется для целей отладки. Сведения о том, как получить доступ к выходному файлу, см. в разделе [Отладка скриптов развертывания](#debug-deployment-scripts).  Описания свойств см. в разделе [примеры шаблонов](#sample-templates).

Чтобы запустить скрипт, выберите **попробовать** открыть Cloud Shell, а затем вставьте следующий код в область оболочки.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to be created"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$id = Read-Host -Prompt "Enter the user-assigned managed identity ID"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json" -identity $id

Write-Host "Press [ENTER] to continue ..."
```

Он возвращает примерно такие выходные данные:

![диспетчер ресурсов сценарий развертывания шаблона, выходные данные Hello World](./media/deployment-script-template/resource-manager-template-deployment-script-helloworld-output.png)

## <a name="use-external-scripts"></a>Использование внешних скриптов

В дополнение к встроенным сценариям можно также использовать внешние файлы скриптов. Поддерживаются только основные скрипты PowerShell с расширением файла **PS1** . Для скриптов CLI первичные скрипты могут иметь расширения (или без расширения), если скрипты являются допустимыми скриптами bash. Чтобы использовать внешние файлы скриптов, замените `scriptContent` на `primaryScriptUri`. Пример:

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

Чтобы увидеть пример, выберите [здесь](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json).

Внешние файлы скриптов должны быть доступны.  Сведения о защите файлов скриптов, которые хранятся в учетных записях хранения Azure, см. в статье [развертывание частного шаблона ARM с помощью маркера SAS](./secure-template-with-sas-token.md).

Вы несете ответственность за обеспечение целостности скриптов, на которые ссылается скрипт развертывания, либо **примарискриптури** , либо **суппортингскриптурис**.  Ссылаться только на те сценарии, которым вы доверяете.

## <a name="use-supporting-scripts"></a>Использование вспомогательных скриптов

Можно разделить сложные логики на один или несколько вспомогательных файлов скриптов. `supportingScriptURI` Свойство позволяет предоставить массив URI для вспомогательных файлов скриптов, если это необходимо:

```json
"scriptContent": "
    ...
    ./Create-Cert.ps1
    ...
"

"supportingScriptUris": [
  "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/create-cert.ps1"
],
```

Вспомогательные файлы скриптов можно вызывать как из встроенных скриптов, так и из первичных файлов скриптов. Поддержка файлов скриптов не имеет ограничений на расширение файла.

Вспомогательные файлы копируются в азскриптс/азскриптинпут во время выполнения. Используйте относительный путь для ссылки на вспомогательные файлы из встроенных скриптов и первичных файлов скриптов.

## <a name="work-with-outputs-from-powershell-script"></a>Работа с выходами из скрипта PowerShell

В следующем шаблоне показано, как передавать значения между двумя ресурсами deploymentScripts:

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic.json" range="1-84" highlight="39-40,66":::

В первом ресурсе необходимо определить переменную с именем **$DeploymentScriptOutputs**и использовать ее для хранения выходных значений. Чтобы получить доступ к выходному значению из другого ресурса в шаблоне, используйте:

```json
reference('<ResourceName>').output.text
```

## <a name="work-with-outputs-from-cli-script"></a>Работа с выходами из скрипта CLI

В отличие от скрипта развертывания PowerShell, поддержка CLI/Bash не предоставляет общую переменную для хранения выходных данных скрипта. вместо этого существует переменная среды с именем **AZ_SCRIPTS_OUTPUT_PATH** , в которой хранится расположение, в котором находится файл выходных данных сценария. Если скрипт развертывания запускается из шаблона диспетчер ресурсов, эта переменная среды автоматически задается оболочкой bash.

Выходные данные скрипта развертывания должны быть сохранены в расположении AZ_SCRIPTS_OUTPUT_PATH, а выходные данные должны быть допустимым строковым объектом JSON. Содержимое файла должно быть сохранено как пара "ключ-значение". Например, массив строк сохраняется как {"Миресулт": ["foo", "Bar"]}.  Сохранение только результатов массива, например ["foo", "Bar"], недопустимо.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic-cli.json" range="1-44" highlight="32":::

[JQ](https://stedolan.github.io/jq/) используется в предыдущем примере. Он поставляется с образами контейнеров. См. раздел [Настройка среды разработки](#configure-development-environment).

## <a name="develop-deployment-scripts"></a>Разработка скриптов развертывания

### <a name="handle-non-terminating-errors"></a>Обработано устранимые ошибки

Вы можете управлять тем, как PowerShell реагирует на устранимые ошибки, используя переменную [**$ErrorActionPreference**](/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-7#erroractionpreference
) в скрипте развертывания. Служба скриптов не устанавливает или не изменяет значение.  Несмотря на значение, заданное для $ErrorActionPreference, сценарий развертывания задает для состояния подготовки ресурсов значение *Failed* при возникновении ошибки в скрипте.

### <a name="pass-secured-strings-to-deployment-script"></a>Передача защищенных строк в скрипт развертывания

Настройка переменных среды (EnvironmentVariable) в экземплярах контейнеров позволяет предоставить динамическую конфигурацию приложения или скрипта, выполняемого контейнером. Скрипт развертывания обрабатывает незащищенные и защищенные переменные среды так же, как и экземпляр контейнера Azure. Дополнительные сведения см. [в разделе Установка переменных среды в экземплярах контейнеров](../../container-instances/container-instances-environment-variables.md#secure-values).

## <a name="debug-deployment-scripts"></a>Отладка скриптов развертывания

Служба скриптов создает [учетную запись хранения](../../storage/common/storage-account-overview.md) (если не указана существующая учетная запись хранения) и [экземпляр контейнера](../../container-instances/container-instances-overview.md) для выполнения скрипта. Если служба скриптов автоматически создает эти ресурсы, то оба ресурса имеют суффикс **азскриптс** в именах ресурсов.

![Имена ресурсов скрипта развертывания шаблона диспетчер ресурсов](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

Пользовательский сценарий, результаты выполнения и файл stdout хранятся в общих папках учетной записи хранения. Существует папка с именем **азскриптс**. В папке есть еще две папки для входных и выходных файлов: **азскриптинпут** и **азскриптаутпут**.

Выходная папка содержит файл **executionresult.json** и выходной файл скрипта. Сообщение об ошибке выполнения сценария можно увидеть в **ExecutionResult. JSON**. Выходной файл создается только при успешном выполнении скрипта. Входная папка содержит системный файл скрипта PowerShell и пользовательские файлы скрипта развертывания. Вы можете заменить файл скрипта развертывания пользователя измененным и повторно запустить скрипт развертывания из экземпляра контейнера Azure.

Сведения о развертывании ресурсов скрипта развертывания можно получить на уровне группы ресурсов и на уровне подписки с помощью REST API.

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>?api-version=2019-10-01-preview
```

```rest
/subscriptions/<SubscriptionID>/providers/microsoft.resources/deploymentScripts?api-version=2019-10-01-preview
```

В следующем примере используется [ARMClient](https://github.com/projectkudu/ARMClient):

```azurepowershell
armclient login
armclient get /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/myrg/providers/microsoft.resources/deploymentScripts/myDeployementScript?api-version=2019-10-01-preview
```

Выходные данные должны быть следующего вида.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-status.json" range="1-37" highlight="15,34":::

В выходных данных отображается состояние развертывания и идентификаторы ресурсов скрипта развертывания.

Следующий REST API возвращает журнал:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

Он работает только до удаления ресурсов скрипта развертывания.

Чтобы просмотреть ресурс deploymentScripts на портале, выберите **Показать скрытые типы**:

![Диспетчер ресурсов скрипт развертывания шаблона, отображение скрытых типов, портала](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="use-an-existing-storage-account"></a>Использование имеющейся учетной записи хранения

Учетная запись хранения и экземпляр контейнера необходимы для выполнения скриптов и устранения неполадок. Вы можете указать существующую учетную запись хранения, в противном случае учетная запись хранения вместе с экземпляром контейнера автоматически создается службой сценариев. Требования к использованию существующей учетной записи хранения:

- Поддерживаются следующие типы учетных записей хранения: универсальное назначение v2, учетные записи общего назначения v1 и Филестораже. Только Филестораже поддерживает SKU уровня "Премиум". Дополнительные сведения см. в разделе [типы учетных записей хранения](../../storage/common/storage-account-overview.md).
- Правила брандмауэра учетной записи хранения еще не поддерживаются. Дополнительные сведения см. в статье [Настройка брандмауэров службы хранилища Azure и виртуальных сетей](../../storage/common/storage-network-security.md).
- Управляемое удостоверение, назначенное пользователем для скрипта развертывания, должно иметь разрешения на управление учетной записью хранения, включая чтение, создание и удаление файловых ресурсов.

Чтобы указать существующую учетную запись хранения, добавьте следующий код JSON в элемент Property объекта `Microsoft.Resources/deploymentScripts`:

```json
"storageAccountSettings": {
  "storageAccountName": "myStorageAccount",
  "storageAccountKey": "myKey"
},
```

- **storageAccountName**: укажите имя учетной записи хранения.
- **storageAccountKey "**: укажите один из ключей учетной записи хранения. Для получения ключа можно [`listKeys()`](./template-functions-resource.md#listkeys) использовать функцию. Пример:

    ```json
    "storageAccountSettings": {
        "storageAccountName": "[variables('storageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
    }
    ```

Пример полного `Microsoft.Resources/deploymentScripts` определения см. в разделе [примеры шаблонов](#sample-templates) .

При использовании существующей учетной записи хранения служба скриптов создает общую папку с уникальным именем. Сведения о том, как служба скриптов очищает файловый ресурс, см. в разделе [Очистка ресурсов скрипта развертывания](#clean-up-deployment-script-resources) .

## <a name="clean-up-deployment-script-resources"></a>Очистка ресурсов скрипта развертывания

Учетная запись хранения и экземпляр контейнера необходимы для выполнения скриптов и устранения неполадок. Вы можете указать существующую учетную запись хранения, в противном случае учетная запись хранения вместе с экземпляром контейнера автоматически создается службой сценариев. Два автоматически создаваемых ресурса удаляются службой сценариев, когда выполнение скрипта развертывания получает состояние терминала. Плата взимается за ресурсы, пока они не будут удалены. Сведения о ценах см. в статье цены на [экземпляры контейнеров](https://azure.microsoft.com/pricing/details/container-instances/) и [цены на хранилище Azure](https://azure.microsoft.com/pricing/details/storage/).

Жизненный цикл этих ресурсов определяется следующими свойствами в шаблоне:

- **клеануппреференце**: Очистка предпочтений при выполнении скрипта в состоянии терминала. Поддерживаются такие значения:

  - **Всегда**: удаляйте автоматически созданные ресурсы после того, как выполнение скрипта получит состояние терминала. Если используется существующая учетная запись хранения, служба скриптов удаляет общую папку, созданную в учетной записи хранения. Так как ресурс deploymentScripts по-прежнему может присутствовать после очистки ресурсов, службы скриптов сохраняют результаты выполнения сценария, например stdout, Outputs, возвращаемое значение и т. д., перед удалением ресурсов.
  - **Onsuccesss**: удаление автоматически созданных ресурсов только после успешного выполнения скрипта. Если используется существующая учетная запись хранения, служба скриптов удаляет файловый ресурс только после успешного выполнения скрипта. Вы по-прежнему можете получить доступ к ресурсам для поиска отладочной информации.
  - **Onexpir**: Удаление автоматических ресурсов только при истечении срока действия параметра **ретентионинтервал** . Если используется существующая учетная запись хранения, служба скриптов удаляет файловый ресурс, но оставляет учетную запись хранения.

- **ретентионинтервал**: укажите интервал времени, в течение которого ресурс скрипта будет храниться, после чего он будет удален.

> [!NOTE]
> Не рекомендуется использовать учетную запись хранения и экземпляр контейнера, созданные службой скриптов для других целей. Эти два ресурса могут быть удалены в зависимости от жизненного цикла скрипта.

## <a name="run-script-more-than-once"></a>Запустить скрипт несколько раз

Выполнение скрипта развертывания является операцией идемпотентными. Если ни одно из свойств ресурса deploymentScripts (включая встроенный скрипт) не изменилось, скрипт не будет выполнен при повторном развертывании шаблона. Служба скриптов развертывания сравнивает имена ресурсов в шаблоне с существующими ресурсами в той же группе ресурсов. Существует два варианта выполнения одного скрипта развертывания несколько раз:

- Измените имя ресурса deploymentScripts. Например, используйте функцию шаблона [UtcNow](./template-functions-date.md#utcnow) в качестве имени ресурса или как часть имени ресурса. При изменении имени ресурса создается новый ресурс deploymentScripts. Удобно хранить историю выполнения скрипта.

    > [!NOTE]
    > Функцию utcNow можно использовать только в значении по умолчанию для параметра.

- Укажите другое значение в свойстве `forceUpdateTag` шаблона.  Например, используйте utcNow в качестве значения.

> [!NOTE]
> Напишите идемпотентными скрипты развертывания. Это гарантирует, что если они снова будут выполняться случайно, это не приведет к изменениям системы. Например, если скрипт развертывания используется для создания ресурса Azure, убедитесь, что ресурс не существует, прежде чем создавать его, поэтому сценарий завершится с ошибкой или вы не создадите ресурс повторно.

## <a name="configure-development-environment"></a>Настройка среды разработки

Вы можете использовать предварительно настроенный образ контейнера DOCKER в качестве среды разработки скриптов развертывания. В следующей процедуре показано, как настроить образ DOCKER в Windows. Для Linux и Mac можно найти сведения в Интернете.

1. Установка [DOCKER Desktop](https://www.docker.com/products/docker-desktop) на компьютере разработчика.
1. Откройте классическое окно DOCKER.
1. Выберите значок DOCKER Desktop на панели задач и щелкните **Параметры**.
1. Выберите **Общие диски**, выберите локальный диск, который должен быть доступен для контейнеров, а затем нажмите кнопку **Применить** .

    ![Диск DOCKER сценария развертывания шаблона диспетчер ресурсов](./media/deployment-script-template/resource-manager-deployment-script-docker-setting-drive.png)

1. Введите учетные данные Windows в командной строке.
1. Откройте окно терминала — либо командную строку, либо Windows PowerShell (не используйте PowerShell ISE).
1. Вытяните образ контейнера скрипта развертывания на локальный компьютер:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    В примере используется версия PowerShell 2.7.0.

    Чтобы извлечь образ CLI из реестра контейнеров Майкрософт (мкр), сделайте следующее:

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    В этом примере используется версия 2.0.80 CLI. Скрипт развертывания использует образы по умолчанию контейнеров CLI, расположенные [здесь](https://hub.docker.com/_/microsoft-azure-cli).

1. Локальное выполнение образа DOCKER.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    Замените ** &lt;букву драйвера узла>** и ** &lt;имя каталога узла>** существующей папкой на общем диске.  Она сопоставляет папку с папкой **/Дата** в контейнере. Например, для сопоставлений Д:\доккер:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **— это** означает поддержание активности образа контейнера.

    Пример интерфейса командной строки:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. При появлении запроса выберите **поделиться им** .
1. На следующем снимке экрана показано, как запустить сценарий PowerShell, учитывая, что файл HelloWorld. ps1 находится в папке д:\доккер

    ![диспетчер ресурсов скрипт развертывания шаблона DOCKER cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

После успешного тестирования скрипта его можно использовать в качестве скрипта развертывания.

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали, как использовать скрипты развертывания. Для просмотра учебника по скриптам развертывания выполните следующие действия.

> [!div class="nextstepaction"]
> [Учебник. Использование скриптов развертывания в шаблонах Azure Resource Manager](./template-tutorial-deployment-script.md)