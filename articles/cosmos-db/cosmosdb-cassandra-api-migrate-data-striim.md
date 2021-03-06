---
title: Перенос данных в учетную запись Azure Cosmos DB API Cassandra с помощью Стриим
description: Узнайте, как использовать Стриим для переноса данных из базы данных Oracle в учетную запись API Cassandra Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 50028e81c4ca130aa3266c164a431dc935a271cb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81730038"
---
# <a name="migrate-data-to-azure-cosmos-db-cassandra-api-account-using-striim"></a>Перенос данных в учетную запись Azure Cosmos DB API Cassandra с помощью Стриим

Образ Стриим в Azure Marketplace обеспечивает непрерывное перемещение данных из хранилищ и баз данных в Azure в режиме реального времени. При перемещении данных можно выполнить встроенную денормализацию, преобразование данных, включить аналитику в реальном времени и сценарии создания отчетов с данными. Вы можете легко приступить к работе с Стриим, чтобы постоянно перемещать корпоративные данные в Azure Cosmos DB API Cassandra. Azure предоставляет предложение Marketplace, которое упрощает развертывание Стриим и перенос данных в Azure Cosmos DB. 

В этой статье показано, как использовать Стриим для переноса данных из **базы данных Oracle** в **учетную запись API Cassandra Azure Cosmos DB**.

## <a name="prerequisites"></a>Предварительные условия

* Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), прежде чем начать работу.

* База данных Oracle, работающая в локальной среде, с некоторыми данными.

## <a name="deploy-the-striim-marketplace-solution"></a>Развертывание решения Стриим Marketplace

