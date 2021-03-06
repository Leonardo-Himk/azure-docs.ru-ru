---
title: 'Azure Active Directory: политики самостоятельного сброса пароля'
description: Дополнительные сведения о различных параметрах политики Azure Active Directory самостоятельного сброса пароля
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8b6d08dd2073de80ac0f7fd08f510d9cda80545
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82143239"
---
# <a name="self-service-password-reset-policies-and-restrictions-in-azure-active-directory"></a>Политики самостоятельного сброса пароля и ограничения в Azure Active Directory

В этой статье описываются политики паролей и требования к сложности, связанные с учетными записями пользователей в клиенте Azure Active Directory (Azure AD).

## <a name="administrator-reset-policy-differences"></a>Различия политики сброса администратора

**Корпорация Майкрософт применяет строгую политику сброса паролей с *двумя шлюзами* по умолчанию для любой роли администратора Azure**. Эта политика может отличаться от той, которая была определена для пользователей, и эта политика не может быть изменена. Всегда следует тестировать функции сброса пароля от имени пользователя, которому не назначена какая-либо роль администратора Azure.

При использовании двухфакторной политики у **администраторов нет возможности применять контрольные вопросы**.

Двухфакторная политика требует ввода двух фрагментов данных для аутентификации, таких как *адрес электронной почты*, *приложение Authenticator* или *номер телефона*. Двухфакторная политика применяется в следующих случаях:

* затронуты все следующие роли администратора:
  * администратор службы технической поддержки;
  * администратор службы поддержки;
  * Администратор выставления счетов
  * Служба поддержка партнеров уровня 1
  * Служба поддержка партнеров уровня 2
  * Администратор Exchange.
  * администратор Skype для бизнеса;
  * Администратор пользователей.
  * создатели каталогов;
  * глобальный или корпоративный администратор;
  * Администратор SharePoint.
  * администратор соответствия требованиям.
  * администратор приложений;
  * Администратор безопасности.
  * администратор привилегированных ролей;
  * Администратор Intune
  * администратор прокси-сервера приложений;
  * Администратор Dynamics 365
  * администратор службы Power BI.
  * Администратор проверки подлинности
  * Администратор привилегированной проверки подлинности

* если прошло 30 дней с момента установки пробной подписки; или
* Для вашего клиента Azure AD настроен пользовательский домен, например *contoso.com*; ни
* Azure AD Connect выполняет синхронизацию удостоверений из вашего локального каталога.

### <a name="exceptions"></a>Исключения

Однофакторная политика требует ввода одного фрагмента данных для аутентификации, такого как адрес электронной почты или номер телефона. Однофакторная политика применяется в следующих случаях:

* еще не прошло 30 дней с момента установки пробной подписки; или
* Для вашего клиента Azure AD не настроен пользовательский домен, поэтому используется значение по умолчанию **. onmicrosoft.com*. Домен по умолчанию **. onmicrosoft.com* не рекомендуется для использования в рабочей среде. перетаскивани
* Azure AD Connect не выполняет синхронизацию удостоверений.

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Политики UserPrincipalName, которые применяются ко всем учетным записям пользователей

Все учетные записи пользователей, используемые для входа в Azure AD, должны иметь уникальное значение атрибута имени субъекта-пользователя (UPN), связанное с этой учетной записью. В следующей таблице перечислены политики, применяемые к локальным учетным записям пользователей домен Active Directory Services, которые синхронизируются с облаком и с учетными записями пользователей в облаке:

| Свойство | Требования UserPrincipalName |
| --- | --- |
| Допустимые символы |<ul> <li>A–Z</li> <li>a–z</li><li>0–9</li> <li> ' \. - \_ ! \# ^ \~</li></ul> |
| Недопустимые символы |<ul> <li>Любой знак \@\", который не отделяет имя пользователя от домена.</li> <li>Не может содержать знак точки "." непосредственно перед знаком \@\".</li></ul> |
| Ограничения длины |<ul> <li>Общая длина не должна превышать 113 знаков.</li><li>Перед знаком \@\" может быть до 64 знаков.</li><li>После знака \@\" может быть до 48 знаков.</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Политики паролей, которые применяются только к облачным учетным записям пользователей.

В следующей таблице описаны параметры политики паролей, применяемые к учетным записям пользователей, которые создаются и управляются в Azure AD.

