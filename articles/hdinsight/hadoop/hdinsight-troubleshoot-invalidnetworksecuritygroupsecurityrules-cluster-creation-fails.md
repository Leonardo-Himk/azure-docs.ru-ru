---
title: Ошибка InvalidNetworkSecurityGroupSecurityRules — Azure HDInsight
description: Сбой создания кластера с InvalidNetworkSecurityGroupSecurityRules ErrorCode
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 2cf4859d3bf4c34fff4cb076eec11bcd2d81e4ab
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780782"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Сценарий: InvalidNetworkSecurityGroupSecurityRules — сбой создания кластера в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при взаимодействии с кластерами Azure HDInsight.

## <a name="issue"></a>Проблема

Вы получаете код `InvalidNetworkSecurityGroupSecurityRules` ошибки с описанием, похожим на "правила безопасности в группе безопасности сети, настроенной с подсетью, не допускают обязательного входящего и исходящего подключения".

## <a name="cause"></a>Причина

Вероятно, возникла ошибка в правилах [группы безопасности входящих сетей](../../virtual-network/virtual-network-vnet-plan-design-arm.md) , настроенных для кластера.

## <a name="resolution"></a>Решение

Перейдите в портал Azure и найдите NSG, связанный с подсетью, в которой развертывается кластер. В разделе **правила безопасности для входящего трафика** убедитесь, что правила разрешают входящий доступ к порту 443 для IP-адресов, указанных [здесь](../control-network-traffic.md).

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы от экспертов Azure через [службу поддержки сообщества Azure](https://azure.microsoft.com/support/community/).

* Подключайтесь с помощью [@AzureSupport](https://twitter.com/azuresupport) официальной учетной записи Microsoft Azure для улучшения качества работы клиентов, подключив сообщество Azure к нужным ресурсам: ответы, поддержка и эксперты.

* Если вам нужна дополнительная помощь, можно отправить запрос в службу поддержки из [портал Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите пункт **Поддержка** в строке меню или откройте центр **справки и поддержки** . Дополнительные сведения см. [в](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)этой службе. Доступ к управлению подписками и поддержкой выставления счетов включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется через один из [планов поддержки Azure](https://azure.microsoft.com/support/plans/).
