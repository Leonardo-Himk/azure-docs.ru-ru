---
title: Расширение драйвера GPU NVIDIA — виртуальные машины Azure под управлением Windows
description: Расширение Microsoft Azure для установки драйверов GPU NVIDIA на вычислительных виртуальных машинах серии N под управлением Windows.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: akjosh
ms.openlocfilehash: c388f433327b5328483f10fbef637a6fdfd08832
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79250547"
---
# <a name="nvidia-gpu-driver-extension-for-windows"></a>Расширение драйвера GPU NVIDIA для Windows

## <a name="overview"></a>Обзор

Это расширение устанавливает драйверы GPU NVIDIA на виртуальных машинах серии N для Windows. В зависимости от семейства виртуальных машин расширение устанавливает драйверы CUDA или GRID. При установке драйверов NVIDIA с помощью этого расширения требуется принять условия [лицензионного соглашения NVIDIA](https://go.microsoft.com/fwlink/?linkid=874330). Во время установки драйвера виртуальная машина может быть перезагружена для завершения процедуры.

Инструкции по установке драйверов вручную и текущим поддерживаемым версиям доступны [здесь](https://docs.microsoft.com/azure/virtual-machines/windows/n-series-driver-setup).
Это расширение также доступно для установки драйверов GPU NVIDIA на [виртуальных машинах Linux серии N](hpccompute-gpu-linux.md).

## <a name="prerequisites"></a>Предварительные условия

### <a name="operating-system"></a>Операционная система

Это расширение поддерживает следующие операционные системы:

| Distribution | Версия |
|---|---|
| быть под управлением ОС Windows 10; | Ядро |
| Windows Server 2016 | Ядро |
| Windows Server 2012 R2 | Ядро |

### <a name="internet-connectivity"></a>Подключение к Интернету

Для работы расширения Microsoft Azure для драйверов GPU NVIDIA требуется, чтобы целевая виртуальная машина была доступна и подключена к Интернету.

## <a name="extension-schema"></a>Схема расширения

В следующем коде JSON показана схема расширения.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>Элемент Property

| Имя | Значение и пример | Тип данных |
| ---- | ---- | ---- |
| версия_API | 2015-06-15 | Дата |
| publisher | Microsoft.HpcCompute | строка |
| type | NvidiaGpuDriverWindows | строка |
| typeHandlerVersion | 1.2 | INT |


## <a name="deployment"></a>Развертывание

### <a name="azure-resource-manager-template"></a>Шаблон Azure Resource Manager 

Расширения виртуальной машины Azure можно развернуть с помощью шаблонов Azure Resource Manager. Шаблоны идеально подходят для развертывания одной или нескольких виртуальных машин, требующих настройки после развертывания.

Конфигурацию JSON для расширения виртуальной машины можно вложить в ресурс виртуальной машины или поместить в корень или на верхний уровень JSON-файла шаблона Resource Manager. Размещение конфигурации JSON влияет на значения имени и типа ресурса. Дополнительные сведения см. в разделе [Задание имени и типа для дочерних ресурсов](../../azure-resource-manager/resource-manager-template-child-resource.md). 

В следующем примере предполагается, что расширение виртуальной машины расположено в ресурсе виртуальной машины. При вложении ресурса расширения JSON помещается в объект `"resources": []` виртуальной машины.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverWindows" `
    -ExtensionType "NvidiaGpuDriverWindows" `
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverWindows `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
  }'
```

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Диагностика

Сведения о состоянии развертывания расширения можно получить на портале Azure, а также с помощью Azure PowerShell или Azure CLI. Чтобы просмотреть состояние развертывания расширений для определенной виртуальной машины, выполните следующую команду.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Выходные данные выполнения расширения регистрируются в следующем каталоге:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverMicrosoft\
```

### <a name="error-codes"></a>Коды ошибок

| Код ошибки | Значение | Возможное действие |
| :---: | --- | --- |
| 0 | Операция выполнена успешно |
| 1 | Операция выполнена успешно. Требуется перезагрузка. |
| 100 | Операция не поддерживается или не может быть выполнена. | Возможные причины: версия PowerShell не поддерживается, размер виртуальной машины отличается от размера виртуальной машины серии N, сбой при загрузке данных. Проверьте файлы журнала, чтобы определить причину ошибки. |
| 240, 840 | Время ожидания операции истекло. | Повторите операцию. |
| -1 | Возникло исключение. | Проверьте файлы журнала, чтобы определить причину исключения. |
| -5x | Операция прервана из-за ожидаемой перезагрузки. | Перезагрузите виртуальную машину. Установка будет продолжена после перезагрузки. Удаление должно быть вызвано вручную. |


### <a name="support"></a>Поддержка

Если вам нужна дополнительная помощь в любой момент в этой статье, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack overflow](https://azure.microsoft.com/support/community/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и выберите получить поддержку. Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Дальнейшие шаги
См. дополнительные сведения о [расширениях и компонентах виртуальных машин Windows](features-windows.md).

См. дополнительные сведения о [размерах виртуальных машин серии N, оптимизированных для GPU](../windows/sizes-gpu.md).
