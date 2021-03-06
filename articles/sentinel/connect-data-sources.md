---
title: Подключение источников данных к Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить источники данных к Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: angrobe
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: yelevin
ms.openlocfilehash: 4060cfe08e91c87467a8ef6801adab6f027473bf
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83696868"
---
# <a name="connect-data-sources"></a>Подключение источников данных

Чтобы подключить Azure Sentinel, необходимо сначала подключиться к источникам данных. Azure Sentinel поставляется с несколькими соединителями для решений Майкрософт, которые доступны по умолчанию и обеспечивают интеграцию в режиме реального времени, включая решения Microsoft Threat Protection, источники Microsoft 365, Office 365, Azure AD, Azure ATP, Microsoft Cloud App Security и многое другое. Кроме того, доступны встроенные соединители для более широкой экосистемы безопасности для решений сторонних разработчиков. Чтобы подключить источники данных к Azure Sentinel, вы также можете использовать общий формат событий, Syslog или REST API.  

1. В меню выберите **Data connectors** (Соединители данных). На этой странице приведен полный список соединителей Azure Sentinel и показано состояние каждого из них. Выберите нужный соединитель и щелкните **Open connector page** (Открыть страницу соединителя). 

   ![Сборщики данных](./media/collect-data/collect-data-page.png)

1. На странице соединителя проверьте, выполнены ли все предварительные требования, и следуйте инструкциям, чтобы подключить данные к Azure Sentinel. Синхронизация журналов с Azure Sentinel может начаться не сразу. После подключения вы увидите сводку данных в графе **Data received** (Полученные данные) и состояние подключения для типов данных.

   ![Подключение сборщиков](./media/collect-data/opened-connector-page.png)
  
1. Перейдите на вкладку **Next steps** (Дальнейшие действия), чтобы посмотреть список готового содержимого, предоставляемого Azure Sentinel для определенного типа данных.

   ![Сборщики данных](./media/collect-data/data-insights.png)
 

## <a name="data-connection-methods"></a>Методы подключения к данным

Azure Sentinel поддерживает следующие методы подключения к данным:

- **Интеграция между службами**.<br> Некоторые службы подключены в собственном коде, например службы AWS и Майкрософт. Они используют платформу Azure для готовой интеграции. Следующие решения можно подключить несколькими щелчками мыши:
    - [Amazon Web Services — CloudTrail](connect-aws.md);
    - [Действия Azure](connect-azure-activity.md)
    - [Данные для входа и журналы аудита Azure AD](connect-azure-active-directory.md)
    - [Защита идентификации Azure AD](connect-azure-ad-Identity-protection.md)
    - [Расширенная защита от угроз Azure](connect-azure-atp.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Центр безопасности Azure](connect-azure-security-center.md)
    - [Cloud App Security](connect-cloud-app-security.md)
    - [Сервер доменных имен](connect-dns.md)
    - [Office 365](connect-office-365.md)
    - [ATP в Microsoft Defender](connect-microsoft-defender-advanced-threat-protection.md)
    - [Брандмауэр веб-приложения (Майкрософт)](connect-microsoft-waf.md)
    - [Брандмауэр Windows](connect-windows-firewall.md)
    - [События безопасности Windows](connect-windows-security-events.md)

- **Внешние решения через API**. Некоторые источники данных подключены с помощью интерфейсов API, предоставляемых подключенным источником данных. Как правило, большинство технологий безопасности предоставляют набор API, с помощью которых можно получить журналы событий. API подключаются к Azure Sentinel и собирают определенные типы данных, а затем отправляют их в Azure Log Analytics. Устройства, подключенные с помощью API:
    - [Barracuda](connect-barracuda.md)
    - [Barracuda CloudGen Firewall](connect-barracuda-cloudgen-firewall.md)
    - [Citrix Analytics (модуль безопасности)](connect-citrix-analytics.md)
    - [F5 BIG-IP](connect-f5-big-ip.md)
    - [Forcepoint DLP](connect-forcepoint-dlp.md)
    - [Squadra Technologies secRMM](connect-squadra-secrmm.md)
    - [Symantec ICDX](connect-symantec.md)
    - [Zimperium](connect-zimperium-mtd.md)


