---
title: Управляемые реестры контейнеров
description: Общие сведения о службе реестра контейнеров Azure, предоставляющей облачные, управляемые и частные реестры Docker.
author: stevelas
ms.topic: overview
ms.date: 02/10/2020
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: 1992a2a63d16a955d136459f5dbaece7df815c71
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2020
ms.locfileid: "77132031"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Общие сведения о частных реестрах контейнеров Docker в Azure

Реестр контейнеров Azure — это управляемая частная служба реестра Docker, основанная на реестре Docker 2.0 с открытым кодом. Создавайте и настраивайте реестры контейнеров Azure, чтобы хранить частные образы контейнеров Docker и связанные артефакты, а также управлять ими.

Используйте реестры контейнеров Azure с существующими конвейерами разработки и развертывания контейнеров или используйте задачи реестра контейнеров Azure для создания образов контейнеров в Azure. Создавайте сборки по требованию или полностью автоматизированные сборки с помощью триггеров, таких как принятие исходного кода и обновление базового образа.

Дополнительные сведения о концепции Docker и реестра см. в разделах [Docker overview](https://docs.docker.com/engine/docker-overview/) (Обзор Docker) и [About registries, repositories, and images](container-registry-concepts.md) (О реестрах, репозиториях и образах).

## <a name="use-cases"></a>Варианты использования

Извлекайте образы из реестра контейнеров Azure и отправляйте их в разные места назначения развертывания:

* **Масштабируемые системы управления**, управляющие контейнерными приложениями в кластерах узлов, в том числе [Kubernetes](https://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/) и [Docker Swarm](https://docs.docker.com/swarm/).
* **Службы Azure**, поддерживающие создание и выполнение масштабированных приложений, в том числе [служба Azure Kubernetes (AKS)](../aks/index.yml), [служба приложений](../app-service/index.yml), [пакетная служба](../batch/index.yml), [Service Fabric](/azure/service-fabric/) и т. д.

Разработчики также могут отправлять образы в реестр контейнеров в рамках рабочего процесса разработки контейнера. Например, цель может получить доступ к реестру контейнеров из средства доставки и обеспечения непрерывной интеграции, таких как [Azure Pipelines](/azure/devops/pipelines/ecosystems/containers/acr-template) или [Jenkins](https://jenkins.io/).

Настройте в Задачах ACR автоматическое восстановление образов приложений при обновлении базовых образов или автоматизацию сборок образов, если ваша команда фиксирует код в репозиторий Git. Создавайте многошаговые задачи для автоматизации сборки, тестирования и внедрения исправлений нескольких образов контейнеров в параллельном режиме в облаке.

Azure предоставляет средства, включая интерфейс командной строки Azure, портал Azure и поддержку API, для управления реестрами контейнеров Azure. При необходимости установите [расширение Docker для Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) и расширение [учетной записи Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) для работы со своими реестрами контейнеров Azure. Извлекайте и отправляйте образы в реестр контейнеров Azure или запускайте Задачи ACR в Visual Studio Code.

## <a name="key-features"></a>Основные возможности

* **Номера SKU реестра контейнеров**. Создайте один или несколько реестров контейнеров в своей подписке Azure. Реестры доступны в трех номерах SKU: ["Базовый", "Стандартный" и "Премиум"](container-registry-skus.md), каждый из которых поддерживает интеграцию веб-перехватчика, проверку подлинности в реестре с помощью Azure Active Directory и функцию удаления. Создание реестра в том же расположении Azure, где находятся развертывания, позволяет воспользоваться преимуществами локального хранилища образов контейнеров с доступом по сети. Используйте возможность [георепликации](container-registry-geo-replication.md) реестров уровня "Премиум" для сценариев расширенной репликации и распределения образов контейнеров. 

* **Безопасность и доступ**. Войдите в реестр с помощью Azure CLI или стандартной команды `docker login`. Реестр контейнеров Azure передает образы контейнеров по протоколу HTTPS и поддерживает протокол TLS для защиты клиентских подключений. 

  > [!IMPORTANT]
  > С 13 января 2020 года Реестр контейнеров Azure будет требовать использовать протокол TLS версии 1.2 для всех безопасных подключений серверов и приложений. Включите TLS 1.2 с помощью любого нового клиента Docker (версии не ниже 18.03.0). Поддержка протоколов TLS версии 1.0 и 1.1 будет прекращена. 

  [Контроль доступа](container-registry-authentication.md) к реестру контейнеров осуществляется с помощью удостоверения Azure, [субъекта-службы](../active-directory/develop/app-objects-and-service-principals.md) на основе Azure Active Directory или предоставленной учетной записи администратора. Используйте управление доступом на основе ролей для назначения пользователям или системам детальных разрешений для реестра.

  Функции безопасности номера SKU уровня "Премиум" охватывают [доверие к содержимому](container-registry-content-trust.md) для подписи тега образа, а также [брандмауэры и виртуальные сети (предварительная версия)](container-registry-vnet.md) для ограничения доступа к реестру. При необходимости Центр безопасности Azure интегрируется с Реестром контейнеров Azure для [сканирования образов](../security-center/azure-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json) каждый раз при помещении образа в реестр.

* **Поддерживаемые образы и артефакты**. Каждое изображение, сгруппированное в репозитории, является образом, доступным только для чтения в Docker-совместимом контейнере. Реестры контейнеров Azure могут содержать образы Windows и Linux. Имена образов нужно указывать для всех развертываний контейнеров. Используйте стандартные [команды Docker](https://docs.docker.com/engine/reference/commandline/), чтобы отправлять образы в репозиторий и извлекать их из него. Кроме образов контейнера Docker, в Реестре контейнеров Azure хранятся [связанные форматы содержимого](container-registry-image-formats.md), например [схемы Helm](container-registry-helm-repos.md) и встроенные изображения [для спецификации формата изображений Open Container Initiative (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md).

* **Автоматизированная сборка образа**. Используйте [Задачи Реестра контейнеров Azure](container-registry-tasks-overview.md) (Задачи ACR) для упрощения создания, тестирования, отправки и развертывания образов в Azure. Например, используйте сборку задач ACR для расширения внутреннего цикла разработки в облако за счет выгрузки операций `docker build` в Azure. Настройте задачи сборки, чтобы автоматизировать конвейер установки исправлений ОС и платформы контейнера и автоматически создавать образы, когда ваша команда фиксирует код в системе управления версиями.

  Функция [Многошаговые задачи](container-registry-tasks-overview.md#multi-step-tasks) обеспечивает определение и выполнение задач на основе шага для создания, тестирования и исправления изображений контейнера в облаке. Шаги задач определяют отдельный образ контейнера сборки и перемещения контейнеров. Они также могут определять выполнение одного или нескольких контейнеров, причем каждый шаг использует контейнер в качестве среды выполнения.

## <a name="next-steps"></a>Дальнейшие действия

* [Создание реестра контейнеров на портале Azure](container-registry-get-started-portal.md)
* [Создание реестра контейнеров с помощью Azure CLI](container-registry-get-started-azure-cli.md)
* [Automate container image builds and maintenance with ACR Tasks](container-registry-tasks-overview.md) (Автоматизация сборки и обслуживание контейнеров с помощью задач ACR)