---
title: Резервное копирование и восстановление — база данных Azure для MySQL
description: Сведения об автоматическом резервном копировании и восстановлении сервера в службе "База данных Azure для MySQL".
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/27/2020
ms.openlocfilehash: 3a6162bb381f4e54114e3cabbf138f5b1c6aaae0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80373032"
---
# <a name="backup-and-restore-in-azure-database-for-mysql"></a>Резервное копирование и восстановление в службе "База данных Azure для MySQL"

В службе "База данных Azure для MySQL" для сервера автоматически создаются резервные копии, которые сохраняются в локально избыточном или геоизбыточном хранилище, настроенном пользователем. Резервные копии можно использовать для восстановления сервера до точки во времени. Резервное копирование и восстановление данных являются важной частью любой стратегии непрерывности бизнес-процессов. Таким образом данные защищаются от случайного повреждения или удаления.

## <a name="backups"></a>Резервные копии

База данных Azure для MySQL выполняет резервное копирование файлов данных и журнала транзакций. В зависимости от поддерживаемого максимального размера хранилища мы будем использовать полные и разностные резервные копии (максимум 4 ТБ серверов хранения) или резервные копии моментальных снимков (до 16 ТБ серверов хранилища). При помощи этих резервных копий вы можете восстановить сервер до любой точки во времени в пределах заданного срока хранения резервных копий. По умолчанию срок хранения резервных копий составляет 7 дней. При [необходимости можно настроить его](howto-restore-server-portal.md#set-backup-configuration) до 35 дней. Все резервные копии шифруются с помощью 256-битового шифрования AES.

Эти файлы резервных копий не предоставлены пользователю и не могут быть экспортированы. Эти резервные копии можно использовать только для операций восстановления в базе данных Azure для MySQL. [Mysqldump](concepts-migrate-dump-restore.md) можно использовать для копирования базы данных.

### <a name="backup-frequency"></a>Частота резервного копирования

Как правило, полное резервное копирование выполняется еженедельно, а разностное резервное копирование выполняется дважды в день для серверов с максимальным поддерживаемым объемом хранения в 4 ТБ. Резервное копирование моментальных снимков выполняется по крайней мере один раз в день для серверов, которые поддерживают до 16 ТБ хранилища. Создание резервных копий журналов транзакций в обоих случаях происходят каждые 5 минут. Первый моментальный снимок полной резервной копии планируется сразу же после создания сервера. Начальная полная архивация может занять больше времени на большом восстановленном сервере. Самая ранняя точка во времени, до которой можно восстановить новый сервер, — это время, в которое завершилось создание полной резервной копии. При мгновенном создании моментальных снимков серверы с поддержкой до 16 ТБ хранилища можно восстановить до момента создания.

### <a name="backup-redundancy-options"></a>Типы избыточности для резервного копирования

В службе "База данных Azure для MySQL" вы можете выбрать локально избыточное или геоизбыточное хранилище резервных копий в ценовой категории "Общего назначения" или "С оптимизацией для операций в памяти". Резервные копии в геоизбыточном хранилище не только хранятся в регионе, где размещен сервер, но и реплицируются в [связанный центр обработки данных](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Благодаря этому сервер лучше защищен и его можно восстановить в другом регионе в случае аварии. На уровне "Базовый" предоставляется только локально избыточное хранилище резервных копий.

> [!IMPORTANT]
> Хранилище резервных копий (локально избыточное или геоизбыточное) можно настроить только на этапе создания сервера. После подготовки сервера невозможно изменить тип избыточности для хранилища резервных копий.

### <a name="backup-storage-cost"></a>Стоимость хранилищ резервных копий

В службе "База данных Azure для MySQL" хранилище для резервных копий размером до 100 % объема подготовленного хранилища базы данных предоставляется бесплатно. Как правило, это подходит для срока хранения резервных копий, составляющего 7 дней. За дополнительный объем хранилища резервных копий взимается плата (за ГБ за месяц).

Например, если вы подготовили сервер размером 250 ГБ, вам будет предоставлено бесплатное хранилище резервных копий объемом 250 ГБ. За хранилище объемом более 250 ГБ будет взиматься плата.

## <a name="restore"></a>Восстановить

В базе данных Azure для MySQL при восстановлении создается новый сервер из резервных копий исходного сервера и восстанавливаются все базы данных, содержащиеся на сервере.

Есть два типа восстановления:

- **Восстановление до точки во времени** доступно с помощью параметра избыточность резервного копирования и создает новый сервер в том же регионе, что и исходный сервер, использующий сочетание полных резервных копий и архивов журналов транзакций.
- **Геовосстановление** доступно только в том случае, если на сервере настроено геоизбыточное хранилище, и оно позволяет восстановить сервер в другой регион, использующий самую последнюю резервную копию.

Предполагаемое время восстановления будет зависеть от нескольких факторов, включая размер базы данных, размер журнала транзакций, пропускную способность сети и общее количество баз данных, восстанавливаемых в том же регионе и в то же время. Обычно время восстановления составляет менее 12 часов.

> [!IMPORTANT]
> Удаленные серверы **нельзя** восстановить. Если вы удалите сервер, все связанные с ним базы данных также будут удалены без возможности восстановления. Чтобы защитить ресурсы сервера после развертывания от случайного удаления или внесения непредвиденных изменений, администраторы могут использовать [блокировки управления](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources).

### <a name="point-in-time-restore"></a>Восстановление на момент времени

Независимо от типа избыточности для резервного копирования, вы можете выполнить восстановление до любой точки во времени в пределах срока хранения резервной копии. Новый сервер создается в том же регионе Azure, где расположен исходный сервер, и с такими же конфигурацией ценовой категории, поколением вычислительных ресурсов, числом виртуальных ядер, размером хранилища, сроком хранения резервных копий и типом избыточности резервного копирования.

Восстановление до точки во времени подходит для большинства сценариев. Например, если пользователь случайно удалил данные, важные таблицы или базы данных либо если приложение в результате ошибки случайно перезаписало правильные данные неправильными.

Возможно, вам придется подождать, пока создастся резервная копия журналов транзакций, прежде чем выполнить восстановление до точки во времени в пределах последних пяти минут.

### <a name="geo-restore"></a>геовосстановлением;

Вы можете восстановить сервер в другом регионе Azure, где доступна служба, если вы настроили для сервера геоизбыточное хранилище. Серверы, поддерживающие до 4 ТБ хранилища, можно восстановить в геопарный регион или в любой регион, поддерживающий хранилище объемом до 16 ТБ. Для серверов, поддерживающих до 16 ТБ хранилища, георезервное копирование можно также восстановить в любом регионе, поддерживающем серверы размером 16 ТБ. Ознакомьтесь со списком поддерживаемых регионов для [ценовых категорий базы данных Azure для MySQL](concepts-pricing-tiers.md) .

Геовосстановление используется по умолчанию, когда сервер недоступен из-за аварии в регионе, в котором он размещен. Если из-за масштабной аварии в регионе приложение базы данных станет недоступным, сервер можно будет восстановить из геоизбыточных резервных копий на сервер в любом другом регионе. При геовосстановлении используется самая последняя резервная копия сервера. Между созданием резервной копии и ее репликацией в другом регионе может пройти некоторое время. Эта задержка может длиться до часа, поэтому в случае аварии возможна потеря данных за час или менее.

Во время геовосстановления можно изменить такие конфигурации сервера, как поколение вычислительных ресурсов, виртуальное ядро, срок хранения резервных копий и варианты избыточности для резервного копирования. Изменить ценовую категорию ("Базовый", "Общего назначения" или "С оптимизацией для операций в памяти") и объем хранилища во время геовосстановления нельзя.

### <a name="perform-post-restore-tasks"></a>Задачи после восстановления

После восстановления с помощью любого из механизмов восстановления необходимо выполнить следующие задачи, прежде чем данные и приложения станут доступными:

- Если новый сервер заменит исходный, перенаправьте клиенты и клиентские приложения на новый сервер.
- Чтобы пользователи могли подключаться, убедитесь, что установлены соответствующие правила виртуальной сети. Эти правила не копируются с исходного сервера.
- Убедитесь, что заданы соответствующие данные для входа и разрешений уровня базы данных.
- Настройте оповещения соответствующим образом.

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о непрерывности бизнес-процессов см. в  [этой статье](concepts-business-continuity.md).
- Для восстановления на момент времени с помощью портал Azure см. раздел [восстановление сервера на момент времени с помощью портал Azure](howto-restore-server-portal.md).
- Для восстановления на момент времени с помощью Azure CLI см. раздел [восстановление сервера до точки во времени с помощью интерфейса командной строки](howto-restore-server-cli.md).
