---
title: Поддержка протокола для заголовков HTTP в передней дверце Azure | Документация Майкрософт
description: В этой статье описываются протоколы HTTP Header, поддерживаемые интерфейсом front двери.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: bb1de5d51afd01cf0aa519f12aa3665bee804efd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79471682"
---
# <a name="protocol-support-for-http-headers-in-azure-front-door"></a>Поддержка протокола для заголовков HTTP в передней дверце Azure
В этой статье описывается протокол, который поддерживает передняя дверь с частями пути вызова (см. изображение). В следующих разделах содержатся дополнительные сведения о заголовках HTTP, поддерживаемых передней дверцей.

![Протокол заголовков HTTP для передней дверцы Azure][1]

>[!IMPORTANT]
>Передняя дверца не удостоверяет все заголовки HTTP, которые здесь не описаны.

## <a name="client-to-front-door"></a>Клиент — Front Door
Передняя дверца принимает большинство заголовков из входящего запроса, не изменяя их. Некоторые зарезервированные заголовки удаляются из входящего запроса, если он отправлен, включая заголовки с префиксом X-демон-*.

## <a name="front-door-to-backend"></a>Front Door — серверная система

Передняя дверца включает заголовки входящего запроса, если они не удалены из-за ограничений. Передняя дверца также добавляет следующие заголовки:

| Заголовок  | Пример и описание |
| ------------- | ------------- |
| Via |  Via: 1,1 Azure </br> Передняя дверь добавляет версию HTTP клиента, а затем *Azure* в качестве значения для заголовка Via. Этот заголовок указывает версию HTTP клиента, и эта Передняя дверца является промежуточным получателем для запроса между клиентом и серверной частью.  |
| X-Azure-ClientIP | X-Azure-ClientIP: 127.0.0.1 </br> Представляет IP-адрес клиента, связанный с обрабатываемым запросом. Например, запрос, поступающий от прокси-сервера, может добавить заголовок X-Forwardd-for, чтобы указать IP-адрес исходного вызывающего объекта. |
| X-Azure-Соккетип |  X-Azure-Соккетип: 127.0.0.1 </br> Представляет IP-адрес сокета, связанный с подключением TCP, из которого был получен текущий запрос. IP-адрес клиента запроса может не совпадать с IP-адресом сокета, так как он может быть произвольно перезаписан пользователем.|
| X — Azure-ref |  X-Azure-ref: 0zxV + XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz </br> Уникальная строка ссылки, идентифицирующая запрос, обслуживаемый передней дверцей. Он используется для поиска в журналах доступа и критически важных для устранения неполадок.|
| X-Azure-Рекуестчаин |  X-Azure-Рекуестчаин: прыжков = 1 </br> Заголовок, который используется интерфейсной дверцей для обнаружения циклов запросов, и пользователи не должны зависеть от него. |
| X-Forwarded-For | X-переслано — для: 127.0.0.1 </br> Поле заголовка HTTP X-Forwardd-for (КСФФ) часто определяет исходный IP-адрес клиента, подключающегося к веб-серверу через прокси-сервер HTTP или подсистему балансировки нагрузки. Если имеется существующий заголовок КСФФ, то передняя дверь добавляет к нему IP-адрес сокета клиента или добавляет заголовок КСФФ с IP-адресом сокета клиента. |
| X-Forwarded-Host | X-Forwarded — узел: contoso.azurefd.net </br> Поле заголовка HTTP X-Forwardd-Host НТТР является общим методом, используемым для обнаружения исходного узла, запрошенного клиентом в заголовке HTTP-запроса узла. Это связано с тем, что имя узла из передней дверцы может отличаться для внутреннего сервера, обрабатывающего запрос. |
| X-Forwarded-Proto | X-Forwarded-определяемое: http </br> Поле заголовка HTTP X-Forwarded-обозначать, как правило, используется для обнаружения исходного протокола HTTP-запроса, так как передняя дверца на основе конфигурации может взаимодействовать с серверной частью по протоколу HTTPS. Это справедливо, даже если запрос к обратным прокси-серверу имеет значение HTTP. |
| X-демон-HealthProbe | Поле заголовка HTTP X-демо-HealthProbe используется для определения пробы работоспособности из передней дверцы. Если для этого заголовка задано значение 1, то запрос является зондом работоспособности. Вы можете использовать, если вы хотите ограничивать доступ из определенной передней дверцы с помощью поля заголовка X-Forwardd-Host. |

## <a name="front-door-to-client"></a>Front Door — клиент

Все заголовки, отправленные в переднюю дверцу из серверной части, также передаются клиенту. Ниже приведены заголовки, отправляемые с передней дверцы клиентам.

| Заголовок  | Пример |
| ------------- | ------------- |
| X — Azure-ref |  *X-Azure-ref: 0zxV + XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Это уникальная эталонная строка, которая идентифицирует запрос, обслуженный Front Door. Это важно для устранения неполадок, так как оно используется для поиска в журналах доступа.|

## <a name="next-steps"></a>Дальнейшие шаги

- [Создание передней дверцы](quickstart-create-front-door.md)
- [Принцип работы передней дверцы](front-door-routing-architecture.md)

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png
