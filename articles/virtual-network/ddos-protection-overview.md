---
title: Обзор стандарта защиты Azure от атак DDoS
description: Сведения о службе защиты от атак DDoS Azure.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/22/2020
ms.author: kumud
ms.openlocfilehash: fc47e1f4fbdb48e6e0abc1f2a7e32127b0325f47
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82130973"
---
# <a name="azure-ddos-protection-standard-overview"></a>Общие сведения о защите от атак DDoS Azure уровня "Стандартный"

Распределенные атаки типа "отказ в обслуживании" (DDoS) — это одна из наиболее крупных угроз доступности и безопасности, с которой сталкиваются клиенты, перемещающие свои приложения в облако. Целью DDoS-атаки является исчерпание ресурсов приложения, чтобы оно стало недоступным для обычных пользователей. Атаку DDoS можно направить на любую конечную точку, публично доступную через Интернет.

Служба "Защита от атак DDoS Azure" обеспечит защиту от атак DDoS, если соблюдаются рекомендации по проектированию приложений. Служба "Защита от атак DDoS Azure" предоставляет следующие уровни служб.

- **Базовый**: функция автоматически включена на платформе Azure. Постоянно отслеживая трафик и устраняя распространенные атаки на уровне сети в реальном времени, обеспечивают те же механизмы защиты, которые использует веб-службы Майкрософт.Весь масштаб глобальной сети Azure можно использовать для распространения и смягчения трафика атак в разных регионах.Защита обеспечивается для [общедоступных IP-адресов](virtual-network-public-ip-address.md) Azure IPv4 и IPv6.
- **Стандартный**: предоставляет дополнительные возможности по устранению рисков по сравнению с уровнем служб "Базовый", которые разработаны для ресурсов виртуальной сети Azure. Службу "Защита от атак DDoS" уровня "Стандартный" легко включить, и для ее использования не требуется изменять приложение. Политики защиты корректируются на основе мониторинга выделенного трафика и алгоритмов машинного обучения. Они применяются к общедоступным IP-адресам, связанным с ресурсами, развернутыми в виртуальных сетях, такими как Azure Load Balancer, Шлюз приложений Azure и экземпляры Azure Service Fabric, но эта защита не применяется к Средам службы приложений.Данные телеметрии доступны в представлениях Azure Monitor в режиме реального времени во время атаки и после ее окончания. Широкие возможности для анализа устранения рисков атак доступны с помощью параметров диагностики. Можно также включить защиту на прикладном уровне с помощью [брандмауэра веб-приложения в шлюзе приложений Azure](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или установки брандмауэра стороннего производителя из Azure Marketplace. Защита обеспечивается для [общедоступных IP-адресов](virtual-network-public-ip-address.md) Azure IPv4 и IPv6.

|Компонент                                         |Защита от атак DDoS уровня "Базовый"                 |Защита от атак DDoS уровня "Стандартный"                      |
|------------------------------------------------|--------------------------------------|----------------------------------------------|
|Активный мониторинг трафика & обнаружения Always on |Да                                   |Да                                           |
|Автоматическая защита от атак                    |Да                                   |Да                                           |
|Гарантия доступности                          |Регион Azure                          |Приложение                                   |
|Политики устранения рисков                             |Настроено для тома области трафика Azure |Настроено для тома трафика приложений          |
|Метрики & оповещения                                |Нет                                    |Метрики атак в реальном времени & журналы ресурсов с помощью Azure Monitor                                 |
|Отчеты об устранении рисков                              |Нет                                    |Отправка отчетов о предотвращении атаки                |
|Журналы потоков для устранения рисков                            |Нет                                    |Поток журнала ПРЕВЕНТИВНОЙ для интеграции SIEM           |
|Настройка политики устранения рисков                 |Нет                                    |Привлекать экспертов от атак DDoS                           |
|Поддержка                                         |Рекомендуется                           |Доступ к экспертам от атак DDoS во время активной атаки|
|Соглашение об уровне обслуживания                                             |Регион Azure                          |Гарантия защиты приложений & стоимость       |
|Цены                                         |Free                                  |Месячный & на основе использования                         |

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Типы атак DDoS, которые можно устранить с помощью защиты от атак DDoS уровня "Стандартный"

С помощью службы "Защита от атак DDoS" уровня "Стандартный" можно устранить следующие типы атак.

- **Объемные атаки** нацелены на то, чтобы заполнить сетевой уровень значительным объемом сетевого трафика, который очень похож на обычный. Такие атаки включают UDP-флуд, заполнение с усилением и другие типы заполнения с использованием поддельных пакетов. От атак DDoS Protection Standard позволяет уменьшить эти потенциальные атаки с несколькими гигабайтами, изменив и выполнив их очистку с помощью глобальной сети Azure, автоматически.
- **Атаки протокола** пытаются ухудшить доступность целевого объекта, используя уязвимости в стеке протоколов уровней 3 и 4. Такие атаки включают SYN-флуд, атаки отражения и другие атаки протокола. Служба "Защита от атак DDoS" уровня "Стандартный" позволяет устранить эти атаки, разделяя допустимый и вредоносный трафик путем взаимодействия с клиентом и блокировки вредоносного трафика. 
- **Атаки уровня ресурса (приложения)** направлены на пакеты веб-приложения и стремятся нарушить передачу данных между узлами. Такие атаки включают в себя нарушение протокола HTTP, атаки путем внедрения кода SQL, межсайтовые сценарии и другие атаки уровня 7. Используйте брандмауэр веб-приложения, например [брандмауэр веб-приложения шлюза приложений](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)Azure, а также стандарт защиты от атак DDoS, чтобы обеспечить защиту от этих атак. На сайте [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall) доступны брандмауэры веб-приложения от сторонних производителей.

Защита от атак DDoS уровня "Стандартный" распространяется на ресурсы в виртуальной сети, включая общедоступные IP-адреса, связанные с виртуальными машинами, подсистемами балансировки нагрузки и шлюзами приложений. В сочетании с брандмауэром веб-приложения шлюза приложений или брандмауэром веб-приложения стороннего производителя, развернутым в виртуальной сети с общедоступным IP-адресом, стандарт защиты от атак DDoS может предоставить полный уровень 3 для возможности устранения рисков уровня 7.

## <a name="ddos-protection-standard-features"></a>Возможности защиты от атак DDoS уровня "Стандартный"

![Возможности защиты от атак DDoS](./media/ddos-protection-overview/ddosfeatures.png)

Возможности защиты от атак DDoS ценовой категории "Стандартный" включают в себя:

- **Интеграция с собственной платформой:** изначальная интеграция в Azure. Включает в себя настройку на портале Azure. Защита от атак DDoS ценовой категории "Стандартный" распознает ваши ресурсы и их конфигурацию.
- **Моментальная активация защиты:** сразу же после активации защиты от атак DDoS ценовой категории "Стандартный" применяется упрощенная конфигурация, которая позволяет мгновенно защитить все ресурсы виртуальной сети. От пользователя не требуется никаких действий или настроек. Механизм защиты от атак DDoS ценовой категории "Стандартный" автоматически и мгновенно отразит атаку сразу же после ее обнаружения.
- **Постоянный мониторинг трафика:** Трафик вашего приложения круглосуточно и ежедневно проверяется на наличие признаков DDoS-атак. В случае превышения показателей в политиках защиты выполняется устранение рисков.
- **Адаптивная Настройка:** Интеллектуальное профилирование трафика позволяет получить сведения о трафике приложения в течение определенного времени, а также выбрать и обновить профиль, который наиболее подходит для вашей службы. Этот профиль корректируется с изменением трафика с течением времени.
- **Многоуровневая защита**: при использовании брандмауэром веб-приложения предоставляется полная комплексная защита от атак DDoS.
- **Широкий диапазон устраняемых атак.** Защита от более чем 60 различных типов известных DDoS-атак, для борьбы с крупнейшими из которых используются все возможности глобальной сети Azure.
- **Аналитика атак**: во время атаки вам будут отправляться подробные отчеты с пятиминутными интервалами, а по ее завершении вы получите полную сводку. Выполняйте потоковую передачу журналов потоков устранения рисков в автономную систему управления информационной безопасностью и событиями безопасности (SIEM), чтобы обеспечить во время атаки мониторинг почти в реальном времени.
- **Метрики атак.** В Azure Monitor доступны обобщенные метрики для каждой атаки.
- **Предупреждение об атаках:** Оповещения могут быть настроены в начале и при зависании атаки, а также во время атаки с помощью встроенных метрик атак. Оповещения интегрируются в операционное программное обеспечение, например Microsoft Azure журналы мониторинга, Splunk, службу хранилища Azure, электронную почту и портал Azure.
- **Гарантия стоимости:** Кредиты по переносу данных и масштабированию приложений для документированных атак от атак DDoS.

## <a name="ddos-protection-standard-mitigation"></a>Устранение атак с помощью защиты от атак DDoS уровня "Стандартный"

Служба "Защита от атак DDoS" отслеживает фактическое использование трафика и постоянно сравнивает его с пороговыми значениями, определенными в политике DDoS. При превышении порогового значения трафика атака DDoS автоматически устраняется. Когда трафик опускается ниже порогового значения, устранение DDoS-атаки отключается.

![Решение](./media/ddos-protection-overview/mitigation.png)

В процессе устранения атаки служба "Защита от атак DDoS" перенаправляет весь трафик, нацеленный на защищаемый ресурс, и выполняет несколько проверок, включая следующие.

- Проверка соответствия пакетов спецификациям Интернета и проверка правильности пакетов.
- Взаимодействие с клиентом для определения того, является ли трафик поддельным пакетом (например, с помощью SYN Auth или SYN Cookie или путем отбрасывания пакета, чтобы источник направил его повторно).
- Ограничение частоты пакетов, если другой метод применить не удается.

Служба "Защита от атак DDoS" блокирует трафик атаки и направляет оставшийся трафик в место назначения. В течение нескольких минут после обнаружения атаки вы получите оповещение с помощью метрик Azure Monitor. Настроив ведение журналов телеметрии для службы защиты от атак DDoS уровня "Стандартный", вы можете сохранить журналы для последующего анализа. В Azure Monitor хранятся данные метрик для службы "Защита от атак DDoS" уровня "Стандартный" за 30 дней.

Корпорация Майкрософт совместно с [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) разработала интерфейс, в котором клиенты Azure могут создавать трафик для общедоступных IP-адресов, защищенных от атак DDoS, в целях имитации. Имитация BreakPoint Cloud позволит вам:

- Проверить, как служба "Защита от атак DDoS Microsoft Azure" уровня "Стандартный" защищает ваши ресурсы Azure.
- Оптимизировать процесс реагирования на инциденты во время атаки DDoS.
- Обеспечить соответствие документации по защите от атак DDoS.
- Обучить команды безопасности сети.

## <a name="next-steps"></a>Дальнейшие шаги

- [Настройка службы "Защита от атак DDoS" уровня "Стандартный"](manage-ddos-protection.md)
