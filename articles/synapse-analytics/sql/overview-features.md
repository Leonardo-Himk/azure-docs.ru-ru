---
title: Отличия функций T-SQL в Azure Synapse SQL
description: Список функций Transact-SQL, доступных в Synapse SQL.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: b3e4d705bea7243966959038cf15373097c73ebc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421288"
---
# <a name="transact-sql-features-supported-in-azure-synapse-sql"></a>Функции T-SQL, которые поддерживаются в Azure Synapse SQL

Azure Synapse SQL — это служба аналитики больших данных, которая позволяет выполнять запросы и анализ данных с помощью языка T-SQL. Вы можете использовать для анализа данных диалект SQL, соответствующий стандарту ANSI, который используется в SQL Server и Базе данных SQL Azure. 

Язык Transact-SQL используется в Synapse SQL бессерверно, а подготовленная модель может ссылаться на разные объекты и имеет некоторые отличия в наборе поддерживаемых функций. На этой странице перечислены основные отличия языка Transact-SQL в разных моделях потребления Synapse SQL.

## <a name="database-objects"></a>Объекты базы данных

Модели потребления в Synapse SQL позволяют использовать разные объекты базы данных. В следующей таблице представлено сравнение поддерживаемых типов объектов.

|   | Подготовлено | Бессерверные приложения |
| --- | --- | --- |
| Таблицы | [Да](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Нет, бессерверная модель может запрашивать только внешние данные, размещенные в [службе хранилище Azure](#storage-options) |
| Представления | [Да.](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Представления могут использовать [элементы языка запросов](#query-language), доступные в подготовленной модели. | [Да.](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Представления могут использовать [элементы языка запросов](#query-language), доступные в бессерверной модели. |
| Схемы | [Да](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Да](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| Временные таблицы | [Да](../sql-data-warehouse/sql-data-warehouse-tables-temporary.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | нет |
| Процедуры | [Да](/sql/t-sql/statements/create-procedure-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | нет |
| Функции | [Да](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | нет |
| Триггеры | нет | нет |
| Внешние таблицы | [Да.](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) См. поддерживаемые [форматы данных](#data-formats). | [Да.](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) См. поддерживаемые [форматы данных](#data-formats). |
| Кэширование запросов | Да, несколько форм (кэширование на твердотельных накопителях, кэширование в памяти, кэширование ResultSet). Кроме того, поддерживаются материализованные представления | нет |
| Табличные переменные | [Нет](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), используйте временные таблицы | нет |

## <a name="query-language"></a>Язык запросов

Языки запросов, используемые в Synapse SQL, могут поддерживать разный набор функций в зависимости от модели потребления. В следующей таблице приведены наиболее важные отличия в диалектах языка запросов для Transact-SQL:

|   | Подготовлено | Бессерверные приложения |
| --- | --- | --- |
| Инструкция SELECT | Да. Не поддерживаются предложения запроса Transact-SQL [FOR XML/FOR JSON](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да. Не поддерживаются предложения запроса Transact-SQL [FOR XML](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), а также указания запроса. [OFFSET/FETCH](/sql/t-sql/queries/select-order-by-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#using-offset-and-fetch-to-limit-the-rows-returned) и [PIVOT/UNPIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) могут использоваться только для запроса по данным во временных таблицах (но не по внешним данным). |
| Инструкция INSERT | Да | нет |
| Инструкция UPDATE | Да | нет |
| Инструкция DELETE | Да | нет |
| Оператор MERGE | Да | нет |
| Загрузка данных | Да. Лучше всего использовать инструкцию [COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), но система поддерживает для загрузки данных как массовую загрузку (BCP), так и [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | нет |
| Экспорт данных | Да. Использование [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да. Использование [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
| Типы | Да, все типы Transact-SQL, за исключением [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ntext, text и image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [пространственных типов](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да, все типы Transact-SQL, за исключением [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ntext, text и image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [пространственных типов](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и табличного типа. |
| Межбазовые запросы | нет | Да, включая инструкции [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
| Встроенные функции (анализ) | Да, все функции [аналитики](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), преобразования, [даты и времени](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), логические и [математические](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) функции из Transact-SQL, кроме [CHOOSE](/sql/t-sql/functions/logical-functions-choose-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [IIF](/sql/t-sql/functions/logical-functions-iif-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [PARSE](/sql/t-sql/functions/parse-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да, все функции [аналитики](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), преобразования, [даты и времени](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), логические и [математические](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) функции из Transact-SQL. |
| Встроенные функции (текст) | Да. Все функции Transact-SQL для работы со [строками](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и параметрами сортировки, кроме [STRING_ESCAPE](/sql/t-sql/functions/string-escape-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [TRANSLATE](/sql/t-sql/functions/translate-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да. Все функции Transact-SQL для работы со [строками](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и параметрами сортировки. |
| Встроенные функции с табличным значением | Да, все [функции наборов строк в Transact-SQL](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), кроме [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да, все [функции наборов строк в Transact-SQL](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), кроме [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).  |
| Статистические выражения |  Все встроенные статистические функции Transact-SQL, за исключением [CHECKSUM_AGG](/sql/t-sql/functions/checksum-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [GROUPING_ID](/sql/t-sql/functions/grouping-id-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Все встроенные статистические функции Transact-SQL, за исключением [STRING\_AGG](/sql/t-sql/functions/string-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
| Операторы | Да, все [операторы Transact-SQL](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), кроме [!>](/sql/t-sql/language-elements/not-greater-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [!<](/sql/t-sql/language-elements/not-less-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да, только [операторы Transact-SQL](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).  |
| Управление потоком | Да. Все [инструкции Transact-SQL для управления потоком](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), за исключением [CONTINUE](/sql/t-sql/language-elements/continue-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [GOTO](/sql/t-sql/language-elements/goto-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и [WAITFOR](/sql/t-sql/language-elements/waitfor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да. Все [инструкции Transact-SQL для управления потоком](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), за исключением [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) и запроса SELECT в условии `WHILE (...)`. |
| Запросы DDL (создание, изменение и удаление) | Да. Все инструкции DDL из Transact-SQL, применимые к поддерживаемым типам объектов. | Да. Все инструкции DDL из Transact-SQL, применимые к поддерживаемым типам объектов. |

## <a name="security"></a>Безопасность

Synapse SQL позволяет использовать встроенные функции безопасности для защиты данных и управления доступом. В следующей таблице перечислены основные различия между моделями потребления в Synapse SQL.

|   | Подготовлено | Бессерверные приложения |
| --- | --- | --- |
| Имена входа | Недоступно (в базах данных поддерживаются только автономные пользователи). | Да |
| Пользователи |  Недоступно (в базах данных поддерживаются только автономные пользователи). | Да |
| [Автономные пользователи](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Да. **Примечание.** Только один пользователь AAD может быть администратором с неограниченными правами. | Да |
| Аутентификация Azure Active Directory (AAD) | Да, все пользователи AAD. | Да, имена входа и пользователи AAD. |
| Сквозная проверка подлинности AAD для службы хранилища. | Да | Да |
| Аутентификация по маркерам SAS для службы хранилища. | нет | Да, с использованием [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) в [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) или [CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) на уровне объекта. |
| Проверка подлинности по ключу доступа к хранилищу | Да, с использованием [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) в [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | нет |
| Проверка подлинности по управляемому удостоверению для службы хранилища | Да, с использованием [управляемого удостоверения службы](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Да, с использованием учетных данных `Managed Identity`. |
| Проверка подлинности по удостоверению приложения для службы хранилища | [Да](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | нет |
| Разрешения — на уровне объектов | Да, включая применение операций GRANT, DENY и REVOKE к разрешениям пользователей. | Да, включая применение операций GRANT, DENY и REVOKE к разрешениям пользователей и (или) имен входа для поддерживаемых системных объектов. |
| Разрешения — на уровне схемы | Да, включая применение операций GRANT, DENY и REVOKE к разрешениям пользователей и (или) имен входа для схемы. | Да, включая применение операций GRANT, DENY и REVOKE к разрешениям пользователей и (или) имен входа для схемы. |
| Разрешения — [на уровне базы данных](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Да | Да |
| Разрешения — [на уровне сервера](/sql/relational-databases/security/authentication-access/server-level-roles) | нет | Да, поддерживаются sysadmin и другие серверные роли |
| Роли и группы | Да (в области базы данных) | Да (в области сервера и базы данных) |
| Безопасность и функции идентификации | Некоторые функции и операторы безопасности Transact-SQL: `CURRENT_USER`, `HAS_DBACCESS`, `IS_MEMBER`, `IS_ROLEMEMBER`, `SESSION_USER`, `SUSER_NAME`, `SUSER_SNAME`, `SYSTEM_USER`, `USER`, `USER_NAME`, `EXECUTE AS`, `OPEN/CLOSE MASTER KEY`. | Некоторые функции и операторы безопасности Transact-SQL: `CURRENT_USER`, `HAS_DBACCESS`, `HAS_PERMS_BY_NAME`, `IS_MEMBER', 'IS_ROLEMEMBER`, `IS_SRVROLEMEMBER`, `SESSION_USER`, `SUSER_NAME`, `SUSER_SNAME`, `SYSTEM_USER`, `USER`, `USER_NAME`, `EXECUTE AS` и `REVERT`. Функции безопасности нельзя использовать для запроса по внешним данным (сначала сохраните результат в переменной, чтобы использовать ее в запросе).  |
| DATABASE SCOPED CREDENTIAL | Да | Да |

Пул SQL и SQL по запросу используют для запроса данных стандартный язык Transact-SQL. Подробные сведения о различиях см. в [справочнике по языку Transact-SQL](/sql/t-sql/language-reference).

## <a name="tools"></a>Инструменты

Вы можете использовать разные средства, чтобы подключиться к Synapse SQL для запроса по данным.

|   | Подготовлено | Бессерверные приложения |
| --- | --- | --- |
| Synapse Studio | Да, скрипты SQL. | Да, скрипты SQL. |
| Power BI | Да | [Да](tutorial-connect-power-bi-desktop.md) |
| Azure Analysis Service | Да | Да |
| Azure Data Studio | Да | Да, версия 1.14 или более поздняя. Поддерживаются скрипты SQL и записные книжки SQL. |
| SQL Server Management Studio | Да | Да, версия 18.4 или более поздняя. |

Большинство приложений используют стандартный язык Transact-SQL, который может выполнять запросы по подготовленным и бессерверным моделям потребления в Synapse SQL.

## <a name="storage-options"></a>Варианты хранилищ

Анализируемые данные могут храниться в разных типах хранилищ. Все доступные варианты хранилищ перечислены в следующей таблице.

|   | Подготовлено | Бессерверные приложения |
| --- | --- | --- |
| Внутреннее хранилище | Да | нет |
| Azure Data Lake версии 2 | Да | Да |
| хранилище BLOB-объектов Azure | Да | Да |

## <a name="data-formats"></a>Форматы данных

Анализируемые данные могут храниться в разных форматах. Все доступные для анализа варианты форматов перечислены в следующей таблице.

|   | Подготовлено | Бессерверные приложения |
| --- | --- | --- |
| С разделителями | [Да](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Да](query-single-csv-file.md) |
| CSV | Да (многосимвольные разделители не поддерживаются). | [Да](query-single-csv-file.md) |
| Parquet | [Да](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Да](query-parquet-files.md), включая файлы с [вложенными типами](query-parquet-nested-types.md). |
| Hive ORC | [Да](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | нет |
| Hive (релиз-кандидат) | [Да](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | нет |
| JSON | Да | [Да](query-json-files.md) |
| [Delta-lake](https://delta.io/) | нет | нет |
| [CDM](https://docs.microsoft.com/common-data-model/) | нет | нет |

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о рекомендациях для пула SQL и SQL по запросу можно найти в следующих статьях:

- [Рекомендации по использованию пула SQL](best-practices-sql-pool.md)
- [Рекомендации по использованию SQL по запросу](best-practices-sql-on-demand.md)