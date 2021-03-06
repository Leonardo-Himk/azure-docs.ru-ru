---
title: Включить многофакторную проверку подлинности для каждого пользователя Azure Active Directory
description: Узнайте, как включить многофакторную идентификацию Azure для каждого пользователя, изменив пользовательское состояние
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/13/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e8ceaf13324864c7ec3df731c3e710815b0eba9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81309779"
---
# <a name="enable-per-user-azure-multi-factor-authentication-to-secure-sign-in-events"></a>Включение многофакторной идентификации Azure для отдельных пользователей для защиты событий входа

Существует два способа защитить события входа пользователя, запрашивая многофакторную проверку подлинности в Azure AD. Первым, и предпочтительным вариантом является настройка политики условного доступа, которая требует многофакторной проверки подлинности при определенных условиях. Второй способ — включить многофакторную идентификацию Azure для каждого пользователя. Когда пользователи включаются отдельно, они выполняют многофакторную проверку подлинности при каждом входе (с некоторыми исключениями, например при входе с доверенных IP-адресов или при включении функции " _сохраненные устройства_ ").

> [!NOTE]
> Рекомендуемым подходом является включение многофакторной идентификации Azure с использованием политик условного доступа. Изменение состояния пользователя больше не рекомендуется, если лицензии не включают условный доступ, так как они требуют, чтобы пользователи выполняли MFA каждый раз при входе.
>
> Чтобы приступить к работе с условным доступом, см. раздел [учебник. Защита событий входа пользователя с помощью многофакторной идентификации Azure](tutorial-enable-azure-mfa.md).

## <a name="azure-multi-factor-authentication-user-states"></a>Пользовательские состояния многофакторной идентификации Azure

Учетные записи пользователей в службе Многофакторной идентификации Azure имеют три различных состояния:

> [!IMPORTANT]
> Включение многофакторной идентификации Azure с помощью политики условного доступа не меняет состояние пользователя. Не выдавать сигнал, если пользователи отображаются как отключенные. Условный доступ не изменяет состояние.
>
> **Если вы используете политики условного доступа, вы не должны включать и не применять пользователей.**

| Состояние | Описание | Затронутые приложения, не использующие браузер | Затронутые приложения, использующие браузер | Затронутая современная аутентификация |
|:---:| --- |:---:|:--:|:--:|
| Отключен | Состояние по умолчанию для нового пользователя, не зарегистрированного в службе многофакторной идентификации Azure. | Нет | Нет | Нет |
| Активировано | Пользователь зарегистрирован в службе многофакторной идентификации Azure, но не зарегистрирован. Ему будет предложено зарегистрироваться при следующем входе в систему. | Нет.  Они будут продолжать работать, пока не завершится регистрация. | Да. После истечения срока действия сеанса требуется регистрация многофакторной идентификации Azure.| Да. После истечения срока действия маркера доступа требуется регистрация многофакторной идентификации Azure. |
| Принудительно | Пользователь зарегистрировался и завершил процесс регистрации для многофакторной идентификации Azure. | Да. Для приложений нужны пароли приложений. | Да. При входе в систему требуется многофакторная идентификация Azure. | Да. При входе в систему требуется многофакторная идентификация Azure. |

Состояние пользователя показывает, зарегистрирован ли администратор в службе многофакторной идентификации Azure и завершил ли процесс регистрации.

Все пользователи начинают с состояния *Отключено*. При регистрации пользователей в службе многофакторной идентификации Azure их состояние изменяется на *включено*. После включения пользователи входят в систему и завершают регистрацию, затем их состояние меняется на *Enforced* (Принудительно).

> [!NOTE]
> Если MFA повторно включается для объекта пользователя, который уже содержит сведения о регистрации, такие как телефон или электронная почта, администраторы должны повторно зарегистрировать MFA с помощью портал Azure или PowerShell. Если пользователь не регистрируется повторно, состояние MFA не переходит из состояния *включено* *в пользовательский* интерфейс управления mfa.

## <a name="view-the-status-for-a-user"></a>Просмотр состояния пользователя

Выполните следующие действия, чтобы открыть страницу портал Azure, где можно просматривать состояния пользователей и управлять ими.

