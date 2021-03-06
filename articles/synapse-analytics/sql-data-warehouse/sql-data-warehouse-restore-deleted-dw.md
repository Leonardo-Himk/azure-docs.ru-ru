---
title: Восстановление удаленного пула SQL
description: Руководство по восстановлению удаленного пула SQL.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: d2e2fdb181b553d330368b043b75159e211dd0d2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80745126"
---
# <a name="restore-a-deleted-sql-pool-using-azure-synapse-analytics"></a>Восстановление удаленного пула SQL с помощью Azure синапсе Analytics

Из этой статьи вы узнаете, как восстановить SQL с помощью портал Azure или PowerShell.

## <a name="before-you-begin"></a>Подготовка к работе

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**Проверьте ресурсы DTU.** Каждый пул SQL размещается на сервере SQL Server (например, myserver.database.windows.net), который имеет квоту DTU по умолчанию.  Убедитесь, что SQL Server имеет достаточное количество оставшихся квот DTU для восстанавливаемой базы данных. Чтобы узнать, как вычислить необходимое количество DTU или запросить дополнительные единицы DTU, ознакомьтесь с разделом [Создание запроса в службу поддержки для хранилища данных SQL](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-a-deleted-data-warehouse-through-powershell"></a>Восстановление удаленного хранилища данных с помощью PowerShell

Чтобы восстановить удаленный пул SQL, используйте командлет [RESTORE-азсклдатабасе](/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) . Если соответствующий логический сервер также был удален, вы не сможете восстановить это хранилище данных.

1. Перед началом убедитесь, что [установлен Azure PowerShell](/powershell/azure/overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
2. Откройте PowerShell.
3. Подключитесь к своей учетной записи Azure и выведите список всех подписок, связанных с ней.
4. Выберите подписку, содержащую удаленное хранилище данных для восстановления.
5. Получение конкретного удаленного хранилища данных.
6. Восстановление удаленного хранилища данных
    1. Чтобы восстановить удаленное хранилище данных SQL на другой логический сервер, убедитесь, что указано другое имя логического сервера.  Этот логический сервер также может находиться в другой группе ресурсов и регионе.
    1. Чтобы выполнить восстановление в другую подписку, используйте кнопку [переместить](../../azure-resource-manager/management/move-resource-group-and-subscription.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#use-the-portal) , чтобы переместить логический сервер в другую подписку.
7. Убедитесь, что восстановленное хранилище данных подключено.
8. После завершения восстановления можно настроить восстановленное хранилище данных, следуя [настройке базы данных после восстановления](../../sql-database/sql-database-disaster-recovery.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#configure-your-database-after-recovery).

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different logical server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Use the following command to restore deleted data warehouse to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>Восстановление удаленной базы данных на портале Azure

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Перейдите к серверу SQL Server, на котором размещено удаленное хранилище данных.
3. Выберите значок **Удаленные базы данных** в содержании.

    ![Удаленные базы данных](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-01.png)

4. Выберите удаленное хранилище данных SQL, которое необходимо восстановить.

    ![Выбор удаленных баз данных](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-11.png)

5. Укажите новое **имя базы данных** и нажмите кнопку **ОК** .

    ![Указание нового имени базы данных](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-21.png)

## <a name="next-steps"></a>Дальнейшие шаги

- [Восстановление существующего пула SQL](sql-data-warehouse-restore-active-paused-dw.md)
- [Восстановление из пула SQL с географическим резервным копированием](sql-data-warehouse-restore-from-geo-backup.md)
