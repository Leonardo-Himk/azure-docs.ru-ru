---
title: Создание моментального снимка виртуального жесткого диска в Azure
description: Узнайте, как создать копию виртуальной машины Azure в качестве резервной копии или для устранения неполадок.
author: roygara
manager: twooley
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 10/08/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 92957bd078c04a9bb7ac35f9d30f042a44e10764
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82100640"
---
# <a name="create-a-snapshot"></a>Создание моментального снимка

Моментальный снимок — это полная копия виртуального жесткого диска, доступная только для чтения. Создайте моментальный снимок диска операционной системы или виртуального жесткого диска данных для использования в качестве резервных ресурсов или устранения неполадок виртуальной машины.

Если вы намерены создать новую виртуальную машину на основе моментального снимка, мы рекомендуем аккуратно завершить работу виртуальной машины перед созданием моментального снимка, чтобы очистить все выполняющиеся процессы.

## <a name="use-the-azure-portal"></a>Использование портала Azure 

Чтобы создать моментальный снимок, выполните следующие действия. 
1.  На [портал Azure](https://portal.azure.com)выберите **создать ресурс**.
2. Найдите и выберите **snapshot (моментальный снимок**).
3. В окне **Моментальный снимок** нажмите кнопку **Создать**. Откроется окно **Создание моментального снимка**.
4. Заполните поле **Имя** для моментального снимка.
5. Введите имя новой [группы ресурсов](../../azure-resource-manager/management/overview.md#resource-groups) или выберите имеющуюся. 
6. Выберите **расположение** центра обработки данных Azure.  
7. В поле **Исходный диск** выберите управляемый диск, моментальный снимок которого необходимо создать.
8. Выберите **тип учетной записи**, которая будет использоваться для хранения моментального снимка. Выберите **Standard_HDD**, если вам не нужно хранить моментальный снимок на высокопроизводительном диске.
9. Щелкните **Создать**.

## <a name="use-powershell"></a>Использование PowerShell

В следующих шагах показано, как скопировать диск виртуального жесткого диска и создать конфигурацию моментального снимка. Затем можно создать моментальный снимок диска с помощью командлета [New-азснапшот](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot) . 

 

1. Задайте некоторые параметры: 

   ```azurepowershell-interactive
   $resourceGroupName = 'myResourceGroup' 
   $location = 'eastus' 
   $vmName = 'myVM'
   $snapshotName = 'mySnapshot'  
   ```

2. Получите виртуальную машину.

   ```azurepowershell-interactive
   $vm = get-azvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
   ```

3. Создайте конфигурацию моментального снимка. В этом примере моментальный снимок — это снимок диска операционной системы:

   ```azurepowershell-interactive
   $snapshot =  New-AzSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
   ```
   
   > [!NOTE]
   > Перед сохранением моментального снимка в хранилище, отказоустойчивое в пределах зоны, его необходимо создать в регионе, который поддерживает [зоны доступности](../../availability-zones/az-overview.md) и в котором включен параметр `-SkuName Standard_ZRS`.   
   
4. Создайте моментальный снимок.

   ```azurepowershell-interactive
   New-AzSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
   ```


## <a name="next-steps"></a>Дальнейшие шаги

Создайте виртуальную машину из моментального снимка, преобразовав его в управляемый диск, а затем подключив этот диск как диск ОС. Дополнительные сведения см. в статье [Создание виртуальной машины из моментального снимка с помощью PowerShell](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json).
