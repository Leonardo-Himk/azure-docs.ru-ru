---
title: Изменение ключей доступа к учетной записи хранения
titleSuffix: Azure Machine Learning
description: Узнайте, как изменить ключи доступа для учетной записи хранения Azure, используемой рабочей областью. Машинное обучение Azure использует учетную запись хранения Azure для хранения данных и моделей.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 03/06/2020
ms.openlocfilehash: f1541c177cea2d223a5e7df576d95fab7eafb310
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80296941"
---
# <a name="regenerate-storage-account-access-keys"></a>Повторное создание ключей доступа для учетной записи хранения
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Узнайте, как изменить ключи доступа для учетных записей хранения Azure, используемых Машинное обучение Azure. Машинное обучение Azure могут использовать учетные записи хранения для хранения данных или обученных моделей.

В целях безопасности может потребоваться изменить ключи доступа для учетной записи хранения Azure. При повторном создании ключа доступа Машинное обучение Azure необходимо обновить, чтобы использовать новый ключ. Машинное обучение Azure может использовать учетную запись хранения как для хранилища модели, так и для хранилища данных.

## <a name="prerequisites"></a>Предварительные условия

* Рабочая область машинного обучения Azure. Дополнительные сведения см. в статье [Создание рабочей области](how-to-manage-workspace.md) .

* [Пакет SDK для машинное обучение Azure](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [Расширение CLI машинное обучение Azure](reference-azure-machine-learning-cli.md).

> [!NOTE]
> Фрагменты кода в этом документе были протестированы с версией 1.0.83 пакета SDK для Python.

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>Что необходимо обновить

Учетные записи хранения могут использоваться рабочей областью Машинное обучение Azure (хранение журналов, моделей, моментальных снимков и т. д.) и в качестве хранилища данных. Процесс обновления рабочей области является единственной командой Azure CLI и может быть запущен после обновления ключа хранилища. Процесс обновления хранилищ данных больше вовлечен и требует обнаружения того, какие хранилища данных в настоящее время используют эту учетную запись хранения, а затем их повторная регистрация.

> [!IMPORTANT]
> Одновременно обновите рабочую область, используя Azure CLI и хранилища данных с помощью Python. Обновление только одного или другого не является достаточным, и может привести к ошибкам, пока оба не будут обновлены.

Чтобы найти учетные записи хранения, используемые хранилищами данных, используйте следующий код:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " +
      default_ds.account_name + ", container name: " + default_ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob":
        print("Blob store - datastore name: " + name + ", storage account name: " +
              ds.account_name + ", container name: " + ds.container_name)
    if ds.datastore_type == "AzureFile":
        print("File share - datastore name: " + name + ", storage account name: " +
              ds.account_name + ", container name: " + ds.container_name)
```

Этот код ищет все зарегистрированные хранилища данных, в которых используется служба хранилища Azure, и перечисляет следующие сведения:

* Имя хранилища данных: имя хранилища данных, в котором зарегистрирована учетная запись хранения.
* Имя учетной записи хранения — имя учетной записи хранения Azure.
* Контейнер: контейнер в учетной записи хранения, используемый при регистрации.

Оно также указывает, предназначено ли хранилище данных для большого двоичного объекта Azure или файлового ресурса Azure, так как существуют различные методы повторной регистрации каждого типа хранилища данных.

Если для учетной записи хранения, в которой планируется повторное создание ключей доступа, существует запись, то сохраните имя хранилища данных, имя учетной записи хранения и имя контейнера.

## <a name="update-the-access-key"></a>Обновление ключа доступа

Чтобы обновить Машинное обучение Azure для использования нового ключа, выполните следующие действия.

> [!IMPORTANT]
> Выполните все действия, обновив рабочую область с помощью интерфейса командной строки и хранилища данных с помощью Python. Обновление только одного или другого может вызвать ошибки до тех пор, пока оба обновления не будут обновлены.

1. Повторно создайте ключ. Сведения о повторном создании ключа доступа см. в статье [Управление ключами доступа учетной записи хранения](../storage/common/storage-account-keys-manage.md). Сохраните новый ключ.

1. Чтобы обновить рабочую область для использования нового ключа, выполните следующие действия.

    1. Чтобы войти в подписку Azure, содержащую рабочую область, используйте следующую команду Azure CLI:

        ```azurecli-interactive
        az login
        ```

        [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)]

    1. Чтобы обновить рабочую область для использования нового ключа, используйте следующую команду. Замените `myworkspace` именем рабочей области машинное обучение Azure и замените `myresourcegroup` именем группы ресурсов Azure, содержащей рабочую область.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

        Эта команда автоматически синхронизирует новые ключи для учетной записи хранения Azure, используемой рабочей областью.

1. Для повторной регистрации хранилищ данных, использующих учетную запись хранения, используйте значения из раздела [что необходимо обновить](#whattoupdate) и ключ из шага 1 в следующем коде:

    ```python
    # Re-register the blob container
    ds_blob = Datastore.register_azure_blob_container(workspace=ws,
                                              datastore_name='your datastore name',
                                              container_name='your container name',
                                              account_name='your storage account name',
                                              account_key='new storage account key',
                                              overwrite=True)
    # Re-register file shares
    ds_file = Datastore.register_azure_file_share(workspace=ws,
                                          datastore_name='your datastore name',
                                          file_share_name='your container name',
                                          account_name='your storage account name',
                                          account_key='new storage account key',
                                          overwrite=True)
    
    ```

    Так `overwrite=True` как указан, этот код перезаписывает существующую регистрацию и обновляет ее для использования нового ключа.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о регистрации хранилищ данных см. в справочнике [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) по классам.
