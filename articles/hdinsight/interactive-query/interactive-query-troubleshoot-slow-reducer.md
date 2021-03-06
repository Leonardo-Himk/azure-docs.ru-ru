---
title: Снижение производительности в Azure HDInsight
description: Уменьшение скорости в Azure HDInsight от возможного смещения данных
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 8a9c7ed9f6b5b8ec89bfca6dd59034b11f05f9a3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75895167"
---
# <a name="scenario-reducer-is-slow-in-azure-hdinsight"></a>Сценарий: снижение производительности в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при использовании интерактивных компонентов запросов в кластерах Azure HDInsight.

## <a name="issue"></a>Проблема

При выполнении запроса, такого как `insert into table1 partition(a,b) select a,b,c from table2` план запроса, запускается модулей сжатия, но данные из каждой секции отправляются одному элементу reduce. Это приводит к тому, что запрос будет работать так же, как и в случае с более крупным уменьшением секции.

## <a name="cause"></a>Причина:

Откройте [Beeline](../hadoop/apache-hadoop-use-hive-beeline.md) и проверьте значение SET `hive.optimize.sort.dynamic.partition`.

Значение этой переменной должно быть равно true или false в зависимости от характера данных.

Если секции во входной таблице меньше (скажем, меньше 10) и так и равны числу выходных секций, а переменной присвоено значение `true`, это приводит к тому, что данные будут глобально отсортированы и записаны с использованием одного уменьшения на секцию. Даже если число доступных модулей сжатия больше, модулей сжатия может быть задержкой за пределами данных, и не удается достичь максимального параллелизма. Если изменить на `false`, то более чем один из них может обрабатывать одну секцию, и будет записано несколько меньших файлов, что приведет к более быстрой вставке. Это может повлиять на последующие запросы из-за наличия файлов меньшего размера.

Значение `true` имеет смысл, если число секций больше, а данные не наклонены. В таких случаях результат фазы Map будет записан таким образом, что каждая секция будет обрабатываться одним средством уменьшения, что приведет к повышению производительности последующих запросов.

## <a name="resolution"></a>Разрешение

1. Попробуйте выполнить повторное секционирование данных, чтобы нормализовать их в несколько секций.

1. Если #1 невозможно, установите для параметра config значение false в сеансе Beeline и повторите запрос. `set hive.optimize.sort.dynamic.partition=false`. Установка значения false на уровне кластера не рекомендуется. Значение `true` является оптимальным и устанавливает параметр по мере необходимости на основе природы данных и запросов.

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы от экспертов Azure через [службу поддержки сообщества Azure](https://azure.microsoft.com/support/community/).

* Подключайтесь с помощью [@AzureSupport](https://twitter.com/azuresupport) официальной учетной записи Microsoft Azure для улучшения качества работы клиентов, подключив сообщество Azure к нужным ресурсам: ответы, поддержка и эксперты.

* Если вам нужна дополнительная помощь, можно отправить запрос в службу поддержки из [портал Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите пункт **Поддержка** в строке меню или откройте центр **справки и поддержки** . Дополнительные сведения см. [в](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)этой службе. Доступ к управлению подписками и поддержкой выставления счетов включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется через один из [планов поддержки Azure](https://azure.microsoft.com/support/plans/).
