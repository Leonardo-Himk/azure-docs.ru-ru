---
title: включить файл
description: включить файл
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 012800806aeff81939baa2cee88e78191e4fb6c5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195270"
---
### <a name="create-a-user-assigned-identity"></a>Создание назначаемого пользователем удостоверения

Создайте удостоверение с именем *мякртасксид* в подписке с помощью команды [AZ Identity Create][az-identity-create] . Вы можете использовать ту же группу ресурсов, которая использовалась ранее для создания реестра контейнеров или другого.

```azurecli
az identity create \
  --resource-group myResourceGroup \
  --name myACRTasksId
```

Чтобы настроить назначенное пользователем удостоверение в следующих шагах, используйте команду [AZ Identity][az-identity-show] , чтобы сохранить идентификатор ресурса, идентификатор субъекта и идентификатор клиента в переменных.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query id --output tsv)

# Get principal ID of the task's user-assigned identity
principalID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query clientId --output tsv)
```

<!-- LINKS - Internal -->
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show