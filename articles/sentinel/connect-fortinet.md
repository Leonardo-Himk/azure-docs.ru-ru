---
title: Подключение данных Fortinet к Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить данные Fortinet к Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: aabc95fcb8d3b32b89bb83f4f6892c3d40b03417
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588201"
---
# <a name="connect-fortinet-to-azure-sentinel"></a>Подключение Fortinet к Azure Sentinel



В этой статье объясняется, как подключить устройство Fortinet к Azure Sentinel. Соединитель данных Fortinet позволяет легко подключать журналы Fortinet с помощью Azure Sentinel, просматривать панели мониторинга, создавать пользовательские оповещения и улучшать исследование. Использование Fortinet в Azure Sentinel предоставит вам более подробные сведения об использовании Интернета в вашей организации и улучшит возможности ее работы. 


 
## <a name="forward-fortinet-logs-to-the-syslog-agent"></a>Пересылка журналов Fortinet в агент системного журнала

Настройте Fortinet для пересылки сообщений системного журнала в формате CEF в рабочую область Azure с помощью агента системного журнала.

1. Откройте интерфейс командной строки на устройстве Fortinet и выполните следующие команды:

        config log syslogd setting
        set format cef
        set port 514
        set server <ip_address_of_Receiver>
        set status enable
        end

    - Замените **IP-адрес** сервера IP-адресом агента.
    - Задайте для **порта системного журнала** значение **514** или порт, установленный на агенте.
    - Чтобы включить формат CEF в ранних версиях Фортиос, может потребоваться выполнить команду Set **CSV Disabled**.
 
   > [!NOTE] 
   > Для получения дополнительных сведений перейдите в [библиотеку документов Fortinet](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary). Выберите свою **версию и воспользуйтесь** **справочной ссылкой и сообщением журнала**.

1. Чтобы использовать соответствующую схему в Azure Monitor Log Analytics для событий Fortinet, выполните поиск по `CommonSecurityLog`запросу.

1. Перейдите к [шагу 3. Проверка подключения](connect-cef-verify.md).


## <a name="next-steps"></a>Дальнейшие шаги
Из этой статьи вы узнали, как подключить устройства Fortinet к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Используйте книги](tutorial-monitor-your-data.md) для отслеживания данных.


