---
title: Географическое аварийное восстановление в облаке Azure | Документация Майкрософт
description: Узнайте, как защитить приложение с пружинным облаком от региональных простоев
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: brendm
ms.openlocfilehash: e8f32f574a4ff7be0cc3cc7915b8203b53824c63
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792332"
---
# <a name="azure-spring-cloud-disaster-recovery"></a>Аварийное восстановление облака Azure весны

В этой статье объясняются некоторые стратегии, которые можно использовать для защиты облачных приложений Azure весны в течение простоя.  Любой регион или центр обработки данных может столкнуться с простоями, вызванными региональными авариями, но тщательное планирование может снизить влияние на клиентов.

## <a name="plan-your-application-deployment"></a>Планирование развертывания приложения

Облачные приложения Azure "Весна" работают в определенном регионе.  Azure работает в различных странах по всему миру. Географическая территория Azure — это определенная область мира, содержащая по крайней мере один регион Azure. Регион Azure — это область внутри географического элемента, содержащая один или несколько центров обработки данных.  Каждый регион Azure образует пару с другим регионом в пределах той же географической территории. Эти два региона формируют пару регионов. Azure сериализует обновления платформы (плановое обслуживание) по парам региональных стандартов, гарантируя, что в каждый момент времени обновляется только один регион в каждой паре. Если сбой затрагивает несколько регионов, по меньшей мере один регион в каждой паре будет приоритетным для восстановления.

Обеспечение высокой доступности и защиты от аварий требует развертывания приложений с пружинным облаком в нескольких регионах.  Azure предоставляет список [парных регионов](../best-practices-availability-paired-regions.md) , чтобы вы могли планировать облачные развертывания на территориальных парах.  При проектировании архитектуры микрослужбы рекомендуется учитывать три ключевых фактора: доступность региона, парные регионы Azure и доступность службы.

*  Доступность региона. Выберите географическую область, которая будет близка к пользователям для снижения задержки сети и времени передачи.
*  Парные регионы Azure. Выберите парные регионы в выбранной географической области, чтобы обеспечить согласованное обновление платформы и приоритеты при необходимости восстановления.
*  Доступность службы. Определите, должны ли парные регионы запускаться "горячий", "горячий", "горячий", "горячий" или "теплый" или "холодный".

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Использование диспетчера трафика Azure для маршрутизации трафика

[Диспетчер трафика Azure](../traffic-manager/traffic-manager-overview.md) обеспечивает балансировку нагрузки трафика на основе DNS и может распределять сетевой трафик между несколькими регионами.  Используйте диспетчер трафика Azure, чтобы направить клиентам ближайший к нему экземпляр облачной службы Azure весны.  Для повышения производительности и избыточности направляйте весь трафик приложений через диспетчер трафика Azure перед отправкой в облачную службу Azure весны.

Если у вас есть приложения Azure "Весна" в нескольких регионах, используйте диспетчер трафика Azure для управления потоком трафика приложений в каждом регионе.  Определите конечную точку диспетчера трафика Azure для каждой службы с помощью IP-адреса службы. Клиенты должны подключаться к DNS-имени диспетчера трафика Azure, указывающего на облачную службу Azure весны.  Нагрузка диспетчера трафика Azure распределяет трафик между определенными конечными точками.  Если при аварии центр обработки данных возникнет сбой, диспетчер трафика Azure направляет трафик из этого региона в его пару, гарантируя непрерывность обслуживания.

## <a name="create-azure-traffic-manager-for-azure-spring-cloud"></a>Создание диспетчера трафика Azure для Azure Веснного облака

1. Создание Azure Веснного облака в двух разных регионах.
Вам понадобятся два экземпляра службы Azure Веснного облака, развернутые в двух разных регионах (Восточная часть США и Западная Европа). Запустите существующее облачное приложение Azure весны с помощью портал Azure, чтобы создать два экземпляра службы. Каждый из них будет служить основной и отработкой отказа конечной точки для трафика. 

**Сведения о двух экземплярах службы:**

| Имя службы | Расположение | Развертывание |
|--|--|--|
| Служба-пример — a | Восточная часть США | Шлюз/auth-служба/учетная запись-Служба |
| Service-Sample-b | Западная Европа | Шлюз/auth-служба/учетная запись-Служба |

2. Настройка личного домена для службы используйте [документ личный домен](spring-cloud-tutorial-custom-domain.md) , чтобы настроить личный домен для этих двух существующих экземпляров службы. После успешной настройки оба экземпляра службы будут привязаны к пользовательскому домену: bcdr-test.contoso.com

3. Создайте диспетчер трафика и две конечные точки: [Создайте профиль диспетчера трафика с помощью портал Azure](https://docs.microsoft.com/azure/traffic-manager/quickstart-create-traffic-manager-profile).

Ниже приведен профиль диспетчера трафика.
* DNS-имя диспетчера трафика:http://asc-bcdr.trafficmanager.net
* Профили конечных точек: 

| Профиль | Type | Назначение | Priority | Параметры настраиваемого заголовка |
|--|--|--|--|--|
| Профиль конечной точки | Внешняя конечная точка | service-sample-a.asc-test.net | 1 | узел: bcdr-test.contoso.com |
| Профиль конечной точки B | Внешняя конечная точка | service-sample-b.asc-test.net | 2 | узел: bcdr-test.contoso.com |

4. Создайте запись CNAME в зоне DNS: bcdr-test.contoso.com CNAME asc-bcdr.trafficmanager.net. 

5. Теперь среда полностью настроена. Клиенты должны иметь доступ к приложению через: bcdr-test.contoso.com.