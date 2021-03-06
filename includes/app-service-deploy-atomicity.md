---
title: включить файл
description: включить файл
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/07/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: feed8b21833d4244d027d64d5e6547b94e4fa66f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75945146"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Что происходит с моим приложением во время развертывания?

Все официально поддерживаемые методы развертывания вносит изменения в файлы в `/home/site/wwwroot` папке приложения. Эти файлы используются для запуска приложения. Поэтому развертывание может завершиться ошибкой из-за заблокированных файлов. Приложение может также вести себя непредсказуемо во время развертывания, так как не все файлы обновляются одновременно. Это нежелательно для приложения, ориентированного на клиента. Избежать этих проблем можно несколькими способами.

- [Запустите приложение из ZIP-пакета напрямую](../articles/app-service/deploy-run-package.md) без его распаковки.
- Остановите приложение или включите для него автономный режим на время развертывания. Дополнительные сведения см. в разделе Работа [с заблокированными файлами во время развертывания](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Выполняйте развертывание в [промежуточный слот](../articles/app-service/deploy-staging-slots.md) с включенным [автоматическим переключением](../articles/app-service/deploy-staging-slots.md#configure-auto-swap). 
