---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/13/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 22f6f040d333a526940a688c16277be568520f50
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83651356"
---
|Имя |Description |Действие |Версия |GitHub |
|---|---|---|---|---|
|[Свойство ClusterProtectionLevel в кластерах Service Fabric должно иметь значение EncryptAndSign](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F617c02be-7f02-4efd-8836-3180d47b6c68) |Service Fabric поддерживает три уровня защиты (None (Нет), Sign (Подписывание) и EncryptAndSign (Шифрование и подписывание)) для взаимодействия между узлами с использованием основного сертификата кластера. Задайте уровень защиты, чтобы обеспечить шифрование и цифровое подписывание всех сообщений между узлами. |Audit, Deny, Disabled |1.1.0 |[Ссылка](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json) |
|[Кластеры Service Fabric должны использовать только Azure Active Directory для проверки подлинности клиента](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb54ed75b-3e1a-44ac-a333-05ba39b99ff0) |Аудит проверки подлинности клиента только через Azure Active Directory в Service Fabric |Audit, Deny, Disabled |1.1.0 |[Ссылка](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json) |
