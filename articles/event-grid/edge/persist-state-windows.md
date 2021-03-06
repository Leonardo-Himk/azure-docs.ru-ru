---
title: Сохранение состояния в Windows — служба "Сетка событий Azure" IoT Edge | Документация Майкрософт
description: Сохранение состояния в Windows
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: c2bae3bd268dba8efdf23ae314671b17a2c89420
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77086627"
---
# <a name="persist-state-in-windows"></a>Сохранение состояния в Windows

Разделы и подписки, созданные в модуле сетки событий, по умолчанию хранятся в файловой системе контейнера. Без сохранения при повторном развертывании модуля все созданные метаданные будут потеряны. Чтобы сохранить данные между развертываниями и перезапусками, необходимо сохранить данные вне файловой системы контейнера. 

По умолчанию сохраняются только метаданные, и события по-прежнему хранятся в памяти для повышения производительности. Чтобы включить сохраняемость событий, следуйте разделу материализованные события.

В этой статье описаны шаги, необходимые для развертывания модуля службы "Сетка событий" с сохраняемостью в развертываниях Windows.

> [!NOTE]
>Модуль сетки событий выполняется как пользователь с низким уровнем привилегий **контаинерусер** в Windows.

## <a name="persistence-via-volume-mount"></a>Сохраняемость через подключение тома

