---
title: Ценовые категории службы "База данных Azure для MariaDB"
description: Узнайте о различных ценовых категориях для базы данных Azure для MariaDB, включая поколения вычислений, типы хранилища, размер хранилища, виртуальных ядер, память и сроки хранения резервных копий.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: f00d93a639bacd1d0862fed7b6b003302bb2920e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82097665"
---
# <a name="azure-database-for-mariadb-pricing-tiers"></a>Ценовые категории службы "База данных Azure для MariaDB"

Вы можете создать сервер базы данных Azure для MariaDB в одной из трех ценовых категорий: "Базовый", "Общего назначения" и "Оптимизированный для операций в памяти". Ценовые категории различаются объемом вычислительных ресурсов в виртуальных ядрах, которые можно подготовить, объемом памяти на виртуальное ядро и технологиями хранения данных. Все ресурсы подготавливаются на уровне сервера MariaDB. У сервера может быть одна или несколько баз данных.

|    | **Basic** | **общего назначения** | **Оптимизированная для памяти** |
|:---|:----------|:--------------------|:---------------------|
| Поколение вычислительных ресурсов | Поколение 5 |Поколение 5 | Поколение 5 |
| Виртуальные ядра | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Объем памяти на виртуальное ядро | 2 ГБ | 5 ГБ | 10 ГБ |
| Объем памяти | От 5 ГБ до 1 ТБ | От 5 ГБ до 4 ТБ | От 5 ГБ до 4 ТБ |
| Срок хранения резервной копии | От 7 до 35 дней | От 7 до 35 дней | От 7 до 35 дней |

Чтобы выбрать ценовую категорию, в качестве отправной точки используйте следующую таблицу.

| Ценовая категория | Целевые рабочие нагрузки |
|:-------------|:-----------------|
| Basic | Рабочие нагрузки с невысокими вычислительными потребностями и производительностью операций ввода-вывода. В качестве примера можно привести серверы, используемые для разработки или тестирования, или небольшие редко используемые приложения. |
| Общее назначение | Для решения большинства бизнес-задач требуются сбалансированные вычислительные ресурсы и память с масштабируемой пропускной способностью операций ввода-вывода. К примерам относятся серверы для размещения мобильных и веб-приложений, а также других корпоративных приложений.|
| С оптимизацией для операций в памяти | Рабочие нагрузки с высокой производительностью базы данных, требующие эффективной обработки данных в памяти для быстрого выполнения транзакций и повышения уровня параллелизма. К примерам относятся серверы для обработки данных в режиме реального времени, а также аналитические или транзакционные приложения с высоким уровнем производительности.|

