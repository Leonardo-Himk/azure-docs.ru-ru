---
title: Команды расширения MongoDB для управления данными в API Azure Cosmos DB для MongoDB
description: В этой статье описывается, как использовать команды расширения MongoDB для управления данными, хранящимися в API Azure Cosmos DB для MongoDB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sngun
ms.openlocfilehash: f99c4d096bcbe1fbdc42cac80a491d6017266cb2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80583584"
---
# <a name="use-mongodb-extension-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>Используйте команды расширения MongoDB для управления данными, хранящимися в API-интерфейсе Azure Cosmos DB для MongoDB 

Azure Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. Вы можете взаимодействовать с API Azure Cosmos DB для MongoDB с помощью любого из [драйверов клиента](https://docs.mongodb.org/ecosystem/drivers)с открытым кодом MongoDB. API Azure Cosmos DB для MongoDB позволяет использовать существующие клиентские драйверы, применяя [протокол MongoDB Wire](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

Используя API Azure Cosmos DB для MongoDB, вы можете воспользоваться преимуществами Cosmos DB таких, как глобальное распространение, автоматическое сегментирование, высокая доступность, гарантия задержки, автоматическое шифрование при хранении, резервное копирование и многое другое, сохраняя инвестиции в приложение MongoDB.

## <a name="mongodb-protocol-support"></a>Поддержка протокола MongoDB

По умолчанию API Azure Cosmos DB для MongoDB совместим с MongoDB Server версии 3,2. Дополнительные сведения см. в разделе [Поддерживаемые функции и синтаксис](mongodb-feature-support.md). Функции или операторы запросов, добавленные в MongoDB версии 3,4, в настоящее время доступны в виде предварительной версии в API Azure Cosmos DB для MongoDB. Следующие команды расширения поддерживают определенные функции Azure Cosmos DB при выполнении операций CRUD с данными, хранящимися в API Azure Cosmos DB для MongoDB:

* [Создание базы данных](#create-database)
* [Обновление базы данных](#update-database)
* [Получение базы данных](#get-database)
* [Создать коллекцию](#create-collection)
* [Обновить коллекцию](#update-collection)
* [Получить коллекцию](#get-collection)

## <a name="create-database"></a><a id="create-database"></a>Создание базы данных

Команда создания расширения базы данных создает новую базу данных MongoDB. Имя базы данных используется в контексте баз данных, для которого выполняется команда. Команда CreateDatabase имеет следующий формат:

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

В следующей таблице описаны параметры в команде.

|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
| customAction   |  строка  |   Имя пользовательской команды должно быть "CreateDatabase".      |
| офферсраугхпут | INT  | Подготовленная пропускная способность, заданная для базы данных. Это необязательный параметр. |

### <a name="output"></a>Выходные данные

Возвращает ответ пользовательской команды по умолчанию. Просмотрите [выходные данные пользовательской команды по умолчанию](#default-output) для параметров в выходных данных.

### <a name="examples"></a>Примеры

**Создание базы данных**

Чтобы создать базу данных с именем Test, используйте следующую команду:

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**Создание базы данных с пропускной способностью**

Чтобы создать базу данных с именем "Test" и подготовленную пропускную способность 1000 RUs, используйте следующую команду:

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

## <a name="update-database"></a><a id="update-database"></a>Обновить базу данных

Команда обновить расширение базы данных обновляет свойства, связанные с указанной базой данных. В настоящее время можно обновить только свойство "Офферсраугхпут".

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

В следующей таблице описаны параметры в команде.

|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
| customAction    |    строка     |   Имя пользовательской команды. Должно быть "Упдатедатабасе".      |
|  офферсраугхпут   |  INT       |     Новая подготовленная пропускная способность, которую необходимо задать для базы данных.    |

### <a name="output"></a>Выходные данные

Возвращает ответ пользовательской команды по умолчанию. Просмотрите [выходные данные пользовательской команды по умолчанию](#default-output) для параметров в выходных данных.

### <a name="examples"></a>Примеры

**Обновление подготовленной пропускной способности, связанной с базой данных**

Чтобы обновить подготовленную пропускную способность базы данных с именем Test, на 1200 RUs, используйте следующую команду:

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

## <a name="get-database"></a><a id="get-database"></a>Получение базы данных

Команда получения расширения базы данных возвращает объект базы данных. Имя базы данных используется в контексте базы данных, для которой выполняется команда.

```
{
  customAction: "GetDatabase"
}
```

В следующей таблице описаны параметры в команде.


|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
|  customAction   |   строка      |   Имя пользовательской команды. Необходимо указать "".|
        
### <a name="output"></a>Выходные данные

Если команда выполнена, ответ содержит документ со следующими полями:

|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
|  `ok`   |   `int`     |   Состояние ответа. 1 = = успешное завершение. 0 = = сбой.      |
| `database`    |    `string`        |   Имя базы данных.      |
|   `provisionedThroughput`  |    `int`      |    Подготовленная пропускная способность, заданная для базы данных. Это необязательный параметр ответа.     |

Если команда завершается ошибкой, возвращается пользовательский ответ команды по умолчанию. Просмотрите [выходные данные пользовательской команды по умолчанию](#default-output) для параметров в выходных данных.

### <a name="examples"></a>Примеры

**Получение базы данных**

Чтобы получить объект базы данных с именем Test, используйте следующую команду:

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

## <a name="create-collection"></a><a id="create-collection"></a>Создать коллекцию

Команда Create Collection создает новую коллекцию MongoDB. Имя базы данных используется в контексте баз данных, для которого выполняется команда. Команда CreateCollection имеет следующий формат:

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

В следующей таблице описаны параметры в команде.

| **Поле** | **Type** | **Обязательное** | **Описание** |
|---------|---------|---------|---------|
| customAction | строка | Обязательный | Имя пользовательской команды. Должно быть "CreateCollection".|
| коллекция | строка | Обязательный | Имя коллекции. Специальные символы не допускаются.|
| офферсраугхпут | INT | Используемых | Подготовленная пропускная способность для установки в базе данных. Если этот параметр не указан, по умолчанию он будет иметь минимальный 400 единиц запросов в секунду. * Чтобы указать пропускную способность свыше 10 000 единиц запросов `shardKey` в секунду, параметр является обязательным.|
| шардкэй | строка | Используемых | Путь к ключу сегмента для сегментированной коллекции. Этот параметр является обязательным, если задано более 10 000 единиц запросов в `offerThroughput`секунду.  Если он указан, это значение потребуется для всех вставленных документов. |

### <a name="output"></a>Выходные данные

Возвращает ответ пользовательской команды по умолчанию. Просмотрите [выходные данные пользовательской команды по умолчанию](#default-output) для параметров в выходных данных.

### <a name="examples"></a>Примеры

**Создание несегментированной коллекции**

Чтобы создать несегментированную коллекцию с именем "Тестколлектион" и подготовленной пропускной способностью 1000 RUs, используйте следующую команду: 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**Создание сегментированной коллекции**

Чтобы создать сегментированную коллекцию с именем "Тестколлектион" и подготовленную пропускную способность 1000 RUs, и свойство шардкэй "a. b", используйте следующую команду:

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

## <a name="update-collection"></a><a id="update-collection"></a>Обновить коллекцию

Команда Update Collection обновляет свойства, связанные с указанной коллекцией.

```
{
  customAction: "UpdateCollection",
  collection: <Name of the collection that you want to update>,
  offerThroughput: <New throughput that you want to provision on the collection> 
}
```

В следующей таблице описаны параметры в команде.

|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
|  customAction   |   строка      |   Имя пользовательской команды. Должно быть "Упдатеколлектион".      |
|  коллекция   |   строка      |   Имя коллекции.       |
| офферсраугхпут   |INT|   Подготовленная пропускная способность для установки в коллекции.|

## <a name="output"></a>Выходные данные

Возвращает ответ пользовательской команды по умолчанию. Просмотрите [выходные данные пользовательской команды по умолчанию](#default-output) для параметров в выходных данных.

### <a name="examples"></a>Примеры

**Обновление подготовленной пропускной способности, связанной с коллекцией**

Чтобы обновить подготовленную пропускную способность коллекции с именем "Тестколлектион" до 1200 RUs, используйте следующую команду:

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

## <a name="get-collection"></a><a id="get-collection"></a>Получить коллекцию

Настраиваемая команда Get Collection возвращает объект коллекции.

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

В следующей таблице описаны параметры в команде.


|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
| customAction    |   строка      |   Имя пользовательской команды. Должен быть "IsCollection".      |
| коллекция    |    строка     |    Имя коллекции.     |

### <a name="output"></a>Выходные данные

Если команда выполнена, ответ содержит документ со следующими полями


|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Состояние ответа. 1 = = успешное завершение. 0 = = сбой.      |
| `database`    |    `string`     |   Имя базы данных.      |
| `collection`    |    `string`     |    Имя коллекции.     |
|  `shardKeyDefinition`   |   `document`      |  Документ спецификации индекса, используемый в качестве ключа сегмента. Это необязательный параметр ответа.       |
|  `provisionedThroughput`   |   `int`      |    Подготовленная пропускная способность для установки в коллекции. Это необязательный параметр ответа.     |

Если команда завершается ошибкой, возвращается пользовательский ответ команды по умолчанию. Просмотрите [выходные данные пользовательской команды по умолчанию](#default-output) для параметров в выходных данных.

### <a name="examples"></a>Примеры

**Получение коллекции**

Чтобы получить объект коллекции для коллекции с именем "Тестколлектион", используйте следующую команду:

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

## <a name="default-output-of-a-custom-command"></a><a id="default-output"></a>Выходные данные пользовательской команды по умолчанию

Если не указано, пользовательский ответ содержит документ со следующими полями:

|**Поле**|**Type** |**Описание** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Состояние ответа. 1 = = успешное завершение. 0 = = сбой.      |
| `code`    |   `int`      |   Возвращается только при сбое команды (т. е. ОК = = 0). Содержит код ошибки MongoDB. Это необязательный параметр ответа.      |
|  `errMsg`   |  `string`      |    Возвращается только при сбое команды (т. е. ОК = = 0). Содержит понятное сообщение об ошибке. Это необязательный параметр ответа.      |

## <a name="next-steps"></a>Дальнейшие шаги

Далее можно перейти к рассмотрению следующих понятий Azure Cosmos DB: 

* [Индексирование в Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Срок жизни для данных Azure Cosmos DB](../cosmos-db/time-to-live.md)
