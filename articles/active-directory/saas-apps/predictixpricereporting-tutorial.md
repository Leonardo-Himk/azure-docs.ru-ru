---
title: Руководство по интеграции Azure Active Directory с Predictix Price Reporting | Документация Майкрософт
description: Из этого руководства вы узнаете, как настроить единый вход Azure Active Directory в Predictix Price Reporting.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 691d0c43-3aa1-4220-9e46-e7a88db234ad
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 808b2d964bb39af6b410a84563717102ebece454
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "67094106"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a>Учебник. Интеграция Azure Active Directory с Predictix Price Reporting

В этом руководстве описано, как интегрировать Predictix Price Reporting с Azure Active Directory (Azure AD).

Такая интеграция обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Predictix Price Reporting.
* Вы можете включить автоматический вход пользователей в Predictix Price Reporting (единый вход) с использованием учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно — через портал Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>предварительные требования

Чтобы настроить интеграцию Azure AD с Predictix Price Reporting, вам потребуется:

* подписка Azure AD; Если у вас нет среды Azure AD, вы можете получить [пробную подписку на один месяц](https://azure.microsoft.com/pricing/free-trial/).
* подписка Predictix Price Reporting с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure Active Directory в тестовой среде.

* Predictix Price Reporting поддерживает единый вход, инициируемый поставщиком услуг.

## <a name="adding-predictix-price-reporting-from-the-gallery"></a>Добавление приложения Predictix Price Reporting из коллекции

Чтобы настроить интеграцию Predictix Price Reporting с Azure AD, необходимо добавить Predictix Price Reporting из коллекции в список управляемых приложений SaaS.

1. На [портале Azure](https://portal.azure.com) в области слева щелкните **Azure Active Directory**.

    ![Выберите Azure Active Directory.](common/select-azuread.png)

2. Выберите **Корпоративные приложения** > **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, выберите **Новое приложение** в верхней части окна.

    ![Выбор элемента "Новое приложение"](common/add-new-app.png)

4. В поле поиска введите **Predictix Price Reporting**. Выберите **Predictix Price Reporting** в результатах поиска, а затем щелкните **Добавить**.

     ![Результаты поиска](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Predictix Price Reporting с использованием тестового пользователя Britta Simon.
Для единого входа необходимо установить связь между пользователем в Azure AD и соответствующим пользователем в Predictix Price Reporting.

Чтобы настроить и проверить единый вход Azure AD в Predictix Price Reporting, потребуется выполнить действия в приведенных ниже стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Predictix Price Reporting](#configure-predictix-price-reporting-single-sign-on)** на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить ему использовать единый вход Azure AD.
5. **[Создание тестового пользователя Predictix Price Reporting](#create-a-predictix-price-reporting-test-user)** , связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться, что конфигурация работает правильно.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Predictix Price Reporting, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Predictix Price Reporting** щелкните **Единый вход**.

    ![Выбор пункта "Единый вход"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Выбор метода единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Значок "Изменить"](common/edit-urls.png)

4. В диалоговом окне **Базовая конфигурация SAML** выполните следующие действия.

    ![Диалоговое окно "Базовая конфигурация SAML"](common/sp-identifier.png)

    1. В поле **URL-адрес для входа** введите URL-адрес в следующем формате:

       `https://<companyname-pricing>.predictix.com/sso/request`

    1. В поле **Идентификатор (сущности)** введите URL-адрес в следующем формате:

        | |
        |--|
        | `https://<companyname-pricing>.predictix.com` |
        | `https://<companyname-pricing>.dev.predictix.com` |
        | |

    > [!NOTE]
    > Эти значения представляют собой заполнители. Необходимо указать фактические URL-адрес для входа и идентификатор. Чтобы получить эти значения, обратитесь к [группе поддержки Predictix Price Reporting](https://www.infor.com/company/customer-center/). Можно также ознакомиться с шаблонами в диалоговом окне **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните ссылку **Скачать** рядом с нужным **сертификатом (Base64)** и сохраните сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка Predictix Price Reporting**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    1. **URL-адрес входа**.

    1. **Идентификатор Azure AD**

    1. **URL-адрес выхода**.

### <a name="configure-predictix-price-reporting-single-sign-on"></a>Настройка единого входа в Predictix Price Reporting

Чтобы настроить единый вход на стороне Predictix Price Reporting, нужно отправить скачанный сертификат и URL-адреса, скопированные на портале Azure, [группе поддержки Predictix Price Reporting](https://www.infor.com/company/customer-center/). Эта группа обеспечит правильную настройку подключения для единого входа SAML с обеих сторон.

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

    1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как позволить пользователю Britta Simon использовать единый вход Azure AD, предоставив этому пользователю доступ к Predictix Price Reporting.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Predictix Price Reporting**.

    ![корпоративные приложения.](common/enterprise-applications.png)

2. Из списка приложений выберите **Predictix Price Reporting**.

    ![Список приложений](common/all-applications.png)

3. В области слева выберите **Пользователи и группы**.

    ![Выбор параметра "Пользователи и группы"](common/users-groups-blade.png)

4. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** — **Пользователи и группы**.

    ![Выбор элемента "Добавить пользователя"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка. В нижней части экрана нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** выберите **Назначить**.

### <a name="create-a-predictix-price-reporting-test-user"></a>Создание тестового пользователя Predictix Price Reporting

Далее необходимо создать пользователя Britta Simon в Predictix Price Reporting. Чтобы добавить пользователей, обратитесь к [группе поддержки Predictix Price Reporting](https://www.infor.com/company/customer-center/). Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="test-single-sign-on"></a>Проверка единого входа

Теперь необходимо проверить конфигурацию единого входа Azure AD с помощью Панели доступа.

Щелкнув элемент "Predictix Price Reporting" на Панели доступа, вы автоматически войдете в приложение Predictix Price Reporting, для которого настроили единый вход. Дополнительные сведения см. в разделе [Доступ и использование приложений на портале "Мои приложения"](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)