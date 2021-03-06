---
title: Учебник. Настройка матрицы приоритетов для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической инициализации и отзыва учетных записей пользователей в матрице приоритетов.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: a4598a99-3c98-4c14-86c2-95cc562e2439
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2019
ms.author: Zhchia
ms.openlocfilehash: 80ffaba6713027d216958e0be2cd4ae35a8d2d70
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77063447"
---
# <a name="tutorial-configure-priority-matrix-for-automatic-user-provisioning"></a>Учебник. Настройка матрицы приоритетов для автоматической подготовки пользователей

Цель этого руководства — продемонстрировать шаги, которые необходимо выполнить в матрице приоритетов и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической инициализации и отзыва пользователей и (или) групп в матрице приоритетов.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Сейчас этот соединитель предоставляется в общедоступной предварительной версии. Дополнительные сведения об общих условиях использования продуктов в предварительной версии см. в документе [Дополнительные условия использования Предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Предварительные условия

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* клиент Azure AD;
* [Клиент матрицы приоритета](https://appfluence.com/pricing/)
* Учетная запись пользователя в матрице приоритетов с разрешениями администратора.

## <a name="assign-users-to-priority-matrix"></a>Назначение пользователям матрицы приоритета

Azure Active Directory использует концепцию, называемую назначениями, чтобы определить, какие пользователи должны получать доступ к выбранным приложениям. В контексте автоматической подготовки учетных записей пользователей синхронизируются только пользователи и группы, назначенные приложению в Azure AD.

Перед настройкой и включением автоматической подготовки пользователей следует решить, каким пользователям и (или) группам в Azure AD требуется доступ к матрице приоритетов. После принятия решения эти пользователи и (или) группы можно назначить матрице приоритетов, следуя приведенным ниже инструкциям.

* [Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-priority-matrix"></a>Важные советы по назначению пользователей в матрицу приоритетов

* Рекомендуется назначить одного пользователя Azure AD в матрицу приоритетов для проверки конфигурации автоматической подготовки пользователей. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователю матрицы с приоритетом в диалоговом окне назначения необходимо выбрать допустимую роль конкретного приложения (если она доступна). Пользователи с ролью **доступа по умолчанию** исключаются из подготовки.

## <a name="set-up-priority-matrix-for-provisioning"></a>Настройка матрицы приоритетов для подготовки

Перед настройкой матрицы приоритетов для автоматической подготовки пользователей с помощью Azure AD необходимо получить сведения об инициализации из матрицы приоритетов.

1. Войдите в [консоль администрирования матрицы приоритета](https://sync.appfluence.com/accounts/login/?next=/accounts/provisioning).

3. Щелкните **маркер входа OAuth** для матрицы приоритетов

    ![Матрица приоритетов Добавление SCIM](media/priority-matrix-provisioning-tutorial/oauthlogin.png)

4. Нажмите кнопку " **получить новый токен** ". Скопируйте **строку токена**. Это значение будет указано в поле **секретный токен** на вкладке Подготовка матричного приложения с приоритетом в портал Azure. 

    ![Маркер создания матрицы приоритета](media/priority-matrix-provisioning-tutorial/token.png)

## <a name="add-priority-matrix-from-the-gallery"></a>Добавление матрицы приоритета из коллекции

Чтобы настроить матрицу приоритетов для автоматической подготовки пользователей с помощью Azure AD, необходимо добавить в список управляемых приложений SaaS матрицу приоритетов из коллекции приложений Azure AD.

1. В **[портал Azure](https://portal.azure.com)** на панели навигации слева выберите **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в раздел **корпоративные приложения**, а затем выберите **все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, нажмите кнопку **новое приложение** в верхней части области.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **матрицу приоритета**, выберите **матрицу приоритета** на панели результатов. 

    ![Матрица Priority в списке результатов](common/search-new-app.png)

5. Нажмите кнопку **Регистрация для создания приоритетной матрицы** , которая перенаправит вас на страницу входа в матрицу с приоритетом. 

    ![Матрица с приоритетом OIDC добавить](media/priority-matrix-provisioning-tutorial/signup.png)

6. Так как матрица с приоритетом является OpenIDConnect приложением, выберите вход в матрицу приоритетов с помощью рабочей учетной записи Майкрософт.

    ![Имя OIDC матрицы с приоритетом](media/priority-matrix-provisioning-tutorial/msftsignin.png)

7. После успешной проверки подлинности примите запрос согласия на страницу согласия. Приложение будет автоматически добавлено в клиент, и вы будете перенаправлены на учетную запись с матрицей приоритета.

    ![OIDc согласие на получение приоритета матрицы](media/priority-matrix-provisioning-tutorial/consent.png)

## <a name="configure-automatic-user-provisioning-to-priority-matrix"></a>Настройка автоматической подготовки пользователей в матрице приоритетов 

В этом разделе описано, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и (или) групп в матрице приоритетов на основе назначения пользователей и групп в Azure AD.

> [!NOTE]
> Дополнительные сведения о конечной точке SCIM в матрице приоритетов см. в статье [подготовка пользователей и приоритетная матрица](https://appfluence.com/help/article/user-provisioning/).

### <a name="to-configure-automatic-user-provisioning-for-priority-matrix-in-azure-ad"></a>Чтобы настроить автоматическую подготовку пользователей для матрицы приоритетов в Azure AD, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **корпоративные приложения**, а затем выберите **все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите элемент **Матрица приоритета**.

    ![Ссылка на матрицу приоритетов в списке "приложения"](common/all-applications.png)

3. Перейдите на вкладку **Подготовка** .

    ![Вкладка "подготовка"](common/provisioning.png)

4. Установите для **режима подготовки** значение **автоматически**.

    ![Вкладка "подготовка"](common/provisioning-automatic.png)

5. В разделе **учетные данные администратора** введите `https://sync.appfluence.com/scim/v2/` **URL-адрес клиента**. Введите значение, полученное и сохраненное ранее из матрицы приоритета в **маркере секрета**. Нажмите кнопку **проверить подключение** , чтобы убедиться, что Azure AD может подключиться к матрице приоритетов. В случае сбоя подключения убедитесь, что учетная запись матрицы приоритета имеет разрешения администратора, и повторите попытку.

    ![URL-адрес клиента + токен](common/provisioning-testconnection-tenanturltoken.png)

6. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Send an email notification when a failure occurs** (Отправить уведомление по электронной почте при сбое).

    ![Почтовое уведомление](common/provisioning-notification-email.png)

7. Нажмите кнопку **Сохранить**.

8. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей с приоритетной матрицей**.

    ![Сопоставление приоритетов пользователей матрицы](media/priority-matrix-provisioning-tutorial/usermappings.png)

9. Проверьте атрибуты пользователя, которые синхронизированы из Azure AD в матрицу приоритетов в разделе **сопоставление атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в матрице Priority для операций обновления. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

    ![Атрибуты пользователя матрицы приоритета](media/priority-matrix-provisioning-tutorial/userattributes.png)

10. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Чтобы включить службу подготовки Azure AD для матрицы приоритетов, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

    ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

12. Определите пользователей и (или) группы, которые вы хотите подготавливать к матрице приоритетов, выбрав нужные значения в **области** в разделе **Параметры** .

    ![Область действия подготовки](common/provisioning-scope.png)

13. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

После этого начнется начальная синхронизация пользователей и (или) групп, определенных в поле **Область** раздела **Параметры**. Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчетов по подготовке, в которых описаны все действия, выполняемые службой подготовки Azure AD по приоритетной матрице.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие шаги

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)


