---
title: Экспорт шаблона Azure Resource Manager задания Azure Stream Analytics
description: В этой статье описывается, как экспортировать шаблон Azure Resource Manager для задания Azure Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 52ea7b45d0dcdb3ae16b8212557ba6ab3344ff15
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78968935"
---
# <a name="export-an-azure-stream-analytics-job-azure-resource-manager-template"></a>Экспорт шаблона Azure Resource Manager задания Azure Stream Analytics

[Шаблоны Azure Resource Manager](../azure-resource-manager/templates/overview.md) позволяют реализовать инфраструктуру как код. Шаблон — это файл нотация объектов JavaScript (JSON), который определяет инфраструктуру и конфигурацию ресурсов. Вы указываете развертываемые ресурсы и свойства этих ресурсов.

Вы можете повторно развернуть задание Azure Stream Analytics, экспортировав шаблон Azure Resource Manager.

## <a name="open-a-job-in-vs-code"></a>Открытие задания в VS Code

Перед экспортом шаблона необходимо сначала открыть существующее задание Stream Analytics в Visual Studio Code. 

Чтобы экспортировать задание в локальный проект, выберите задание, которое необходимо экспортировать, в **Stream Analytics Explorer** в портал Azure. На странице **запрос** выберите **Открыть в Visual Studio**. Затем выберите **Visual Studio Code**.

![Открыть задание Stream Analytics в Visual Studio Code](./media/resource-manager-export/open-job-vs-code.png)

Дополнительные сведения об использовании Visual Studio Code для управления заданиями Stream Analytics см. в [руководстве по Visual Studio Code](quick-create-vs-code.md).

## <a name="compile-the-script"></a>Компиляция скрипта 

Следующим шагом является компиляция скрипта задания в шаблон Azure Resource Manager. Перед компиляцией скрипта убедитесь, что у задания есть хотя бы один вход и один выход. Если входные или выходные данные не настроены, необходимо сначала настроить входные и выходные данные.

1. В Visual Studio Code перейдите к файлу *преобразования. asaql* задания.

   ![Файл преобразования. asaql в Visual Studio Code](./media/resource-manager-export/transformation-asaql.png)

1. Щелкните правой кнопкой мыши файл *преобразование. asaql* и в меню выберите **ASA: компилировать скрипт** .

1. Обратите внимание, что в рабочей области задания Stream Analytics отображается папка **deploy (развертывание** ).

1. Изучите файл *JobTemplate. JSON* , который является шаблоном управления ресурсами Azure, который используется для развертывания.

## <a name="complete-the-parameters-file"></a>Завершение работы с файлом параметров

Затем заполните файл параметров шаблона управления ресурсами Azure.

1. Откройте файл *JobTemplate. parameters. JSON* , расположенный в папке **deploy** рабочей области задания Stream Analytics, в Visual Studio Code.

1. Обратите внимание, что входные и выходные ключи имеют значение null. Замените значения NULL фактическими ключами доступа для входных и выходных ресурсов.

1. Сохраните файл параметров.

## <a name="deploy-using-templates"></a>Развертывание с помощью шаблонов

Вы можете развернуть задание Azure Stream Analytics с помощью шаблонов Azure Resource Manager, созданных в предыдущем разделе.

В окне PowerShell выполните следующую команду. Не забудьте реаплце *ResourceGroupName*, *TemplateFile*и *TemplateParameterFile* с фактическим именем группы ресурсов, а также полный путь к файлам *JobTemplate. JSON* и *JobTemplate. parameters. JSON* в **папке Deploy (развертывание** ) рабочей области задания.

Если вы не настроили Azure PowerShell, выполните действия, описанные в разделе [установка Azure PowerShell модуля](https://docs.microsoft.com/powershell/azure/install-Az-ps).

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName "<your resource group>" -TemplateFile "<path to JobTemplate.json>" -TemplateParameterFile "<path to JobTemplate.parameters.json>"
```

## <a name="next-steps"></a>Дальнейшие шаги

* [Тестирование Azure Stream Analytics заданий локально с входными данными в режиме реального времени с помощью Visual Studio Code](visual-studio-code-local-run-live-input.md)

* [Просмотр Azure Stream Analytics заданий с помощью Visual Studio Code (Предварительная версия)](visual-studio-code-explore-jobs.md)