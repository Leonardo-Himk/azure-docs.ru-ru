---
title: Интеграция реестра контейнеров Azure с помощью службы Kubernetes Azure
description: Узнайте, как интегрировать службу Kubernetes Azure (AKS) с реестром контейнеров Azure (запись контроля доступа).
services: container-service
manager: gwallace
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: 514cc25e1959145c65fe60cd3054cec4ed28f44d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80617427"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Аутентификация с помощью реестра контейнеров Azure из Службы Azure Kubernetes

При использовании реестра контейнеров Azure (ACR) со Службой Azure Kubernetes (AKS) необходимо установить механизм аутентификации. В этой статье приведены примеры настройки проверки подлинности между этими двумя службами Azure. 

Вы можете настроить AKS для интеграции записей контроля доступа в нескольких простых командах с Azure CLI. Эта интеграция назначает роль Акрпулл субъекту-службе, связанному с кластером AKS.

## <a name="before-you-begin"></a>Подготовка к работе

Для этих примеров требуются:

* Роль **владельца** или **администратора учетной записи Azure** в **подписке Azure**
* Azure CLI версии 2.0.73 или более поздней

Чтобы избежать необходимости в роли администратора или **владельца** **учетной записи Azure** , можно вручную настроить субъект-службу или использовать существующий субъект-службу для проверки ПОдлинности записей контроля доступа из AKS. Дополнительные сведения см. в статьях [Аутентификация в реестре контейнеров Azure с помощью субъектов-служб](../container-registry/container-registry-auth-service-principal.md) и [Проверка подлинности в Kubernetes с использованием секрета для извлечения](../container-registry/container-registry-auth-kubernetes.md).

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>Создание нового кластера AKS с интеграцией записей контроля доступа

Вы можете настроить интеграцию AKS и записей контроля доступа во время первоначального создания кластера AKS.  Чтобы разрешить кластеру AKS взаимодействовать с записью контроля доступа, используется **субъект-служба** Azure Active Directory. Следующая команда CLI позволяет авторизовать существующую запись контроля доступа в подписке и настроить соответствующую роль **акрпулл** для субъекта-службы. Укажите допустимые значения для параметров ниже.

```azurecli
# set this to the name of your Azure Container Registry.  It must be globally unique
$MYACR=myContainerRegistry

# Run the following line to create an Azure Container Registry if you do not already have one
az acr create -n $MYACR -g myContainerRegistryResourceGroup --sku basic

# Create an AKS cluster with ACR integration
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr $MYACR
```

Кроме того, можно указать имя записи контроля доступа с помощью идентификатора ресурса записей контроля доступа в следующем формате:

`/subscriptions/\<subscription-id\>/resourceGroups/\<resource-group-name\>/providers/Microsoft.ContainerRegistry/registries/\<name\>` 

```azurecli
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr /subscriptions/<subscription-id>/resourceGroups/myContainerRegistryResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry
```

Выполнение этого шага может занять несколько минут.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Настройка интеграции записей контроля доступа для существующих кластеров AKS

Интегрируйте существующую запись контроля доступа с существующими кластерами AKS, указав допустимые значения для записей **контроля учетных** записей с именем или записи **контроля доступа (ИД ресурса** ), как показано ниже.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
```

или

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

Вы также можете удалить интеграцию между записью контроля доступа и кластером AKS, выполнив следующие

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
```

или диспетчер конфигурации служб

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```

## <a name="working-with-acr--aks"></a>Работа с записью контроля доступа & AKS

### <a name="import-an-image-into-your-acr"></a>Импорт изображения в запись контроля доступа

Импортируйте образ из DOCKER Hub в запись контроля доступа, выполнив следующую команду:


```azurecli
az acr import  -n <myContainerRegistry> --source docker.io/library/nginx:latest --image nginx:v1
```

### <a name="deploy-the-sample-image-from-acr-to-aks"></a>Развертывание примера образа из записи контроля доступа в AKS

Убедитесь, что у вас есть правильные учетные данные AKS

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

Создайте файл с именем записи **контроля доступа nginx. YAML** , который содержит следующее:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: <replace this image property with you acr login server, image and tag>
        ports:
        - containerPort: 80
```

Затем запустите это развертывание в кластере AKS:

```console
kubectl apply -f acr-nginx.yaml
```

Вы можете отслеживать развертывание, выполнив:

```console
kubectl get pods
```

Необходимо иметь два работающих модуля.

```output
NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->
[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
