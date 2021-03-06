---
title: Руководство по интеграции Azure Active Directory с приложением PageDNA | Документация Майкрософт
description: Сведения о настройке единого входа между Azure Active Directory и PageDNA.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c8765864-45f4-48c2-9d86-986a4aa431e4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c7f8c8efcad0a07a3d3a56925866b10d94f82ed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "68227477"
---
# <a name="tutorial-azure-active-directory-integration-with-pagedna"></a>Руководство по интеграции Azure Active Directory с PageDNA

В этом руководстве описано, как интегрировать PageDNA с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением PageDNA обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к PageDNA.
* Вы можете включить автоматический вход пользователей в PageDNA (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно — через портал Azure.

Дополнительные сведения об интеграции приложения SaaS с Azure AD см. в статье [о доступе к приложениям и едином входе с помощью Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с PageDNA, вам потребуется:

* Подписка Azure AD. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.
* подписка PageDNA с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде и интеграцию PageDNA с Azure AD.

PageDNA поддерживает следующие функции:

* единый вход, инициируемый поставщиком услуг;

* JIT-подготовка пользователей.

## <a name="add-pagedna-from-the-azure-marketplace"></a>Добавление PageDNA из Azure Marketplace

Чтобы настроить интеграцию PageDNA с Azure AD, необходимо добавить PageDNA из Azure Marketplace в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com?azure-portal=true).
1. В области слева выберите **Azure Active Directory**.

    ![Пункт Azure Active Directory](common/select-azuread.png)

1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Область "Корпоративные приложения"](common/enterprise-applications.png)

1. Чтобы добавить новое приложение, выберите **+ Новое приложение** в верхней части панели.

    ![Элемент "Новое приложение"](common/add-new-app.png)

1. В поле поиска введите **PageDNA**. В результатах поиска выберите **PageDNA** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![PageDNA в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в PageDNA с использованием тестового пользователя **Britta Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в PageDNA.

Чтобы настроить и проверить единый вход Azure AD в PageDNA, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройте единый вход в Azure AD](#configure-azure-ad-single-sign-on)** , чтобы пользователи могли использовать эту функцию.
1. **[Настройка единого входа в PageDNA](#configure-pagedna-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
1. **[Создайте тестового пользователя Azure AD](#create-an-azure-ad-test-user)** для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Назначьте тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** , чтобы позволить Britta Simon использовать единый вход в Azure AD.
1. **[Создание тестового пользователя PageDNA](#create-a-pagedna-test-user)** требуется для того, чтобы в PageDNA существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверьте единый вход](#test-single-sign-on)** , чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в PageDNA, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **PageDNA** выберите **Единый вход**.

    ![Параметр для настройки единого входа](common/select-sso.png)

1. На панели **Выбрать метод единого входа** выберите режим **SAML/WS-FED**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

1. На панели **Настройка единого входа с помощью SAML** щелкните **Изменить** (значок карандаша), чтобы открыть панель **Базовая конфигурация SAML**.

    ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В области **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения PageDNA](common/sp-identifier.png)

    1. В поле **URL-адрес для входа** введите URL-адрес в одном из следующих форматов:

        ||
        |--|
        | `https://stores.pagedna.com/<your site>` |
        | `https://<your domain>` |
        | `https://<your domain>/<your site>` |
        | `https://www.nationsprint.com/<your site>` |
        | |

    1. В поле **Идентификатор (сущности)** введите URL-адрес в одном из следующих форматов:

        ||
        |--|
        | `https://stores.pagedna.com/<your site>/saml2ep.cgi` |
        | `https://www.nationsprint.com/<your site>/saml2ep.cgi` |
        | |

    > [!NOTE]
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки PageDNA](mailto:success@pagedna.com). Можно также посмотреть шаблоны на панели **Базовая конфигурация SAML** на портале Azure.

1. В области **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать подходящий **сертификат (необработанный)** из предложенных вариантов и сохранить его на своем компьютере.

    ![Параметр "Скачать" для необработанного сертификата](common/certificateraw.png)

1. Требуемые URL-адреса можно скопировать в разделе **Настройка PageDNA**.

   * **Login URL** (URL-адрес для входа)
   * **идентификатор Azure AD**;
   * **URL-адрес выхода**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="configure-pagedna-single-sign-on"></a>Настройка единого входа в PageDNA

Чтобы настроить единый вход на стороне PageDNA, отправьте скачанный сертификат (необработанный) и соответствующие URL-адреса, скопированные на портале Azure, [группе поддержки PageDNA](mailto:success@pagedna.com). Группа поддержки PageDNA обеспечит правильную настройку подключения для единого входа SAML с обеих сторон.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure на панели слева выберите **Azure Active Directory**   > **Пользователи** > **Все пользователи**.

    ![Пункты меню "Пользователи" и "Все пользователи"](common/users.png)

1. В верхней части экрана щелкните **+ Новый пользователь**.

    ![Элемент "Новый пользователь"](common/new-user.png)

1. На панели **Пользователь** выполните следующие действия:

    ![Панель "Пользователь"](common/user-properties.png)

    1. В поле **Имя** введите **BrittaSimon**.
  
    1. В поле **Имя пользователя** введите **BrittaSimon\@\<домен_вашей_компании>.\<доменная_зона>** . Например **BrittaSimon\@contoso.com**.

    1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    1. Нажмите кнопку **создания**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к PageDNA.

1. На портале Azure выберите **Корпоративные приложения** > **Все приложения** > **PageDNA**.

    ![Область "Корпоративные приложения"](common/enterprise-applications.png)

1. В списке приложений выберите **PageDNA**.

    ![PageDNA в списке "Приложения"](common/all-applications.png)

1. В области слева последовательно выберите **Управление** и **Пользователи и группы**.

    ![Пункт меню "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **+ Добавить пользователя**, а в области **Добавление назначения** щелкните **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

1. На панели **Пользователи и группы** выберите **Britta Simon** в списке **Пользователи**, а в нижней панели щелкните **Выбрать**.

1. Если вы планируете получать значение роли в утверждении SAML, на панели **Выбор роли** выберите в списке соответствующую роль для пользователя. В нижней части панели нажмите кнопку **Выбрать**.

1. В области **Добавление назначения** выберите **Назначить**.

### <a name="create-a-pagedna-test-user"></a>Создание тестового пользователя PageDNA

В PageDNA создан пользователь с именем Britta Simon. Вам не нужно ничего делать для его создания. Приложение PageDNA поддерживает JIT-подготовку пользователей, которая включена по умолчанию. Если пользователь Britta Simon еще не существует в PageDNA, он создается после выполнения аутентификации.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе объясняется, как проверить конфигурацию единого входа Azure AD с помощью портала "Мои приложения".

Щелкнув **PageDNA** на портале "Мои приложения", вы автоматически выполните вход с подпиской PageDNA, для которой настроили единый вход. Дополнительные сведения о портале "Мои приложения" см. в статье [Access and use apps on the My Apps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Доступ к приложениям и их использование на портале "Мои приложения").

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список руководств по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

