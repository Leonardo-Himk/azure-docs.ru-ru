---
title: Ограничения — база данных Azure для MySQL
description: В этой статье описываются ограничения в службе "База данных Azure для MySQL", например количество подключений и параметры подсистемы хранилища.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: 6ca09ab0578fb88e443d6e9e1f920c22457eb042
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80548475"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Ограничения в службе "База данных Azure для MySQL"
В следующих разделах приводятся ограничения, касающиеся емкости, поддерживаемых подсистем хранилища, поддерживаемых разрешений, поддерживаемых инструкций языка обработки данных и функциональных возможностей в службе базы данных. Кроме того, ознакомьтесь с [общими ограничениями](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html), применимыми к ядру СУБД базы данных MySQL.

## <a name="server-parameters"></a>Параметры сервера

Минимальное и максимальное значения нескольких популярных параметров сервера определяются ценовой категорией и виртуальных ядер. Ограничения см. в таблицах ниже.

### <a name="max_connections"></a>max_connections

|**Ценовая категория**|**Виртуальные ядра**|**Значение по умолчанию**|**Минимальное значение**|**Максимальное значение**|
|---|---|---|---|---|
|Basic|1|50|10|50|
|Basic|2|100|10|100|
|Общее назначение|2|300|10|600|
|Общее назначение|4|625|10|1250|
|Общее назначение|8|1250|10|2500|
|Общее назначение|16|2500|10|5000|
|Общее назначение|32|5000|10|10000|
|Общее назначение|64|10000|10|20 000|
|С оптимизацией для операций в памяти|2|600|10|800|
|С оптимизацией для операций в памяти|4|1250|10|2500|
|С оптимизацией для операций в памяти|8|2500|10|5000|
|С оптимизацией для операций в памяти|16|5000|10|10000|
|С оптимизацией для операций в памяти|32|10000|10|20 000|

При превышении предельного количества подключений может появиться следующая ошибка:
> ОШИБКА 1040 (08004): Слишком много подключений

> [!IMPORTANT]
> Для обеспечения оптимальной работы рекомендуется использовать пул подключений, например Проксискл, для эффективного управления подключениями.

