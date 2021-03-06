---
title: Диагностика и устранение неполадок Azure Cosmos DB пакета SDK для асинхронного Java версии 2
description: Используйте такие функции, как ведение журнала на стороне клиента и другие сторонние средства для выявления, диагностики и устранения Azure Cosmos DB проблем в асинхронном пакете SDK для Java версии 2.
author: anfeldma-ms
ms.service: cosmos-db
ms.date: 05/08/2020
ms.author: anfeldma
ms.devlang: java
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 04fa8d65ffb822fcd37f6da1bf3074a4e6a1d088
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982621"
---
# <a name="troubleshoot-issues-when-you-use-the-azure-cosmos-db-async-java-sdk-v2-with-sql-api-accounts"></a>Устранение неполадок при использовании Azure Cosmos DB пакета SDK версии 2 для асинхронного Java с учетными записями API SQL

> [!div class="op_single_selector"]
> * [Пакет SDK для Java v4](troubleshoot-java-sdk-v4-sql.md)
> * [Пакет SDK для Async Java версии 2](troubleshoot-java-async-sdk.md)
> * [.NET](troubleshoot-dot-net-sdk.md)
> 

> [!IMPORTANT]
> Это *не* самый последний пакет SDK для Java для Azure Cosmos DB! Рассмотрите возможность использования Azure Cosmos DB пакета SDK для Java версии 4 для вашего проекта. Следуйте инструкциям из руководств по [переходу на Azure Cosmos DB пакета SDK для Java версии 4](migrate-java-v4-sdk.md) и инструкции по обновлению для [реактора VS рксжава](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md) . 
>
> В этой статье описывается устранение неполадок только для Azure Cosmos DB асинхронного пакета SDK для Java версии 2. Дополнительные сведения см. в [комментариях к Выпуску](sql-api-sdk-async-java.md)Azure Cosmos DB Async Java SDK v2, о [репозитории](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) и [Советы по повышению производительности](performance-tips-async-java.md) .
>

В этой статье рассматриваются распространенные проблемы, обходные пути, шаги диагностики и средства, используемые при использовании [пакета SDK для Java Async](sql-api-sdk-async-java.md) с учетными ЗАПИСЯМи API для Azure Cosmos DB SQL.
Пакет SDK Java Async обеспечивает клиентское логическое представление для доступа к API SQL для Azure Cosmos DB. В этой статье описываются средства и подходы, которые помогут вам, если вы столкнетесь с проблемами.

Начните с этого списка:

