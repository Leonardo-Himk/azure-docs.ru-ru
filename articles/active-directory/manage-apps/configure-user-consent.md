---
title: Настройка согласия конечных пользователей для приложений, использующих Azure AD
description: Узнайте, как и когда пользователи могут согласиться на приложения, которые будут иметь доступ к данным вашей организации.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: mimart
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42337fe958a881ee263d16c866dda69f13fe09c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80519622"
---
# <a name="configure-how-end-users-consent-to-applications"></a>Настройка согласия конечных пользователей для приложений

Приложения могут интегрироваться с платформой идентификации Майкрософт, чтобы пользователи могли входить с помощью рабочей или учебной учетной записи в Azure Active Directory (Azure AD), а также получать доступ к данным Организации для предоставления обширных возможностей, управляемых данными. Различные разрешения позволяют приложению различного уровня доступа к данным пользователей и вашей организации.

По умолчанию пользователи могут получать согласие на доступ приложений к данным Организации, хотя только для некоторых разрешений. Например, по умолчанию пользователь может предоставить приложению разрешение на доступ к своему почтовому ящику или к беседам групп для команды, которой владеет пользователь, но не может предоставить пользователю разрешение на чтение и запись на всех сайтах SharePoint в Организации. Хотя предоставление пользователям разрешения на предоставление пользователям возможности легко получать полезные приложения, которые интегрируются с Microsoft 365, Azure и другими службами, это может представлять риск, если он не используется и не отслеживается осторожно.

Корпорация Майкрософт рекомендует отключить будущие операции предоставления согласия пользователя, чтобы сократить контактную зону и снизить риск. Если согласие пользователя отключено, предыдущее предоставление согласия будет по-прежнему учитываться, но все будущие операции согласия должны выполняться администратором. Пользователь может запросить согласие администратора на уровне клиента через встроенный [Рабочий процесс запроса согласия администратора](configure-admin-consent-workflow.md) или с помощью собственных процессов поддержки. Дополнительные сведения см. [в пяти шагах по защите инфраструктуры удостоверений](../../security/fundamentals/steps-secure-identity.md) .

## <a name="configure-user-consent-to-applications"></a>Настройка согласия пользователя для приложений
### <a name="disable-or-enable-user-consent-from-the-azure-portal"></a>Отключение или включение согласия пользователя в портал Azure

Вы можете использовать портал Azure, чтобы отключить или разрешить пользователям предоставлять разрешения на доступ к данным вашей организации для приложений.

