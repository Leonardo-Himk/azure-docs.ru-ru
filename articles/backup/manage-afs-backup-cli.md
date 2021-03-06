---
title: Управление резервными копиями файловых ресурсов Azure с помощью Azure CLI
description: Узнайте, как использовать Azure CLI для управления и мониторинга файловых ресурсов Azure, резервное копирование которых осуществляется Azure Backup.
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: 06e1f29874085c3943a5207f36eff313dc670e88
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82184118"
---
# <a name="manage-azure-file-share-backups-with-the-azure-cli"></a>Управление резервными копиями файловых ресурсов Azure с помощью Azure CLI

Azure CLI предоставляет возможности командной строки для управления ресурсами Azure. Это отличный инструмент для создания пользовательской автоматизации для использования ресурсов Azure. В этой статье объясняется, как выполнять задачи по управлению файловыми ресурсами Azure, которые архивируются с помощью [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview). Эти действия также можно выполнить с помощью [портал Azure](https://portal.azure.com/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать интерфейс командной строки локально, необходимо запустить Azure CLI версии 2.0.18 или более поздней. Чтобы получить необходимую версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Предварительные требования

В этой статье предполагается, что у вас уже есть резервная копия общего файлового ресурса Azure, [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview). Если у вас ее нет, см. раздел [резервное копирование файловых ресурсов Azure с помощью интерфейса командной строки](backup-afs-cli.md) для настройки резервного копирования общих файловых ресурсов. В этой статье вы используете следующие ресурсы:

* **Группа ресурсов**: *azurefiles*
* **RecoveryServicesVault**: *азурефилесваулт*
* **Учетная запись хранения**: *афсаккаунт*
* **Общая папка**: *azurefiles*

## <a name="monitor-jobs"></a>Мониторинг заданий

При запуске операций резервного копирования или восстановления служба резервного копирования создает задание для отслеживания. Для отслеживания завершенных или выполняющихся заданий используйте командлет [AZ Backup Job List](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) . С помощью интерфейса командной строки можно также [приостановить выполнение текущего задания](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-stop) или [дождаться завершения задания](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-wait).

В следующем примере отображается состояние заданий резервного копирования для хранилища служб восстановления *азурефилесваулт* .

```azurecli-interactive
az backup job list --resource-group azurefiles --vault-name azurefilesvault
```

```output
[
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/d477dfb6-b292-4f24-bb43-6b14e9d06ab5",
    "location": null,
    "name": "d477dfb6-b292-4f24-bb43-6b14e9d06ab5",
    "properties": {
      "actionsInfo": null,
      "activityId": "3cef43ed-0af4-43e2-b9cb-1322c496ccb4",
      "backupManagementType": "AzureStorage",
      "duration": "0:00:29.718011",
      "endTime": "2020-01-13T08:05:29.180606+00:00",
      "entityFriendlyName": "azurefiles",
      "errorDetails": null,
      "extendedInfo": null,
      "jobType": "AzureStorageJob",
      "operation": "Backup",
      "startTime": "2020-01-13T08:04:59.462595+00:00",
      "status": "Completed",
      "storageAccountName": "afsaccount",
      "storageAccountVersion": "MicrosoftStorage"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupJobs"
  },
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/1b9399bf-c23c-4caa-933a-5fc2bf884519",
    "location": null,
    "name": "1b9399bf-c23c-4caa-933a-5fc2bf884519",
    "properties": {
      "actionsInfo": null,
      "activityId": "2663449c-94f1-4735-aaf9-5bb991e7e00c",
      "backupManagementType": "AzureStorage",
      "duration": "0:00:28.145216",
      "endTime": "2020-01-13T08:05:27.519826+00:00",
      "entityFriendlyName": "azurefilesresource",
      "errorDetails": null,
      "extendedInfo": null,
      "jobType": "AzureStorageJob",
      "operation": "Backup",
      "startTime": "2020-01-13T08:04:59.374610+00:00",
      "status": "Completed",
      "storageAccountName": "afsaccount",
      "storageAccountVersion": "MicrosoftStorage"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupJobs"
  }
]
```

## <a name="modify-policy"></a>Изменение политики

Вы можете изменить политику резервного копирования, чтобы изменить периодичность резервного копирования или диапазон хранения с помощью команды [AZ Backup Set-Policy](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-set-policy).

Чтобы изменить политику, определите следующие параметры:

* **--Container-Name**: имя учетной записи хранения, в которой размещена общая папка. Чтобы получить **имя** или **понятное имя** контейнера, используйте команду [AZ Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) .
* **--Name**: имя общей папки, для которой необходимо изменить политику. Чтобы получить **имя** или **понятное имя** архивированного элемента, используйте команду [AZ Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) .
* **--Policy-Name**: имя политики резервного копирования, которую вы хотите задать для общей папки. Чтобы просмотреть все политики для хранилища, можно использовать команду [AZ Backup Policy List](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-list) .

В следующем примере задается политика резервного копирования *schedule2* для общей папки *azurefiles* , которая находится в учетной записи хранения *афсаккаунт* .

```azurecli-interactive
az backup item set-policy --policy-name schedule2 --name azurefiles --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --name "AzureFileShare;azurefiles" --backup-management-type azurestorage --out table
```

Можно также выполнить предыдущую команду, используя понятные имена для контейнера и элемента, предоставив следующие два дополнительных параметра:

* **--BACKUP-Management-Type**: *azurestorage*
* **--Рабочая нагрузка — тип**: *azurefileshare*

```azurecli-interactive
az backup item set-policy --policy-name schedule2 --name azurefiles --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --name azurefiles --backup-management-type azurestorage --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
fec6f004-0e35-407f-9928-10a163f123e5  azurefiles
```

Атрибут **Name** в выходных данных соответствует имени задания, созданного службой резервного копирования для операции изменения политики. Для наблюдения за состоянием задания используйте команду [AZ Backup Job показывать](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) командлет.

## <a name="stop-protection-on-a-file-share"></a>Остановка защиты файлового ресурса

Есть два способа отключить защиту файловых ресурсов Azure:

* Завершите все будущие задания резервного копирования и *удалите* все точки восстановления.
* Завершите все будущие задания резервного копирования, но *оставьте* точки восстановления.

При хранении точек восстановления в хранилище могут возникать затраты, поскольку базовые моментальные снимки, созданные Azure Backup, будут сохранены. Преимуществом использования точек восстановления является возможность восстановления общей папки позже, если требуется. Сведения о затратах на уход точек восстановления см. в [сведениях о ценах](https://azure.microsoft.com/pricing/details/storage/files). Если вы решили удалить все точки восстановления, вы не сможете восстановить общую папку.

Чтобы отключить защиту для общей папки, определите следующие параметры:

* **--Container-Name**: имя учетной записи хранения, в которой размещена общая папка. Чтобы получить **имя** или **понятное имя** контейнера, используйте команду [AZ Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) .
* **--Item-Name**: имя общего файлового ресурса, для которого требуется отключить защиту. Чтобы получить **имя** или **понятное имя** архивированного элемента, используйте команду [AZ Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) .

### <a name="stop-protection-and-retain-recovery-points"></a>Отключение защиты и сохранение точек восстановления

Чтобы отключить защиту во время удержания данных, используйте командлет [AZ Backup Protection Disable](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) .

В следующем примере останавливается Защита для общей папки *azurefiles* , но все точки восстановления сохранены.

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name “AzureFileShare;azurefiles” --out table
```

Можно также выполнить предыдущую команду, используя понятное имя для контейнера и элемента, предоставив следующие два дополнительных параметра:

* **--BACKUP-Management-Type**: *azurestorage*
* **--Рабочая нагрузка — тип**: *azurefileshare*

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
fec6f004-0e35-407f-9928-10a163f123e5  azurefiles
```

Атрибут **Name** в выходных данных соответствует имени задания, созданного службой резервного копирования для операции отмены защиты. Для наблюдения за состоянием задания используйте команду [AZ Backup Job показывать](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) командлет.

### <a name="stop-protection-without-retaining-recovery-points"></a>Отключение защиты без удержания точек восстановления

Чтобы отключить защиту без удержания точек восстановления, используйте командлет [AZ Backup Protection Disable](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) , указав для параметра **Delete-Backup-Data** значение **true**.

В следующем примере останавливается Защита для общей папки *azurefiles* без удержания точек восстановления.

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name “AzureFileShare;azurefiles” --delete-backup-data true --out table
```

Можно также выполнить предыдущую команду, используя понятное имя для контейнера и элемента, предоставив следующие два дополнительных параметра:

* **--BACKUP-Management-Type**: *azurestorage*
* **--Рабочая нагрузка — тип**: *azurefileshare*

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --delete-backup-data true --out table
```

## <a name="resume-protection-on-a-file-share"></a>Возобновление защиты файлового ресурса

Если вы остановили защиту для файлового ресурса Azure, но сохранили точки восстановления, вы можете возобновить защиту позже. Если точки восстановления не сохранены, вы не сможете возобновить защиту.

Чтобы возобновить защиту для общей папки, задайте следующие параметры:

* **--Container-Name**: имя учетной записи хранения, в которой размещена общая папка. Чтобы получить **имя** или **понятное имя** контейнера, используйте команду [AZ Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) .
* **--Item-Name**: имя общего файлового ресурса, для которого необходимо возобновить защиту. Чтобы получить **имя** или **понятное имя** архивированного элемента, используйте команду [AZ Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) .
* **--Policy-Name**: имя политики резервного копирования, для которой требуется возобновить защиту для общей папки.

В следующем примере используется командлет [AZ Backup Protection Resume](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-resume) для возобновления защиты файлового ресурса *azurefiles* с помощью политики резервного копирования *schedule1* .

```azurecli-interactive
az backup protection resume --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --policy-name schedule2 --out table
```

Можно также выполнить предыдущую команду, используя понятное имя для контейнера и элемента, предоставив следующие два дополнительных параметра:

* **--BACKUP-Management-Type**: *azurestorage*
* **--Рабочая нагрузка — тип**: *azurefileshare*

```azurecli-interactive
az backup protection resume --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --policy-name schedule2 --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
75115ab0-43b0-4065-8698-55022a234b7f  azurefiles
```

Атрибут **Name** в выходных данных соответствует имени задания, созданного службой архивации для операции возобновления защиты. Для наблюдения за состоянием задания используйте команду [AZ Backup Job показывать](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) командлет.

## <a name="unregister-a-storage-account"></a>Отмена регистрации учетной записи хранения

Если вы хотите защитить файловые ресурсы в определенной учетной записи хранения с помощью другого хранилища служб восстановления, сначала [Отключите защиту для всех файловых ресурсов](#stop-protection-on-a-file-share) в этой учетной записи хранения. Затем отмените регистрацию учетной записи в хранилище служб восстановления, которое сейчас используется для защиты.

Чтобы отменить регистрацию учетной записи хранения, необходимо указать имя контейнера. Чтобы получить **имя** или **понятное имя** контейнера, используйте команду [AZ Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) .

В следующем примере выполняется отмена регистрации учетной записи хранения *афсаккаунт* из *азурефилесваулт* с помощью командлета [AZ Backup Container Unregister](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-unregister) .

```azurecli-interactive
az backup container unregister --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --out table
```

Вы также можете запустить предыдущий командлет, используя понятное имя для контейнера, предоставив следующий дополнительный параметр:

* **--BACKUP-Management-Type**: *azurestorage*

```azurecli-interactive
az backup container unregister --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --backup-management-type azurestorage --out table
```

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в статье [Устранение неполадок резервного копирования файловых ресурсов Azure](troubleshoot-azure-files.md).
