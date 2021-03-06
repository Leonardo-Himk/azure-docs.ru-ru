---
title: IP-адреса управления Azure HDInsight
description: Узнайте, какие IP-адреса необходимо разрешить из входящего трафика, чтобы правильно настроить группы безопасности сети и определяемые пользователем маршруты для виртуальных сетей с помощью Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/03/2020
ms.openlocfilehash: f1a539096ac1a154ca37bbe6703f820787f927fb
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2020
ms.locfileid: "82778266"
---
# <a name="hdinsight-management-ip-addresses"></a>IP-адреса управления HDInsight

> [!Important]
> В большинстве случаев теперь можно использовать функцию [тега службы](hdinsight-service-tags.md) для групп безопасности сети, а не добавлять IP-адреса вручную. Новые регионы будут добавлены только для тегов служб, а статические IP-адреса в конечном итоге будут считаться устаревшими.

Если вы используете группы безопасности сети (группы безопасности сети) или определяемые пользователем маршруты (определяемые пользователем маршруты) для управления входящим трафиком к кластеру HDInsight, необходимо убедиться, что кластер может взаимодействовать с критически важными службами работоспособности и управления Azure.  Некоторые IP-адреса для этих служб зависят от региона, и некоторые из них применяются ко всем регионам Azure. Если у вас нет пользовательской службы DNS, вам, возможно, нужно будет разрешить трафик из службы Azure DNS.

В следующих разделах рассматриваются конкретные IP-адреса, которые должны быть разрешены.

## <a name="azure-dns-service"></a>Служба Azure DNS

Если вы используете службу DNS, предоставляемую Azure, разрешите доступ из __168.63.129.16__ через порт 53. Дополнительные сведения см. в документе [разрешение имен для виртуальных машин и экземпляров ролей](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) . Если вы используете настраиваемый DNS, пропустите этот шаг.

## <a name="health-and-management-services-all-regions"></a>Службы работоспособности и управления: все регионы

Разрешите трафик со следующих IP-адресов для служб работоспособности и управления Azure HDInsight, которые применяются ко всем регионам Azure:

| Исходный IP-адрес | Назначение  | Направление |
| ---- | ----- | ----- |
| 168.61.49.99 | \*: 443 | Входящий трафик |
| 23.99.5.239 | \*: 443 | Входящий трафик |
| 168.61.48.131 | \*: 443 | Входящий трафик |
| 138.91.141.162 | \*: 443 | Входящий трафик |

## <a name="health-and-management-services-specific-regions"></a>Службы работоспособности и управления: конкретные регионы

Разрешите трафик с IP-адресов, перечисленных для служб работоспособности и управления Azure HDInsight в определенном регионе Azure, где находятся ваши ресурсы:

> [!IMPORTANT]  
> Если используемый регион Azure отсутствует в списке, используйте функцию [тега службы](hdinsight-service-tags.md) для групп безопасности сети.

