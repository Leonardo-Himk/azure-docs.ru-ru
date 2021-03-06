---
title: Управление кластерами Azure HDInsight с помощью Azure CLI
description: Узнайте, как использовать Azure CLI для управления кластерами Azure HDInsight. К типам кластеров относятся Apache Hadoop, Spark, HBase, Kafka, Интерактивные запросы и службы ML.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 02/26/2020
ms.openlocfilehash: 2c6495454e5ba2449d4b3c74a096681f74610813
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79272777"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Управление кластерами Azure HDInsight с помощью Azure CLI

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Узнайте, как использовать [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) для управления кластерами Azure HDInsight. Azure CLI — это кроссплатформенный интерфейс командной строки от Майкрософт для управления ресурсами Azure.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

* Azure CLI. Если вы еще не установили Azure CLI, обратитесь к статье [Установка Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

* Кластер Apache Hadoop в HDInsight. Ознакомьтесь со статьей [Краткое руководство. Использование Apache Hadoop и Apache Hive в Azure HDInsight с шаблоном Resource Manager](hadoop/apache-hadoop-linux-tutorial-get-started.md).

## <a name="connect-to-azure"></a>Подключение к Azure

Войдите в свою подписку Azure. Если вы планируете использовать Azure Cloud Shell, щелкните **Попробовать** в правом верхнем углу блока кода. В противном случае введите следующую команду:

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>список кластеров

Список кластеров можно получить с помощью команды [AZ hdinsight List](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) . Измените приведенные ниже команды, `RESOURCE_GROUP_NAME` заменив именем своей группы ресурсов, а затем введите команды:

```azurecli-interactive
# List all clusters in the current subscription
az hdinsight list

# List only cluster name and its resource group
az hdinsight list --query "[].{Cluster:name, ResourceGroup:resourceGroup}" --output table

# List all cluster for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME

# List all cluster names for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME --query "[].{clusterName:name}" --output table
```

## <a name="show-cluster"></a>Отображение кластеров

Чтобы отобразить сведения об указанном кластере, используйте команду [AZ hdinsight демонстрация](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) . Измените приведенную ниже команду, `RESOURCE_GROUP_NAME`заменив `CLUSTER_NAME` и соответствующей информацией, а затем введите команду:

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>Удаление кластеров

Чтобы удалить указанный кластер, используйте команду [AZ hdinsight Delete](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) . Измените приведенную ниже команду, `RESOURCE_GROUP_NAME`заменив `CLUSTER_NAME` и соответствующей информацией, а затем введите команду:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

Можно также удалить кластер, удалив группу ресурсов, которая содержит этот кластер. Обратите внимание, что это приведет к удалению всех ресурсов в группе, включая учетную запись хранения по умолчанию.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>Масштабирование кластеров

Чтобы изменить размер указанного кластера HDInsight до указанного размера, используйте команду [AZ hdinsight изменить](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) размер. Измените приведенную ниже команду, `RESOURCE_GROUP_NAME`заменив `CLUSTER_NAME` и соответствующими данными. Замените `WORKERNODE_COUNT` требуемым числом рабочих узлов для кластера. Дополнительные сведения о масштабировании кластеров см. в статье [масштабирование кластеров HDInsight](./hdinsight-scaling-best-practices.md). Введите команду:

```azurecli-interactive
az hdinsight resize --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --workernode-count WORKERNODE_COUNT
```

## <a name="next-steps"></a>Дальнейшие шаги

В этой статье вы узнали, как выполнять различные задачи администрирования кластера HDInsight. Дополнительные сведения см. в следующих статьях:

* [Управление кластерами Apache Hadoop в HDInsight с помощью портала Azure](hdinsight-administer-use-portal-linux.md)
* [Управление кластерами Hadoop в HDInsight с помощью Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Приступая к работе с Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Приступая к работе с Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