Создание клиентских подключений к MySQL занимает некоторое время и после установки, поэтому эти подключения занимают ресурсы базы данных даже при простое. Большинство приложений запрашивают много кратковременных соединений, в которых возникает такая ситуация. В результате будет меньше ресурсов, доступных для реальной рабочей нагрузки, что приведет к снижению производительности. Это поможет предотвратить использование пула подключений, который уменьшает бездействующие подключения и повторно использует существующие подключения. Дополнительные сведения о настройке Проксискл см. в этой [записи блога](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

### <a name="query_cache_size"></a>query_cache_size

По умолчанию кэш запросов отключен. Чтобы включить кэш запросов, настройте `query_cache_type` параметр. 

Дополнительные сведения об этом параметре см. в [документации по MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_query_cache_size) .

> [!NOTE]
> Кэш запросов устарел в MySQL 5.7.20 и был удален в MySQL 8,0

|**Ценовая категория**|**Виртуальные ядра**|**Значение по умолчанию**|**Минимальное значение**|**Максимальное значение**|
|---|---|---|---|---|
|Basic|1|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Basic|2|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Общее назначение|2|0|0|16777216|
|Общее назначение|4|0|0|33554432|
|Общее назначение|8|0|0|67108864|
|Общее назначение|16|0|0|134217728|
|Общее назначение|32|0|0|134217728|
|Общее назначение|64|0|0|134217728|
|С оптимизацией для операций в памяти|2|0|0|33554432|
|С оптимизацией для операций в памяти|4|0|0|67108864|
|С оптимизацией для операций в памяти|8|0|0|134217728|
|С оптимизацией для операций в памяти|16|0|0|134217728|
|С оптимизацией для операций в памяти|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

Дополнительные сведения об этом параметре см. в [документации по MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_sort_buffer_size) .

|**Ценовая категория**|**Виртуальные ядра**|**Значение по умолчанию**|**Минимальное значение**|**Максимальное значение**|
|---|---|---|---|---|
|Basic|1|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Basic|2|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Общее назначение|2|524288|32768|4194304|
|Общее назначение|4|524288|32768|8388608|
|Общее назначение|8|524288|32768|16777216|
|Общее назначение|16|524288|32768|33554432|
|Общее назначение|32|524288|32768|33554432|
|Общее назначение|64|524288|32768|33554432|
|С оптимизацией для операций в памяти|2|524288|32768|8388608|
|С оптимизацией для операций в памяти|4|524288|32768|16777216|
|С оптимизацией для операций в памяти|8|524288|32768|33554432|
|С оптимизацией для операций в памяти|16|524288|32768|33554432|
|С оптимизацией для операций в памяти|32|524288|32768|33554432|

### <a name="join_buffer_size"></a>join_buffer_size

Дополнительные сведения об этом параметре см. в [документации по MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_join_buffer_size) .

|**Ценовая категория**|**Виртуальные ядра**|**Значение по умолчанию**|**Минимальное значение**|**Максимальное значение**|
|---|---|---|---|---|
|Basic|1|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Basic|2|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Общее назначение|2|262144|128|268435455|
|Общее назначение|4|262144|128|536 870 912|
|Общее назначение|8|262144|128|1073741824|
|Общее назначение|16|262144|128|2147483648|
|Общее назначение|32|262144|128|4294967295|
|Общее назначение|64|262144|128|4294967295|
|С оптимизацией для операций в памяти|2|262144|128|536 870 912|
|С оптимизацией для операций в памяти|4|262144|128|1073741824|
|С оптимизацией для операций в памяти|8|262144|128|2147483648|
|С оптимизацией для операций в памяти|16|262144|128|4294967295|
|С оптимизацией для операций в памяти|32|262144|128|4294967295|

### <a name="max_heap_table_size"></a>max_heap_table_size

Дополнительные сведения об этом параметре см. в [документации по MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_max_heap_table_size) .

|**Ценовая категория**|**Виртуальные ядра**|**Значение по умолчанию**|**Минимальное значение**|**Максимальное значение**|
|---|---|---|---|---|
|Basic|1|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Basic|2|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Общее назначение|2|16777216|16384|268435455|
|Общее назначение|4|16777216|16384|536 870 912|
|Общее назначение|8|16777216|16384|1073741824|
|Общее назначение|16|16777216|16384|2147483648|
|Общее назначение|32|16777216|16384|4294967295|
|Общее назначение|64|16777216|16384|4294967295|
|С оптимизацией для операций в памяти|2|16777216|16384|536 870 912|
|С оптимизацией для операций в памяти|4|16777216|16384|1073741824|
|С оптимизацией для операций в памяти|8|16777216|16384|2147483648|
|С оптимизацией для операций в памяти|16|16777216|16384|4294967295|
|С оптимизацией для операций в памяти|32|16777216|16384|4294967295|

### <a name="tmp_table_size"></a>tmp_table_size

Дополнительные сведения об этом параметре см. в [документации по MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_tmp_table_size) .

|**Ценовая категория**|**Виртуальные ядра**|**Значение по умолчанию**|**Минимальное значение**|**Максимальное значение**|
|---|---|---|---|---|
|Basic|1|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Basic|2|Не настраивается на уровне "базовый"|Недоступно|Недоступно|
|Общее назначение|2|16777216|1024|67108864|
|Общее назначение|4|16777216|1024|134217728|
|Общее назначение|8|16777216|1024|268435456|
|Общее назначение|16|16777216|1024|536 870 912|
|Общее назначение|32|16777216|1024|1073741824|
|Общее назначение|64|16777216|1024|1073741824|
|С оптимизацией для операций в памяти|2|16777216|1024|134217728|
|С оптимизацией для операций в памяти|4|16777216|1024|268435456|
|С оптимизацией для операций в памяти|8|16777216|1024|536 870 912|
|С оптимизацией для операций в памяти|16|16777216|1024|1073741824|
|С оптимизацией для операций в памяти|32|16777216|1024|1073741824|

### <a name="time_zone"></a>time_zone

Таблицы часовых поясов можно заполнить, вызвав `mysql.az_load_timezone` хранимую процедуру из средства вроде командной строки MySQL или MySQL Workbench. Инструкции по вызову хранимой процедуры и настройке часовых поясов глобального времени или уровня сеанса см. в [портал Azure](howto-server-parameters.md#working-with-the-time-zone-parameter) или [Azure CLI](howto-configure-server-parameters-using-cli.md#working-with-the-time-zone-parameter) статьях.

## <a name="storage-engine-support"></a>Поддержка подсистем хранилища

### <a name="supported"></a>Поддерживается
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html);
- [СВОБОДНОЙ](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Не поддерживается
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html);
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html);
- [УПРАЖНЕНИ](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATED](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html).