| Country | Регион | Разрешенные исходные IP-адреса | Разрешенное назначение | Направление |
| ---- | ---- | ---- | ---- | ----- |
| Азия | Восточная Азия | 23.102.235.122</br>52.175.38.134 | \*: 443 | Входящий трафик |
| &nbsp; | Юго-Восточная Азия | 13.76.245.160</br>13.76.136.249 | \*: 443 | Входящий трафик |
| Австралия | Восточная Австралия | 104.210.84.115</br>13.75.152.195 | \*: 443 | Входящий трафик |
| &nbsp; | Юго-Восточная часть Австралии | 13.77.2.56</br>13.77.2.94 | \*: 443 | Входящий трафик |
| Бразилия | Южная Бразилия | 191.235.84.104</br>191.235.87.113 | \*: 443 | Входящий трафик |
| Canada | Восточная Канада | 52.229.127.96</br>52.229.123.172 | \*: 443 | Входящий трафик |
| &nbsp; | Центральная Канада | 52.228.37.66</br>52.228.45.222 |\*: 443 | Входящий трафик |
| Китай | Северный Китай | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | \*: 443 | Входящий трафик |
| &nbsp; | Восточный Китай | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | \*: 443 | Входящий трафик |
| &nbsp; | Северный Китай 2 | 40.73.37.141</br>40.73.38.172 | \*: 443 | Входящий трафик |
| &nbsp; | Восточный Китай 2 | 139.217.227.106</br>139.217.228.187 | \*: 443 | Входящий трафик |
| Европа | Северная Европа | 52.164.210.96</br>13.74.153.132 | \*: 443 | Входящий трафик |
| &nbsp; | Западная Европа| 52.166.243.90</br>52.174.36.244 | \*: 443 | Входящий трафик |
| Франция | Центральная Франция| 20.188.39.64</br>40.89.157.135 | \*: 443 | Входящий трафик |
| Германия | Центральная Германия | 51.4.146.68</br>51.4.146.80 | \*: 443 | Входящий трафик |
| &nbsp; | Северо-восточная Германия | 51.5.150.132</br>51.5.144.101 | \*: 443 | Входящий трафик |
| Индия | Центральная Индия | 52.172.153.209</br>52.172.152.49 | \*: 443 | Входящий трафик |
| &nbsp; | Южная Индия | 104.211.223.67<br/>104.211.216.210 | \*: 443 | Входящий трафик |
| Япония | Восточная Япония | 13.78.125.90</br>13.78.89.60 | \*: 443 | Входящий трафик |
| &nbsp; | Западная Япония | 40.74.125.69</br>138.91.29.150 | \*: 443 | Входящий трафик |
| Корея | Республика Корея, центральный регион | 52.231.39.142</br>52.231.36.209 | \*: 443 | Входящий трафик |
| &nbsp; | Республика Корея, южный регион | 52.231.203.16</br>52.231.205.214 | \*: 443 | Входящий трафик
| United Kingdom | западная часть Соединенного Королевства | 51.141.13.110</br>51.141.7.20 | \*: 443 | Входящий трафик |
| &nbsp; | южная часть Соединенного Королевства | 51.140.47.39</br>51.140.52.16 | \*: 443 | Входящий трафик |
| США | Центральная часть США | 13.89.171.122</br>13.89.171.124 | \*: 443 | Входящий трафик |
| &nbsp; | Восточная часть США | 13.82.225.233</br>40.71.175.99 | \*: 443 | Входящий трафик |
| &nbsp; | Центрально-северная часть США | 157.56.8.38</br>157.55.213.99 | \*: 443 | Входящий трафик |
| &nbsp; | центрально-западная часть США | 52.161.23.15</br>52.161.10.167 | \*: 443 | Входящий трафик |
| &nbsp; | западная часть США | 13.64.254.98</br>23.101.196.19 | \*: 443 | Входящий трафик |
| &nbsp; | западная часть США 2 | 52.175.211.210</br>52.175.222.222 | \*: 443 | Входящий трафик |
| &nbsp; | Северная часть ОАЭ; | 65.52.252.96</br>65.52.252.97 | \*: 443 | Входящий трафик |

Сведения об IP-адресах для Azure для государственных организаций см. в документе [Аналитика Azure для государственных организаций](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics).

Дополнительные сведения см. в разделе [Управление сетевым трафиком](./control-network-traffic.md).

Если вы используете определяемые пользователем маршруты (определяемые пользователем маршруты), укажите маршрут и разрешите исходящий трафик из виртуальной сети на указанные выше IP-адреса со следующим прыжком в значение "Интернет".

## <a name="next-steps"></a>Следующие шаги

* [Создание виртуальных сетей для кластеров Azure HDInsight](hdinsight-create-virtual-network.md)
* [Теги службы группы безопасности сети (NSG) для Azure HDInsight](hdinsight-service-tags.md)
