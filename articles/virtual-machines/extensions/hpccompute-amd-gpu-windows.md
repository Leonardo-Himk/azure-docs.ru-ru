---
title: Расширение драйвера GPU AMD — виртуальные машины Azure под управлением Windows
description: Расширение Microsoft Azure для установки драйверов GPU AMD на виртуальных машинах серии NVv4 под управлением Windows.
services: virtual-machines-windows
documentationcenter: ''
author: vikancha
manager: jkabat
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/10/2020
ms.author: vikancha
ms.openlocfilehash: 0819eec5275fa9783fff363cabbd8c3ed22b7cd6
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83119558"
---
# <a name="amd-gpu-driver-extension-for-windows"></a>Расширение драйвера GPU AMD для Windows

В этой статье представлен обзор расширения виртуальной машины, которое позволяет развертывать драйверы GPU AMD на виртуальных машинах Windows [серии NVv4](https://docs.microsoft.com/azure/virtual-machines/nvv4-series) . При установке драйверов AMD с помощью этого расширения вы принимаете и принимаете условия [лицензионного соглашения AMD](https://amd.com/radeonsoftwarems). Во время установки драйвера виртуальная машина может быть перезагружена для завершения процедуры.

Инструкции по установке драйверов вручную и текущим поддерживаемым версиям доступны [здесь](https://docs.microsoft.com/azure/virtual-machines/windows/n-series-amd-driver-setup).

## <a name="prerequisites"></a>Предварительные требования

### <a name="operating-system"></a>Операционная система

Это расширение поддерживает следующие операционные системы:

| Distribution | Версия |
|---|---|
| Windows 10 EMS | Сборка 1903 |
| Windows 10 | Сборка 1809 |
| Windows Server 2016 | Ядро |
| Windows Server 2019 | Ядро |

### <a name="internet-connectivity"></a>Подключение к Интернету

Для расширения Microsoft Azure для драйверов AMD GPU требуется, чтобы Целевая виртуальная машина была подключена к Интернету и была доступна.

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
    "type": "AmdGpuDriverWindows",
    "typeHandlerVersion": "1.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>Свойства

| Название | Значение и пример | Тип данных |
| ---- | ---- | ---- |
| версия_API | 2015-06-15 | Дата |
| publisher | Microsoft.HpcCompute | строка |
| type | амдгпудривервиндовс | строка |
| typeHandlerVersion | 1.0 | INT |


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
    "type": "AmdGpuDriverWindows",
    "typeHandlerVersion": "1.0",
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
    -ExtensionName "AmdGpuDriverWindows" `
    -ExtensionType "AmdGpuDriverWindows" `
    -TypeHandlerVersion 1.0 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name AmdGpuDriverWindows `
  --publisher Microsoft.HpcCompute `
  --version 1.0 `
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

## <a name="next-steps"></a>Дальнейшие действия
См. дополнительные сведения о [расширениях и компонентах виртуальных машин Windows](features-windows.md).

См. дополнительные сведения о [размерах виртуальных машин серии N, оптимизированных для GPU](../windows/sizes-gpu.md).
