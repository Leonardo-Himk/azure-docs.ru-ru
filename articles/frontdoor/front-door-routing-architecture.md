---
title: Передняя дверца Azure — Архитектура маршрутизации | Документация Майкрософт
description: В этой статье вы получите глобальное представление об архитектуре Front Door.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: a088e52f742f96a13ba61969c2d7a6697c96b145
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80879298"
---
# <a name="routing-architecture-overview"></a>Общие сведения об архитектуре решений для маршрутизации

Передняя дверца Azure, получающая клиентские запросы, либо отвечает на них (если кэширование включено), либо перенаправляет их в соответствующую серверную часть приложения (как обратный прокси-сервер).

</br>Существуют возможности оптимизировать трафик при маршрутизации к Azure Front Door, а также при маршрутизации к серверным системам.

## <a name="selecting-the-front-door-environment-for-traffic-routing-anycast"></a><a name = "anycast"></a>Выбор среды Front Door для маршрутизации трафика (произвольная передача)

Для маршрутизации к средам Azure Front Door используется [произвольная передача](https://en.wikipedia.org/wiki/Anycast) как для трафика DNS (служба доменных имен), так и для трафика HTTP (протокол HTTP), поэтому трафик пользователя перейдет в ближайшую среду с точки зрения топологии сети (наименьшее количество переходов). Эта архитектура, как правило, обеспечивает лучшее время кругового пути для пользователей (максимизируя преимущества разделенного протокола TCP). Front Door упорядочивает среды в первичные и резервные "кольца".  Во внешнем кольце находятся среды, которые ближе к пользователям, что обеспечивает низкую задержку.  Во внутреннем кольце находятся среды, которые могут выполнить отработку отказа для среды внешнего кольца в случае возникновения проблемы. Внешнее кольцо является предпочтительной целевой средой для всего трафика, но внутреннее кольцо необходимо для обработки переполнения трафика из внешнего кольца. Что касается виртуальных IP-адресов (адресов виртуального протокола IP), каждому узлу внешнего интерфейса или домену, обслуживаемому Front Door, назначается основной виртуальный IP-адрес, который объявлен средами как внутреннего, так и внешнего кольца, а также резервный виртуальный IP-адрес, который объявлен средами во внутреннем кольце. 

</br>Эта общая стратегия гарантирует, что запросы пользователей всегда достигают ближайшей среды Front Door, и даже если предпочтительная среда Front Door неработоспособна, тогда трафик автоматически переместится в следующую ближайшую среду.

## <a name="connecting-to-front-door-environment-split-tcp"></a><a name = "splittcp"></a>Подключение к среде Front Door (разделенный протокол TCP)

[Разделенный протокол TCP](https://en.wikipedia.org/wiki/Performance-enhancing_proxy) представляет собой метод сокращения задержек и проблем с протоколом TCP путем разделения подключения, которое иначе привело бы к большому времени кругового пути, на более мелкие части.  Одно TCP-подключение с большим временем кругового пути (RTT) к серверной части приложения разбивается на два подключения за счет размещения сред Front Door ближе к пользователям и завершения TCP-подключения в этой среде. Короткое соединение между конечным пользователем и средой передней дверцы означает, что соединение устанавливается на три коротких обращения, а не в три длительных круговых пути, что сокращает задержку.  Длительное подключение между средой Front Door и серверной частью может быть предварительно установлено и повторно использовано во многих вызовах пользователя, сохраняя время TCP-подключения.  Эффект умножается при установлении подключения по протоколу SSL или TLS, так как для обеспечения безопасного подключения выполняется больше круговых путей.

## <a name="processing-request-to-match-a-routing-rule"></a>Обработка запроса для сопоставления с правилом маршрутизации
После установления соединения и выполнения подтверждения TLS, когда запрос завершается в среде с передней дверцей, первым шагом является сопоставление правила маршрутизации. Это сопоставление, по сути, определяет, какому правилу маршрутизации (среди всех конфигураций в Front Door) соответствует поступивший запрос. Ознакомьтесь с тем, как Front Door выполняет [сопоставление маршрутов](front-door-route-matching.md).

## <a name="identifying-available-backends-in-the-backend-pool-for-the-routing-rule"></a>Определение доступных внутренних серверов во внутреннем пуле для правила маршрутизации
Если Front Door соответствует правилу маршрутизации на основе входящего запроса, а также если нет кэширования, то следующим шагом будет выведение состояния пробы работоспособности для внутреннего пула, связанного с соответствующим маршрутом. Узнайте о том, как Front Door контролирует работоспособность серверной части с помощью [проб работоспособности](front-door-health-probes.md).

## <a name="forwarding-the-request-to-your-application-backend"></a>Перенаправление запроса в серверную часть приложения
Наконец, при условии, что кэширование не настроено, запрос пользователя пересылается в "лучшую" серверную часть на основе конфигурации [метода маршрутизации Front Door](front-door-routing-methods.md).

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [создании Front Door](quickstart-create-front-door.md).
