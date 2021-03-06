---
title: Как работает диспетчер трафика | Документация Майкрософт
description: Из этой статьи вы узнаете, как маршрутизация в диспетчере трафика помогает обеспечить высокую производительность и доступность ваших веб-приложений.
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2019
ms.author: rohink
ms.openlocfilehash: 4863ffd383cfcd46bad462156e26293d145fd418
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80294867"
---
# <a name="how-traffic-manager-works"></a>Как работает диспетчер трафика

Диспетчер трафика Azure позволяет управлять распределением трафика между конечными точками приложения. Конечная точка — это любая служба, размещенная в среде Azure или за ее пределами, с доступом в Интернет.

Диспетчер трафика предоставляет два основных преимущества.

- Распределение трафика в соответствии с одним из нескольких [методов маршрутизации трафика](traffic-manager-routing-methods.md)
- [Постоянный мониторинг работоспособности конечной точки](traffic-manager-monitoring.md) и автоматический переход на другой ресурс при сбое конечных точек.

Когда клиент пытается подключиться к службе, он должен сначала разрешить IP-адрес этой службы по ее DNS-имени. Затем клиент подключается к этому IP-адресу для доступа к службе.

**Самое главное — понимать, как диспетчер трафика работает на уровне DNS.**  Диспетчер трафика с помощью службы DNS направляет клиентов к разным конечным точкам службы, используя правила маршрутизации трафика. Клиент **напрямую** подключается к выбранной конечной точке. Диспетчер трафика не выполняет функции прокси-сервера или шлюза. Диспетчер трафика не может отслеживать трафик, проходящий между клиентом и службой.

## <a name="traffic-manager-example"></a>Пример диспетчера трафика

Корпорация Contoso разработала новый партнерский портал. URL-адрес этого портала — `https://partners.contoso.com/login.aspx`. Приложение размещается в трех регионах Azure. Чтобы повысить доступность и общую производительность, компания использует диспетчер трафика, который направляет запросы клиентов в ближайшую доступную конечную точку.

Чтобы добиться такой конфигурации, сделайте следующее:

1. Развертывается три экземпляра службы. Этим развертываниям присваиваются DNS-имена contoso-us.cloudapp.net, contoso-eu.cloudapp.net и contoso-asia.cloudapp.net.
1. Создайте профиль диспетчера трафика с именем contoso.trafficmanager.net и настройте в этом профиле маршрутизацию трафика по производительности между тремя конечными точками.
1. Для личного домена (partners.contoso.com) настройте запись CNAME в DNS, которая указывает на contoso.trafficmanager.net.

![Конфигурация DNS диспетчера трафика][1]

> [!NOTE]
> При использовании личного домена с диспетчером трафика Azure необходимо использовать запись CNAME для связи личного доменного имени с доменным именем диспетчера трафика. Стандарты DNS не позволяют создавать записи CNAME для корневой вершины домена. Поэтому вы не сможете создать запись CNAME для contoso.com (такой адрес иногда называют "голым" доменом). Запись CNAME можно создать только для поддомена contoso.com (например, www.contoso.com). Чтобы обойти это ограничение, рекомендуется разместить DNS-домен на [Azure DNS](../dns/dns-overview.md) и использовать [записи псевдонимов](../dns/tutorial-alias-tm.md) для указания профиля диспетчера трафика. Кроме того, можете использовать простое перенаправление HTTP для перенаправления запросов к "contoso.com" на другой домен, например "www.contoso.com".

### <a name="how-clients-connect-using-traffic-manager"></a>Как клиенты могут подключаться с помощью диспетчера трафика

Когда пользователь в рамках описанной конфигурации запрашивает страницу `https://partners.contoso.com/login.aspx`, его клиент выполняет следующие действия, чтобы разрешить имя DNS и установить подключение:

![Установка подключения с помощью диспетчера трафика][2]

