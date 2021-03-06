---
title: Руководство по Интеграция Azure Active Directory с Evidence.com | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Evidence.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/24/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b2e04c49b05173c6eef27da30c96e2fd8b40b5d
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2020
ms.locfileid: "82734694"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-evidencecom"></a>Руководство по Интеграция единого входа Azure Active Directory с Evidence.com

В этом руководстве описано, как интегрировать Evidence.com с Azure Active Directory (Azure AD). Интеграция Evidence.com с Azure AD обеспечивает следующие возможности:

* Контроль доступа к Evidence.com с помощью Azure AD.
* Автоматический вход пользователей в Evidence.com с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Evidence.com с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Evidence.com поддерживает единый вход инициированного **пакета обновления**.
* После настройки Evidence.com можно применять функцию управления сеансами, которая в режиме реального времени защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-evidencecom-from-the-gallery"></a>Добавление Evidence.com из коллекции

Чтобы настроить интеграцию Evidence.com с Azure AD, необходимо добавить Evidence.com из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Evidence.com**.
1. Выберите **Evidence.com** в области результатов и добавьте приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-evidencecom"></a>Настройка и проверка единого входа Azure Active Directory для Evidence.com

Настройте и проверьте единый вход Azure AD в Evidence.com с использованием данных тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Evidence.com.

Чтобы настроить и проверить единый вход Azure AD в Evidence.com, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Evidence.com](#configure-evidencecom-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Evidence.com](#create-evidencecom-test-user)** требуется для того, чтобы в Evidence.com существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Evidence.com** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Настройка единого входа с помощью SAML** выполните следующие действия:

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<yourtenant>.evidence.com`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<yourtenant>.evidence.com`.

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<your tenant>.evidence.com/?class=UIX&proc=Login`.
    
    > [!NOTE]
    > Эти значения приведены для примера. Вместо них необходимо указать фактические значения URL-адреса входа, идентификатора и URL-адреса ответа. Чтобы получить их, обратитесь в [службу поддержки клиентов Evidence.com](https://communities.taser.com/support/SupportContactUs?typ=LE). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка Evidence.com**.

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

В этом разделе описано, как включить единый вход Azure для пользователя B.Simon, предоставив этому пользователю доступ к Evidence.com.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Evidence.com**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-evidencecom-sso"></a>Настройка единого входа в Evidence.com

1. В отдельном окне веб-браузера войдите в клиент Evidence.com с правами администратора и перейдите на вкладку **Admin** (Администрирование).

2. Щелкните **Agency Single Sign On**

3. Выберите параметр **SAML Based Single Sign On**

4. Скопируйте **идентификатор Azure AD**, **URL-адрес входа** и **URL-адрес выхода**, отображаемые на портале Azure, и добавьте их в соответствующие поля в приложении Evidence.com.

5. Откройте скачанный файл сертификата в кодировке Base64 в Блокноте, скопируйте его содержимое в буфер обмена, а затем вставьте его в поле **Security Certificate** (Сертификат безопасности). 

6. Сохраните конфигурацию в Evidence.com.

### <a name="create-evidencecom-test-user"></a>Создание тестового пользователя Evidence.com

Чтобы пользователи Azure AD могли выполнять вход, нужно подготовить их для доступа в приложении Evidence.com. В этом разделе описано, как создавать учетные записи пользователей Azure AD в Evidence.com.

**Чтобы подготовить учетную запись пользователя в Evidence.com, выполните следующие действия.**

1. В окне веб-браузера войдите на корпоративный веб-сайт Evidence.com с правами администратора.

2. Перейдите к вкладке **Admin** (Администрирование).

3. Щелкните **Add User**(Добавить пользователя).

4. Нажмите кнопку **Добавить**.

5. Параметр **Email Address** (Адрес электронной почты) для нового пользователя должен совпадать с именем пользователя в Azure AD, которому вы хотите предоставить доступ. Возможно, в вашей организации в качестве имени пользователя не используется адрес электронной почты. Тогда зайдите на портале Azure в раздел **Evidence.com > Атрибуты > Единый вход** и измените параметр nameidentifier (идентификатор имени, отправляемый приложению Evidence.com) так, чтобы передавался именно адрес электронной почты.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкните плитку Evidence.com на панели доступа, чтобы автоматически войти в приложение Evidence.com, для которого вы настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Проверьте работу Evidence.com с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Как защитить Evidence.com с помощью функции управления настройками условного доступа](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