[Тома DOCKER](https://docs.docker.com/storage/volumes/) используются для сохранения данных в разных развертываниях. Чтобы подключить том, необходимо создать его с помощью команд DOCKER, предоставить разрешения, чтобы контейнер мог читать, записывать в него, а затем развертывать модуль.

1. Создайте том, выполнив следующую команду:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume create <your-volume-name-here>
    ```

    Например, примененная к объекту директива

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume create myeventgridvol
   ```
1. Получите каталог узлов, на который сопоставляется том, выполнив команду ниже.

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume inspect <your-volume-name-here>
    ```

    Например, примененная к объекту директива

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume inspect myeventgridvol
   ```

   Пример выходных данных:-

   ```json
   [
          {
            "CreatedAt": "2019-07-30T21:20:59Z",
            "Driver": "local",
            "Labels": {},
            "Mountpoint": "C:\\ProgramData\\iotedge-moby\u000bolumes\\myeventgridvol\\_data",
            "Name": "myeventgridvol",
            "Options": {},
            "Scope": "local"
          }
   ]
   ```
1. Добавьте группу " **Пользователи** " в значение, на которое указывает **точка** подключения, следующим образом:
    1. Запустите проводник.
    1. Перейдите в папку, на которую указывает **точка**подключения.
    1. Щелкните правой кнопкой мыши и выберите пункт **Свойства**.
    1. Выберите **Безопасность**.
    1. В разделе * имена групп или пользователей выберите **изменить**.
    1. Нажмите кнопку **Добавить**, `Users`введите, выберите **Проверить имена**и нажмите кнопку **ОК**.
    1. В разделе *разрешения для пользователей*выберите **изменить**и нажмите кнопку **ОК**.
1. Используйте **привязки** для подключения этого тома и повторного развертывания модуля сетки событий из портал Azure

   Например, примененная к объекту директива

    ```json
        {
              "Env": [
                "inbound__serverAuth__tlsPolicy=strict",
                "inbound__serverAuth__serverCert__source=IoTEdge",
                "inbound__clientAuth__sasKeys__enabled=false",
                "inbound__clientAuth__clientCert__enabled=true",
                "inbound__clientAuth__clientCert__source=IoTEdge",
                "inbound__clientAuth__clientCert__allowUnknownCA=true",
                "outbound__clientAuth__clientCert__enabled=true",
                "outbound__clientAuth__clientCert__source=IoTEdge",
                "outbound__webhook__httpsOnly=true",
                "outbound__webhook__skipServerCertValidation=false",
                "outbound__webhook__allowUnknownCA=true"
              ],
              "HostConfig": {
                "Binds": [
                  "<your-volume-name-here>:C:\\app\\metadataDb"
                ],
                "PortBindings": {
                  "4438/tcp": [
                     {
                        "HostPort": "4438"
                     }
                  ]
                }
              }
        }
    ```

   >[!IMPORTANT]
   >Не изменяйте вторую часть значения привязки. Он указывает на определенное расположение в модуле. Для модуля сетки событий в Windows он должен иметь значение **C:\\App\\метадатадб**.


    Например, примененная к объекту директива

    ```json
    {
        "Env": [
            "inbound__serverAuth__tlsPolicy=strict",
            "inbound__serverAuth__serverCert__source=IoTEdge",
            "inbound__clientAuth__sasKeys__enabled=false",
            "inbound__clientAuth__clientCert__enabled=true",
            "inbound__clientAuth__clientCert__source=IoTEdge",
            "inbound__clientAuth__clientCert__allowUnknownCA=true",
            "outbound__clientAuth__clientCert__enabled=true",
            "outbound__clientAuth__clientCert__source=IoTEdge",
            "outbound__webhook__httpsOnly=true",
            "outbound__webhook__skipServerCertValidation=false",
            "outbound__webhook__allowUnknownCA=true"
         ],
         "HostConfig": {
            "Binds": [
                "myeventgridvol:C:\\app\\metadataDb",
                "C:\\myhostdir2:C:\\app\\eventsDb"
             ],
             "PortBindings": {
                    "4438/tcp": [
                         {
                            "HostPort": "4438"
                          }
                    ]
              }
         }
    }
    ```

## <a name="persistence-via-host-directory-mount"></a>Сохранение с помощью подключения к каталогу узлов

Вместо подключения тома можно создать каталог в главной системе и подключить этот каталог.

1. Создайте каталог в файловой системе узла, выполнив следующую команду.

   ```sh
   mkdir <your-directory-name-here>
   ```

   Например, примененная к объекту директива

   ```sh
   mkdir C:\myhostdir
   ```
1. Используйте **привязки** для подключения каталога и повторного развертывания модуля службы "Сетка событий" из портал Azure.

    ```json
    {
         "HostConfig": {
            "Binds": [
                "<your-directory-name-here>:C:\\app\\metadataDb"
             ]
         }
    }
    ```

    >[!IMPORTANT]
    >Не изменяйте вторую часть значения привязки. Он указывает на определенное расположение в модуле. Для модуля "Сетка событий" в Windows он должен иметь значение **C:\\App\\метадатадб**.

    Например, примененная к объекту директива

    ```json
    {
        "Env": [
            "inbound__serverAuth__tlsPolicy=strict",
            "inbound__serverAuth__serverCert__source=IoTEdge",
            "inbound__clientAuth__sasKeys__enabled=false",
            "inbound__clientAuth__clientCert__enabled=true",
            "inbound__clientAuth__clientCert__source=IoTEdge",
            "inbound__clientAuth__clientCert__allowUnknownCA=true",
            "outbound__clientAuth__clientCert__enabled=true",
            "outbound__clientAuth__clientCert__source=IoTEdge",
            "outbound__webhook__httpsOnly=true",
            "outbound__webhook__skipServerCertValidation=false",
            "outbound__webhook__allowUnknownCA=true"
         ],
         "HostConfig": {
            "Binds": [
                "C:\\myhostdir:C:\\app\\metadataDb",
                "C:\\myhostdir2:C:\\app\\eventsDb"
             ],
             "PortBindings": {
                    "4438/tcp": [
                         {
                            "HostPort": "4438"
                          }
                    ]
              }
         }
    }
    ```
## <a name="persist-events"></a>Сохранить события

Чтобы включить сохраняемость событий, необходимо сначала включить сохранение событий с помощью подключения тома или подключения к каталогу узла, используя приведенные выше разделы.

Важные моменты, которые следует учитывать при сохранении событий:

* Сохранение событий включено для каждой подписки на события и является явной присоединением тома или каталога.
* Сохраняемость событий настраивается в подписке на событие во время создания и не может быть изменена после создания подписки на события. Чтобы переключить сохраняемость событий, необходимо удалить и повторно создать подписку на события.
* Сохранение событий почти всегда выполняется медленнее, чем в операциях с памятью, однако разница в скорости сильно зависит от характеристик диска. Компромисс между скоростью и надежностью относится ко всем системам обмена сообщениями, но становится заметным только при большом масштабе.

Чтобы включить сохраняемость событий в подписке на события, `persistencePolicy` задайте `true`для параметра значение:

 ```json
        {
          "properties": {
            "persistencePolicy": {
              "isPersisted": "true"
            },
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-webhook-url>"
              }
            }
          }
        }
 ```