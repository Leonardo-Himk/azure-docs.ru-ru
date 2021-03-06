---
title: Руководство по Интеграции единого входа Azure Active Directory с Cornerstone OnDemand | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Cornerstone OnDemand.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34a92a11880d61ff3e75e0b2a982b6375841b4b7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "76983580"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cornerstone-ondemand"></a>Руководство по Интеграции и настройке единого входа Azure Active Directory с Cornerstone OnDemand

В этом руководстве описано, как интегрировать Cornerstone OnDemand с Azure Active Directory (Azure AD). Интеграция Cornerstone OnDemand с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD можно контролировать доступ к Cornerstone OnDemand.
* Включить автоматический вход пользователей в Cornerstone OnDemand с помощью учетной записи Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Cornerstone OnDemand с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Cornerstone OnDemand поддерживает единый вход, инициированный **поставщиком услуг**.
* Cornerstone OnDemand поддерживает [автоматическую подготовку пользователей](cornerstone-ondemand-provisioning-tutorial.md).
* После настройки Cornerstone OnDemand можете применить функцию управления сеансом, которая защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad).

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Добавление Cornerstone OnDemand из коллекции

Чтобы настроить интеграцию Cornerstone OnDemand с Azure AD, необходимо добавить это приложение из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе**Добавление из коллекции** введите в окне поиска **Cornerstone OnDemand**.
1. Выберите **Cornerstone OnDemand** на панели результатов, а затем добавьте приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cornerstone-ondemand"></a>Настройка и проверка единого входа Azure AD для Cornerstone OnDemand

Настройка и проверка единого входа Azure AD с Cornerstone OnDemand, используя тестового пользователя **B.Simon**. Чтобы обеспечить работу единого входа, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Cornerstone OnDemand.

Чтобы настроить и проверить единый вход Azure AD с Cornerstone OnDemand, выполните следующие стандартные блоки действий.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
2. **[Настройка единого входа в Cornerstone OnDemand](#configure-cornerstone-ondemand-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Cornerstone OnDemand](#create-cornerstone-ondemand-test-user)** требуется для того, чтобы в Cornerstone OnDemand существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
3. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Cornerstone OnDemand** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<company>.csod.com`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<company>.csod.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Cornerstone OnDemand](mailto:moreinfo@csod.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Скопируйте требуемый URL-адрес из раздела **Настройка Cornerstone OnDemand**.

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

В этом разделе описано, как включить единый вход Azure для пользователя B.Simon, предоставив этому пользователю доступ к Cornerstone OnDemand.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Cornerstone OnDemand**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-cornerstone-ondemand-sso"></a>Настройка единого входа Cornerstone OnDemand

Чтобы настроить единый вход на стороне **Cornerstone OnDemand**, нужно отправить скачанный **сертификат (Base64)** и соответствующие URL-адреса, скопированные на портале Azure, [группе поддержки Cornerstone OnDemand](mailto:moreinfo@csod.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-cornerstone-ondemand-test-user"></a>Создание тестового пользователя Cornerstone OnDemand

Цель этого раздела — создать пользователя с именем B.Simon в Cornerstone OnDemand. Cornerstone OnDemand поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](https://docs.microsoft.com/azure/active-directory/saas-apps/cornerstone-ondemand-provisioning-tutorial).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

Чтобы настроить подготовку пользователей, отправьте сведения (например, имя, адрес электронной почты) о том пользователе Azure AD, которого необходимо подготовить, [группе поддержки Cornerstone OnDemand](mailto:moreinfo@csod.com).

>[!NOTE]
>Вы можете использовать любые другие средства создания учетной записи пользователя Cornerstone OnDemand или API, предоставляемые Cornerstone OnDemand, для подготовки учетных записей пользователей AAD.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Cornerstone OnDemand на панели доступа, вы автоматически войдете в приложение Cornerstone OnDemand, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование Cornerstone OnDemand с Azure AD](https://aad.portal.azure.com)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)