---
title: Краткое руководство. созданию многоконтейнерного приложения
description: Начните работу с многоконтейнерными приложениями в Службе приложений Azure, развернув первое многоконтейнерное приложение.
keywords: служба приложений azure, веб-приложение, linux, docker, compose, многоконтейнерное, несколько контейнеров, веб-приложение для контейнеров, много контейнеров, контейнер, wordpress, azure db для mysql, рабочая база данных с контейнерами
author: msangapu-msft
ms.topic: quickstart
ms.date: 08/23/2019
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: ab8e416e460546dcf758c83aa8b357139c6dbefb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82085881"
---
# <a name="create-a-multi-container-preview-app-using-a-docker-compose-configuration"></a>Создание многоконтейнерного приложения (предварительная версия) с использованием конфигурации Docker Compose

> [!NOTE]
> Многоконтейнерное приложение поддерживается в предварительной версии.

Платформа [Веб-приложение для контейнеров](app-service-linux-intro.md) предоставляет гибкие возможности для использования образов Docker. В этом кратком руководстве показано, как в [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) развернуть многоконтейнерное приложение (предварительная версия) на платформе "Веб-приложение для контейнеров", используя конфигурацию Docker Compose.

Действия, описанные в этом руководстве, выполняются в Cloud Shell, но эти же команды можно выполнить локально в [Azure CLI](/cli/azure/install-azure-cli) (2.0.32 или более поздней версии). 

![Пример многоконтейнерного приложения на платформе "Веб-приложение для контейнеров"][1]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Скачивание примера приложения

В этом кратком руководстве используется файл compose из [Docker](https://docs.docker.com/compose/wordpress/#define-the-project). Файл конфигурации можно найти в [репозитории примеров для Azure](https://github.com/Azure-Samples/multicontainerwordpress).

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

В Cloud Shell создайте каталог quickstart и перейдите в него.

```bash
mkdir quickstart

cd $HOME/quickstart
```

Затем выполните следующую команду, чтобы клонировать репозиторий с примером приложения в локальный каталог quickstart. Затем перейдите в каталог `multicontainerwordpress`.

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress

cd multicontainerwordpress
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

В Cloud Shell создайте группу ресурсов с помощью команды [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create). В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *Центрально-южная часть США*. Чтобы просмотреть все поддерживаемые расположения для службы приложений в Linux на уровне **Стандартный**, выполните команду [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations).

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

Обычно группа ресурсов и ресурсы создаются в ближайших регионах.

По завершении команды в выходных данных JSON будут отображаться свойства группы ресурсов.

## <a name="create-an-azure-app-service-plan"></a>Создание плана службы приложений Azure

В Cloud Shell создайте план службы приложений в группе ресурсов с помощью команды [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create).

В следующем примере создается план службы приложений с именем `myAppServicePlan` в ценовой категории **Стандартный** (`--sku S1`) в контейнере Linux (`--is-linux`).

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

После создания плана службы приложений в Azure CLI отображается информация следующего вида:

<pre>
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
</pre>

## <a name="create-a-docker-compose-app"></a>Создание приложения Docker Compose

> [!NOTE]
> Для Docker Compose в Службах приложений Azure сейчас настроено ограничение в 4000 символов.

В терминале Cloud Shell создайте многоконтейнерное [веб-приложение](app-service-linux-intro.md) в рамках плана службы приложений `myAppServicePlan` с помощью команды [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Замените _\<app_name>_ уникальным именем приложения (допустимые символы: `a-z`, `0-9` и `-`).

```azurecli
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Когда веб-приложение будет создано, в Azure CLI отобразится примерно следующее:

<pre>
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;app_name&gt;.azurewebsites.net",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

### <a name="browse-to-the-app"></a>Переход в приложение

Перейдите в развернутое приложение по адресу `http://<app_name>.azurewebsites.net`. Для загрузки приложения может потребоваться несколько минут. Если произойдет ошибка, подождите несколько минут, а затем обновите страницу в браузере.

![Пример многоконтейнерного приложения на платформе "Веб-приложение для контейнеров"][1]

**Поздравляем!** Вы создали многоконтейнерное приложение на платформе "Веб-приложение для контейнеров".

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство. по приложению WordPress с несколькими контейнерами](tutorial-multi-container-app.md)

> [!div class="nextstepaction"]
> [Настройка пользовательского контейнера](configure-custom-container.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png
