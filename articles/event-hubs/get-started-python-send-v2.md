---
title: Отправка и получение событий через Центры событий Azure с помощью Python (последняя версия)
description: В статье описано, как создать приложение Python, которое отправляет события или получает их из службы "Центры событий Azure" с помощью последнего пакета azure-eventhub версии 5.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 6b16398c7c1fd53562df7e4ac8e801a8c97162f6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82159443"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-python-azure-eventhub-version-5"></a>Отправка событий или получение событий из концентраторов событий с помощью Python (azure-eventhub версии 5)
В этом кратком руководстве показано, как отправлять события в концентратор событий и получать события из него с помощью пакета Python **azure-eventhub версии 5**.

> [!IMPORTANT]
> При работе с этим кратким руководством используется новый пакет azure-eventhub версии 5. Краткое руководство по использованию старого пакета azure-eventhub версии 1 для отправки и получения событий см. [здесь](event-hubs-python-get-started-send.md). 

## <a name="prerequisites"></a>Предварительные требования
Если вы впервые используете Центры событий Azure, ознакомьтесь с [общими сведениями](event-hubs-about.md), прежде чем приступить к работе с этим руководством. 

Для работы с данным руководством необходимо следующее:

- **Подписка Microsoft Azure.** Чтобы использовать службы Azure, в том числе Центры событий Azure, потребуется действующая подписка.  Если у вас еще нет учетной записи Azure, [зарегистрируйтесь для работы с бесплатной пробной версией](https://azure.microsoft.com/free/) или [активируйте преимущества для подписчиков MSDN при создании учетной записи](https://azure.microsoft.com).
- Python 2.7, 3.5 или более поздней версии, для которого установлено и обновлено PIP.
- Пакет Python для Центров событий. 

    Чтобы установить этот пакет, выполните следующую команду в окне командной строки, где путь к Python сохранен в переменной окружения path.

    ```cmd
    pip install azure-eventhub
    ```

    Установите следующий пакет для получения событий с помощью хранилища BLOB-объектов Azure в качестве хранилища контрольных точек.

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```
- **Создайте пространство имен Центров событий и концентратор событий**. Первым шагом является использование [портала Azure](https://portal.azure.com) для создания пространства имен типа Центров событий и получение учетных данных управления, необходимых приложению для взаимодействия с концентратором событий. Чтобы создать пространство имен и концентратор событий, выполните инструкции из [этой статьи](event-hubs-create.md). Получите **строку подключения для пространства имен Центров событий**, следуя инструкциям из статьи [Получение строки подключения на портале](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Строка подключения понадобится нам позже в рамках этого краткого руководства.

## <a name="send-events"></a>Отправка событий
В этом разделе вы создадите скрипт Python для отправки событий в концентратор событий, созданный ранее.

1. Откройте предпочитаемый редактор Python, например [Visual Studio Code](https://code.visualstudio.com/).
2. Создайте сценарий с именем *send.py*. Этот сценарий отправляет пакет событий в концентратор событий, созданный ранее.
3. Скопируйте приведенный ниже код в файл *send.py*.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubProducerClient
    from azure.eventhub import EventData

    async def run():
        # Create a producer client to send messages to the event hub.
        # Specify a connection string to your event hubs namespace and
            # the event hub name.
        producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESPACE - CONNECTION STRING", eventhub_name="EVENT HUB NAME")
        async with producer:
            # Create a batch.
            event_data_batch = await producer.create_batch()

            # Add events to the batch.
            event_data_batch.add(EventData('First event '))
            event_data_batch.add(EventData('Second event'))
            event_data_batch.add(EventData('Third event'))

            # Send the batch of events to the event hub.
            await producer.send_batch(event_data_batch)

    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())

    ```

    > [!NOTE]
    > Полный исходный код, включая информационные комментарии, можно найти на странице [GitHub send_async.py](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py).
    

## <a name="receive-events"></a>Получение событий
В этом кратком руководстве в качестве хранилища контрольных точек используется хранилище BLOB-объектов Azure. Хранилище контрольных точек используется для сохранения контрольных точек (т. е. последних позиций чтения).  

> [!NOTE]
> При использовании Azure Stack Hub учтите, что эта платформа может поддерживать версию пакета SDK для хранилища BLOB-объектов, отличающуюся от доступных в Azure. Например, если вы используете [Azure Stack Hub версии 2002](https://docs.microsoft.com/azure-stack/user/event-hubs-overview), последней доступной версией для службы хранилища будет 2017-11-09. В таком случае кроме действий, описанных в этом разделе, вам нужно добавить код для включения поддержки API службы хранилища версии 2017-11-09. См. сведения о том, как включить поддержку определенной версии API службы хранилища для [синхронного](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/receive_events_using_checkpoint_store_storage_api_version.py) и [асинхронного](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/receive_events_using_checkpoint_store_storage_api_version_async.py) выполнения на GitHub. См. сведения о версиях службы хранилища Azure, поддерживаемых в Azure Stack Hub, в статье [Хранилище Azure Stack Hub. Отличия и рекомендации](https://docs.microsoft.com/azure-stack/user/azure-stack-acs-differences).


### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Создание учетной записи хранения Azure и контейнера больших двоичных объектов
Создайте учетную запись хранения Azure и контейнер больших двоичных объектов в ней, выполнив следующие действия.

1. [Создайте учетную запись хранения Azure](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Создание контейнера больших двоичных объектов](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Получение строки подключения к учетной записи хранения](../storage/common/storage-configure-connection-string.md).

Обязательно запишите строку подключения и имя контейнера для последующего использования в коде получения.


### <a name="create-a-python-script-to-receive-events"></a>Создание сценария Python для получения событий

В этом разделе вы создадите скрипт Python для получения событий из концентратора событий.

1. Откройте предпочитаемый редактор Python, например [Visual Studio Code](https://code.visualstudio.com/).
2. Создайте сценарий с именем *recv.py*.
3. Вставьте в *recv.py* следующий код.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubConsumerClient
    from azure.eventhub.extensions.checkpointstoreblobaio import BlobCheckpointStore


    async def on_event(partition_context, event):
        # Print the event data.
        print("Received the event: \"{}\" from the partition with ID: \"{}\"".format(event.body_as_str(encoding='UTF-8'), partition_context.partition_id))

        # Update the checkpoint so that the program doesn't read the events
        # that it has already read when you run it next time.
        await partition_context.update_checkpoint(event)

    async def main():
        # Create an Azure blob checkpoint store to store the checkpoints.
        checkpoint_store = BlobCheckpointStore.from_connection_string("AZURE STORAGE CONNECTION STRING", "BLOB CONTAINER NAME")

        # Create a consumer client for the event hub.
        client = EventHubConsumerClient.from_connection_string("EVENT HUBS NAMESPACE CONNECTION STRING", consumer_group="$Default", eventhub_name="EVENT HUB NAME", checkpoint_store=checkpoint_store)
        async with client:
            # Call the receive method.
            await client.receive(on_event=on_event)

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # Run the main method.
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > Полный исходный код, включая дополнительные информационные комментарии, можно найти на странице [GitHub recv_with_checkpoint_store_async.py](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py).


### <a name="run-the-receiver-app"></a>Запуск приложения получателя

Чтобы выполнить сценарий, откройте окно командной строки с Python в пути, а затем выполните следующую команду.

```bash
python recv.py
```

### <a name="run-the-sender-app"></a>Запуск приложения отправителя

Чтобы выполнить сценарий, откройте окно командной строки с Python в пути, а затем выполните следующую команду.

```bash
python send.py
```

В окне получателя должны отобразиться сообщения, отправленные в концентратор событий.


## <a name="next-steps"></a>Дальнейшие действия
В этом кратком руководстве события отправлены и получены асинхронно. Чтобы узнать, как отправлять и получать события синхронно, перейдите на страницу [GitHub sync_samples](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples).

Для всех примеров (синхронных и асинхронных) в GitHub перейдите на страницу [Azure Event Hubs client library for Python Samples](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples) (Клиентская библиотека Центров событий Azure для примеров Python).
