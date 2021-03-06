---
title: Брандмауэр веб-приложения Azure на шлюзе приложений — часто задаваемые вопросы
description: В этой статье содержатся ответы на часто задаваемые вопросы о брандмауэре веб-приложения в шлюзе приложений.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: article
ms.date: 05/05/2020
ms.author: victorh
ms.openlocfilehash: 57081cd948bcee1261415eae31f91b3c61f08c9f
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82842855"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall-on-application-gateway"></a>Часто задаваемые вопросы о брандмауэре веб-приложения Azure в шлюзе приложений

В этой статье содержатся ответы на часто задаваемые вопросы о брандмауэре веб-приложения Azure (WAF) на функциях и функциях шлюза приложений. 

## <a name="what-is-azure-waf"></a>Что такое Azure WAF?

Azure WAF — это брандмауэр веб-приложения, который помогает защитить веб-приложения от распространенных угроз, таких как внедрение кода SQL, межсайтовые сценарии и другие веб-атаки. Вы можете определить политику WAF, состоящую из сочетания настраиваемых и управляемых правил для управления доступом к веб-приложениям.

Политику Azure WAF можно применять к веб-приложениям, размещенным на шлюзе приложений или в передней дверце Azure.

## <a name="what-features-does-the-waf-sku-support"></a>Какие функции поддерживает SKU WAF?

SKU WAF поддерживает все функции, доступные в стандартном SKU.

## <a name="how-do-i-monitor-waf"></a>Как выполнять мониторинг WAF?

Мониторинг WAF с помощью ведения журнала диагностики. Дополнительные сведения см. в разделе [ведение журнала диагностики и метрики для шлюза приложений](../../application-gateway/application-gateway-diagnostics.md).

## <a name="does-detection-mode-block-traffic"></a>Блокируется ли трафик в режиме обнаружения?

Нет. Режим обнаружения только регистрирует трафик, который запускает правило WAF.

## <a name="can-i-customize-waf-rules"></a>Могу ли я настроить правила WAF?

Да. Дополнительные сведения см. в разделе [Настройка групп правил и правил WAF](application-gateway-customize-waf-rules-portal.md).

## <a name="what-rules-are-currently-available-for-waf"></a>Какие правила в настоящее время доступны для WAF?

В настоящее время WAF поддерживает запросы CR [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229), [3,0](application-gateway-crs-rulegroups-rules.md#owasp30)и [3,1](application-gateway-crs-rulegroups-rules.md#owasp31). Эти правила обеспечивают безопасность на основе большинства первых 10 уязвимостей, в которых открытый проект безопасности веб-приложений (OWASP) определяет: 

* Защита от внедрения кода SQL.
* Защита от межсайтовых сценариев
* Защита от распространенных веб-атак, таких как внедрение команд, HTTP-запрос несанкционированных, разделение HTTP-ответов и атака с удаленным включением файлов.
* Защита от нарушений протокола HTTP.
* Защита от аномалий протокола HTTP, например отсутствия агента пользователя узла и заголовков accept.
* Защита от программ-роботов, программ-обходчиков и сканеров.
* Обнаружение распространенных недопустимых конфигураций приложений (то есть Apache, IIS и т. д.)

Дополнительные сведения см. в статье [OWASP Top-10 уязвимостей](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013).

## <a name="does-waf-support-ddos-protection"></a>Поддерживает ли WAF защиту от атак DDoS?

Да. Защиту от атак DDoS можно включить в виртуальной сети, в которой развернут шлюз приложений. Этот параметр гарантирует, что служба защиты Azure от атак DDoS также защищает виртуальный IP-адрес шлюза приложений (VIP).


## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [брандмауэре веб-приложения Azure](../overview.md).
- Дополнительные сведения о [передней дверце Azure](../../frontdoor/front-door-overview.md).
