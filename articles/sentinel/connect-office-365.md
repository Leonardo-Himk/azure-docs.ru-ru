---
title: Подключение данных Office 365 к Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить данные Office 365 к Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/12/2020
ms.author: yelevin
ms.openlocfilehash: 43eba727b1dc724aae6eea3ec77de1363c5db73f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78252511"
---
# <a name="connect-data-from-office-365-logs"></a>Подключение данных из журналов Office 365



Вы можете выполнить потоковую передачу журналов аудита из [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) в Azure Sentinel одним щелчком мыши. Вы можете выполнять потоковую передачу журналов аудита из Office 365 в рабочую область Sentinel Azure в том же клиенте. Соединитель журнала действий Office 365 предоставляет информацию о текущих действиях пользователей. Вы получите сведения о различных действиях пользователя, администратора, системы и политики, а также о событиях из Office 365. Подключив журналы Office 365 в Azure Sentinel, вы можете использовать эти данные для просмотра панелей мониторинга, создания пользовательских оповещений и улучшения процесса расследования.

> [!IMPORTANT]
> Если у вас есть лицензия E3, перед тем как получить доступ к данным через API-интерфейс действия управления Office 365, необходимо включить единое ведение журнала аудита для организации Office 365. Это можно сделать, включив журнал аудита Office 365. Инструкции см. [в разделе Включение или отключение поиска по журналам аудита Office 365](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Дополнительные сведения см. в [справочнике по API действий управления Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Предварительные условия

- Вы должны быть глобальным администратором или администратором безопасности вашего клиента.
- Для клиента должен быть включен единый аудит. Для клиентов с лицензиями Office 365 E3 или водопо умолчанию включен единый аудит. <br>Если у клиента нет одной из этих лицензий, необходимо включить единый аудит в клиенте с помощью одного из следующих методов:
    - [С помощью командлета Set-админаудитлогконфиг](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) и включите параметр "унифиедаудитлогинжестионенаблед").
    - [С помощью пользовательского интерфейса центра соответствия & безопасности](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin).

## <a name="connect-to-office-365"></a>Подключение к Office 365

1. В поле Sentinel Azure выберите **соединители данных** , а затем щелкните плитку **Office 365** .

2. Если вы еще не включили эту возможность, перейдите в колонку **соединители данных** и выберите соединитель **Office 365** . Здесь можно щелкнуть **страницу открыть соединитель** и в разделе Конфигурация с меткой **Конфигурация** выбрать все журналы действий Office 365, которые вы хотите подключить к Azure Sentinel. 
   > [!NOTE]
   > Если вы уже подключили несколько клиентов в ранее поддерживаемую версию соединителя Office 365 в Azure Sentinel, вы сможете просматривать и изменять журналы, собранные из каждого клиента. Вы не сможете добавить дополнительных клиентов, но можете удалить ранее добавленные клиенты.
3. Чтобы использовать соответствующую схему в Log Analytics журналов Office 365, выполните поиск по запросу **оффицеактивити**.


## <a name="next-steps"></a>Дальнейшие шаги
В этом документе вы узнали, как подключить Office 365 к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats-built-in.md).

