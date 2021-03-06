---
title: Настройка проверки актуальности в экземпляре контейнера
description: Подробные сведения о настройке проб активности для перезагрузки неработоспособных контейнеров в службе "Экземпляры контейнеров Azure"
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 11c6c9d39067c536bf4325f74eb24b2ab64ef515
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76934165"
---
# <a name="configure-liveness-probes"></a>Настройка проб активности

Контейнерные приложения могут выполняться в течение продолжительного периода времени, что приводит к повреждению состояний, которые, возможно, потребуется восстановить путем перезапуска контейнера. Служба "экземпляры контейнеров Azure" поддерживает проверки актуальности, чтобы можно было настроить контейнеры в группе контейнеров для перезапуска, если критические функции не работают. Проверка актуальности ведет себя так же, как проверка [Kubernetesности в режиме реального времени](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/).

В этой статье описывается развертывание группы контейнеров, содержащей пробу активности. В рамках этого примера демонстрируется автоматический перезапуск имитированного неработоспособного контейнера.

Служба "экземпляры контейнеров Azure" также поддерживает [проверки готовности](container-instances-readiness-probe.md), которые можно настроить так, чтобы трафик достиг контейнера только в том случае, если он готов к работе.

> [!NOTE]
> В настоящее время проверка актуальности не может использоваться в группе контейнеров, развернутой в виртуальной сети.

## <a name="yaml-deployment"></a>Развертывание файла YAML

Создайте файл `liveness-probe.yaml` со следующим фрагментом кода. Этот файл определяет группу контейнера, содержащую контейнер NGNIX, который в конечном итоге становится неработоспособным.

```yaml
apiVersion: 2018-10-01
location: eastus
name: livenesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: nginx
      command:
        - "/bin/sh"
        - "-c"
        - "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      livenessProbe:
        exec:
            command:
                - "cat"
                - "/tmp/healthy"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Выполните следующую команду, чтобы развернуть эту группу контейнеров с представленной выше конфигурацией YAML:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Команда Start

Развертывание включает `command` свойство, определяющее начальную команду, которая выполняется при первом запуске контейнера. Это свойство принимает массив строк. Эта команда имитирует контейнер при неработоспособном состоянии.

Во-первых, он запускает сеанс Bash и создает файл с `healthy` именем в `/tmp` каталоге. Затем он заждет 30 секунд перед удалением файла, а затем переходит в 10-минутный спящий режим:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

### <a name="liveness-command"></a>Команда активности

Это развертывание определяет `livenessProbe` , который поддерживает команду `exec` Live, которая действует как проверка на актуальность. Если эта команда завершается с ненулевым значением, контейнер удаляется и перезапускается, и сообщается о том `healthy` , что файл не найден. Если эта команда успешно завершает работу с кодом выхода 0, никакие действия не выполняются.

Свойство `periodSeconds` определяет команду активности, которая должна выполняться каждые 5 секунд.

## <a name="verify-liveness-output"></a>Проверка выходных данных активности

В течение первых 30 секунд завершается работа файла `healthy`, созданного с помощью команды запуска. Когда команда Live проверяет наличие `healthy` файла, код состояния возвращает 0, сигнал успешно завершается, поэтому перезагрузка не выполняется.

Через 30 секунд `cat /tmp/healthy` команда завершается ошибкой, что приводит к возникновению неработоспособности и прерыванию событий.

Эти события можно просмотреть на портале Azure или в Azure CLI.

![Неработоспособные события на портале][portal-unhealthy]

Просмотрев события в портал Azure, события типа `Unhealthy` вызываются при сбое команды в режиме реального времени. Последующее событие относится к типу `Killing`, что означает удаление контейнера, чтобы можно было начать перезагрузку. Счетчик перезапусков для контейнера увеличивается каждый раз, когда происходит это событие.

Перезапуски выполняются на месте, поэтому сохраняются ресурсы, такие как общедоступные IP-адреса и содержимое, относящееся к конкретному узлу.

![Счетчик перезагрузки на портале][portal-restart]

Если проверка актуальности постоянно завершается сбоем и активирует слишком много перезапусков, контейнер переходит в экспоненциальную задержку.

## <a name="liveness-probes-and-restart-policies"></a>Пробы активности и политики перезагрузки

Политики перезагрузки заменяют поведение перезагрузки, активируемое пробами активности. Например, если задать `restartPolicy = Never` *и* проверку актуальности, Группа контейнеров не будет перезапущена из-за неудачной проверки на активное время. Вместо этого Группа контейнеров соответствует политике перезапуска группы контейнеров `Never`.

## <a name="next-steps"></a>Дальнейшие действия

Для выполнения сценариев на основе задачи может понадобиться, чтобы пробы активности включали функцию автоматической перезагрузки в случае, если необходимая функция не работает должным образом. Дополнительные сведения о запуске контейнеров на основе задач см. статье [Выполнение задачи-контейнера в службе "Экземпляры контейнеров Azure"](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
