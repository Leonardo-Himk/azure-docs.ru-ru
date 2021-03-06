---
title: Руководство по Интеграция Azure Active Directory с Percolate | Документация Майкрософт
description: Из этого руководства вы узнаете, как настроить единый вход между Azure Active Directory и Percolate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 355f9659-b378-44c9-aa88-236e9b529a53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: a6c1f893757baf1e6c85420b31997a5073cff684
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "67094599"
---
# <a name="tutorial-azure-active-directory-integration-with-percolate"></a>Руководство по Интеграция Azure Active Directory с Percolate

В этом руководстве описано, как интегрировать Percolate с Azure Active Directory (Azure AD).

Такая интеграция обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Percolate.
* Вы можете включить автоматический вход пользователей в Percolate (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно — через портал Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Percolate, вам потребуется:

* Подписка Azure AD. (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Percolate с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure Active Directory в тестовой среде.

* Percolate поддерживает единый вход, инициируемый поставщиком услуг и поставщиком удостоверений.

## <a name="add-percolate-from-the-gallery"></a>Добавление Percolate из коллекции

Чтобы настроить интеграцию Percolate с Azure AD, необходимо добавить Percolate из коллекции в список управляемых приложений SaaS.

1. На [портале Azure](https://portal.azure.com) в области слева щелкните **Azure Active Directory**.

    ![Выберите Azure Active Directory.](common/select-azuread.png)

2. Выберите **Корпоративные приложения** > **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, выберите **Новое приложение** в верхней части окна.

    ![Выбор элемента "Новое приложение"](common/add-new-app.png)

4. В поле поиска введите **Percolate**. В результатах поиска выберите **Percolate**, а затем щелкните **Добавить**.

     ![Результаты поиска](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Percolate с использованием тестового пользователя Britta Simon.
Для единого входа необходимо установить связь между пользователем в Azure AD и соответствующим пользователем в Percolate.

Чтобы настроить и проверить единый вход Azure AD в Percolate, вам потребуется выполнить следующие действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Percolate](#configure-percolate-single-sign-on)** на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить ему использовать единый вход Azure AD.
5. **[Создание тестового пользователя Percolate](#create-a-percolate-test-user)** , связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться, что конфигурация работает правильно.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure Active Directory в Percolate, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Percolate** выберите **Единый вход**.

    ![Выбор параметра "Единый вход"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Выбор метода единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Значок "Изменить"](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** не нужно выполнять какую-либо настройку приложения в режиме, инициируемом поставщиком удостоверений. Приложение уже интегрировано с Azure.

    ![Сведения о домене и URL-адресах единого входа приложения Percolate](common/preintegrated.png)

5. Если вы хотите настроить приложение для работы в режиме, инициируемом поставщиком услуг, выберите **Задать дополнительные URL-адреса**, а затем в поле **URL-адрес входа** введите **https://percolate.com/app/login** .

   ![Сведения о домене и URL-адресах единого входа приложения Percolate](common/metadata-upload-additional-signon.png)
6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните значок **Копировать**, чтобы скопировать **URL-адрес метаданных федерации приложений**. Сохраните этот URL-адрес.

    ![Копирование URL-адреса метаданных федерации приложений](common/copy-metadataurl.png)

7. Требуемые URL-адреса можно скопировать из раздела **Настройка Percolate**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    1. **URL-адрес входа**.

    1. **Идентификатор Azure AD**

    1. **URL-адрес выхода**.

### <a name="configure-percolate-single-sign-on"></a>Настройка единого входа в Percolate

1. В новом окне веб-браузера войдите в Percolate с правами администратора.

2. В левой части домашней страницы выберите **Settings** (Параметры).
    
    ![Выбор элемента "Параметры"](./media/percolate-tutorial/configure01.png)

3. В левой области в разделе **Organization** (Организация) выберите **SSO** (Единый вход).

    ![Выбор параметра "SSO" (Единый вход) в разделе "Organization" (Организация)](./media/percolate-tutorial/configure02.png)

    1. В поле **Login URL** (URL-адрес входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    1. В поле **Entity ID** (Идентификатор сущности) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.

    1. Откройте в Блокноте сертификат в кодировке Base-64, скачанный с портала Azure. Скопируйте его содержимое и вставьте в поле **x509 certificates** (Сертификаты x509).

    1. В поле **Email attribute** (Атрибут электронной почты) введите **emailaddress**.

    1. Поле **Identity provider metadata URL** (URL-адрес метаданных поставщика удостоверений) является необязательным. Если вы скопировали значение **URL-адрес метаданных федерации приложений** на портале Azure, то можно вставить его в это поле.

    1. Из списка **Should AuthNRequests be signed?** (Подписывать AuthNRequests?) выберите значение **No** (Нет).

    1. Из списка **Enable SSO auto-Provisioning** (Активировать автоматическую подготовку для единого входа) выберите значение **No** (Нет).

    1. Щелкните **Сохранить**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Выбор "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Выбор "Новый пользователь"](common/new-user.png)

3. В диалоговом окне **Пользователь** сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    1. В поле **Имя** введите **BrittaSimon**.
  
    1. В поле **Имя пользователя** введите **BrittaSimon @\<ваш_домен>.\<доменная_зона>** . (Например, BrittaSimon@contoso.com).

    1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    1. Нажмите кнопку **создания**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Percolate.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Percolate**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Percolate**.

    ![Список приложений](common/all-applications.png)

3. В области слева выберите **Пользователи и группы**.

    ![Выбор параметра "Пользователи и группы"](common/users-groups-blade.png)

4. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** — **Пользователи и группы**.

    ![Выбор параметра "Пользователи и группы"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка. В нижней части экрана нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** выберите **Назначить**.

### <a name="create-a-percolate-test-user"></a>Создание тестового пользователя Percolate

Чтобы разрешить пользователям Azure AD входить в Percolate, их нужно добавить в Percolate. Необходимо добавить их вручную.

Чтобы создать учетную запись пользователя, выполните следующие действия.

1. Войдите в Percolate с правами администратора.

2. В левой области в разделе **Organization** (Организация) выберите **Users** (Пользователи). Выберите **New users** (Новые пользователи).

    ![Выбор "New users" (Новые пользователи)](./media/percolate-tutorial/configure03.png)

3. На странице **Create users** (Создание пользователей) выполните следующие действия.

    ![Страница "Create users" (Создание пользователей)](./media/percolate-tutorial/configure04.png)

    1. В текстовом поле **Email** (Электронная почта) введите адрес электронной почты пользователя. Например, brittasimon@contoso.com.

    1. В поле **Full name** (Полное имя) введите имя пользователя. Например, **Brittasimon**.

    1. Выберите **Create users** (Создать пользователей).

### <a name="test-single-sign-on"></a>Проверка единого входа

Теперь необходимо проверить конфигурацию единого входа Azure AD с помощью Панели доступа.

Щелкнув элемент "Percolate" на Панели доступа, вы автоматически войдете в приложение Percolate, для которого настроили единый вход. Дополнительные сведения см. в разделе [Доступ и использование приложений на портале "Мои приложения"](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)