---
title: Руководство по интеграции единого входа Azure Active Directory с Cisco AnyConnect | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Cisco AnyConnect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 46f327d0-21fb-4914-be71-9e444f247ebe
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/30/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4af7dc5d55e451e4f6873df42e2b740fd1e5cd56
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80891753"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cisco-anyconnect"></a>Руководство по интеграции единого входа Azure Active Directory с Cisco AnyConnect

В этом руководстве описано, как интегрировать Cisco AnyConnect с Azure Active Directory (Azure AD). Интеграция Cisco AnyConnect с Azure AD обеспечивает следующие возможности:

* С помощью Azure AD вы можете контролировать доступ к Cisco AnyConnect.
* Вы можете включить автоматический вход пользователей в Cisco AnyConnect с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Cisco AnyConnect с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Cisco AnyConnect поддерживает единый вход, инициируемый **поставщиком удостоверений**.
* После настройки Cisco AnyConnect вы можете применить функцию управления сеансом, которая защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-cisco-anyconnect-from-the-gallery"></a>Добавление Cisco AnyConnect из коллекции

Чтобы настроить интеграцию Cisco AnyConnect с Azure AD, необходимо добавить Cisco AnyConnect из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Cisco AnyConnect**.
1. Выберите **Cisco AnyConnect** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cisco-anyconnect"></a>Настройка и проверка единого входа Azure AD для Cisco AnyConnect

Настройте и проверьте единый вход Azure AD в Cisco AnyConnect с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и связанным пользователем в Cisco AnyConnect.

Чтобы настроить и проверить единый вход Azure AD в Cisco AnyConnect, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Cisco AnyConnect](#configure-cisco-anyconnect-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Cisco AnyConnect](#create-cisco-anyconnect-test-user)** требуется для того, чтобы в Cisco AnyConnect существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Cisco AnyConnect** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Настройка единого входа с помощью SAML** введите значения для следующих полей:

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `< YOUR CISCO ANYCONNECT VPN VALUE >`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `< YOUR CISCO ANYCONNECT VPN VALUE >`.

    > [!NOTE]
    > Эти значения приведены для примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Для получения этих значений обратитесь в [службу поддержки клиентов Cisco AnyConnect](https://www.cisco.com/c/en/us/support/index.html). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать файл сертификата. Сохраните этот файл на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Скопируйте нужные URL-адреса в разделе **Настройка Cisco AnyConnect**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

> [!NOTE]
> Если вы хотите подключить несколько TGT сервера, необходимо добавить несколько экземпляров приложения Cisco AnyConnect из коллекции. Кроме того, вы можете отправить собственный сертификат в Azure AD для всех этих экземпляров приложения. Таким образом, у вас может быть один и тот же сертификат для приложений, но для каждого приложения можно настроить разные идентификаторы и URL-адреса ответа.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Cisco AnyConnect.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Cisco AnyConnect**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-cisco-anyconnect-sso"></a>Настройка единого входа в Cisco AnyConnect

1. Сейчас мы выполним этот процесс с помощью CLI, а к варианту с ASDM вы можете вернуться в любое время позднее.

1. Подключитесь к устройству VPN, вы будете использовать Adaptive Security Appliance (ASA) с кодом версии 9.8, а VPN-клиенты — версию 4.6 или более позднюю.

1. Для начала создайте точку доверия и импортируйте предоставленный сертификат SAML.

   ```
    config t

    crypto ca trustpoint AzureAD-AC-SAML
      revocation-check none
      no id-usage
      enrollment terminal
      no ca-check
    crypto ca authenticate AzureAD-AC-SAML
    -----BEGIN CERTIFICATE-----
    …
    PEM Certificate Text from download goes here
    …
    -----END CERTIFICATE-----
    quit
   ```

1. Следующие команды позволяют подготовить поставщик удостоверений SAML.

   ```
    webvpn
    saml idp https://sts.windows.net/xxxxxxxxxxxxx/ (This is your Azure AD Identifier from the Set up Cisco AnyConnect section in the Azure portal)
    url sign-in https://login.microsoftonline.com/xxxxxxxxxxxxxxxxxxxxxx/saml2 (This is your Login URL from the Set up Cisco AnyConnect section in the Azure portal)
    url sign-out https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0 (This is Logout URL from the Set up Cisco AnyConnect section in the Azure portal)
    trustpoint idp AzureAD-AC-SAML
    trustpoint sp (Trustpoint for SAML Requests - you can use your existing external cert here)
    no force re-authentication
    no signature
    base-url https://my.asa.com
    ```

1. Теперь вы можете применить проверку подлинности SAML к конфигурации VPN-туннеля.

    ```
    tunnel-group AC-SAML webvpn-attributes
      saml identity-provider https://sts.windows.net/xxxxxxxxxxxxx/
      authentication saml
    end

    write mem
    ```

    > [!NOTE]
    > Для конфигурации поставщика удостоверений SAML реализована особая функция: при внесении изменений в эту конфигурацию необходимо удалить параметр saml identity-provider config из группы туннелей и повторно применить его, чтобы изменения вступили в силу.

### <a name="create-cisco-anyconnect-test-user"></a>Создание тестового пользователя Cisco AnyConnect

В этом разделе описано, как создать пользователя B.Simon в Cisco AnyConnect. Обратитесь к  [группе поддержки Cisco AnyConnect](https://www.cisco.com/c/en/us/support/index.html) для добавления пользователей на платформу Cisco AnyConnect. Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Cisco AnyConnect на Панели доступа, вы автоматически войдете в приложение Cisco AnyConnect, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование Cisco AnyConnect с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

