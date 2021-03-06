---
title: Схема аналитики трафика Azure | Документация Майкрософт
description: Сведения о схеме Аналитика трафика для анализа журналов потоков для групп безопасности сети Azure.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: vinigam
ms.openlocfilehash: ccfbb92c27e4508595f19c2ea6900730cde609b9
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "74666381"
---
# <a name="schema-and-data-aggregation-in-traffic-analytics"></a>Схема и агрегирование данных в Аналитика трафика

"Аналитика трафика" — это облачное решение, которое позволяет следить за действиями пользователя и приложения в облачных сетях. Это решение анализирует журналы потоков группы безопасности сети (NSG) Наблюдателя за сетями, чтобы предоставить сведения о потоке трафика в облаке Azure. Решение "Аналитика трафика" позволяет выполнять следующее:

- Визуализировать сетевую активность в подписках Azure и определять самые активные узлы.
- Выявлять угрозы безопасности в сети и защитить ее инфраструктуру на основе сведений об открытых портах, приложениях, пытающихся получить доступ к Интернету, а также виртуальных машинах, подключающихся к несанкционированным сетям.
- Понять шаблоны потока трафика в регионах Azure и Интернете, чтобы оптимизировать развертывание сети для лучшей производительности и эффективного использования емкости.
- Оперативно обнаружить неверные сетевые конфигурации, которые приводят к сбоям подключений в сети.
- Сведения об использовании сети в байтах, пакетах или потоках.

### <a name="data-aggregation"></a>Агрегация данных

1. Все журналы потоков в NSG между "FlowIntervalStartTime_t" и "FlowIntervalEndTime_t" фиксируются с интервалом в одну минуту в учетной записи хранения в виде больших двоичных объектов перед обработкой Аналитика трафика.
2. Интервал обработки по умолчанию Аналитика трафика составляет 60 минут. Это означает, что каждые 60 минут Аналитика трафика выберет большие двоичные объекты из хранилища для статистической обработки. Если выбран интервал обработки 10 минут, Аналитика трафика будет выбирать большие двоичные объекты из учетной записи хранения каждые 10 минут.
3. Потоки с одинаковым исходным IP-адресом, IP-адресом назначения, портом назначения, именем NSG, правилом NSG, направлением потока и протоколом транспортного уровня (TCP или UDP) (Примечание: исходный порт, исключенный для агрегирования), клуббед в один поток, Аналитика трафика
4. Эта отдельная запись дополнена (подробно описывается в разделе ниже) и принимается в Log Analytics Аналитика трафика. Этот процесс может занять до 1 часа.
5. FlowStartTime_t поле указывает первое вхождение такого агрегированного потока (то же четыре кортежа) в интервале обработки журнала потока между "FlowIntervalStartTime_t" и "FlowIntervalEndTime_t".
6. Для любого ресурса в TA потоки, указанные в пользовательском интерфейсе, являются общими потоками, видимыми в NSG, но в Log Analytics пользователь увидит только одну, сокращенную запись. Чтобы просмотреть все потоки, используйте поле blob_id, на которое можно ссылаться из хранилища. Общее число потоков для этой записи будет соответствовать отдельным потокам, отображаемым в большом двоичном объекте.

Приведенный ниже запрос позволяет взглянуть на все журналы потоков из локальной среды за последние 30 дней.
```
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FlowStartTime_t >= ago(30d) and FlowType_s == "ExternalPublic"
| project Subnet_s  
```
Чтобы просмотреть путь к большому двоичному объекту для потоков в приведенном выше запросе, используйте следующий запрос:

