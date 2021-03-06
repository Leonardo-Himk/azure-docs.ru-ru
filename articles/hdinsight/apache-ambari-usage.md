---
title: Использование Apache Ambari в Azure HDInsight
description: Обсуждение использования Apache Ambari в Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: 466c170985715be52a90d579c19ca23aefefe2e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77067400"
---
# <a name="apache-ambari-usage-in-azure-hdinsight"></a>Использование Apache Ambari в Azure HDInsight

HDInsight использует Apache Ambari для развертывания кластера и управления им. Агенты Ambari запускаются на каждом узле (головного узла, Рабочий узел, Zookeeper и граничном узле, если он существует). Сервер Ambari работает только в головного узла (hn0 или HN1). Одновременно должен выполняться только один экземпляр сервера Ambari. Это управляется контроллером отработки отказа HDInsight. Когда один из головных узлах не работает для перезагрузки или обслуживания, остальные головного узла становятся активными, и Ambari Server на второй головного узла будет запущен.

Все настройки кластера должны выполняться через [Пользовательский интерфейс Ambari](./hdinsight-hadoop-manage-ambari.md), любое локальное изменение будет перезаписано при перезапуске узла.

## <a name="failover-controller-services"></a>Службы отказоустойчивого контроллера

Контроллер отработки отказа HDInsight также отвечает за обновление IP-адреса узла головного узла, который указывает на текущий активный головной узел. Все агенты Ambari настроены для передачи состояния и пульса в узел головного узла. Контроллер отработки отказа — это набор служб, выполняющихся на каждом узле в кластере. Если они не выполняются, головного узланая отработка отказа может работать неправильно и при попытке доступа к серверу Ambari будет использоваться HTTP 502.

Чтобы проверить, какая головного узла активна, одним из способов является подключение SSH к одному из узлов в кластере, затем `ping headnodehost` выполняется и сравнивается IP-адрес с этими двумя головных узлах.

Если службы контроллера отработки отказа не работают, головного узланая отработка отказа может выполняться неправильно, что может привести к невозможности запуска Ambari Server. Чтобы проверить, выполняются ли службы отказоустойчивого контроллера, выполните:

```bash
ps -ef | grep failover
```

## <a name="logs"></a>Журналы

На активной головного узла можно проверить журналы сервера Ambari по адресу:

```
/var/log/ambari-server/ambari-server.log
/var/log/ambari-server/ambari-server-check-database.log
```

На любом узле кластера вы можете проверить журналы агента Ambari по адресу:

```bash
/var/log/ambari-agent/ambari-agent.log
```

## <a name="service-start-sequences"></a>Последовательности запусков службы

Это последовательность запуска службы во время загрузки:

1. Hdinsight — агент запускает службы отказоустойчивого контроллера.
1. Службы контроллера отработки отказа запускают агент Ambari на каждом узле и сервере Ambari на активном головного узла.

## <a name="ambari-database"></a>База данных Ambari

HDInsight создает SQL Azure базу данных внутри, чтобы служить базой данных для сервера Ambari. Уровень служб по умолчанию [— S0](../sql-database/sql-database-elastic-pool-scale.md).

Для любого кластера с числом рабочих узлов больше 16 при создании кластера S2 — это уровень служб базы данных.

## <a name="takeaway-points"></a>Точки мысль

Никогда не запускайте и останавливаются службы ambari-Server или ambari-Agent, если только вы не пытаетесь перезапустить службу для устранения проблемы. Чтобы принудительно выполнить отработку отказа, можно перезапустить активный головного узла.

Никогда не изменяйте файлы конфигурации на любом узле кластера вручную, позвольте Ambari пользовательскому интерфейсу выполнять задание.

## <a name="next-steps"></a>Дальнейшие шаги

* [Управление кластерами HDInsight с помощью веб-интерфейса Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [Управление кластерами HDInsight с помощью Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы от экспертов Azure через [службу поддержки сообщества Azure](https://azure.microsoft.com/support/community/).

* Подключение с [@AzureSupport](https://twitter.com/azuresupport) — официальная учетная запись Microsoft Azure для улучшения качества обслуживания клиентов. Подключение сообщества Azure к нужным ресурсам: ответы, поддержка и эксперты.

* Если вам нужна дополнительная помощь, можно отправить запрос в службу поддержки из [портал Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите пункт **Поддержка** в строке меню или откройте центр **справки и поддержки** . Для получения более подробных сведений см. статью [о создании запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется через один из [планов поддержки Azure](https://azure.microsoft.com/support/plans/).