После создания сервера количество виртуальных ядер и ценовую категорию (кроме переключения на категорию "Базовый" и с нее) можно увеличить и уменьшить в считаные секунды. Кроме того, можно независимо друг от друга увеличивать и уменьшать объем хранилища и срок хранения резервных копий без простоя приложения. После создания сервера невозможно изменить тип хранилища резервных копий. Дополнительные сведения см. в разделе [масштабирование ресурсов](#scale-resources) .

## <a name="compute-generations-and-vcores"></a>Поколения вычислительных ресурсов и виртуальные ядра

Вычислительные ресурсы поставляются в виде виртуальных ядер, которые представляют собой логические ЦП основного оборудования. Логические ЦП 5-го поколения основаны на процессорах Intel E5-2673 v4 (Broadwell) с тактовой частотой 2,3 ГГц.

## <a name="storage"></a>Память

Хранилище, которое вы подготавливаете, определяет объем доступной емкости хранилища для сервера службы "База данных Azure для MariaDB". Хранилище используется для файлов базы данных, временных файлов, журналов транзакций и журналов сервера MariaDB. Общий объем хранилища, который вы подготовили, также определяет доступную производительность операций ввода-вывода для сервера.

|    | **Basic** | **общего назначения** | **Оптимизированная для памяти** |
|:---|:----------|:--------------------|:---------------------|
| Тип хранилища | Базовое хранилище | Хранилище общего назначения | Хранилище общего назначения |
| Объем памяти | От 5 ГБ до 1 ТБ | От 5 ГБ до 4 ТБ | От 5 ГБ до 4 ТБ |
| Шаг приращения хранилища | 1 ГБ | 1 ГБ | 1 ГБ |
| ОПЕРАЦИЙ ВВОДА-ВЫВОДА | Переменная |3 операции ввода-вывода в секунду на ГБ<br/>Минимум 100 операций ввода-вывода в секунду<br/>Макс. 6000 операций ввода-вывода в секунду | 3 операции ввода-вывода в секунду на ГБ<br/>Минимум 100 операций ввода-вывода в секунду<br/>Макс. 6000 операций ввода-вывода в секунду |

Можно добавить дополнительную емкость хранилища во время и после создания сервера, а также разрешить системе автоматически увеличивать размер хранилища в зависимости от объема используемой рабочей нагрузки.

>[!NOTE]
> Масштаб хранилища можно масштабировать, а не уменьшать.

Для ценовой категории "Базовый" фиксированное число операций ввода-вывода в секунду не гарантируется. В категориях "Общего назначения" и "С оптимизацией для операций в памяти" показатель операций ввода-вывода в секунду соотносится с подготовленным размером хранилища как 3 к 1.

Вы можете отслеживать показатель операций ввода-вывода на портале Azure или с помощью команд Azure CLI. Соответствующие метрики для мониторинга — это [предел хранилища, процент использования хранилища, используемый объем хранилища и процент операций ввода-вывода](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Достигнут размер хранилища

Серверы с подготовленным хранилищем объемом не менее 100 ГБ будут помечены как доступные только для чтения, если объем свободного хранилища меньше 5% от подготовленного размера хранилища. Серверы с подготовленным хранилищем более чем на 100 ГБ помечаются как доступные только для чтения, когда остается менее 5 ГБ свободного хранилища.

Например, если подготовлено 110 ГБ хранилища и фактическое использование превышает 105 ГБ, сервер помечается как доступный только для чтения. Кроме того, если вы подготовили 5 ГБ хранилища, сервер помечается как доступная только для чтения, если объем свободного хранилища превысит 256 МБ.

Пока служба пытается пометить сервер как доступный только для чтения, блокируются все новые запросы на запись транзакций, а выполнение существующих активных транзакций продолжается. Если сервер помечен как доступный только для чтения, все последующие операции записи и фиксации транзакций не выполняются. Запросы на чтение продолжат обрабатываться без перерыва. После повышения емкости подготовленного хранилища сервер снова будет готов к принятию транзакций записи.

Рекомендуется включить автоматическое увеличение размера хранилища или настроить оповещение, чтобы уведомлять вас о приближении хранилища сервера к пороговому значению, чтобы избежать появления в состоянии "только для чтения". Дополнительные сведения см. в статье о том, [как настроить оповещение](howto-alert-metric.md).

### <a name="storage-auto-grow"></a>Автоматическое увеличение размера хранилища

Автоматическое увеличение хранилища не дописывает сервера на работу с хранилищем и становится доступно только для чтения. Если автоматическое увеличение размера хранилища включено, хранилище автоматически растет, не влияя на рабочую нагрузку. Для серверов с подготовленным хранилищем объемом менее равного 100 ГБ размер подготовленного хранилища увеличивается на 5 ГБ, когда свободное хранилище меньше 10% от подготовленного хранилища. Для серверов с более чем 100 ГБ подготовленного хранилища размер подготовленного хранилища увеличивается на 5%, если объем свободного пространства не превышает 10 ГБ подготовленного объема хранилища. Применяются максимальные ограничения хранилища, указанные выше.

Например, если подготовлено 1000 ГБ хранилища и фактическое использование превышает 990 ГБ, размер хранилища сервера увеличится до 1050 ГБ. Кроме того, если вы подготовили 10 ГБ хранилища, размер хранилища увеличится до 15 ГБ, если менее 1 ГБ хранилища свободны.

Помните, что хранилище можно масштабировать только вверх, а не вниз.

## <a name="backup"></a>Резервное копирование

В службе автоматически создаются резервные копии сервера. Можно выбрать срок хранения в диапазоне от 7 до 35 дней. Общего назначения и оптимизированные для памяти серверы могут выбрать геоизбыточное хранилище для резервных копий. Дополнительные сведения о резервном копировании см. в [статье основные понятия](concepts-backup.md).

## <a name="scale-resources"></a>Масштабирование ресурсов

После создания сервера можно независимо друг от друга изменять количество виртуальных ядер, ценовую категорию (кроме переключения на категорию "Базовый" и с нее), объем хранилища и срок хранения резервных копий. После создания сервера невозможно изменить тип хранилища резервных копий. Число виртуальных ядер можно увеличивать или уменьшать. Срок хранения резервных копий можно увеличивать и уменьшать в диапазоне от 7 до 35 дней. Размер хранилища можно только увеличить. Масштабирование ресурсов можно выполнить с помощью портала или Azure CLI. 

<!--For an example of scaling by using Azure CLI, see [Monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->

При изменении количества виртуальных ядер или ценовой категории создается копия сервера-источника с новым распределением вычислительных ресурсов. После того как новый сервер заработает, подключения переключатся на него. Во время перехода системы на новый сервер нельзя будет установить новые подключения, а для всех незафиксированных транзакций будет выполнен откат. Длительность этого промежутка может быть разной, но, как правило, он составляет меньше минуты.

Масштабирование хранилища и изменение срока хранения резервных копий — это полностью интерактивные операции. Они не останавливают работу приложения и не влияют на нее. Так как количество операций ввода-вывода в секунду возрастает по мере увеличения размера подготовленного хранилища, вы можете увеличивать количество доступных операций ввода-вывода на сервере путем масштабирования хранилища.

## <a name="pricing"></a>Цены

Наиболее актуальные сведения о стоимости см. в статье [Цены на Базу данных Azure для PostgreSQL](https://azure.microsoft.com/pricing/details/mariadb/). Расходы на вашу конфигурацию можно посмотреть на [портале Azure](https://portal.azure.com/#create/Microsoft.MariaDBServer). На вкладке **Ценовая категория** отображается ежемесячная стоимость выбранных параметров. Если у вас нет подписки Azure, для расчета цены можно воспользоваться калькулятором цен Azure. На сайте с [калькулятором цен Azure](https://azure.microsoft.com/pricing/calculator/) нажмите кнопку **Добавить элементы**, разверните категорию **Базы данных** и выберите **База данных Azure для MariaDB**, чтобы настроить параметры.

## <a name="next-steps"></a>Дальнейшие шаги
- Сведения об [ограничениях службы](concepts-limits.md).
- Узнайте, как [создать сервер MariaDB на портале Azure](quickstart-create-mariadb-server-database-using-azure-portal.md).

<!--
- Learn how to [monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->
