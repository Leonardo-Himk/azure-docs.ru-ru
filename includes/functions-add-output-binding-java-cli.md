---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 2b2c043e70aac14c7fc6f0b58aae257624b05d13
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673301"
---
В проекте Java привязки определяются как заметки привязки для метода функции. Затем на основе этих заметок автоматически создается файл *function.json*.

Найдите расположение вашего кода функции в _src/main/java_, откройте файл проекта *Function.java* и добавьте следующий параметр в `run` определение метода:

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

Параметр `msg` имеет тип [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding), представляющий собой набор строк, записываемых в виде сообщений в выходную привязку после завершения выполнения функции. В этом случае выходные данные представляют собой очередь хранилища с именем `outqueue`. Строка подключения для учетной записи хранения задается методом `connection`. Вместо самой строки подключения передается параметр приложения, содержащий строку подключения к учетной записи хранилища.

Определение метода `run` теперь должно выглядеть следующим образом:  

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION)  
        HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    ...
}
```