---
title: Миграция пакетов служб SSIS в управляемый экземпляр SQL
titleSuffix: Azure Database Migration Service
description: Узнайте, как перенести пакеты и проекты SQL Server Integration Services (SSIS) в управляемый экземпляр базы данных SQL Azure с помощью Azure Database Migration Service или Помощник по миграции данных.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 97a466ab033a42016c0d82465d1f98e2dcae8080
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80297184"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-database-managed-instance"></a>Перенос пакетов SQL Server Integration Services в Управляемый экземпляр Базы данных SQL Azure
Если вы используете SQL Server Integration Services (SSIS) и хотите перенести проекты или пакеты служб SSIS из исходной базы данных SSISDB, размещенной SQL Server, в целевую SSISDB, размещенную в управляемом экземпляре SQL Azure, можно использовать Azure Database Migration Service.

Если используемая версия служб SSIS более ранняя, чем 2012, или вы используете типы хранилищ пакетов, отличные от SSISDB, перед переносом проектов или пакетов служб SSIS их необходимо преобразовать с помощью мастера преобразования Integration Services проектов, который также можно запустить из SSMS. Дополнительные сведения см. в статье [Преобразование проектов в модель развертывания проекта](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure Database Migration Service (DMS) сейчас не поддерживает базу данных SQL Azure в качестве целевого назначения миграции. Сведения о повторном развертывании проектов и пакетов служб SSIS в базе данных SQL Azure см. в статье [Повторное развертывание пакетов SQL Server Integration Services в базе данных SQL Azure](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

Вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
>
> * Оценка проектов и пакетов исходной SSIS.
> * Перенос проектов и пакетов SSIS в Azure.

## <a name="prerequisites"></a>Предварительные условия

Для выполнения этих действий вам потребуется следующее:

* Чтобы создать виртуальная сеть Microsoft Azure для Azure Database Migration Service с помощью модели развертывания Azure Resource Manager, которая обеспечивает подключение типа "сеть — сеть" к локальным исходным серверам с помощью [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) или [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Дополнительные сведения см. в статье [сетевые топологии для миграции управляемого экземпляра базы данных SQL Azure с помощью Azure Database Migration Service]( https://aka.ms/dmsnetworkformi). Дополнительные сведения о создании виртуальной сети см. в [документации по виртуальной сети](https://docs.microsoft.com/azure/virtual-network/), особенно в кратком руководстве, где приведены пошаговые инструкции.
* Чтобы гарантировать, что правила группы безопасности сети виртуальной сети не блокируют следующие порты входящего трафика для Azure Database Migration Service: 443, 53, 9354, 445, 12000. Дополнительные сведения о фильтрации трафика NSG в виртуальной сети см. в статье [Фильтрация сетевого трафика с помощью групп безопасности сети](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Настройка [брандмауэра Windows для доступа к ядру базы данных источника](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017).
* Чтобы открыть брандмауэр Windows, чтобы разрешить Azure Database Migration Service доступ к исходному SQL Server, по умолчанию — TCP-порт 1433.
* Если вы запустили несколько именованных экземпляров SQL Server, использующих динамические порты, вы можете включить службу обозревателя SQL и разрешить доступ к UDP-порту 1434 через брандмауэры. Это позволит службе Azure Database Migration Service подключиться к именованному экземпляру на исходном сервере.
* Если перед исходными базами данных используется брандмауэр, его правила должны разрешать службе Azure Database Migration Service доступ к исходным базам данных для миграции и файлам SMB через порт 445.
* Управляемый экземпляр базы данных SQL Azure для размещения SSISDB. Если необходимо создать его, следуйте указаниям в статье [создание управляемый экземпляр базы данных SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* Чтобы гарантировать, что имена входа, используемые для подключения исходного SQL Server и целевого управляемого экземпляра, являются членами роли сервера sysadmin.
* Чтобы убедиться, что службы SSIS подготовлены в службе "Фабрика данных Azure" (ADF), которая содержит Azure-SSIS Integration Runtime (IR) с целевой базой данных SSISDB, размещенной в управляемом экземпляре SQL Azure, (как описано в статье [Создание среды выполнения интеграции Azure SSIS в фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)).

## <a name="assess-source-ssis-projectspackages"></a>Оценка проектов и пакетов исходной SSIS

Хотя оценка исходного SSISDB еще не интегрирована в базу данных Помощник по миграции (DMA), проекты и пакеты служб SSIS будут оцениваться и проверяться при повторном развертывании на целевом сервере SSISDB, размещенном в управляемом экземпляре базы данных SQL Azure.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Регистрация поставщика ресурсов Microsoft.DataMigration

1. Войдите на портал Azure, щелкните **Все службы** и выберите **Подписки**.

    ![Отображение подписок на портале](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Выберите подписку, в которой нужно создать экземпляр Azure Database Migration Service, а затем щелкните **Поставщики ресурсов**.

    ![Отображение поставщиков ресурсов](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. Найдите миграцию, а затем справа от **Microsoft. Migration**выберите **Register**.

    ![Регистрация поставщика ресурсов](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Создание экземпляра Azure Database Migration Service

1. В портал Azure выберите + **создать ресурс**, найдите **Azure Database Migration Service**, а затем выберите **Azure Database Migration Service** из раскрывающегося списка.

     ![Azure Marketplace](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. На экране **Azure Database Migration Service** выберите **Создать**.

    ![Создание экземпляра Azure Database Migration Service](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. На экране **Создание службы миграции** укажите имя службы, подписку и новую или существующую группу ресурсов.

4. Выберите расположение, в котором хотите создать экземпляр DMS.

5. Выберите существующую виртуальную сеть или создайте ее.

    Виртуальная сеть предоставляет Azure Database Migration Service с доступом к исходному SQL Server и целевому управляемому экземпляру базы данных SQL Azure.

    Дополнительные сведения о создании виртуальной сети в портал Azure см. в статье [Создание виртуальной сети с помощью портал Azure](https://aka.ms/DMSVnet).

    Подробные сведения см. в статье [Сетевые топологии для переноса Управляемого экземпляра Базы данных Azure SQL с помощью Azure Database Migration Service](https://aka.ms/dmsnetworkformi).

6. Выберите ценовую категорию.

    Дополнительные сведения о затратах и ценовых категориях см. на [странице с ценами](https://aka.ms/dms-pricing).

    ![Создание службы DMS](media/how-to-migrate-ssis-packages-mi/dms-create-service2.png)

7. Выберите **Создать**, чтобы создать службу.

## <a name="create-a-migration-project"></a>Создание проекта миграции

После создания экземпляра службы найдите его на портале Azure, откройте и создайте проект миграции.

1. На портале Azure щелкните **Все службы**, выполните поиск по запросу "Azure Database Migration Service" и выберите **Azure Database Migration Services** (Службы Azure Database Migration Service).

    ![Поиск всех экземпляров Azure Database Migration Service](media/how-to-migrate-ssis-packages-mi/dms-search.png)

2. На экране **Azure Database Migration Service** найдите имя созданного экземпляра, а затем выберите экземпляр.

3. Выберите **+ Новый проект миграции**.

4. На экране **Новый проект миграции** укажите имя проекта, в текстовом поле **Тип исходного сервера** выберите **SQL Server**, в текстовом поле **тип целевого сервера** выберите **управляемый экземпляр базы данных SQL Azure**, а затем для параметра **выберите тип действия**выберите **Миграция пакетов служб SSIS**.

   ![Создание проекта DMS](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Выберите **Создать**, чтобы создать проект.

## <a name="specify-source-details"></a>Указание сведений об источнике

1. На экране **Migration source detail** (Сведения об источнике миграции) задайте сведения о подключении для исходного SQL Server.

2. Если на сервере не установлен доверенный сертификат, установите флажок **Доверять сертификату сервера**.

    Если доверенный сертификат не установлен, SQL Server создаст самозаверяющий сертификат при запуске экземпляра. Этот сертификат используется с целью шифрования учетных данных для клиентских подключений.

    > [!CAUTION]
    > TLS-подключения, зашифрованные с помощью самозаверяющего сертификата, не обеспечивают надежную защиту. Они уязвимы для атак "злоумышленник в середине". Не следует полагаться на TLS, используя самозаверяющие сертификаты в рабочей среде или на серверах, подключенных к Интернету.

   ![Сведения об источнике](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. Нажмите кнопку **Сохранить**.

## <a name="specify-target-details"></a>Указание сведений о цели

1. На экране **сведения о целевом объекте миграции** укажите сведения о подключении для целевого объекта.

     ![Сведения о целевом объекте](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. Нажмите кнопку **Сохранить**.

## <a name="review-the-migration-summary"></a>Просмотр сводки по миграции

1. На экране **Сводка по миграции** в текстовом поле **Имя активности** задайте имя действия миграции.

2. Для **параметра перезапись проектов и сред служб SSIS**укажите, следует ли перезаписывать или игнорировать существующие проекты и среды служб SSIS.

    ![Сводка по проекту миграции](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Проверьте и подтвердите сведения, связанные с проектом миграции.

## <a name="run-the-migration"></a>Выполнение миграции

* Выберите **Запустить миграцию**.

## <a name="next-steps"></a>Дальнейшие шаги

* Просмотрите [руководство по миграции базы данных Майкрософт](https://datamigration.microsoft.com/).
