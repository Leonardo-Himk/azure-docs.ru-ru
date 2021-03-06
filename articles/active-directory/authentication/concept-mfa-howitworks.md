---
title: Обзор многофакторной идентификации Azure
description: Узнайте, как многофакторная идентификация Azure помогает защитить доступ к данным и приложениям, а также выполнять простой процесс входа в систему.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: c50232abd12c8c0390409bd7bf72833b4f153e02
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80667359"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Как это работает: служба Многофакторной идентификации Azure

Многофакторная идентификация — это процесс, при котором пользователю для входа в систему предлагается дополнительная форма идентификации, например ввод кода с мобильного телефона или сканирование отпечатков пальцев.

Если для проверки подлинности пользователя используется только пароль, система подвергается риску атаки. Если пароль ненадежен или скомпрометирован, как вы сможете отличить пользователя от злоумышленника при входе по имени пользователя или паролю? Требование применять второй метод проверки подлинности повышает безопасность, так как злоумышленнику будет нелегко получить или скопировать дополнительный фактор проверки.

![Схема применения нескольких вариантов многофакторной проверки подлинности](./media/concept-mfa-howitworks/methods.png)

Многофакторная идентификация Azure требует использовать два или несколько методов проверки подлинности:

* что-то, известное только вам (обычно это пароль);
* что-то, что у вас есть, например доверенное устройство, которое нельзя легко скопировать, например телефон или аппаратный ключ;
* ваша персональная характеристика, например, отпечаток пальца или изображение лица.

Пользователи могут самостоятельно регистрироваться для самостоятельного сброса пароля и применения Многофакторной идентификации Azure, выполнив всего один шаг. Эта возможность позволяет быстро начать работу. Администраторы могут выбирать формы дополнительной проверки подлинности. Многофакторную идентификацию Azure можно также назначить обязательным действием при самостоятельном сбросе пароля, чтобы дополнительно защитить этот процесс.

![Методы проверки подлинности, используемые на экране входа](media/concept-authentication-methods/overview-login.png)

Многофакторная идентификация Azure помогает защитить доступ к данным и приложениям, одновременно обеспечивая простоту для пользователей. Эта служба предлагает дополнительную защиту за счет дополнительного способа аутентификации и, используя ряд простых [методов проверки подлинности](concept-authentication-methods.md), обеспечивает строгую проверку. Направление пользователям запросов о необходимости Многофакторной идентификации зависит от настроек, заданных администратором.

Приложениям или службам не нужно вносить какие-либо изменения для использования многофакторной идентификации Azure. Запросы на проверку являются частью события входа в Azure AD, которое при необходимости автоматически запрашивает и обрабатывает запрос MFA.

## <a name="available-verification-methods"></a>Доступные методы проверки

Когда пользователь входит в приложение или службу и получает запрос MFA, он может выбрать одну из зарегистрированных форм дополнительной проверки. Администратор может потребовать регистрацию этих методов проверки многофакторной идентификации Azure, или пользователь может получить доступ к своему [моему профилю](https://myprofile.microsoft.com) , чтобы изменить или добавить методы проверки.

С многофакторной идентификацией Azure можно использовать следующие дополнительные формы проверки.

* Приложение Microsoft Authenticator
* Токен оборудования OATH
* SMS
* Голосовой звонок

## <a name="how-to-enable-and-use-azure-multi-factor-authentication"></a>Как включить и использовать многофакторную идентификацию Azure

Пользователи и группы могут включить многофакторную идентификацию Azure для запроса дополнительной проверки во время события входа. [Параметры безопасности по умолчанию](../fundamentals/concept-fundamentals-security-defaults.md) доступны для всех клиентов Azure AD, чтобы быстро разрешить использование приложения Microsoft Authenticator для всех пользователей.

Для более детализированных элементов управления политики [условного доступа](../conditional-access/overview.md) можно использовать для определения событий или приложений, требующих mfa. Эти политики могут разрешать обычные события входа, когда пользователь находится в корпоративной сети или зарегистрированном устройстве, но запрашивает дополнительные факторы проверки при удаленном или личном устройстве.

![Обзорная схема защиты процесса входа с помощью условного доступа](media/tutorial-enable-azure-mfa/conditional-access-overview.png)

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о лицензировании см. в статье [функции и лицензии для службы многофакторной идентификации Azure](concept-mfa-licensing.md).

Чтобы увидеть MFA в действии, включите многофакторную идентификацию Azure для набора тестовых пользователей в следующем учебнике:

> [!div class="nextstepaction"]
> [Включение Многофакторной идентификации Azure](tutorial-mfa-applications.md)
