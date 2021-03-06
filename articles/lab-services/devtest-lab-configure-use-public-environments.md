---
title: Настройка и использование общедоступных сред в Azure DevTest Labs | Документация Майкрософт
description: В этой статье описывается, как настроить и использовать общедоступные среды (шаблоны Azure Resource Manager в репозитории Git) в Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 127a6986e04cf90f69b2a8ec70b90b877e534708
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76721699"
---
# <a name="configure-and-use-public-environments-in-azure-devtest-labs"></a>Настройка и использование общедоступных сред в Azure DevTest Labs
Azure DevTest Labs имеет [общедоступный репозиторий шаблонов Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/Environments), который можно использовать, чтобы создавать среды без необходимости самостоятельно подключаться к внешнему источнику GitHub. В этом репозитории содержатся часто используемые шаблоны, такие как веб-приложения Azure, кластер Service Fabric и среда фермы SharePoint для разработки. Этот компонент похож на общедоступный репозиторий артефактов, включенный для каждой лаборатории, которую вы создаете. Репозиторий среды позволяет быстро приступить к работе с помощью предварительно созданных шаблонов с минимальным количеством входных параметров, чтобы вы могли быстро приступить к работе с ресурсами PaaS в лаборатории. 

## <a name="configuring-public-environments"></a>Настройка общедоступных сред
Владелец лаборатории может включить репозиторий общедоступной среды для лаборатории во время ее создания. Чтобы включить общедоступные среды для лаборатории во время ее создания, щелкните **Вкл.** для поля **Общедоступные среды**. 

![Включение общедоступной среды для новой лаборатории](media/devtest-lab-configure-use-public-environments/enable-public-environment-new-lab.png)


Для имеющихся лабораторий репозиторий общедоступной среды не включен. Включите его вручную, чтобы использовать содержащиеся в нем шаблоны. Для лабораторий, созданных с помощью шаблонов Resource Manager, репозиторий также отключен по умолчанию.

Вы можете включить и отключить общедоступные среды для лаборатории, а также предоставить пользователям доступ только к определенным средам, выполнив следующие действия: 

1. Выберите **Configuration and policies** (Конфигурация и политики) для своей лаборатории. 
2. В разделе **Базы виртуальных машин** выберите **Общедоступные среды**.
3. Чтобы включить общедоступные среды для лаборатории, нажмите кнопку **Да**. В противном случае нажмите кнопку **Нет**. 
4. Если вы включили общедоступные среды, то все среды в репозитории включены по умолчанию. Можно отменить выбор среды, чтобы сделать ее недоступной пользователям лаборатории. 

![Страница общедоступных сред](media/devtest-lab-configure-use-public-environments/public-environments-page.png)

## <a name="use-environment-templates-as-a-lab-user"></a>Использование шаблонов среды в качестве пользователя лаборатории
Пользователь лаборатории может создать новую среду с помощью одного из доступных шаблонов в списке, просто щелкнув **+ Добавить** на панели инструментов на странице лаборатории. В верхней части списка баз содержатся шаблоны общедоступных сред, включенные администратором лаборатории.

![Шаблоны общедоступной среды](media/devtest-lab-configure-use-public-environments/public-environment-templates.png)

## <a name="next-steps"></a>Дальнейшие шаги
Это репозиторий с открытым исходным кодом, в который вы можете добавлять собственные часто используемые и полезные шаблоны Resource Manager. Чтобы добавить шаблон, просто отправьте запрос на включение внесенных изменений в репозиторий.  
