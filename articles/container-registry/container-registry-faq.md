---
title: Часто задаваемые вопросы
description: Ответы на часто задаваемые вопросы, связанные со службой реестра контейнеров Azure
author: sajayantony
ms.topic: article
ms.date: 03/18/2020
ms.author: sajaya
ms.openlocfilehash: 39b543c5f886b22d488198873b75cf76555692fa
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2020
ms.locfileid: "82731650"
---
# <a name="frequently-asked-questions-about-azure-container-registry"></a>Часто задаваемые вопросы о реестре контейнеров Azure

В этой статье рассматриваются часто задаваемые вопросы и известные проблемы реестра контейнеров Azure.

## <a name="resource-management"></a>Управление ресурсами

- [Можно ли создать реестр контейнеров Azure с помощью шаблона диспетчер ресурсов?](#can-i-create-an-azure-container-registry-using-a-resource-manager-template)
- [Есть ли проверка уязвимости системы безопасности на наличие образов в записи контроля доступа?](#is-there-security-vulnerability-scanning-for-images-in-acr)
- [Разделы справки настроить Kubernetes с помощью реестра контейнеров Azure?](#how-do-i-configure-kubernetes-with-azure-container-registry)
- [Разделы справки получить учетные данные администратора для реестра контейнеров?](#how-do-i-get-admin-credentials-for-a-container-registry)
- [Разделы справки получить учетные данные администратора в шаблоне диспетчер ресурсов?](#how-do-i-get-admin-credentials-in-a-resource-manager-template)
- [Удаление репликации завершается сбоем с запрещенным состоянием, хотя репликация удаляется с помощью Azure CLI или Azure PowerShell](#delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell)
- [Правила брандмауэра успешно обновлены, но они не вступают в силу](#firewall-rules-are-updated-successfully-but-they-do-not-take-effect)

### <a name="can-i-create-an-azure-container-registry-using-a-resource-manager-template"></a>Можно ли создать реестр контейнеров Azure с помощью шаблона диспетчер ресурсов?

Да. Ниже приведен [шаблон](https://github.com/Azure/azure-quickstart-templates/tree/master/101-container-registry) , который можно использовать для создания реестра.

### <a name="is-there-security-vulnerability-scanning-for-images-in-acr"></a>Есть ли проверка уязвимости системы безопасности на наличие образов в записи контроля доступа?

Да. См. документацию из [центра безопасности Azure](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration), [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry/) и [голубого](https://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry).

### <a name="how-do-i-configure-kubernetes-with-azure-container-registry"></a>Разделы справки настроить Kubernetes с помощью реестра контейнеров Azure?

См. документацию по [Kubernetes](https://kubernetes.io/docs/user-guide/images/#using-azure-container-registry-acr) и действиям для [службы Kubernetes Azure](../aks/cluster-container-registry-integration.md).

### <a name="how-do-i-get-admin-credentials-for-a-container-registry"></a>Разделы справки получить учетные данные администратора для реестра контейнеров?

> [!IMPORTANT]
> Учетная запись администратора разработана для доступа одного пользователя к реестру, в основном для целей тестирования. Не рекомендуется совместное использование учетных данных администратора несколькими пользователями. Для пользователей и субъектов-служб в сценариях автоматического входа рекомендуется использовать отдельные удостоверения. См. раздел [Обзор проверки подлинности](container-registry-authentication.md).

Перед получением учетных данных администратора убедитесь, что включен администратор реестра.

Чтобы получить учетные данные с помощью Azure CLI:

```azurecli
az acr credential show -n myRegistry
```

С помощью Azure PowerShell:

```powershell
Invoke-AzureRmResourceAction -Action listCredentials -ResourceType Microsoft.ContainerRegistry/registries -ResourceGroupName myResourceGroup -ResourceName myRegistry
```

### <a name="how-do-i-get-admin-credentials-in-a-resource-manager-template"></a>Разделы справки получить учетные данные администратора в шаблоне диспетчер ресурсов?

> [!IMPORTANT]
> Учетная запись администратора разработана для доступа одного пользователя к реестру, в основном для целей тестирования. Не рекомендуется совместное использование учетных данных администратора несколькими пользователями. Для пользователей и субъектов-служб в сценариях автоматического входа рекомендуется использовать отдельные удостоверения. См. раздел [Обзор проверки подлинности](container-registry-authentication.md).

Перед получением учетных данных администратора убедитесь, что включен администратор реестра.

Чтобы получить первый пароль, сделайте следующее:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[0].value]"
}
```

Чтобы получить второй пароль, сделайте следующее:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[1].value]"
}
```

### <a name="delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell"></a>Удаление репликации завершается сбоем с запрещенным состоянием, хотя репликация удаляется с помощью Azure CLI или Azure PowerShell

Эта ошибка возникает, когда пользователь имеет разрешения на доступ к реестру, но не имеет разрешений уровня чтения для подписки. Чтобы устранить эту проблему, назначьте пользователю разрешения читателя для подписки:


```azurecli  
az role assignment create --role "Reader" --assignee user@contoso.com --scope /subscriptions/<subscription_id> 
```

### <a name="firewall-rules-are-updated-successfully-but-they-do-not-take-effect"></a>Правила брандмауэра успешно обновлены, но они не вступают в силу

Распространение изменений правил брандмауэра занимает некоторое время. После изменения параметров брандмауэра подождите несколько минут, прежде чем проверять это изменение.


## <a name="registry-operations"></a>Операции с реестром

- [Разделы справки доступ к API HTTP версии 2 реестра DOCKER?](#how-do-i-access-docker-registry-http-api-v2)
- [Разделы справки удалить все манифесты, на которые не ссылаются какие-либо теги в репозитории?](#how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository)
- [Почему использование квоты реестра не уменьшается после удаления образов?](#why-does-the-registry-quota-usage-not-reduce-after-deleting-images)
- [Разделы справки проверить изменения квоты хранилища?](#how-do-i-validate-storage-quota-changes)
- [Разделы справки выполнить проверку подлинности с помощью реестра при запуске CLI в контейнере?](#how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container)
- [Как включить TLS 1,2?](#how-to-enable-tls-12)
- [Поддерживает ли реестр контейнеров Azure отношение доверия с содержимым?](#does-azure-container-registry-support-content-trust)
- [Разделы справки предоставить доступ к образам или Push-уведомлениям без разрешения на управление ресурсом реестра?](#how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource)
- [Разделы справки включить автоматическое помещение образа в карантин для реестра?](#how-do-i-enable-automatic-image-quarantine-for-a-registry)
- [Разделы справки включить анонимный доступ по запросу?](#how-do-i-enable-anonymous-pull-access)

### <a name="how-do-i-access-docker-registry-http-api-v2"></a>Разделы справки доступ к API HTTP версии 2 реестра DOCKER?

Запись контроля доступа поддерживает HTTP API версии 2 реестра DOCKER. Доступ к API можно получить по `https://<your registry login server>/v2/`адресу. Пример: `https://mycontainerregistry.azurecr.io/v2/`

### <a name="how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository"></a>Разделы справки удалить все манифесты, на которые не ссылаются какие-либо теги в репозитории?

Если вы используете bash:

```azurecli
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv  | xargs -I% az acr repository delete -n myRegistry -t myRepository@%
```

Для PowerShell:

```azurecli
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv | %{ az acr repository delete -n myRegistry -t myRepository@$_ }
```

Примечание. чтобы пропустить подтверждение `-y` , можно добавить команду Delete.

Дополнительные сведения см. [в статье Удаление образов контейнеров в реестре контейнеров Azure](container-registry-delete.md).

### <a name="why-does-the-registry-quota-usage-not-reduce-after-deleting-images"></a>Почему использование квоты реестра не уменьшается после удаления образов?

Такая ситуация может возникнуть, если на базовые слои по-прежнему ссылаются другие образы контейнеров. Если удалить образ без ссылок, использование реестра будет обновляться через несколько минут.

### <a name="how-do-i-validate-storage-quota-changes"></a>Разделы справки проверить изменения квоты хранилища?

Создайте образ с уровнем 1 ГБ, используя следующий файл DOCKER. Это гарантирует, что изображение будет иметь слой, который не является общим для любого другого образа в реестре.

```dockerfile
FROM alpine
RUN dd if=/dev/urandom of=1GB.bin  bs=32M  count=32
RUN ls -lh 1GB.bin
```

Создайте образ и отправьте его в реестр с помощью DOCKER CLI.

```bash
docker build -t myregistry.azurecr.io/1gb:latest .
docker push myregistry.azurecr.io/1gb:latest
```

Вы увидите, что использование хранилища увеличилось в портал Azure, или вы можете запросить использование с помощью интерфейса командной строки.

```azurecli
az acr show-usage -n myregistry
```

Удалите образ с помощью Azure CLI или портала и проверьте обновленное использование через несколько минут.

```azurecli
az acr repository delete -n myregistry --image 1gb
```

### <a name="how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container"></a>Разделы справки выполнить проверку подлинности с помощью реестра при запуске CLI в контейнере?

Необходимо запустить контейнер Azure CLI, подключив сокет docker:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock azuresdk/azure-cli-python:dev
```

В контейнере установите `docker`:

```bash
apk --update add docker
```

Затем выполните аутентификацию в реестре:

```azurecli
az acr login -n MyRegistry
```

### <a name="how-to-enable-tls-12"></a>Как включить TLS 1,2?

Включите TLS 1,2 с помощью любого последнего клиента DOCKER (версия 18.03.0 и выше). 

> [!IMPORTANT]
> С 13 января 2020 года Реестр контейнеров Azure будет требовать использовать протокол TLS версии 1.2 для всех безопасных подключений серверов и приложений. Поддержка протоколов TLS версии 1.0 и 1.1 будет прекращена.

### <a name="does-azure-container-registry-support-content-trust"></a>Поддерживает ли реестр контейнеров Azure отношение доверия с содержимым?

Да, вы можете использовать надежные образы в реестре контейнеров Azure, так как [DOCKER Нотари](https://docs.docker.com/notary/getting_started/) интегрирован и может быть включен. Дополнительные сведения см. [в статье о доверии содержимого в реестре контейнеров Azure](container-registry-content-trust.md).


####  <a name="where-is-the-file-for-the-thumbprint-located"></a>Где находится файл для отпечатка?

В `~/.docker/trust/tuf/myregistry.azurecr.io/myrepository/metadata`разделе:

* Открытые ключи и сертификаты всех ролей (за исключением ролей делегирования) хранятся в `root.json`.
* Открытые ключи и сертификаты роли делегирования хранятся в JSON-файле своей родительской роли (например `targets.json` , `targets/releases` для роли).

Рекомендуется проверить эти открытые ключи и сертификаты после общей проверки туф, выполненной клиентом DOCKER и Нотари.

### <a name="how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource"></a>Разделы справки предоставить доступ к образам или Push-уведомлениям без разрешения на управление ресурсом реестра?

Запись контроля доступа поддерживает [пользовательские роли](container-registry-roles.md) , предоставляющие различные уровни разрешений. В частности `AcrPull` , `AcrPush` роли позволяют пользователям получать и (или) отправлять изображения без разрешения на управление ресурсом реестра в Azure.

* Портал Azure. Реестр — > управления доступом (IAM) — > добавить (выберите `AcrPull` или `AcrPush` для роли).
* Azure CLI. Найдите идентификатор ресурса реестра, выполнив следующую команду:

  ```azurecli
  az acr show -n myRegistry
  ```
  
  Затем можно назначить роль `AcrPull` или `AcrPush` пользователю (в следующем примере используется `AcrPull`):

  ```azurecli
  az role assignment create --scope resource_id --role AcrPull --assignee user@example.com
  ```

  Или назначьте роль субъекту-службе, определяемому ИДЕНТИФИКАТОРом приложения:

  ```azurecli
  az role assignment create --scope resource_id --role AcrPull --assignee 00000000-0000-0000-0000-000000000000
  ```

Затем уполномоченные могут проходить проверку подлинности и получать доступ к изображениям в реестре.

* Для проверки подлинности в реестре:
    
  ```azurecli
  az acr login -n myRegistry 
  ```

* Вывод списка репозиториев:

  ```azurecli
  az acr repository list -n myRegistry
  ```

* Чтобы извлечь изображение, сделайте следующее:

  ```bash
  docker pull myregistry.azurecr.io/hello-world
  ```

При использовании только роли `AcrPull` или `AcrPush` у уполномоченного объекта нет разрешения на управление ресурсом реестра в Azure. Например, `az acr list` или `az acr show -n myRegistry` не будет показывать реестр.

### <a name="how-do-i-enable-automatic-image-quarantine-for-a-registry"></a>Разделы справки включить автоматическое помещение образа в карантин для реестра?

Карантин изображений в настоящее время является предварительной версией функции записи контроля доступа. Можно включить режим карантина для реестра, чтобы только те образы, которые успешно прошли проверку безопасности, были видны обычным пользователям. Дополнительные сведения см. в [репозитории GitHub](https://github.com/Azure/acr/tree/master/docs/preview/quarantine)для записи контроля доступа.

### <a name="how-do-i-enable-anonymous-pull-access"></a>Разделы справки включить анонимный доступ по запросу?

Настройка реестра контейнеров Azure для анонимного доступа через запрос на вытягивание в настоящее время является функцией предварительной версии. Чтобы включить общий доступ, отправьте запрос в службу поддержки по https://aka.ms/acr/support/create-ticketадресу. Дополнительные сведения см. на [форуме обратной связи Azure](https://feedback.azure.com/forums/903958-azure-container-registry/suggestions/32517127-enable-anonymous-access-to-registries).


## <a name="diagnostics-and-health-checks"></a>Диагностика и проверка работоспособности

- [Проверить работоспособность с помощью`az acr check-health`](#check-health-with-az-acr-check-health)
- [Сбой docker pull с ошибкой: NET/http: запрос отменен при ожидании соединения (превышение времени ожидания клиента.](#docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers)
- [Отправка DOCKER завершается успешно, но docker pull завершается ошибкой: не санкционировано: требуется проверка подлинности](#docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required)
- [`az acr login`успешно, но команды DOCKER завершаются ошибкой: не санкционировано: требуется проверка подлинности](#az-acr-login-succeeds-but-docker-fails-with-error-unauthorized-authentication-required)
- [Включение и получение журналов отладки управляющей программы DOCKER](#enable-and-get-the-debug-logs-of-the-docker-daemon)    
- [Новые разрешения пользователя могут не действовать сразу после обновления](#new-user-permissions-may-not-be-effective-immediately-after-updating)
- [Данные проверки подлинности не задаются в правильном формате при вызове Direct REST API](#authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls)
- [Почему портал Azure не содержит список всех репозиториев или тегов?](#why-does-the-azure-portal-not-list-all-my-repositories-or-tags)
- [Почему портал Azure не удается получить репозитории или Теги?](#why-does-the-azure-portal-fail-to-fetch-repositories-or-tags)
- [Почему запрос на вытягивание или принудительную отправку завершился с неразрешенной операцией?](#why-does-my-pull-or-push-request-fail-with-disallowed-operation)
- [Разделы справки собирайте трассировки HTTP в Windows?](#how-do-i-collect-http-traces-on-windows)

### <a name="check-health-with-az-acr-check-health"></a>Проверить работоспособность с помощью`az acr check-health`

Сведения об устранении распространенных проблем среды и реестра см. в статье [Проверка работоспособности реестра контейнеров Azure](container-registry-check-health.md).

### <a name="docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers"></a>Сбой docker pull с ошибкой: NET/http: запрос отменен при ожидании соединения (превышение времени ожидания клиента.

 - Если эта ошибка является временной, повторная попытка будет выполнена.
 - Если `docker pull` сбой постоянно завершается, возможно, возникла проблема с управляющей программой DOCKER. Как правило, проблему можно устранить путем перезапуска управляющей программы DOCKER. 
 - Если эта проблема продолжает возникать после перезапуска управляющей программы DOCKER, проблема может быть связана с неполадками с сетевым подключением к компьютеру. Чтобы проверить работоспособность общей сети на компьютере, выполните следующую команду, чтобы проверить подключение к конечной точке. Минимальная `az acr` версия, содержащая эту команду проверки подключения, — 2.2.9. Обновите Azure CLI, если используется более старая версия.
 
  ```azurecli
  az acr check-health -n myRegistry
  ```

 - Всегда следует использовать механизм повтора во всех операциях клиента DOCKER.

### <a name="docker-pull-is-slow"></a>Слишком большое извлечение DOCKER
Используйте [это](http://www.azurespeed.com/Azure/Download) средство для проверки скорости загрузки сети компьютера. Если сеть компьютера работает слишком долго, попробуйте использовать виртуальную машину Azure в том же регионе, что и реестр. Обычно это обеспечивает более быструю скорость сети.

### <a name="docker-push-is-slow"></a>Задержка принудительной отправки DOCKER
Используйте [это](http://www.azurespeed.com/Azure/Upload) средство для проверки скорости передачи сети компьютера. Если сеть компьютера работает слишком долго, попробуйте использовать виртуальную машину Azure в том же регионе, что и реестр. Обычно это обеспечивает более быструю скорость сети.

### <a name="docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required"></a>Отправка DOCKER завершается успешно, но docker pull завершается ошибкой: не санкционировано: требуется проверка подлинности

Эта ошибка может возникать при использовании версии Red Hat управляющей программы DOCKER, где `--signature-verification` включена по умолчанию. Можно проверить параметры управляющей программы DOCKER для Red Hat Enterprise Linux (RHEL) или Fedora, выполнив следующую команду:

```bash
grep OPTIONS /etc/sysconfig/docker
```

Например, Fedora 28 Server имеет следующие параметры управляющей программы docker:

`OPTIONS='--selinux-enabled --log-driver=journald --live-restore'`

В `--signature-verification=false` `docker pull` случае отсутствия происходит сбой со следующим сообщением:

```output
Trying to pull repository myregistry.azurecr.io/myimage ...
unauthorized: authentication required
```

Чтобы устранить эту ошибку, сделайте следующее:
1. Добавьте параметр `--signature-verification=false` в файл `/etc/sysconfig/docker`конфигурации управляющей программы DOCKER. Пример:
   
   `OPTIONS='--selinux-enabled --log-driver=journald --live-restore --signature-verification=false'`
   
2. Перезапустите службу управляющей программы DOCKER, выполнив следующую команду:
   
   ```bash
   sudo systemctl restart docker.service
   ```

Подробные сведения `--signature-verification` о них можно найти, `man dockerd`выполнив.

### <a name="az-acr-login-succeeds-but-docker-fails-with-error-unauthorized-authentication-required"></a>AZ запись контроля доступа завершается успешно, но DOCKER завершается с ошибкой: не санкционировано: требуется проверка подлинности

Убедитесь, что используется URL-адрес сервера со всеми строчными буквами, например `docker push myregistry.azurecr.io/myimage:latest`, даже если имя ресурса реестра имеет регистр букв в верхнем или `myRegistry`смешанном регистре, например.

### <a name="enable-and-get-the-debug-logs-of-the-docker-daemon"></a>Включение и получение журналов отладки управляющей программы DOCKER    

Начните `dockerd` с `debug` параметра. Сначала создайте файл конфигурации управляющей программы DOCKER (`/etc/docker/daemon.json`), если он не существует, и добавьте `debug` параметр:

```json
{    
    "debug": true    
}
```

Затем перезапустите управляющую программу. Например, в Ubuntu 14,04:

```bash
sudo service docker restart
```

Подробные сведения можно найти в [документации по DOCKER](https://docs.docker.com/engine/admin/#enable-debugging).    

 * Журналы могут быть созданы в разных местах в зависимости от используемой системы. Например, для Ubuntu 14,04 это `/var/log/upstart/docker.log`.    
Дополнительные сведения см. в [документации по DOCKER](https://docs.docker.com/engine/admin/#read-the-logs) .    

 * Для Docker для Windows журналы создаются в папке% LOCALAPPDATA%/доккер/. Однако он еще не может содержать всю отладочную информацию.    

   Для доступа к полному журналу управляющей программы могут потребоваться некоторые дополнительные действия.

    ```console
    docker run --privileged -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/local/bin/docker alpine sh

    docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/host alpine /bin/sh
    chroot /host
    ```
    Теперь у вас есть доступ ко всем файлам виртуальной машины, на `dockerd`которых выполняется. Журнал находится в `/var/log/docker.log`.

### <a name="new-user-permissions-may-not-be-effective-immediately-after-updating"></a>Новые разрешения пользователя могут не действовать сразу после обновления

При предоставлении новых разрешений (новых ролей) субъекту-службе изменение может не вступить в силу немедленно. Существуют две возможные причины этой ошибки.

* Задержка назначения роли Azure Active Directory. Обычно это происходит быстро, но может занять несколько минут из-за задержки распространения.
* Задержка разрешений на сервере маркеров записи контроля доступа. Этот процесс может занять больше 10 минут. Чтобы устранить эту проблемы, можно `docker logout` снова выполнить проверку подлинности с тем же пользователем через 1 минуту:

  ```bash
  docker logout myregistry.azurecr.io
  docker login myregistry.azurecr.io
  ```

В настоящее время запись контроля доступа пользователей не поддерживает удаление домашних репликаций. Обходной путь состоит в том, чтобы включить в шаблон домашнюю репликацию, но пропустить ее `"condition": false` создание, добавив, как показано ниже:

```json
{
    "name": "[concat(parameters('acrName'), '/', parameters('location'))]",
    "condition": false,
    "type": "Microsoft.ContainerRegistry/registries/replications",
    "apiVersion": "2017-10-01",
    "location": "[parameters('location')]",
    "properties": {},
    "dependsOn": [
        "[concat('Microsoft.ContainerRegistry/registries/', parameters('acrName'))]"
     ]
},
```

### <a name="authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls"></a>Данные проверки подлинности не задаются в правильном формате при вызове Direct REST API

Может возникнуть `InvalidAuthenticationInfo` ошибка, особенно с помощью `curl` средства с параметром `-L` `--location` (для соблюдения перенаправлений).
Например, можно получить большой двоичный объект `curl` с `-L` помощью параметра with и обычной проверки подлинности:

```bash
curl -L -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest
```

может привести к следующему ответу:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Error><Code>InvalidAuthenticationInfo</Code><Message>Authentication information is not given in the correct format. Check the value of Authorization header.
RequestId:00000000-0000-0000-0000-000000000000
Time:2019-01-01T00:00:00.0000000Z</Message></Error>
```

Основная причина заключается в том, `curl` что некоторые реализации следуют перенаправлениям с заголовками исходного запроса.

Чтобы устранить эту проблему, необходимо следовать перенаправлениям вручную без заголовков. Распечатайте заголовки ответа с `-D -` параметром `curl` , а затем извлеките `Location` : заголовок:

```bash
redirect_url=$(curl -s -D - -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest | grep "^Location: " | cut -d " " -f2 | tr -d '\r')
curl $redirect_url
```

### <a name="why-does-the-azure-portal-not-list-all-my-repositories-or-tags"></a>Почему портал Azure не содержит список всех репозиториев или тегов? 

Если вы используете браузер Microsoft ребр/IE, можно увидеть не более 100 репозиториев или тегов. Если в реестре более 100 репозиториев или тегов, для их перечисления рекомендуется использовать браузер Firefox или Chrome.

### <a name="why-does-the-azure-portal-fail-to-fetch-repositories-or-tags"></a>Почему портал Azure не удается получить репозитории или Теги?

Возможно, браузер не сможет отправить запрос на получение репозиториев или тегов на сервер. Возможны различные причины:

* Отсутствие подключения к сети
* Брандмауэр
* Блокирование рекламы
* Ошибки DNS

Обратитесь к администратору сети или проверьте конфигурацию сети и подключение. Попробуйте выполнить `az acr check-health -n yourRegistry` с помощью Azure CLI, чтобы проверить, может ли ваша среда подключаться к реестру контейнеров. Кроме того, в браузере можно попытаться использовать режиме инкогнито или частный сеанс, чтобы избежать устаревших кэшей браузера или файлов cookie.

### <a name="why-does-my-pull-or-push-request-fail-with-disallowed-operation"></a>Почему запрос на вытягивание или принудительную отправку завершился с неразрешенной операцией?

Ниже приведены некоторые сценарии, в которых операции могут быть запрещены.
* Классические реестры больше не поддерживаются. Обновите до поддерживаемых [SKU](https://aka.ms/acr/skus) с помощью команды [AZ контроля доступа Update](https://docs.microsoft.com/cli/azure/acr?view=azure-cli-latest#az-acr-update) или портал Azure.
* Образ или репозиторий могут быть заблокированы, поэтому их нельзя удалить или обновить. Для просмотра текущих атрибутов можно использовать команду [AZ запись контроля](https://docs.microsoft.com/azure/container-registry/container-registry-image-lock) доступа.
* Некоторые операции запрещены, если образ находится в карантине. Дополнительные сведения о помещении в [Карантин](https://github.com/Azure/acr/tree/master/docs/preview/quarantine).

### <a name="how-do-i-collect-http-traces-on-windows"></a>Разделы справки собирайте трассировки HTTP в Windows?

#### <a name="prerequisites"></a>Предварительные условия

- Включение расшифровки HTTPS в Fiddler:<https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS>
- Включение DOCKER для использования прокси-сервера через пользовательский интерфейс docker:<https://docs.docker.com/docker-for-windows/#proxies>
- Не забудьте выполнить откат после завершения.  DOCKER не будет работать, если этот параметр включен и Fiddler не работает.

#### <a name="windows-containers"></a>Контейнеры Windows

Настройка прокси-сервера DOCKER в значение 127.0.0.1:8888

#### <a name="linux-containers"></a>Контейнеры Linux

Найдите IP-адрес виртуального коммутатора виртуальной машины docker:

```powershell
(Get-NetIPAddress -InterfaceAlias "*Docker*" -AddressFamily IPv4).IPAddress
```

Настройте прокси-сервер DOCKER для вывода предыдущей команды и порта 8888 (например, 10.0.75.1:8888).

## <a name="tasks"></a>Задания

- [Разделы справки выполнения пакетной отмены?](#how-do-i-batch-cancel-runs)
- [Разделы справки включить папку Git в команду AZ контроля доступа?](#how-do-i-include-the-git-folder-in-az-acr-build-command)
- [Поддерживают ли задачи GitLab для исходных триггеров?](#does-tasks-support-gitlab-for-source-triggers)
- [Какая служба управления репозиторием Git поддерживает задачи?](#what-git-repository-management-service-does-tasks-support)

### <a name="how-do-i-batch-cancel-runs"></a>Разделы справки выполнения пакетной отмены?

Следующие команды отменяют все выполняемые задачи в указанном реестре.

```azurecli
az acr task list-runs -r $myregistry --run-status Running --query '[].runId' -o tsv \
| xargs -I% az acr task cancel-run -r $myregistry --run-id %
```

### <a name="how-do-i-include-the-git-folder-in-az-acr-build-command"></a>Разделы справки включить папку Git в команду AZ контроля доступа?

Если передать в `az acr build` команду локальную исходную папку, `.git` папка будет исключена из переданного пакета по умолчанию. Можно создать `.dockerignore` файл со следующим параметром. Она сообщает команде восстановить все файлы `.git` в переданном пакете. 

`!.git/**`

Этот параметр также применяется к `az acr run` команде.

### <a name="does-tasks-support-gitlab-for-source-triggers"></a>Поддерживают ли задачи GitLab для исходных триггеров?

Сейчас мы не поддерживаем GitLab для исходных триггеров.

### <a name="what-git-repository-management-service-does-tasks-support"></a>Какая служба управления репозиторием Git поддерживает задачи?

| Служба Git | Исходный контекст | Сборка вручную | Автоматическая сборка с помощью триггера Commit |
|---|---|---|---|
| GitHub | `https://github.com/user/myapp-repo.git#mybranch:myfolder` | Да | Да |
| Azure Repos | `https://dev.azure.com/user/myproject/_git/myapp-repo#mybranch:myfolder` | Да | Да |
| GitLab | `https://gitlab.com/user/myapp-repo.git#mybranch:myfolder` | Да | нет |
| Bitbucket; | `https://user@bitbucket.org/user/mayapp-repo.git#mybranch:myfolder` | Да | нет |

## <a name="run-error-message-troubleshooting"></a>Выполнение устранения неполадок с сообщением об ошибке

| Сообщение об ошибке | Руководство по устранению неполадок |
|---|---|
|Доступ к виртуальной машине не был настроен, поэтому подписки не найдены|Это может произойти, если вы используете `az login --identity` в ЗАДАЧе контроля доступа. Это временная ошибка, которая возникает, когда не распространяется назначение роли для управляемого удостоверения. Подождите несколько секунд, прежде чем повторять попытку.|

## <a name="cicd-integration"></a>Интеграция CI/CD

- [ЦирклеЦи](https://github.com/Azure/acr/blob/master/docs/integration/CircleCI.md)
- [Действия GitHub](https://github.com/Azure/acr/blob/master/docs/integration/github-actions/github-actions.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Дополнительные сведения](container-registry-intro.md) о реестре контейнеров Azure.
