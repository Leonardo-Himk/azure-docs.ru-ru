---
title: Создание ресурсов Azure Cosmos DB с помощью шаблонов Resource Manager и управление ими
description: Использование шаблонов Azure Resource Manager для создания и настройки Azure Cosmos DB для API Core (SQL)
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: mjbrown
ms.openlocfilehash: dfdeb210c59377822b2ee69bde286b87c2251021
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592497"
---
# <a name="manage-azure-cosmos-db-core-sql-api-resources-with-azure-resource-manager-templates"></a>Управление ресурсами API Core (SQL) в Azure Cosmos DB с помощью шаблонов Azure Resource Manager

Из этой статьи вы узнаете, как использовать шаблоны Azure Resource Manager для развертывания учетных записей, баз данных и контейнеров Azure Cosmos DB и управления ими.

В этой статье показаны только примеры шаблонов Azure Resource Manager для учетных записей API Core (SQL). Кроме того, можно найти примеры шаблонов для API [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) и [таблиц](manage-table-with-resource-manager.md).

> [!IMPORTANT]
>
> * Длина имен учетных записей ограничена 44 символами (только строчные буквы).
> * Чтобы изменить значения пропускной способности, повторно разверните шаблон с обновленным значением ЕЗ/с.
> * Одновременно с добавлением или удалением расположений в учетной записи Azure Cosmos нельзя изменять другие свойства. Эти операции должны выполняться отдельно.

Чтобы создать любой из перечисленных ниже ресурсов Azure Cosmos DB, скопируйте следующий пример шаблона в новый JSON-файл. При необходимости можно создать JSON-файл параметров, который будет использоваться при развертывании нескольких экземпляров одного ресурса с разными именами и значениями. Шаблоны Azure Resource Manager можно развертывать множеством способов, в том числе с помощью [портала Azure](../azure-resource-manager/templates/deploy-portal.md), [Azure CLI](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) и [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md).

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-with-autoscale-throughput"></a>Учетная запись Azure Cosmos c автомасштабированием пропускной способности

Этот шаблон используется для создания учетной записи Azure Cosmos в двух регионах с заданными параметрами для согласованности и отработки отказа. При этом база данных и контейнер настроены для автомасштабирования пропускной способности, вследствие чего большинство параметров политики включено. Этот шаблон также доступен для развертывания одним щелчком из коллекции шаблонов быстрого запуска Azure.

[![Развертывание в Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql-autoscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-autoscale/azuredeploy.json":::

<a id="create-analytical-store"></a>

## <a name="azure-cosmos-account-with-analytical-store"></a>Учетная запись Azure Cosmos с аналитическим хранилищем

Этот шаблон используется для создания учетной записи Azure Cosmos в одном регионе с контейнером, для которого включена поддержка аналитического срока жизни и возможность выбрать для пропускной способности настройку вручную или с помощью автомасштабирования.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
       "accountName": {
          "type": "string",
          "defaultValue": "",
          "metadata": {
             "description": "Cosmos DB account name"
          }
       },
       "location": {
          "type": "string",
          "defaultValue": "",
          "metadata": {
             "description": "Location for the Cosmos DB account."
          }
       },
        "databaseName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The name for the database"
            }
        },
        "containerName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The name for the container"
            }
        },
        "partitionKeyPath": {
            "type": "string",
            "defaultValue": "/partitionKey",
            "metadata": {
                "description": "The partition key for the container"
            }
        },
        "throughputPolicy":{
            "type": "string",
            "defaultValue": "Autoscale",
            "allowedValues": [ "Manual", "Autoscale" ],
            "metadata": {
                "description": "The throughput policy for the container"
            }
        },
        "manualProvisionedThroughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "Throughput value when using Manual Throughput Policy for the container"
            }
        },
        "maxAutoscaleThroughput": {
            "type": "int",
            "defaultValue": 4000,
            "minValue": 4000,
            "maxValue": 1000000,
            "metadata": {
                "description": "Maximum throughput when using Autoscale Throughput Policy for the container"
            }
        }
    },
    "variables": {
        "accountName": "[toLower(parameters('accountName'))]",
        "locations": 
        [ 
            {
                "locationName": "[parameters('location')]",
                "failoverPriority": 0,
                "isZoneRedundant": false
            }
        ],
        "throughputPolicy": {
            "Manual": {
                "Throughput": "[parameters('manualProvisionedThroughput')]"
            },
            "Autoscale": {
                "ProvisionedThroughputSettings": "[concat('{\"maxThroughput\":\"', parameters('maxAutoscaleThroughput'), '\"}')]"
            }            
        },
        "throughputPolicyToUse": "[if(equals(parameters('throughputPolicy'), 'Manual'), variables('throughputPolicy').Manual, variables('throughputPolicy').Autoscale)]"
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "name": "[variables('accountName')]",
            "apiVersion": "2020-03-01",
            "location": "[parameters('location')]",
            "properties": {
                "consistencyPolicy": {"defaultConsistencyLevel": "Session"},
                "databaseAccountOfferType": "Standard",
                "locations": "[variables('locations')]",
                "enableAnalyticalStorage": true
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'))]",
            "apiVersion": "2020-03-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts', variables('accountName'))]" ],
            "properties":{
                "resource":{
                    "id": "[parameters('databaseName')]"
                }
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('containerName'))]",
            "apiVersion": "2020-03-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases', variables('accountName'), parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('containerName')]",
                    "partitionKey": {
                        "paths": [ "[parameters('partitionKeyPath')]" ],
                        "kind": "Hash"
                    },
                    "analyticalStorageTtl": -1
                },
                "options": "[variables('throughputPolicyToUse')]"
            }
        }
    ]
}
```

<a id="create-manual"></a>

## <a name="azure-cosmos-account-with-standard-manual-throughput"></a>Учетная запись Azure Cosmos c применением стандартной настройки пропускной способности (вручную)

Этот шаблон используется для создания учетной записи Azure Cosmos в двух регионах с заданными параметрами для согласованности и отработки отказа. При этом база данных и контейнер настроены для регулирования пропускной способности вручную, вследствие чего большинство параметров политики включено. Этот шаблон также доступен для развертывания одним щелчком из коллекции шаблонов быстрого запуска Azure.

[![Развертывание в Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json":::

<a id="create-sproc"></a>

## <a name="azure-cosmos-db-container-with-server-side-functionality"></a>Контейнер Azure Cosmos DB с функциональностью на стороне сервера

Этот шаблон используется для создания учетной записи, базы данных и контейнера Azure Cosmos с хранимой процедурой, триггером и определяемой пользователем функцией. Этот шаблон также доступен для развертывания одним щелчком из коллекции шаблонов быстрого запуска Azure.

[![Развертывание в Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql-container-sprocs%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json":::

<a id="free-tier"></a>

## <a name="free-tier-azure-cosmos-db-account"></a>Учетная запись Azure Cosmos DB уровня "Бесплатный"

Этот шаблон используется для создания учетной записи Azure Cosmos уровня "Бесплатный" и базы данных с общей пропускной способностью, которую могут совместно использовать до 25 контейнеров. Этот шаблон также доступен для развертывания одним щелчком из коллекции шаблонов быстрого запуска Azure.

[![Развертывание в Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql-free%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-free/azuredeploy.json":::

## <a name="next-steps"></a>Дальнейшие действия

Ниже приведены некоторые дополнительные ресурсы.

* [Документация по Azure Resource Manager](/azure/azure-resource-manager/)
* [Схема поставщика ресурсов Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
* [Шаблоны быстрого запуска Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb&pageNumber=1&sort=Popular)
* [Устранение распространенных ошибок при развертывании с помощью Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
