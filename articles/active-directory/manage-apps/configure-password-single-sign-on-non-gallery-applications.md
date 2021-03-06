---
title: Настройка единого входа по паролю для приложений Azure AD | Документация Майкрософт
description: Настройка единого входа с использованием пароля для корпоративных приложений Azure AD на платформе Microsoft Identity Platform (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 563bda275b73f76b042b5e57a9909ca78c504bb3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77063532"
---
# <a name="configure-password-single-sign-on"></a>Настройка пароля единого входа

При [добавлении приложения из коллекции](add-gallery-app.md) или веб- [приложения не из коллекции](add-non-gallery-app.md) в корпоративные приложения Azure AD одним из доступных вариантов единого входа является [единый вход на основе пароля](what-is-single-sign-on.md#password-based-sso). Этот параметр доступен для любого веб-сайта со страницей входа в HTML. Единый вход с помощью пароля (хранение пароля в хранилище) позволяет управлять доступом и паролями для веб-приложений, которые не поддерживают федерацию удостоверений. Это также полезно для сценариев, в которых несколько пользователей должны совместно использовать одну учетную запись, например учетные записи приложения социальных сетей в вашей организации. 

Единый вход на основе пароля — это отличный способ начать интеграцию приложений в Azure AD быстро и позволяет:

-   Использование **единого входа для пользователей** с помощью безопасного хранения и воспроизведения имен пользователей и паролей для приложений, интегрированных с Azure AD.

-   **Поддержка приложений, использующих несколько полей ввода учетных данных** помимо обычных имени пользователя и пароля.

-   **Настройка меток** для полей ввода имени пользователя и пароля, которые будут отображаться при вводе учетных данных на [панели доступа к приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

-   **Пользователи** могут использовать имена пользователей и пароли для любых существующих учетных записей при вводе вручную в [панели доступа к приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

-   **Члены бизнес-группы** могут назначать пользователям имена для входа и пароли, используя функцию [самостоятельной настройки доступа к приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access).

-   Разрешить **администратору** указывать имя пользователя и пароль для использования пользователями или группами при входе в приложение с помощью функции обновления учетных данных 

## <a name="before-you-begin"></a>Перед началом

Если приложение еще не добавлено в клиент Azure AD, ознакомьтесь со статьей о [добавлении приложения из коллекции](add-gallery-app.md) или [добавлении приложения не из коллекции](add-non-gallery-app.md).

## <a name="open-the-app-and-select-password-single-sign-on"></a>Откройте приложение и выберите "единый вход по паролю".

1. Войдите на [портал Azure](https://portal.azure.com) в качестве администратора облачных приложений или администратора приложения для клиента Azure AD.

2. Перейдите в раздел **Azure Active Directory** > **корпоративные приложения**. Вы увидите случайную выборку приложений, размещенных в арендаторе Azure AD. 

3. В меню **Тип приложения** выберите **Все приложения** и щелкните **Применить**.

4. В поле поиска введите имя приложения, а затем выберите нужное приложение на панели результатов.

5. В разделе **Управление** щелкните **Единый вход**. 

6. Выберите **на основе пароля**.

7. Введите URL-адрес веб-страницы входа приложения. Эта строка должна быть страницей, содержащей поле ввода имени пользователя.

   ![Единый вход на основе пароля](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

8. Щелкните **Сохранить**. Azure AD пытается проанализировать страницу входа для ввода имени пользователя и ввода пароля. Если эта операция завершается с ошибкой, то все готово. 
 
> [!NOTE]
> Следующим шагом является [Назначение пользователям или группам приложения](methods-for-assigning-users-and-groups.md). После назначения пользователей и групп можно указать учетные данные, которые будут использоваться от имени пользователя при входе в приложение. Выберите **Пользователи и группы**, установите флажок для строки пользователя или группы и нажмите кнопку **обновить учетные данные**. Затем введите имя пользователя и пароль, которые будут использоваться от имени пользователей или групп. В противном случае пользователям будет предложено ввести учетные данные при запуске.
 

## <a name="manual-configuration"></a>Настройка вручную

Если попытка синтаксического анализа Azure AD завершается неудачно, можно настроить вход вручную.

1. В разделе ** \<имя приложения> конфигурация**выберите **настроить \<имя приложения> параметры единого входа** , чтобы отобразить страницу **Настройка единого входа** . 

2. Выберите **вручную определять поля входа**. Отображаются дополнительные инструкции, описывающие Ручное обнаружение полей входа.

   ![Ручная настройка единого входа на основе пароля](./media/configure-password-single-sign-on/password-configure-sign-on.png)
3. Выберите **запись полей входа**. На новой вкладке откроется страница состояние захвата, **в которой в настоящее время выполняется запись метаданных**сообщения.

4. Если в новой вкладке появится поле **обязательное расширение панели доступа** , выберите **установить сейчас** , чтобы установить расширение браузера для **безопасного входа в мои приложения** . (Для расширения браузера требуется Microsoft ребро, Chrome или Firefox.) Затем установите, запустите и включите расширение, а затем обновите страницу Состояние захвата.

   Затем расширение браузера открывает другую вкладку, на которой отображается введенный URL-адрес.
5. На вкладке с указанным URL-адресом перейдите к процессу входа. Заполните поля имя пользователя и пароль и попробуйте войти. (Вам не нужно указывать правильный пароль.)

   Появится запрос на сохранение захваченных полей входа.
6. Щелкните **ОК**. Расширение браузера обновляет страницу Состояние захвата, при этом метаданные сообщения **были обновлены для приложения**. Вкладка браузер закрывается.

7. На странице " **Настройка входа** Azure AD" нажмите кнопку **ОК, я смог успешно войти в приложение**.

8. Щелкните **ОК**.

После записи страницы входа можно назначить пользователей и группы, а также настроить политики учетных данных так же, как обычные [приложения единого входа с паролем](what-is-single-sign-on.md).

> [!NOTE]
> Вы можете загрузить логотип, который будет отображаться на плитке приложения. Для этого нажмите кнопку **Загрузить логотип** на вкладке **Настройка** для этого приложения.

## <a name="next-steps"></a>Дальнейшие действия

- [Assign users and groups to an application in Azure Active Directory](methods-for-assigning-users-and-groups.md) (Назначение пользователей и групп для приложения в Azure Active Directory)
- [Managing user account provisioning for enterprise apps in the Azure portal](../app-provisioning/configure-automatic-user-provisioning-portal.md) (Управление подготовкой учетных записей пользователей для корпоративных приложений на портале Azure)
