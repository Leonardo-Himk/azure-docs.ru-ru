---
title: Подключение данных брандмауэра Windows к Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить данные брандмауэра Windows к Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 5d2f68261143c3fc5bbcda0b739af17251eeee63
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588065"
---
# <a name="connect-windows-firewall"></a>Подключение брандмауэра Windows



Соединитель брандмауэра Windows позволяет легко подключать журналы брандмауэра Windows, если они подключены к рабочей области Sentinel Azure. Это подключение позволяет просматривать панели мониторинга, создавать пользовательские предупреждения и улучшать исследование. Это позволяет получить более подробные сведения о сети организации и улучшить возможности обеспечения безопасности. Решение собирает события брандмауэра Windows с компьютеров Windows, на которых установлен агент Log Analytics. 


> [!NOTE]
> - Данные будут храниться в географическом расположении рабочей области, на которой вы используете метку Azure.
> - Если Azure Sentinel и центр безопасности Azure собираются в одну и ту же рабочую область, нет необходимости включать решение брандмауэра Windows через этот соединитель. Если вы включили его, это не приведет к дублированию данных. 

## <a name="enable-the-connector"></a>Включение соединителя 

1. На портале Sentinel Azure выберите **соединители данных** , а затем щелкните плитку **Брандмауэр Windows** . 
1.  Если ваши компьютеры Windows находятся в Azure:
    1. Щелкните **установить агент на виртуальной машине Windows в Azure**.
    1. В списке **виртуальные машины** выберите компьютер Windows, который требуется передать в качестве потока в Azure Sentinel. Убедитесь, что это виртуальная машина Windows.
    1. В открывшемся окне для этой виртуальной машины нажмите кнопку **подключить**.  
    1. В окне **соединителя брандмауэра Windows** нажмите кнопку **включить** . 

2. Если компьютер Windows не является виртуальной машиной Azure, выполните следующие действия.
    1. Щелкните **установить агент на компьютерах, не**относящихся к Azure.
    1. В окне **Direct Agent (прямое агент** ) выберите **скачать агент Windows (64 bit)** или **скачать агент Windows (32 бит)**.
    1. Установите агент на компьютере Windows. Скопируйте **идентификатор рабочей области**, **первичный ключ**и **вторичный ключ** и используйте их при появлении запроса во время установки.

4. Выберите типы данных, которые необходимо передать в поток.
5. Нажмите кнопку **установить решение**.
6. Чтобы использовать соответствующую схему в Log Analytics брандмауэра Windows, выполните поиск по запросу **SecurityEvent**.

## <a name="validate-connectivity"></a>Проверка подключения

Если журналы начнут появляться в Log Analytics, это может занять до 20 минут. 



## <a name="next-steps"></a>Дальнейшие шаги
В этом документе вы узнали, как подключить брандмауэр Windows к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats-built-in.md).

