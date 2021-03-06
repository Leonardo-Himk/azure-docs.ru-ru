---
title: Использование самостоятельного доступа к приложениям | Документация Майкрософт
description: Включите самостоятельный доступ к приложениям, позволяющий пользователям найти свои приложения
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55da8731855c8afda496edff33f3fbb7982cd44b
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "65784553"
---
# <a name="how-to-use-self-service-application-access"></a>Использование самостоятельного доступа к приложениям

Прежде чем пользователи смогут самостоятельно находить приложения на панели доступа, необходимо включить **самостоятельный доступ к приложениям**, чтобы пользователи либо искали их сами, либо запрашивали к ним доступ.

Эта функция представляет собой отличный способ сэкономить время и деньги в ИТ-подразделении. Мы настоятельно рекомендуем использовать ее в развертывании современных приложений с помощью Azure Active Directory.

Это позволит вам:

-   Разрешить пользователям самостоятельно находить приложения на [панели доступа к приложениям](https://myapps.microsoft.com/) без обращения к специалистам ИТ-подразделения.

-   Добавьте этих пользователей к предварительно настроенным группам, чтобы видеть, кто запросил доступ, удалять доступ и управлять назначенными ролями.

-   При необходимости разрешать корпоративному утверждающему подтверждать запросы на доступ к приложениям без привлечения ИТ-подразделения.

-   При необходимости настраивать для не более 10 сотрудников возможность разрешать доступ к этому приложению.

-   При необходимости разрешать корпоративному утверждающему задавать пароли пользователей для входа в приложение прямо с [панели доступа к приложению](https://myapps.microsoft.com/).

-   При необходимости автоматически назначать пользователей, находящихся на самообслуживании, роли приложения напрямую.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Включите самостоятельный доступ к приложениям, позволяющий пользователям найти свои приложения

Самостоятельный доступ к приложениям — это отличный способ разрешить пользователям самим находить приложения и при необходимости позволять бизнес-группе утверждать доступ к этим приложениям. Вы можете разрешить бизнес-группе управлять учетными данными пользователей для приложений, использующих единый вход по паролю, прямо на панелях доступа.

Чтобы включить для приложения самостоятельный доступ к приложениям, сделайте следующее:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор.**

2. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4. Щелкните **Корпоративные приложения** в меню навигации Azure Active Directory слева.

5. Щелкните **Все приложения**, чтобы открыть полный список приложений.

   * Если нужное приложение не отображается здесь, используйте элемент управления **Фильтр** в верхней части **списка все приложения** и задайте для параметра **Показать** значение **все приложения.**

6. Выберите из списка приложение, для которого необходимо включить функцию самостоятельного доступа.

7. После загрузки приложения щелкните **Самообслуживание** в меню навигации слева.

8. Чтобы включить самостоятельный доступ к приложениям для этого приложения, установите переключатель **Разрешить пользователям запрашивать доступ к этому приложению?** в состояние **Да**.

9. Затем выберите группу, в которую необходимо добавить пользователей, запрашивающих доступ к этому приложению, щелкните селектор рядом с меткой **К какой группе следует добавить назначенных пользователей?** и выберите группу.

10. **Необязательно.** Если вы хотите запрашивать бизнес-утверждение для предоставления пользователям доступа, установите переключатель **Требовать утверждение перед предоставлением доступа к этому приложению?** в состояние **Да**.

11. **(Необязательно.) В приложениях, использующих единый вход по паролю,** при необходимости можно разрешить бизнес-утверждениям указывать пароли, которые отправляются приложению для утвержденных пользователей. Для этого установите переключатель **Разрешить утверждающим лицам задавать пароли пользователей для этого приложения?** в положение **Да**.

12. **Необязательно.** Чтобы указать утверждающие лица, которым разрешено утверждать доступ к этому приложению, щелкните селектор рядом с меткой **Кому разрешено утверждать доступ к этому приложению?** и выберите до 10 отдельных утверждающих лиц.

    * Группы не поддерживаются.

13. **Необязательно.** **В приложениях, предоставляющих роли,** при необходимости можно назначить самостоятельно утвержденных пользователей для роли, щелкнув селектор рядом с меткой **Какую роль следует назначить пользователям в этом приложении?** и выбрав нужную роль.

14. Нажмите кнопку **Сохранить** в верхней части колонки, чтобы завершить процедуру.

После завершения настройки самостоятельного доступа к приложению пользователи могут перейти на [панель доступа к приложениям](https://myapps.microsoft.com/) и нажать кнопку **Добавить**, чтобы найти приложения, для которых включен самостоятельный доступ. Чтобы просмотреть уведомления для бизнес-утверждений, [используйте панель доступа к приложениям](https://myapps.microsoft.com/). Вы можете получать уведомления по электронной почте, когда пользователь запрашивает доступ к приложению, требующему утверждения. 

Эти утверждения поддерживают только отдельные рабочие процессы, то есть если вы зададите несколько утверждающих лиц, отдельное утверждающее лицо может подтвердить доступ к приложению.

## <a name="next-steps"></a>Дальнейшие действия
[Настройка Azure Active Directory для самостоятельного управления группами](../users-groups-roles/groups-self-service-management.md)
