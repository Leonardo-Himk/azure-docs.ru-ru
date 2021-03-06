---
title: Масштабирование вычислительных ресурсов пула SQL Synapse (Azure PowerShell)
description: Вы можете масштабировать вычислительные ресурсы для пула SQL Synapse (хранилища данных) с помощью Azure PowerShell.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e3038617c6270acf9af295c910e9fd5c7dae2043
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80633779"
---
# <a name="quickstart-scale-compute-for-synapse-sql-pool-with-azure-powershell"></a>Краткое руководство. Масштабирование вычислительных ресурсов пула SQL Synapse (Azure PowerShell)

Вы можете масштабировать вычислительные ресурсы для пула SQL Synapse (хранилища данных) с помощью Azure PowerShell. [Горизонтально увеличивайте масштаб вычислительных ресурсов](sql-data-warehouse-manage-compute-overview.md), чтобы повысить производительность, или уменьшайте их масштаб, чтобы сократить затраты.

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

## <a name="before-you-begin"></a>Перед началом

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

В этом кратком руководстве предполагается, что у вас уже есть пул SQL, который можно масштабировать. Если его требуется создать, используйте инструкции из статьи [Краткое руководство. Создание Хранилища данных SQL Azure на портале Azure и выполнение запроса к нему](create-data-warehouse-portal.md), чтобы создать пул SQL **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Вход в Azure

С помощью команды [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) войдите в подписку Azure и следуйте инструкциям на экране.

```powershell
Connect-AzAccount
```

Чтобы узнать, какие подписки вы используете, выполните [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Get-AzSubscription
```

Если необходимо использовать подписку не по умолчанию, выполните [Set-AzContext](/powershell/module/az.accounts/set-azcontext?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Поиск сведений о хранилище данных

Найдите имя базы данных, имя сервера и группу ресурсов для хранилища данных, работу которого вы собираетесь приостановить и возобновить.

Выполните следующие действия, чтобы найти сведения о расположении хранилища данных.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. На портале Azure в области навигации слева выберите раздел **Azure Synapse Analytics (ранее — Хранилище данных SQL)** .
3. Выберите **mySampleDataWarehouse** на странице **Azure Synapse Analytics (ранее — Хранилище данных SQL)** , чтобы открыть хранилище данных.

    ![Имя сервера и группа ресурсов](./media/quickstart-scale-compute-powershell/locate-data-warehouse-information.png)

4. Запишите имя хранилища данных, которое будет использоваться в качестве имени базы данных. Помните, что хранилище данных — это один из типов базы данных. Также запишите имя сервера и группу ресурсов. Имя сервера и группы ресурсов будут использоваться в командах приостановки и возобновления работы.
5. Используйте только первую часть имени сервера в командлетах PowerShell. На предыдущем изображении полное имя сервера — sqlpoolservername.database.windows.net. Мы используем **sqlpoolservername** в качестве имени сервера в командлете PowerShell.

## <a name="scale-compute"></a>Масштабирование вычислительных ресурсов

В пуле SQL вы можете увеличивать и уменьшать объем вычислительных ресурсов, изменяя число единиц использования хранилища данных. В статье [Создание хранилища данных SQL Azure на портале Azure и отправка запросов к этому хранилищу данных](create-data-warehouse-portal.md) мы создали хранилище **mySampleDataWarehouse** и инициализировали его со значением 400 DWU. Ниже описаны шаги по изменению числа единиц DWU для **mySampleDataWarehouse**.

Чтобы изменить число единиц DWU, используйте командлет PowerShell [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). В следующем примере для параметра числа единиц задано значение DW300c для базы данных **mySampleDataWarehouse**, размещенной в группе ресурсов **resourcegroupname** на сервере **sqlpoolservername**.

```Powershell
Set-AzSqlDatabase -ResourceGroupName "resourcegroupname" -DatabaseName "mySampleDataWarehouse" -ServerName "sqlpoolservername" -RequestedServiceObjectiveName "DW300c"
```

## <a name="check-data-warehouse-state"></a>Проверка состояния хранилища данных

Чтобы просмотреть текущее состояние хранилища данных, используйте командлет PowerShell [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Этот командлет показывает состояние базы данных **mySampleDataWarehouse**, размещенной в группе ресурсов **resourcegroupname** на сервере **sqlpoolservername.database.windows.net**.

```powershell
$database = Get-AzSqlDatabase -ResourceGroupName resourcegroupname -ServerName sqlpoolservername -DatabaseName mySampleDataWarehouse
```

Результат будет примерно таким:

```powershell
ResourceGroupName             : resourcegroupname
ServerName                    : sqlpoolservername
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300c
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/resourcegroupname/providers/Microsoft.Sql/servers/sqlpoolservername/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

Вы увидите **состояние** базы данных в выходных данных. Как видите, в этом случае база данных находится в сети (Online).  При выполнении этой команды должно отобразиться одно из следующих значений состояния: "Online" (В сети), "Pausing" (Приостановка), "Resuming" (Возобновление), "Scaling" (Масштабирование) или "Paused" (Приостановлено).

Чтобы просмотреть само состояние службы, используйте следующую команду:

```powershell
$database | Select-Object DatabaseName,Status
```

## <a name="next-steps"></a>Дальнейшие действия

Вы научились масштабировать вычислительные ресурсы для пула SQL. Чтобы узнать больше о пуле SQL, перейдите к учебнику по загрузке данных.

> [!div class="nextstepaction"]
>[Загрузка данных в пул SQL](load-data-from-azure-blob-storage-using-polybase.md)