1. Войдите в [портал Azure](https://portal.azure.com) в качестве [глобального администратора](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator).
2. Выберите **Azure Active Directory**, **корпоративные приложения**, а затем — **Параметры пользователя**.
3. Включение или отключение согласия пользователя с помощью элемента управления с меткой **пользователи могут получать согласие на доступ к приложениям, обращающимся к корпоративным данным от их имени**.
4. Используемых Настройте [Рабочий процесс запроса согласия администратора](configure-admin-consent-workflow.md) , чтобы убедиться, что пользователи, которым не разрешено согласие с приложением, могут запрашивать утверждение.

> [!TIP]
> Чтобы разрешить пользователям запрашивать проверку приложения, к которому пользователю не разрешено согласие (например, если согласие пользователя отключено или приложение запрашивает разрешения, которые пользователь не может предоставить), рассмотрите возможность [настройки рабочего процесса согласия администратора](configure-admin-consent-workflow.md).

### <a name="disable-or-enable-user-consent-using-powershell"></a>Отключение или включение согласия пользователя с помощью PowerShell

Вы можете использовать модуль Azure AD PowerShell v1 версии 1 ([MSOnline](https://docs.microsoft.com/powershell/module/msonline/?view=azureadps-1.0)), чтобы включить или отключить возможность пользователей получать разрешения на доступ к данным Организации.

1. Войдите в свою организацию, выполнив следующий командлет:

    ```powershell
    Connect-MsolService
    ```

2. Проверьте, включено ли согласие пользователя, запустив этот командлет:

    ```powershell
    Get-MsolCompanyInformation | Format-List UsersPermissionToUserConsentToAppEnabled
    ```

3. Включение или отключение согласия пользователя. Например, чтобы отключить согласие пользователя, выполните следующий командлет:

    ```powershell
    Set-MsolCompanySettings -UsersPermissionToUserConsentToAppEnabled $false
    ```

## <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Настройка согласия владельца группы на приложения, обращающиеся к данным группы

> [!IMPORTANT]
> Ниже приведены сведения о предстоящей функции, которая позволит владельцам групп предоставлять приложениям доступ к данным своих групп. После выпуска этой возможности она будет включена по умолчанию. Несмотря на то что эта функция еще не выпускается широко, вы можете использовать эти инструкции, чтобы отключать возможности заранее.

Владельцы групп могут выполнять авторизацию приложений (например, приложений, опубликованных сторонними поставщиками) для доступа к данным Организации, связанным с группой. Например, владелец группы (который является владельцем группы Office 365 для группы) может разрешить приложению читать все сообщения групп в группе или составить список базовых профилей членов группы.

> [!NOTE]
> Независимо от этого параметра, владельцу группы всегда разрешено добавлять других пользователей или приложения непосредственно в качестве владельцев групп.

### <a name="configure-group-owner-consent-using-powershell"></a>Настройка согласия владельца группы с помощью PowerShell

Вы можете использовать модуль предварительной версии Azure AD PowerShell ([AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)), чтобы разрешить или запретить владельцам групп предоставлять согласие на доступ к приложениям, обращающимся к данным Организации, для групп, которыми они владеют.

1. Убедитесь, что вы используете модуль [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) (этот шаг важен, если вы установили модуль [AzureAD](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0) и модуль [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) ).

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

2. Подключитесь к Azure AD PowerShell.

   ```powershell
   Connect-AzureAD
   ```

3. Получение текущего значения параметров *политики согласия* в клиенте. Для этого необходимо проверить, были ли созданы параметры каталога для этой функции, и, если нет, использовать значения из соответствующего шаблона параметров каталога.

    ```powershell
    $consentSettingsTemplateId = "dffd5d46-495d-40a9-8e21-954ff55e198a" # Consent Policy Settings
    $settings = Get-AzureADDirectorySetting -All $true | Where-Object { $_.TemplateId -eq $consentSettingsTemplateId }

    if (-not $settings) {
        $template = Get-AzureADDirectorySettingTemplate -Id $consentSettingsTemplateId
        $settings = $template.CreateDirectorySetting()
    }

    $enabledValue = $settings.Values | ? { $_.Name -eq "EnableGroupSpecificConsent" }
    $limitedToValue = $settings.Values | ? { $_.Name -eq "ConstrainGroupSpecificConsentToMembersOfGroupId" }
    ```

4. Изучите значения параметров. Есть два значения параметров, которые определяют, какие пользователи смогут предоставлять приложению доступ к данным своей группы:

    | Параметр       | Тип         | Описание  |
    | ------------- | ------------ | ------------ |
    | _енаблеграупспеЦификконсент_   | логический |  Флаг, указывающий, разрешено ли владельцам групп предоставлять разрешения для конкретной группы. |
    | _констраинграупспеЦификконсенттомемберсофграупид_ | Guid | Если для _енаблеграупспеЦификконсент_ задано значение "true", а для этого значения задан идентификатор объекта группы, члены указанной группы будут иметь права на предоставление разрешений, относящихся к группам. |

5. Обновите значения параметров для требуемой конфигурации:

    ```powershell
    # Disable group-specific consent entirely
    $enabledValue.Value = "False"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for all users
    $enabledValue.Value = "True"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for users in a given group
    $enabledValue.Value = "True"
    $limitedToValue.Value = "{group-object-id}"
    ```

6. Сохраните параметры.

    ```powershell
    if ($settings.Id) {
        # Update an existing directory settings
        Set-AzureADDirectorySetting -Id $settings.Id -DirectorySetting $settings
    } else {
        # Create a new directory settings to override the default setting 
        New-AzureADDirectorySetting -DirectorySetting $settings
    }
    ```

## <a name="configure-risk-based-step-up-consent"></a>Настройка согласия на действие на основе риска

Согласие на повышение уровня риска позволяет снизить уязвимость пользователя вредоносными приложениями, делающими [незаконные запросы на согласие](https://docs.microsoft.com/microsoft-365/security/office-365-security/detect-and-remediate-illicit-consent-grants). Если корпорация Майкрософт обнаружит рискованный запрос согласия конечного пользователя, запрос потребует «пошаговое завершение» для предоставления согласия администратора. Эта возможность включена по умолчанию, но это приведет к изменению поведения только при включенном согласии конечных пользователей.

При обнаружении запроса на рискованное согласие в запросе на согласие будет отображаться сообщение о необходимости утверждения администратором. Если [Рабочий процесс запроса согласия администратора](configure-admin-consent-workflow.md) включен, пользователь может отправить запрос администратору для дальнейшей проверки непосредственно из запроса согласия. Если он не включен, появится следующее сообщение:

* **AADSTS90094:** &lt;клиентаппдисплайнаме&gt; требуется разрешение на доступ к ресурсам в вашей организации, которые может предоставить только администратор. Попросите администратора предоставить разрешение этому приложению, прежде чем его использовать.

В этом случае событие аудита также будет регистрироваться с категорией "Аппликатионманажемент", типом действия "согласие на приложение" и состоянием "обнаружено рискованное приложение".

> [!IMPORTANT]
> Перед утверждением администраторы должны тщательно [оценить все запросы на согласие](manage-consent-requests.md#evaluating-a-request-for-tenant-wide-admin-consent) , особенно если корпорация Майкрософт выявляет риск.

### <a name="disable-or-re-enable-risk-based-step-up-consent-using-powershell"></a>Отключение и повторное включение согласия на основе риска с помощью PowerShell

Вы можете использовать модуль предварительной версии Azure AD PowerShell ([AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)), чтобы отключить действие до согласия администратора в случаях, когда корпорация Майкрософт обнаружит риск или повторно включите ее, если она была отключена ранее.

Это можно сделать, выполнив те же действия, что и для [настройки согласия владельца группы с помощью PowerShell](#configure-group-owner-consent-using-powershell), но подставив другое значение параметров. Существуют три различия в действиях. 

1. Изучите значения параметров для согласия на получение предупреждения по шагам.

    | Параметр       | Тип         | Описание  |
    | ------------- | ------------ | ------------ |
    | _блоккусерконсентфоррискяппс_   | логический |  Флаг, указывающий, будет ли согласие пользователя блокироваться при обнаружении рискованного запроса. |

2. Замените следующее значение на шаге 3.

    ```powershell
    $riskBasedConsentEnabledValue = $settings.Values | ? { $_.Name -eq "BlockUserConsentForRiskyApps" }
    ```
3. Замените один из следующих элементов на шаге 5.

    ```powershell
    # Disable risk-based step-up consent entirely
    $riskBasedConsentEnabledValue.Value = "False"
    ```

    ```powershell
    # Re-enable risk-based step-up consent, if disabled previously
    $riskBasedConsentEnabledValue.Value = "True"
    ```

## <a name="next-steps"></a>Дальнейшие действия

[Настройка рабочего процесса согласия администратора](configure-admin-consent-workflow.md)

[Узнайте, как управлять согласия приложений и оценивать запросы на согласие](manage-consent-requests.md)

[Предоставление согласия администратора на уровне клиента для приложения](grant-admin-consent.md)

[Разрешения и согласие на платформе Microsoft Identity](../develop/active-directory-v2-scopes.md)

[Azure AD на StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
