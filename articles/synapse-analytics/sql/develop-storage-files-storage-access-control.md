---
title: Управление доступом к учетной записи хранения в SQL по запросу (предварительная версия)
description: В этой статье приведены сведения о том, как служба SQL по запросу (предварительная версия) обращается к службе хранилища Azure и как можно управлять доступом к хранилищу для SQL по запросу в Azure Synapse Analytics.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 2d5d508afe81975cbeda448b497a098e8a3bbcf3
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589284"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview"></a>Управление доступом к учетной записи хранения в SQL по запросу (предварительная версия)

SQL по запросу позволяет создавать запросы для чтения файлов непосредственно из службы хранилища Azure. Разрешения на доступ к файлам в службе хранилища Azure контролируются на двух уровнях.
- **Уровень хранилища** — пользователь должен иметь разрешение на доступ к базовым файлам хранилища. Администратор хранилища должен разрешить субъекту Azure AD чтение и запись файлов или создать ключ SAS, который будет использоваться для доступа к хранилищу.
- **Уровень службы SQL** — пользователь должен иметь разрешение `SELECT` на чтение данных из [внешней таблицы](develop-tables-external-tables.md) или разрешение `ADMINISTER BULK ADMIN` на выполнение `OPENROWSET` и разрешение на использование учетных данных, которые предназначены для доступа к хранилищу.

В этой статье описываются типы учетных данных, которые вы можете использовать, и способ поиска учетных данных для пользователей SQL и Azure AD.

## <a name="supported-storage-authorization-types"></a>Поддерживаемые типы авторизации в службе хранилища

Пользователь, выполнивший вход в ресурс SQL по запросу, должен иметь права на доступ и отправку запросов к файлам в службе хранилища Azure, если эти файлы не являются общедоступными. Поддерживаются три типа авторизации.

- [Подписанный URL-адрес](?tabs=shared-access-signature)
- [Удостоверение пользователя](?tabs=user-identity)
- [Управляемое удостоверение](?tabs=managed-identity)

