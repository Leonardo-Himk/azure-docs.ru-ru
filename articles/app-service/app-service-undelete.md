---
title: Восстановить удаленные приложения
description: Узнайте, как восстановить удаленное приложение в службе приложений Azure. Избегайте случайного удаления случайно удаленного приложения.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: 296c8e2dfe99e3b0aea66f364ac6f6d9b2f60a1a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81272497"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Restore deleted App Service app Using PowerShell (Восстановление удаленного приложения Службы приложений с помощью PowerShell)

Если вы случайно удалили приложение в службе приложений Azure, его можно восстановить с помощью команд из [модуля AZ PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0).

> [!NOTE]
> Удаленные приложения очищаются из системы через 30 дней после первоначального удаления. После очистки приложения его нельзя восстановить.
>

## <a name="re-register-app-service-resource-provider"></a>Повторная регистрация поставщика ресурсов службы приложений
Некоторые клиенты могут столкнуться с проблемой, при которой не удается получить список удаленных приложений. Чтобы устранить эту проблему, выполните следующую команду:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Список удаленных приложений

Чтобы получить коллекцию удаленных приложений, можно использовать `Get-AzDeletedWebApp`.

Дополнительные сведения о конкретном удаленном приложении можно использовать:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

Подробные сведения включают:

- **Делетедситеид**: уникальный идентификатор приложения, используемый для сценариев, в которых несколько приложений с одинаковым именем были удалены
- **SubscriptionID**: подписка, содержащая удаленный ресурс
- **Расположение**: расположение исходного приложения
- **ResourceGroupName**: имя исходной группы ресурсов.
- **Имя**: имя исходного приложения.
- **Слот**— имя слота.
- **Время удаления**: когда приложение было удалено  

## <a name="restore-deleted-app"></a>Восстановить удаленное приложение

После определения приложения, которое вы хотите восстановить, его можно восстановить с помощью `Restore-AzDeletedWebApp`.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```
> [!NOTE]
> Слоты развертывания не восстанавливаются в составе приложения. Если необходимо восстановить промежуточный слот, используйте `-Slot <slot-name>` флаг.
>

Входные данные для команды:

- **Группа ресурсов**: Целевая группа ресурсов, в которую будет восстановлено приложение
- **Имя**: имя приложения должно быть глобально уникальным.
- **Таржетаппсервицепланнаме**: план службы приложений, связанный с приложением

По умолчанию `Restore-AzDeletedWebApp` восстановит как конфигурацию приложения, так и содержимое. Если вы хотите восстановить только содержимое, используйте `-RestoreContentOnly` флаг с этим командлет.

> [!NOTE]
> Если приложение было размещено в, а затем удалено из Среда службы приложений то его можно восстановить только в том случае, если соответствующий Среда службы приложений по-прежнему существует.
>

Полную ссылку на командлет можно найти здесь: [RESTORE-азделетедвебапп](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
