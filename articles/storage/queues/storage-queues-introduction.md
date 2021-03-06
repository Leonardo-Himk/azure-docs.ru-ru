---
title: Общие сведения о службе "Очереди Azure" — служба хранилища Azure
description: Общие сведения о службе "Очереди Azure"
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/18/2020
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: cbrooks
ms.openlocfilehash: 4a2bea77578282d68d86bc1a8cea765aa2cbd555
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2020
ms.locfileid: "80060844"
---
# <a name="what-are-azure-queues"></a>Что такое очереди Azure?

Хранилище очередей Azure — это служба для хранения большого количества сообщений. Доступ к сообщениям возможен из любой точки мира с помощью вызовов с проверкой подлинности по протоколу HTTP или HTTPS. Максимальный размер сообщения в очереди составляет 64 КБ. Очередь может содержать миллионы сообщений вплоть до лимита всей емкости учетной записи хранения. Очереди обычно используются для создания списка невыполненных заданий для асинхронной обработки.

## <a name="queue-service-concepts"></a>Основные понятия службы очередей

Служба очереди содержит следующие компоненты:

![Схема, на которой показана связь между учетной записью хранения, очередями и сообщениями](./media/storage-queues-introduction/queue1.png)

* **Формат URL-адреса**. К очереди можно обратиться, используя следующий формат URL-адреса:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Следующий URL-адрес позволяет обратиться к очереди на схеме:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Учетная запись хранения**. Весь доступ к хранилищу Azure осуществляется с помощью учетной записи хранения. Сведения о емкости учетной записи хранения см. в статье о [целевых показателях масштабируемости и производительности для учетных записей хранения ценовой категории "Стандартный"](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Очередь**. Очередь содержит набор сообщений. Имя очереди **должно** содержать только строчные символы. Дополнительные сведения см. в статье о [присвоении имен очередям и метаданным](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Сообщение**. Сообщение в любом формате размером до 64 КБ. В версиях, предшествовавших 2017-07-29, максимальный срок жизни сообщения составлял семь дней. Начиная с версии 2017-07-29, максимальный срок жизни может быть задан любым положительным числом или значением -1, свидетельствующим о том, что срок жизни сообщения неограничен. Если этот параметр не указан, срок жизни по умолчанию составляет семь дней.

## <a name="next-steps"></a>Дальнейшие действия

* [создать учетную запись хранения;](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Начало работы с очередями с использованием .NET](storage-dotnet-how-to-use-queues.md)