1. Войдите на [портал Azure](https://portal.azure.com/).

1. Выберите **создать ресурс** и выполните поиск по запросу **Стриим** в Azure Marketplace. Выберите первый вариант и **Создайте**.

   ![Найти элемент Стриим Marketplace](./media/cosmosdb-sql-api-migrate-data-striim/striim-azure-marketplace.png)

1. Затем введите свойства конфигурации для экземпляра Стриим. Среда Стриим развертывается на виртуальной машине. В области **Основные сведения** введите **имя пользователя виртуальной машины**, **пароль виртуальной** машины (этот пароль используется для подключения к виртуальной машине по протоколу SSH). Выберите **подписку**, **группу ресурсов**и **сведения о расположении** , где вы хотите развернуть Стриим. После завершения нажмите кнопку **ОК**.

   ![Настройка основных параметров для Стриим](./media/cosmosdb-sql-api-migrate-data-striim/striim-configure-basic-settings.png)


1. В области **Параметры кластера Стриим** выберите тип развертывания Стриим и размер виртуальной машины.

   |Параметр | Значение | Описание |
   | ---| ---| ---|
   |Тип развертывания Стриим |Автономный | Стриим может выполняться в **автономном** или **кластерном** типах развертывания. В автономном режиме сервер Стриим будет развернут на одной виртуальной машине. в зависимости от объема данных можно выбрать размер виртуальных машин. В режиме кластера будет развернут сервер Стриим на двух или более виртуальных машинах с выбранным размером. Кластерные среды с более чем 2 узлами обеспечивают автоматическую высокую доступность и отработку отказа.</br></br> В этом учебнике можно выбрать автономный параметр. Используйте виртуальную машину размера "Standard_F4s" по умолчанию. | 
   | Имя кластера Стриим|    <Striim_cluster_Name>|  Имя кластера Стриим.|
   | Пароль кластера Стриим|   <Striim_cluster_password>|  Пароль для кластера.|

   После заполнения формы нажмите кнопку **ОК** , чтобы продолжить.

1. В области **параметры доступа Стриим** настройте **общедоступный IP-адрес** (выберите значения по умолчанию), **доменное имя для Стриим**, **пароль администратора** , который вы хотите использовать для входа в пользовательский интерфейс Стриим. Настройте ВИРТУАЛЬную сеть и подсети (выберите значения по умолчанию). После заполнения сведений нажмите кнопку **ОК** , чтобы продолжить.

   ![Параметры доступа Стриим](./media/cosmosdb-sql-api-migrate-data-striim/striim-access-settings.png)

1. Azure будет проверять развертывание и убедиться в том, что все выглядит хорошо. выполнение проверки занимает несколько минут. После завершения проверки нажмите кнопку **ОК**.
  
1. Наконец, ознакомьтесь с условиями использования и выберите **создать** , чтобы создать экземпляр Стриим. 

## <a name="configure-the-source-database"></a>Настройка базы данных источника

В этом разделе вы настроите базу данных Oracle в качестве источника для перемещения данных.  Для подключения к Oracle требуется [драйвер Oracle JDBC](https://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html) . Чтобы считать изменения из исходной базы данных Oracle, можно использовать API [поступающие](https://www.oracle.com/technetwork/database/features/availability/logmineroverview-088844.html) или [ксстреам](https://docs.oracle.com/cd/E11882_01/server.112/e16545/xstrm_intro.htm#XSTRM72647). Для чтения, записи или сохранения данных из базы данных Oracle драйвер для Oracle JDBC должен присутствовать в подкаталогах Java Стриим.

Скачайте драйвер [ojdbc8. jar](https://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html) на локальный компьютер. Он будет установлен в кластере Стриим позже.

## <a name="configure-target-database"></a>Настройка целевой базы данных

В этом разделе вы настроите учетную запись Azure Cosmos DB API Cassandra в качестве целевой для перемещения данных.

1. Создайте [учетную запись API Cassandra Azure Cosmos DB](create-cassandra-dotnet.md#create-a-database-account) с помощью портал Azure.

1. Перейдите в область **Обозреватель данных** в учетной записи Azure Cosmos. Выберите **создать таблицу** , чтобы создать новый контейнер. Предположим, что выполняется перенос *продуктов* и данных *заказов* из базы данных Oracle в Azure Cosmos DB. Создайте новый пространства ключей с именем **стриимдемо** и контейнером Orders. Подготавливаете контейнер с помощью **1000 RUS**(в этом примере используется 1000 RUS, но для рабочей нагрузки следует использовать оценку пропускной способности) и **/ORDER_ID** в качестве первичного ключа. Эти значения будут различаться в зависимости от исходных данных. 

   ![Создание учетной записи API Cassandra](./media/cosmosdb-cassandra-api-migrate-data-striim/create-cassandra-api-account.png)

## <a name="configure-oracle-to-azure-cosmos-db-data-flow"></a>Настройка Oracle для Azure Cosmos DB потока данных

1. Теперь вернемся к Стриим. Прежде чем взаимодействовать с Стриим, установите драйвер Oracle JDBC, скачанный ранее.

1. Перейдите к экземпляру Стриим, развернутому в портал Azure. Нажмите кнопку **подключить** в верхней строке меню и на вкладке **SSH** скопируйте URL-адрес в поле **Вход с помощью локальной учетной записи виртуальной машины** .

   ![Получение URL-адреса SSH](./media/cosmosdb-sql-api-migrate-data-striim/get-ssh-url.png)

1. Откройте новое окно терминала и выполните команду SSH, скопированную из портал Azure. В этой статье используется терминал в MacOS. Вы можете выполнить аналогичные инструкции с помощью выводимых инструкций или другого клиента SSH на компьютере Windows. При появлении запроса введите **Да** , чтобы продолжить, и введите **пароль** , заданный для виртуальной машины на предыдущем шаге.

   ![Подключение к виртуальной машине Стриим](./media/cosmosdb-sql-api-migrate-data-striim/striim-vm-connect.png)

1. Теперь откройте новую вкладку терминала, чтобы скопировать ранее загруженный файл **ojdbc8. jar** . Используйте следующую команду SCP для копирования JAR-файла с локального компьютера в папку tmp экземпляра Стриим, работающего в Azure:

   ```bash
   cd <Directory_path_where_the_Jar_file_exists> 
   scp ojdbc8.jar striimdemo@striimdemo.westus.cloudapp.azure.com:/tmp
   ```

   ![Скопируйте JAR-файл с компьютера Location в Стриим.](./media/cosmosdb-sql-api-migrate-data-striim/copy-jar-file.png)

1. Затем вернитесь к окну, где вы выполнили подключение SSH к экземпляру Стриим и выполните вход как sudo. Переместите файл **ojdbc8. jar** из каталога **/tmp** в каталог **lib** экземпляра Стриим с помощью следующих команд:

   ```bash
   sudo su
   cd /tmp
   mv ojdbc8.jar /opt/striim/lib
   chmod +x ojdbc8.jar
   ```

   ![Перемещение JAR-файла в папку lib](./media/cosmosdb-sql-api-migrate-data-striim/move-jar-file.png)


1. В том же окне терминала перезапустите сервер Стриим, выполнив следующие команды:

   ```bash
   Systemctl stop striim-node
   Systemctl stop striim-dbms
   Systemctl start striim-dbms
   Systemctl start striim-node
   ```
 
1. Запуск Стриим займет около минуты. Если вы хотите просмотреть состояние, выполните следующую команду: 

   ```bash
   tail -f /opt/striim/logs/striim-node.log
   ```

1. Теперь вернитесь в Azure и скопируйте общедоступный IP-адрес виртуальной машины Стриим. 

   ![Копировать IP-адрес виртуальной машины Стриим](./media/cosmosdb-sql-api-migrate-data-striim/copy-public-ip-address.png)

1. Чтобы перейти к веб-ИНТЕРФЕЙСу Стриим, откройте новую вкладку в браузере и скопируйте общедоступный IP-адрес, за которым следует: 9080. Войдите, используя имя **администратора** , а также пароль администратора, указанный в портал Azure.

   ![Вход в Стриим](./media/cosmosdb-sql-api-migrate-data-striim/striim-login-ui.png)

1. Теперь вы приступили к домашней странице Стриим. Существует три различные панели: **панели мониторинга**, **приложения**и **саурцепревиев**. Панель панели мониторинга позволяет перемещать данные в режиме реального времени и визуализировать их. Панель приложения содержит конвейеры потоковых данных или потоки данных. В правой части страницы находится Саурцепревиев, где можно просмотреть данные перед их перемещением.

1. Выберите область **приложения** . Сейчас мы рассмотрим эту панель. Существует множество примеров приложений, которые можно использовать для изучения Стриим, однако в этой статье вы создадите свой собственный. Нажмите кнопку **Добавить приложение** в правом верхнем углу.

   ![Добавление приложения Стриим](./media/cosmosdb-sql-api-migrate-data-striim/add-striim-app.png)

1. Существует несколько различных способов создания Стриим приложений. Для этого сценария выберите **Запуск с нуля** .

   ![Запуск приложения с нуля](./media/cosmosdb-cassandra-api-migrate-data-striim/start-app-from-scratch.png)

1. Присвойте приложению понятное имя, например **оратокосмосдб** , и выберите **сохранить**.

   ![Создание приложения](./media/cosmosdb-cassandra-api-migrate-data-striim/create-new-application.png)

1. Вы поступите в конструкторе Flow, где вы можете перетаскивать соединители Box для создания приложений потоковой передачи. В строке поиска введите **Oracle** , перетащите источник **Oracle CDC** на холст приложения.  

   ![Источник Oracle CDC](./media/cosmosdb-cassandra-api-migrate-data-striim/oracle-cdc-source.png)

1. Введите свойства конфигурации источника для экземпляра Oracle. Имя источника — это просто соглашение об именовании для приложения Стриим. можно использовать имя, например **src_onPremOracle**. Также введите другие сведения, такие как тип адаптера, URL-адрес подключения, имя пользователя, пароль, имя таблицы. Чтобы продолжить, нажмите кнопку **сохранить** .

   ![Настройка параметров источника](./media/cosmosdb-cassandra-api-migrate-data-striim/configure-source-parameters.png)

1. Теперь щелкните значок Wave потока, чтобы подключить целевой экземпляр Azure Cosmos DB. 

   ![Подключение к целевому устройству](./media/cosmosdb-cassandra-api-migrate-data-striim/connect-to-target.png)

1. Перед настройкой целевого объекта убедитесь, что вы добавили [корневой сертификат Baltimore в среду Java Стриим](/azure/developer/java/sdk/java-sdk-add-certificate-ca-store#to-add-a-root-certificate-to-the-cacerts-store).

1. Введите свойства конфигурации целевого Azure Cosmos DB экземпляра и нажмите кнопку **сохранить** , чтобы продолжить. Ниже приведены ключевые параметры, которые следует отметить:

   * **Адаптер** — используйте **датабасевритер**. При записи в Azure Cosmos DB API Cassandra требуется Датабасевритер. Драйвер Cassandra 3.6.0 входит в состав Стриим. Если Датабасевритер превышает число получателей, подготовленное в контейнере Cosmos для Azure, произойдет сбой приложения.

   * **URL-адрес подключения** — укажите URL-адрес подключения JDBC Azure Cosmos DB. URL-адрес имеет формат`jdbc:cassandra://<contactpoint>:10350/<databaseName>?SSL=true`

   * **Username** — укажите имя учетной записи Azure Cosmos.
   
   * **Пароль** — укажите первичный ключ учетной записи Azure Cosmos.

   * **Таблицы** — целевые таблицы должны иметь первичные ключи, а первичные ключи не могут быть обновлены.

   ![Настройка свойств целевого объекта](./media/cosmosdb-cassandra-api-migrate-data-striim/configure-target-parameters1.png)

   ![Настройка свойств целевого объекта](./media/cosmosdb-cassandra-api-migrate-data-striim/configure-target-parameters2.png)

1. Теперь выполним запуск приложения Стриим. В верхней строке меню выберите **создать**, а затем **разверните приложение**. В окне Развертывание можно указать, следует ли запускать определенные части приложения в определенных частях топологии развертывания. Так как мы выполняем в простой топологии развертывания с помощью Azure, мы будем использовать параметр по умолчанию.

   ![Развертывание приложения](./media/cosmosdb-cassandra-api-migrate-data-striim/deploy-the-app.png)


1. Теперь мы проверим поток для просмотра данных через Стриим. Щелкните значок волна и щелкните значок глаза рядом с ним. После развертывания можно просмотреть поток для просмотра данных, передаваемых по. Щелкните значок **волна** и **проводим** рядом с ним. Нажмите кнопку **Развернутая** в верхней строке меню и выберите **запустить приложение**.

   ![Запуск приложения](./media/cosmosdb-cassandra-api-migrate-data-striim/start-the-app.png)

1. С помощью средства чтения **CDC (отслеживания измененных данных)** Стриим выберет только новые изменения в базе данных. При наличии данных, передаваемых через исходные таблицы, вы увидите их. Однако, поскольку это образец таблицы, источник, не подключенный к какому-либо приложению. При использовании образца генератора данных можно вставить цепочку событий в базу данных Oracle.

1. Вы увидите данные, передаваемые через платформу Стриим. Стриим также берет все метаданные, связанные с таблицей, что полезно для отслеживания данных и обеспечения того, что распространители данных должны быть в правильном целевом объекте.

   ![Настройка конвейера CDC](./media/cosmosdb-cassandra-api-migrate-data-striim/setup-cdc-pipeline.png)

1. Наконец, давайте выполним вход в Azure и перейду к своей учетной записи Azure Cosmos. Обновите обозреватель данных, и вы увидите, что данные получены. 

С помощью решения Стриим в Azure можно непрерывно переносить данные в Azure Cosmos DB из различных источников, таких как Oracle, Cassandra, MongoDB и других, для Azure Cosmos DB. Дополнительные сведения см. на [веб-сайте Стриим](https://www.striim.com/), [Загрузите бесплатную 30-дневную пробную версию Стриим](https://go2.striim.com/download-free-trial)и в случае возникновения проблем при настройке пути миграции с помощью Стриим отправьте [запрос в службу поддержки.](https://go2.striim.com/request-support-striim)

## <a name="next-steps"></a>Дальнейшие шаги

* Сведения о переносе данных в Azure Cosmos DB API SQL см. в статье [Перенос данных в учетную запись API Cassandra с помощью Стриим](cosmosdb-sql-api-migrate-data-striim.md) .

* [Мониторинг и отладка данных с помощью метрик Azure Cosmos DB](use-metrics.md)
