---
title: Мониторинг нормализованных единиц запросов/с для контейнера Azure Cosmos или учетной записи
description: Узнайте, как отслеживать нормализованное использование единиц запросов для операции в Azure Cosmos DB. Владельцы учетной записи Azure Cosmos DB могут понять, какие операции потребляют больше единиц запросов.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 05/10/2020
ms.openlocfilehash: 23001bdaab0732dbeb088ebadefa90a27e622b19
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83118821"
---
# <a name="how-to-monitor-normalized-rus-for-an-azure-cosmos-container-or-an-account"></a>Мониторинг нормализованных единиц запросов в секунду для контейнера Azure Cosmos или учетной записи

Azure Monitor для Azure Cosmos DB предоставляет представление метрик для мониторинга учетной записи и создания панелей мониторинга. Метрики по Azure Cosmos DB собираются по умолчанию, для этого компонента не требуется явно включать или настраивать что-либо.

**Нормализованная метрика потребления на единицу** времени используется для того, чтобы увидеть, насколько хорошо насыщенность реплик WRT к потреблению единиц запросов по диапазонам ключей секций. Azure Cosmos DB равномерно распределяет пропускную способность по всем физическим секциям. Эта метрика обеспечивает в секунду представление максимального использования пропускной способности в наборе реплик. Используя эту метрику, если вы видите высокий процент использования единиц запросов, следует увеличить пропускную способность в соответствии с потребностями рабочей нагрузки.

## <a name="what-to-expect-and-do-when-normalized-rus-is-higher"></a>Что делать, если нормализованные единицы запросов в секунду больше

Когда нормализованное потребление единиц запросов/s достигает 100%, клиент получает ошибки ограничения скорости. Клиент должен учитывать время ожидания и повторить попытку. Если существует небольшой пик, который достигает 100%, это означает, что пропускная способность реплики достигла максимального предела производительности. Например, одна операция, например хранимая процедура, которая потребляет все единицы запросов в реплике, приведет к короткому пиковому потреблению в нормализованных единицах запросов/с. В таких случаях не будет непосредственных ошибок ограничения частоты, если частота запросов мала. Это обусловлено тем, что Azure Cosmos DB позволяет запросам выплатить больше, чем подготовленный единиц запросов в секунду, для конкретного запроса, а другие запросы за этот период времени будут ограничены.

Метрики Azure Monitor помогают найти операции на код состояния с помощью метрики " **всего запросов** ". Позже можно будет отфильтровать эти запросы по коду состояния 429 и разделить их по **типу операции**.

Чтобы найти запросы с ограниченными частотами, рекомендуется получить эти сведения с помощью журналов диагностики.

Если существует непрерывная Пиковая нагрузка на 100%, или близко к 100%, рекомендуется увеличить пропускную способность. С помощью метрик Azure Monitor и журналов Azure Monitor Вы можете узнать, какие операции являются высокими и их пиковым использованием.

**Нормализованная метрика потребления единиц** измерения используется также для того, чтобы узнать, какой диапазон ключей разделов более тепло в плане использования. Таким образом, вы обеспечиваете отклонение пропускной способности к диапазону ключей секций. Позже вы сможете просмотреть журнал **партитионкэйруконсумптион** в журналах Azure Monitor, чтобы получить сведения о том, какие ключи логических секций являются горячими в условиях использования.

## <a name="view-the-normalized-request-unit-consumption-metric"></a>Просмотр нормализованной метрики потребления единиц запросов

1. Войдите на [портал Azure](https://portal.azure.com/).

2. Выберите **монитор** на панели навигации слева и щелкните **метрики**.

   ![Панель "метрики" в Azure Monitor](./media/monitor-normalized-request-units/monitor-metrics-blade.png)

3. В области **метрики** > **выберите ресурс** > выберите нужную **подписку**и **группу ресурсов**. Для параметра **тип ресурса**выберите **учетные записи Azure Cosmos DB**, выберите одну из существующих учетных записей Azure Cosmos и нажмите кнопку **Применить**.

   ![Выберите учетную запись Azure Cosmos для просмотра метрик](./media/monitor-normalized-request-units/select-cosmos-db-account.png)

4. Затем можно выбрать метрику из списка доступных метрик. Вы можете выбрать метрики, связанные с единицами запросов, хранилищем, задержкой, доступностью, Cassandra и другими. Подробные сведения обо всех доступных метриках в этом списке см. в статье [метрики по категориям](monitor-cosmos-db-reference.md) . В этом примере мы выбираем **нормализованную метрику потребления единиц запросов** и **максимум** в качестве значения статистической обработки.

   В дополнение к этим сведениям можно также выбрать **диапазон времени** и **степень гранулярности времени** метрик. В поле максимум можно просмотреть метрики за последние 30 дней.  После применения фильтра на основе фильтра отобразится диаграмма.

   ![Выберите метрику из портал Azure](./media/monitor-normalized-request-units/normalized-request-unit-usage-metric.png)

### <a name="filters-for-normalized-request-unit-consumption"></a>Фильтры для нормализованного потребления единиц запросов

Можно также фильтровать метрики и диаграмму, отображаемую конкретными **CollectionName**, **DatabaseName**, **партитионкэйранжеид**и **регионом**. Чтобы отфильтровать метрики, щелкните **Добавить фильтр** и выберите требуемое свойство, например **CollectionName** , и соответствующее значение, которое вас интересует. Затем на диаграмме отображаются нормализованные единицы потребления единиц запросов, потребляемые для контейнера за выбранный период.  

Метрики можно группировать с помощью параметра **Применить разделение** .  

Нормализованная метрика потребления единиц запросов для каждого контейнера отображается, как показано на следующем рисунке:

![Применение фильтров к нормализованной метрике потребления единиц запросов](./media/monitor-normalized-request-units/normalized-request-unit-usage-filters.png)

## <a name="next-steps"></a>Дальнейшие действия

* Отслеживайте Azure Cosmos DB данные с помощью [параметров диагностики](cosmosdb-monitor-resource-logs.md) в Azure.
* [Аудит операций Azure Cosmos DB плоскости управления](audit-control-plane-logs.md)