---
title: Учебник. Использование распределенной трассировки с помощью Azure Spring Cloud
description: В этом учебнике показано, как использовать распределенную трассировку в Spring Cloud с помощью Azure Application Insights.
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: brendm
ms.openlocfilehash: 0815aa084462d1b829d64cd7c5d6fa7cebf534fc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "78273213"
---
# <a name="use-distributed-tracing-with-azure-spring-cloud"></a>Использование распределенной трассировки в Azure Spring Cloud

Инструменты распределенной трассировки в Azure Spring Cloud позволяют легко выполнять отладку и мониторинг сложных проблем. Azure Spring Cloud интегрирует [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) с Azure [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Эта интеграция предоставляет мощные возможности распределенной трассировки через портал Azure.

В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * Включение распределенной трассировки на портале Azure.
> * Добавление Spring Cloud Sleuth в приложение.
> * Просмотр сопоставления зависимостей для приложений для микрослужб.
> * Поиск данных трассировки с использованием фильтров.

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить процедуры из этого учебника, вам потребуется подготовленная и запущенная служба Azure Spring Cloud. Выполните [краткое руководство по развертыванию приложения с помощью Azure CLI](spring-cloud-quickstart-launch-app-cli.md), чтобы подготовить и запустить службу Azure Spring Cloud.
    
## <a name="add-dependencies"></a>Добавление зависимостей

1. Добавьте следующую строку в конец файла application.properties.

   ```xml
   spring.zipkin.sender.type = web
   ```

   После этого изменения отправитель Zipkin сможет отправлять данные в Интернет.

1. Вы можете пропустить этот шаг, если уже выполнили [руководство по подготовке приложения Azure Spring Cloud](spring-cloud-tutorial-prepare-app-deployment.md). В противном случае перейдите в локальную среду разработки и измените файл pom.xml, чтобы включить в него зависимость Spring Cloud Sleuth.

    ```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-sleuth</artifactId>
                <version>${spring-cloud-sleuth.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-sleuth</artifactId>
        </dependency>
    </dependencies>
    ```

1. Повторите сборку и развертывание для службы Azure Spring Cloud, чтобы отразить эти изменения.

## <a name="modify-the-sample-rate"></a>Изменение частоты выборки

Частоту сбора данных телеметрии можно изменить, модифицируя частоту выборки. Например, если вы хотите выполнять выборку вдвое реже, откройте файл application.properties и измените следующую строку.

```xml
spring.sleuth.sampler.probability=0.5
```

Вы можете изменить частоту выборки, даже если уже скомпилировали и развернули приложение. Для этого добавьте указанную выше строку в переменную среды с помощью Azure CLI или портала Azure.

## <a name="enable-application-insights"></a>Включение Application Insights

1. Перейдите к странице своей службы Azure Spring Cloud на портале Azure.
1. На странице **Мониторинг** выберите элемент **Распределенная трассировка**.
1. Выберите **Изменить параметр**, чтобы изменить или добавить новый параметр.
1. Создайте новый запрос Application Insights или выберите имеющийся.
1. Выберите категорию ведения журнала, которую требуется отслеживать, и укажите период удержания (в днях).
1. Выберите **Применить**, чтобы применить новую трассировку.

## <a name="view-the-application-map"></a>Просмотр схемы приложений

Вернитесь на страницу **Распределенная трассировка** и выберите действие **Просмотреть схему приложений**. Проверьте визуальное представление приложения и параметры мониторинга. Сведения об использовании схемы приложения см. в [статье о рассмотрении распределенных приложений](https://docs.microsoft.com/azure/azure-monitor/app/app-map).

## <a name="use-search"></a>Использование поиска

Используйте функцию поиска для запроса других конкретных элементов телеметрии. На странице **Distributed Tracing** (Распределенная трассировка) выберите **Поиск**. Дополнительные сведения об использовании функции поиска см. в статье [Поиск в Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/diagnostic-search).

## <a name="use-application-insights"></a>Использование Application Insights

Кроме функций поиска и схемы приложений Application Insights предоставляет возможности для мониторинга. Найдите на портале Azure имя своего приложения и откройте для него страницу Application Insights, чтобы получить сведения о мониторинге. Дополнительные сведения об использовании этих средств см. в [документации по запросам к журналам Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language).

## <a name="disable-application-insights"></a>Отключение Application Insights

1. Перейдите к странице своей службы Azure Spring Cloud на портале Azure.
1. В разделе **Мониторинг** выберите элемент **Распределенная трассировка**.
1. Щелкните **Отключить**, чтобы отключить Application Insights

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике вы узнали, как включить и изучить распределенную трассировку в Azure Spring Cloud. Чтобы узнать, как привязать приложение к базе данных Azure Cosmos DB, перейдите к следующему учебнику.

> [!div class="nextstepaction"]
> [Узнайте, как настроить привязку к базе данных Azure Cosmos DB](spring-cloud-tutorial-bind-cosmos.md)