1. Клиент отправляет запрос DNS рекурсивной службе DNS для разрешения имени partners.contoso.com. (Рекурсивная (локальная) служба DNS сама не размещает информацию о доменах. Она просто освобождает клиента от необходимости взаимодействия через Интернет с полномочными службами DNS для разрешения DNS-имени.)
2. Чтобы разрешить имя DNS, рекурсивная служба DNS находит серверы доменных имен для домена contoso.com. Затем она обращается к этим серверам доменных имен и запрашивает запись DNS для имени partners.contoso.com. DNS-серверы, связанные с доменом contoso.com, возвращают запись CNAME, которая указывает на contoso.trafficmanager.net.
3. Рекурсивная служба DNS находит серверы доменных имен для домена trafficmanager.net, которые входят в службу диспетчера трафика Azure. Затем служба DNS отправляет этим серверам запрос на запись DNS для имени contoso.trafficmanager.net.
4. Серверы имен диспетчера трафика получают запрос. Они выбирают конечную точку, учитывая следующее:

    - состояние каждой конечной точки (отключенные конечные точки не возвращаются);
    - работоспособность каждой конечной точки, определенная с помощью средства проверки работоспособности диспетчера трафика. Дополнительные сведения см. в статье [Мониторинг и отработка отказов конечной точки диспетчера трафика](traffic-manager-monitoring.md).
    - выбранный метод маршрутизации трафика. Дополнительные сведения см. в разделе [методы маршрутизации диспетчера трафика](traffic-manager-routing-methods.md).

5. Возвращается запись DNS CNAME с именем выбранной конечной точки. В нашем примере это — contoso-us.cloudapp.net.
6. Теперь рекурсивная служба DNS находит серверы доменных имен для домена cloudapp.net. Она обращается к этим серверам доменных имен и запрашивает запись DNS для contoso-us.cloudapp.net. Возвращается запись DNS A, которая содержит IP-адрес конечной точки службы, размещенной в США.
7. Рекурсивная служба DNS объединяет полученные результаты и возвращает клиенту один ответ DNS.
8. Клиент получает информацию DNS и подключается к указанному IP-адресу. Клиент подключается к конечной точке службы приложения напрямую, а не через диспетчер трафика. Так как это конечная точка HTTPS, клиент выполняет необходимое подтверждение SSL/TLS и отправляет запрос HTTP GET на получение страницы /login.aspx.

Рекурсивная служба DNS кэширует все полученные ответы DNS. Сопоставитель DNS на клиентском устройстве также кэширует результат. Это позволяет быстрее отвечать на последующие запросы DNS, используя данные из кэша, а не результаты запросов к другим серверам доменных имен. Длительность хранения записей DNS в кэше определяется свойством time-to-live (срок жизни, TTL). Чем меньше это значение, тем быстрее запись в кэше будет считаться истекшей, следовательно, сервера доменных имен, используемые диспетчером трафика, будут получать больше запросов. Более высокие значения приведут к тому, что при сбое конечной точки потребуется больше времени, чтобы переключить трафик на другие конечные точки. Диспетчер трафика позволяет настраивать срок жизни, используемый в ответах службы доменных имен (DNS) диспетчера трафика, в диапазоне от 0 до 2 147 483 647 секунд (максимальный диапазон в соответствии с [RFC-1035](https://www.ietf.org/rfc/rfc1035.txt)), позволяя вам выбрать значение, которое лучше всего соответствует требованиям приложения.

## <a name="faqs"></a>Частые вопросы

* [Какой IP-адрес использует диспетчер трафика?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-ip-address-does-traffic-manager-use)

* [Какого типа трафик можно маршрутизировать с помощью диспетчера трафика?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-types-of-traffic-can-be-routed-using-traffic-manager)

* [Поддерживает ли диспетчер трафика "прикрепленные" сеансы?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-manager-support-sticky-sessions)

* [Почему при использовании диспетчера трафика отображается HTTP-ошибка?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#why-am-i-seeing-an-http-error-when-using-traffic-manager)

* [Как сказывается на производительности использование диспетчера трафика?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-is-the-performance-impact-of-using-traffic-manager)

* [Какие протоколы приложения пригодны для использования с диспетчером трафика?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-application-protocols-can-i-use-with-traffic-manager)

* [Можно ли использовать диспетчер трафика с именем домена "naked"?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-traffic-manager-with-a-naked-domain-name)

* [Учитывает ли диспетчер трафика адрес подсети клиента при обработке запросов DNS?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries)

* [Что такое срок жизни DNS и как он влияет на пользователей?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-is-dns-ttl-and-how-does-it-impact-my-users)

* [Каковы минимальный и максимальный сроки жизни, которые можно задать для ответов диспетчера трафика?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses)

* [Что подразумевается под объемом запросов, поступающих в мой профиль?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-understand-the-volume-of-queries-coming-to-my-profile)

## <a name="next-steps"></a>Дальнейшие шаги

См. дополнительные сведения в статье [Мониторинг и отработка отказов конечной точки диспетчера трафика](traffic-manager-monitoring.md).

См. дополнительные сведения в статье [Методы маршрутизации трафика диспетчером трафика](traffic-manager-routing-methods.md).

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

