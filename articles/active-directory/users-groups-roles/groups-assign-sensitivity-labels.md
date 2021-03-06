---
title: Назначение меток чувствительности группам — Azure AD | Документация Майкрософт
description: Сведения о создании правил членства для автоматического заполнения групп и о ссылках на эти правила.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/24/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e4dabad5057fda39fe3753c810a85e6aeb55b3a
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82582953"
---
# <a name="assign-sensitivity-labels-to-office-365-groups-in-azure-active-directory-preview"></a>Назначение меток чувствительности группам Office 365 в Azure Active Directory (Предварительная версия)

Azure Active Directory (Azure AD) поддерживает применение меток чувствительности, опубликованных [центром соответствия Microsoft 365](https://sip.protection.office.com/homepage) , в группах Office 365. Метки чувствительности применяются к группам в разных службах, например Outlook, Microsoft Teams и SharePoint. Эта функция сейчас доступна в виде общедоступной предварительной версии. Дополнительные сведения о поддержке приложений Office 365 см. в разделе [Поддержка меток конфиденциальности в office 365](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites#support-for-the-sensitivity-labels).

> [!IMPORTANT]
> Чтобы настроить эту функцию, в Организации Azure AD должна быть по крайней мере одна активная лицензия Azure Active Directory Premium P1.

## <a name="enable-sensitivity-label-support-in-powershell"></a>Включение поддержки меток чувствительности в PowerShell

Чтобы применить опубликованные метки к группам, сначала необходимо включить эту функцию. Эти действия включают функцию в Azure AD.

1. Откройте окно Windows PowerShell на компьютере. Его можно открыть без более высокого уровня привилегий.
1. Выполните следующие команды, чтобы подготовить среду к выполнению командлетов.

    ```PowerShell
    Import-Module AzureADPreview
    Connect-AzureAD
    ```

    На странице **входа в учетную запись** введите учетную запись администратора и пароль, чтобы подключиться к службе, и выберите **Вход**.
1. Получение текущих параметров группы для Организации Azure AD.

    ```PowerShell
    $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
    ```

    > [!NOTE]
    > Если для этой Организации Azure AD не были созданы параметры группы, необходимо сначала создать параметры. Выполните действия, описанные в [Azure Active Directory командлетах для настройки параметров группы](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-cmdlets) , чтобы создать параметры группы для этой Организации Azure AD.

1. Затем отобразите текущие параметры группы.

    ```PowerShell
    $Setting.Values
    ```

1. Затем включите эту функцию:

    ```PowerShell
    $Setting["EnableMIPLabels"] = "True"
    ```

1. Затем сохраните изменения и примените параметры.

    ```PowerShell
    Set-AzureADDirectorySetting -Id $Setting.Id -DirectorySetting $Setting
    ```

Вот и все. Вы включили функцию и можете применить опубликованные метки к группам.

## <a name="assign-a-label-to-a-new-group-in-azure-portal"></a>Назначение метки новой группе в портал Azure

1. Войдите в [центр администрирования Azure AD](https://aad.portal.azure.com).
1. Выберите **группы**и щелкните **Новая группа**.
1. На странице **Новая группа** выберите **Office 365**, а затем введите необходимые сведения для новой группы и выберите в списке метку с учетом конфиденциальности.

   ![Назначение метки чувствительности на странице «Создание групп»](./media/groups-assign-sensitivity-labels/new-group-page.png)

1. Сохраните изменения и нажмите кнопку **создать**.

Ваша группа будет создана, а параметры сайта и группы, связанные с выбранной меткой, будут автоматически применены.

## <a name="assign-a-label-to-an-existing-group-in-azure-portal"></a>Назначение метки существующей группе в портал Azure

1. Войдите в [центр администрирования Azure AD](https://aad.portal.azure.com) с помощью учетной записи администратора групп или владельца группы.
1. Выберите **Группы**.
1. На странице **все группы** выберите группу, для которой нужно добавить метку.
1. На странице выбранной группы выберите **Свойства** и в списке выберите метку чувствительности.

   ![Назначение метки чувствительности на странице обзора для группы](./media/groups-assign-sensitivity-labels/assign-to-existing.png)

1. Выберите **Сохранить**, чтобы сохранить изменения.

## <a name="remove-a-label-from-an-existing-group-in-azure-portal"></a>Удаление метки из существующей группы в портал Azure

1. Войдите в [центр администрирования Azure AD](https://aad.portal.azure.com) с помощью учетной записи администратора глобального администратора или группы или владельца группы.
1. Выберите **Группы**.
1. На странице **все группы** выберите группу, из которой нужно удалить метку.
1. На странице **Группа** выберите **свойства**.
1. Выберите **Удалить**.
1. Щелкните **Сохранить**, чтобы применить изменения.

## <a name="using-classic-azure-ad-classifications"></a>Использование классических классификаций Azure AD

После включения этой функции "классические" классификации для групп будут отображаться только существующие группы и сайты. их следует использовать только для новых групп, если создание групп в приложениях, которые не поддерживают метки чувствительности. При необходимости администратор может впоследствии преобразовать их в метки чувствительности. Классические классификации — это старые классификации, которые вы настроили, определив значения для `ClassificationList` параметра в Azure AD PowerShell. Если эта функция включена, эти классификации не будут применяться к группам.

## <a name="troubleshooting-issues"></a>Устранение неполадок

### <a name="sensitivity-labels-are-not-available-for-assignment-on-a-group"></a>Метки чувствительности недоступны для назначения в группе

Параметр метка конфиденциальности отображается только для групп при соблюдении всех следующих условий.

1. Метки публикуются в центре соответствия Microsoft 365 для этой Организации Azure AD.
1. Эта функция включена, в PowerShell для Енаблемиплабелс задано значение true.
1. Группа является группой Office 365.
1. У Организации есть активная лицензия Azure Active Directory Premium P1.
1. Текущий пользователь, выполнивший вход, имеет достаточные привилегии для назначения меток. Пользователь должен быть либо глобальным администратором, либо администратором группы, либо владельцем группы.

Убедитесь, что выполнены все условия для назначения меток группе.

### <a name="the-label-i-want-to-assign-is-not-in-the-list"></a>Метка, которую требуется назначить, отсутствует в списке

Если искомая метка отсутствует в списке, это может быть вызвано одной из следующих причин:

- Метка может не публиковаться в центре соответствия Microsoft 365. Это также может применяться к меткам, которые больше не публикуются. Для получения дополнительных сведений обратитесь к администратору.
- Метка может быть опубликована, однако она недоступна для пользователя, выполнившего вход. Обратитесь к администратору за дополнительными сведениями о том, как получить доступ к метке.

### <a name="how-to-change-the-label-on-a-group"></a>Изменение метки в группе

Метки можно переключать в любое время, используя те же действия, что и при назначении метки существующей группе, следующим образом:

1. Войдите в [центр администрирования Azure AD](https://aad.portal.azure.com) с помощью учетной записи глобального пользователя или администратора группы или владельца группы.
1. Выберите **Группы**.
1. На странице **все группы** выберите группу, для которой нужно добавить метку.
1. На странице выбранной группы выберите **Свойства** и выберите в списке новую метку чувствительности.
1. Щелкните **Сохранить**.

### <a name="group-setting-changes-to-published-labels-are-not-updated-on-the-groups"></a>Изменения параметров группы для опубликованных меток не обновлены в группах

Рекомендуется изменять параметры группы для метки после применения метки к группам, как рекомендуется. При внесении изменений в параметры групп, связанные с опубликованными метками в [Microsoft 365 центре соответствия требованиям](https://sip.protection.office.com/homepage), эти изменения политики не применяются к затронутым группам автоматически.

Если необходимо внести изменения, используйте [Скрипт Azure AD PowerShell](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1) , чтобы вручную применить обновления к затронутым группам. Этот метод гарантирует, что все существующие группы применяют новый параметр.

## <a name="next-steps"></a>Дальнейшие действия

- [Используйте метки чувствительности к Microsoft Teams, группам Office 365 и сайтам SharePoint](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites)
- [Обновление групп после изменения политики меток вручную с помощью скрипта Azure AD PowerShell](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1)
- [Изменение параметров группы](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-settings-azure-portal)
- [Управление группами с помощью команд PowerShell](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-v2-cmdlets)
