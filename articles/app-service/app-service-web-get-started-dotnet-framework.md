---
title: Краткое руководство. Создание приложения ASP.NET на C#
description: Узнайте, как запускать веб-приложения в Службе приложений Azure, развертывая шаблон веб-приложения ASP.NET на C# по умолчанию в Visual Studio.
ms.assetid: 04a1becf-7756-4d4e-92d8-d9471c263d23
ms.topic: quickstart
ms.date: 04/21/2020
ms.custom: mvc, devcenter, seodec18
ms.openlocfilehash: 056c96807fed7d5acca85635440c8fe4d989c933
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82592403"
---
# <a name="create-an-aspnet-framework-web-app-in-azure"></a>Создание веб-приложения ASP.NET Framework в Azure

[Служба приложений Azure](overview.md) — это служба веб-размещения с самостоятельной установкой исправлений и высоким уровнем масштабируемости.

В этом кратком руководстве рассматривается развертывание первого веб-приложения ASP.NET в Службе приложений Azure. По завершении вы получите план службы приложений. У вас также будет приложение Службы приложений с развернутым веб-приложением.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством установите <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> с рабочей нагрузкой **ASP.NET и разработка веб-приложений**.

Если у вас уже установлена версия Visual Studio 2019, сделайте следующее.

- Установите последние обновления для Visual Studio, выбрав **Справка** > **Проверить обновления**.
- Добавьте рабочую нагрузку, выбрав **Инструменты** > **Получить средства и компоненты**.

## <a name="create-an-aspnet-web-app"></a>Создание веб-приложения ASP.NET <a name="create-and-publish-the-web-app"></a>

Создайте веб-приложение ASP.NET, сделав следующее:

1. Откройте Visual Studio и выберите **Создать проект**.

2. В окне **Создание проекта** найдите и выберите **Веб-приложение ASP.NET (.NET Framework)** , а затем нажмите кнопку **Далее**.

3. В окне **Настройка проекта** присвойте приложению имя _myFirstAzureWebApp_ и щелкните **Создать**.

   ![Настройка проекта веб-приложения](./media/app-service-web-get-started-dotnet-framework/configure-web-app-project-framework.png)

4. Вы можете развернуть любой тип веб-приложения ASP.NET в Azure. В рамках этого краткого руководства выберите шаблон **MVC**.

5. Обязательно выберите **Без проверки подлинности** в параметрах аутентификации. Нажмите кнопку **создания**.

   ![Создание веб-приложения ASP.NET](./media/app-service-web-get-started-dotnet-framework/select-mvc-template-vs2019.png)

6. В меню Visual Studio выберите **Отладка** > **Запустить без отладки**, чтобы запустить приложение локально.

   ![Локальный запуск приложения](./media/app-service-web-get-started-dotnet-framework/local-web-app.png)

## <a name="publish-your-web-app"></a>Публикация веб-приложения <a name="launch-the-publish-wizard"></a>

1. Щелкните правой кнопкой мыши проект **myFirstAzureWebApp** в **обозревателе решений** и выберите **Опубликовать**.

1. Выберите **Служба приложений** и щелкните **Создать профиль**.

   ![Публикация с помощью страницы обзора проекта](./media/app-service-web-get-started-dotnet-framework/publish-app-framework-vs2019.png)

1. Выбор параметров на странице **создания Службы приложений** зависит от того, вошли ли вы в Azure и есть ли у вас учетная запись Visual Studio, связанная с учетной записью Azure. Выберите **Добавить учетную запись** или **Войти**, чтобы войти в подписку Azure. Если вы уже вошли, выберите нужную учетную запись.

   > [!NOTE]
   > Если вы уже выполнили вход, пока не нажимайте кнопку **Создать**.
   >
   >

   ![Вход в Azure](./media/app-service-web-get-started-dotnet-framework/sign-in-azure-framework-vs2019.png)

   [!INCLUDE [resource group intro text](../../includes/resource-group.md)]

