---
title: Расширение система диагностики Azure для Windows
description: Мониторинг виртуальных машин Windows Azure с помощью расширения системы диагностики Azure
author: johnkemnetz
manager: ashwink
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: 1d38a3cac5525de6835bbb0f9873cbd0636d44a9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81869670"
---
# <a name="azure-diagnostics-extension-for-windows-vms"></a>Расширение системы диагностики Azure для виртуальных машин Windows

## <a name="overview"></a>Обзор

Расширение системы диагностики Azure для виртуальных машин позволяет собирать с виртуальной машины Windows данные мониторинга, например счетчики производительности и журналы событий. Вы можете детально настроить сбор данных и целевые объекты для передачи этих данных, например учетную запись хранения Azure или концентратор событий Azure. Также на основе этих данных вы сможете создать диаграммы или оповещения о метриках на портале Azure.

## <a name="prerequisites"></a>Предварительные условия

### <a name="operating-system"></a>Операционная система

Расширение системы диагностики Azure можно запустить в клиентской ОС Windows 10 и в Windows Server 2008 R2, 2012, 2012 R2 и 2016.

### <a name="internet-connectivity"></a>Подключение к Интернету

Для расширения системы диагностики Azure требуется, чтобы целевая виртуальная машина была подключена к Интернету. 

## <a name="extension-schema"></a>Схема расширения

[В этом документе описываются схемы и значения свойств для расширения системы диагностики Azure.](../../azure-monitor/platform/diagnostics-extension-schema-windows.md)

## <a name="template-deployment"></a>Развертывание шаблона

Расширения виртуальной машины Azure можно развернуть с помощью шаблонов Azure Resource Manager. Чтобы во время развертывания шаблона Azure Resource Manager запустить расширение системы диагностики Azure, включите в этот шаблон схему JSON, которая описана в предыдущем разделе. См. также [Использование мониторинга и системы диагностики с виртуальной машиной Windows и шаблонами Azure Resource Manager](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Развертывание с помощью Azure CLI

Azure CLI можно использовать для развертывания расширения системы диагностики Azure на существующей виртуальной машине. Замените защищенные параметры и свойства допустимыми аналогами JSON из представленной выше схемы. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>Развертывание с помощью PowerShell

С помощью команды `Set-AzVMDiagnosticsExtension` можно добавить расширение системы диагностики Azure на существующую виртуальную машину. См. также [Включение системы диагностики Azure на виртуальной машине под управлением Windows с помощью PowerShell](ps-extensions-diagnostics.md).

 


```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Данные о состоянии развертывания расширения можно получить на портале Azure, а также использовав Azure CLI. Чтобы просмотреть состояние развертывания расширений для определенной виртуальной машины, выполните следующую команду в Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

В [этой статье](../../azure-monitor/platform/diagnostics-extension-troubleshooting.md) вы найдете подробные рекомендации по устранению неполадок для расширения системы диагностики Azure.

### <a name="support"></a>Поддержка

Если вам нужна дополнительная помощь в любой момент в этой статье, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack overflow](https://azure.microsoft.com/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и выберите получить поддержку. Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Дальнейшие шаги
* [Дополнительные сведения о расширении системы диагностики Azure](../../azure-monitor/platform/diagnostics-extension-overview.md)
* [Журнал и версии схемы конфигурации расширения системы диагностики Azure](../../azure-monitor/platform/diagnostics-extension-schema-windows.md)
