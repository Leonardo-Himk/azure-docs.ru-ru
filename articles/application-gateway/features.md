---
title: Функции шлюза приложений Azure
description: Сведения о функциях шлюза приложений Azure
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: victorh
ms.openlocfilehash: f021eed959ef88a1ef3671e1d0ace8080710c92a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80810229"
---
# <a name="azure-application-gateway-features"></a>Функции шлюза приложений Azure

[Шлюз приложений Azure](overview.md) — это подсистема балансировки нагрузки веб-трафика, предназначенная для управления трафиком веб-приложений.

![Принцип работы Шлюза приложений](media/overview/figure1-720.png)

Шлюз приложений включает следующие функции.

- [Завершение запросов SSL/TLS](#secure-sockets-layer-ssltls-termination)
- [Автомасштабирование](#autoscaling)
- [Избыточность зоны](#zone-redundancy)
- [Статический виртуальный IP-адрес](#static-vip)
- [Брандмауэр веб-приложения](#web-application-firewall)
- [Контроллер входящего трафика для AKS](#ingress-controller-for-aks)
- [Маршрутизация на основе URL-адреса](#url-based-routing)
- [Размещение нескольких сайтов](#multiple-site-hosting)
- [Перенаправление](#redirection)
- [Сходство сеансов](#session-affinity)
- [Трафик WebSocket и HTTP/2](#websocket-and-http2-traffic)
- [фильтрация подключений;](#connection-draining)
- [Пользовательские страницы ошибок](#custom-error-pages)
- [Перезапись заголовков HTTP](#rewrite-http-headers)
- [Определение размера](#sizing)

## <a name="secure-sockets-layer-ssltls-termination"></a>Завершение запросов SSL/TLS

Шлюз приложений поддерживает завершение запросов SSL/TLS на шлюзе, после чего трафик обычно передается в незашифрованном виде на внутренние серверы. Это позволяет избавить веб-серверы от накладных расходов, связанных с ресурсоемкими операциями шифрования и расшифровки. Но иногда небезопасное взаимодействие с серверами не является приемлемым вариантом. Причина может заключаться в требованиях безопасности и соответствия или в том, что приложение может принимать только безопасные подключения. Для таких приложений шлюз приложений поддерживает сквозное шифрование SSL/TLS.

Дополнительные сведения см. в разделе Общие сведения о [завершении SSL и сквозном использовании SSL с помощью шлюза приложений](ssl-overview.md) .

## <a name="autoscaling"></a>Автомасштабирование

Шлюз приложений Standard_v2 поддерживает автоматическое масштабирование и может увеличивать или уменьшать масштаб в зависимости от изменения шаблонов нагрузки на трафик. Кроме того, автоматическое масштабирование позволяет не выбирать размер развертывания или количество экземпляров во время подготовки. 

Дополнительные сведения о компонентах Standard_v2 шлюза приложений см. в разделе [Автомасштабирование SKU v2](application-gateway-autoscaling-zone-redundant.md).

## <a name="zone-redundancy"></a>Избыточность в пределах зоны

Шлюз приложений Standard_v2 может охватывать несколько Зоны доступности, обеспечивая более высокую устойчивость к сбоям и устраняя необходимость в подготовке отдельных шлюзов приложений в каждой зоне.

## <a name="static-vip"></a>Статический виртуальный IP-адрес

Шлюз приложений Standard_v2 SKU поддерживает только тип статического виртуального IP-адреса. Это гарантирует, что виртуальный IP-адрес, связанный со Шлюзом приложений, не изменится даже с течением времени существования Шлюза приложений.

## <a name="web-application-firewall"></a>Брандмауэр веб-приложения

Брандмауэр веб-приложения (WAF) — это служба, которая обеспечивает централизованную защиту веб-приложений от распространенных эксплойтов и уязвимостей. Брандмауэр веб-приложения основан на правилах из [основных наборов правил OWASP (открытый проект безопасности веб-приложений)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) версии 3.1 (только WAF_v2), 3.0 или 2.2.9. 

Веб-приложения все чаще подвергаются вредоносным атакам, использующим общеизвестные уязвимости. Повсеместно используются уязвимости для атак путем внедрения кода SQL и межсайтовых сценариев, и это лишь немногие из них. Предотвращение таких атак в коде приложения может быть сложной задачей и потребовать тщательного обслуживания, исправления и мониторинга на многих уровнях топологии приложения. Централизованный брандмауэр веб-приложения значительно упрощает управление безопасностью и помогает администраторам защищать приложение от угроз вторжения. Решение WAF может быстрее реагировать на угрозы безопасности по сравнению с защитой каждого отдельного веб-приложения благодаря установке исправлений известных уязвимостей в центральном расположении. Существующие шлюзы приложений можно легко преобразовать в шлюз приложений с поддержкой брандмауэра веб-приложения.

Дополнительные сведения см. в статье [Что такое брандмауэр веб-приложения Azure?](../web-application-firewall/overview.md).

## <a name="ingress-controller-for-aks"></a>Контроллер входящего трафика для AKS
Контроллер входящего трафика Шлюза приложений (AGIC) позволяет использовать трафик от Шлюза приложений в качестве входящего трафика для кластера [Службы Azure Kubernetes (AKS)](https://azure.microsoft.com/services/kubernetes-service/). 

Входной контроллер работает как модуль в кластере AKS и потребляет [ресурсы Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/) , а затем преобразует их в конфигурацию шлюза приложений, что позволяет шлюзу балансировать нагрузку на трафик в модули Kubernetes. Входной контроллер поддерживает только шлюзы приложений Standard_v2 и WAF_v2 SKU. 

Дополнительные сведения о контроллере входящего трафика Шлюза приложений см. в [этой статье](ingress-controller-overview.md).

## <a name="url-based-routing"></a>Маршрутизация на основе URL-адреса

Маршрутизация на основе URL-путей позволяет направлять трафик в пулы тыловых серверов в зависимости от URL-путей поступающих запросов. Один из сценариев — это маршрутизация запросов на различные типы содержимого в различные пулы.

Например, запросы для `http://contoso.com/video/*` направляются в VideoServerPool, а запросы для `http://contoso.com/images/*` — в ImageServerPool. Если ни один из шаблонов пути не подходит, выбирается пул DefaultServerPool.

Дополнительные сведения см. в разделе [Общие сведения о маршрутизации на основе URL-путей](url-route-overview.md).

## <a name="multiple-site-hosting"></a>Размещение нескольких сайтов

Размещение нескольких сайтов позволяет настроить в одном экземпляре шлюза приложений несколько веб-сайтов. Эта функция позволяет настроить более эффективную топологию для развертываний, добавив до 100 веб-сайтов в один шлюз приложений (для оптимальной производительности). Каждый веб-сайт может быть направлен к собственному пулу. Например, шлюз приложений может обслуживать трафик для `contoso.com` и `fabrikam.com` из двух пулов серверов с именами ContosoServerPool и FabrikamServerPool.

Запросы для `http://contoso.com` маршрутизируются в ContosoServerPool, а запросы для `http://fabrikam.com` — в FabrikamServerPool.

Точно так же в одном развернутом шлюзе приложений можно разместить два поддомена одного родительского домена. К примерам использования поддоменов можно отнести размещение `http://blog.contoso.com` и `http://app.contoso.com` в одном развернутом шлюзе приложений.

Дополнительные сведения см. в разделе [Размещение нескольких сайтов шлюза приложений](multiple-site-overview.md).

## <a name="redirection"></a>Перенаправление

Для многих веб-приложений привычна поддержка автоматического перенаправления с HTTP на HTTPS, чтобы все взаимодействия между приложением и его пользователями осуществлялись по зашифрованному пути.

В прошлом вы могли использовать такие методы, как создание выделенного пула, единственным назначением которого было перенаправление запросов, получаемых по HTTP, на HTTPS. Шлюз приложений поддерживает возможность перенаправления трафика на шлюз приложений. Это упрощает конфигурацию приложения, оптимизирует использование ресурсов и обеспечивает поддержку новых сценариев перенаправления, включая глобальное перенаправление на основе пути. Поддержка перенаправления для Шлюза приложений не ограничена лишь перенаправлением с HTTP на HTTPS. Это универсальный механизм перенаправления, позволяющий перенаправить трафик с любого порта и на любой порт, определяемый с помощью правил. Также поддерживается перенаправление на внешние веб-сайты.

Возможности поддержки перенаправления для шлюза приложений:

- Глобальное перенаправление с одного порта на другой через шлюз. Это позволяет организовать на сайте перенаправление с HTTP на HTTPS.
- Перенаправление на основе пути. Этот тип перенаправления позволяет организовать перенаправление с HTTP на HTTPS только в указанную область сайта, например в область корзины для покупок, которая обозначается как `/cart/*`.
- Перенаправление на внешний сайт.

Дополнительные сведения см. в статье [Обзор перенаправления шлюза приложений](redirect-overview.md).

## <a name="session-affinity"></a>Сходство сеансов

Функция сходства сеансов на основе файлов cookie удобна, если нужно, чтобы сеанс пользователя выполнялся на одном и том же сервере. Используя управляемые шлюзом файлы cookie, шлюз приложений может направлять последующий трафик из сеанса пользователя на тот же сервер для обработки. Эта функция важна, когда состояние сеанса сохраняется локально на сервере для сеанса пользователя.

Дополнительные сведения см. в разделе [как работает шлюз приложений](how-application-gateway-works.md#modifications-to-the-request).

## <a name="websocket-and-http2-traffic"></a>Трафик WebSocket и HTTP/2

Шлюз приложений обеспечивает встроенную поддержку протоколов WebSocket и HTTP/2. Настраиваемый пользователем параметр для выборочного включения или отключения поддержки WebSocket отсутствует.

Протоколы WebSocket и HTTP/2 обеспечивают полную дуплексную связь между клиентом и сервером через длительное TCP-подключение. Благодаря этому обеспечивается лучшее интерактивное взаимодействие между веб-сервером и клиентом, которое может быть двунаправленным, без необходимости выполнять опрос, как это требуется в реализациях на основе HTTP. Эти протоколы имеют низкую нагрузку, в отличие от HTTP, и могут повторно использовать одно и то же TCP-соединение для нескольких запросов и ответов, что приводит к более эффективному использованию ресурсов. Эти протоколы предназначены для работы через традиционные HTTP-порты 80 и 443.

Дополнительные сведения см. в разделах о [поддержке WebSocket](application-gateway-websocket.md) и [поддержке HTTP/2](configuration-overview.md#http2-support).

## <a name="connection-draining"></a>фильтрация подключений;

Фильтрация подключений поможет обеспечить корректное удаление элементов серверного пула на протяжении запланированных обновлений службы. Этот параметр включается через параметры HTTP серверной части и может применяться ко всем элементам серверного пула при создании правила. После включения шлюз приложений гарантирует, что все незарегистрированные экземпляры внутреннего пула не получат новый запрос, одновременно разрешив выполнение существующих запросов в течение заданного предела времени. Это относится к экземплярами серверного пула, которые явно удалены из него путем изменения пользовательской конфигурации, и которые после проверки работоспособности определены как неработоспособные. Единственным исключением из этого являются запросы, связанные для отмены регистрации экземпляров, которые были отменены явным образом, из-за сходства сеанса, управляемого шлюзом, и по-своему, а также по-своемуу прокси для отмены регистрации экземпляров.

Дополнительные сведения см. в статье [Обзор конфигурации шлюза приложений](configuration-overview.md#connection-draining).

## <a name="custom-error-pages"></a>Пользовательские страницы ошибок

Шлюз приложений позволяет создавать пользовательские страницы ошибок вместо отображения страниц ошибок по умолчанию. На пользовательской странице ошибок вы можете использовать собственные символику и макет.

См. также о [пользовательских ошибках](custom-error.md).

## <a name="rewrite-http-headers"></a>Перезапись заголовков HTTP

Заголовки HTTP позволяют клиенту и серверу передавать дополнительную информацию вместе с запросом или ответом. Перезаписывая эти заголовки HTTP, можно реализовать несколько важных сценариев, в том числе:

- Добавление полей заголовка, связанных с безопасностью, например HSTS или X-XSS-Protection.
- Удаление полей заголовка ответа, которые могут раскрыть конфиденциальную информацию.
- Чередование информации о портах из заголовков X-Forwarded-For.

Шлюз приложений поддерживает возможность добавления, удаления и обновления заголовков запросов и ответов HTTP, пока пакеты запросов и ответов перемещаются между клиентским и внутренним пулами. Он также дает возможность добавить условия, чтобы убедиться, что указанные заголовки перезаписываются только в том случае, если соблюдены определенные условия.

Дополнительные сведения см. в разделе о [перезаписи заголовков HTTP](rewrite-http-headers.md).

## <a name="sizing"></a>Определение размера

Standard_v2 шлюза приложений можно настроить для развертывания с автомасштабированием или с фиксированным размером. Этот SKU не поддерживает различные размеры экземпляров. Дополнительные сведения о производительности и ценах на версию 2 см. в статье [Автоматическое масштабирование и Шлюз приложений, избыточный между зонами, версии 2](application-gateway-autoscaling-zone-redundant.md#pricing).

Стандарт шлюза приложений предлагается в трех размерах: **малый**, **средний**и **крупный**. Экземпляры малого размера предназначены для разработки и тестирования сценариев.

Полный список ограничений шлюза приложений см. [здесь](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

В таблице ниже показана средняя пропускная способность каждого экземпляра шлюза приложений версии 1 с активированной разгрузкой SSL.

| Средний размер ответа страницы сервера | Малый | Средний | большой |
| --- | --- | --- | --- |
| 6 КБ |7,5 Мбит/с |13 Мбит/с |50 Мбит/с |
| 100 КБ |35 Мбит/с |100 Мбит/с |200 Мбит/с |

> [!NOTE]
> Это примерные значения для настройки пропускной способности шлюза приложений. Фактическая пропускная способность зависит от различных параметров среды, таких как средний размер страницы, расположение внутренних экземпляров и время обработки страницы сервером. Чтобы точно определить производительность, выполните собственные тесты. Приведенные здесь значения служат только для планирования емкости.

## <a name="version-feature-comparison"></a>Сравнение функций версий

Сравнение компонентов шлюза приложений версии 1 – v2 см. в разделе [Автомасштабирование и избыточность в зонах — шлюз приложений версии 2](application-gateway-autoscaling-zone-redundant.md#feature-comparison-between-v1-sku-and-v2-sku) .

## <a name="next-steps"></a>Дальнейшие шаги

- Сведения о работе шлюза приложений [How an application gateway works](how-application-gateway-works.md)