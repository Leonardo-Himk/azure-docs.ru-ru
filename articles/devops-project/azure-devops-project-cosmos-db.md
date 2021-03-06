---
title: Руководство. Развертывание приложений Node. js на платформе Azure Cosmos DB с помощью Azure DevOps Starter
description: Azure DevOps Starter позволяет легко начать работу в Azure. С помощью DevOps Starter вы можете развернуть приложение Node. js, которое работает под управлением Azure Cosmos DB веб-приложения Windows, за несколько быстрых действий.
ms.author: mlearned
ms.manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 03/24/2020
author: mlearned
ms.openlocfilehash: 07579cf22738e195e3e4ae7a2aa18ffeb885bbe2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82233273"
---
# <a name="deploy-nodejs-apps-powered-by-azure-cosmos-db-with-devops-starter"></a>Развертывание приложений Node. js на платформе Azure Cosmos DB с помощью DevOps Starter

Azure DevOps Starter предлагает упрощенный интерфейс, позволяющий создать конвейер непрерывной интеграции (CI) и непрерывного развертывания (CD) в Azure. Это можно сделать на основе существующего кода и репозитория Git или выбрав пример приложения.

DevOps Starter также:

* автоматически создавать ресурсы Azure, например Azure Cosmos DB, Application Insights и планы Службы приложений;

* создавать и настраивать конвейер выпуска CI/CD в Azure DevOps.

Выполняя данное руководство, вы сделаете следующее:

> [!div class="checklist"]
> * Используйте DevOps Starter для развертывания приложения Node. js на базе Azure Cosmos DB
> * настройка Azure DevOps и подписки Azure;
> * Проверка Azure Cosmos DB
> * Просмотр конвейера CI
> * Просмотр конвейера CD
> * фиксировать изменения в Git и автоматически развертывать их в Azure;
> * очищать ресурсы.

## <a name="prerequisites"></a>Предварительные требования