> [!NOTE]
> По умолчанию при создании рабочей области действует [сквозная аутентификация Azure AD](#force-azure-ad-pass-through). Это избавляет от необходимости создавать учетные данные для каждой учетной записи хранения, к которой осуществляется доступ по имени для входа Azure AD. Это поведение [можно отключить](#disable-forcing-azure-ad-pass-through).

### <a name="shared-access-signature"></a>[Подписанный URL-адрес](#tab/shared-access-signature)

**Подписанный URL-адрес (SAS)** предоставляет делегированный доступ к ресурсам в учетной записи хранения. С помощью SAS можно предоставить клиентам доступ к ресурсам в учетной записи хранения, не передавая им ключи учетной записи. SAS обеспечивает детальный контроль над типом доступа, который вы предоставляете клиентам с конкретным SAS, включая период действия, предоставленные разрешения, допустимый диапазон IP-адресов и допустимый протокол (HTTPS или HTTP).

Маркер SAS можно получить на портале Azure, последовательно открыв пункты **Учетная запись хранения —> Подписанный URL-адрес —> Настроить разрешения —> Создать SAS и строку подключения**.

> [!IMPORTANT]
> Созданный маркер SAS содержит вопросительный знак ("?") в начале строки. Чтобы применить этот маркер для SQL по запросу, удалите символ вопросительного знака ("?") при создании учетных данных. Пример:
>
> Маркер SAS: ?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D

Чтобы разрешить доступ с помощью маркера SAS, необходимо создать учетные данные уровня базы данных или уровня сервера.

### <a name="user-identity"></a>[Удостоверение пользователя](#tab/user-identity)

**Удостоверение пользователя** используется для типа авторизации "сквозная аутентификация", при которой удостоверение пользователя Azure AD, выполнившего вход в SQL по запросу, применяется для авторизации доступа к данным. Перед обращением к данным администратор службы хранилища Azure должен предоставить разрешения соответствующему пользователю Azure AD. Как указано в таблице выше, этот тип не поддерживается для типа пользователя SQL.

> [!IMPORTANT]
> Чтобы использовать удостоверение для доступа к данным, необходимо иметь роль владельца, участника или читателя данных для BLOB-объекта в хранилище.
> Даже если вы являетесь владельцем учетной записи хранения, вам придется добавить себя в одну из ролей для данных BLOB-объекта хранилища.
>
> Дополнительные сведения см. в статье [Access control in Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-access-control.md) (Контроль доступа в Azure Data Lake Storage 2-го поколения).
>

Чтобы пользователи Azure AD могли получить доступ к хранилищу с собственными удостоверениями, необходимо явным образом включить сквозную проверку подлинности Azure AD.

#### <a name="force-azure-ad-pass-through"></a>Принудительное применение сквозной аутентификации Azure AD

Сквозная аутентификация Azure AD применяется принудительно по умолчанию, если указано специальное значение `UserIdentity` для параметра CREDENTIAL NAME, которое создается автоматически во время подготовки рабочей области Azure Synapse. В этом режиме использование сквозной аутентификации Azure AD является обязательным для каждого запроса от любого имени входа Azure AD, независимо от наличия других учетных данных.

> [!NOTE]
> Сквозная аутентификация Azure AD является поведением по умолчанию. Вам не нужно создавать учетные данные для каждой учетной записи хранения, к которой осуществляется доступ по именам входа AD.

Если вы [отключили принудительное применение сквозной аутентификации Azure AD для каждого запроса](#disable-forcing-azure-ad-pass-through) и намерены снова включить ее, выполните следующую команду:

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

Чтобы включить принудительное применение сквозной аутентификации Azure AD для конкретного пользователя, предоставьте этому пользователю разрешение REFERENCE для учетных данных `UserIdentity`. Следующий пример включает принудительное применение сквозной аутентификации Azure AD для пользователя user_name:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

#### <a name="disable-forcing-azure-ad-pass-through"></a>Отключение принудительного применения сквозной аутентификации Azure AD

Вы можете отключить [принудительное применение сквозной аутентификации Azure AD для каждого запроса](#force-azure-ad-pass-through). Для этого удалите учетные данные `Userdentity` с помощью этой команды:

```sql
DROP CREDENTIAL [UserIdentity];
```

Если вы решите снова включить этот режим, воспользуйтесь инструкцией [Принудительное применение сквозной аутентификации Azure AD](#force-azure-ad-pass-through).

### <a name="managed-identity"></a>[Управляемое удостоверение](#tab/managed-identity)

**Управляемое удостоверение** иногда обозначается как MSI. Это функция Azure Active Directory (Azure AD), которая предоставляет службы Azure для SQL по запросу. Также она развертывает автоматически управляемое удостоверение в Azure AD. Это удостоверение можно использовать для авторизации запроса на доступ к данным в службе хранилища Azure.

Перед обращением к данным администратор службы хранилища Azure должен предоставить управляемому удостоверению разрешения на доступ к данным. Предоставление разрешений управляемому удостоверению выполняется точно так же, как любому другому пользователю Azure AD.

### <a name="anonymous-access"></a>[Анонимный доступ](#tab/public-access)

Вы можете использовать файлы, размещенные в общедоступных расположениях в учетных записях хранения Azure, которые [допускают анонимный доступ](/azure/storage/blobs/storage-manage-access-to-resources.md).

---

### <a name="supported-authorization-types-for-databases-users"></a>Поддерживаемые типы авторизации для пользователей баз данных

В таблице ниже перечислены доступные типы авторизации.

| Тип авторизации                    | *Пользователь SQL*    | *Пользователь Azure AD*     |
| ------------------------------------- | ------------- | -----------    |
| [Удостоверение пользователя](?tabs=user-identity#supported-storage-authorization-types)       | Не поддерживается | Поддерживается      |
| [SAS](?tabs=shared-access-signature#supported-storage-authorization-types)       | Поддерживается     | Поддерживается      |
| [Управляемое удостоверение](?tabs=managed-identity#supported-storage-authorization-types) | Не поддерживается | Поддерживается      |

### <a name="supported-storages-and-authorization-types"></a>Поддерживаемые типы авторизации и хранилища

Вы можете использовать следующие сочетания типов авторизации и хранилища Azure.

|                     | Хранилище BLOB-объектов   | ADLS 1-го поколения        | ADLS 2-го поколения     |
| ------------------- | ------------   | --------------   | -----------   |
| *SAS*               | Поддерживается      | Не поддерживается   | Поддерживается     |
| *Управляемое удостоверение* | Поддерживается      | Поддерживается        | Поддерживается     |
| *Удостоверение пользователя*    | Поддерживается      | Поддерживается        | Поддерживается     |

## <a name="credentials"></a>Учетные данные

Чтобы выполнить запрос по файлу, который размещен в службе хранилища Azure, конечная точка SQL по запросу должна иметь учетные данные с информацией об аутентификации. Используются два типа учетных данных.
- Объект CREDENTIAL уровня сервера используется для нерегламентированных запросов, выполняемых с помощью функции `OPENROWSET`. Имя этого объекта должно соответствовать URL-адресу хранилища.
- DATABASE SCOPED CREDENTIAL используется для внешних таблиц. Внешняя таблица ссылается на `DATA SOURCE` с учетными данными, которые должны использоваться для доступа к хранилищу.

Чтобы разрешить пользователю создавать или удалять учетные данные, администратор может предоставить пользователю разрешение GRANT/DENY ALTER ANY CREDENTIAL.

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

Пользователи базы данных, которые обращаются к внешнему хранилищу, должны иметь разрешение на использование учетных данных.

### <a name="grant-permissions-to-use-credential"></a>Предоставление разрешений на использование учетных данных

Чтобы использовать учетные данные, пользователь должен иметь разрешение `REFERENCES` для этих учетных данных. Чтобы предоставить разрешение `REFERENCES` для учетных данных хранилища (storage_credential) конкретному пользователю (specific_user), выполните такую команду:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Чтобы обеспечить беспроблемную сквозную аутентификацию Azure AD, все пользователи по умолчанию получают право использовать учетные данные `UserIdentity`. Для этого при подготовке рабочей области Azure Synapse автоматически выполняется следующая инструкция:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

## <a name="server-scoped-credential"></a>Учетные данные уровня сервера

Учетные данные уровня сервера используются при вызове функции `OPENROWSET` из процесса входа в SQL без `DATA_SOURCE` для чтения файлов в определенной учетной записи хранения. Имя учетных данных уровня сервера **должно** совпадать с URL-адресом службы хранилища Azure. Для добавления учетных данных выполните инструкцию [CREATE CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Необходимо указать аргумент CREDENTIAL NAME. Он должен соответствовать пути к данным в хранилище или некоторой части этого пути (см. ниже).

> [!NOTE]
> Аргумент FOR CRYPTOGRAPHIC PROVIDER не поддерживается.

Имя объекта CREDENTIAL уровня сервера должно содержать полный путь к учетной записи хранения и к контейнеру (необязательно) в следующем формате: `<prefix>://<storage_account_path>/<storage_path>` Описание путей к учетной записи хранения приведено в следующей таблице.

| Внешний источник данных       | Prefix | Путь к учетной записи хранения                                |
| -------------------------- | ------ | --------------------------------------------------- |
| хранилище BLOB-объектов Azure         | HTTPS  | <учетная_запись_хранилища>.blob.core.windows.net             |
| Хранилище Azure Data Lake Storage 1-го поколения | HTTPS  | <учетная_запись_хранилища>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage 2-го поколения | HTTPS  | <учетная_запись_хранения>.dfs.core.windows.net              |

> [!NOTE]
> Существует специальное значение `UserIdentity` для CREDENTIAL уровня сервера, которое [принудительно применяет сквозную аутентификацию Azure AD](?tabs=user-identity#force-azure-ad-pass-through).

Учетные данные уровня сервера позволяют получить доступ к службе хранилища Azure через следующие типы проверки подлинности:

### <a name="shared-access-signature"></a>[Подписанный URL-адрес](#tab/shared-access-signature)

Следующий скрипт создает учетные данные уровня сервера, которые могут использоваться функцией `OPENROWSET` для доступа к любому файлу в службе хранилища Azure через токен SAS. Создайте такие учетные данные, чтобы разрешить субъекту SQL, который выполняет функцию `OPENROWSET`, считывать файлы, защищенные ключом SAS в службе хранилища Azure, которая соответствует URL-адресу в имени учетных данных.

Замените <*mystorageaccountname*> реальным именем учетной записи хранения, а <*mystorageaccountcontainername*> — именем контейнера:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

### <a name="user-identity"></a>[Удостоверение пользователя](#tab/user-identity)

Следующий скрипт создает учетные данные уровня сервера, которые позволяют олицетворять пользователя с помощью удостоверения Azure AD.

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

### <a name="managed-identity"></a>[Управляемое удостоверение](#tab/managed-identity)

Следующий скрипт создает учетные данные уровня сервера, которые могут использоваться функцией `OPENROWSET` для доступа к любому файлу в службе хранилища Azure через управляемое удостоверение рабочей области.

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='Managed Identity'
```

### <a name="public-access"></a>[Открытый доступ](#tab/public-access)

Следующий скрипт создает учетные данные уровня сервера, которые могут использоваться функцией `OPENROWSET` для доступа к любому файлу в общедоступном хранилище Azure. Создайте такие учетные данные, чтобы разрешить субъекту SQL, выполняющему функцию `OPENROWSET`, считывать общедоступные файлы в службе хранилища Azure, которая соответствует URL-адресу в имени учетных данных.

Замените здесь заполнитель <*mystorageaccountname*> реальным именем учетной записи хранения, а <*mystorageaccountcontainername*> — именем контейнера:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = '';
GO
```
---

## <a name="database-scoped-credential"></a>Учетные данные уровня базы данных

Учетные данные уровня базы данных используются, когда какой-либо субъект вызывает функцию `OPENROWSET` с `DATA_SOURCE` или выбирает данные из [внешней таблицы](develop-tables-external-tables.md) без использования общедоступных файлов. Учетные данные уровня базы данных не должны совпадать с именем учетной записи хранения, так как они будут явно указаны в объекте DATA SOURCE, который определяет расположение хранилища.

Такие учетные данные позволяют получить доступ к службе хранилища Azure через следующие типы проверки подлинности:

### <a name="shared-access-signature"></a>[Подписанный URL-адрес](#tab/shared-access-signature)

Следующий скрипт создает учетные данные, которые используются для доступа к файлам в хранилище с помощью маркера SAS, указанного в этих учетных данных.

```sql
CREATE DATABASE SCOPED CREDENTIAL [SasToken]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

### <a name="azure-ad-identity"></a>[Удостоверение Azure AD](#tab/user-identity)

Следующий скрипт создает учетные данные уровня базы данных, которые используются для [внешней таблицы](develop-tables-external-tables.md) и в функциях `OPENROWSET`, использующих источник данных с учетными данными для доступа к файлам хранилища с собственными удостоверениями Azure AD.

```sql
CREATE DATABASE SCOPED CREDENTIAL [AzureAD]
WITH IDENTITY = 'User Identity';
GO
```

### <a name="managed-identity"></a>[Управляемое удостоверение](#tab/managed-identity)

Следующий скрипт создает учетные данные уровня базы данных, которые можно использовать для олицетворения текущего пользователя Azure AD в управляемом удостоверении службы. 

```sql
CREATE DATABASE SCOPED CREDENTIAL [SynapseIdentity]
WITH IDENTITY = 'Managed Identity';
GO
```

Учетные данные уровня базы данных не должны совпадать с именем учетной записи хранения, так как они будут явно указаны в объекте DATA SOURCE, который определяет расположение хранилища.

### <a name="public-access"></a>[Открытый доступ](#tab/public-access)

Учетные данные уровня базы данных не обязательны для обращения к общедоступным файлам. Создайте [источник данных без учетных данных уровня базы данных](develop-tables-external-tables.md?tabs=sql-ondemand#example-for-create-external-data-source), чтобы обращаться к общедоступным файлам в службе хранилища Azure.

---

Учетные данные уровня базы данных используются во внешних источниках данных для указания метода проверки подлинности, который будет использоваться для доступа к этому хранилищу.

```sql
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://*******.blob.core.windows.net/samples',
          CREDENTIAL = <name of database scoped credential> 
)
```

## <a name="examples"></a>Примеры

**Обращение к общедоступному источнику данных**

Используйте следующий скрипт, чтобы создать таблицу, которая обращается к общедоступному источнику данных.

```sql
CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] WITH ( FORMAT_TYPE = PARQUET)
GO
CREATE EXTERNAL DATA SOURCE publicData
WITH (    LOCATION   = 'https://****.blob.core.windows.net/public-access' )
GO

CREATE EXTERNAL TABLE dbo.userPublicData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet', DATA_SOURCE = [publicData], FILE_FORMAT = [SynapseParquetFormat] )
```

Пользователь базы данных может считывать содержимое файлов из такого источника данных, используя внешнюю таблицу или функцию [OPENROWSET](develop-openrowset.md), которая ссылается на источник данных:

```sql
SELECT TOP 10 * FROM dbo.userPublicData;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FORMAT=PARQUET) as rows;
GO
```

**Доступ к источнику данных с учетными данными**

Измените следующий скрипт, чтобы создать внешнюю таблицу, которая обращается к службе хранилища Azure с маркером SAS, удостоверением пользователя Azure AD или управляемым удостоверением рабочей области.

```sql
-- Create master key in databases with some password (one-off per database)
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Y*********0'
GO

-- Create databases scoped credential that use User Identity, Managed Identity, or SAS. User needs to create only database-scoped credentials that should be used to access data source:

CREATE DATABASE SCOPED CREDENTIAL MyIdentity WITH IDENTITY = 'User Identity'
GO
CREATE DATABASE SCOPED CREDENTIAL WorkspaceIdentity WITH IDENTITY = 'Managed Identity'
GO
CREATE DATABASE SCOPED CREDENTIAL SasCredential WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2019-10-1********ZVsTOL0ltEGhf54N8KhDCRfLRI%3D'

-- Create data source that one of the credentials above, external file format, and external tables that reference this data source and file format:

CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] WITH ( FORMAT_TYPE = PARQUET)
GO

CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://*******.blob.core.windows.net/samples'
-- Uncomment one of these options depending on authentication method that you want to use to access data source:
--,CREDENTIAL = MyIdentity 
--,CREDENTIAL = WorkspaceIdentity 
--,CREDENTIAL = SasCredential 
)

CREATE EXTERNAL TABLE dbo.userData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FILE_FORMAT = [SynapseParquetFormat] )

```

Пользователь базы данных может считывать содержимое файлов из такого источника данных, используя [внешнюю таблицу](develop-tables-external-tables.md) или функцию [OPENROWSET](develop-openrowset.md), которая ссылается на источник данных:

```sql
SELECT TOP 10 * FROM dbo.userdata;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FORMAT=PARQUET) as rows;
GO
```

## <a name="next-steps"></a>Дальнейшие действия

Приведенные ниже статьи помогут узнать, как включать в запрос разные типы папок и файлов, а также создавать и использовать представления.

- [Запрашивание одного CSV-файла](query-single-csv-file.md)
- [Запрашивание папок и нескольких CSV-файлов](query-folders-multiple-csv-files.md)
- [Запрашивание конкретных файлов](query-specific-files.md)
- [Запрашивание файлов Parquet](query-parquet-files.md)
- [Создание и использование представлений](create-use-views.md)
- [Запрашивание JSON-файлов](query-json-files.md)
- [Запрашивание вложенных типов Parquet](query-parquet-nested-types.md)
