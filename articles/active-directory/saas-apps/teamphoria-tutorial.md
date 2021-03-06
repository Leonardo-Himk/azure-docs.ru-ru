---
title: Руководство по интеграции единого входа Azure Active Directory с Teamphoria | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Teamphoria.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/09/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2631b34f5658c9d4f76ca26d378bc63fe59ad156
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "72373269"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-teamphoria"></a>Руководство по интеграции единого входа Azure Active Directory с Teamphoria

В этом учебнике описано, как интегрировать приложение Teamphoria с Azure Active Directory (Azure AD). Интеграция Teamphoria с Azure AD обеспечивает следующие возможности:

* С помощью Azure AD вы можете контролировать доступ к Teamphoria.
* Вы можете включить автоматический вход пользователей в Teamphoria с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Teamphoria с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Teamphoria поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-teamphoria-from-the-gallery"></a>Добавление Teamphoria из коллекции

Чтобы настроить интеграцию Teamphoria с Azure AD, необходимо добавить Teamphoria из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Teamphoria**.
1. Выберите **Teamphoria** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-teamphoria"></a>Настройка и проверка единого входа Azure AD для Teamphoria

Настройте и проверьте единый вход Azure AD в Teamphoria с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Teamphoria.

Чтобы настроить и проверить единый вход Azure AD в Teamphoria, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Teamphoria](#configure-teamphoria-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Teamphoria](#create-teamphoria-test-user)** требуется для того, чтобы в Teamphoria существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Teamphoria** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<sub-domain>.teamphoria.com/login`.

    > [!NOTE]
    > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [группу поддержки клиентов Teamphoria](https://www.teamphoria.com/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемый URL-адрес можно скопировать из раздела **Настройка Teamphoria**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к Teamphoria.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Teamphoria**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-teamphoria-sso"></a>Настройка единого входа в Teamphoria

1. Для автоматизации настройки в Teamphoria необходимо установить **расширение браузера "Безопасный вход в мои приложения"** , щелкнув **Установить расширение**.

    ![Расширение "Мои приложения"](common/install-myappssecure-extension.png)

2. После добавления расширения в браузер щелкните **Set up Teamphoria** (Настроить Teamphoria), чтобы перейти в приложение Teamphoria. После этого укажите учетные данные администратора для входа в Teamphoria. Расширение браузера автоматически настроит приложение и автоматизирует шаги 3–6.

    ![Настройка конфигурации](common/setup-sso.png)

3. Если вы хотите настроить Teamphoria вручную, откройте новое окно веб-браузера, войдите на свой корпоративный сайт Teamphoria в качестве администратора и выполните следующие действия:

4. Перейдите к пункту **ADMIN SETTINGS** (Параметры администратора) на панели слева инструментов и на вкладке Configure (Настройка) щелкните **SINGLE SIGN-ON** (Единый вход), чтобы открыть окно настройки единого входа.

    ![Настройка единого входа](./media/teamphoria-tutorial/admin_sso_configure.png)

5. Щелкните параметр **ADD NEW IDENTITY PROVIDER** (Добавить нового поставщика удостоверений) в верхнем правом углу, чтобы открыть форму для добавления параметров единого входа.

    ![Настройка единого входа](./media/teamphoria-tutorial/add_new_identity_provider.png)

6. Введите сведения в полях, как описано ниже.

    ![Настройка единого входа](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    а. **Отображаемое имя**. Введите отображаемое имя подключаемого модуля на странице администрирования.

    b. **BUTTON NAME** (Имя кнопки). Имя вкладки, которая будет отображаться на странице входа при едином входе.

    c. **CERTIFICATE** (Сертификат). Откройте в Блокноте сертификат, скачанный ранее с портала Azure, скопируйте его содержимое и вставьте в это поле.

    d. **ENTRY POINT** (Точка входа). Вставьте **URL-адрес входа**, скопированный ранее на портале Azure.

    д) Установите переключатель в положение **Вкл.** и нажмите кнопку **Сохранить**.

### <a name="create-teamphoria-test-user"></a>Создание тестового пользователя в Teamphoria

Чтобы пользователи Azure AD могли выполнить вход в Teamphoria, они должны быть подготовлены для Teamphoria. В случае с Teamphoria подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите на корпоративный сайт Teamphoria в качестве администратора.

1. Щелкните **параметры администратора** на левой панели и на вкладке **Управление** щелкните **Пользователи**, чтобы открыть страницу администрирования для пользователей.

    ![Добавление сотрудника](./media/teamphoria-tutorial/admin_manage_users.png)

1. Щелкните **MANUAL INVITE** (Пригласить вручную).

    ![Приглашение пользователей](./media/teamphoria-tutorial/admin_manage_add_users.png)

1. На этой странице сделайте следующее.

    ![Приглашение пользователей](./media/teamphoria-tutorial/manual_user_invite.png)

    а. В текстовом поле **EMAIL ADDRESS** (Адрес электронной почты) введите адрес **электронной почты** пользователя, например пользователя B. Simon.

    b. В текстовом поле **FIRST NAME** (Имя) введите имя пользователя, например **B**.

    c. В текстовое поле **LAST NAME** (Фамилия) введите фамилию пользователя, например **Simon**.

    d. Нажмите кнопку **INVITE 1 USER** (Пригласить одного пользователя). Пользователь должен принять приглашение, чтобы его создали в системе.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Teamphoria на Панели доступа, вы автоматически войдете в приложение Teamphoria, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать приложение Teamphoria с Azure AD](https://aad.portal.azure.com/)

