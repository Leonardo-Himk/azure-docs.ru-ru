---
title: Тип назначения "персональный Рабочий стол для виртуальных рабочих столов Windows" — Azure
description: Настройка типа назначения для пула узлов личного рабочего стола Windows.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2541e9e10103d66c6c2fb6978c3029d61b813eab
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614971"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Настройка типа назначения пула узлов личного рабочего стола

>[!IMPORTANT]
>Это содержимое относится к выпуску 2019, который не поддерживает Azure Resource Manager объекты виртуальных рабочих столов Windows. Если вы пытаетесь управлять Azure Resource Manager объектов виртуальных рабочих столов Windows, представленных в обновлении пружины 2020, см. [эту статью](../configure-host-pool-personal-desktop-assignment-type.md).

Вы можете настроить тип назначения для пула узлов личного рабочего стола, чтобы настроить среду виртуальных рабочих столов Windows в соответствии с вашими потребностями. В этом разделе мы покажем, как настроить автоматическое или прямое назначение для пользователей.

>[!NOTE]
> Инструкции, приведенные в этой статье, применимы только к пулам узлов персональных рабочих столов, а не к пулам узлов, так как пользователи в пулах пулов узлов не назначены конкретным узлам сеансов.

## <a name="configure-automatic-assignment"></a>Настройка автоматического назначения

Автоматическое назначение — это тип назначения по умолчанию для новых пулов узлов личного рабочего стола, созданных в среде виртуальных рабочих столов Windows. Автоматическое назначение пользователей не требует определенного узла сеансов.

Чтобы автоматически назначить пользователей, сначала назначьте им пул узлов личного рабочего стола, чтобы они могли видеть Рабочий стол в своем канале. Когда назначенный пользователь запускает рабочий стол в своем веб-канале, он запросит доступный узел сеансов, если они еще не подключены к пулу узлов, что завершает процесс назначения.

Прежде чем начать, [скачайте и импортируйте модуль PowerShell для виртуальных рабочих столов Windows](/powershell/windows-virtual-desktop/overview/) , если вы еще этого не сделали. 

> [!NOTE]
> Перед выполнением инструкций убедитесь, что вы установили модуль PowerShell для виртуальных рабочих столов Windows версии 1.0.1534.2001 или более поздней.

После этого выполните следующий командлет, чтобы войти в учетную запись:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Чтобы настроить пул узлов для автоматического назначения пользователей виртуальным машинам, выполните следующий командлет PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

Чтобы назначить пользователя пулу узлов личного рабочего стола, выполните следующий командлет PowerShell:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>Настройка прямого назначения

В отличие от автоматического назначения, при использовании прямого назначения необходимо назначить пользователю пул узлов личного рабочего стола и конкретный узел сеанса, прежде чем они смогут подключиться к своему личному рабочему столу. Если пользователь назначен только пулу узлов без назначения узла сеансов, он не сможет получить доступ к ресурсам.

Чтобы настроить пул узлов так, чтобы требовать непосредственного назначения пользователей для узлов сеансов, выполните следующий командлет PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

Чтобы назначить пользователя пулу узлов личного рабочего стола, выполните следующий командлет PowerShell:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Чтобы назначить пользователя определенному узлу сеансов, выполните следующий командлет PowerShell:

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы настроили тип назначения личный рабочий стол, вы можете войти в клиент виртуальных рабочих столов Windows, чтобы протестировать его в рамках сеанса пользователя. Эти две следующие инструкции помогут вам подключиться к сеансу с помощью выбранного клиента:

- [Подключение к клиенту Windows Desktop](../connect-windows-7-and-10.md)
- [Подключение к веб-клиенту](connect-web-2019.md)