```
let TableWithBlobId =
(AzureNetworkAnalytics_CL
   | where SubType_s == "Topology" and ResourceType == "NetworkSecurityGroup" and DiscoveryRegion_s == Region_s and IsFlowEnabled_b
   | extend binTime = bin(TimeProcessed_t, 6h),
            nsgId = strcat(Subscription_g, "/", Name_s),
            saNameSplit = split(FlowLogStorageAccount_s, "/")
   | extend saName = iif(arraylength(saNameSplit) == 3, saNameSplit[2], '')
   | distinct nsgId, saName, binTime)
| join kind = rightouter (
   AzureNetworkAnalytics_CL
   | where SubType_s == "FlowLog"  
   | extend binTime = bin(FlowEndTime_t, 6h)
) on binTime, $left.nsgId == $right.NSGList_s  
| extend blobTime = format_datetime(todatetime(FlowIntervalStartTime_t), "yyyy MM dd hh")
| extend nsgComponents = split(toupper(NSGList_s), "/"), dateTimeComponents = split(blobTime, " ")
| extend BlobPath = strcat("https://", saName,
                        "@insights-logs-networksecuritygroupflowevent/resoureId=/SUBSCRIPTIONS/", nsgComponents[0],
                        "/RESOURCEGROUPS/", nsgComponents[1],
                        "/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/", nsgComponents[2],
                        "/y=", dateTimeComponents[0], "/m=", dateTimeComponents[1], "/d=", dateTimeComponents[2], "/h=", dateTimeComponents[3],
                        "/m=00/macAddress=", replace(@"-", "", MACAddress_s),
                        "/PT1H.json")
| project-away nsgId, saName, binTime, blobTime, nsgComponents, dateTimeComponents;

TableWithBlobId
| where SubType_s == "FlowLog" and FlowStartTime_t >= ago(30d) and FlowType_s == "ExternalPublic"
| project Subnet_s , BlobPath
```

Приведенный выше запрос строит URL-адрес для прямого доступа к большому двоичному объекту. Ниже приведен URL-адрес с местозаполнителями.

```
https://{saName}@insights-logs-networksecuritygroupflowevent/resoureId=/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroup}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json

```

### <a name="fields-used-in-traffic-analytics-schema"></a>Поля, используемые в схеме Аналитика трафика
  > [!IMPORTANT]
  > Схема Аналитика трафика была обновлена в 22 августа 2019. Новая схема предоставляет исходные и конечные IP-адреса отдельно, при этом необходимо упростить анализ запросов в поле FlowDirection. </br>
  > FASchemaVersion_s обновлен с 1 до 2. </br>
  > Нерекомендуемые поля: VMIP_s, Subscription_s, Region_s, NSGRules_s, Subnet_s, VM_s, NIC_s, PublicIPs_s, FlowCount_d </br>
  > Новые поля: SrcPublicIPs_s, DestPublicIPs_s, NSGRule_s </br>
  > Устаревшие поля будут доступны до 22 ноября 2019.

Аналитика трафика построены поверх Log Analytics, поэтому можно выполнять пользовательские запросы к данным, декорированным Аналитика трафика, и задавать оповещения для них.

Ниже перечислены поля схемы и их назначение.

