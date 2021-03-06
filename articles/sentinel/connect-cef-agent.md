---
title: Развертывание сервера пересылки журналов для подключения данных CEF к Azure Sentinel | Документация Майкрософт
description: Узнайте, как развернуть агент для подключения данных CEF к Azure Sentinel.
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
ms.date: 04/19/2020
ms.author: yelevin
ms.openlocfilehash: 5a8b97e5bef57b29f388c86628f0af5d05e1724a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81731659"
---
# <a name="step-1-deploy-the-log-forwarder"></a>Шаг 1. Развертывание сервера пересылки журналов


На этом шаге вы назначите и настроите компьютер Linux, который будет пересылать журналы из решения безопасности в рабочую область Azure Sentinel. Это может быть физический компьютер или виртуальная машина в локальной среде, ВИРТУАЛЬная машина Azure или ВИРТУАЛЬная машина в другом облаке. Используя указанную ссылку, вы запустите сценарий на указанном компьютере, который выполняет следующие задачи:
- Устанавливает агент Log Analytics для Linux (также известный как агент OMS) и настраивает его для следующих целей:
    - Прослушивание сообщений CEF из встроенной управляющей программы Linux syslog на TCP-порте 25226
    - Безопасная отправка сообщений по протоколу TLS в рабочую область "Sentinel" Azure, где они анализируются и дополнены

- Настраивает встроенную управляющую программу Linux syslog (rsyslog. d/syslog-ng) в следующих целях:
    - Прослушивание сообщений системного журнала в решениях безопасности через TCP-порт 514
    - Пересылка только тех сообщений, которые он идентифицирует как CEF агенту Log Analytics на localhost с помощью TCP-порта 25226
 
## <a name="prerequisites"></a>Предварительные условия

- Необходимо иметь повышенные разрешения (sudo) на назначенном компьютере Linux.
- На компьютере Linux должна быть установлена Python.<br>Используйте `python -version` команду для проверки.
- Перед установкой агента Log Analytics компьютер Linux не должен быть подключен к рабочим областям Azure.

## <a name="run-the-deployment-script"></a>Выполнение скрипта развертывания
 
1. В меню навигации меток Azure щелкните **соединители данных**. В списке соединителей щелкните плитку **общий формат событий (CEF)** , а затем нажмите кнопку **Открыть соединительную страницу** в правом нижнем углу. 

1. В разделе **1,2 Установка СБОРЩИКА CEF на компьютере Linux**скопируйте ссылку, указанную в разделе **выполните следующий сценарий, чтобы установить и применить сборщик CEF**, или из следующего текста:

     `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`

1. Во время выполнения скрипта убедитесь, что не получены сообщения об ошибках и предупреждения.

Перейдите к [шагу 2. Настройка решения безопасности для пересылки сообщений CEF](connect-cef-solution-config.md) .

## <a name="deployment-script-explained"></a>Описание скрипта развертывания

Ниже приведено описание действий сценария развертывания с помощью команды.

Выберите управляющую программу системного журнала, чтобы просмотреть соответствующее описание.

# <a name="rsyslog-daemon"></a>[Управляющая программа rsyslog](#tab/rsyslog)

1. **Загрузка и установка агента Log Analytics:**

    - Скачивает скрипт установки для агента Log Analytics (OMS) Linux<br>
        `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh`

    - Устанавливает агент Log Analytics<br>
        `sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com`

1. **Настройка управляющей программы syslog:**

    1. Открывает порт 514 для связи TCP с помощью файла `/etc/rsyslog.conf`конфигурации системного журнала.

    1. Настраивает управляющую программу на пересылку сообщений CEF агенту Log Analytics через TCP-порт 25226, вставляя специальный файл `security-config-omsagent.conf` конфигурации в каталог `/etc/rsyslog.d/`управляющей программы системного журнала.

        Содержимое `security-config-omsagent.conf` файла:

            :rawmsg, regex, "CEF\|ASA" ~
            *.* @@127.0.0.1:25226

1. **Перезапуск управляющей программы системного журнала**

    `service rsyslog restart`

1. **Настройка конфигурации агента Log Analytics для прослушивания порта 25226 и пересылка сообщений CEF в Azure Sentinel**

    1. Скачивает конфигурацию из репозитория GitHub агента Log Analytics<br>
        `wget -o /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/omsagent.d/security_events.conf`


    1. Перезапускает агент Log Analytics<br>
        `/opt/microsoft/omsagent/bin/service_control restart [workspaceID]`

# <a name="syslog-ng-daemon"></a>[Демон syslog-ng](#tab/syslogng)

1. **Загрузка и установка агента Log Analytics:**

    - Скачивает скрипт установки для агента Log Analytics (OMS) Linux<br>`wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh`

    - Устанавливает агент Log Analytics<br>`sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com`

1. **Настройка управляющей программы syslog:**

    1. Открывает порт 514 для связи TCP с помощью файла `/etc/syslog-ng/syslog-ng.conf`конфигурации системного журнала.

    1. Настраивает управляющую программу на пересылку сообщений CEF агенту Log Analytics через TCP-порт 25226, вставляя специальный файл `security-config-omsagent.conf` конфигурации в каталог `/etc/syslog-ng/conf.d/`управляющей программы системного журнала.

        Содержимое `security-config-omsagent.conf` файла:

            filter f_oms_filter {match(\"CEF\|ASA\" ) ;};
            destination oms_destination {tcp(\"127.0.0.1\" port("25226"));};
            log {source(s_src);filter(f_oms_filter);destination(oms_destination);};

1. **Перезапуск управляющей программы системного журнала**

    `service syslog-ng restart`

1. **Настройка конфигурации агента Log Analytics для прослушивания порта 25226 и пересылка сообщений CEF в Azure Sentinel**

    1. Скачивает конфигурацию из репозитория GitHub агента Log Analytics<br>
        `wget -o /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/omsagent.d/security_events.conf`


    1. Перезапускает агент Log Analytics<br>
        `/opt/microsoft/omsagent/bin/service_control restart [workspaceID]`

---

## <a name="next-steps"></a>Дальнейшие шаги
В этом документе вы узнали, как развернуть агент Log Analytics, чтобы подключить устройства CEF к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats.md).

