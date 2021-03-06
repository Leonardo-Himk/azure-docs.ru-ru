---
title: Назначение роли RBAC для доступа к данным с помощью портал Azure
titleSuffix: Azure Storage
description: Узнайте, как использовать портал Azure для назначения разрешений участнику безопасности Azure Active Directory с помощью управления доступом на основе ролей (RBAC). Служба хранилища Azure поддерживает встроенные и настраиваемые роли RBAC для проверки подлинности с помощью Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/31/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: d224bd9e9e7b1f8fc9eb45d85e78811d8642fc78
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80519562"
---
# <a name="use-the-azure-portal-to-assign-an-rbac-role-for-access-to-blob-and-queue-data"></a>Используйте портал Azure, чтобы назначить роль RBAC для доступа к данным BLOB-объектов и очередей.

Azure Active Directory (Azure AD) разрешает права доступа к защищенным ресурсам с помощью [управления доступом на основе ролей (RBAC)](../../role-based-access-control/overview.md). Служба хранилища Azure определяет набор встроенных ролей RBAC, охватывающих общие наборы разрешений, используемых для доступа к данным BLOB-объектов или очередей.

Когда роль RBAC назначается субъекту безопасности Azure AD, Azure предоставляет доступ к этим ресурсам для этого субъекта безопасности. Доступ может ограничиваться уровнем подписки, группой ресурсов, учетной записью хранения или отдельным контейнером или очередью. Субъект безопасности Azure AD может быть пользователем, группой, субъектом-службой приложения или [управляемым удостоверением для ресурсов Azure](../../active-directory/managed-identities-azure-resources/overview.md).