## <a name="privilege-support"></a>Поддержка разрешений

### <a name="unsupported"></a>Не поддерживается
- Роль DBA. Большое количество параметров и настроек может случайно снизить производительность сервера или отключить свойства ACID СУБД. Поэтому, чтобы обеспечить целостность данных службы и соблюсти соглашение об уровне обслуживания на уровне продукта, мы не предоставляем роль DBA. Учетная запись по умолчанию, которая создается при создании экземпляра базы данных, позволяет пользователям выполнять большинство инструкций языка описания данных (DDL) и языка обработки данных (DML) в управляемом экземпляре базы данных. 
- SUPER Privilege: также ограничены [права суперпользователя](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) .
- Более строгие: для создания и ограничения требуются права суперпользователя. Если импортируются данные с помощью резервной копии, удалите команды `CREATE DEFINER` вручную или с помощью команды `--skip-definer` при выполнении mysqldump.

## <a name="data-manipulation-statement-support"></a>Поддержка инструкций языка обработки данных

### <a name="supported"></a>Поддерживается
- `LOAD DATA INFILE` поддерживается, но должен быть указан параметр `[LOCAL]`, указывающий на путь в формате UNC (хранилище Azure, подключенное по протоколу SMB).

### <a name="unsupported"></a>Не поддерживается
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Ограничения функциональных возможностей

### <a name="scale-operations"></a>Операции масштабирования
- В настоящее время динамическое масштабирование из ценовой категории "Базовый" и в нее не поддерживается.
- Уменьшение размера хранилища сервера не поддерживается.

### <a name="server-version-upgrades"></a>Обновления версии сервера
- В настоящее время автоматический переход между основными версиями ядра СУБД не поддерживается. Чтобы выполнить обновление до следующей основной версии, [восстановите дамп](./concepts-migrate-dump-restore.md) на сервере, который был создан с новой версией ядра.

### <a name="point-in-time-restore"></a>Восстановление до точки во времени
- При использовании компонента PITR создается новый сервер с конфигурацией сервера, с которого он восстанавливается.
- Восстановление удаленного сервера не поддерживается.

### <a name="vnet-service-endpoints"></a>Конечные точки службы виртуальной сети
- Поддержка конечных точек службы виртуальной сети предназначена только для серверов общего назначения и серверов, оптимизированных для операций в памяти.

### <a name="storage-size"></a>Объем памяти
- Сведения об ограничениях на размер хранилища для ценовой категории см. на [этой ценовой категории](concepts-pricing-tiers.md) .

## <a name="current-known-issues"></a>Известные на данный момент проблемы
- После установки подключения для экземпляра сервера MySQL отображается неправильная версия сервера. Чтобы получить правильную версию ядра экземпляра сервера, используйте команду `select version();`.

## <a name="next-steps"></a>Дальнейшие шаги
- [Доступные на каждом уровне служб](concepts-pricing-tiers.md)
- [Поддерживаемые версии базы данных PostgreSQL](concepts-supported-versions.md)
