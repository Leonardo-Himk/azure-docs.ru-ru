---
title: Автоматическое расширение хранилища — Azure CLI — база данных Azure для MariaDB
description: В этой статье описывается, как включить автоматическое расширение хранилища с помощью Azure CLI в базе данных Azure для MariaDB.
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 4be84c750f6a3ca7a0d48aa2b98d75272c1cbadf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79529090"
---
# <a name="auto-grow-azure-database-for-mariadb-storage-using-the-azure-cli"></a>Автоматическое увеличение хранилища базы данных Azure для MariaDB с помощью Azure CLI
В этой статье описывается, как можно настроить расширение хранилища базы данных Azure для MariaDB, не влияя на рабочую нагрузку.

Сервер достигает [предельного размера хранилища](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#reaching-the-storage-limit)и устанавливается в значение "только для чтения". Если автоматическое увеличение размера хранилища включено, то для серверов с подготовленным хранилищем объемом менее 100 ГБ размер подготовленного хранилища увеличивается на 5 ГБ, как только объем свободного хранилища превысит 1 ГБ или 10% подготовленного хранилища. Для серверов с более чем 100 ГБ подготовленного хранилища размер подготовленного хранилища увеличивается на 5%, если объем свободного пространства превышает 5% от подготовленного объема хранилища. Максимальный объем хранилища, указанный [здесь](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) , применим.

## <a name="prerequisites"></a>Предварительные условия
Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:
- [Сервер базы данных Azure для MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы проверить версию, в командной строке Azure CLI введите `az --version`. Чтобы выполнить установку или обновление, см. сведения в статье [Установка Azure CLI]( /cli/azure/install-azure-cli).

## <a name="enable-mariadb-server-storage-auto-grow"></a>Включить автоматическое увеличение хранилища сервера MariaDB

Включите автоматическое расширение хранилища на сервере на существующем сервере с помощью следующей команды:

```azurecli-interactive
az mariadb server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Включите автоматическое увеличение размера сервера при создании нового сервера с помощью следующей команды:

```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.3
```

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте [, как создавать оповещения по метрикам](howto-alert-metric.md).