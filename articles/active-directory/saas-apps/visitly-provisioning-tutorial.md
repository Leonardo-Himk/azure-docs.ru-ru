---
title: Руководство. Настройка службы автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической подготовкой и отмене предоставления учетных записей пользователей для посещения.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fb48deae-4653-448a-ba2f-90258edab3a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2019
ms.author: Zhchia
ms.openlocfilehash: 73cc1a58689db7902843f222aa4874a5e188be44
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77063175"
---
# <a name="tutorial-configure-visitly-for-automatic-user-provisioning"></a>Учебник. Настройка посещения автоматической подготовки пользователей

Цель этого руководства — продемонстрировать шаги, выполняемые в центре справки и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической подготовки и отмены наполнения пользователей или групп для посещения.

> [!NOTE]
> В этом руководстве описывается соединитель, созданный на основе службы подготовки пользователей Azure AD. Важные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. [в разделе Автоматизация подготовки пользователей и ее отмене в приложениях программного обеспечения как услуги (SaaS) с Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Сейчас этот соединитель предоставляется в общедоступной предварительной версии. Дополнительные сведения об общих Microsoft Azure условиях использования предварительных версий функций см. в разделе Дополнительные [условия использования для предварительного просмотра Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Предварительные условия

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* клиент Azure AD;
* [Посещаемый клиент](https://www.visitly.io/pricing/)
* Учетная запись пользователя в центре администрирования с разрешениями администратора

## <a name="assign-users-to-visitly"></a>Назначение пользователей для посещения 

Azure Active Directory использует концепцию, называемую *назначениями* , чтобы определить, какие пользователи должны получать доступ к выбранным приложениям. В контексте автоматической подготовки учетных записей пользователей синхронизируются только те пользователи или группы, которые были назначены приложению в Azure AD.

Перед настройкой и включением автоматической подготовки пользователей решите, какие пользователи или группы в Azure AD нуждаются в доступе. Затем назначьте этих пользователей или группы, следуя приведенным ниже инструкциям.
* [Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-visitly"></a>Важные советы по назначению пользователей для посещения 

* Рекомендуется назначить одного пользователя Azure AD для посещения, чтобы протестировать конфигурацию автоматической подготовки пользователей. Дополнительные пользователи или группы могут быть назначены позже.

* При назначении пользователя для посещения в диалоговом окне Назначение необходимо выбрать допустимую роль для конкретного приложения (если она доступна). Пользователи с ролью доступа по умолчанию исключаются из подготовки.

## <a name="set-up-visitly-for-provisioning"></a>Настройка для посещения

Перед настройкой автоматической подготовки пользователей с помощью Azure AD необходимо включить систему для подготовки на основе междоменного управления удостоверениями (SCIM).

1. Войдите в систему, чтобы [посетить](https://app.visitly.io/login). Выберите **интеграции** > **узел Синхронизация**.

    ![Синхронизация узла](media/Visitly-provisioning-tutorial/login.png)

2. Выберите раздел **Azure AD** .

    ![Раздел Azure AD](media/Visitly-provisioning-tutorial/integration.png)

3. Скопируйте **ключ API**. Эти значения указаны в поле **секретный токен** на вкладке **подготовка** приложения в портал Azure.

    ![Ключ API](media/Visitly-provisioning-tutorial/token.png)


## <a name="add-visitly-from-the-gallery"></a>Добавление посещения из коллекции

Чтобы настроить центр автоматической подготовки пользователей с помощью Azure AD, добавьте посетить веб – сайт из коллекции приложений Azure AD в список управляемых приложений SaaS.

Чтобы добавить посещения из коллекции приложений Azure AD, выполните следующие действия.

1. В [портал Azure](https://portal.azure.com)в области навигации слева выберите **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в раздел **корпоративные приложения**, а затем выберите **все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, нажмите кнопку **новое приложение** в верхней части области.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите " **посетить**", выберите " **посетить** " на панели результатов и нажмите кнопку " **добавить** ", чтобы добавить приложение.

    ![Visitly в списке результатов](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-visitly"></a>Настройка автоматической подготовки пользователей для посещения 

В этом разделе описано, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей или групп в центре в соответствии с назначениями пользователей или групп в Azure AD.

> [!TIP]
> Чтобы включить единый вход на основе SAML, следуйте инструкциям в [руководстве по использованию единого входа](Visitly-tutorial.md). Единый вход можно настроить независимо от автоматической подготовки пользователей, хотя эти две функции дополняют друг друга.

### <a name="configure-automatic-user-provisioning-for-visitly-in-azure-ad"></a>Настройка автоматической подготовки пользователей для посещения в Azure AD

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **корпоративные приложения** > **все приложения**.

    ![Все приложения](common/enterprise-applications.png)

2. В списке приложений выберите **Visitly**.

    ![Ссылка на Visitly в списке приложений](common/all-applications.png)

3. Перейдите на вкладку **Подготовка** .

    ![Вкладка "подготовка"](common/provisioning.png)

4. Установите для **режима подготовки** значение **автоматически**.

    ![Режим подготовки установлен в значение "автоматически"](common/provisioning-automatic.png)

5. В разделе Учетные данные администратора введите значения `https://api.visitly.io/v1/usersync/SCIM` **ключей API** и, полученные ранее в поле **URL-адрес клиента** и **секретный токен**соответственно. Выберите **проверить подключение** , чтобы убедиться, что Azure AD может подключиться для посещения. Если подключение не удается выполнить, убедитесь, что у учетной записи пользователя есть разрешения администратора, и повторите попытку.

    ![URL-адрес клиента + токен](common/provisioning-testconnection-tenanturltoken.png)

6. В поле e-mail **Notification (уведомление** ) введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки. Установите флажок **отправлять уведомление по электронной почте при возникновении сбоя** .

    ![Уведомление по электронной почте](common/provisioning-notification-email.png)

7. Нажмите кнопку **Сохранить**.

8. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей для посещения**.

    ![Посещаемые пользователем сопоставления](media/visitly-provisioning-tutorial/usermapping.png)

9. Просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD, чтобы посетить раздел " **сопоставления атрибутов** ". Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в центре обновления. Чтобы зафиксировать изменения, щелкните **Сохранить**.

    ![Пользовательские атрибуты](media/visitly-provisioning-tutorial/userattribute.png)

10. Чтобы настроить фильтры области, следуйте инструкциям в [руководстве по фильтрации областей](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Чтобы включить службу подготовки Azure AD для посещения, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

    ![Состояние подготовки выключено](common/provisioning-toggle-on.png)

12. Определите пользователей или группы, которые нужно подготавливать для посещения, выбрав нужные значения в **области** в разделе **Параметры** .

    ![Область действия подготовки](common/provisioning-scope.png)

13. Когда вы будете готовы к подготовке к работе, нажмите кнопку **сохранить**.

    ![Идет сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

Эта операция запускает начальную синхронизацию всех пользователей или групп, определенных в **области** , в разделе **параметров** . Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Дополнительные сведения о том, сколько времени требуется для подсчета пользователей или групп, см. [в разделе сколько времени потребуется для предоставления пользователям?](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users).

Вы можете использовать **Текущий раздел состояния** для отслеживания хода выполнения и перехода по ссылкам к отчету о действиях по подготовке, в котором описаны все действия, выполняемые службой подготовки Azure AD. Дополнительные сведения см. [в разделе Проверка состояния подготовки пользователей](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md). Чтобы прочитать журналы подготовки Azure AD, см. раздел [Создание отчетов об автоматической подготовке учетных записей пользователей](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Ограничения соединителя

Не поддерживает жесткие удаления. Все это обратимое удаление.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие шаги

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)
