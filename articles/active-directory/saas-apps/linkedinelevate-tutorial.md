---
title: Руководство по интеграции единого входа Azure Active Directory с LinkedIn Elevate | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и LinkedIn Elevate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 03efae5f9dec904f141a6776766850aa1f328892
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "74892164"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-elevate"></a>Руководство по интеграции единого входа Azure Active Directory с LinkedIn Elevate

В этом учебнике описано, как интегрировать приложение LinkedIn Elevate с Azure Active Directory (Azure AD). Интеграция LinkedIn Elevate с Azure AD обеспечивает следующие возможности:

* Контроль доступа к приложению LinkedIn Elevate с помощью Azure AD.
* Включение автоматического входа пользователей в LinkedIn Elevate с использованием учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка LinkedIn Elevate с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.



* LinkedIn Elevate поддерживает единый вход, инициированный **поставщиком услуг и поставщиком удостоверений**.
* LinkedIn Elevate поддерживает **JIT**-подготовку пользователей.
* LinkedIn Elevate поддерживает [**автоматическую** подготовку пользователей](linkedinelevate-provisioning-tutorial.md).

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Добавление LinkedIn Elevate из коллекции

Чтобы настроить интеграцию приложения LinkedIn Elevate с Azure AD, вам нужно добавить это приложение из коллекции в свой список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **LinkedIn Elevate**.
1. Выберите **LinkedIn Elevate** на панели результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-elevate"></a>Настройка и проверка единого входа Azure AD для LinkedIn Elevate

Настройте и проверьте единый вход Azure AD в LinkedIn Elevate с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в LinkedIn Elevate.

Чтобы настроить и проверить единый вход Azure AD в LinkedIn Elevate, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в LinkedIn Elevate](#configure-linkedin-elevate-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя LinkedIn Elevate](#create-linkedin-elevate-test-user)** требуется для того, чтобы в LinkedIn Elevate существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **LinkedIn Elevate** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите значение **идентификатора сущности**. Далее в этом учебнике описано, как скопировать его на портале LinkedIn.

    b. В текстовом поле **URL-адрес ответа** введите значение **URL-адреса службы обработчика утверждений (ACS)** . Далее в этом учебнике описано, как скопировать это значение URL-адреса на портале LinkedIn.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>`.

1. Приложение LinkedIn Elevate ожидает утверждения SAML в определенном формате, а для этого вам нужно добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию, в котором **nameidentifier** сопоставляется с **user.userprincipalname**. Для приложения LinkedIn Elevate требуется сопоставление nameidentifier с **user.mail**. поэтому вам нужно изменить сопоставление атрибутов (щелкните значок "Изменить").

    ![Изображение](common/edit-attribute.png)

1. В дополнение к описанному выше приложение LinkedIn Elevate ожидает в ответе SAML несколько дополнительных атрибутов, которые показаны ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    | Имя | Исходный атрибут|
    | -------| -------------|
    | department | user.department |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Требуемый URL-адрес можно скопировать из раздела **Настройка LinkedIn Elevate**.

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

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к LinkedIn Elevate.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **LinkedIn Elevate**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-linkedin-elevate-sso"></a>Настройка единого входа в LinkedIn Elevate

1. В другом окне веб-браузера войдите в свой клиент LinkedIn Elevate с правами администратора.

1. В **Account Center** (Центр учетных записей) в разделе **Settings** (Параметры) щелкните **Global Settings** (Глобальные параметры). Кроме того, выберите **Elevate — Elevate AAD Test** (Проверка Azure AD для Elevate) из раскрывающегося списка.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Щелкните **OR Click Here to load and copy individual fields from the form** (Или щелкните здесь, чтобы загрузить и скопировать отдельные поля из формы) и сделайте следующее:

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

    а. Скопируйте **идентификатор сущности** и вставьте его в текстовое поле **Идентификатор** в разделе **Базовая конфигурация SAML** на портале Azure.

    b. Скопируйте **URL-адрес службы обработчика утверждений (ACS)** и вставьте его в текстовое поле **URL-адрес ответа** в разделе **Базовая конфигурация SAML** на портале Azure.

1. Перейдите в раздел **LinkedIn Admin Settings** (Параметры администратора LinkedIn). Передайте XML-файл, скачанный с портала Azure. Для этого нажмите кнопку Upload XML file (Передать XML-файл).

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Нажмите кнопку **On** (Включить), чтобы включить единый вход. Состояние единого входа изменится с **Not Connected** (Не подключено) на **Connected** (Подключено.)

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-elevate-test-user"></a>Создание тестового пользователя LinkedIn Elevate

Приложение LinkedIn Elevate поддерживает JIT-подготовку пользователей, поэтому после аутентификации пользователи будут созданы в приложении автоматически. На странице параметров администратора на портале LinkedIn Elevate включите параметр **Automatically assign licenses** (Автоматически назначать лицензии), чтобы активировать JIT-подготовку, одновременно с которой пользователю будет сразу назначена лицензия. LinkedIn Elevate также поддерживает автоматическую подготовку пользователей. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](linkedinelevate-provisioning-tutorial.md).

   ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку LinkedIn Elevate на Панели доступа, вы автоматически войдете в приложение LinkedIn Elevate, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать LinkedIn Elevate с Azure AD](https://aad.portal.azure.com/)

