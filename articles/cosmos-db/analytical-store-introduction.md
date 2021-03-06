---
title: Что такое Аналитическое хранилище Azure Cosmos DB (предварительная версия)?
description: Общие сведения о Azure Cosmos DB транзакционных (основанных на строках) и аналитических (основанных на столбцах) хранилищах. Преимущества аналитического хранилища, влияние на производительность для крупномасштабных рабочих нагрузок и автоматическая синхронизация данных из хранилища транзакций в аналитическое хранилище
author: SriChintala
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: srchi
ms.openlocfilehash: c78a7d26100d3c3454cd96e2ac79e1767e5efcdb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594383"
---
# <a name="what-is-azure-cosmos-db-analytical-store-preview"></a>Что такое Аналитическое хранилище Azure Cosmos DB (предварительная версия)?

> [!IMPORTANT]
> Аналитическое хранилище Azure Cosmos DB в настоящее время находится на этапе предварительной версии. Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Дополнительные сведения см. в статье [Дополнительные условия использования Предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Аналитическое хранилище Azure Cosmos DB — это полностью изолированное хранилище столбцов, позволяющее выполнять крупномасштабный анализ операционных данных в Azure Cosmos DB без каких-либо последствий для транзакционных рабочих нагрузок.  

## <a name="challenges-with-large-scale-analytics-on-operational-data"></a>Сложности крупномасштабного анализа операционных данных

Многомодельные операционные данные в контейнере Azure Cosmos DB хранятся в индексированном транзакционном хранилище на основе строк. Формат хранилища строк предназначен для ускорения операций чтения и записи транзакций при миллисекундном времени для ответа и оперативных запросов. Если размер набора данных растет, сложные аналитические запросы могут быть дорогостоящими с точки зрения подготовленной пропускной способности для данных, хранящихся в этом формате. Высокий уровень потребления подготовленной пропускной способности, в свою очередь, влияет на производительность транзакционных рабочих нагрузок, используемых приложениями и службами в режиме реального времени.

Обычно для анализа больших объемов данных операционные данные извлекаются из хранилища транзакций Azure Cosmos DB и хранятся на отдельном уровне данных. Например, данные хранятся в хранилище данных или озере данных в подходящем формате. Эти данные позже используются для крупномасштабной аналитики и анализируются с помощью подсистемы вычислений, например кластеров Apache Spark. Такое разделение аналитического и вычислительного уровней для операционных данных приводит к дополнительной задержке, так как конвейеры ETL (извлечение, преобразование, загрузка) выполняются реже, чтобы сократить влияние на транзакционные рабочие нагрузки.

Конвейеры ETL также усложняются при обработке обновлений операционных данных по сравнению с обработкой только новых принимаемых операционных данных. 

## <a name="column-oriented-analytical-store"></a>Аналитическое хранилище для столбцов

Аналитическое хранилище Azure Cosmos DB устраняет проблемы сложности и задержки, возникающие при использовании традиционных конвейеров ETL. Аналитическое хранилище Azure Cosmos DB может автоматически синхронизировать операционные данные с отдельным хранилищем столбцов. Формат хранилища столбцов подходит для выполнения крупномасштабных аналитических запросов оптимизированным образом, что приводит к сокращению задержки таких запросов.

Используя Azure Synapse Link, можно создавать решения HTAP без ETL, напрямую связываясь с аналитическим хранилищем Azure Cosmos DB из Synapse Analytics. Благодаря этому можно выполнять крупномасштабную аналитику на операционных данных почти в реальном времени.

## <a name="analytical-store-details"></a>Сведения о аналитическом хранилище

При включении аналитического хранилища в контейнере Azure Cosmos DB новое хранилище столбцов создается на основе операционных данных в контейнере. Это хранилище столбцов сохраняется отдельно от транзакционного хранилища строк для этого контейнера. Операции вставки, обновления и удаления для операционных данных автоматически синхронизируются с аналитическим хранилищем. Для синхронизации данных не требуется канал изменений или ETL.

### <a name="column-store-for-analytical-workloads-on-operational-data"></a>Хранилище столбцов для аналитических рабочих нагрузок в операционных данных

Аналитические рабочие нагрузки обычно содержат агрегаты и последовательные проверки выбранных полей. Благодаря хранению данных в виде столбцов аналитическое хранилище позволяет группировать значения для каждого поля, чтобы сериализовать их вместе. Такой формат сокращает количество операций ввода-вывода, необходимых для сканирования или вычислений статистики по конкретным полям. Он значительно сокращает время отклика запросов для больших наборов данных. 

Например, если рабочие таблицы имеют следующий формат:

![Пример операционной таблицы](./media/analytical-store-introduction/sample-operational-data-table.png)

Хранилище строк сохраняет приведенные выше данные в сериализованном формате в строке на диске. Такой формат позволяет быстрее выполнять транзакционные операции чтения, записи и оперативные запросы, например "Вернуть сведения о product1". Однако по мере роста набора данных и необходимости выполнять сложные аналитические запросы к данным, такие запросы могут оказаться дорогостоящими. Например, если вы хотите получить тенденции продаж для продукта в категории "Оборудование"по разным подразделениям и месяцам, необходимо выполнить сложный запрос. Крупные проверки этого набора данных могут оказаться затратными в плане подготовленной пропускной способности и могут повлиять на производительность транзакционных рабочих нагрузок, обеспечивающих работу приложений и служб в режиме реального времени.

Аналитическое хранилище, которое является хранилищем столбцов, лучше подходит для таких запросов, так как оно сериализует похожие поля данных вместе и сокращает число операций ввода-вывода на диск.

На следующем рисунке показано хранилище транзакций и хранилище аналитических столбцов в Azure Cosmos DB:

![Транзакционное хранилище строк и хранилище аналитических столбцов в Azure Cosmos DB](./media/analytical-store-introduction/transactional-analytical-data-stores.png)

### <a name="decoupled-performance-for-analytical-workloads"></a>Несвязанная производительность для аналитических рабочих нагрузок

Аналитические запросы не влияют на производительность транзакционных рабочих нагрузок, так как аналитическое хранилище отделено от хранилища транзакций.  Для аналитического хранилища не требуется выделять отдельные единицы запроса (ЕЗ).

### <a name="auto-sync"></a>Автосинхронизация

Автоматическая синхронизация — это полностью управляемая возможность Azure Cosmos DB, при которой операции вставки, обновления и удаления в операционных данных автоматически синхронизируются из хранилища транзакций в аналитическое хранилище в течение 5 минут.

Функция автоматической синхронизации вместе с аналитическим хранилищем предоставляет следующие основные преимущества.

#### <a name="scalability--elasticity"></a>Масштабируемость и эластичность

Используя горизонтальное секционирование, хранилище транзакций Azure Cosmos DB может эластично масштабировать хранилище и пропускную способность без простоев. Горизонтальное секционирование в хранилище транзакций обеспечивает масштабируемость и эластичность при автоматической синхронизации, чтобы обеспечить синхронизацию данных с аналитическим хранилищем почти в реальном времени. Синхронизация данных происходит независимо от объема трафика транзакций, который составляет 1000 операций/с или 1 000 000 операций/с, и не влияет на подготовленную пропускную способность в хранилище транзакций. 

#### <a name="automatically-handle-schema-updates"></a><a id="analytical-schema"></a>Автоматическая работа с обновлениями схемы

Хранилище транзакций Azure Cosmos DB не зависит от схемы и позволяет выполнять итерации по транзакционным приложениям без необходимости управлять схемой или индексами. В отличие от этого, аналитическое хранилище Azure Cosmos DB разработано с целью оптимизации производительности аналитических запросов. С возможностью автоматической синхронизации Azure Cosmos DB управляет выводом схемы поверх последних обновлений из хранилища транзакций.  Она также управляет готовым представлением схемы в аналитическом хранилище, которое выполняет обработку вложенных типов данных.

В случае эволюции схемы, когда со временем добавляются новые свойства, аналитическое хранилище автоматически представляет объединенную схему для всех предыдущих версий схем в хранилище транзакций.

Если все операционные данные в Azure Cosmos DB следуют четко определенной схеме для аналитики, то схема автоматически выводится и правильно представляется в аналитическом хранилище. Если четко определенная схема для аналитики, как определено ниже, нарушается определенными элементами, они не будут включаться в аналитическое хранилище. Если у вас есть сценарии, заблокированные из-за четко определенной схемы для определения аналитики, отправьте сообщение электронной почты [команде Azure Cosmos DB](mailto:cosmosdbsynapselink@microsoft.com).

Четко определенная схема для аналитики определяется следующими соображениями.

* У нескольких элементов свойство всегда имеет одинаковый тип

  * Например, `{"a":123} {"a": "str"}` не имеет четко определенной схемы, так как `"a"` иногда является строкой, а иногда числом. 
  
    В этом случае аналитическое хранилище регистрирует тип данных `“a”` как тип данных `“a”` в первом появившемся элементе на время существования контейнера. Элементы, в которых тип данных `“a”` отличается, не будут включаться в аналитическое хранилище.
  
    Это условие не применяется к свойствам со значением NULL. Например, `{"a":123} {"a":null}` по-прежнему четко определено.

* Типы массивов должны содержать один повторяющийся тип

  * Например, `{"a": ["str",12]}` не является четко определенной схемой, так как массив содержит сочетание целочисленных и строковых типов

* Существует максимум 200 свойств на любом уровне вложенности схемы, а максимальная глубина вложенности равна 5

  * Элемент с 201 свойством на верхнем уровне не имеет четко определенной схемы.

  * Элемент с более чем пятью вложенными уровнями в схеме также не имеет четко определенной схемы. Например `{"level1": {"level2":{"level3":{"level4":{"level5":{"too many":12}}}}}}`.

* Имена свойств уникальны при сравнении без учета регистра

  * Например, следующие элементы не имеют четко определенной схемы `{"Name": "fred"} {"name": "john"}` — `"Name"` и `"name"` одинаковы при сравнении без учета регистра.

### <a name="cost-effective-archival-of-historical-data"></a>Экономичное архивирование исторических данных

Распределение данных по уровням означает разделение данных между инфраструктурами хранилища, оптимизированными для различных сценариев. Таким образом повышается общая производительность и экономичность всего стека данных. С помощью аналитического хранилища Azure Cosmos DB теперь поддерживается автоматическое распределение данных из хранилища транзакций в аналитическое хранилище с различными макетами данных. При использовании аналитического хранилища, оптимизированного с точки зрения стоимости хранилища по сравнению с хранилищем транзакций, можно хранить более длинные горизонты операционных данных для исторического анализа.

После включения аналитического хранилища в зависимости от потребностей хранения данных в транзакционных рабочих нагрузках можно настроить свойство "время жизни в хранилище транзакций (транзакционное TTL)", чтобы записи автоматически удалялись из хранилища транзакций по истечении определенного периода времени. Аналогично, "время жизни в аналитическом хранилище (аналитическое TTL)" позволяет управлять жизненным циклом данных, хранимых в аналитическом хранилище, независимом от хранилища транзакций. Включив аналитическое хранилище и настроив свойства TTL, можно эффективно распределять и определять срок хранения данных для двух хранилищ.

### <a name="global-distribution"></a>Глобальное распределение

Если вы используете глобально распределенную учетную запись Azure Cosmos DB, после включения аналитического хранилища для контейнера он будет доступен во всех регионах этой учетной записи.  Любые изменения в операционных данных глобально реплицируются во всех регионах. Это позволяет эффективно выполнять аналитические запросы по отношению к ближайшей региональной копии данных в Azure Cosmos DB.

### <a name="security"></a>Безопасность

Проверка подлинности для аналитического хранилища совпадает с проверкой для хранилища транзакций в заданной базе данных. Для проверки подлинности можно использовать главный ключ или ключи только для чтения. Чтобы не вставлять ключи Azure Cosmos DB в записные книжки Spark, можно использовать связанную службу в Synapse Studio. Доступ к этой связанной службе предоставляется всем, у кого есть доступ к рабочей области.

### <a name="support-for-multiple-azure-synapse-analytics-runtimes"></a>Поддержка нескольких сред выполнения Azure Synapse Analytics

Аналитическое хранилище оптимизировано для обеспечения масштабируемости, эластичности и производительности для аналитических рабочих нагрузок без какой-либо зависимости от времени выполнения вычислений. Технология хранения самостоятельно оптимизирует рабочие нагрузки аналитики без вмешательства вручную.

Отделив систему аналитического хранилища от аналитической системы вычислений, данные в аналитическом хранилище Azure Cosmos DB можно запрашивать одновременно из разных сред выполнения аналитики, поддерживаемых Azure Synapse Analytics. На сегодняшний день Synapse Analytics поддерживает Apache Spark и бессерверную среду SQL с аналитическим хранилищем Azure Cosmos DB.

> [!NOTE]
> Чтение данных из аналитического хранилища можно выполнять только с помощью среды выполнения Synapse Analytics. Данные можно записать обратно в хранилище транзакций в качестве уровня обслуживания.

## <a name="pricing"></a><a id="analytical-store-pricing"></a> Цены

Аналитическое хранилище соответствует модели ценообразования на основе потребления, в которой вы платите за:

* Хранилище: объем данных, хранящихся в хранилище в течение месяца, включая исторические данные в соответствии с аналитическим TTL.

* Аналитические операции записи: полностью управляемая синхронизация обновлений операционных данных в хранилище транзакций (автоматическая синхронизация).

* Аналитические операции чтения: операции чтения, выполняемые для аналитического хранилища из Synapse Analytics Spark и бессерверной среды SQL.

> [!NOTE]
> Аналитическое хранилище Azure Cosmos DB доступно как общедоступная предварительная бесплатная до 30 августа 2020 года версия.

Цены на аналитическое хранилище определяются отдельно от модели ценообразования для хранилища транзакций. В аналитическом хранилище нет понятия подготовленных ЕЗ. Подробные сведения о модели ценообразования для аналитического хранилища см. на странице цен для [Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).

Чтобы получить общую оценку затрат для включения аналитического хранилища для контейнера Azure Cosmos DB, можно воспользоваться планировщиком ресурсов [Azure Cosmos DB](https://cosmos.azure.com/capacitycalculator/) и получить оценку затрат на аналитическое хранилище и операции записи. Затраты на аналитические операции чтения зависят от характеристик рабочей нагрузки аналитики, но согласно общей оценке проверка 1 ТБ данных в аналитическом хранилище обычно приводит к 130 000 аналитических операций чтения и стоит 0,065 $.

## <a name="analytical-time-to-live-ttl"></a><a id="analytical-ttl"></a> Аналитическое время жизни (TTL)

Аналитическое TTL указывает, как долго данные должны храниться в аналитическом хранилище для контейнера. 

Операции вставки, обновления и удаления данных в операционных данных автоматически синхронизируются из хранилища транзакций в аналитическое хранилище независимо от конфигурации для транзакционного TTL. Хранение этих операционных данных в аналитическом хранилище может осуществляться с помощью аналитического TTL на уровне контейнера, как указано ниже:

Аналитическое TTL в контейнере задается с помощью свойства `AnalyticalStoreTimeToLiveInSeconds`:

* Если задано значение "0", не задано (или установлено значение NULL): аналитическое хранилище отключено, а данные из хранилища транзакций в аналитическое хранилище не реплицируются

* Если параметр есть и для него задано значение "-1", аналитическое хранилище сохраняет все исторические данные независимо от хранения данных в хранилище транзакций. Этот параметр указывает на бесконечное время хранения операционных данных в аналитическом хранилище.

* Если параметр есть и для него задано положительное число "n", срок хранения элементов в аналитическом хранилище истечет через "n" секунд после их последнего изменения в хранилище транзакций. Этот параметр можно использовать, если требуется сохранить операционные данные в течение ограниченного периода времени в аналитическом хранилище независимо от хранения данных в хранилище транзакций.

Учитывайте следующие факторы.
*   После активации аналитического хранилища с заданным значением аналитического TTL, это значение можно обновить до другого допустимого значения позднее. 
*   Хотя параметр TTL можно установить на уровне контейнера или элемента, аналитическое время жизни можно задать только на уровне контейнера.
*   Длительного хранения операционных данных в аналитическом хранилище можно достичь, установив аналитическое время жизни > = транзакционного времени жизни на уровне контейнера.
*   Аналитическое хранилище может быть создано для зеркалирования хранилища транзакций путем установки аналитического TTL равным транзакционному TTL.

Дополнительные сведения см. в разделе [Настройка аналитического времени жизни для контейнера](configure-synapse-link.md#create-analytical-ttl).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в следующих документах:

* [Сведения об Azure Synapse Link для Azure Cosmos DB](synapse-link.md)

* [Начало работы с Azure Synapse Link для Azure Cosmos DB](configure-synapse-link.md)

* [Часто задаваемые вопросы о Azure Synapse Link для Azure Cosmos DB](synapse-link-frequently-asked-questions.md).

* [Варианты использования Azure Synapse Link для Azure Cosmos DB](synapse-link-use-cases.md)