В этой статье описывается, как использовать портал Azure для назначения ролей RBAC. Портал Azure предоставляет простой интерфейс для назначения ролей RBAC и управления доступом к ресурсам хранилища. Можно также назначать роли RBAC для ресурсов BLOB и очередей с помощью программ командной строки Azure или API управления хранилищем Azure. Дополнительные сведения о ролях RBAC для ресурсов хранилища см. [в статье Проверка подлинности доступа к BLOB-объектам и очередям Azure с помощью Azure Active Directory](storage-auth-aad.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>Роли RBAC для больших двоичных объектов и очередей

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Определение области действия ресурса

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Назначение ролей RBAC с помощью портал Azure

Определив соответствующую область для назначения ролей, перейдите к этому ресурсу в портал Azure. Отобразить параметры **управления доступом (IAM)** для ресурса и выполнить следующие инструкции для управления назначениями ролей:

1. Назначьте соответствующую роль RBAC хранилища Azure для предоставления доступа субъекту безопасности Azure AD.

1. Назначьте роль [модуля чтения](../../role-based-access-control/built-in-roles.md#reader) Azure Resource Manager пользователям, которым требуется доступ к контейнерам или очередям через портал Azure используя учетные данные Azure AD. 

В следующих разделах эти действия описаны более подробно.

> [!NOTE]
> Как владельцу учетной записи хранения Azure вам автоматически не назначаются разрешения на доступ к данным. Для службы хранилища Azure вы должны назначить себе роль RBAC явным образом. Вы можете назначить ее на уровне подписки, группы ресурсов, учетной записи хранения, контейнера или очереди.
>
> Невозможно назначить роль для контейнера или очереди, если в учетной записи хранения включено иерархическое пространство имен.

### <a name="assign-a-built-in-rbac-role"></a>Назначение встроенной роли RBAC

Перед назначением роли субъекту безопасности следует учитывать область предоставленных разрешений. Чтобы выбрать соответствующую область, ознакомьтесь с разделом [Определение области действия ресурса](#determine-resource-scope) .

В приведенной здесь процедуре назначается роль, ограниченная областью контейнера, но вы можете выполнить те же действия для назначения роли, ограниченной областью очереди:

1. В [портал Azure](https://portal.azure.com)перейдите к своей учетной записи хранения и отобразите **Обзор** учетной записи.
1. В разделе "Службы" выберите **BLOB-объекты**.
1. Найдите контейнер, для которого нужно назначить роль, и откройте его параметры.
1. Выберите **Управление доступом (IAM)**, чтобы отобразить параметры управления доступом для контейнера. Выберите вкладку **Назначения ролей**, чтобы просмотреть список назначений ролей.

    ![Снимок экрана, показывающий параметры управления доступом к контейнеру](media/storage-auth-aad-rbac-portal/portal-access-control-for-storage.png)

1. Нажмите кнопку **Добавить назначение ролей**, чтобы добавить новую роль.
1. В окне **Добавление назначения ролей** выберите роль службы хранилища Azure, которую необходимо назначить. Затем найдите субъект безопасности, которому нужно назначить эту роль.

    ![Снимок экрана, на котором показано, как назначить роль RBAC](media/storage-auth-aad-rbac-portal/add-rbac-role.png)

1. Нажмите кнопку **Сохранить**. Удостоверение, которому назначена роль RBAC, будет отображаться в списке под этой ролью. Например, на следующем изображении показано, что у добавленных пользователей теперь есть разрешения на чтение для данных в контейнере с именем *sample-container*.

    ![Снимок экрана, показывающий список пользователей, назначенных роли](media/storage-auth-aad-rbac-portal/container-scoped-role.png)

Вы можете выполнить аналогичные действия, чтобы назначить роль для учетной записи хранения, группы ресурсов или подписки.

### <a name="assign-the-reader-role-for-portal-access"></a>Назначение роли читателя для доступа к порталу

При назначении встроенной или пользовательской роли для службы хранилища Azure субъекту безопасности вы предоставляете разрешения на выполнение операций с данными в вашей учетной записи хранения для этого субъекта безопасности. Встроенные роли **чтения данных** предоставляют разрешения на чтение данных в контейнере или очереди, тогда как встроенные роли **участника данных** предоставляют разрешения на чтение, запись и удаление для контейнера или очереди. Разрешения ограничены указанным ресурсом.  
Например, если назначить роль **участника данных BLOB-объекта хранилища** пользователю Mary на уровне контейнера с именем **Sample-Container**, то Мэри предоставляет доступ на чтение, запись и удаление для всех больших двоичных объектов в этом контейнере.

Однако, если Мэри хочет просмотреть большой двоичный объект в портал Azure, то сама роль **участника данных BLOB-объекта хранилища** не будет предоставлять достаточные разрешения для перемещения по порталу в большой двоичный объект, чтобы его можно было просмотреть. Дополнительные разрешения Azure AD необходимы для навигации по порталу и просмотра других ресурсов, которые там видны.

Если пользователи должны иметь доступ к BLOB-объектам в портал Azure, назначьте им дополнительную роль RBAC, роль [читателя](../../role-based-access-control/built-in-roles.md#reader) для этих пользователей, на уровне учетной записи хранения или выше. Роль **читателя** — это роль Azure Resource Manager, которая позволяет пользователям просматривать ресурсы учетной записи хранения, но не может изменять их. Он не предоставляет разрешения на чтение данных в службе хранилища Azure, но только в ресурсы управления учетной записью.

Выполните следующие действия, чтобы назначить роль **читателя** , чтобы пользователь мог получить доступ к BLOB-объектам из портал Azure. В этом примере назначение ограничивается учетной записью хранения:

1. Войдите в свою учетную запись хранения на [портале Azure](https://portal.azure.com).
1. Выберите **Управление доступом (IAM)** , чтобы отобразить параметры управления доступом для учетной записи хранения. Выберите вкладку **Назначения ролей**, чтобы просмотреть список назначений ролей.
1. В окне **Добавление назначения ролей** выберите роль **читатель** . 
1. Из поля **Назначение доступа к** выберите **пользователя, группу или субъект-службу Azure AD**.
1. Найдите субъекта безопасности, которому нужно назначить роль.
1. Сохраните назначение роли.

Назначение роли **читателя** необходимо только для пользователей, которым необходим доступ к BLOB-объектам или очередям с помощью портал Azure.

> [!IMPORTANT]
> Предварительная версия Обозреватель службы хранилища в портал Azure не поддерживает использование учетных данных Azure AD для просмотра и изменения данных BLOB-объектов или очередей. Обозреватель службы хранилища в портал Azure всегда использует ключи учетной записи для доступа к данным. Чтобы использовать Обозреватель службы хранилища в портал Azure, необходимо назначить роль, включающую **Microsoft. Storage/storageAccounts/listkeys/Action**.

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о ролях RBAC для ресурсов хранилища см. [в статье Проверка подлинности доступа к BLOB-объектам и очередям Azure с помощью Azure Active Directory](storage-auth-aad.md). 
- Дополнительные сведения см. в статье [Что такое управление доступом на основе ролей (RBAC)?](../../role-based-access-control/overview.md)
- Чтобы узнать, как назначать роли RBAC и управлять назначениями ролей с помощью Azure PowerShell, Azure CLI или REST API, см. сведения в статьях ниже:
    - [Управление доступом на основе ролей с помощью Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
    - [Управление доступом на основе ролей с помощью интерфейса командной строки Azure](../../role-based-access-control/role-assignments-cli.md)
    - [Управление доступом на основе ролей с помощью REST API](../../role-based-access-control/role-assignments-rest.md)
- Чтобы узнать об авторизации доступа к контейнерам и очередям из приложений службы хранилища, ознакомьтесь со статьей об [аутентификации с помощью Azure Active Directory из приложения службы хранилища Azure (предварительная версия)](storage-auth-aad-app.md).