1. В разделе **Группа ресурсов** выберите **Создать**.

1. В разделе **Новое имя группы ресурсов** введите *myResourceGroup* и щелкните **ОК**.

   [!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

1. В разделе **План размещения** щелкните **Создать**.

1. В разделе **Настройка плана размещения** введите значения из следующей таблицы и щелкните **ОК**.

   | Параметр | Рекомендуемое значение | Описание |
   |-|-|-|
   | План размещения| myAppServicePlan | Имя плана службы приложений. |
   | Расположение | Западная Европа | Центр обработки данных, где размещается веб-приложение. |
   | Размер | Free | [Ценовая категория](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) определяет возможности размещения. |

   ![Создание плана службы приложений](./media/app-service-web-get-started-dotnet-framework/app-service-plan-framework-vs2019.png)

1. В поле **Имя** введите уникальное имя приложения, включающее только допустимые символы: `a-z`, `A-Z`, `0-9` и `-`. Вы можете использовать автоматически созданное уникальное имя. URL-адрес веб-приложения: `http://<app-name>.azurewebsites.net`, где `<app-name>` — имя приложения.

2. Нажмите кнопку **Создать**, чтобы начать создавать ресурсы Azure.

   ![Указание имени приложения](./media/app-service-web-get-started-dotnet-framework/web-app-name-framework-vs2019.png)

    После завершения работы мастера ресурсы Azure будут созданы и готовы к публикации.

3. На странице **Публикация** щелкните **Опубликовать**. Visual Studio создает, упаковывает и публикует приложение в Azure, а затем запускает его в браузере по умолчанию.

    ![Опубликованное веб-приложение ASP.NET в Azure](./media/app-service-web-get-started-dotnet-framework/published-azure-web-app.png)

Имя приложения, указанное на странице **создания приложения в Службе приложений**, используется как префикс URL-адреса в формате `http://<app-name>.azurewebsites.net`.

**Поздравляем!** Ваше веб-приложение ASP.NET работает в Службе приложений Azure в режиме реального времени.

## <a name="update-the-app-and-redeploy"></a>Обновление и повторное развертывание приложения

1. В **обозревателе решений** в вашем проекте откройте **Представления** > **Главная страница** > **Index.cshtml**.

1. Найдите тег HTML `<div class="jumbotron">` в верхней области и замените его следующим кодом:

   ```HTML
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we've built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. Чтобы выполнить повторное развертывание в Azure, щелкните правой кнопкой мыши проект **myFirstAzureWebApp** в **обозревателе решений**, а затем выберите **Опубликовать**. Затем щелкните **Опубликовать**.

    По завершении публикации Visual Studio открывает в браузере страницу с URL-адресом веб-приложения.

    ![Обновленное веб-приложение ASP.NET в Azure](./media/app-service-web-get-started-dotnet-framework/updated-azure-web-app.png)

## <a name="manage-the-azure-app"></a>Управление приложением Azure

1. Чтобы управлять веб-приложением, перейдите на [портал Azure](https://portal.azure.com), найдите в поиске и выберите **Службы приложений**.

   ![Выбор Служб приложений](./media/app-service-web-get-started-dotnet-framework/app-services.png)

2. На странице **Службы приложений** выберите имя веб-приложения.

   ![Переход к приложению Azure на портале](./media/app-service-web-get-started-dotnet-framework/access-portal-framework-vs2019.png)

   Отобразится страница обзора вашего веб-приложения. Здесь вы можете выполнять базовые задачи управления: просмотр, завершение, запуск, перезагрузку и удаление.

   ![Обзор Службы приложений на портале Azure](./media/app-service-web-get-started-dotnet-framework/web-app-general-framework-vs2019.png)

   В меню слева доступно несколько страниц для настройки приложения.

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание приложения ASP.NET в Azure с подключением к базе данных SQL](app-service-web-tutorial-dotnet-sqldatabase.md)
