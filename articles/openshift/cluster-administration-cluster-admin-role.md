---
title: Роль администратора кластера Azure Red Hat OpenShift | Документация Майкрософт
description: Назначение и использование роли администратора кластера Azure Red Hat OpenShift
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: ae9a421a165d6c8bda688819c5233ae5bb1a8562
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79139102"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Роль администратора клиента Azure Red Hat OpenShift

Вы являетесь администратором кластера Azure Red Hat OpenShift. Ваша учетная запись обладает повышенными разрешениями и доступом ко всем создаваемым пользователями проектам.

Если с учетной записью связана роль авторизации "клиент-Администратор-кластер", она может автоматически управлять проектом.

> [!Note] 
> Роль кластера "клиент-Администратор-кластер" отличается от роли кластера "Администратор кластера".

Например, можно выполнять действия, связанные с набором глаголов (`create`), для работы с набором имен ресурсов (`templates`). Чтобы просмотреть подробные сведения об этих ролях и их наборах команд и ресурсов, выполните следующую команду:

`$ oc get clusterroles customer-admin-cluster -o yaml`

Имена команд не обязательно сопоставляются непосредственно с `oc` командами. Они более широко соответствуют типам операций CLI, которые можно выполнять. 

Например, наличие `list` глагола означает, что можно отобразить список всех объектов имени ресурса (`oc get`). `get` Команда означает, что можно отобразить сведения об определенном объекте, если известно его имя (`oc describe`).

## <a name="configure-the-customer-administrator-role"></a>Настройка роли администратора клиента

Вы можете настроить роль кластера "клиент-Администратор-кластер" только во время создания кластера, указав `--customer-admin-group-id`флаг. Это поле сейчас не настраивается в портал Azure. Сведения о настройке Azure Active Directory и группы администраторов см. в статье [интеграция Azure Active Directory для Azure Red Hat OpenShift](howto-aad-app-configuration.md).

## <a name="confirm-membership-in-the-customer-administrator-role"></a>Подтверждение членства в роли "Администратор клиента"

Чтобы подтвердить членство в группе администраторов клиентов, попробуйте использовать команды `oc get nodes` интерфейса командной строки OpenShift или `oc projects`. `oc get nodes`отобразит список узлов, если у вас есть роль "клиент-Администратор-кластер" и ошибка разрешения, если у вас только роль "клиент-Администратор-проект". `oc projects`Отображает все проекты в кластере, а не только проекты, в которых вы работаете.

Для дальнейшего изучения ролей и разрешений в кластере можно использовать [`oc policy who-can <verb> <resource>`](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_rbac.html#managing-role-bindings) команду.

## <a name="next-steps"></a>Дальнейшие шаги

Настройте роль "клиент-Администратор-кластер кластера":
> [!div class="nextstepaction"]
> [Интеграция Azure Active Directory для Azure Red Hat OpenShift](howto-aad-app-configuration.md)
