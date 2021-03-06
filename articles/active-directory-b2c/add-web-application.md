---
title: Добавление приложения веб-API — Azure Active Directory B2C | Документация Майкрософт
description: Узнайте, как добавить приложение веб-API в клиент Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 04/16/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: e6dbf3d6fd5a43ab2d075c193c5bc589dc3566a0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78190183"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Добавление веб-API приложения в клиент Azure Active Directory B2C

 Зарегистрируйте ресурсы веб-API в клиенте, чтобы они могли принимать запросы и отвечать на них клиентскими приложениями, которые представляют маркер доступа. В этой статье показано, как зарегистрировать веб-API в Azure Active Directory B2C (Azure AD B2C).

Чтобы зарегистрировать приложение в клиенте Azure AD B2C, можно использовать текущий интерфейс **приложений** или новый объединенный интерфейс **Регистрация приложений (предварительная версия)**. [См. дополнительные сведения о новом интерфейсе](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Приложения](#tab/applications/)

1. Войдите на [портал Azure](https://portal.azure.com).
2. Убедитесь, что вы используете каталог, содержащий клиент Azure AD B2C. В верхнем меню выберите фильтр **каталог и подписка** и выберите каталог, содержащий ваш клиент.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
4. Выберите **приложения**и нажмите кнопку **Добавить**.
5. Введите имя приложения. Например, *webapi1*.
6. Для поля **Включить веб-приложение или веб-API** и **Разрешить неявный поток** выберите **Да**.
7. Для **URL-адреса ответа** введите конечные точки, куда Azure AD B2C возвращает все маркеры, запрашиваемые вашим приложением. В рабочем приложении вы можете задать для URL-адреса ответа значение, например `https://localhost:44332`. Для целей тестирования задайте для `https://jwt.ms`URL-адреса ответа значение.
8. Для поля **URI идентификатора приложения** выберите идентификатор, используемый для веб-API. Полный URI код с доменом создается автоматически. Например, `https://contosotenant.onmicrosoft.com/api`.
9. Нажмите кнопку **Создать**.
10. На странице свойств запишите идентификатор приложения, который будет использоваться при настройке веб-приложения.

#### <a name="app-registrations-preview"></a>[Регистрация приложений (предварительная версия)](#tab/app-reg-preview/)

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите фильтр **Каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure AD B2C**. Либо щелкните **Все службы**, а затем найдите и выберите **Azure AD B2C**
1. Щелкните **Регистрация приложений (предварительная версия)** и выберите **Новая регистрация**.
1. Введите **имя** приложения. Например, *webapi1*.
1. В разделе **URL-перенаправления** выберите **Интернет**, а затем введите конечную точку, куда Azure AD B2C будет возвращать все маркеры, запрашиваемые вашим приложением. В рабочем приложении можно задать универсальный код ресурса (URI) перенаправления `https://localhost:5000`конечной точки, например. Во время разработки или тестирования можно задать для `https://jwt.ms`него веб-приложение Майкрософт, которое отображает декодированное содержимое маркера (содержимое маркера никогда не будет выходить из браузера). Вы можете в любое время добавить и изменить URI перенаправления в зарегистрированных приложениях.
1. Выберите **Зарегистрировать**.
1. Запишите **идентификатор приложения (клиента)** для использования в коде веб-API.

Если у вас есть приложение, которое реализует неявный поток предоставления, например одностраничное приложение на основе JavaScript (SPA), можно включить поток, выполнив следующие действия.

1. В разделе **Управление** выберите **Проверка подлинности**.
1. Выберите **Попробовать новый пользовательский интерфейс** (если этот параметр показан).
1. В разделе **неявное предоставление**установите флажки **маркеры доступа** и **идентификаторы маркеров** .
1. Щелкните **Сохранить**.

* * *

## <a name="configure-scopes"></a>Настройка областей

Области предоставляют способ контроля доступа к защищенным ресурсам. Области используются веб-API для реализации управления доступом на уровне области. Например, пользователи веб-API могут иметь доступ на чтение и запись или доступ только на чтение. В этом руководстве также можно использовать области для определения разрешений на чтение и запись для веб-API.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

## <a name="grant-permissions"></a>Предоставить разрешения

Чтобы вызвать защищенный веб-API из приложения, необходимо предоставить приложению разрешения на доступ к API. Например, в [руководстве: регистрация приложения в Azure Active Directory B2C](tutorial-register-applications.md)в Azure AD B2C регистрируется веб-приложение с именем *APP1* . С помощью этого приложения вы можете вызвать веб-API.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Приложение зарегистрировано для вызова защищенного веб-API. Пользователь выполняет аутентификацию в Azure AD B2C для использования приложения. Приложение получает предоставление авторизации из Azure AD B2C для доступа к защищенному веб-API.
