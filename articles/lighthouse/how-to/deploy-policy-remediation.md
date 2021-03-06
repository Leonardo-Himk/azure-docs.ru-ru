---
title: Развертывание политики, которую можно исправить
description: Узнайте, как подключить клиента к системе делегированного управления ресурсами Azure, предоставив доступ к ресурсам и возможность управления ими через собственный клиент.
ms.date: 10/11/2019
ms.topic: conceptual
ms.openlocfilehash: b625e9e3c96866cfbc655a55b770c9ac07a626bd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80985173"
---
# <a name="deploy-a-policy-that-can-be-remediated-within-a-delegated-subscription"></a>Развертывание политики, которую можно исправить в рамках делегированной подписки

[Azure Lighthouse](../overview.md) позволяет поставщикам служб создавать и редактировать определения политики в рамках делегированной подписки. Однако для развертывания политик, использующих [задачу по исправлению](../../governance/policy/how-to/remediate-resources.md) (то есть политики с [deployIfNotExists](../../governance/policy/concepts/effects.md#deployifnotexists) или [изменением](../../governance/policy/concepts/effects.md#modify) ), необходимо создать [управляемое удостоверение](../../active-directory/managed-identities-azure-resources/overview.md) в клиенте клиента. Это управляемое удостоверение может использоваться политикой Azure для развертывания шаблона в политике. Для включения этого сценария, как при подключении клиента к управлению делегированными ресурсами Azure, так и при развертывании самой политики необходимо выполнять некоторые шаги.

## <a name="create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant"></a>Создание пользователя, который может назначать роли управляемому удостоверению в клиенте клиента

При подключении клиента к управлению делегированными ресурсами Azure используется [шаблон Azure Resource Manager](onboard-customer.md#create-an-azure-resource-manager-template) с файлом параметров, определяющим пользователей, группы пользователей и субъекты-службы в управляющем клиенте, который будет иметь доступ к делегированным ресурсам в клиенте клиента. В файле параметров каждому из этих пользователей (**principalId**) назначается [встроенная роль](../../role-based-access-control/built-in-roles.md) (**roleDefinitionId**), определяющая уровень доступа.

Чтобы разрешить **principalId** создать управляемое удостоверение в клиенте клиента, необходимо задать для его **roleDefinitionId** значение **Администратор доступа пользователя**. Хотя эта роль обычно не поддерживается, ее можно использовать в этом конкретном сценарии, позволяя пользователям с этим разрешением назначать одну или несколько конкретных встроенных ролей управляемым удостоверениям. Эти роли определяются в свойстве **delegatedRoleDefinitionIds**. Сюда можно включить любую встроенную роль, кроме администратора доступа пользователей или владельца.

После подключения клиента **principalId**, созданный в этой авторизации, сможет назначать эти встроенные роли управляемым удостоверениям в клиенте клиента. Однако у них не будет никаких других разрешений, обычно связанных с ролью администратора доступа пользователей.

В следующем примере показан **principalId**, которому будет назначена роль администратора доступа пользователей. Этот пользователь будет иметь возможность назначать две встроенные роли управляемым удостоверениям в клиенте клиента: участник и Log Analytics участник.

```json
{
    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
    "principalIdDisplayName": "Policy Automation Account",
    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "delegatedRoleDefinitionIds": [
         "b24988ac-6180-42a0-ab88-20f7382dd24c",
         "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
    ]
}
```

## <a name="deploy-policies-that-can-be-remediated"></a>Развертывание политик, которые можно исправить

После создания пользователя с необходимыми разрешениями, как описано выше, этот пользователь может развернуть политики в клиенте клиента, использующем задачи по исправлению.

Например, предположим, что вы хотите включить диагностику для Azure Key Vault ресурсов в клиенте клиента, как показано в этом [примере](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring). Пользователь в управляющем клиенте с соответствующими разрешениями (как описано выше) развернет [шаблон Azure Resource Manager](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/policy-enforce-keyvault-monitoring/enforceAzureMonitoredKeyVault.json), чтобы включить этот сценарий.

Обратите внимание, что создание назначения политики для использования с делегированной подпиской в настоящее время должно осуществляться через API, а не на портале Azure. При этом для **apiVersion** должно быть задано **2019-04-01-preview**, включающее новое свойство **DelegatedManagedIdentityResourceId**. Это свойство позволяет включить управляемое удостоверение, которое находится в клиенте клиента (в подписке или группе ресурсов, подключенной к управлению делегированными ресурсами Azure).

В следующем примере показано назначение роли с **delegatedManagedIdentityResourceId**.

```json
"type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2019-04-01-preview",
            "name": "[parameters('rbacGuid')]",
            "dependsOn": [
                "[variables('policyAssignment')]"
            ],
            "properties": {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', variables('rbacContributor'))]",
                "principalType": "ServicePrincipal",
                "delegatedManagedIdentityResourceId": "[concat(subscription().id, '/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment'))]",
                "principalId": "[toLower(reference(concat('/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment')), '2018-05-01', 'Full' ).identity.principalId)]"
            }
```

> [!TIP]
> Доступен [аналогичный пример](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag), демонстрирующий, как развернуть политику, которая добавляет или удаляет тег (используя эффект изменения) в делегированной подписке.

## <a name="next-steps"></a>Дальнейшие шаги

- Сведения о службе [Политика Azure](../../governance/policy/index.yml).
- Сведения об [управляемых удостоверениях для ресурсов Azure](../../active-directory/managed-identities-azure-resources/overview.md).
