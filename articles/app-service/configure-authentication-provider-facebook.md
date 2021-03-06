---
title: Как настроить приложение службы приложений для использования имени для входа Facebook
description: Узнайте, как настроить проверку подлинности Facebook в качестве поставщика удостоверений для службы приложений или приложения функций Azure.
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.topic: article
ms.date: 06/06/2019
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: b6aad323c0d6fa8f59c9fad203640c477b162503
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80519961"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-facebook-login"></a>Настройка службы приложений или приложения "функции Azure" для использования имени для входа Facebook

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

В этой статье показано, как настроить службу приложений Azure или функцию Azure для использования Facebook в качестве поставщика проверки подлинности.

Для выполнения процедуры, описанной в этой статье, требуется учетная запись Facebook с проверенным адресом электронной почты и номером мобильного телефона. Чтобы создать новую учетную запись Facebook, перейдите по ссылке [facebook.com].

## <a name="register-your-application-with-facebook"></a><a name="register"> </a>Регистрация приложения с помощью Facebook

1. Перейдите на веб-сайт [разработчиков Facebook] и выполните вход с использованием учетных данных Facebook.

   Если у вас нет учетной записи Facebook for Developers, щелкните начало **работы** и выполните действия по регистрации.
1. Выберите " **Мои приложения** > "**Добавить новое приложение**.
1. В поле **Отображаемое имя** :
   1. Введите уникальное имя для приложения.
   1. Укажите свой **контактный адрес электронной почты**.
   1. Выберите **создать идентификатор приложения**.
   1. Завершите проверку безопасности.

   Откроется панель мониторинга разработчика для нового приложения Facebook.
1. Выберите **панель мониторинга** > **Facebook вход** > **Настройка** > **веб-сайта**.
1. В левой области навигации в разделе **имя входа Facebook**выберите **Параметры**.
1. В поле **допустимые URI перенаправления OAuth** введите `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback`. Не забудьте `<app-name>` заменить именем приложения службы приложений Azure.
1. Щелкните **Save changes** (Сохранить изменения).
1. В левой области выберите **Параметры** > **базовый**. 
1. В поле **секрет приложения** выберите команду **отобразить**. Скопируйте значения **идентификатора приложения** и **секрета приложения**. Их можно использовать позже для настройки приложения службы приложений в Azure.

   > [!IMPORTANT]
   > Секрет приложения — это важные учетные данные безопасности. Не сообщайте этот секрет никому и не раскрывайте его в клиентском приложении.
   >

1. Учетная запись Facebook, которая использовалась для регистрации приложения, является администратором приложения. На этом этапе только администраторы могут войти в это приложение.

   Чтобы проверить подлинность других учетных записей Facebook, выберите **Проверка приложения** и **> \<** включить, чтобы разрешить общий доступ к приложению, используя проверку подлинности Facebook.

## <a name="add-facebook-information-to-your-application"></a><a name="secrets"> </a>Добавление данных Facebook в приложение

1. Войдите в [портал Azure] и перейдите к приложению службы приложений.
1. Выберите **Параметры** > **Проверка подлинности и авторизация**и убедитесь, что **Проверка подлинности службы приложений** **включена.**
1. Выберите **Facebook**и вставьте значения в поле идентификатор приложения и секрет приложения, полученные ранее. Включите все области, необходимые для приложения.
1. Нажмите кнопку **OK**.

   ![Снимок экрана параметров Facebook мобильного приложения][0]

    По умолчанию служба приложений обеспечивает проверку подлинности, но не ограничивает разрешенный доступ к содержимому и API-интерфейсам сайта. Необходимо авторизовать пользователей в коде приложения.
1. Используемых Чтобы ограничить доступ только для пользователей, прошедших проверку подлинности в Facebook, задайте **действие, которое будет выполняться, если запрос не прошел проверку подлинности** в **Facebook**. При установке этой функции приложению требуется проверка подлинности всех запросов. Он также перенаправляет все запросы без проверки подлинности на Facebook для проверки подлинности.

   > [!CAUTION]
   > Таким образом, ограниченный доступ применяется ко всем вызовам приложения, что может быть нежелательно для приложений, имеющих общедоступную домашнюю страницу, как во многих одностраничных приложениях. Для таких приложений рекомендуется **Разрешить анонимные запросы (без действий)** , чтобы приложение вручную начинало проверку подлинности. Дополнительные сведения см. в разделе [поток проверки подлинности](overview-authentication-authorization.md#authentication-flow).

1. Нажмите кнопку **Сохранить**.

Теперь вы можете использовать Facebook для проверки подлинности в приложении.

## <a name="next-steps"></a><a name="related-content"> </a>Дальнейшие шаги

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[разработчиков для Facebook]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Портал Azure]: https://portal.azure.com/
