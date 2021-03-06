---
title: Аудит операций Azure Cosmos DB плоскости управления
description: Узнайте, как выполнять аудит операций плоскости управления, таких как добавление региона, пропускная способность обновления, переход на другой ресурс, Добавление виртуальной сети и т. д. в Azure Cosmos DB
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: sngun
ms.openlocfilehash: a5df7866f7897109dbd7a0ea8a52b857ab671875
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735357"
---
# <a name="how-to-audit-azure-cosmos-db-control-plane-operations"></a>Аудит операций Azure Cosmos DB плоскости управления

Плоскость управления в Azure Cosmos DB — это служба RESTFUL, которая позволяет выполнять различные операции с учетной записью Azure Cosmos. Она предоставляет доступ к модели открытого ресурса (например, базе данных, учетной записи) и различным операциям конечным пользователям для выполнения действий с моделью ресурсов. Операции плоскости управления включают изменения в учетную запись или контейнер Azure Cosmos. Например, такие операции, как создание учетной записи Azure Cosmos, добавление региона, пропускная способность обновления, регион отработки отказа, Добавление виртуальной сети и т. д. — это некоторые операции плоскости управления. В этой статье объясняется, как выполнять аудит операций плоскости управления в Azure Cosmos DB. Вы можете выполнять операции плоскости управления в учетных записях Cosmos Azure с помощью Azure CLI, PowerShell или портал Azure, в то время как контейнеры используют Azure CLI или PowerShell.

Ниже приведено несколько примеров сценариев, в которых удобно использовать операции с плоскостью управления аудитом.

* Вы хотите получать оповещение при изменении правил брандмауэра для учетной записи Azure Cosmos. Оповещение необходимо для поиска несанкционированных изменений в правилах, регулирующих сетевую безопасность учетной записи Azure Cosmos, и выполнять быстрые действия.

* Вы хотите получать оповещение, если новый регион добавляется в учетную запись Azure Cosmos или удаляется из нее. Добавление или удаление регионов влияет на требования к выставлению счетов и независимостиу данных. Это предупреждение поможет обнаружить случайное добавление или удаление региона в учетной записи.

* Вы хотите получить дополнительные сведения из журналов диагностики на основе изменений. Например, была изменена виртуальная сеть.

## <a name="disable-key-based-metadata-write-access"></a>Отключить доступ на запись метаданных на основе ключей

