---
title: Основные понятия сетевых взаимодействий в Службе Azure Kubernetes (AKS)
description: Дополнительные сведения о сетях в Службе Azure Kubernetes (AKS), включая сети kubenet и Azure CNI, входящие контроллеры, подсистемы балансировки нагрузки и статические IP-адреса.
ms.topic: conceptual
ms.date: 02/28/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 51773a46b77cb1e9a89b9c85a5f62c4a6b7af3be
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146059"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Основные понятия сети в Службе Azure Kubernetes (AKS)

В подходе микрослужб контейнерного формата к использованию разработки приложений компоненты приложения должны работать вместе. Kubernetes предоставляет различные ресурсы, которые позволяют приложениям взаимодействовать. К приложениям можно подключиться и предоставить внутренний или внешний доступ. Чтобы создать приложения с высоким уровнем доступности, можно распределить их нагрузку. Более сложные приложения требуют настройку входящего трафика для маршрутизации из нескольких компонентов и подключений SSL/TLS. По соображениям безопасности может потребоваться ограничение передачи трафика в или между модулями pod и узлами.

В этой статье приводится обзор ключевых понятий сети в приложениях в AKS.

- [Службы](#services)
- [Виртуальные сети Azure](#azure-virtual-networks)
- [Контроллеры входящих данных](#ingress-controllers)
- [Политики сети](#network-policies)

## <a name="kubernetes-basics"></a>Базовые сведения о Kubernetes

Чтобы предоставить доступ к приложениям или разрешить компонентам приложений взаимодействовать друг с другом, Kubernetes предоставляет уровень абстракции для виртуальной сети. Узлы Kubernetes подключены к виртуальной сети, а также могут предоставлять входящее и исходящее подключение для модулей pod. Чтобы предоставить сетевые компоненты, компонент *прокси-сервера kube* выполняется на каждом узле.

В Kubernetes *Службы* логически группируют модули, чтобы разрешать прямой доступ с помощью IP-адреса или DNS-имени и через определенный порт. *Подсистема балансировки нагрузки* может распределять трафик. Более сложную маршрутизацию трафика приложения можно получить с помощью *контролеров входящего трафика*. Безопасность и фильтрацию сетевого трафика модулей pod можно совершить с помощью *политик сети* Kubernetes.

Платформа Azure также помогает упростить виртуальные сети для кластеров AKS. При создании подсистемы балансировки нагрузки Kubernetes создается и настраивается подсистема балансировки нагрузки Azure. При открытии сетевых портов для модулей pod настраиваются соответствующие правила группы безопасности сети Azure. Для маршрутизации приложений протокола HTTP Azure может настроить *внешний DNS-сервер* как новый входящий трафик.

## <a name="services"></a>Службы

Чтобы упростить конфигурацию сети для рабочих нагрузок приложений, Kubernetes использует *Службы*, чтобы логически сгруппировать набор модулей и установить сетевое подключение. Ниже приведены доступные типы служб.

- **IP-адрес кластера** — создает внутренний IP-адрес для использования в кластере AKS. Подходит только для внутренних приложений, которые поддерживают другие рабочие нагрузки в кластере.

    ![Схема, показывающая поток трафика IP-адреса кластера в кластере AKS][aks-clusterip]

- **NodePort** — создает сопоставление портов на основной узел, который позволяет получить приложение непосредственно с узла IP-адреса и порта.

    ![Схема, показывающая поток трафика NodePort в кластере AKS][aks-nodeport]

- **LoadBalancer** — создает ресурс подсистемы балансировки нагрузки Azure, настраивает внешний IP-адрес и подключает запрошенные модули pod, содержащиеся во внутреннем пуле подсистемы балансировки нагрузки. Чтобы разрешить трафику клиентов достичь приложения, на нужных портах создаются правила балансировки нагрузки. 

    ![Схема, показывающая поток трафика Load Balancer в кластере AKS][aks-loadbalancer]

    Для дополнительного контроля и маршрутизации входящего трафика можно вместо этого использовать [контроллер входящего трафика](#ingress-controllers).

- **ExternalName** — создает определенную запись DNS для упрощения доступа к приложениям.

IP-адрес для подсистемы балансировки нагрузки и служб может назначаться динамически, или можно указать существующий статический IP-адрес для использования. Можно назначить внутренние и внешние статические IP-адреса. Часто этот существующий статический IP-адрес связан с записью DNS.

Можно создать *внутреннюю* и *внешнюю* подсистему балансировки нагрузки. Внутренним подсистемам балансировки нагрузки назначается только частный IP-адрес, поэтому к ним нельзя получить доступ из Интернета.

## <a name="azure-virtual-networks"></a>Виртуальные сети Azure

В AKS можно развернуть кластер, использующий одну из двух следующих моделей сети.

- Сеть *kubenet* — ресурсы обычно создаются и настраиваются при развертывании кластера AKS.
- Сеть *Azure Container Networking Interface (CNI)*  — кластер AKS подключен к имеющимся ресурсам виртуальной сети и конфигурациям.

### <a name="kubenet-basic-networking"></a>Сетевое взаимодействие с kubenet (базовое)

Сетевой параметр *kubenet* — это конфигурация сети по умолчанию для создания кластера AKS. При использовании *kubenet* узлы получают IP-адрес из подсети виртуальной сети Azure. Объекты pod получают IP-адрес из логически отличного адресного пространства в подсеть узлов виртуальной сети Azure. Затем настраивается преобразование сетевых адресов (NAT) таким образом, чтобы объекты pod могли получить доступ к ресурсам в виртуальной сети Azure. Исходный IP-адрес трафика преобразуется в первичный IP-адрес узла.

Узлы используют подключаемый модуль Kubernetes [kubenet][kubenet]. Вы можете предоставить платформе Azure разрешение на создание и настройку виртуальных сетей или выполнить развертывание кластера AKS в имеющуюся подсеть виртуальной сети. Опять же, только узлы получают маршрутизируемый IP-адрес, а модули Pod используют NAT для связи с другими ресурсами за пределами кластера AKS. Такой подход значительно уменьшает количество IP-адресов, которые необходимо зарезервировать в пространстве сети для объектов pod.

Дополнительные сведения см. в статье [Использование сети kubenet с собственными диапазонами IP-адресов в Службе Azure Kubernetes (AKS)][aks-configure-kubenet-networking].

### <a name="azure-cni-advanced-networking"></a>Сетевое взаимодействие с Azure CNI (расширенное)

При использовании Azure CNI каждый модуль pod получает IP-адрес из подсети, и к нему можно получить прямой доступ. Эти IP-адреса должны быть уникальными во всей сети, и их следует планировать заранее. Для каждого узла предусмотрен параметр конфигурации, в котором указывается максимальное число объектов pod, которые он поддерживает. Затем на каждом узле заранее резервируется эквивалентное число IP-адресов для этого узла. Этот подход требует большего планирования, так как в противном случае это может привести к исчерпанию IP-адреса или необходимости перестроения кластеров в более крупной подсети по мере роста требований приложения.

Узлы используют подключаемый модуль Kubernetes [Azure Container Networking Interface (CNI)][cni-networking].

![Схема, демонстрирующая два узла с мостами, подключающими каждый из них к одной виртуальной сети Azure][advanced-networking-diagram]

Дополнительные сведения см. в статье о [настройке Azure CNI для кластера AKS][aks-configure-advanced-networking].

### <a name="compare-network-models"></a>Сравнить сетевые модели

Кубенет и Azure CNI обеспечивают сетевое подключение к кластерам AKS. Однако у каждого есть свои преимущества и недостатки. На высоком уровне прилагаются следующие соображения.

* **кубенет**
    * Экономит пространство IP-адресов.
    * Использует внутреннюю или внешнюю подсистему балансировки нагрузки Kubernetes для доступа к модулям Pod извне кластера.
    * Необходимо вручную управлять и поддерживать определяемые пользователем маршруты (определяемые пользователем маршруты).
    * Максимум 400 узлов на кластер.
* **Azure CNI**
    * Модули Pod получают полное подключение к виртуальной сети и могут напрямую доставляться через их частный IP-адрес из подключенных сетей.
    * Требуется больше пространства IP-адресов.

Между кубенет и Azure CNI существуют следующие различия в работе:

| Функция                                                                                   | кубенет   | Azure CNI |
|----------------------------------------------------------------------------------------------|-----------|-----------|
| Развертывание кластера в существующей или новой виртуальной сети                                            | Supported-определяемые пользователем маршруты применено вручную | Поддерживается |
| Подключение Pod-Pod                                                                         | Поддерживается | Поддерживается |
| Pod — подключение виртуальной машины; Виртуальная машина в той же виртуальной сети                                          | Работает при инициации модулем Pod | Работает обоими способами |
| Pod — подключение виртуальной машины; Виртуальная машина в одноранговой виртуальной сети                                            | Работает при инициации модулем Pod | Работает обоими способами |
| Локальный доступ с использованием VPN или Express Route                                                | Работает при инициации модулем Pod | Работает обоими способами |
| Доступ к ресурсам, защищенным конечными точками службы                                             | Поддерживается | Поддерживается |
| Предоставление Kubernetes служб с помощью службы балансировщика нагрузки, шлюза приложений или контроллера входящих данных | Поддерживается | Поддерживается |
| Azure DNS и частные зоны по умолчанию                                                          | Поддерживается | Поддерживается |

В отношении DNS с кубенет и DNS-подключаемыми модулями Azure CNI предоставляется Кореднс, набор управляющих программ, выполняющийся в AKS. Дополнительные сведения о Кореднс на Kubernetes см. в разделе [Настройка службы DNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/). Кореднс настроен по умолчанию для перенаправления неизвестных доменов на DNS-серверы узла, иными словами, на функциональность DNS виртуальной сети Azure, в которой развернут кластер AKS. Таким образом, Azure DNS и частные зоны будут работать для модулей Pod, работающих в AKS.

### <a name="support-scope-between-network-models"></a>Поддержка области между сетевыми моделями

Независимо от используемой модели сети, кубенет и Azure CNI можно развернуть одним из следующих способов:

* Платформа Azure может автоматически создавать и настраивать ресурсы виртуальной сети при создании кластера AKS.
* Вы можете вручную создать и настроить ресурсы виртуальной сети и присоединить их к этим ресурсам при создании кластера AKS.

Хотя такие возможности, как конечные точки службы или определяемые пользователем маршруты, поддерживаются как в кубенет, так и в Azure CNI, [политики поддержки для AKS][support-policies] определяют, какие изменения можно выполнить. Пример:

* Если вы вручную создаете ресурсы виртуальной сети для кластера AKS, вы будете поддерживать настройку собственных конечных точек определяемые пользователем маршруты или служб.
* Если платформа Azure автоматически создает ресурсы виртуальной сети для кластера AKS, то не может вручную изменить эти ресурсы, управляемые AKS, чтобы настроить собственные конечные точки определяемые пользователем маршруты или службы.

## <a name="ingress-controllers"></a>Контроллеры входящего трафика

При создании службы типа LoadBalancer создается базовый ресурс балансировщика нагрузки Azure. Подсистема балансировки нагрузки настраивается для распределения трафика к модулям pod, содержащихся в службе на данном порте. LoadBalancer работает только в подсистеме балансировки нагрузки уровня 4 — служба не знает о приложениях и не может вносить любые дополнительные рекомендации относительно маршрутизации.

*Входящие контроллеры* работают на уровне 7 и могут использовать более интеллектуальные правила для распределения трафика приложения. Контроллер входящего трафика обычно используется для передачи трафика HTTP для различных приложений, в зависимости от входящего URL-адреса.

![Схема, показывающая поток входящего трафика в кластере AKS][aks-ingress]

В AKS создайте входящий ресурс, например NGINX, или используйте функцию маршрутизации приложения AKS HTTP. При включении маршрутизации приложений HTTP для кластера AKS платформа Azure создает контроллер входящего трафика и контроллер *внешнего DNS*. Новые входящие ресурсы создаются в Kubernetes. В зоне DNS кластеров создаются необходимые записи DNS A. Дополнительные сведения см. в статье [Маршрутизация приложений HTTP][aks-http-routing].

Другой характерной входящего трафика является действие SSL/TLS. На больших веб-приложениях, доступных через HTTPS, терминация TLS может обрабатываться входящим ресурсом, а не самим приложением. Чтобы обеспечить автоматическое создание сертификации TLS и конфигурации, можно настроить входящий ресурс для использования поставщиком, например Let's Encrypt. Дополнительные сведения о настройке контроллера входящего трафика NGINX с помощью Let's Encrypt см. в статье [Создание контроллера входящего трафика HTTPS в Службе Azure Kubernetes (AKS)][aks-ingress-tls].

Вы также можете настроить контроллер входящих данных так, чтобы он сохранял исходный IP-адрес клиента в запросах к контейнерам в кластере AKS. Когда клиентский запрос направляется в контейнер в кластере AKS через входной контроллер, исходный IP-адрес этого запроса не будет доступен для целевого контейнера. При включении *сохранения IP-адресов источника клиента*исходный IP-адрес клиента доступен в заголовке запроса в разделе *X-forwardd-for*. Если вы используете сохранение IP-адресов источника клиента на контроллере входящего трафика, вы не сможете использовать транзитный TLS-трафик. IP-адрес источника и транзитный протокол TLS можно использовать с другими службами, такими как тип балансировщика *нагрузки* .

## <a name="network-security-groups"></a>Группы безопасности сети

Группа безопасности сети фильтрует трафик для виртуальных машин, например для узлов AKS. При создании служб (например, LoadBalancer) платформа Azure автоматически настраивает все необходимые правила группы безопасности сети. Чтобы фильтровать трафик для моделей pod в кластере AKS, не следует настраивать правила группы безопасности сети вручную. Определите все необходимые порты и пересылку как часть манифестов службы Kubernetes и позвольте платформе Azure создавать или обновлять соответствующие правила. Вы также можете использовать сетевые политики, как описано в следующем разделе, чтобы автоматически применять правила фильтрации трафика к модулям Pod.

## <a name="network-policies"></a>Политики сети

По умолчанию все контейнеры pod в кластере AKS могут отправлять и получать трафик без ограничений. Для повышения безопасности вы можете определить правила, управляющие потоком трафика. Серверные приложения представляются только необходимым внешним службам, или компоненты базы данных доступны только для уровней приложений, которые к ним подключаются.

Политика сети — это функция Kubernetes, доступная в AKS, которая позволяет управлять потоком трафика между модулями Pod. Вы можете разрешать или запрещать трафик в зависимости от параметров, например назначенных меток, пространства имен или порта трафика. Группы безопасности сети являются дополнительными для этих узлов AKS, а не для контейнеров pod. Использование политик сети — это более подходящий облачный способ управления потоком трафика. Поскольку контейнеры pod динамически создаются в кластере AKS, необходимые политики сети могут применяться автоматически.

Дополнительные сведения см. в статье [Secure traffic between pods using network policies in Azure Kubernetes Service (AKS)][use-network-policies] (Защита трафика между контейнерами pod с использованием политик сети в Azure Kubernetes Service (AKS)).

## <a name="next-steps"></a>Дальнейшие шаги

Чтобы начать работу с сетью AKS, создайте и настройте кластер AKS с собственными диапазонами IP-адресов, используя [kubenet][aks-configure-kubenet-networking] или [Azure CNI][aks-configure-advanced-networking].

Соответствующие рекомендации см. в разделе рекомендации [по подключению к сети и безопасности в AKS][operator-best-practices-network].

Дополнительные сведения о ключевых концепциях Kubernetes и AKS вы получите в следующих статьях:

- [Кластеры и рабочие нагрузки Kubernetes и AKS][aks-concepts-clusters-workloads]
- [Доступ и идентификация в Kubernetes и AKS][aks-concepts-identity]
- [Безопасность Kubernetes и AKS][aks-concepts-security]
- [Хранилище Kubernetes и AKS][aks-concepts-storage]
- [Масштабирование Kubernetes и AKS][aks-concepts-scale]

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ingress.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[aks-configure-advanced-networking]: configure-azure-cni.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[use-network-policies]: use-network-policies.md
[operator-best-practices-network]: operator-best-practices-network.md
[support-policies]: support-policies.md
