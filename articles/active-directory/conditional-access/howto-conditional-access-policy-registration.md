---
title: Условный доступ — объединенные сведения о безопасности — Azure Active Directory
description: Создание настраиваемой политики условного доступа для регистрации сведений безопасности
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c8081bb8145a6654c168fb2d664e1666b32dc18
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81457915"
---
# <a name="conditional-access-securing-security-info-registration"></a>Условный доступ: защита регистрации сведений безопасности

Теперь, когда и как пользователи регистрируются в службе многофакторной идентификации Azure и самостоятельного сброса пароля, вы можете использовать действия пользователя в политике условного доступа. Эта предварительная версия функции доступна для организаций, которые включили [объединенную предварительную версию регистрации](../authentication/concept-registration-mfa-sspr-combined.md). Эта функция может быть включена в организациях, где они хотят использовать такие условия, как доверенное сетевое расположение, чтобы ограничить доступ для регистрации в службе многофакторной идентификации Azure и самостоятельного сброса пароля (SSPR). Дополнительные сведения о возможных условиях см. в статье [Условный доступ: условия](concept-conditional-access-conditions.md).

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Создание политики, требующей регистрации из надежного расположения

Следующая политика применяется ко всем выбранным пользователям, которые пытаются зарегистрироваться с помощью Объединенной функции регистрации, и блокирует доступ, если они не подключены из расположения, помеченного как Доверенная сеть.

1. В **портал Azure**перейдите к **Azure Active Directory** > **Безопасность** > **Условный доступ**.
1. Выберите **создать политику**.
1. В списке Имя введите имя политики. Например, **Объединенная регистрация сведений о безопасности в доверенных сетях**.
1. В разделе **назначения**выберите **Пользователи и группы**, а затем выберите пользователей и группы, к которым должна применяться эта политика.

   > [!WARNING]
   > Пользователи должны быть включены для [совместной регистрации](../authentication/howto-registration-mfa-sspr-combined.md).

1. В разделе **облачные приложения или действия**выберите **действия пользователя**, установите флажок **зарегистрировать сведения о безопасности**.
1. В области**расположения** **условий** > .
   1. Настройте **Да**.
   1. Включите **любое расположение**.
   1. Исключите **все надежные расположения**.
   1. В колонке расположения выберите **Готово** .
   1. Выберите **Готово** в колонке условия.
1. В разделе **условия** > **клиентские приложения (Предварительная версия)** задайте для параметра **настроить** значение **Да**и нажмите кнопку **Готово**.
1. В разделе **элементы управления** > доступом**Предоставьте**.
   1. Выберите **Заблокировать доступ**.
   1. Затем щелкните **Выбрать**.
1. Для параметра **Включить политику** задайте значение **Вкл**.
1. Затем нажмите кнопку **Save** (Сохранить).

На шаге 6 этой политики у организаций есть варианты, которые они могут сделать. Приведенная выше политика требует регистрации из надежного сетевого расположения. Организации могут использовать любые доступные условия вместо **расположений**. Помните, что эта политика блокируется, так что все включенные объекты блокируются и разрешены все, что не соответствует этим параметрам. 

Некоторые из них могут использовать состояние устройства вместо расположения на шаге 6.

6. В разделе **условия** > **состояние устройства (Предварительная версия)**.
   1. Настройте **Да**.
   1. Включить **все состояния устройств**.
   1. Исключить **гибридное устройство, присоединенное к Azure AD** , или **устройство помечено как соответствующее требованиям**
   1. В колонке расположения выберите **Готово** .
   1. Выберите **Готово** в колонке условия.

> [!WARNING]
> Если вы используете состояние устройства в качестве условия в политике, это может повлиять на гостевых пользователей в каталоге. [Режим «только отчет»](concept-conditional-access-report-only.md) может помочь определить влияние решений политики.

## <a name="next-steps"></a>Дальнейшие шаги

[Общие политики условного доступа](concept-conditional-access-policy-common.md)

[Определение влияния с использованием режима "только отчет с условным доступом"](howto-conditional-access-report-only.md)

[Моделирование поведения входа с помощью средства What If условного доступа](troubleshoot-conditional-access-what-if.md)
