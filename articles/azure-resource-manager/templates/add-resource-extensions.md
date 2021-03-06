---
title: Настройка после развертывания с помощью расширений
description: Узнайте, как использовать расширения шаблона Azure Resource Manager для предоставления конфигураций после развертывания.
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: b3c4110c8761b3e8daf324d65ac7fa1dcbcdf61f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77023503"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Предоставление конфигураций после развертывания с помощью расширений

Расширения шаблона — это небольшие приложения, которые выполняют задачи конфигурации и автоматизации после развертывания ресурсов Azure. Самые популярные — расширения виртуальных машин. Ознакомьтесь со статьей [Обзор расширений и компонентов виртуальной машины под управлением Windows](../../virtual-machines/extensions/features-windows.md) или [Обзор расширений и компонентов виртуальных машин под управлением Linux](../../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Расширения

Ниже приведены имеющиеся расширения:

- [Microsoft. COMPUTE/virtualMachines/Extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight clusters/extensions](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters)
- [Microsoft.Sql servers/databases/extensions](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Чтобы узнать доступные расширения, перейдите по [ссылке на шаблон](https://docs.microsoft.com/azure/templates/). В **фильтре по названию** введите **расширение**.

Подробнее об использовании этих расширений см. в следующих статьях:

- [Руководство. развертывание расширений виртуальной машины с помощью шаблонов Azure Resource Manager](template-tutorial-deploy-vm-extensions.md).
- [Руководство. Импорт BACPAC-файлов SQL с помощью шаблонов Azure Resource Manager](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Дальнейшие шаги

> [!div class="nextstepaction"]
> [Руководство. Развертывание расширений виртуальной машины с помощью шаблонов Azure Resource Manager](template-tutorial-deploy-vm-extensions.md)
