---
title: Запуск Azure IoT Edge на виртуальных машинах Ubuntu | Документация Майкрософт
description: Инструкции по установке Azure IoT Edge для виртуальных машин Ubuntu 18,04 LTS
author: toolboc
manager: veyalla
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: pdecarlo
ms.openlocfilehash: 64e2787aa282e75893fa34e6de1373e6afed09fe
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80349597"
---
# <a name="run-azure-iot-edge-on-ubuntu-virtual-machines"></a>Запуск Azure IoT Edge на виртуальных машинах Ubuntu

Среда выполнения Azure IoT Edge превращает устройство в устройство IoT Edge. Среду выполнения можно развернуть на всех устройствах — от небольшого Raspberry Pi до технического сервера. Как только на устройстве будет настроена среда выполнения IoT Edge, вы можете начать развертывать в нее бизнес-логику из облака.

Дополнительные сведения о работе среды выполнения IoT Edge и ее компонентах см. в статье [Общие сведения о среде выполнения Azure IoT Edge и ее архитектуре](iot-edge-runtime.md).

В этой статье перечислены действия по развертыванию виртуальной машины Ubuntu 18,04 LTS с установленной и настроенной средой выполнения Azure IoT Edge с использованием предварительно указанной строки подключения устройства. Развертывание выполняется с помощью [шаблона Azure Resource Manager](../azure-resource-manager/templates/overview.md) на основе [Cloud-init](../virtual-machines/linux/using-cloud-init.md
) , поддерживаемого в репозитории проектов [iotedge-VM-Deploy](https://github.com/Azure/iotedge-vm-deploy) .

При первой загрузке виртуальная машина Ubuntu 18,04 LTS [устанавливает последнюю версию среды выполнения Azure IOT Edge через Cloud-init](https://github.com/Azure/iotedge-vm-deploy/blob/master/cloud-init.txt). Он также задает предоставленную строку подключения перед запуском среды выполнения, что позволяет легко настраивать и подключать устройство IoT Edge без необходимости запуска сеанса SSH или удаленного рабочего стола. 

## <a name="deploy-using-deploy-to-azure-button"></a>Развертывание с помощью кнопки "развернуть в Azure"

[Кнопка развертывание в Azure](../azure-resource-manager/templates/deploy-to-azure-button.md) позволяет упростить развертывание [шаблонов Azure Resource Manager](../azure-resource-manager/templates/overview.md) , поддерживаемых на GitHub.  В этом разделе будет показано использование кнопки развернуть в Azure в репозитории проектов [iotedge-VM-Deploy](https://github.com/Azure/iotedge-vm-deploy) .  


1. Вы развернете виртуальную машину Linux с поддержкой Azure IoT Edge с помощью шаблона Azure Resource Manager iotedge-VM-Deploy.  Для начала нажмите кнопку ниже:

    [![Кнопка развертывания в Azure для iotedge-VM-Deploy](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fiotedge-vm-deploy%2Fmaster%2FedgeDeploy.json)

1. В вновь запущенном окне заполните доступные поля формы:

    > [!div class="mx-imgBorder"]
    > [![Снимок экрана, показывающий шаблон iotedge-VM-Deploy](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-deploy.png)](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-deploy.png)

    **Подписка**: активная подписка Azure для развертывания виртуальной машины.

    **Группа ресурсов**: существующая или только что созданная группа ресурсов, содержащая виртуальную машину и связанные с ней ресурсы.

    **Префикс метки DNS**. необходимое значение, которое используется для добавления префикса имени узла виртуальной машины.

    **Имя пользователя администратора**: имя пользователя, которому будут предоставлены привилегии root при развертывании.

    **Строка подключения устройства**. [строка подключения](how-to-register-device.md) устройства, созданного в нужном [центре Интернета вещей](../iot-hub/about-iot-hub.md).

    **Размер виртуальной машины**: [Размер](../cloud-services/cloud-services-sizes-specs.md) развертываемой виртуальной машины.

    **Версия ОС Ubuntu**: версия операционной системы Ubuntu, устанавливаемая на базовую виртуальную машину.

    **Расположение**. [географический регион](https://azure.microsoft.com/global-infrastructure/locations/) для развертывания виртуальной машины в это значение по умолчанию равно расположению выбранной группы ресурсов.

    **Тип проверки подлинности**. Выберите **sshPublicKey** или **пароль** в зависимости от ваших предпочтений.

    **Пароль или ключ администратора**: значение открытого ключа SSH или значение пароля в зависимости от выбранного типа проверки подлинности.

    Когда все поля заполнены, установите флажок в нижней части страницы, чтобы принять условия, и выберите **купить** , чтобы начать развертывание.

1. Убедитесь, что развертывание успешно завершено.  Ресурс виртуальной машины должен быть развернут в выбранной группе ресурсов.  Запишите имя компьютера, оно должно быть в формате `vm-0000000000000`. Кроме того, обратите внимание на соответствующее **DNS-имя**, которое должно быть в `<dnsLabelPrefix>`формате. `<location>`. cloudapp.Azure.com.

    **DNS-имя** можно получить из раздела **Обзор** только что развернутой виртуальной машины в портал Azure.

    > [!div class="mx-imgBorder"]
    > [![Снимок экрана, показывающий DNS-имя виртуальной машины iotedge](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)

1. Если вы хотите установить подключение по протоколу SSH к этой виртуальной машине после установки, используйте соответствующее **DNS-имя** с помощью команды:`ssh <adminUsername>@<DNS_Name>`

## <a name="deploy-from-azure-cli"></a>Развертывание с помощью Azure CLI

1. Убедитесь, что вы установили расширение Azure CLI IOT с помощью:
    ```azurecli-interactive
    az extension add --name azure-iot
    ```

1. Затем, если вы используете Azure CLI на рабочем столе, начните с входа в систему:

   ```azurecli-interactive
   az login
   ```

1. Если у вас несколько подписок, выберите подписку, которую вы хотите использовать:
   1. Отобразите список подписок:

      ```azurecli-interactive
      az account list --output table
      ```

   1. Скопируйте поле SubscriptionID для подписки, которую вы хотите использовать.

   1. Укажите в рабочей подписке идентификатор, который вы скопировали:

      ```azurecli-interactive
      az account set -s <SubscriptionId>
      ```

1. Создайте группу ресурсов (или выберите уже имеющуюся на следующих этапах):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

1. Создайте виртуальную машину:

    Чтобы использовать **authenticationType** из `password`, см. пример ниже.

   ```azurecli-interactive
   az group deployment create \
   --name edgeVm \
   --resource-group IoTEdgeResources \
   --template-uri "https://aka.ms/iotedge-vm-deploy" \
   --parameters dnsLabelPrefix='my-edge-vm1' \
   --parameters adminUsername='<REPLACE_WITH_USERNAME>' \
   --parameters deviceConnectionString=$(az iot hub device-identity show-connection-string --device-id <REPLACE_WITH_DEVICE-NAME> --hub-name <REPLACE-WITH-HUB-NAME> -o tsv) \
   --parameters authenticationType='password' \
   --parameters adminPasswordOrKey="<REPLACE_WITH_SECRET_PASSWORD>"
   ```

    Для проверки подлинности с помощью ключа SSH это можно сделать, указав **authenticationType** из `sshPublicKey`, а затем укажите значение ключа SSH в параметре **админпассвордоркэй** .  Ниже приведен пример такого файла.

    ```azurecli-interactive
    #Generate the SSH Key
    ssh-keygen -m PEM -t rsa -b 4096 -q -f ~/.ssh/iotedge-vm-key -N ""  

    #Create a VM using the iotedge-vm-deploy script
    az group deployment create \
    --name edgeVm \
    --resource-group IoTEdgeResources \
    --template-uri "https://aka.ms/iotedge-vm-deploy" \
    --parameters dnsLabelPrefix='my-edge-vm1' \
    --parameters adminUsername='<REPLACE_WITH_USERNAME>' \
    --parameters deviceConnectionString=$(az iot hub device-identity show-connection-string --device-id <REPLACE_WITH_DEVICE-NAME> --hub-name <REPLACE-WITH-HUB-NAME> -o tsv) \
    --parameters authenticationType='sshPublicKey' \
    --parameters adminPasswordOrKey="$(< ~/.ssh/iotedge-vm-key.pub)"
     
    ```

1. Убедитесь, что развертывание успешно завершено.  Ресурс виртуальной машины должен быть развернут в выбранной группе ресурсов.  Запишите имя компьютера, оно должно быть в формате `vm-0000000000000`. Кроме того, обратите внимание на соответствующее **DNS-имя**, которое должно быть в `<dnsLabelPrefix>`формате. `<location>`. cloudapp.Azure.com.

    **DNS-имя** можно получить из выходных данных в формате JSON предыдущего шага в разделе **Outputs (выходные** данные) как часть **общедоступной записи SSH** .  Значение этой записи можно использовать для подключения к только что развернутой машине по протоколу SSH.

    ```bash
    "outputs": {
      "public SSH": {
        "type": "String",
        "value": "ssh <adminUsername>@<DNS_Name>"
      }
    }
    ```

    **DNS-имя** также можно получить из раздела **Обзор** только что развернутой виртуальной машины в портал Azure.

    > [!div class="mx-imgBorder"]
    > [![Снимок экрана, показывающий DNS-имя виртуальной машины iotedge](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)

1. Если вы хотите установить подключение по протоколу SSH к этой виртуальной машине после установки, используйте соответствующее **DNS-имя** с помощью команды:`ssh <adminUsername>@<DNS_Name>`

## <a name="next-steps"></a>Дальнейшие шаги

Теперь, когда подготовлено устройство IoT Edge и установлена среда выполнения, вы можете [развернуть модули IoT Edge](how-to-deploy-modules-portal.md).

Если при правильной установке среды выполнения IoT Edge возникли проблемы, ознакомьтесь со страницей [устранения неполадок](troubleshoot.md) .

Чтобы обновить существующую установку до последней версии IoT Edge, см. раздел [Обновление управляющей программы безопасности и среды выполнения IoT Edge](how-to-update-iot-edge.md).

Если вы хотите открыть порты для доступа к виртуальной машине через SSH или другие входящие подключения, обратитесь к документации по виртуальным машинам Azure, посвященной [открытию портов и конечных точек для виртуальной машины Linux](../virtual-machines/linux/nsg-quickstart.md) .
