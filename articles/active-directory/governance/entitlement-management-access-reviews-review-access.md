---
title: Проверка доступа к пакету Access в управлении назначениями Azure AD
description: Узнайте, как выполнить проверку доступа к пакетам доступа для управления обслуживанием в Azure Active Directory проверках доступа (Предварительная версия).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 11/01/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99de022b7259b33baab3aa825673a8f85e932bff
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78968746"
---
# <a name="review-access-of-an-access-package-in-azure-ad-entitlement-management"></a>Проверка доступа к пакету Access в управлении назначениями Azure AD

Управление назначением Azure AD упрощает управление доступом предприятия к группам, приложениям и сайтам SharePoint. В этой статье описывается, как выполнять проверки доступа для других пользователей, которым назначен пакет доступа, в качестве назначенного рецензента.

## <a name="prerequisites"></a>Предварительные требования

Чтобы проверить назначения пакетов Active Access для пользователей, необходимо выполнить необходимые условия для проверки доступа:
- Azure AD Premium P2
- Глобальный администратор
- Назначенный администратор пользователей, владелец каталога или диспетчер пакетов Access

Дополнительные сведения см. в статье [Лицензионные требования](entitlement-management-overview.md#license-requirements).


## <a name="open-the-access-review"></a>Открытие проверки доступа

Чтобы найти и открыть проверку доступа, выполните следующие действия.

1. Вы можете получить сообщение электронной почты от корпорации Майкрософт, предлагающее ознакомиться с доступом. Перейдите по адресу электронной почты, чтобы открыть проверку доступа. Ниже приведен пример электронного письма для проверки доступа:
    
    ![Электронная почта рецензента проверки доступа](./media/entitlement-management-access-reviews-review-access/review-access-reviewer-email.png)

1. Щелкните ссылку **проверить доступ пользователя** , чтобы открыть проверку доступа. 

1. Если у вас нет электронного письма, вы можете найти незавершенные проверки доступа, перейдя https://myaccess.microsoft.comнепосредственно к.  (Для государственных организаций США используйте `https://myaccess.microsoft.us` вместо него.)

1. Щелкните проверки **доступа** на левой панели навигации, чтобы просмотреть список незавершенных проверок доступа, назначенных вам.
    
    ![Выбор проверок доступа при доступе](./media/entitlement-management-access-reviews-review-access/review-access-myaccess-select-access-review.png)

1. Щелкните проверку, которую вы хотите начать.
    
    ![Выбор проверки доступа](./media/entitlement-management-access-reviews-review-access/review-access-select-access-review.png)

## <a name="perform-the-access-review"></a>Выполнение проверки доступа

После открытия проверки доступа вы увидите имена пользователей, которых необходимо просмотреть. Существует два способа утверждения или запрета доступа:
- Вы можете вручную утвердить или отклонить доступ для одного или нескольких пользователей.
- Вы можете принять рекомендации по системе

### <a name="manually-approve-or-deny-access-for-one-or-more-users"></a>Утверждение или запрет доступа для одного или нескольких пользователей вручную
1. Проверьте список пользователей и определите, какие пользователи должны продолжать получать доступ.

    ![Список пользователей для проверки](./media/entitlement-management-access-reviews-review-access/review-access-list-of-users.png)

1. Чтобы утвердить или отклонить доступ, установите переключатель слева от имени пользователя.

1. На панели над именами пользователей выберите **утвердить** или **отклонить** .

    ![Выбор пользователя](./media/entitlement-management-access-reviews-review-access/review-access-select-users.png)

1. Если вы не уверены, можно нажать кнопку **не знаете** .

    Если выбрать этот вариант, пользователь сохранит доступ, и этот выбор будет выполняться в журналах аудита. В журнале отображаются все другие рецензенты, которые по-прежнему прошли проверку.

1. Может потребоваться указать причину для вашего решения. Введите причину и нажмите кнопку **Отправить**.

    ![Утверждение или отклонение доступа](./media/entitlement-management-access-reviews-review-access/review-access-decision-approve.png)

1. Вы можете изменить свое решение в любое время до завершения проверки. Для этого выберите пользователя из списка и измените решение. Например, можно утвердить доступ для ранее отклоненного пользователя.

При наличии нескольких рецензентов записывается последний отправленный ответ. Рассмотрим пример, в котором администратор назначает два рецензента — Алиса и Боб. Алиса сначала открывает проверку и утверждает доступ. Перед окончанием проверки Боб откроет проверку и отказывает в доступе. В этом случае записывается Последнее решение запрета доступа.

>[!NOTE]
>Если пользователю отказано в доступе, он сразу не удаляется из пакета Access. Пользователь будет удален из пакета Access после завершения проверки, или Администратор завершит проверку.

### <a name="approve-or-deny-access-using-the-system-generated-recommendations"></a>Утверждение или отклонение доступа с помощью созданных системой рекомендаций

Чтобы быстрее просматривать доступ для нескольких пользователей, можно использовать созданные системой рекомендации, принимая рекомендации одним щелчком. Рекомендации создаются на основе действия входа пользователя.

1.  На панели в верхней части страницы щелкните **принять рекомендации**.
    
    ![Выбор принятия рекомендаций](./media/entitlement-management-access-reviews-review-access/review-access-use-recommendations.png)
    
    Вы увидите сводку рекомендуемых действий.

1.  Нажмите кнопку **Отправить** , чтобы принять рекомендации.

## <a name="next-steps"></a>Следующие шаги

- [Самостоятельный обзор пакетов доступа](entitlement-management-access-reviews-self-review.md)