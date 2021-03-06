---
title: Закрытие рабочей или учебной учетной записи в неуправляемой Организации Azure AD
description: Как закрыть рабочую или учебную учетную запись в неуправляемой Azure Active Directory.
services: active-directory
author: rolyon
manager: mtillman
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 05/20/2019
ms.author: rolyon
ms.reviewer: ''
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11c337f4838279771523a1f375b7349387d68f45
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82582537"
---
# <a name="close-your-work-or-school-account-in-an-unmanaged-azure-ad-organization"></a>Закрытие рабочей или учебной учетной записи в неуправляемой Организации Azure AD

Если вы являетесь пользователем в неуправляемой Организации Azure Active Directory (Azure AD) и вам больше не нужно использовать приложения из этой организации или не хотите поддерживать связь с ней, вы можете закрыть учетную запись в любое время. Неуправляемая организация не имеет глобального администратора. Пользователи в неуправляемой организации могут закрыть свои учетные записи самостоятельно, не обращаясь к администратору.

Пользователи в неуправляемой организации часто создаются во время самостоятельной регистрации. Примером может быть информационный работник в Организации, который подписывается на бесплатную службу. Дополнительные сведения о самостоятельной регистрации см. в разделе [что такое самостоятельная регистрация для Azure Active Directory?](directory-self-service-signup.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="before-you-begin"></a>Перед началом

Прежде чем можно будет закрыть учетную запись, необходимо подтвердить следующие элементы:

* Убедитесь, что вы являетесь пользователем неуправляемой Организации Azure AD. Вы не можете закрыть учетную запись, если вы принадлежите к управляемой организации. Если вы принадлежите к управляемой организации и хотите закрыть учетную запись, обратитесь к администратору. Сведения о том, как определить, принадлежит ли вы к неуправляемой Организации, см. [в разделе Удаление пользователя из неуправляемого клиента](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant).

* Сохраните все данные, которые нужно сохранить. Сведения о том, как отправить запрос на экспорт, см. в разделе [доступ к журналам, созданным системой, и их экспорт для неуправляемых клиентов](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants).

> [!WARNING]
> Закрытие учетной записи необратимо. При закрытии учетной записи все личные данные будут удалены. У вас больше не будет доступа к вашей учетной записи и данным, связанным с вашей учетной записью.

## <a name="close-your-account"></a>Закрытие учетной записи

Чтобы закрыть неуправляемую рабочую или учебную учетную запись, выполните следующие действия.

1. Войдите, чтобы [Закрыть учетную запись](https://go.microsoft.com/fwlink/?linkid=873123), используя учетную запись, которую нужно закрыть.

1. В **окне Мои запросы к данным**выберите **Закрыть учетную запись**.

    ![Мои запросы к данным — закрытие учетной записи](./media/users-close-account/close-account.png)

1. Просмотрите сообщение с подтверждением и нажмите кнопку **Да**.

    ![Мои запросы к данным — подтвердить закрытие](./media/users-close-account/confirm-close.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Что такое самостоятельная регистрация для Azure Active Directory?](directory-self-service-signup.md)
- [Удаление пользователя из неуправляемого клиента](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant)
- [Доступ к системным журналам для неуправляемых клиентов и их экспорт](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants)
