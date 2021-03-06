---
title: Запуск приложения из ZIP-пакета
description: Развертывание ZIP-пакета приложения с атомарностью. Повысьте предсказуемость и надежность работы приложения во время процесса развертывания ZIP.
ms.topic: article
ms.date: 01/14/2020
ms.openlocfilehash: 5cc909d79b3f5ea2b4c6a3da12bc7250addbe00c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77920728"
---
# <a name="run-your-app-in-azure-app-service-directly-from-a-zip-package"></a>Запуск приложения в службе приложений Azure непосредственно из ZIP-пакета

В [службе приложений Azure](overview.md)приложения можно запускать непосредственно из файла пакета развертывания ZIP. В этой статье показано, как включить эту функцию в приложении.

Все остальные методы развертывания в службе приложений имеют что-то общее: файлы развертываются в *D:\home\site\wwwroot* в приложении (или */Хоме/Сите/ввврут* для приложений Linux). Так как в среде выполнения приложение использует тот же каталог, развертывание может завершиться неудачей из-за конфликтов блокировки файлов, и приложение может вести себя непредсказуемо, так как некоторые файлы еще не обновлены.

В отличие от этого, при запуске непосредственно из пакета файлы в пакете не копируются в каталог *wwwroot* . Вместо этого сам ZIP-пакет монтируется непосредственно в каталог *wwwroot* только для чтения. Существует несколько преимуществ, выполняемых непосредственно из пакета.

- Устраняет конфликты блокировки файлов между развертыванием и средой выполнения.
- Гарантирует, что в любое время выполняются только полностью развернутые приложения.
- Можно выполнить развертывание в рабочее приложение (с помощью перезапуска).
- Повышается производительность развертываний Azure Resource Manager.
- Может сократиться время "холодного запуска", особенно для функций JavaScript с большими деревьями пакетов npm.

> [!NOTE]
> В настоящее время поддерживаются только ZIP-файлы пакетов.

[!INCLUDE [Create a project ZIP file](../../includes/app-service-web-deploy-zip-prepare.md)]

## <a name="enable-running-from-package"></a>Включить Запуск из пакета

Параметр `WEBSITE_RUN_FROM_PACKAGE` приложения включает запуск из пакета. Чтобы задать его, выполните следующую команду с Azure CLI.

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBSITE_RUN_FROM_PACKAGE="1"
```

`WEBSITE_RUN_FROM_PACKAGE="1"`позволяет запускать приложение из пакета, который является локальным для приложения. Можно также [запустить из удаленного пакета](#run-from-external-url-instead).

## <a name="run-the-package"></a>Запуск пакета

Проще всего запустить пакет в службе приложений с помощью команды Azure CLI [AZ webapp Deployment Source config-ZIP](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) . Пример:

```azurecli-interactive
az webapp deployment source config-zip --resource-group <group-name> --name <app-name> --src <filename>.zip
```

Так как `WEBSITE_RUN_FROM_PACKAGE` параметр приложения задан, эта команда не извлекает содержимое пакета в каталог *D:\home\site\wwwroot* приложения. Вместо этого он передает ZIP-файл "как есть" в *д:\хоме\дата\ситепаккажес*и создает *packagename. txt* в том же каталоге, который содержит имя пакета zip для загрузки во время выполнения. При отправке ZIP-пакета другим способом (например, [FTP](deploy-ftp.md)) необходимо вручную создать каталог *д:\хоме\дата\ситепаккажес* и файл *packagename. txt* .

Команда также перезапускает приложение. Так `WEBSITE_RUN_FROM_PACKAGE` как параметр задан, служба приложений подключает отправленный пакет как каталог *wwwroot* только для чтения и запускает приложение непосредственно из этого подключенного каталога.

## <a name="run-from-external-url-instead"></a>Запустить из внешнего URL-адреса

Вы также можете запустить пакет из внешнего URL-адреса, например хранилища больших двоичных объектов Azure. Можно использовать [Обозреватель службы хранилища Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md) для передачи файлов пакета в учетную запись хранения больших двоичных объектов. Для безопасного доступа к пакету среда выполнения службы приложений должна использовать частный контейнер хранилища с [подписанным URL-адресом (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) . 

Когда вы отправите файл в хранилище BLOB-объектов и получите для него URL для SAS, `WEBSITE_RUN_FROM_PACKAGE` задайте для параметра приложения URL-адрес. В следующем примере он выполняется с помощью Azure CLI:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_RUN_FROM_PACKAGE="https://myblobstorage.blob.core.windows.net/content/SampleCoreMVCApp.zip?st=2018-02-13T09%3A48%3A00Z&se=2044-06-14T09%3A48%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=bNrVrEFzRHQB17GFJ7boEanetyJ9DGwBSV8OM3Mdh%2FM%3D"
```

При публикации обновленного пакета с тем же именем в хранилище BLOB-объектов необходимо перезапустить приложение, чтобы обновленный пакет загружался в службу приложений.

## <a name="troubleshooting"></a>Устранение неполадок

- Выполнение непосредственно из пакета делает `wwwroot` его доступным только для чтения. При попытке записи файлов в этот каталог приложение получит сообщение об ошибке.
- Форматы TAR и GZIP не поддерживаются.
- Эта функция несовместима с [локальным кэшем](overview-local-cache.md).
- Для повышения производительности холодного запуска используйте параметр local ZIP (`WEBSITE_RUN_FROM_PACKAGE`= 1).

## <a name="more-resources"></a>Дополнительные ресурсы

- [Непрерывное развертывание для службы приложений Azure](deploy-continuous-deployment.md)
- [Развертывание кода с помощью ZIP-файла или с файлом WAR](deploy-zip.md)
