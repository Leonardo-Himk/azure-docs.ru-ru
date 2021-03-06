---
title: Изменение пароля учетной записи соединителя Azure AD | Документация Майкрософт
description: В этом разделе описывается, как восстановить учетную запись соединителя Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 7f6b623cc5f864106dc2f119308370e80014a4c2
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611030"
---
# <a name="change-the-azure-ad-connector-account-password"></a>Изменение пароля учетной записи соединителя Azure AD
Учетная запись соединителя Azure AD должна быть бесплатной. Если требуется сбросить ее учетные данные, в этой статье вы найдете необходимые сведения. Например, если глобальный администратор по ошибке сбрасывает пароль учетной записи с помощью PowerShell.

## <a name="reset-the-credentials"></a>Сброс учетных данных
Если учетной записи соединителя Azure AD не удается связаться с Azure AD из-за проблем с проверкой подлинности, пароль можно сбросить.

1. Войдите на сервер синхронизации Azure AD Connect и запустите PowerShell.
2. Запустите `Add-ADSyncAADServiceAccount`.
   ![Командлет PowerShell addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Введите учетные данные глобального администратора Azure AD.

Этот командлет сбрасывает пароль для учетной записи службы и обновляет его в Azure AD и в модуле синхронизации.

## <a name="known-issues-these-steps-can-solve"></a>Известные проблемы, которые можно решить с помощью описанных действий
В этом разделе приведен список ошибок, о которых сообщили клиенты, которые были исправлены учетными данными, сброшенными в учетной записи соединителя Azure AD.

---
Событие 6900 на сервере произошла непредвиденная ошибка при обработке уведомления об изменении пароля: AADSTS70002: ошибка при проверке учетных данных. AADSTS50054: старый пароль используется для проверки подлинности.

---
Событие 659 ошибка при получении конфигурации синхронизации политики паролей. Microsoft. IdentityModel. Clients. ActiveDirectory. Адалсервицеексцептион: AADSTS70002: ошибка при проверке учетных данных. AADSTS50054: старый пароль используется для проверки подлинности.

## <a name="next-steps"></a>Дальнейшие действия
**Обзорные статьи**

* [Службы синхронизации Azure AD Connect: общие сведений о синхронизации и ее настройка](how-to-connect-sync-whatis.md)
* [Интеграция локальных удостоверений с Azure Active Directory.](whatis-hybrid-identity.md)
