---
title: Пример скрипта Azure CLI для добавления приложения в пакетную службу
description: В этом примере скрипта показано, как добавить приложение для использования в пуле или задаче пакетной службы Azure.
ms.topic: sample
ms.date: 01/29/2018
ms.openlocfilehash: 792af55be17551d1635b6821456c6e8e53e9daea
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681939"
---
# <a name="cli-example-add-an-application-to-an-azure-batch-account"></a>Пример использования CLI: добавление приложения в учетную запись пакетной службы Azure

В этом скрипте показано, как добавить приложение для использования в пуле или задаче пакетной службы Azure. Чтобы настроить приложение для его добавления в учетную запись пакетной службы, упакуйте его исполняемый файл и зависимые компоненты в ZIP-файл. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этой статьей вам понадобится Azure CLI 2.0.20 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Пример сценария

[!code-azurecli-interactive[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Выполните следующую команду, чтобы удалить группу ресурсов и все связанные с ней ресурсы.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды.
Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Создание учетной записи хранения. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Создает учетную запись пакетной службы. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Выполняет проверку подлинности с помощью указанной учетной записи пакетной службы для дальнейшего взаимодействия с интерфейсом командной строки.  |
| [az batch application create](/cli/azure/batch/application#az-batch-application-create) | Создает приложение.  |
| [az batch application package create](/cli/azure/batch/application/package#az-batch-application-package-create) | Добавляет пакет приложения в указанное приложение.  |
| [az batch application set](/cli/azure/batch/application#az-batch-application-set) | Обновляет свойства приложения.  |
| [az group delete](/cli/azure/group#az-group-delete) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).