Прежде чем выполнять аудит операций плоскости управления в Azure Cosmos DB, отключите доступ на запись метаданных на основе ключа в вашей учетной записи. Когда доступ на запись метаданных на основе ключей отключен, клиенты, подключающиеся к учетной записи Cosmos Azure через ключи учетной записи, не смогут получить доступ к этой учетной записи. Доступ на запись можно отключить, задав `disableKeyBasedMetadataWriteAccess` свойству значение true. После установки этого свойства изменения любого ресурса могут происходить от пользователя с правильной ролью управления доступом на основе ролей (RBAC) и учетными данными. Дополнительные сведения о том, как задать это свойство, см. в статье [предотвращение изменений из пакетов SDK](role-based-access-control.md#preventing-changes-from-cosmos-sdk) . 

Если после `disableKeyBasedMetadataWriteAccess` включения приложение на базе пакета SDK выполняет операции создания или обновления, ошибка *"операция POST" для ресурса "контаинернамеордатабасенаме" не будет разрешена через конечную точку Azure Cosmos DB* . Необходимо включить доступ к таким операциям для вашей учетной записи или выполнить операции создания или обновления с помощью Azure Resource Manager, Azure CLI или Azure PowerShell. Чтобы переключиться обратно, присвойте параметру Дисаблекэйбаседметадатавритеакцесс **значение false** , используя Azure CLI, как описано в статье о [предотвращении изменений из пакета SDK для Cosmos](role-based-access-control.md#preventing-changes-from-cosmos-sdk) . Не забудьте изменить значение `disableKeyBasedMetadataWriteAccess` на false, а не на true.

При отключении доступа на запись метаданных необходимо учитывать следующие моменты.

* Оцените и убедитесь, что ваши приложения не выполняют вызовы метаданных, которые изменяют указанные выше ресурсы (например, создание коллекции, пропускную способность обновления,...) с помощью пакета SDK или ключей учетной записи.

* В настоящее время портал Azure использует ключи учетной записи для операций с метаданными, поэтому эти операции будут заблокированы. Кроме того, для выполнения таких операций можно использовать развертывания шаблонов Azure CLI, пакетов SDK или диспетчер ресурсов.

## <a name="enable-diagnostic-logs-for-control-plane-operations"></a>Включение журналов диагностики для операций управления плоскостью

Вы можете включить журналы диагностики для операций управления плоскостью с помощью портал Azure. После включения журналы диагностики будут записывать операции в виде пары событий начала и завершения с соответствующими подробностями. Например, *регионфаиловерстарт* и *регионфаиловеркомплете* выполнит событие региона отработки отказа.

Чтобы включить ведение журнала для операций плоскости управления, выполните следующие действия.

1. Войдите в [портал Azure](https://portal.azure.com) и перейдите к своей учетной записи Azure Cosmos.

1. Откройте область **параметры диагностики** и укажите **имя** создаваемых журналов.

1. Выберите **контролпланерекуестс** для типа журнала и выберите параметр **отправить в log Analytics** .

Кроме того, можно хранить журналы в учетной записи хранения или потоке в концентраторе событий. В этой статье показано, как отправлять журналы в log Analytics, а затем запрашивать их. После включения для того, чтобы журналы диагностики вступили в силу, потребуется несколько минут. Все операции плоскости управления, выполненные после этой точки, могут быть отслеживанием. На следующем снимке экрана показано, как включить журналы плоскости управления.

![Включить ведение журнала запросов плоскости управления](./media/audit-control-plane-logs/enable-control-plane-requests-logs.png)

## <a name="view-the-control-plane-operations"></a>Просмотр операций плоскости управления

После включения ведения журнала выполните следующие действия для мониторинга операций для конкретной учетной записи.

1. Войдите в [портал Azure](https://portal.azure.com).

1. Откройте вкладку **монитор** на панели навигации слева и выберите панель **журналы** . Откроется пользовательский интерфейс, в котором можно легко выполнять запросы с этой конкретной учетной записью в области. Чтобы просмотреть журналы плоскости управления, выполните следующий запрос:

   ```kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="ControlPlaneRequests"
   | where TimeGenerated >= ago(1h)
   ```

На следующих снимках экрана записываются журналы при изменении уровня согласованности для учетной записи Azure Cosmos.

![Журналы управляющей плоскости при добавлении виртуальной сети](./media/audit-control-plane-logs/add-ip-filter-logs.png)

На следующих снимках экрана записываются журналы при обновлении пропускной способности таблицы Cassandra:

![Журналы плоскости управления при обновлении пропускной способности](./media/audit-control-plane-logs/throughput-update-logs.png)

## <a name="identify-the-identity-associated-to-a-specific-operation"></a>Идентификация удостоверения, связанного с определенной операцией

Если вы хотите выполнить отладку дальше, можно найти конкретную операцию в **журнале действий** с помощью идентификатора действия или метки времени операции. Отметка времени используется для некоторых диспетчер ресурсов клиентов, где идентификатор действия не передается явным образом. Журнал действий содержит подробные сведения об удостоверении, с помощью которого была инициирована операция. На следующем снимке экрана показано, как использовать идентификатор действия и найти связанные с ним операции в журнале действий:

![Использование идентификатора действия и поиск операций](./media/audit-control-plane-logs/find-operations-with-activity-id.png)

## <a name="control-plane-operations-for-azure-cosmos-account"></a>Операции плоскости управления для учетной записи Azure Cosmos

Ниже перечислены операции плоскости управления, доступные на уровне учетной записи. Большинство операций ведется на уровне учетной записи. Эти операции доступны в виде метрик в Azure Monitor.

* Регион добавлен
* Регион удален
* Учетная запись удалена
* Отработка отказа региона
* Созданная учетная запись
* Виртуальная сеть удалена
* Параметры сети учетной записи обновлены
* Параметры репликации учетной записи обновлены
* Ключи учетной записи обновлены
* Параметры резервного копирования учетной записи обновлены
* Параметры диагностики учетной записи обновлены

## <a name="control-plane-operations-for-database-or-containers"></a>Операции плоскости управления для базы данных или контейнеров

Ниже перечислены операции плоскости управления, доступные на уровне базы данных и контейнера. Эти операции доступны в виде метрик в Azure Monitor.

* База данных SQL обновлена
* Контейнер SQL обновлен
* Пропускная способность базы данных SQL обновлена
* Пропускная способность контейнера SQL обновлена
* База данных SQL удалена
* Контейнер SQL удален
* Cassandra пространства ключей обновлен
* Таблица Cassandra обновлена
* Пропускная способность Cassandra пространства ключей обновлена
* Пропускная способность таблицы Cassandra обновлена
* Cassandra пространства ключей удален
* Таблица Cassandra удалена
* База данных Gremlin обновлена
* Граф Gremlin обновлен
* Пропускная способность базы данных Gremlin обновлена
* Пропускная способность графа Gremlin обновлена
* База данных Gremlin удалена
* Граф Gremlin удален
* База данных Mongo обновлена
* Коллекция Mongo обновлена
* Пропускная способность базы данных Mongo обновлена
* Пропускная способность коллекции Mongo обновлена
* База данных Mongo удалена
* Коллекция Mongo удалена
* Таблица AzureTable обновлена
* Пропускная способность таблицы AzureTable обновлена
* Таблица AzureTable удалена

## <a name="diagnostic-log-operations"></a>Операции с журналом диагностики

Ниже приведены имена операций в журналах диагностики для различных операций.

* Регионаддстарт, Регионаддкомплете
* Регионремовестарт, Регионремовекомплете
* Аккаунтделетестарт, Аккаунтделетекомплете
* Регионфаиловерстарт, Регионфаиловеркомплете
* Аккаунткреатестарт, Аккаунткреатекомплете
* Аккаунтупдатестарт, Аккаунтупдатекомплете
* Виртуалнетворкделетестарт, Виртуалнетворкделетекомплете
* Диагностиклогупдатестарт, Диагностиклогупдатекомплете

Для операций, зависящих от API, операция имеет имя в следующем формате:

* Типа API + Апикиндресаурцетипе + OperationType + начало и завершение
* Типа API + Апикиндресаурцетипе + "пропускная способность" + operationType + запуск/завершение

**Пример** 

* Кассандракэйспацесупдатестарт, Кассандракэйспацесупдатекомплете
* Кассандракэйспацессраугхпутупдатестарт, Кассандракэйспацессраугхпутупдатекомплете
* Склконтаинерсупдатестарт, Склконтаинерсупдатекомплете

Свойство *ресаурцедетаилс* содержит весь текст ресурса как полезные данные запроса и содержит все свойства, запрошенные для обновления

## <a name="diagnostic-log-queries-for-control-plane-operations"></a>Запросы журналов диагностики для операций плоскости управления

Ниже приведены некоторые примеры получения журналов диагностики для операций управления плоскостью.

```kusto
AzureDiagnostics 
| where Category =="ControlPlaneRequests"
| where  OperationName startswith "SqlContainersUpdateStart"
```

```kusto
AzureDiagnostics 
| where Category =="ControlPlaneRequests"
| where  OperationName startswith "SqlContainersThroughputUpdateStart"
```

## <a name="next-steps"></a>Дальнейшие действия

* [Просмотр Azure Monitor для Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json)
* [Monitor and debug with metrics in Azure Cosmos DB](use-metrics.md) (Мониторинг и отладка с помощью метрик в Azure Cosmos DB)