| Свойство | Requirements (Требования) |
| --- | --- |
| Допустимые символы |<ul><li>A–Z</li><li>a–z</li><li>0–9</li> <li>@ # $ % ^ & * - _ ! + = [] {} &#124; \: ",. ? / \`~ " ( ) ;</li> <li>пустое пространство</li></ul> |
| Недопустимые символы | Знаки Юникода. |
| Ограничения для пароля |<ul><li>Не менее 8 символов и не более 256 символов.</li><li>Необходимо выполнить 3 из 4 следующих условий:<ul><li>строчные буквы;</li><li>прописные буквы;</li><li>числа (0–9);</li><li>символы (см. ограничения для пароля выше).</li></ul></li></ul> |
| Длительность срока действия пароля (максимальный возраст пароля) |<ul><li>Значение по умолчанию: **90** дней.</li><li>Значение можно изменить с помощью командлета `Set-MsolPasswordPolicy` из модуля Azure Active Directory для Windows PowerShell.</li></ul> |
| Уведомление об истечении срока действия пароля (когда пользователи получают уведомление об истечении срока действия пароля) |<ul><li>Значение по умолчанию: **14** дней (до истечения срока действия пароля).</li><li>Это значение можно настроить с помощью командлета `Set-MsolPasswordPolicy`.</li></ul> |
| Срок действия пароля истекает (разрешить пароли никогда не истекает) |<ul><li>Значение по умолчанию: **false** (указывает, что у пароля есть Дата окончания срока действия).</li><li>Это значение можно настроить для каждой учетной записи пользователя с помощью командлета `Set-MsolUser`.</li></ul> |
| Журнал изменения пароля | Последний пароль *невозможно* использовать повторно, когда пользователь изменяет пароль. |
| Журнал сброса пароля | Последний пароль *можно* использовать повторно, когда пользователь сбрасывает забытый пароль. |
| Блокировка учетной записи | После 10 неудачных попыток входа (с неправильным паролем) пользователь будет заблокирован на одну минуту. Последующие неудачные попытки входа приведут к блокировке пользователя на более длительное время. При [интеллектуальной блокировке](howto-password-smart-lockout.md) отслеживаются последние три неправильные хэши паролей, чтобы избежать увеличения счетчика блокировки для одного и того же пароля. Если пользователь введет один и тот же неправильный пароль несколько раз, это не приведет к блокировке учетной записи. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Установка политик срока действия пароля в Azure AD

*Глобальный администратор* или *администратор пользователей* для облачной службы Майкрософт может использовать *модуль Microsoft Azure AD для Windows PowerShell* , чтобы задать для паролей пользователей не срок действия. Можно также использовать командлеты Windows PowerShell, чтобы удалить бессрочную конфигурацию или просмотреть, какие пароли пользователей имеют неограниченный срок действия.

Эти указания применимы к другим поставщикам, таким как Intune и Office 365, которые используют Azure AD для служб идентификации и каталогов. Срок действия пароля — это единственное, что может быть изменено в политике.

> [!NOTE]
> Неограниченный срок действия можно настроить только для паролей учетных записей пользователей, которые не синхронизируются в процессе синхронизации каталогов. Дополнительные сведения о синхронизации каталогов см. в статье [Интеграция локальных каталогов с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>Задание или проверка политик пароля с помощью PowerShell

Чтобы приступить к работе, [скачайте и установите модуль Azure AD PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0) и [подключите его к клиенту Azure AD](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0#examples). После установки модуля выполните следующие действия, чтобы настроить каждое поле.

### <a name="check-the-expiration-policy-for-a-password"></a>Проверка политики срока действия пароля

1. Подключитесь к Windows PowerShell с помощью учетных данных администратора или администратора организации.
1. Выполните одну из следующих команд:

   * Чтобы узнать, не просрочен ли пароль одного пользователя, выполните следующий командлет, используя имя участника-пользователя (например, *april\@contoso.ONMICROSOFT.com*) или идентификатор пользователя, которого необходимо проверить:

   ```powershell
   Get-AzureADUser -ObjectId <user ID> | Select-Object @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

   * Чтобы просмотреть параметр **Пароль не имеет окончания срока действия** для всех пользователей, выполните следующий командлет: .

   ```powershell
   Get-AzureADUser -All $true | Select-Object UserPrincipalName, @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

### <a name="set-a-password-to-expire"></a>Задание срока действия пароля

1. Подключитесь к Windows PowerShell с помощью учетных данных администратора или администратора организации.
1. Выполните следующие команды:

   * Чтобы установить пароль отдельного пользователя со сроком действия, выполните следующий командлет, используя имя участника-пользователя или идентификатор пользователя: .

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies None
   ```

   * Чтобы задать срок действия паролей всех пользователей в организации, используйте следующий командлет: 

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies None
   ```

### <a name="set-a-password-to-never-expire"></a>Установка бессрочного пароля

1. Подключитесь к Windows PowerShell с помощью учетных данных администратора или администратора организации.
1. Выполните следующие команды:

   * Чтобы установить бессрочный пароль отдельного пользователя, выполните следующий командлет, используя имя участника-пользователя или идентификатор пользователя: .

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies DisablePasswordExpiration
   ```

   * Чтобы установить бессрочный пароль для всех пользователей в организации, запустите следующий командлет:

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies DisablePasswordExpiration
   ```

   > [!WARNING]
   > Пароль, для которого задано `-PasswordPolicies DisablePasswordExpiration`, по-прежнему имеет срок использования, определяемый атрибутом `pwdLastSet`. В зависимости от атрибута `pwdLastSet`, если задать срок действия, указав `-PasswordPolicies None`, то всем пользователям, у паролей которых значение `pwdLastSet` превышает 90 дней, будет необходимо сменить пароль при следующем входе. Это изменение может повлиять на большое количество пользователей.

## <a name="next-steps"></a>Дальнейшие шаги

Чтобы приступить к работе с SSPR, см. раздел [учебник. предоставление пользователям возможности разблокировать учетную запись или сбросить пароли с помощью Azure Active Directory самостоятельного сброса пароля](tutorial-enable-sspr.md).

Если у вас или у пользователей возникли проблемы с SSPR, см. раздел [Устранение неполадок самостоятельного сброса пароля](active-directory-passwords-troubleshoot.md)
