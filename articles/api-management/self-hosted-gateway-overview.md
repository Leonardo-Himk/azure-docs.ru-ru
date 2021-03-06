---
title: Обзор самостоятельно размещенного шлюза | Документация Майкрософт
description: Узнайте, как самостоятельный шлюз в службе управления API Azure помогает организациям управлять API в гибридных и многооблачных средах.
services: api-management
documentationcenter: ''
author: vlvinogr
manager: gwallace
editor: ''
ms.service: api-management
ms.topic: article
ms.date: 04/26/2020
ms.author: apimpm
ms.openlocfilehash: b560b02544eeb96167e68ed305d4d9942d2b1e0f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232978"
---
# <a name="self-hosted-gateway-overview"></a>Общие сведения о независимом шлюзе

В этой статье объясняется, как функция самостоятельного размещения шлюза в службе управления API Azure обеспечивает гибридное и многооблачное управление API, предоставляет свою высокоуровневую архитектуру и выделяет ее возможности.

## <a name="hybrid-and-multi-cloud-api-management"></a>Гибридное и многооблачное управление API

Функция самостоятельно размещенного шлюза расширяет возможности управления API для гибридных и многооблачных сред, а также позволяет организациям эффективно и безопасно управлять интерфейсами API, размещенными локально и в разных облаках, из одной службы управления API в Azure.

С помощью самостоятельно размещенного шлюза клиенты могут развертывать контейнерную версию компонента шлюза управления API в тех же средах, где размещены их API. Все локальные шлюзы управляются из службы управления API, с которой они объединяются, тем самым предоставляя клиентам возможность просмотра и унифицированного управления для всех внутренних и внешних API-интерфейсов. Размещение шлюзов близко к интерфейсам API позволяет клиентам оптимизировать потоки трафика API и обеспечить безопасность и соответствие требованиям.

Каждая служба управления API состоит из следующих ключевых компонентов:

-   Плоскость управления, представленная в виде API, используемая для настройки службы с помощью портал Azure, PowerShell и других поддерживаемых механизмов.
-   Шлюз (или плоскость данных) отвечает за прокси-запросы API, применение политик и сбор данных телеметрии.
-   Портал разработчика, используемый разработчиками для обнаружения, изучения и адаптации к использованию API-интерфейсов

По умолчанию все эти компоненты развертываются в Azure, что приводит к тому, что весь трафик API (показанный на рисунке ниже с помощью сплошных стрелок) будет проходить через Azure независимо от того, где размещаются интерфейсы, реализующие API. Оперативная простота этой модели связана с затратами на увеличение задержки, проблем соответствия и в некоторых случаях за дополнительную плату за перемещение данных.

![Поток трафика API без самостоятельно размещенных шлюзов](media/self-hosted-gateway-overview/without-gateways.png)

Развертывание автономных шлюзов в тех же средах, где размещены реализации API серверной части, позволяет передавать трафик API непосредственно в серверные API-интерфейсы, что повышает задержку, оптимизирует затраты на передачу данных и обеспечивает соответствие, сохраняя преимущества единой точки управления, наблюдения и обнаружения всех API в организации независимо от того, где размещаются их реализации.

![Поток трафика API с помощью самостоятельно размещенных шлюзов](media/self-hosted-gateway-overview/with-gateways.png)

## <a name="packaging-and-features"></a>Упаковка и компоненты

Автономный шлюз — это контейнерная, функционально эквивалентная версия управляемого шлюза, развернутого в Azure в составе каждой службы управления API. Автономный шлюз доступен в виде [контейнера](https://aka.ms/apim/sputnik/dhub) DOCKER на основе Linux из реестра контейнеров Майкрософт. Его можно развернуть в DOCKER, Kubernetes или любом другом решении оркестрации контейнеров, работающем в локальном кластере серверов, в облачной инфраструктуре, а также в целях оценки и разработки на персональном компьютере.

Следующие функции, доступные в управляемых шлюзах, **недоступны** в шлюзах, размещенных на собственном сервере:

- Журналы Azure Monitor
- Вышестоящее (серверная сторона) версия TLS и управление шифрами
- Проверка сертификатов сервера и клиента с помощью [корневых сертификатов ЦС](api-management-howto-ca-certificates.md) , отправленных в службу управления API. Чтобы добавить поддержку пользовательского ЦС, добавьте слой в образ контейнера для автономного шлюза, который устанавливает корневой сертификат ЦС.
- Интеграция с [Service Fabric](../service-fabric/service-fabric-api-management-overview.md)
- Возобновление сеанса TLS
- Повторное согласование сертификата клиента. Это означает, что для [проверки подлинности с помощью сертификата клиента](api-management-howto-mutual-certificates-for-clients.md) для пользователей API необходимо предоставить свои сертификаты в ходе первоначального подтверждения TLS. Чтобы убедиться в этом, включите параметр согласовать сертификат клиента при настройке пользовательского имени узла для самостоятельно размещенного шлюза.
- Встроенный кэш. Ознакомьтесь с этим [документом](api-management-howto-cache-external.md) , чтобы узнать об использовании внешнего кэша в самостоятельно размещенных шлюзах.

## <a name="connectivity-to-azure"></a>Подключение к Azure

Для самостоятельно размещенных шлюзов требуется исходящее подключение TCP/IP к Azure через порт 443. Каждый самостоятельно размещенный шлюз должен быть связан с одной службой управления API и настроен с помощью своей плоскости управления. Шлюз с автономным размещением использует подключение к Azure для:

-   Сообщает о состоянии, отправляя сообщения пульса каждую минуту
-   Регулярная проверка (каждые 10 секунд) и применение обновлений конфигурации, когда они доступны
-   Отправка журналов запросов и метрик в Azure Monitor, если это настроено.
-   Отправка событий в Application Insights, если для этого задано значение

При потере подключения к Azure шлюзу, размещенному в собственном расположении, не удастся получить обновления конфигурации, сообщить о своем состоянии или отправить телеметрию.

Шлюз, размещенный в собственном расположении, предназначен для "сбоя статического" и может выдерживать временную утрату подключения к Azure. Его можно развернуть с резервной копией локальной конфигурации или без нее. В первом случае самостоятельно размещенные шлюзы регулярно сохраняют резервную копию последней скачанной конфигурации на постоянном томе, подключенном к контейнеру или модулю Pod.

Когда резервное копирование конфигурации отключено и подключение к Azure прерывается:

-   Выполнение автономных шлюзов будет продолжать работать с использованием копии конфигурации, находящиеся в памяти
-   Остановленные автономные шлюзы не смогут запуститься

Если резервная копия конфигурации включена и подключение к Azure прервано:

-   Выполнение автономных шлюзов будет продолжать работать с использованием копии конфигурации, находящиеся в памяти
-   Остановленные локальные шлюзы могут начать использовать резервную копию конфигурации.

Когда подключение будет восстановлено, каждый размещенный шлюз, затронутый сбоем, автоматически подключится к соответствующей службе управления API и загрузит все обновления конфигурации, произошедшие, когда шлюз был отключен от сети.

## <a name="next-steps"></a>Следующие шаги

-   [Прочитайте технический документ с дополнительными сведениями по этой теме](https://aka.ms/hybrid-and-multi-cloud-api-management)
-   [Развертывание самостоятельно размещенного шлюза в DOCKER](how-to-deploy-self-hosted-gateway-docker.md)
-   [Развертывание самостоятельно размещенного шлюза в Kubernetes](how-to-deploy-self-hosted-gateway-kubernetes.md)
