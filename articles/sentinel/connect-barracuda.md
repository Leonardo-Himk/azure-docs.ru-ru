---
title: Подключение данных Barracuda к Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить данные Barracuda к Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 4e87bb57e6bdfea6307a166383da9dea187eea4f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588490"
---
# <a name="connect-your-barracuda-appliance"></a>Подключение устройства Barracuda 



Соединитель брандмауэра веб-приложения Barracuda (WAF) позволяет легко подключать журналы Barracuda с помощью Sentinel Azure, просматривать панели мониторинга, создавать пользовательские оповещения и улучшать исследование. Это позволяет получить более подробные сведения о сети организации и улучшить возможности обеспечения безопасности. Azure Sentinel использует преимущества собственной интеграции между **Barracuda** и агентом log Analytics для обеспечения беспрепятственной интеграции. 


> [!NOTE]
> Данные будут храниться в географическом расположении рабочей области, на которой вы используете метку Azure.

## <a name="configure-and-connect-barracuda-waf"></a>Настройка и подключение Barracuda WAF
Брандмауэр веб-приложения Barracuda может интегрировать и экспортировать журналы непосредственно в Azure Sentinel с помощью агента Log Analytics.
1. Перейдите в [Barracuda WAF Configuration Flow](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/)и следуйте инструкциям по настройке подключения, используя следующие параметры:
    - **Идентификатор рабочей области**: СКОПИРУЙТЕ значение идентификатора рабочей области со страницы соединителя Azure Sentinel Barracuda.
    - **Первичный ключ**. Скопируйте значение первичного ключа со страницы соединителя Azure Sentinel Barracuda.
1. Чтобы использовать соответствующую схему в Log Analytics для событий Barracuda, выполните поиск **CommonSecurityLog** и **barracuda_CL**.


## <a name="validate-connectivity"></a>Проверка подключения

Если журналы начнут появляться в Log Analytics, это может занять до 20 минут. 



## <a name="next-steps"></a>Дальнейшие шаги
В этом документе вы узнали, как подключить устройства Barracuda к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Используйте книги](tutorial-monitor-your-data.md) для отслеживания данных.