| Поле | Формат | Комментарии |
|:---   |:---    |:---  |
| TableName | AzureNetworkAnalytics_CL | Таблица для Аналитика трафика данных
| SubType_s | фловлог | Подтип для журналов потоков. Использовать только "Фловлог", другие значения SubType_s предназначены для внутренней работы продукта |
| FASchemaVersion_s |   2   | Версия схемы. Не отражает версию журнала потока NSG |
| TimeProcessed_t   | Дата и время в формате UTC  | Время, когда Аналитика трафика обработала журналы необработанных потоков из учетной записи хранения |
| FlowIntervalStartTime_t | Дата и время в формате UTC |  Время начала интервала обработки журнала потока. Время, с которого измеряется интервал потока |
| FlowIntervalEndTime_t | Дата и время в формате UTC | Время окончания интервала обработки журнала потока |
| FlowStartTime_t | Дата и время в формате UTC |  Первое вхождение потока (который будет агрегирован) в интервале обработки журнала потока между "FlowIntervalStartTime_t" и "FlowIntervalEndTime_t". Этот поток получает агрегирование на основе логики агрегирования |
| FlowEndTime_t | Дата и время в формате UTC | Последнее вхождение потока (который будет агрегирован) в интервале обработки журнала потока между "FlowIntervalStartTime_t" и "FlowIntervalEndTime_t". В плане журнала потоков версии 2 это поле содержит время начала последнего потока с тем же четырьмя кортежами (помечаются как "B" в записи необработанного потока). |
| FlowType_s |  * Интравнет <br> * Между виртуальными сетями <br> * S2S <br> * P2S <br> * Азурепублик <br> * Екстерналпублик <br> * МалиЦиаусфлов <br> * Неизвестное частное <br> * Неизвестно | Определение в примечаниях под таблицей |
| SrcIP_s | Исходный IP-адрес | Будет пустым в случае потоков Азурепублик и Екстерналпублик |
| DestIP_s | Конечный IP-адрес | Будет пустым в случае потоков Азурепублик и Екстерналпублик |
| VMIP_s | IP-адрес виртуальной машины | Используется для потоков Азурепублик и Екстерналпублик |
| PublicIP_s | Общедоступные IP-адреса | Используется для потоков Азурепублик и Екстерналпублик |
| DestPort_d | Конечный порт | Порт, в котором входящий трафик |
| L4Protocol_s  | * T <br> * U  | Транспортный протокол. T = TCP <br> U = UDP |
| L7Protocol_s  | Имя протокола | Производный от целевого порта |
| FlowDirection_s | * I = входящее<br> * O = исходящий трафик | Направление потока в NSG и из него в соответствии с журналом потока |
| FlowStatus_s  | * A = разрешено правилом NSG <br> * D = отклонено правилом NSG  | Состояние потока Allowed/нблоккед by NSG в соответствии с журналом потоков |
| NSGList_s | \<SUBSCRIPTIONID>\/<RESOURCEGROUP_NAME>\/<NSG_NAME> | Группа безопасности сети (NSG), связанная с потоком |
| NSGRules_s | \<Значение индекса 0) >\| \<NSG_RULENAME>\| \<направление потока>\| \<состояние потока>\| \<FlowCount процесседбируле> |  Правило NSG, которое разрешает или запрещает этот поток |
| NSGRule_s | NSG_RULENAME |  Правило NSG, которое разрешает или запрещает этот поток |
| NSGRuleType_s | * Определено пользователем * по умолчанию |   Тип правила NSG, используемого потоком |
| MACAddress_s | MAC-адрес | MAC-адрес сетевого адаптера, на котором была захвачена последовательность |
| Subscription_s | В этом поле заполнена подписка на виртуальную сеть, сетевой интерфейс или виртуальную машину Azure. | Применимо только к типам потоков Фловтипе = S2S, P2S, Азурепублик, Екстерналпублик, МалиЦиаусфлов и Ункновнпривате (типы потоков, где только одна сторона — Azure) |
| Subscription1_s | Идентификатор подписки | Идентификатор подписки виртуальной сети, сетевого интерфейса или виртуальной машины, к которой принадлежит исходный IP-адрес в потоке |
| Subscription2_s | Идентификатор подписки | Идентификатор подписки виртуальной сети, сетевого интерфейса или виртуальной машины, к которой принадлежит целевой IP-адрес в потоке |
| Region_s | Регион Azure виртуальной сети, сетевого интерфейса или виртуальной машины, к которой принадлежит IP-адрес в потоке | Применимо только к типам потоков Фловтипе = S2S, P2S, Азурепублик, Екстерналпублик, МалиЦиаусфлов и Ункновнпривате (типы потоков, где только одна сторона — Azure) |
| Region1_s | Регион Azure | Регион Azure виртуальной сети, сетевого интерфейса или виртуальной машины, к которой принадлежит исходный IP-адрес в потоке |
| Region2_s | Регион Azure | Регион Azure виртуальной сети, к которой принадлежит целевой IP-адрес в потоке |
| NIC_s | \<resourcegroup_Name>\/ \<NetworkInterfaceName> |  Сетевая карта, связанная с виртуальной машиной при отправке или получении трафика |
| NIC1_s | <resourcegroup_Name>/\<NetworkInterfaceName> | Сетевая карта, связанная с исходным IP-адресом в потоке |
| NIC2_s | <resourcegroup_Name>/\<NetworkInterfaceName> | Сетевая карта, связанная с IP-адресом назначения в потоке |
| VM_s | <resourcegroup_Name>\/ \<NetworkInterfaceName> | Виртуальная машина, связанная с сетевым интерфейсом NIC_s |
| VM1_s | <resourcegroup_Name>/\<VirtualMachineName> | Виртуальная машина, связанная с исходным IP-адресом в потоке |
| VM2_s | <resourcegroup_Name>/\<VirtualMachineName> | Виртуальная машина, связанная с целевым IP-адресом в потоке |
| Subnet_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | Подсеть, связанная с NIC_s |
| Subnet1_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | Подсеть, связанная с исходным IP-адресом в потоке |
| Subnet2_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName>    | Подсеть, связанная с IP-адресом назначения в потоке |
| ApplicationGateway1_s | \<SubscriptionID>/\<ResourceGroupName>/\<аппликатионгатевайнаме> | Шлюз приложений, связанный с исходным IP-адресом в потоке |
| ApplicationGateway2_s | \<SubscriptionID>/\<ResourceGroupName>/\<аппликатионгатевайнаме> | Шлюз приложений, связанный с конечным IP-адресом в потоке |
| LoadBalancer1_s | \<SubscriptionID>/\<ResourceGroupName>/\<LoadBalancerName> | Подсистема балансировки нагрузки, связанная с исходным IP-адресом в потоке |
| LoadBalancer2_s | \<SubscriptionID>/\<ResourceGroupName>/\<LoadBalancerName> | Подсистема балансировки нагрузки, связанная с IP-адресом назначения в потоке |
| LocalNetworkGateway1_s | \<SubscriptionID>/\<ResourceGroupName>/\<локалнетворкгатевайнаме> | Шлюз локальной сети, связанный с исходным IP-адресом в потоке |
| LocalNetworkGateway2_s | \<SubscriptionID>/\<ResourceGroupName>/\<локалнетворкгатевайнаме> | Шлюз локальной сети, связанный с IP-адресом назначения в потоке |
| ConnectionType_s | Возможные значения: VNetPeering, VpnGateway и ExpressRoute. |    Тип соединения |
| ConnectionName_s | \<SubscriptionID>/\<ResourceGroupName>/\<connectionName> | Имя подключения. Для фловтипе P2S это будет отформатировано как <gateway name>_<VPN Client IP> |
| ConnectingVNets_s | Список имен виртуальных сетей, разделенных пробелами | В случае топологии с центральным и периферийным концентратором виртуальные сети будут заполнены. |
| Country_s | Двухбуквенный код страны (ISO 3166-1 Alpha-2) | Заполнено для типа потока Екстерналпублик. Все IP-адреса в поле PublicIPs_s будут иметь одинаковый код страны |
| AzureRegion_s | Расположения регионов Azure | Заполнено для типа потока Азурепублик. Все IP-адреса в поле PublicIPs_s будут предоставлять общий доступ к региону Azure |
| AllowedInFlows_d | | Число разрешенных входящих потоков. Представляет число потоков, которые совместно использовали один и тот же 4-элементный кортеж, входящий в сетевой интерфейс, в котором была захвачена последовательность |
| DeniedInFlows_d |  | Число запрещенных входящих потоков. (Входящий трафик на сетевой интерфейс, в котором была захвачена последовательность) |
| AllowedOutFlows_d | | Число разрешенных исходящих потоков (исходящих от сетевого интерфейса, на котором была записана последовательность) |
| DeniedOutFlows_d  | | Число отклоненных исходящих потоков (исходящих от сетевого интерфейса, на котором была записана последовательность) |
| FlowCount_d | Не рекомендуется. Всего потоков, соответствующих одному кортежу с четырьмя. В случае типов потока Екстерналпублик и Азурепублик число будет включать потоки из различных адресов PublicIP.
| InboundPackets_d | Пакеты, полученные как захваченные на сетевом интерфейсе, где было применено правило NSG | Заполняется только для схемы журнала потока версии 2 (NSG) |
| OutboundPackets_d  | Пакеты, отправленные как захваченные на сетевом интерфейсе, где было применено правило NSG | Заполняется только для схемы журнала потока версии 2 (NSG) |
| InboundBytes_d |  Байт получено как записанное на сетевом интерфейсе, где было применено правило NSG | Заполняется только для схемы журнала потока версии 2 (NSG) |
| OutboundBytes_d | Байт отправлено как записанное на сетевом интерфейсе, где было применено правило NSG | Заполняется только для схемы журнала потока версии 2 (NSG) |
| CompletedFlows_d  |  | Это значение заполняется ненулевым значением только для схемы журнала потоков NSG версии 2 |
| PublicIPs_s | <PUBLIC_IP>\| \<FLOW_STARTED_COUNT>\| \<FLOW_ENDED_COUNT>\| \<OUTBOUND_PACKETS>\| \<INBOUND_PACKETS>\| \<OUTBOUND_BYTES>\| \<INBOUND_BYTES> | Записи, разделенные панелями |
| SrcPublicIPs_s | <SOURCE_PUBLIC_IP>\| \<FLOW_STARTED_COUNT>\| \<FLOW_ENDED_COUNT>\| \<OUTBOUND_PACKETS>\| \<INBOUND_PACKETS>\| \<OUTBOUND_BYTES>\| \<INBOUND_BYTES> | Записи, разделенные панелями |
| DestPublicIPs_s | <DESTINATION_PUBLIC_IP>\| \<FLOW_STARTED_COUNT>\| \<FLOW_ENDED_COUNT>\| \<OUTBOUND_PACKETS>\| \<INBOUND_PACKETS>\| \<OUTBOUND_BYTES>\| \<INBOUND_BYTES> | Записи, разделенные панелями |

