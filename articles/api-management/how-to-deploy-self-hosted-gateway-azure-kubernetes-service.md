---
title: Развертывание автономного шлюза в службе Kubernetes Azure | Документация Майкрософт
description: Узнайте, как развернуть автономный компонент шлюза службы управления Azure API в службе Kubernetes Azure.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.topic: article
ms.date: 04/26/2020
ms.author: apimpm
ms.openlocfilehash: abf7e6d7032e7e5dc35b2f4397a5630d45d762de
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82205122"
---
# <a name="deploy-to-azure-kubernetes-service"></a>Развертывание в Службе Azure Kubernetes

В этой статье описаны действия по развертыванию компонента самостоятельно размещенного шлюза службы управления API Azure в [службе Azure Kubernetes](https://azure.microsoft.com/services/kubernetes-service/). Сведения о развертывании самостоятельно размещенного шлюза в кластере Kubernetes см. в этом[документе](how-to-deploy-self-hosted-gateway-kubernetes.md).

## <a name="prerequisites"></a>Предварительные условия

- [Создание экземпляра службы управления API Azure](get-started-create-service-instance.md)
- [Создание кластера Azure Kubernetes](../aks/kubernetes-walkthrough-portal.md)
- [Подготавливает ресурс шлюза в экземпляре управления API](api-management-howto-provision-self-hosted-gateway.md).

## <a name="deploy-the-self-hosted-gateway-to-aks"></a>Развертывание самостоятельно размещенного шлюза в AKS

1. В разделе **Параметры**выберите **шлюзы** .
2. Выберите ресурс самостоятельно размещенного шлюза, который планируется развернуть.
3. Выберите **развертывание**.
4. Обратите внимание, что новый маркер в текстовом поле **токен** был создан автоматически при использовании значений **срока действия** по умолчанию и **секретного ключа** . При необходимости настройте один или оба варианта и нажмите кнопку **создать** , чтобы создать новый маркер.
5. Убедитесь, что в разделе **сценарии развертывания**выбрано **Kubernetes** .
6. Выберите **<шлюз-name>. yml** ссылка на файл рядом с **развернутой** , чтобы скачать файл.
7. При необходимости измените сопоставления портов и имени контейнера в файле yml.
8. В зависимости от сценария может потребоваться изменить [тип службы](https://docs.microsoft.com/azure/aks/concepts-network#services). Значение по умолчанию — `NodePort`.
9. Щелкните значок **копирования** , расположенный в правой части текстового поля **развернуть** , чтобы сохранить `kubectl` команду в буфер обмена.
10. Вставьте команду в окно терминала (или команду). Обратите внимание, что команда ждет, что загруженный файл окружения будет находиться в текущем каталоге.
```console
    kubectl apply -f <gateway-name>.yaml
```
11. Выполните команду. Команда предписывает кластеру AKS запустить контейнер, использовать образ самостоятельно размещенного шлюза, скачанного из реестра контейнеров Майкрософт, и настроить контейнер для предоставления портов HTTP (8080) и HTTPS (443).
12. Выполните приведенную ниже команду, чтобы проверить, запущен ли модуль шлюза. Обратите внимание, что имя Pod будет отличаться.
```console
kubectl get pods
NAME                                   READY     STATUS    RESTARTS   AGE
contoso-apim-gateway-59f5fb94c-s9stz   1/1       Running   0          1m
```
13. Выполните приведенную ниже команду, чтобы проверить, работает ли служба шлюза. Обратите внимание, что имя службы и IP-адреса будут отличаться.
```console
kubectl get services
NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
contosogateway   NodePort    10.110.230.87   <none>        80:32504/TCP,443:30043/TCP   1m
```
14. Вернитесь к портал Azure и убедитесь, что только что развернутый узел шлюза сообщает о работоспособности.

> [!TIP]
> Используйте <code>kubectl logs <gateway-pod-name></code> команду для просмотра моментального снимка журнала автономного шлюза.

## <a name="next-steps"></a>Дальнейшие шаги

* Дополнительные сведения о самостоятельно размещенном шлюзе см. в статье [Обзор самостоятельного размещения шлюза в службе управления API Azure](self-hosted-gateway-overview.md) .
* Дополнительные сведения о [службе Kubernetes Azure](https://docs.microsoft.com/azure/aks/intro-kubernetes)
* Сведения [о настройке и сохранении журналов в облаке](how-to-configure-cloud-metrics-logs.md)
* * Сведения [о настройке и сохранении журналов в локальной](how-to-configure-local-metrics-logs.md) среде




