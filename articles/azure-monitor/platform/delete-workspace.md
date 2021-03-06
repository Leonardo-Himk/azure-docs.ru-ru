---
title: Удаление и восстановление рабочей области Log Analytics Azure | Документация Майкрософт
description: Узнайте, как удалить рабочую область Log Analytics, если она была создана в личной подписке, или как изменить структуру модели рабочей области.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/30/2020
ms.openlocfilehash: 7ed01a57a4c2a55d777907a6cc14b111fb2086e3
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2020
ms.locfileid: "82731906"
---
# <a name="delete-and-recover-azure-log-analytics-workspace"></a>Удаление и восстановление рабочей области Azure Log Analytics

В этой статье объясняется понятие обратимого удаления в рабочей области Azure Log Analytics и восстановление удаленной рабочей области. 

## <a name="considerations-when-deleting-a-workspace"></a>Рекомендации по удалению рабочей области

При удалении рабочей области Log Analytics выполняется операция обратимого удаления, позволяющая восстановить рабочую область, включая ее данные и подключенные агенты, в течение 14 дней, независимо от того, было ли удаление случайным или умышленно. После периода обратимого удаления ресурс рабочей области и его данные не могут быть восстановлены — данные помещаются в очередь для постоянного удаления и полностью очищаются в течение 30 дней. Имя рабочей области — "releaseed", и его можно использовать для создания новой рабочей области.