Вам потребуется подписка Azure, которую вы можете получить бесплатно с помощью [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="use-devops-starter-to-deploy-nodejs-app"></a>Использование DevOps Starter для развертывания приложения Node. js

DevOps Starter создает конвейер CI/CD в Azure Pipelines. Вы можете создать новую организацию Azure DevOps или использовать существующую. DevOps Starter также создает ресурсы Azure, такие как Azure Cosmos DB, Application Insights, служба приложений и планы службы приложений, в подписке Azure по своему усмотрению.

1. Войдите на [портал Azure](https://portal.azure.com).

1. В поле поиска введите **DevOps Starter**, а затем выберите. Нажмите кнопку **Добавить** , чтобы создать новый.

    ![Панель мониторинга DevOps Starter](_img/azure-devops-starter-aks/search-devops-starter.png)

1. Выберите **Node.js** в качестве среды выполнения, а затем нажмите **Далее**. Для параметра **Выберите платформу приложений** выберите **Express.js**.

1. Включите раздел **Добавление базы данных** для **Cosmos DB**, а затем щелкните **Далее**.

    ![Добавление базы данных](_img/azure-devops-project-cosmos-db/add-database.png)

    Azure DevOps Starter поддерживает различные платформы приложений, такие как **Express. js**, **пример приложения Node. js**и **парус. js**. В этом руководстве мы используем **Express.js**.

1. Выберите службу Azure для развертывания приложения и щелкните **Далее**. Вам будут предложены такие варианты, как Веб-приложение Windows, служба Kubernetes и Веб-приложение для контейнеров. В этом руководстве используется **Веб-приложение Windows**.

## <a name="configure-azure-devops-and-azure-subscription"></a>Настройка Azure DevOps и подписки Azure

1. Задайте имя проекта Azure DevOps.

1. Создайте новую организацию Azure DevOps или выберите существующую.

1. Выберите подписку Azure.

1. Чтобы просмотреть дополнительные параметры конфигурации Azure или указать ценовую категорию и расположение, щелкните **Дополнительные параметры**. В этой области отображаются параметры для настройки ценовой категории и расположения служб Azure.

1. Выйдите из области конфигурации Azure и щелкните **Готово**.

1. Через несколько минут процесс будет завершен. Настроенный пример приложения Node.js размещается в репозитории вашей организации Azure DevOps. Затем создаются ресурсы Azure Cosmos DB, Application Insights, Службы приложений и плана Службы приложений, а также конвейер CI/CD. Затем приложение развертывается в Azure.

   После завершения всех этих процессов панель мониторинга Azure DevOps Starter отобразится в портал Azure. Вы также можете перейти на панель мониторинга DevOps Starter из **всех ресурсов** в портал Azure.

   Эта панель мониторинга позволяет просматривать репозиторий кода Azure DevOps, конвейер CI/CD и базу данных Azure Cosmos DB. В конвейере Azure DevOps можно настроить дополнительные параметры CI/CD. В правой части панели мониторинга выберите **Azure Cosmos DB**, чтобы просмотреть доступные варианты.

## <a name="examine-azure-cosmos-db"></a>Проверка Azure Cosmos DB

DevOps Starter автоматически настраивает Azure Cosmos DB, которые можно исследовать и настраивать. Чтобы ознакомиться с работой Azure Cosmos DB, выполните следующие действия.

1. Перейдите на панель мониторинга DevOps Starter.

    ![Панель мониторинга DevOps Projects](_img/azure-devops-project-cosmos-db/devops-starter-dashboard.png)

1. В области справа выберите Azure Cosmos DB. Откроется панель Azure Cosmos DB. В этом представлении можно выполнять различные действия, например мониторинг операций и поиск в журналах.

    ![Панель Azure Cosmos DB](_img/azure-devops-project-cosmos-db/cosmos-db.png)

## <a name="examine-the-ci-pipeline"></a>Просмотр конвейера CI

DevOps Starter автоматически настраивает конвейер CI/CD в Организации Azure DevOps. Вы можете изучить и настроить конвейер. Для этого сделайте следующее:

1. Перейдите на панель мониторинга DevOps Starter.

1. Щелкните гиперссылку в разделе **Сборка**. На вкладке браузера отобразится конвейер сборки нового проекта.

    ![Панель сборки](_img/azure-devops-project-cosmos-db/build.png)

1. Выберите команду **Изменить**. В этой области можно просмотреть различные задачи для конвейера сборки. Сборка выполняет различные задачи, такие как выборка исходного кода из репозитория Git, сборка приложения, выполнение модульных тестов и публикация выходных данных, используемых для развертываний.

1. Выберите **Триггеры**. DevOps Starter автоматически создает триггер CI, и каждая фиксация в репозитории запускает новую сборку. Вы можете включать отдельные ветви в процесс непрерывной интеграции и исключать их

1. Щелкните **Период удержания**. В зависимости от сценария можно указать политики для хранения или удаления определенного количества сборок.

1. Выберите имя конвейера сборки в верхней части соответствующей области.

1. Замените имя конвейера сборки более понятным для себя, и затем выберите **Сохранить** из раскрывающегося меню **Save & queue** (Сохранить и поместить в очередь).

1. Под именем конвейера сборки щелкните **Журнал**. В этой области отображается журнал аудита последних изменений сборки. Azure DevOps отслеживает все изменения, внесенные в конвейер сборки, и позволяет сравнивать версии.

## <a name="examine-the-cd-release-pipeline"></a>Просмотр конвейера выпуска CD

DevOps Starter автоматически создает и настраивает необходимые шаги для развертывания из Организации Azure DevOps в подписке Azure. Эти шаги включают настройку подключения к службе Azure для аутентификации Azure DevOps в подписке Azure. Также автоматически создается конвейер выпуска, обеспечивающий непрерывное развертывание в Azure. Чтобы получить дополнительные сведения о конвейере выпуска, сделайте следующее:

1. Откройте **Конвейеры**, а затем **Выпуски**.

1. Выберите команду **Изменить**.

1. В разделе **Артефакты** выберите **Удалить**. Конвейер сборки, который вы изучали на предыдущих этапах, создаст выходные данные для артефакта.

1. Справа от значка **Удалить** выберите **Триггер непрерывного развертывания**. В этом конвейере выпуска активирован триггер непрерывного развертывания, выполняющий развертывание каждый раз, когда становится доступным новый артефакт сборки. При желании триггер можно отключить, чтобы выполнять развертывание вручную.

1. В области справа выберите раздел **Просмотреть выпуски**, чтобы отобразился журнал выпусков.

1. Щелкните выпуск, чтобы отобразить конвейер. Щелкните любую среду, чтобы проверить сводные данные, фиксации или связанные рабочие элементы для выпуска.

1. Щелкните **Фиксации**. В этом представлении отображаются фиксации кода, связанные с текущим развертыванием. Сравните выпуски, чтобы просмотреть различия между развертываниями.

1. Выберите **Просмотр журналов**. Журналы содержат полезную информацию о процессе развертывания. Их можно просматривать во время и после развертывания.

## <a name="commit-code-changes-and-execute-the-cicd-pipeline"></a>Фиксация изменений в коде и выполнение конвейера CI/CD

> [!NOTE]
> Ниже описана процедура для проверки конвейера CI/CD путем простого изменения текста.

Теперь вы готовы к совместной работе над приложением с использованием процесса CI/CD для развертывания последних результатов работы в Службе приложений. При каждом изменении в репозитории Git запускается сборка в Azure DevOps и конвейер CD выполняет развертывание в Azure. Используйте описанную в этом разделе процедуру или другой метод, чтобы зафиксировать изменения в репозитории. Например, можно скопировать репозиторий Git в предпочитаемое вами средство или интегрированную среду разработки, а затем отправить изменения в этот репозиторий.

1. В меню Azure DevOps выберите **Репозитории**, а затем **Файлы**. Перейдите к нужному репозиторию.

1. Этот репозиторий уже содержит код для модуля на том языке, который вы выбрали для приложения в процессе его создания. Откройте файл **Application/views/index.pug**.

1. Выберите **Редактировать** и внесите изменения в **строку 15**. Например, вы можете внести сюда текст "Мое первое развертывание в Службе приложений Azure на платформе Azure Cosmos DB".

1. В правом верхнем углу выберите **Зафиксировать**, после чего снова нажмите **Зафиксировать**, чтобы отправить изменения.

     Через несколько секунд в Azure DevOps начнется процесс сборки, после чего будет выполнена операция выпуска для развертывания изменений. Отслеживайте состояние сборки на панели мониторинга DevOps Starter или в браузере с помощью вашей организации Azure DevOps.

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите все созданные ресурсы, если они вам больше не нужны. Используйте функцию **удаления** на панели мониторинга DevOps Starter.

## <a name="next-steps"></a>Дальнейшие действия

Вы можете изменить эти конвейеры сборки и выпуска в соответствии с потребностями вашей команды. Вы также можете использовать этот шаблон CI/CD в качестве шаблона для других конвейеров. В этом руководстве вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
> * Используйте DevOps Starter для развертывания приложения Node. js на базе Azure Cosmos DB
> * настройка Azure DevOps и подписки Azure; 
> * Проверка Azure Cosmos DB
> * Просмотр конвейера CI
> * Просмотр конвейера CD
> * фиксация изменений в Git и их автоматическое развертывание в Azure;
> * Очистка ресурсов

Подробную информацию и описание дальнейших шагов см. в статье [Define your multi-stage continuous deployment (CD) pipeline](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=azure-devops&viewFallbackFrom=vsts) (Настройка многоэтапного конвейера для непрерывного развертывания).