1. Войдите в [портал Azure](https://portal.azure.com) с правами администратора.
1. Найдите и выберите *Azure Active Directory*, а затем выберите **Пользователи** > **все пользователи**.
1. Выберите **многофакторную проверку подлинности**. Чтобы увидеть этот пункт меню, может потребоваться прокрутить экран вправо. Щелкните приведенный ниже снимок экрана, чтобы увидеть полный портал Azure окно и расположение меню:[![](media/howto-mfa-userstates/selectmfa-cropped.png "Выбор многофакторной проверки подлинности в окне "Пользователи" в Azure AD")](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Откроется новая страница, на которой отображается пользовательское состояние, как показано в следующем примере.
   ![Снимок экрана, показывающий пример сведений о пользовательской информации для многофакторной идентификации Azure](./media/howto-mfa-userstates/userstate1.png)

## <a name="change-the-status-for-a-user"></a>Изменение состояния пользователя

Чтобы изменить состояние многофакторной идентификации Azure для пользователя, выполните следующие действия.

1. Выполните шаги выше, чтобы перейти на страницу **пользователей** MFA.
1. Найдите пользователя, которого вы хотите включить для многофакторной идентификации Azure. Может потребоваться изменить представление в верхней части на " **Пользователи**".
   ![Выберите пользователя, состояние которого нужно изменить, на вкладке "Пользователи".](./media/howto-mfa-userstates/enable1.png)
1. Установите флажки рядом с именами пользователей, для которых нужно изменить состояние.
1. На правой стороне в разделе **быстрые шаги**выберите **включить** или **Отключить**. В следующем примере пользователь *John Smith* имеет флажок рядом с именем и включается для использования: включить выбранного пользователя, щелкнув включить ![в меню быстрых действий.](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Включенные* пользователи автоматически переключаются на *принудительное применение* при регистрации для многофакторной идентификации Azure. Не следует вручную изменять пользовательское состояние на *принудительное*.

1. Подтвердите свой выбор во всплывающем окне, которое откроется.

После включения уведомите пользователей по электронной почте. Сообщите пользователям о том, что отображается запрос, чтобы запросить их регистрацию при следующем входе. Кроме того, если ваша организация использует небраузерные приложения, не поддерживающие современную аутентификацию, потребуется создать пароли приложений. Дополнительные сведения см. в разделе [Руководство пользователя по многофакторной идентификации Azure](../user-help/multi-factor-authentication-end-user.md) , которое поможет им приступить к работе.

## <a name="change-state-using-powershell"></a>Изменение состояния с помощью PowerShell

Чтобы изменить пользовательское состояние с помощью [Azure AD PowerShell](/powershell/azure/overview), измените `$st.State` параметр для учетной записи пользователя. Существует три возможных состояния учетной записи пользователя:

* *Включен*
* *Принудительно*
* *Отключен*  

Не переводите пользователей непосредственно в состояние *Применено*. В этом случае приложения, не относящиеся к браузеру, перестают работать, так как пользователь не прошел регистрацию многофакторной идентификации Azure и не получил [пароль приложения](howto-mfa-mfasettings.md#app-passwords).

Чтобы приступить к работе, установите модуль *MSOnline* с помощью [Install-Module](/powershell/module/powershellget/install-module) следующим образом:

```PowerShell
Install-Module MSOnline
```

Затем подключитесь с помощью [Connect-MsolService](/powershell/module/msonline/connect-msolservice):

```PowerShell
Connect-MsolService
```

В следующем примере скрипта PowerShell включает MFA для отдельного пользователя с *bsimon@contoso.com*именем:

```PowerShell
$st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
$st.RelyingParty = "*"
$st.State = "Enabled"
$sta = @($st)

# Change the following UserPrincipalName to the user you wish to change state
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
```

PowerShell — удобный инструмент для массового включения многофакторной проверки подлинности для пользователей. Следующий сценарий выполняет цикл по списку пользователей и включает MFA для своих учетных записей. Определите учетные записи пользователей, заданные в первой строке `$users` , следующим образом:

   ```PowerShell
   # Define your list of users to update state in bulk
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"

   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

Чтобы отключить MFA, в следующем примере пользователь получает команду [Get-MsolUser](/powershell/module/msonline/get-msoluser), а затем удаляет все *стронгаусентикатионрекуирементс* , установленные для определенного пользователя с помощью [Set-MsolUser](/powershell/module/msonline/set-msoluser):

```PowerShell
Get-MsolUser -UserPrincipalName bsimon@contoso.com | Set-MsolUser -StrongAuthenticationRequirements @()
```

Можно также напрямую отключить MFA для пользователя с помощью [Set-MsolUser](/powershell/module/msonline/set-msoluser) следующим образом:

```PowerShell
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements @()
```

## <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Преобразование пользователей из многопользовательского MFA в условный доступ на основе MFA

Приведенная ниже оболочка PowerShell поможет вам выполнить преобразование в службу многофакторной идентификации Azure на основе условного доступа.

```PowerShell
# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

# Disable MFA for all users
Get-MsolUser -All | Set-MfaState -State Disabled
```

> [!NOTE]
> Мы недавно изменили поведение и этот сценарий PowerShell. Ранее скрипт сохранялся из методов MFA, отключил MFA и восстановил методы. Это больше не требуется, так как поведение по умолчанию для Disable не приводит к очистке методов.
>
> Если MFA повторно включается для объекта пользователя, который уже содержит сведения о регистрации, такие как телефон или электронная почта, администраторы должны повторно зарегистрировать MFA с помощью портал Azure или PowerShell. Если пользователь не регистрируется повторно, состояние MFA не переходит из состояния *включено* *в пользовательский* интерфейс управления mfa.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы настроить параметры многофакторной идентификации Azure, такие как надежные IP-адреса, пользовательские голосовые сообщения и предупреждения о мошенничестве, см. статью [Настройка параметров многофакторной идентификации Azure](howto-mfa-mfasettings.md). Сведения об управлении параметрами пользователей для многофакторной идентификации Azure см. в статье [Управление параметрами пользователей с помощью многофакторной идентификации Azure](howto-mfa-userdevicesettings.md).

Сведения о том, почему пользователю было предложено или не предлагается выполнить MFA, см. в разделе [отчеты многофакторной идентификации Azure](howto-mfa-reporting.md#azure-ad-sign-ins-report).