### <a name="notes"></a>Примечания

1. В случае потоков Азурепублик и Екстерналпублик клиентский IP-адрес виртуальной машины Azure заполняется в VMIP_s поле, а общедоступные IP-адреса заполняются в поле PublicIPs_s. Для этих двух типов потоков следует использовать VMIP_s и PublicIPs_s вместо полей SrcIP_s и DestIP_s. Для адресов Азурепублик и ЕкстерналпублиЦип мы рассмотрим дальнейшую статистическую обработку, чтобы количество записей, принимаемых в рабочую область log Analytics, было минимальным. (Это поле скоро станет нерекомендуемым, и мы будем использовать SrcIP_ и DestIP_s в зависимости от того, была ли виртуальная машина Azure источником или назначением в потоке).
1. Сведения о типах потоков. в зависимости от IP-адресов, вовлеченных в последовательность, мы разбивают потоки на следующие типы потоков:
1. Интравнет — оба IP-адреса в потоке находятся в одной виртуальной сети Azure.
1. Между виртуальными сетями — IP-адреса в потоке находятся в двух разных виртуальных сетях Azure.
1. S2S — (сайт-сайт) один из IP-адресов принадлежит виртуальной сети Azure, а другой IP-адрес принадлежит к сети клиента (сайту), подключенной к виртуальной сети Azure через VPN-шлюз или Express Route.
1. P2S — (наведите указатель на сайт) один из IP-адресов принадлежит виртуальной сети Azure, а другой IP-адрес принадлежит к сети клиента (сайту), подключенной к виртуальной сети Azure через VPN-шлюз.
1. Азурепублик — один из IP-адресов принадлежит к виртуальной сети Azure, а другой — к внутренним общедоступным IP-адресам Azure, принадлежащим корпорации Майкрософт. Общедоступные IP-адреса, принадлежащие заказчику, не будут входить в этот тип потока. Например, любая виртуальная машина клиента, отправляющая трафик в службу Azure (конечная точка хранилища), будет классифицироваться по этому типу потока.
1. Екстерналпублик — один из IP-адресов принадлежит виртуальной сети Azure, а другой — общедоступный IP-адрес, не входящий в Azure, не считается вредоносным в каналах ASC, которые Аналитика трафика используют для интервала обработки между "FlowIntervalStartTime_t" и "FlowIntervalEndTime_t".
1. МалиЦиаусфлов — один из IP-адресов, относящихся к виртуальной сети Azure, а другой — общедоступный IP-адрес, не входящий в состав Azure, и сообщается как вредоносное в каналах ASC, которые Аналитика трафика используют для интервала обработки между "FlowIntervalStartTime_t" и "FlowIntervalEndTime_t".
1. Ункновнпривате — один из IP-адресов принадлежит виртуальной сети Azure, а другой IP-адрес принадлежит к диапазону частных IP-адресов, как определено в RFC 1918 и не может быть сопоставлен Аналитика трафика с сайтом, принадлежащим клиенту, или виртуальной сетью Azure.
1. Неизвестно. не удается соотнести ни один из IP-адресов в потоках с топологией клиента в Azure, а также локальным (сайтом).
1. К \_именам полей добавляются s или \_d. Они не обозначают источник и назначение, но указывают типы данных String и Decimal соответственно.

### <a name="next-steps"></a>Дальнейшие действия
Чтобы получить ответы на часто задаваемые вопросы, см. раздел [вопросы и ответы по аналитике трафика](traffic-analytics-faq.md) , чтобы просмотреть сведения о функциональных возможностях. [Traffic analytics documentation](traffic-analytics.md)
