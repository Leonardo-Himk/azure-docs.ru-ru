---
title: Краткое руководство. Настройка и получение ключа из Azure Key Vault
description: Краткое руководство по настройке и получению ключа из Azure Key Vault с помощью Azure CLI
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.date: 03/30/2020
ms.author: mbaldwin
ms.openlocfilehash: defc5317c127d771786989748e404285ca0c0584
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420308"
---
# <a name="quickstart-set-and-retrieve-a-key-from-azure-key-vault-using-azure-cli"></a>Краткое руководство. Настройка и получение ключа из Azure Key Vault с помощью Azure CLI

Из этого краткого руководства вы узнаете, как создать хранилище ключей в Azure Key Vault с помощью Azure CLI. Azure Key Vault — это облачная служба, которая работает как защищенное хранилище секретов. Вы можете безопасно хранить ключи, пароли, сертификаты и другие секреты. Дополнительные сведения о хранилище ключей см. в статье [обзора](../general/overview.md). Azure CLI используется для создания ресурсов Azure и управления ими с помощью скриптов и команд. Создав ресурс, вы сможете сохранить в нем ключ.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций из этого руководства вам потребуется Azure CLI 2.0.4 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI]( /cli/azure/install-azure-cli).

Чтобы войти в Azure с помощью CLI, введите следующее:

```azurecli
az login
```

Дополнительные сведения о параметрах входа с помощью интерфейса командной строки см. в статье [Вход с помощью Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. В следующем примере создается группа ресурсов с именем *ContosoResourceGroup* в расположении *eastus*.

```azurecli
az group create --name "ContosoResourceGroup" --location eastus
```

## <a name="create-a-key-vault"></a>Создание хранилища ключей

Затем вы создадите Key Vault в группе ресурсов, созданной на предыдущем шаге. Необходимо будет указать следующие сведения:

- В этом руководстве мы используем **Contoso-vault2**. Укажите уникальное имя в тестировании.
- Имя группы ресурсов: **ContosoResourceGroup**.
- Расположение: **восточная часть США**.

```azurecli
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location eastus
```

В выходных данных командлета будут показаны свойства созданного Key Vault. Запишите значения двух указанных ниже свойств.

- **Имя хранилища.** В нашем примере это **Contoso-Vault2**. Вы будете использовать это имя для выполнения других команд Key Vault.
- **URI хранилища**. В нашем примере это https://contoso-vault2.vault.azure.net/. Необходимо, чтобы приложения, использующие ваше хранилище через REST API, использовали этот URI.

На этом этапе любые операции в этом хранилище ключей может выполнять только учетная запись Azure.

## <a name="add-a-key-to-key-vault"></a>Добавление ключа в Key Vault

Чтобы добавить ключ в хранилище, вам нужно выполнить несколько дополнительных действий. Этот ключ может использоваться приложением. 

Выполните приведенные ниже команды, чтобы создать вызываемый ключ **ExampleKey**:

```azurecli
az keyvault key create --vault-name "Contoso-Vault2" -n ExampleKey --protection software
```

Теперь ключ, добавленный в Azure Key Vault, можно вызвать, используя его URI. Используйте **https://Contoso-Vault2.vault.azure.net/keys/ExampleKey** , чтобы получить текущую версию. 

Чтобы просмотреть ранее сохраненный ключ, выполните следующие действия:

```azurecli

az keyvault key show --name "ExampleKey" --vault-name "Contoso-Vault2"
```

Вы создали Key Vault, сохранили в нем ключ и извлекли его.

## <a name="clean-up-resources"></a>Очистка ресурсов

Другие руководства в этой серии созданы на основе этого документа. Если вы планируете продолжить работу с последующими краткими руководствами и статьями, эти ресурсы можно не удалять.
Вы можете удалить ставшие ненужными группу ресурсов и все связанные с ней ресурсы, выполнив команду [az group delete](/cli/azure/group). Удалите ресурсы следующим образом:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Дальнейшие действия

С помощью этого краткого руководства вы создали Key Vault и сохранили в нем ключ. Дополнительные сведения о Key Vault и его интеграции в приложения см. в следующих статьях.

- [Обзор Azure Key Vault](../general/overview.md)
- Справочник по [командам az keyvault для Azure CLI](/cli/azure/keyvault?view=azure-cli-latest)
- [Рекомендации по Azure Key Vault](../general/best-practices.md)
