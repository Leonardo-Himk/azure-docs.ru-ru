---
title: Удаление доступа к делегированию
description: Узнайте, как удалить доступ к ресурсам, делегированным поставщику услуг для управления делегированными ресурсами Azure.
ms.date: 04/24/2020
ms.topic: conceptual
ms.openlocfilehash: d0db809eb057f8b4bb48bdf9dd127f4d488f0406
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82149454"
---
# <a name="remove-access-to-a-delegation"></a>Удаление доступа к делегированию

После делегирования подписки или группы ресурсов клиента поставщику услуг для [управления делегированными ресурсами Azure](../concepts/azure-delegated-resource-management.md)делегирование может быть удалено при необходимости. После удаления делегирования доступ, предоставленный пользователям в клиенте поставщика услуг, больше не будет применяться.

Удаление делегирования может осуществляться пользователем в клиенте клиента или в клиенте поставщика услуг при условии, что у пользователя есть соответствующие разрешения.

## <a name="customers"></a>Клиенты

Пользователи в клиенте клиента, у которых есть [Встроенная роль владельца](../../role-based-access-control/built-in-roles.md#owner) подписки, могут удалить доступ поставщика услуг к этой подписке (или группам ресурсов в этой подписке). Для этого пользователь в клиенте клиента может попасть на [страницу поставщики услуг](view-manage-service-providers.md#add-or-remove-service-provider-offers) портал Azure, найти предложение на экране **предложения поставщика услуг** и выбрать значок корзины в строке для этого предложения.

После подтверждения удаления пользователи из клиента поставщика услуг не смогут получить доступ к ресурсам, которые были ранее делегированы.

## <a name="service-providers"></a>Поставщики услуг

Пользователи в управляющем клиенте могут удалить доступ к делегированным ресурсам, если им была предоставлена [роль назначения регистрации управляемых служб удаление роли](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) для ресурсов клиента. Если эта роль не назначена каким-либо пользователям поставщика услуг, делегирование может быть удалено только пользователем в клиенте клиента.

В приведенном ниже примере показано назначение, предоставляющее **роль Delete назначения регистрации управляемых служб** , которую можно включить в файл параметров в процессе [адаптации](onboard-customer.md):

```json
    "authorizations": [ 
        { 
            "principalId": "cfa7496e-a619-4a14-a740-85c5ad2063bb", 
            "principalIdDisplayName": "MSP Operators", 
            "roleDefinitionId": "91c1777a-f3dc-4fae-b103-61d183457e46" 
        } 
    ] 
```

Эту роль можно также выбрать в **авторизации** при [создании предложения управляемой службы](../../marketplace/partner-center-portal/create-new-managed-service-offer.md#authorization) для публикации в Azure Marketplace.

Пользователь с этим разрешением может удалить делегирование одним из следующих способов.

### <a name="azure-portal"></a>Портал Azure

1. Перейдите на страницу [Мои клиенты](view-manage-customers.md).
2. Выберите **делегирования**.
3. Найдите делегирование, которое требуется удалить, а затем щелкните значок корзины, который отображается в строке.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory 

Login-AzAccount

# Select the subscription that is delegated - or contains the delegated resource group(s)

Select-AzSubscription -SubscriptionName "<subscriptionName>"

# Get the registration assignment

Get-AzManagedServicesAssignment -Scope "/subscriptions/{delegatedSubscriptionId}"

# Delete the registration assignment

Remove-AzManagedServicesAssignment -ResourceId "/subscriptions/{delegatedSubscriptionId}/providers/Microsoft.ManagedServices/registrationAssignments/{assignmentGuid}"
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory

az login

# Select the subscription that is delegated – or contains the delegated resource group(s)

az account set -s <subscriptionId/name>

# List registration assignments

az managedservices assignment list

# Delete the registration assignment

az managedservices assignment delete --assignment <id or full resourceId>
```

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь со сведениями о [делегированном управлении ресурсами Azure](../concepts/azure-delegated-resource-management.md).
- [Просматривайте клиентов и управляйте ими](view-manage-customers.md), перейдя в раздел **Мои клиенты** на портале Azure.
