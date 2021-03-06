---
title: Руководство по Интеграция единого входа Azure Active Directory с Firmex VDR | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Firmex VDR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 670ff192-c23e-49e4-8fd1-516e02d8856c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdfb857d3a68081fda84aef33e6b5a4b4d1bce28
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "76761313"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-firmex-vdr"></a>Руководство по Интеграция единого входа Azure Active Directory с Firmex VDR

В этом учебнике описано, как интегрировать Firmex VDR с Azure Active Directory (Azure AD). После интеграции Firmex VDR с Azure AD доступны следующие возможности.

* Контроль доступа к Firmex VDR с помощью Azure AD.
* Вы можете включить автоматический вход пользователей в Firmex VDR с использованием учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Firmex VDR с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Firmex VDR поддерживает единый вход, инициируемый **поставщиком услуг и поставщиком удостоверений**.

* После настройки Firmex можно применять элементы управления сеансами, которые защищают от хищения и несанкционированного доступа к конфиденциальным данным вашей организации в режиме реального времени. Элементы управления сеансом являются расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-firmex-vdr-from-the-gallery"></a>Добавление Firmex VDR из коллекции

Чтобы настроить интеграцию Firmex VDR в Azure AD, необходимо добавить Firmex VDR из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Firmex VDR**.
1. Выберите **Firmex VDR** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-firmex-vdr"></a>Настройка и проверка единого входа Azure AD для Firmex VDR

Настройте и проверьте единый вход Azure AD в Firmex VDR с использованием данных тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Firmex VDR.

Чтобы настроить и проверить единый вход Azure AD в Firmex VDR, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Firmex VDR](#configure-firmex-vdr-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя приложения Firmex VDR](#create-firmex-vdr-test-user)** необходимо для того, чтобы в Firmex VDR существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Firmex VDR** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** не нужно выполнять никаких действий, так как приложение уже предварительно интегрировано с Azure.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес: `https://login.firmex.com`.

1. Выберите команду **Сохранить**.

1. Приложение Firmex VDR ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. В дополнение к описанному выше приложение Firmex VDR ожидает в ответе SAML несколько дополнительных атрибутов, которые показаны ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    | Имя | Исходный атрибут|
    | ------------ | --------- |
    | email | user.mail |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Требуемые URL-адреса вы можете скопировать из раздела **Настройка Firmex VDR**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Firmex VDR.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Firmex VDR**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-firmex-vdr-sso"></a>Настройка единого входа Firmex VDR

### <a name="before-you-get-started"></a>Необходимые условия

#### <a name="what-youll-need"></a>Необходимые ресурсы

-   Активная подписка Firmex.
-   Azure AD в качестве службы единого входа.
-   ИТ-администратор для настройки единого входа.
-   После включения единого входа все пользователи в вашей компании должны войти в Firmex, используя единый вход вместо имени для входа и пароля.

#### <a name="how-long-will-this-take"></a>Сколько времени это займет?

Реализация единого входа занимает несколько минут. Между включением единого входа для вашего сайта службой поддержки Firmex и проверкой подлинности пользователей в вашей компании практически не возникает простоев. Просто выполните шаги ниже.

### <a name="step-1---identify-your-companys-domains"></a>Шаг 1. Указание доменов организации

Укажите домены, в которых пользователи вашей компании будут выполнять вход в систему.

Пример:

- @firmex.com
- @firmex.ca

### <a name="step-2---contact-firmex-support-with-your-domains"></a>Шаг 2. Передача сведений о доменах в службу поддержки Firmex

Отправьте электронное письмо в [службу поддержки Firmex](mailto:support@firmex.com) или позвоните по номеру +1 (888) 688-40-42. Передайте сведения о домене. Служба поддержки Firmex добавит домены в ваш экземпляр VDR как **утвержденные домены**. Теперь администратору необходимо настроить единый вход.

Предупреждение. Пока администратор сайта не настроит утвержденные домены, пользователи вашей компании не смогут войти в VDR. Пользователи, не являющиеся сотрудниками компании (то есть гости), смогут войти в систему, используя свой адрес электронной почты и пароль. Настройка должна занять несколько минут.

### <a name="step-3---configure-the-claimed-domains"></a>Шаг 3. Настройка утвержденных доменов

1. Войдите в Firmex как администратор сайта.
1. В левом верхнем углу щелкните логотип компании.
1. Выберите вкладку **Единый вход**. Выберите раздел **Настройка единого входа**. Выберите домен, который собираетесь настроить.

    ![Утвержденные домены](./media/firmex-vdr-tutorial/edit-sso.png)  

1. ИТ-администратор должен заполнить следующие поля. Значения для полей, которые следует получить у поставщика удостоверений:  

    ![Настройка единого входа](./media/firmex-vdr-tutorial/SSO-config.png)

    а. В текстовое поле **Entity ID** (Идентификатор сущности) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    b. В текстовом поле **Identity Provider URL** (URL-адрес для входа поставщика удостоверений) вставьте значение **URL-адреса входа**, скопированное на портале Azure.

    c. **Сертификат открытого ключа**. Для проверки подлинности в сообщении SAML может быть указана цифровая подпись издателя. Чтобы проверить подпись сообщения, получатель сообщения использует открытый ключ, который принадлежит издателю. Аналогичным образом для шифрования сообщения издателю должен быть известен открытый ключ шифрования, принадлежащий конечному получателю. В обоих случаях (подписывание и шифрование) доверенные открытые ключи нужно предоставить заранее.  Это **X509Certificate** из **XML метаданных федерации**.

    d. Нажмите кнопку **Сохранить**, чтобы завершить настройку единого входа. Изменения вступают в силу немедленно.

1. Теперь единый вход для сайта включен.

### <a name="create-firmex-vdr-test-user"></a>Создание тестового пользователя Firmex VDR

В этом разделе вы узнаете, как создать пользователя B.Simon в приложении Firmex. Чтобы добавить пользователей на платформе Firmex, обратитесь в [службу поддержки Firmex](mailto:support@firmex.com). Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Firmex VDR на Панели доступа, вы автоматически войдете в приложение Firmex VDR, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать Firmex VDR с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
