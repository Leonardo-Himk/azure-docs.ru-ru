---
author: burkeholland
ms.service: static-web-apps
ms.topic: include
ms.date: 05/08/2020
ms.author: buhollan
ms.openlocfilehash: 879d0c2e8c4cd66ce04c0298d00f7d6a1987acf5
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594403"
---
#### <a name="local-proxy"></a>Локальный прокси-сервер

Вы можете настроить прокси-сервер для расширения Live Server Visual Studio Code, которое направляет все запросы к `/api` в работающую конечную точку API по адресу `http://127.0.0.1:7071/api`.

1. Откройте файл _vscode/settings.json_.

1. Добавьте следующие параметры, чтобы указать прокси-сервер для Live Server.

   ```json
   "liveServer.settings.proxy": {
      "enable": true,
      "baseUri": "/api",
      "proxyUri": "http://127.0.0.1:7071/api"
   }
   ```

   Эту конфигурацию лучше сохранить в файле параметров проекта, а не файле параметров пользователя.

   Использование параметров проекта гарантирует, что прокси-сервер не будет применяться ко всем другим проектам, открываемым в Visual Studio Code.
