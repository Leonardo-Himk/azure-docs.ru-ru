---
title: Настройка входа для Организации Azure AD
titleSuffix: Azure AD B2C
description: Настройка входа для определенной организации Azure Active Directory в Azure Active Directory B2C
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 5b21fcd2d3ec5560b01352b112e9ed1bb2404766
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678033"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Настройка входа для определенной организации Azure Active Directory в Azure Active Directory B2C

Чтобы использовать Azure Active Directory (Azure AD) в качестве [поставщика удостоверений](authorization-code-flow.md) для Azure AD B2C, необходимо создать приложение, которое будет представлять этого поставщика. В этой статье описывается, как включить вход для пользователей из определенной организации Azure AD с помощью потока пользователя в Azure AD B2C.

[!INCLUDE [active-directory-b2c-identity-provider-azure-ad](../../includes/active-directory-b2c-identity-provider-azure-ad.md)]

## <a name="configure-azure-ad-as-an-identity-provider"></a>Настройка Azure AD в качестве поставщика удостоверений для клиента

1. Убедитесь, что вы используете каталог, содержащий клиент Azure AD B2C. В верхнем меню выберите фильтр **каталог и подписка** и выберите каталог, содержащий клиент Azure AD B2C.
1. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
1. Выберите **поставщики удостоверений**, а затем выберите **Новый поставщик OpenID Connect Connect**.
1. Введите **имя**. Например, введите *Azure AD Contoso*.
1. В поле **URL-адрес метаданных**введите следующий URL `{tenant}` -адрес, заменяющий доменное имя вашего клиента Azure AD:

    ```
    https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
    ```

    Например, `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`.

1. В поле **идентификатор клиента**введите ранее записанный идентификатор приложения.
1. В поле **секрет клиента**введите секрет клиента, который вы записали ранее.
1. Для **области**введите `openid profile`.
1. Оставьте значения по умолчанию для **типа ответа**и **режим ответа**.
1. Используемых В качестве **подсказки для домена**введите `contoso.com`. Дополнительные сведения см. в статье [Настройка прямого входа в систему с помощью Azure Active Directory B2C](direct-signin.md#redirect-sign-in-to-a-social-provider).
1. В разделе **сопоставление утверждений поставщика удостоверений**выберите следующие утверждения:

    * **Идентификатор пользователя**: *OID*
    * **Отображаемое имя**: *имя*
    * **Заданное имя**: *given_name*
    * **Фамилия**: *family_name*
    * **Адрес электронной почты**: *unique_name*

1. Нажмите кнопку **Сохранить**.
