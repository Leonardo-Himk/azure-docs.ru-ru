---
title: Виртуальные рабочие столы Windows PowerShell — Azure
description: Устранение неполадок с PowerShell при настройке среды виртуальных рабочих столов Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: ce19c670df5062a11bf86e9c383a322f9033818d
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612016"
---
# <a name="windows-virtual-desktop-powershell"></a>Виртуальный рабочий стол Windows — PowerShell

>[!IMPORTANT]
>Это содержимое относится к обновлению пружины 2020 с Azure Resource Manager объектов виртуальных рабочих столов Windows. Если вы используете виртуальный рабочий стол Windows в выпуске 2019 без Azure Resource Manager объектов, см. [эту статью](./virtual-desktop-fall-2019/troubleshoot-powershell-2019.md).
>
> Обновление 2020 виртуального рабочего стола Windows в настоящее время находится в общедоступной предварительной версии. Эта предварительная версия предоставляется без соглашения об уровне обслуживания, и мы не рекомендуем использовать ее для рабочих нагрузок. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. 
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Используйте эту статью для устранения ошибок и проблем при использовании PowerShell с виртуальным рабочим столом Windows. Дополнительные сведения о службы удаленных рабочих столов PowerShell см. в статье [Windows Virtual Desktop PowerShell](/powershell/module/windowsvirtualdesktop/).

## <a name="provide-feedback"></a>Предоставление отзыва

Посетите [техническое сообщество Виртуального рабочего стола Windows](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop), чтобы обсудить службу "Виртуальный рабочий стол Windows" с группой разработчиков и активными членами сообщества.

## <a name="powershell-commands-used-during-windows-virtual-desktop-setup"></a>Команды PowerShell, используемые при установке виртуальных рабочих столов Windows

В этом разделе перечислены команды PowerShell, которые обычно используются при настройке виртуальных рабочих столов Windows, а также способы решения проблем, которые могут возникнуть при их использовании.

### <a name="error-new-azroleassignment-the-provided-information-does-not-map-to-an-ad-object-id"></a>Ошибка: New-Азролеассигнмент: предоставленные сведения не сопоставляются с ИДЕНТИФИКАТОРом объекта AD

```powershell
AzRoleAssignment -SignInName "admins@contoso.com" -RoleDefinitionName "Desktop Virtualization User" -ResourceName "0301HP-DAG" -ResourceGroupName 0301RG -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups' 
```

**Причина:** Пользователь, указанный параметром *-SignInName* , не может быть найден в Azure Active Directory, привязанном к среде виртуальных рабочих столов Windows. 

**Исправление:** Необходимо выполнить следующие действия.

- Пользователь должен быть синхронизирован с Azure Active Directory.
- Пользователь не должен быть привязан к коммерческой коммерции "бизнес-потребитель" (B2C) или "бизнес-бизнес" (B2B).
- Среда виртуальных рабочих столов Windows должна быть привязана к правильному Azure Active Directory.

### <a name="error-new-azroleassignment-the-client-with-object-id-does-not-have-authorization-to-perform-action-over-scope-code-authorizationfailed"></a>Ошибка: New-Азролеассигнмент: "клиент с идентификатором объекта не имеет авторизации для выполнения действия над областью (код: AuthorizationFailed)"

**Причина 1:** Используемая учетная запись не имеет разрешений владельца на подписку. 

**Исправление 1:** Пользователю с разрешениями владельца необходимо выполнить назначение роли. Кроме того, пользователю необходимо назначить роль администратора доступа пользователей, чтобы назначить пользователя группе приложений.

**Причина 2:** Используемая учетная запись имеет разрешения владельца, но не является частью Azure Active Directory среды или не имеет разрешений на запрос Azure Active Directory, где находится пользователь.

**Исправление 2:** Пользователю с разрешениями Active Directory необходимо выполнить назначение ролей.

### <a name="error-new-azwvdhostpool----the-location-is-not-available-for-resource-type"></a>Ошибка: New-Азввдхостпул--расположение недоступно для типа ресурса

```powershell
New-AzWvdHostPool_CreateExpanded: The provided location 'southeastasia' is not available for resource type 'Microsoft.DesktopVirtualization/hostpools'. List of available regions for the resource type is 'eastus,eastus2,westus,westus2,northcentralus,southcentralus,westcentralus,centralus'. 
```

Причина: виртуальные рабочие столы Windows поддерживают выбор расположения пулов узлов, групп приложений и рабочих областей для хранения метаданных службы в определенных расположениях. Параметры доступны только в том месте, где доступна эта функция. Эта ошибка означает, что эта функция недоступна в выбранном расположении.

Исправление. в сообщении об ошибке будет опубликован список поддерживаемых регионов. Вместо этого используйте один из поддерживаемых регионов.

### <a name="error-new-azwvdapplicationgroup-must-be-in-same-location-as-host-pool"></a>Ошибка: New-Азввдаппликатионграуп должен находиться в том же расположении, что и пул узлов

```powershell
New-AzWvdApplicationGroup_CreateExpanded: ActivityId: e5fe6c1d-5f2c-4db9-817d-e423b8b7d168 Error: ApplicationGroup must be in same location as associated HostPool
```

**Причина:** Обнаружено несоответствие расположения. Все пулы узлов, группы приложений и рабочие области имеют расположение для хранения метаданных службы. Все создаваемые объекты, связанные друг с другом, должны находиться в одном расположении. Например, если пул узлов находится в `eastus`, необходимо также создать группы приложений в. `eastus` Если вы создаете рабочую область для регистрации этих групп приложений в, эта Рабочая область также должна `eastus` находиться в этой рабочей области.

**Исправление:** Извлеките расположение пула узлов в, а затем назначьте группу приложений, которая создается в том же расположении.

## <a name="next-steps"></a>Дальнейшие действия

- Общие сведения об устранении неполадок с виртуальным рабочим столом Windows и сведениями о эскалации см. в разделе [Обзор устранения неполадок, обратная связь и поддержка](troubleshoot-set-up-overview.md).
- Сведения об устранении неполадок при настройке среды виртуальных рабочих столов Windows и пулов узлов см. в статье [Создание пула узлов и сред](troubleshoot-set-up-issues.md).
- Сведения об устранении неполадок при настройке виртуальной машины в виртуальном рабочем столе Windows см. в разделе [Конфигурация виртуальной машины узла сеанса](troubleshoot-vm-configuration.md).
- Сведения об устранении неполадок подключения клиентов к виртуальным рабочим столам Windows см. в статье [подключения к службе виртуальных рабочих столов Windows](troubleshoot-service-connection.md).
- Сведения об устранении неполадок, связанных с удаленный рабочий стол клиентами, см. [в разделе Устранение неполадок клиента удаленный рабочий стол](troubleshoot-client.md)
- Дополнительные сведения о службе см. в разделе [Среда виртуальных рабочих столов Windows](environment-setup.md).
- Сведения о действиях аудита см. в статье [Операции аудита с помощью Resource Manager](../azure-resource-manager/management/view-activity-logs.md).
- Дополнительные сведения об определении ошибок во время развертывания см. в статье [Просмотр операций развертывания с помощью портала Azure](../azure-resource-manager/templates/deployment-history.md).
