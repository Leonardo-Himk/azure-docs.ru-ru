---
title: включить файл
description: включить файл
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 11/05/2019
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: a737e130d616a67bab28c7c96c0372216a6707af
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2020
ms.locfileid: "73720315"
---
### <a name="retrieve-host-name-ports-and-access-keys-from-the-azure-portal"></a>Получение имени узла, портов и ключей доступа с помощью портала Azure

Чтобы подключаться к экземпляру Кэша Azure для Redis, клиентам кэша требуются имя узла, сведения о портах и ключ кэша. Некоторые клиенты могут ссылаться на эти элементы с помощью незначительно различающихся имен. Вы можете получить имя узла, порты и ключи на [портале Azure](https://portal.azure.com).

- Чтобы получить ключи доступа, в области навигации кэша слева щелкните **Ключи доступа**. 
  
  ![Ключи кэша Azure для Redis](media/redis-cache-access-keys/redis-cache-keys.png)

- Чтобы получить имя узла и порты, в области навигации кэша слева щелкните **Свойства**. Имя узла имеет вид *\<DNS-имя>.redis.cache.windows.net*.

  ![Свойства кэша Azure для Redis](media/redis-cache-access-keys/redis-cache-hostname-ports.png)

