---
title: Шифрование данных — Azure CLI — база данных Azure для MySQL
description: Узнайте, как настроить шифрование данных для базы данных Azure для MySQL и управлять им с помощью Azure CLI.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: 3c33fdb114356af7707c1aae2eddefd81bf10b9f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82185835"
---
# <a name="data-encryption-for-azure-database-for-mysql-by-using-the-azure-cli"></a>Шифрование данных для базы данных Azure для MySQL с помощью Azure CLI

Узнайте, как использовать Azure CLI для настройки и управления шифрованием данных для базы данных Azure для MySQL.

## <a name="prerequisites-for-azure-cli"></a>Необходимые условия для Azure CLI

* Подписка Azure и права администратора для нее.
* Создайте хранилище ключей и ключ, который будет использоваться для ключа, управляемого клиентом. Также включите защиту от очистки и обратимое удаление в хранилище ключей.

    ```azurecli-interactive
    az keyvault create -g <resource_group> -n <vault_name> --enable-soft-delete true --enable-purge-protection true
    ```

* В созданном Azure Key Vault создайте ключ, который будет использоваться для шифрования данных базы данных Azure для MySQL.

    ```azurecli-interactive
    az keyvault key create --name <key_name> -p software --vault-name <vault_name>
    ```

* Чтобы использовать существующее хранилище ключей, оно должно иметь следующие свойства для использования в качестве ключа, управляемого клиентом:
  * [Обратимое удаление](../key-vault/general/overview-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Очистить защищенные](../key-vault/general/overview-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Ключ должен иметь следующие атрибуты для использования в качестве ключа, управляемого клиентом:
  * без даты окончания срока действия;
  * не отключено;
  * Выполнение операций **получения**, **переноса**и **распаковки**

## <a name="set-the-right-permissions-for-key-operations"></a>Задайте правильные разрешения для операций с ключами.

1. Существует два способа получения управляемого удостоверения для базы данных Azure для MySQL.

    ### <a name="create-an-new-azure-database-for-mysql-server-with-a-managed-identity"></a>Создайте новый сервер базы данных Azure для MySQL с управляемым удостоверением.

    ```azurecli-interactive
    az mysql server create --name -g <resource_group> --location <locations> --storage-size <size>  -u <user>-p <pwd> --backup-retention <7> --sku-name <sku name> --geo-redundant-backup <Enabled/Disabled>  --assign-identity
    ```

    ### <a name="update-an-existing-the-azure-database-for-mysql-server-to-get-a-managed-identity"></a>Обновите существующий сервер базы данных Azure для MySQL, чтобы получить управляемое удостоверение.

    ```azurecli-interactive
    az mysql server update --name  <server name>  -g <resource_group> --assign-identity
    ```

2. Задайте **разрешения ключа** (**Get**, **Wrap**, **Unwrap**) для **участника**, который является именем сервера MySQL.

    ```azurecli-interactive
    az keyvault set-policy --name -g <resource_group> --key-permissions get unwrapKey wrapKey --object-id <principal id of the server>
    ```

## <a name="set-data-encryption-for-azure-database-for-mysql"></a>Настройка шифрования данных для базы данных Azure для MySQL

1. Включите шифрование данных для базы данных Azure для MySQL с помощью ключа, созданного в Azure Key Vault.

    ```azurecli-interactive
    az mysql server key create –name  <server name>  -g <resource_group> --kid <key url>
    ```

    URL-адрес ключа:`https://YourVaultName.vault.azure.net/keys/YourKeyName/01234567890123456789012345678901>`

## <a name="using-data-encryption-for-restore-or-replica-servers"></a>Использование шифрования данных для серверов восстановления или реплик

После того как база данных Azure для MySQL шифруется с помощью управляемого ключа клиента, хранящегося в Key Vault, все вновь созданные копии сервера также шифруются. Эту новую копию можно создать с помощью локальной или географической операции либо с помощью операции реплики (локальной или кросс-региона). Для зашифрованного сервера MySQL можно выполнить следующие действия, чтобы создать зашифрованный восстановленный сервер.

### <a name="creating-a-restoredreplica-server"></a>Создание восстановленного или сервера-реплики

  *  [Создание сервера восстановления](howto-restore-server-cli.md) 
  *  [Создание сервера реплики для чтения](howto-read-replicas-cli.md) 

### <a name="once-the-server-is-restored-revalidate-data-encryption-the-restored-server"></a>После восстановления сервера повторно проверьте шифрование данных на восстановленном сервере.

    ```azurecli-interactive
    az mysql server key create –name  <server name> -g <resource_group> --kid <key url>
    ```

## <a name="additional-capability-for-the-key-being-used-for-the-azure-database-for-mysql"></a>Дополнительные возможности для ключа, используемого для базы данных Azure для MySQL

### <a name="get-the-key-used"></a>Получение ключа, используемого

    ```azurecli-interactive
    az mysql server key show --name  <server name>  -g <resource_group> --kid <key url>
    ```

    Key url:  `https://YourVaultName.vault.azure.net/keys/YourKeyName/01234567890123456789012345678901>`

### <a name="list-the-key-used"></a>Список используемых ключей

    ```azurecli-interactive
    az mysql server key list --name  <server name>  -g <resource_group>
    ```

### <a name="drop-the-key-being-used"></a>Удаление используемого ключа

    ```azurecli-interactive
    az mysql server key delete -g <resource_group> --kid <key url> 
    ```

## <a name="using-an-azure-resource-manager-template-to-enable-data-encryption"></a>Использование шаблона Azure Resource Manager для включения шифрования данных

Помимо портал Azure можно также включить шифрование данных на сервере базы данных Azure для MySQL с помощью шаблонов Azure Resource Manager для новых и существующих серверов.

### <a name="for-a-new-server"></a>Для нового сервера

Используйте один из предварительно созданных шаблонов Azure Resource Manager для подготовки сервера с включенным шифрованием данных: [пример с шифрованием данных](https://github.com/Azure/azure-mysql/tree/master/arm-templates/ExampleWithDataEncryption) .

Этот шаблон Azure Resource Manager создает сервер базы данных Azure для MySQL и использует **KeyVault** и **ключ** , переданные в качестве параметров для включения шифрования данных на сервере.

### <a name="for-an-existing-server"></a>Для существующего сервера
Кроме того, вы можете использовать шаблоны Azure Resource Manager, чтобы включить шифрование данных на существующих серверах базы данных Azure для MySQL.

* Передайте идентификатор ресурса Azure Key Vaultого ключа, скопированный ранее в `Uri` свойстве объекта Properties.

* Используйте *2020-01-01-Preview* в качестве версии API.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string"
    },
    "serverName": {
      "type": "string"
    },
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Key vault name where the key to use is stored"
      }
    },
    "keyVaultResourceGroupName": {
      "type": "string",
      "metadata": {
        "description": "Key vault resource group name where it is stored"
      }
    },
    "keyName": {
      "type": "string",
      "metadata": {
        "description": "Key name in the key vault to use as encryption protector"
      }
    },
    "keyVersion": {
      "type": "string",
      "metadata": {
        "description": "Version of the key in the key vault to use as encryption protector"
      }
    }
  },
  "variables": {
    "serverKeyName": "[concat(parameters('keyVaultName'), '_', parameters('keyName'), '_', parameters('keyVersion'))]"
  },
  "resources": [
    {
      "type": "Microsoft.DBforMySQL/servers",
      "apiVersion": "2017-12-01",
      "kind": "",
      "location": "[parameters('location')]",
      "identity": {
        "type": "SystemAssigned"
      },
      "name": "[parameters('serverName')]",
      "properties": {
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-05-01",
      "name": "addAccessPolicy",
      "resourceGroup": "[parameters('keyVaultResourceGroupName')]",
      "dependsOn": [
        "[resourceId('Microsoft.DBforMySQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.KeyVault/vaults/accessPolicies",
              "name": "[concat(parameters('keyVaultName'), '/add')]",
              "apiVersion": "2018-02-14-preview",
              "properties": {
                "accessPolicies": [
                  {
                    "tenantId": "[subscription().tenantId]",
                    "objectId": "[reference(resourceId('Microsoft.DBforMySQL/servers/', parameters('serverName')), '2017-12-01', 'Full').identity.principalId]",
                    "permissions": {
                      "keys": [
                        "get",
                        "wrapKey",
                        "unwrapKey"
                      ]
                    }
                  }
                ]
              }
            }
          ]
        }
      }
    },
    {
      "name": "[concat(parameters('serverName'), '/', variables('serverKeyName'))]",
      "type": "Microsoft.DBforMySQL/servers/keys",
      "apiVersion": "2020-01-01-preview",
      "dependsOn": [
        "addAccessPolicy",
        "[resourceId('Microsoft.DBforMySQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "serverKeyType": "AzureKeyVault",
        "uri": "[concat(reference(resourceId(parameters('keyVaultResourceGroupName'), 'Microsoft.KeyVault/vaults/', parameters('keyVaultName')), '2018-02-14-preview', 'Full').properties.vaultUri, 'keys/', parameters('keyName'), '/', parameters('keyVersion'))]"
      }
    }
  ]
}

```

## <a name="next-steps"></a>Дальнейшие действия

 Дополнительные сведения о шифровании данных см. в статье [Шифрование данных в базе данных Azure для MySQL с помощью ключа, управляемого клиентом](concepts-data-encryption-mysql.md).
