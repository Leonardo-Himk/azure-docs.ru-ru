---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 80e5166775b0cf5acbfce32e61d91c0889e3b086
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78186371"
---
Чтобы зарегистрировать приложение в клиенте Azure AD B2C, можно использовать текущий интерфейс **приложений** или новый объединенный интерфейс **Регистрация приложений (предварительная версия)**. [См. дополнительные сведения о новом интерфейсе](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Приложения](#tab/applications/)

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите фильтр **Каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure AD B2C**. Либо щелкните **Все службы**, а затем найдите и выберите **Azure AD B2C**
1. Выберите **приложения**и нажмите кнопку **Добавить**.
1. Введите имя приложения. Например, *testapp1*.
1. Для поля **Включить веб-приложение или веб-интерфейс API** выберите **Да**.
1. В качестве **URL-адреса ответа**введите`https://jwt.ms`
1. Нажмите кнопку **создания**.

#### <a name="app-registrations-preview"></a>[Регистрация приложений (предварительная версия)](#tab/app-reg-preview/)

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите фильтр **Каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure AD B2C**. Либо щелкните **Все службы**, а затем найдите и выберите **Azure AD B2C**
1. Щелкните **Регистрация приложений (предварительная версия)** и выберите **Новая регистрация**.
1. Введите **имя** приложения. Например, *testapp1*.
1. Выберите **учетные записи в любом организационном каталоге или любом поставщике удостоверений**.
1. В разделе **URI перенаправления**выберите **веб**, а затем `https://jwt.ms` введите в текстовом поле URL-адрес.
1. В разделе **Разрешения** установите флажок *Предоставьте согласие администратора для разрешений openid и offline_access*.
1. Выберите **Зарегистрировать**.

После завершения регистрации приложения включите поток неявного предоставления:

1. В разделе **Управление** выберите **Проверка подлинности**.
1. Выберите **Попробовать новый пользовательский интерфейс** (если этот параметр показан).
1. В разделе **неявное предоставление**установите флажки **маркеры доступа** и **идентификаторы маркеров** .
1. Щелкните **Сохранить**.