- **Внешние решения через агента**. Azure Sentinel может подключаться ко всем источникам данных, которые могут выполнять потоковую передачу журнала в режиме реального времени с помощью протокола Syslog, через агента. <br>Большинство устройств использует протокол Syslog для отправки сообщений о событиях, которые включают сам журнал и данные о нем. Журналы имеют разный формат, но большинство устройств поддерживает стандарт общего формата событий (CEF) для данных в журнале. <br>Агент Azure Sentinel, основанный на агенте Log Analytics, преобразует журналы в формате CEF в формат, который принимается службой Log Analytics. В зависимости от типа устройства агент устанавливается или непосредственно на устройстве, или на выделенном сервере Linux. Агент для Linux получает события из управляющей программы Syslog по протоколу UDP. Если компьютер Linux должен собирать большое количество событий Syslog, отправка производится по протоколу TCP из управляющей программы Syslog в агент, а затем — в Log Analytics.
    - Брандмауэры, прокси-серверы и конечные точки.
        - [Check Point](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [ExtraHop Reveal(x)](connect-extrahop.md)
        - [F5](connect-f5.md)
        - [Продукты Forcepoint](connect-forcepoint-casb-ngfw.md)
        - [Fortinet](connect-fortinet.md)
        - [Palo Alto Networks](connect-paloalto.md)
        - [One Identity Safeguard](connect-one-identity.md)
        - [Другие устройства CEF](connect-common-event-format.md)
        - [Другие устройства Syslog](connect-syslog.md)
        - [Trend Micro Deep Security](connect-trend-micro.md)
        - [Zscaler](connect-zscaler.md)
    - Решения DLP
    - [Поставщики Threat Intelligence](connect-threat-intelligence.md)
    - [Компьютеры DNS](connect-dns.md) — агент установлен непосредственно на компьютере DNS
    - Серверы Linux
    - Другие облака
    
## <a name="agent-connection-options"></a>Параметры подключения агента<a name="agent-options"></a>

Чтобы подключить внешнее устройство к Azure Sentinel, агент должен быть развернут на выделенном компьютере (виртуальной машине или локальном компьютере) для поддержки связи между устройством и Azure Sentinel. Вы можете развернуть агент автоматически или вручную. Автоматическое развертывание доступно только в том случае, если выделенный компьютер является новой созданной в Azure виртуальной машиной. 


![CEF в Azure](./media/connect-cef/cef-syslog-azure.png)

Вместо этого вы можете развернуть агент вручную на существующей виртуальной машине Azure, на виртуальной машине в другом облаке или на локальном компьютере.

![CEF в локальной среде](./media/connect-cef/cef-syslog-onprem.png)

## <a name="map-data-types-with-azure-sentinel-connection-options"></a>Преобразование типов данных с помощью параметров подключения к Sentinel Azure


| **Data type** | **Инструкции по подключению** | **Соединитель данных?** | **Комментарии** |
|------|---------|-------------|------|
| AWSCloudTrail | [Подключение AWS](connect-aws.md) | V | |
| AzureActivity | [Подключение действий Azure](connect-azure-activity.md) и [обзора журналов действий](../azure-monitor/platform/platform-logs-overview.md)| V | |
| AuditLogs | [Подключение Azure AD](connect-azure-active-directory.md)  | V | |
| SigninLogs | [Подключение Azure AD](connect-azure-active-directory.md)  | V | |
| AzureFirewall |[Система диагностики Azure](../firewall/tutorial-diagnostics.md) | V | |
| InformationProtectionLogs_CL  | [Отчеты Azure Information Protection](https://docs.microsoft.com/azure/information-protection/reports-aip)<br>[Подключение Azure Information Protection](connect-azure-information-protection.md)  | V | Обычно в этом случае в дополнение к типу данных используется функция **InformationProtectionEvents**. Дополнительные сведения см. статью [Как изменять отчеты и создавать пользовательские запросы](https://docs.microsoft.com/azure/information-protection/reports-aip#how-to-modify-the-reports-and-create-custom-queries)|
| AzureNetworkAnalytics_CL  | [Схема аналитики трафика](../network-watcher/traffic-analytics.md) [Аналитика трафика](../network-watcher/traffic-analytics.md)  | | |
| CommonSecurityLog  | [Подключение CEF](connect-common-event-format.md)  | V | |
| OfficeActivity | [Подключение Office 365](connect-office-365.md) | V | |
| SecurityEvents | [Подключение событий безопасности Windows](connect-windows-security-events.md)  | V | Книги небезопасных протоколов см. в разделе [Настройка книг небезопасных протоколов](/azure/sentinel/quickstart-get-visibility#use-built-in-workbooks)  |
| Системный журнал | [Подключение системного журнала](connect-syslog.md) | V | |
| Брандмауэр веб-приложений Майкрософт (WAF) – (AzureDiagnostics) |[Подключение брандмауэра веб-приложений Майкрософт](connect-microsoft-waf.md) | V | |
| SymantecICDx_CL | [Подключение Symantec](connect-symantec.md) | V | |
| ThreatIntelligenceIndicator  | [Подключение аналитики угроз](connect-threat-intelligence.md)  | V | |
| VMConnection <br> ServiceMapComputer_CL<br> ServiceMapProcess_CL|  [Схема службы Azure Monitor](../azure-monitor/insights/service-map.md)<br>[Подключение аналитических сведений для виртуальных машин Azure Monitor](../azure-monitor/insights/vminsights-onboard.md) <br> [Включение аналитических сведений для виртуальных машин Azure Monitor](../azure-monitor/insights/vminsights-enable-overview.md) <br> [Подключение одиночной виртуальной машины](../azure-monitor/insights/vminsights-enable-single-vm.md)<br>  [Использование подключения через политику](../azure-monitor/insights/vminsights-enable-at-scale-policy.md)| X | Книга аналитики виртуальных машин  |
| DnsEvents | [Подключение DNS](connect-dns.md) | V | |
| W3CIISLog | [Подключение журналов IIS](../azure-monitor/platform/data-sources-iis-logs.md)  | X | |
| Данные передачи | [Подключение передаваемых данных](../azure-monitor/insights/wire-data.md) | X | |
| WindowsFirewall | [Подключение брандмауэра Windows](connect-windows-firewall.md) | V | |
| SecurityAlert службы "Защита идентификации Azure AD"  | [Подключения Защиты идентификации Azure AD](connect-azure-ad-identity-protection.md)  | V | |
| AATP SecurityAlert  | [Подключение Azure ATP](connect-azure-atp.md) | V | |
| SecurityAlert сертификатов службы приложений  | [Подключение к центру безопасности Azure](connect-azure-security-center.md)  | V | |
| MCAS SecurityAlert  | [Подключение Microsoft Cloud App Security](connect-cloud-app-security.md)  | V | |
| SecurityAlert | | | |
| Sysmon (событие) | [Подключение Sysmon](https://azure.microsoft.com/blog/detecting-in-memory-attacks-with-sysmon-and-azure-security-center)<br> [Подключение событий Windows](../azure-monitor/platform/data-sources-windows-events.md) <br> [Получение средства синтаксического анализа Sysmon](https://github.com/Azure/Azure-Sentinel/blob/master/Parsers/Sysmon/Sysmon-v10.42-Parser.txt)| X | По умолчанию на виртуальных машинах не установлена коллекция Sysmon. Дополнительные сведения об установке агента Sysmon см. в разделе [Sysmon](https://docs.microsoft.com/sysinternals/downloads/sysmon). |
| ConfigurationData  | [Автоматизация учета виртуальной машины](../automation/automation-vm-inventory.md)| X | |
| ConfigurationChange  | [Автоматизация отслеживания виртуальной машины](../automation/change-tracking.md) | X | |
| F5 BIG-IP | [Подключение F5 BIG-IP](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel)  | X | |
| McasShadowItReporting  |  | X | |
| Barracuda_CL | [Подключение Barracuda](connect-barracuda.md) | V | |


## <a name="next-steps"></a>Дальнейшие действия

- Чтобы начать работу с Azure Sentinel, вам нужна подписка на Microsoft Azure. Если у вас нет подписки, вы можете зарегистрироваться для получения [бесплатной пробной версии](https://azure.microsoft.com/free/).
- Узнайте, как [подключить данные к Azure Sentinel](quickstart-onboard.md), и [отслеживайте свои данные и потенциальные угрозы](quickstart-get-visibility.md).