> [!NOTE]
> Если вы хотите переопределить поведение обратимого удаления и окончательно удалить рабочую область, выполните действия, описанные в статье [Удаление постоянной рабочей области](#permanent-workspace-delete).

При удалении рабочей области необходимо соблюдать осторожность, так как могут возникать важные данные и конфигурация, которые могут негативно повлиять на работу службы. Проверьте, какие агенты, решения и другие службы и источники Azure хранят свои данные в Log Analytics, например:

* Решения для управления
* Служба автоматизации Azure
* агенты, работающие на виртуальных машинах Windows и Linux;
* агенты, работающие на компьютерах Windows и Linux в вашей среде;
* System Center Operations Manager

Операция обратимого удаления удаляет ресурс рабочей области, и все связанные с ним разрешения пользователей нарушаются. Если пользователи связаны с другими рабочими областями, они могут продолжать использовать Log Analytics с другими рабочими областями.

## <a name="soft-delete-behavior"></a>Поведение обратимого удаления

Операция удаления рабочей области удаляет ресурс рабочей области диспетчер ресурсов, но его конфигурация и данные хранятся в течение 14 дней, при этом внешний вид рабочей области будет удален. Все агенты и группы управления System Center Operations Manager, настроенные для передачи в рабочую область, остаются в потерянном состоянии во время периода обратимого удаления. Служба дополнительно предоставляет механизм для восстановления удаленной рабочей области, включая ее данные и подключенные ресурсы, по сути, отменяя удаление.

> [!NOTE] 
> Установленные решения и связанные службы, такие как учетная запись службы автоматизации Azure, окончательно удаляются из рабочей области во время удаления и не могут быть восстановлены. Их необходимо перенастроить после операции восстановления, чтобы перевести рабочую область в ранее настроенное состояние.

Рабочую область можно удалить с помощью [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), [REST API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete)или в [портал Azure](https://portal.azure.com).

### <a name="azure-portal"></a>Портал Azure

1. Чтобы войти в систему, перейдите на [портал Azure](https://portal.azure.com). 
2. В портал Azure выберите **все службы**. В списке ресурсов введите **log Analytics**. Как только вы начнете вводить символы, список отфильтруется соответствующим образом. Выберите **Рабочие области Log Analytics**.
3. В списке рабочих областей Log Analytics выберите рабочую область и щелкните **Удалить** в верхней части средней области.
   ![Параметр "Удалить" в области свойств рабочей области](media/delete-workspace/log-analytics-delete-workspace.png)
4. Когда отобразится окно с запросом подтверждения удаления рабочей области, щелкните **Да**.
   ![Подтверждение удаления рабочей области](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

### <a name="troubleshooting"></a>Устранение неполадок

Для удаления рабочей области необходимо иметь по крайней мере *log Analytics разрешения участника* .<br>
Если появляется сообщение об ошибке *имя рабочей области уже используется* или *конфликтует* при создании рабочей области, это может быть так:
* Имя рабочей области недоступно и используется кем-либо в вашей организации или другим клиентом.
* Рабочая область была удалена за последние 14 дней, а ее имя сохранено для периода обратимого удаления. Чтобы переопределить обратимое удаление и окончательно удалить рабочую область, чтобы создать рабочую область с тем же именем, выполните следующие действия, чтобы сначала восстановить рабочую область, а затем выполнить постоянное удаление:<br>
   1. [Восстановите](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#recover-workspace) рабочую область.
   2. [Окончательное удаление](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#permanent-workspace-delete) рабочей области.
   3. Создайте рабочую область, используя то же имя рабочей области.

## <a name="permanent-workspace-delete"></a>Удаление постоянной рабочей области
Метод обратимого удаления может не помещаться в некоторых сценариях, таких как разработка и тестирование, где необходимо повторить развертывание с теми же параметрами и именем рабочей области. В таких случаях вы можете безвозвратно удалить рабочую область и "переопределить" период обратимого удаления. Операция удаления постоянной рабочей области освобождает имя рабочей области, и вы можете создать новую рабочую область с тем же именем.


> [!IMPORTANT]
> Используйте постоянную операцию удаления рабочей области с осторожностью, так как она необратима и вы не сможете восстановить рабочую область и ее данные.

В настоящее время можно выполнить удаление постоянной рабочей области с помощью REST API.

> [!NOTE]
> Любой запрос API должен содержать маркер авторизации носителя в заголовке запроса.
>
> Получить маркер можно с помощью:
> - [Регистрация приложений](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens)
> - Перейдите к портал Azure с помощью консоли разработчика (F12) в браузере. Просмотрите один из **пакетов** . экземпляры строки проверки подлинности в разделе **заголовки запроса**. Это будет в шаблоне *авторизации: Bearer <token> *. Скопируйте и добавьте его в вызов API, как показано в примерах.
> - Перейдите на сайт документации по Azure RESTFUL. Нажмите кнопку **попробовать** на любом API, скопируйте токен носителя и добавьте его в вызов API.
Чтобы удалить рабочую область без возможности восстановления, используйте функцию "удалить вызов API-интерфейса" из [рабочих областей]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) , используя принудительный тег:
>
> ```rst
> DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2015-11-01-preview&force=true
> Authorization: Bearer eyJ0eXAiOiJKV1Qi….
> ```
Где "eyJ0eXAiOiJKV1Qi..." представляет маркер полной авторизации.

## <a name="recover-workspace"></a>Восстановить рабочую область

Если у вас есть разрешения участника для подписки и группы ресурсов, в которых Рабочая область была связана до операции обратимого удаления, ее можно восстановить в течение периода обратимого удаления, включая его данные, конфигурацию и подключенные агенты. После периода обратимого удаления Рабочая область не может быть восстановлена и назначена для постоянного удаления. Имена удаленных рабочих областей сохраняются во время периода обратимого удаления и не могут использоваться при попытке создать новую рабочую область.  

Вы можете восстановить рабочую область, создав рабочую область с подробными сведениями об удаленной рабочей области, включая *идентификатор подписки*, *имя группы ресурсов*, *имя рабочей области* и *регион*. Если группа ресурсов также была удалена и не существует, создайте группу ресурсов с тем же именем, которое использовалось перед удалением, а затем создайте рабочую область с помощью любого из следующих методов: [портал Azure](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace), [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/New-AzOperationalInsightsWorkspace) или [REST API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/createorupdate).

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Select-AzSubscription "subscription-name-the-workspace-was-in"
PS C:\>New-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name-the-workspace-was-in" -Name "deleted-workspace-name" -Location "region-name-the-workspace-was-in"
```

После завершения операции восстановления Рабочая область и все ее данные возвращаются обратно. Решения и связанные службы были окончательно удалены из рабочей области, когда они были удалены, и их следует перенастроить, чтобы перевести рабочую область в ранее настроенное состояние. Некоторые данные могут быть недоступны для запроса после восстановления рабочей области до тех пор, пока не будут повторно установлены связанные решения и их схемы не будут добавлены в рабочую область.

> [!NOTE]
> * Восстановление рабочей области не поддерживается в [портал Azure](https://portal.azure.com). 
> * Повторное создание рабочей области в течение периода обратимого удаления дает указание на то, что это имя рабочей области уже используется. 
> 