* Ознакомьте с разделом [Распространенные проблемы и их обходные решения] в этой статье.
* Взгляните на пакет SDK [с открытым кодом на GitHub](https://github.com/Azure/azure-cosmosdb-java). Он имеет [раздел проблем](https://github.com/Azure/azure-cosmosdb-java/issues), который активно отслеживается. Вы можете там найти уже готовое обходное решение похожей проблемы.
* Просмотрите [советы по повышению производительности](performance-tips-async-java.md) и следуйте рекомендациям.
* Следуйте указаниям в оставшейся части этой статьи, если решение не нашлось. Затем [сообщите о проблеме в GitHub](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-and-workarounds"></a><a name="common-issues-workarounds"></a>Распространенные проблемы и обходные решения для них

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Проблемы с сетью, ошибка из-за истечения времени ожидания чтения Netty, низкая пропускная способность, высокая задержка

#### <a name="general-suggestions"></a>Общие рекомендации
* Убедитесь, что приложение выполняется в том же регионе, который указан для вашей учетной записи Azure Cosmos DB. 
* Проверьте использование ЦП на узле, где выполняется приложение. Если используются 90 или более процентов ЦП, запустите приложение на узле с лучшей конфигурацией. Можно также распределить нагрузку на большее число компьютеров.

#### <a name="connection-throttling"></a>Регулирование подключения
Регулирование подключения может произойти из-за [ограничения числа подключений на узле] или [нехватки портов Azure SNAT (PAT)].

##### <a name="connection-limit-on-a-host-machine"></a><a name="connection-limit-on-host"></a>Ограничение числа подключений на узле
Некоторые системы Linux (например, Red Hat) имеют ограничение на общее число открытых файлов. Сокеты в Linux реализованы как файлы, поэтому это число также существенно ограничивает общее число подключений.
Выполните следующую команду.

```bash
ulimit -a
```
Максимальное разрешенное количество открытых файлов, которые определены как "nofile", должно быть по крайней мере вдвое больше, чем размер пула подключений. Дополнительные сведения см. в статье [с советами по улучшению производительности](performance-tips-async-java.md).

##### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Нехватка портов SNAT (PAT) Azure

Если ваше приложение развернуто в службе "Виртуальные машины Azure" без общедоступного IP-адреса, по умолчанию [порты Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) используются для установления подключений к любой конечной точке вне вашей виртуальной машины. Количество разрешенных подключений от виртуальной машины к конечной точке Azure Cosmos DB ограничивается [конфигурацией Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Порты Azure SNAT используются, только если у виртуальной машины есть частный IP-адрес и процесс на виртуальной машине пытается установить подключение к общедоступному IP-адресу. Избежать ограничений Azure SNAT можно двумя способами:

* Добавьте конечную точку службы Azure Cosmos DB к подсети виртуальной сети для службы "Виртуальные машины Azure". Дополнительные сведения см. в статье [Конечные точки служб для виртуальной сети Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    При включении конечной точки службы запросы больше не отправляются с общедоступного IP-адреса в Azure Cosmos DB. Вместо этого отправляются идентификаторы виртуальной сети и подсети. Это изменение может привести к сбою брандмауэра, если разрешены только общедоступные IP-адреса. Если вы используете брандмауэр, при включении конечной точки службы добавьте подсеть в брандмауэре с помощью [списков управления доступом для виртуальных сетей](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Назначьте общедоступный IP-адрес виртуальной машине Azure.

##### <a name="cant-reach-the-service---firewall"></a><a name="cant-connect"></a>Не удается связаться со службой — брандмауэр
``ConnectTimeoutException``Указывает, что пакет SDK не может связаться со службой.
При использовании прямого режима может возникнуть ошибка, аналогичная приведенной ниже:
```
GoneException{error=null, resourceAddress='https://cdb-ms-prod-westus-fd4.documents.azure.com:14940/apps/e41242a5-2d71-5acb-2e00-5e5f744b12de/services/d8aa21a5-340b-21d4-b1a2-4a5333e7ed8a/partitions/ed028254-b613-4c2a-bf3c-14bd5eb64500/replicas/131298754052060051p//', statusCode=410, message=Message: The requested resource is no longer available at the server., getCauseInfo=[class: class io.netty.channel.ConnectTimeoutException, message: connection timed out: cdb-ms-prod-westus-fd4.documents.azure.com/101.13.12.5:14940]
```

Если на компьютере приложения работает брандмауэр, откройте диапазон портов 10 000 на 20 000, которые используются в режиме прямого подключения.
Кроме того, следует соблюдать [ограничение на количество подключений на хост-компьютере](#connection-limit-on-host).

#### <a name="http-proxy"></a>Прокси-сервер HTTP

При использовании прокси-сервера HTTP убедитесь, что он может поддерживать число подключений, указанное в свойстве `ConnectionPolicy` пакета SDK.
В противном случае возникнут проблемы с подключением.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Недопустимый шаблон кодировки: блокировка потока ввода-вывода Netty

Пакет SDK использует библиотеку [Netty](https://netty.io/) для ввода-вывода, чтобы связываться с Azure Cosmos DB. Пакет SDK имеет асинхронные API и использует неблокирующие API Netty для ввода-вывода. Операции ввода-вывода, присущие пакету SDK, выполняются в потоках Netty для ввода-вывода. Число потоков Netty для ввода-вывода настроено так, чтобы совпадать с числом ядер ЦП компьютера, где выполняется приложение. 

Потоки Netty для ввода-вывода предназначены только для операций Netty для ввода-вывода без блокировки. Пакет SDK возвращает результат вызова API, касающийся одного из потоков Netty для ввода-вывода, в код приложения. Если приложение выполняет операцию длительное время после получения результатов касательно потока Netty, пакет SDK может не располагать достаточным количеством потоков ввода-вывода для выполнения внутренних операций ввода-вывода. Кодирование такого приложения может привести к низкой пропускной способности, длительной задержке и сбоям `io.netty.handler.timeout.ReadTimeoutException`. Обходное решение заключается в том, чтобы переключить поток, когда вы знаете, что операция займет некоторое время.

Рассмотрим, например, следующий фрагмент кода. Можно выполнить длительную работу, которая занимает более нескольких миллисекунд, в потоке Netty. В таком случае рано или поздно возникнет состояние, при котором не будет потока Netty для ввода-вывода, способного обрабатывать операции ввода-вывода. В результате произойдет сбой ReadTimeoutException.

### <a name="async-java-sdk-v2-maven-commicrosoftazureazure-cosmosdb"></a><a id="asyncjava2-readtimeout"></a>Async Java SDK v2 (Maven com. Microsoft. Azure:: Azure-cosmosdb)

```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // Time-consuming work is, for example,
                        // writing to a file, computationally heavy work, or just sleep.
                        // Basically, it's anything that takes more than a few milliseconds.
                        // Doing such operations on the IO Netty thread
                        // without a proper scheduler will cause problems.
                        // The subscriber will get a ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //It will be io.netty.handler.timeout.ReadTimeoutException.
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
Обходное решение заключается в изменении потока, в котором вы выполняете длительные операции. Определите отдельный экземпляр планировщика для приложения.

### <a name="async-java-sdk-v2-maven-commicrosoftazureazure-cosmosdb"></a><a id="asyncjava2-scheduler"></a>Async Java SDK v2 (Maven com. Microsoft. Azure:: Azure-cosmosdb)

```java
// Have a singleton instance of an executor and a scheduler.
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
```
Может потребоваться выполнить длительную работу (например, такую, которая требует значительных вычислений или блокировки ввода-вывода). В этом случае необходимо переключение потока на рабочую роль, предоставляемую параметром `customScheduler` с помощью API `.observeOn(customScheduler)`.

### <a name="async-java-sdk-v2-maven-commicrosoftazureazure-cosmosdb"></a><a id="asyncjava2-applycustomscheduler"></a>Async Java SDK v2 (Maven com. Microsoft. Azure:: Azure-cosmosdb)

```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // Switches the thread.
        .subscribe(
            // ...
        );
```
Используя `observeOn(customScheduler)`, вы освобождаете поток Netty для ввода-вывода и переключаетесь на собственный поток, предоставляемый настраиваемым планировщиком. Это изменение позволяет решить проблему. Сбой `io.netty.handler.timeout.ReadTimeoutException` больше возникать не будет.

### <a name="connection-pool-exhausted-issue"></a>Проблема исчерпания пула подключений

`PoolExhaustedException` является ошибкой на стороне клиента. Такой сбой означает, что рабочая нагрузка приложения больше той, которую может обрабатывать пул подключений пакета SDK. Увеличьте размер пула подключений или распределите нагрузку между несколькими приложениями.

### <a name="request-rate-too-large"></a>Высокая частота запросов
Этот сбой происходит на сервере. Он означает, что вы использовали подготовленную пропускную способность. Повторите попытку позже. Если эта ошибка возникает часто, попробуйте увеличить пропускную способность коллекции.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Ошибка подключения к эмулятору Azure Cosmos DB

Сертификат HTTPS эмулятора для Azure Cosmos DB является самозаверяющим. Чтобы SDK работал с эмулятором, необходимо импортировать сертификат эмулятора в Java TrustStore. Дополнительные сведения см. в статье [Экспорт сертификатов эмулятора для Azure Cosmos DB](local-emulator-export-ssl-certificates.md).

### <a name="dependency-conflict-issues"></a>Проблемы с конфликтом зависимостей

```console
Exception in thread "main" java.lang.NoSuchMethodError: rx.Observable.toSingle()Lrx/Single;
```

Приведенное выше исключение предполагает, что у вас есть зависимость от более старой версии Рксжава lib (например, 1.2.2). Наш пакет SDK полагается на Рксжава 1.3.8, который содержит интерфейсы API, недоступные в более ранней версии Рксжава. 

Обходной путь для таких проблем заключается в определении того, какая другая зависимость переносится в Рксжава-1.2.2, и исключает косвенную зависимость от Рксжава-1.2.2, а также разрешите CosmosDB SDK вывести новую версию.

Чтобы узнать, какая библиотека переносится в Рксжава-1.2.2, выполните следующую команду рядом с файлом проекта POM. XML:
```bash
mvn dependency:tree
```
Дополнительные сведения см. в разделе [руководств по дереву зависимостей Maven](https://maven.apache.org/plugins/maven-dependency-plugin/examples/resolving-conflicts-using-the-dependency-tree.html).

Определив Рксжава-1.2.2, вы можете изменить зависимость от этой библиотеки в файле POM и исключить Рксжава с косвенной зависимостью. это будет так:

```xml
<dependency>
  <groupId>${groupid-of-lib-which-brings-in-rxjava1.2.2}</groupId>
  <artifactId>${artifactId-of-lib-which-brings-in-rxjava1.2.2}</artifactId>
  <version>${version-of-lib-which-brings-in-rxjava1.2.2}</version>
  <exclusions>
    <exclusion>
      <groupId>io.reactivex</groupId>
      <artifactId>rxjava</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

Дополнительные сведения см. в разделе [исключение транзитивных зависимостей](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html).


## <a name="enable-client-sdk-logging"></a><a name="enable-client-sice-logging"></a>Включение ведения журнала для пакета SDK клиента

Пакет SDK Async Java использует SLF4j как интерфейс ведения журнала, который поддерживает запись на популярные платформы ведения журналов, такие как log4j и logback.

Например, если вы хотите использовать log4j как платформу ведения журналов, добавьте в путь к классу Java следующие библиотеки:

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Добавьте также конфигурацию log4j.
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Дополнительные сведения см. в [руководстве по ведению журналов с использованием sfl4j](https://www.slf4j.org/manual.html).

## <a name="os-network-statistics"></a><a name="netstats"></a>Сетевая статистика ОС
Запустите команду netstat, чтобы понять, сколько подключений находится в состоянии `ESTABLISHED`, `CLOSE_WAIT` и т. д.

В Linux вы можете запустить следующую команду:
```bash
netstat -nap
```
Отфильтруйте результат для отображения только подключений к конечной точке Azure Cosmos DB.

Количество подключений к конечной точке Azure Cosmos DB в состоянии `ESTABLISHED` не должно превышать настроенного размера пула подключений.

Много подключений к конечной точке Azure Cosmos DB могут быть в состоянии `CLOSE_WAIT`. Возможно, более 1000. Такое большое количество указывает, что подключения быстро устанавливаются и быстро разрываются. Подобная ситуация может привести к проблемам. Дополнительные сведения см. в разделе [Распространенные проблемы и их обходные решения].

 <!--Anchors-->
[Распространенные проблемы и обходные решения для них]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Ограничение числа подключений на хост-компьютере]: #connection-limit-on-host
[Нехватка портов Azure SNAT (PAT)]: #snat


