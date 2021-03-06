---
title: Добавление сообщений в очередь службы хранилища Azure с помощью Функций
description: Создавайте независимые от сервера функции, активируемые HTTP-запросом и создающие сообщения в очереди службы хранилища Azure с помощью службы "Функции Azure".
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.topic: how-to
ms.date: 04/24/2020
ms.custom: mvc
ms.openlocfilehash: 5ae282750580ed5b4e53e78c52ca285e40365fd3
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122042"
---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>Добавление сообщений в очередь службы хранилища Azure с помощью Функций

В службе "Функции Azure" входные и выходные привязки предоставляют декларативный способ предоставления коду данных внешних служб. В этом кратком руководстве используется выходная привязка для создания сообщения в очереди при активации функции HTTP-запросом. Для просмотра сообщений очереди, создаваемых функцией, используется контейнер службы хранилища Azure.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством сделайте следующее:

- Подписка Azure. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начать работу.

- Следуйте указаниям, приведенным в статье [Создание первой функции на портале Azure](functions-create-first-azure-function.md), пропустив шаг **Очистка ресурсов**. При работе с этим кратким руководством создаются приложение-функция и функция, которые вы будете использовать здесь.

## <a name="add-an-output-binding"></a><a name="add-binding"></a>Добавление выходной привязки

В этом разделе вам нужно будет добавить выходную привязку хранилища очередей для функции, созданной ранее, с помощью пользовательского интерфейса портала. Эта привязка позволяет написать минимальный код для создания сообщения в очереди. Вам не нужно писать код для таких задач, как открытие подключения к хранилищу, создание очереди или получение ссылки на очередь. Эти задачи выполняет среда выполнения службы "Функции Azure" и выходная привязка очереди.

1. На портале Azure откройте страницу приложения-функции для приложения, созданного [ранее](functions-create-first-azure-function.md). Чтобы открыть страницу, найдите и выберите **приложение-функция**. Затем выберите приложение функции.

1. Выберите приложение функции, а затем выберите функцию, созданную в предыдущем кратком руководстве.

1. Выберите **Интеграция**и щелкните **+ Добавить выходные данные**.

   :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-create-output-binding.png" alt-text="Создайте выходную привязку для функции." border="true":::

1. Выберите тип привязки **хранилища очередей Azure** и добавьте параметры, как указано в таблице, которая следует за этим снимком экрана: 

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-create-output-binding-details.png" alt-text="Добавление выходной привязки хранилища очередей к функции на портале Azure." border="true":::
    
    | Параметр      |  Рекомендуемое значение   | Описание                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Имя параметра сообщения** | outputQueueItem | Имя параметра выходной привязки. | 
    | **Имя очереди**   | outqueue  | Имя очереди для подключения к вашей учетной записи хранения. |
    | **Подключение к учетной записи хранения** | AzureWebJobsStorage | Вы можете использовать подключение к учетной записи хранения, которое уже используется вашим приложением-функцией, или создать его.  |

1. Нажмите кнопку **ОК** , чтобы добавить привязку.

Теперь, когда выходная привязка определена, вам нужно обновить код, чтобы использовать привязку для добавления сообщений в очередь.  

## <a name="add-code-that-uses-the-output-binding"></a>Добавление кода, который использует выходную привязку

В этом разделе вы добавляете код, который записывает сообщение в выходную очередь. Сообщение содержит значение, которое передается в триггер HTTP в строке запроса. Например, если строка запроса содержит `name=Azure`, в сообщении очереди будет указано: *Имя передано функции: Azure*.

1. В функции выберите **код + тест** , чтобы отобразить код функции в редакторе.

1. Измените код функции в соответствии с ее языком.

    # <a name="c"></a>[Ц\#](#tab/csharp)

    Добавьте параметр **outputQueueItem** в сигнатуру метода, как показано в следующем примере.

    ```cs
    public static async Task<IActionResult> Run(HttpRequest req,
        ICollector<string> outputQueueItem, ILogger log)
    {
        ...
    }
    ```

    В теле функции непосредственно перед инструкцией `return` добавьте код, который создает сообщение очереди с помощью этого параметра.

    ```cs
    outputQueueItem.Add("Name passed to the function: " + name);
    ```

    # <a name="javascript"></a>[JavaScript](#tab/nodejs)

    Добавьте код, который использует привязку для вывода в объекте `context.bindings` для создания сообщения очереди. Добавьте этот код перед инструкцией `context.done`.

    ```javascript
    context.bindings.outputQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ---

1. Щелкните **Сохранить**, чтобы сохранить изменения.

## <a name="test-the-function"></a>Проверка функции

1. После сохранения изменений в коде выберите **тест**.
1. Убедитесь, что тест соответствует приведенному ниже изображению, и выберите **выполнить**. 

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png" alt-text="Протестируйте привязку хранилища очередей в портал Azure." border="true":::

    Обратите внимание, что **тело запроса** содержит `name` значение *Azure*. Это значение находится в сообщении очереди, которое создается при вызове функции.
    
    Кроме выбора элемента **Запуск** можно вызвать функцию, введя URL-адрес в браузере и указав значение `name` в строке запроса. Метод браузера описан в предыдущем [кратком руководстве](functions-create-first-azure-function.md#test-the-function).

1. Проверьте журналы, чтобы убедиться, что функция успешно выполнена. 

Новая очередь с именем of **Queue** создается в учетной записи хранения с помощью среды выполнения функций при первом использовании выходной привязки. Для проверки того, что очередь и сообщение в ней были созданы, используется учетная запись хранения.

### <a name="find-the-storage-account-connected-to-azurewebjobsstorage"></a>Найдите учетную запись хранения, подключенную к AzureWebJobsStorage


1. Перейдите к приложению функции и выберите **Конфигурация**.

1. В разделе **Параметры приложения**выберите **AzureWebJobsStorage**.

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-find-storage-account.png" alt-text="Нахождение учетной записи хранения, подключенной к AzureWebJobsStorage." border="true":::

1. Найдите и запишите имя учетной записи.

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-storage-account-name.png" alt-text="Нахождение учетной записи хранения, подключенной к AzureWebJobsStorage." border="true":::

### <a name="examine-the-output-queue"></a>Проверка выходной очереди

1. В группе ресурсов для приложения функции выберите учетную запись хранения, которую вы используете для этого краткого руководства.

1. В разделе **Служба очередей**выберите **очереди** и выберите очередь с именем **Queue**. 

   В ней содержится сообщение о том, что выходная привязка очереди создана при запуске функции, активируемой HTTP. Если вы вызывали функцию со значением по умолчанию `name`*Azure*, в сообщении очереди будет указано *Имя переданной функции: Azure*.

1. Запустите функцию еще раз, и в очереди появится новое сообщение.  

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Дальнейшие действия

Выполнив указания этого краткого руководства, вы добавили выходную привязку в имеющуюся функцию. Дополнительные сведения о привязках к хранилищу очередей см.в статье [Привязки очередей службы хранилища для Функций Azure](functions-bindings-storage-queue.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps-2.md)]
