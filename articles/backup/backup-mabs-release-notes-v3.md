---
title: Заметки о выпуске Microsoft Azure Backup Server версии 3
description: В этой статье содержатся сведения об известных проблемах и способах их решения для Microsoft Azure Backup Server (MABS) v3.
ms.topic: conceptual
ms.date: 11/22/2018
ms.asset: 0c4127f2-d936-48ef-b430-a9198e425d81
ms.openlocfilehash: a5c99bcb95fde39bddc9e9db9ab000881c89081a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82185631"
---
# <a name="release-notes-for-microsoft-azure-backup-server"></a>Заметки о выпуске Microsoft Azure Backup Server

В этой статье представлены известные проблемы и их решения для Microsoft Azure Backup Server (MABS) версии 3.

## <a name="backup-and-recovery-fails-for-clustered-workloads"></a>Сбои резервного копирования и восстановления для кластеризованных рабочих нагрузок

**Описание.** Происходит сбой резервного копирования и восстановления для кластеризованных источников данных, например, таких как кластер Hyper-V, SQL (SQL Always On) или Exchange в группе доступности базы данных (DAG), после обновления с MABS версии 2 до MABS версии 3.

**Обходной путь.** Чтобы избежать этого, откройте SQL Server Management Studio (SSMS) и выполните следующий скрипт SQL для базы данных DPM:

```sql
    IF EXISTS (SELECT * FROM dbo.sysobjects
        WHERE id = OBJECT_ID(N'[dbo].[tbl_PRM_DatasourceLastActiveServerMap]')
        AND OBJECTPROPERTY(id, N'IsUserTable') = 1)
        DROP TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap]
        GO

        CREATE TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] (
            [DatasourceId]          [GUID]          NOT NULL,
            [ActiveNode]            [nvarchar](256) NULL,
            [IsGCed]                [bit]           NOT NULL
            ) ON [PRIMARY]
        GO

        ALTER TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] ADD
    CONSTRAINT [pk__tbl_PRM_DatasourceLastActiveServerMap__DatasourceId] PRIMARY KEY NONCLUSTERED
        (
            [DatasourceId]
        )  ON [PRIMARY],

    CONSTRAINT [DF_tbl_PRM_DatasourceLastActiveServerMap_IsGCed] DEFAULT
        (
            0
        ) FOR [IsGCed]
    GO
```

## <a name="upgrade-to-mabs-v3-fails-in-russian-locale"></a>Сбои при обновлении до MABS версии 3 в русском языковом стандарте

**Описание.** Обновление с MABS версии 2 до MABS версии 3 в русском языковом стандарте завершается сбоем с кодом ошибки **4387**.

**Обходной путь.** Выполните следующие действия для обновления до MABS версии 3 с помощью русского пакета установки:

1. [Создайте резервную копию](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server?view=sql-server-2017#SSMSProcedure) базы данных SQL и удалите MABS версии 2 (выберите сохранение защищенных данных во время удаления).
2. Обновитесь до SQL 2017 (Enterprise) и удалите отчеты в рамках обновления.
3. [Установите](https://docs.microsoft.com/sql/reporting-services/install-windows/install-reporting-services?view=sql-server-2017#install-your-report-server) SQL Server Reporting Services (SSRS).
4. [Установите](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) SQL Server Management Studio (SSMS).
5. Настройте отчетность с использованием параметров, как описано в разделе о [настройке SSRS с помощью SQL 2017](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#upgrade-mabs).
6. [Установите](backup-azure-microsoft-azure-backup.md) MABS версии 3.
7. [Восстановите](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms?view=sql-server-2017) SQL с помощью SSMS и запустите средство DPM-Sync, как описано [здесь](https://docs.microsoft.com/system-center/dpm/back-up-the-dpm-server?view=sc-dpm-2019#using-dpmsync).
8. Обновите свойство DataBaseVersion в таблице dbo.tbl_DLS_GlobalSetting, используя следующую команду:

    ```sql
            UPDATE dbo.tbl_DLS_GlobalSetting
            set PropertyValue = '13.0.415.0'
            where PropertyName = 'DatabaseVersion'
    ```

9. Запустите службу MSDPM.

## <a name="next-steps"></a>Дальнейшие действия

[Новые возможности в MABS v3](backup-mabs-whats-new-mabs.md)
