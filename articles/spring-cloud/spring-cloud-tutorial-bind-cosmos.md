---
title: Учебник. Привязывание Azure Cosmos DB к приложению Azure Spring Cloud
description: В этом учебнике описано, как привязать Azure Cosmos DB к приложению Azure Spring Cloud
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: brendm
ms.openlocfilehash: 1566b6ab59e858217adcf6818e1d62f851f37eb1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "76277563"
---
# <a name="bind-an-azure-cosmos-db-database-to-your-azure-spring-cloud-application"></a>Привязывание базы данных Azure Cosmos DB к приложению Azure Spring Cloud

Чтобы не настраивать приложения Spring Boot вручную, вы можете автоматически привязать выбранные службы Azure к приложениям с помощью Azure Spring Cloud. В этой статье показано, как привязать приложение к базе данных Azure Cosmos DB.

Предварительные требования:

* Развернутый экземпляр Azure Spring Cloud Чтобы приступить к работе, следуйте инструкциям из статьи [Краткое руководство. Запуск приложения Java Spring с помощью Azure CLI](spring-cloud-quickstart-launch-app-cli.md).
* Учетная запись Azure Cosmos DB с минимальным уровнем разрешений участника.

## <a name="bind-azure-cosmos-db"></a>Привязка Azure Cosmos DB

Azure Cosmos DB имеет пять различных типов API, поддерживающих привязку. В приведенной ниже процедуре показано, как их использовать:

1. Создает базу данных Azure Cosmos DB. Дополнительные сведения см. в статье [Краткое руководство. Создание учетной записи, базы данных, контейнера и элементов Azure Cosmos на портале Azure](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal). 

1. Запишите имя базы данных. Для этой процедуры имя базы данных — **testdb**.

1. Добавьте одну из указанных ниже зависимостей в файл pom.xml приложения Azure Spring Cloud. Выберите зависимость, соответствующую вашему типу API.

    * Тип API: Core (SQL)

      ```xml
      <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
          <version>2.1.6</version>
      </dependency>
      ```

    * Тип API: MongoDB

      ```xml
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-mongodb</artifactId>
      </dependency>
      ```

    * Тип API: Cassandra

      ```xml
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-cassandra</artifactId>
      </dependency>
      ```

    * Тип API: Gremlin (граф)

      ```xml
      <dependency>
          <groupId>com.microsoft.spring.data.gremlin</groupId>
          <artifactId>spring-data-gremlin</artifactId>
          <version>2.1.7</version>
      </dependency>
      ```

    * Тип API: таблице Azure

      ```xml
      <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-storage-spring-boot-starter</artifactId>
          <version>2.0.5</version>
      </dependency>
      ```

1. Выполните команду `az spring-cloud app update`, чтобы обновить текущее развертывание, или команду `az spring-cloud app deployment create`, чтобы его создать. Эти команды либо обновляют, либо создают приложение с новой зависимостью.

1. Перейдите к странице своей службы Azure Spring Cloud на портале Azure. Перейдите на **панель мониторинга приложения** и выберите приложение для привязки к Azure Cosmos DB. Это то же приложение, которое вы обновили или развернули на предыдущем шаге.

1. Выберите **Service binding** (Привязка службы), а затем — **Создание привязки службы**. Чтобы заполнить форму, выберите:
   * для **типа привязки** значение **Azure Cosmos DB**;
   * тип API;
   * имя базы данных;
   * учетную запись Azure Cosmos DB.

    > [!NOTE]
    > Если используется Cassandra, укажите пространство ключей для имени базы данных.

1. Перезапустите приложение, нажав кнопку **Перезапустить** на странице приложения.

1. Чтобы обеспечить правильную привязку службы, выберите имя привязки и проверьте сведения о ней. Поле `property` должно выглядеть приблизительно как в приведенном примере:

    ```
    azure.cosmosdb.uri=https://<some account>.documents.azure.com:443
    azure.cosmosdb.key=abc******
    azure.cosmosdb.database=testdb
    ```

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике описывается, как привязать приложение Azure Spring Cloud к базе данных Azure Cosmos DB. Чтобы узнать, как привязать приложение к Кэшу Azure для Redis, перейдите к следующему учебнику.

> [!div class="nextstepaction"]
> [Учебник по настройке привязки к Кэшу Azure для Redis](spring-cloud-tutorial-bind-redis.md)
