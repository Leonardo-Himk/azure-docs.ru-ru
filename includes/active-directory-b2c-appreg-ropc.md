---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: cea3245176e6c38137d68e3ad4b47477bedc78be
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80529190"
---
Чтобы зарегистрировать приложение в клиенте Azure AD B2C, можно использовать текущий интерфейс **приложений** или новый объединенный интерфейс **Регистрация приложений (предварительная версия)**. [См. дополнительные сведения о новом интерфейсе](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Приложения](#tab/applications/)

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите фильтр **Каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure AD B2C**. Либо щелкните **Все службы**, а затем найдите и выберите **Azure AD B2C**
1. Выберите **приложения**и нажмите кнопку **Добавить**.
1. Введите имя приложения. Например, *ROPC_Auth_app*.
1. Для поля **Собственный клиент** выберите **Да**.
1. Оставьте остальные значения, а затем нажмите кнопку **создать**.
1. Запишите **идентификатор приложения**. Он вам потребуется в дальнейшем.

#### <a name="app-registrations-preview"></a>[Регистрация приложений (предварительная версия)](#tab/app-reg-preview/)

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите фильтр **Каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure AD B2C**. Либо щелкните **Все службы**, а затем найдите и выберите **Azure AD B2C**
1. Щелкните **Регистрация приложений (предварительная версия)** и выберите **Новая регистрация**.
1. Введите **имя** приложения. Например, *ROPC_Auth_app*.
1. Оставьте остальные значения, а затем нажмите кнопку **зарегистрировать**.
1. Запишите значение параметра **Идентификатор приложения (клиент)**. Оно вам потребуется в дальнейшем.
1. В разделе **Управление** выберите **Проверка подлинности**.
1. Выберите **Попробовать новый пользовательский интерфейс** (если этот параметр показан).
1. В разделе **Тип клиента по умолчанию**выберите **Да** , чтобы приложение обрабатывалось как общедоступный клиент. Этот параметр является обязательным для потока РОПК.
1. Щелкните **Сохранить**.
1. В меню слева выберите **Манифест** , чтобы открыть редактор манифеста. 
1. Присвойте **oauth2AllowImplicitFlow** атрибуту oauth2AllowImplicitFlow *значение true*:
    ```json
    "oauth2AllowImplicitFlow": true,
    ```
1. Щелкните **Сохранить**.