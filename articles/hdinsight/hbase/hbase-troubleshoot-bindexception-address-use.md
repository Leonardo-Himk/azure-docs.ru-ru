---
title: Биндексцептион-Address уже используется в Azure HDInsight
description: Биндексцептион-Address уже используется в Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/16/2019
ms.openlocfilehash: 80f984643d6d8be88b381881c6fc1cb1cb5f1815
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75887348"
---
# <a name="scenario-bindexception---address-already-in-use-in-azure-hdinsight"></a>Сценарий: Биндексцептион-Address уже используется в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при взаимодействии с кластерами Azure HDInsight.

## <a name="issue"></a>Проблема

Не удается завершить операцию перезапуска на сервере региона Apache HBase. Из каталога `region-server.log` в `/var/log/hbase` каталоге рабочих узлов, в которых происходит сбой запуска сервера Region, может появиться сообщение об ошибке, подобное приведенному ниже.

```
Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
...

Caused by: java.net.BindException: Address already in use
...
```

## <a name="cause"></a>Причина:

Перезапуск серверов регионов Apache HBase во время высокой активности рабочей нагрузки. Ниже показано, что происходит в фоновом режиме, когда пользователь инициирует операцию перезапуска для сервера регионов HBase из пользовательского интерфейса Apache Ambari:

1. Агент Ambari отправляет запрос на завершение работы к региональному серверу.

1. Агент Ambari ожидает 30 секунд для корректного завершения работы сервера региона.

1. Если приложение продолжает подключаться к региональному серверу, его работа не будет завершена немедленно. Перед завершением работы должно истечь 30-секундное время ожидания.

1. Через 30 секунд агент Ambari отправляет команду force-kill (`kill -9`) на региональный сервер.

1. Из-за этого внезапного завершения работы, хотя процесс сервера региона завершается, порт, связанный с процессом, может не быть освобожден, что в конечном `AddressBindException`итоге приводит к.

## <a name="resolution"></a>Разрешение

Перед запуском перезагрузки сократите нагрузку на серверы регионов HBase. Кроме того рекомендуется сначала очистить все таблицы. Подробные сведения об очистке таблиц см. в статье [HDInsight HBase: How to improve the Apache HBase cluster restart time by flushing tables](https://web.archive.org/web/20190112153155/https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/) (HDInsight HBase: как уменьшить время перезапуска кластера Apache HBase с помощью очистки таблиц).

Кроме того, попробуйте вручную перезапустить серверы регионов на рабочих узлах с помощью следующих команд:

```bash
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"
```

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы от экспертов Azure через [службу поддержки сообщества Azure](https://azure.microsoft.com/support/community/).

* Подключение с [@AzureSupport](https://twitter.com/azuresupport) — официальная учетная запись Microsoft Azure для улучшения качества обслуживания клиентов. Подключение сообщества Azure к нужным ресурсам: ответы, поддержка и эксперты.

* Если вам нужна дополнительная помощь, можно отправить запрос в службу поддержки из [портал Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите пункт **Поддержка** в строке меню или откройте центр **справки и поддержки** . Для получения более подробных сведений см. статью [о создании запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется через один из [планов поддержки Azure](https://azure.microsoft.com/support/plans/).
