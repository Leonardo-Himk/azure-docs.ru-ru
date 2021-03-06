---
title: Условный доступ — блокировка доступа — Azure Active Directory
description: Создание настраиваемой политики условного доступа для
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb,
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2834fd3d4901b6394eabe000f9efc572c2efd497
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80755084"
---
# <a name="conditional-access-block-access"></a>Условный доступ: Блокировка доступа

Для организаций с консервативным подходом к миграции в облако можно использовать политику «блокировать все». 

> [!CAUTION]
> Неправильная настройка политики блокировки может привести к блокировке организаций из портал Azure.

Такие политики могут иметь непредвиденные побочные эффекты. Правильное тестирование и проверка крайне важны перед включением. При внесении изменений администраторы должны использовать такие средства, как [режим "только отчет с условным доступом](concept-conditional-access-report-only.md) [" и средство What If в условном доступе](what-if-tool.md) .

## <a name="user-exclusions"></a>Исключения пользователей

Политики условного доступа — это мощные средства, поэтому рекомендуется исключить из политики следующие учетные записи:

* Учетные записи **аварийного доступа** или **прозрачного стекла** для предотвращения блокировки учетной записи на уровне клиента. В маловероятной ситуации, когда все администраторы заблокированы для вашего клиента, можно использовать учетную запись администратора аварийного доступа для входа в клиент, чтобы выполнить действия по восстановлению доступа.
   * Дополнительные сведения можно найти в статье [Управление учетными записями аварийного доступа в Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Учетные записи служб** и **субъекты-службы**, например учетная запись синхронизации Azure AD Connect. Учетные записи служб являются неинтерактивными учетными записями, которые не привязаны к какому-либо конкретному пользователю. Они обычно используются серверными службами для предоставления программного доступа к приложениям, но также используются для входа в системы в целях администрирования. Учетные записи службы, такие как, должны быть исключены, так как MFA невозможно завершить программным способом. Вызовы, выполняемые субъектами-службами, не блокируются условным доступом.
   * Если в вашей организации эти учетные записи используются в сценариях или коде, рассмотрите возможность замены их [управляемыми удостоверениями](../managed-identities-azure-resources/overview.md). В качестве временного решения можно исключить эти конкретные учетные записи из базовой политики.

## <a name="create-a-conditional-access-policy"></a>Создание политики условного доступа

Следующие шаги помогут создать политики условного доступа, чтобы заблокировать доступ ко всем приложениям, кроме [Office 365](concept-conditional-access-cloud-apps.md#office-365-preview) , если пользователи не находятся в доверенной сети. Эти политики помещаются в [режим «только отчет»](howto-conditional-access-report-only.md) для запуска, чтобы администраторы могли определить воздействие, которое они будут иметь на существующих пользователях. Когда администраторы имеют опыт работы с политиками по мере их применения, они могут переключаться **на.**

Первая политика блокирует доступ ко всем приложениям, кроме приложений Office 365, если они не находятся в надежном расположении.

1. Войдите в **портал Azure** в качестве глобального администратора, администратора безопасности или администратора условного доступа.
1. Перейдите к **Azure Active Directory** > **Безопасность** > **условного доступа**.
1. Выберите **создать политику**.
1. Присвойте политике имя. Корпорация Майкрософт рекомендует организациям создавать осмысленные стандартные имена политик.
1. В разделе **Назначения** выберите **Пользователи и группы**.
   1. В разделе **включить**выберите **все пользователи**.
   1. В разделе **исключить**выберите **Пользователи и группы** и выберите учетные записи с аварийным доступом или с непрозрачным разделением вашей организации. 
   1. Нажмите кнопку **Готово**.
1. В разделе **облачные приложения или действия**выберите следующие параметры.
   1. В разделе **включить**выберите **все облачные приложения**.
   1. В разделе **исключить**выберите **Office 365 (Предварительная версия)**, выберите **выбрать**, а затем нажмите кнопку **Готово**.
1. В области **условия**:
   1. В поле**Расположение** **условий** > .
      1. Задайте для параметра **Настройка** значение **Да** .
      1. В разделе **включить**выберите **любое расположение**.
      1. В разделе **исключить**выберите **все надежные расположения**.
      1. Нажмите кнопку **Готово**.
   1. В разделе **клиентские приложения (Предварительная версия)** задайте для параметра **Настройка** значение **Да**и **нажмите кнопку** **Готово**.
1. В разделе **Управление** > доступом**предоставление**выберите **блокировать доступ**, а затем выберите **выбрать**.
1. Подтвердите параметры и установите **флажок Включить политику** **только для отчетов**.
1. Выберите **создать** , чтобы создать, чтобы включить политику.

Вторая политика создается ниже, чтобы требовать многофакторную проверку подлинности или соответствующее устройство для пользователей Office 365.

1. Выберите **создать политику**.
1. Присвойте политике имя. Корпорация Майкрософт рекомендует организациям создавать осмысленные стандартные имена политик.
1. В разделе **Назначения** выберите **Пользователи и группы**.
   1. В разделе **включить**выберите **все пользователи**.
   1. В разделе **исключить**выберите **Пользователи и группы** и выберите учетные записи с аварийным доступом или с непрозрачным разделением вашей организации. 
   1. Нажмите кнопку **Готово**.
1. В разделе **облачные приложения или действия** > **выберите пункт** **выбрать приложения**, выберите **Office 365 (Предварительная версия)** и нажмите **кнопку Выбрать**, а затем **Готово**.
1. В разделе **Управление** > доступом**предоставить**выберите **предоставить доступ**.
   1. Выберите **требовать многофакторную проверку подлинности** и **требовать, чтобы устройство было помечено как соответствующее** , выберите **выбрать**.
   1. Убедитесь, что выбран параметр **требовать все выбранные элементы управления** .
   1. Щелкните **Выбрать**.
1. Подтвердите параметры и установите **флажок Включить политику** **только для отчетов**.
1. Выберите **создать** , чтобы создать, чтобы включить политику.

## <a name="next-steps"></a>Дальнейшие шаги

[Общие политики условного доступа](concept-conditional-access-policy-common.md)

[Определение влияния с использованием режима "только отчет с условным доступом"](howto-conditional-access-report-only.md)

[Моделирование поведения входа с помощью средства What If условного доступа](troubleshoot-conditional-access-what-if.md)
