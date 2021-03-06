---
title: Обновления базовых образов — задачи
description: Сведения об основных образах для образов контейнеров приложений, а также о том, как обновление базового образа может вызвать задачу реестра контейнеров Azure.
ms.topic: article
ms.date: 01/22/2019
ms.openlocfilehash: 017c8f8a3a15896bd6e14a54136ba713e9f9c499
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77617935"
---
# <a name="about-base-image-updates-for-acr-tasks"></a>Основные сведения об обновлениях базовых образов для задач контроля доступа

В этой статье содержатся общие сведения об обновлениях для базового образа приложения и о том, как эти обновления могут активировать задачу реестра контейнеров Azure.

## <a name="what-are-base-images"></a>Что такое базовые образы?

Файлы dockerfile, определяющие большинство образов контейнеров, указывают родительский образ, на основе которого основан образ, часто называемый *базовым образом*. Базовые образы обычно содержат операционную систему, например [Alpine Linux][base-alpine] или [Windows Nano Server][base-windows], к которой применяются остальные слои контейнера. Они также могут включать в себя платформы приложений, например [Node.js][base-node] или [.NET Core][base-dotnet]. Эти базовые образы обычно основаны на общедоступных восходящих образах. Несколько образов вашего приложения могут совместно использовать общий базовый образ.

Разработчик образов часто обновляет базовый образ, чтобы включить новые функции или улучшения в ОС или платформу в образе. Исправления безопасности — еще одна распространенная причина обновления базового образа. При возникновении этих вышестоящего обновления необходимо также обновить базовые образы, включив в них критическое исправление. Затем необходимо повторно выполнить сборку каждого образа приложения, чтобы внести эти вышестоящие исправления, внесенные в базовый образ.

В некоторых случаях, таких как частная группа разработки, базовый образ может указывать больше ОС или Framework. Например, базовый образ может быть образом общего образа компонента службы, который необходимо отслеживанию. Участникам команды может потребоваться отвести этот базовый образ для тестирования или регулярно обновлять образ при разработке образов приложений.

## <a name="track-base-image-updates"></a>Мониторинг обновлений базовых образов

Решение "Задачи ACR" включают в себя возможность автоматически создавать образы при обновлении базового образа контейнера.

Задачи записи контроля доступа динамически обнаруживают зависимости базовых образов при построении образа контейнера. В результате он может определить, когда обновляется базовый образ образа приложения. При использовании одной предварительно настроенной задачи сборки задачи записи контроля доступа могут автоматически перестраивать каждый образ приложения, ссылающийся на базовый образ. Благодаря такому автоматическому обнаружению и повторному выполнению сборок служба "Задачи ACR" экономит время и усилия, необходимые для того, чтобы вручную отслеживать и обновлять каждый образ приложения, ссылающийся на обновленный базовый образ.

## <a name="base-image-locations"></a>Расположения базовых образов

При использовании сборок образов из Dockerfile задача ACR обнаружит зависимости базовых образов в следующих расположениях:

* реестр контейнеров Azure, в котором выполняется задача;
* Другой частный реестр контейнеров Azure в том же или другом регионе 
* общедоступный репозиторий в Docker Hub; 
* общедоступный репозиторий в Реестре контейнеров Azure.

Если базовый образ, указанный в `FROM` инструкции, находится в одном из этих расположений, задача записи контроля доступа добавляет обработчик, чтобы гарантировать, что изображение будет перестроено во время обновления базы.

## <a name="additional-considerations"></a>Дополнительные сведения

* **Базовые образы для образов приложений** . в настоящее время задача записи контроля доступа отслеживает только обновления базовых образов для образов приложений (*сред выполнения*). Они не отслеживают обновления базового образа для промежуточных образов (*время сборки*), которые используются в многоэтапных файлах Dockerfile.  

* **Включено по умолчанию** . при создании задачи контроля учетных записей с помощью команды [AZ запись контроля][az-acr-task-create] доступа задача по умолчанию *включается* для триггера обновлением базового образа. То есть свойству `base-image-trigger-enabled` присвоено значение "True". Если нужно отключить это поведение в задаче, измените свойство на "False". Например, выполните следующую команду [az acr task update][az-acr-task-update]:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* **Триггер для трассировки зависимостей** . позволяет задаче записи контроля доступа определять и относить зависимости образа контейнера, включая его базовый образ, поэтому необходимо сначала запустить задачу для сборки образа **по крайней мере один раз**. Например, запустите задачу вручную с помощью команды [az acr task run][az-acr-task-run].

* **Стабильный тег для базового образа** . чтобы активировать задачу в базовом обновлении образа, базовый образ должен иметь *стабильный* тег, например `node:9-alpine`. Эта расстановка тегов типична для базового образа, который обновляется с помощью исправлений платформы и ОС до последней стабильной версии. Если базовый образ обновляется с помощью нового тега версии, он не запускает задачу. Подробнее о добавлении тегов к образам см. [Расстановка тегов в Docker: рекомендации для расстановки тегов и управления версиями образов Docker](container-registry-image-tag-version.md). 

* **Другие триггеры задач** . в задаче, активируемой обновлениями базовых образов, можно также включить триггеры на основе [фиксации исходного кода](container-registry-tutorial-build-task.md) или [расписания](container-registry-tasks-scheduled.md). Обновление базового образа также может вызвать [многошаговую задачу](container-registry-tasks-multi-step.md).

## <a name="next-steps"></a>Дальнейшие шаги

В следующих руководствах содержатся сценарии для автоматизации сборок образа приложения после обновления базового образа.

* [Автоматизация сборок образа контейнера при обновлении базового образа в том же реестре](container-registry-tutorial-base-image-update.md)

* [Автоматизация сборки образа контейнера при обновлении базового образа в другом реестре](container-registry-tutorial-base-image-update.md)


<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
