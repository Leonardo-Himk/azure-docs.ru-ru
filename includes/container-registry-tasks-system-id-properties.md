---
title: Включить имя файла
description: включить файл
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 700dbfde3be2f24eb57acbdeb9d2841ef2bdfe44
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77112298"
---
В выходных данных команды в `identity` разделе показано, что в задаче `SystemAssigned` задано удостоверение типа. `principalId` — Идентификатор субъекта для удостоверения задачи:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 
Используйте команду [AZ запись контроля][az-acr-task-show] доступа, чтобы сохранить principalId в переменной для использования в последующих командах. Замените имя задачи и реестра в следующей команде:

```azurecli
principalID=$(az acr task show --name mytask --registry myregistry --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
