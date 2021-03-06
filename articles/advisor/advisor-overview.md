---
title: Общие сведения об Azure Advisor
description: Использование Azure Advisor для оптимизации развернутых служб Azure.
ms.topic: article
ms.date: 02/01/2019
ms.openlocfilehash: 74048073677cdf0f9f57d84469959a84e78cd6c7
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82854434"
---
# <a name="introduction-to-azure-advisor"></a>Общие сведения об Azure Advisor

Узнайте об основных возможностях Помощника по Azure, а также ознакомьтесь с ответами на часто задаваемые вопросы.

## <a name="what-is-advisor"></a>Назначение Помощника
Помощник по Azure — это персонализированный облачный консультант, который поможет следовать рекомендациям по оптимизации развернутых служб Azure. Он анализирует конфигурацию ресурсов и данные телеметрии их использования и рекомендует решения, которые помогут повысить эффективность затрат, производительность, уровень доступности и безопасности ресурсов Azure.

С помощью Помощника можно:
* получить упреждающие, действенные и персонализированные практические рекомендации; 
* повысить производительность, безопасность и уровень доступности ресурсов, выявляя при этом любые возможности сократить общие затраты на Azure;
* получить рекомендации о доступных встроенных действиях.

Вы можете получить доступ к помощнику через [портал Azure](https://aka.ms/azureadvisordashboard). Войдите на [портал](https://portal.azure.com), в меню навигации найдите **Помощник** или выполните поиск в меню **Все службы**.

Панель мониторинга Помощника отображает персонализированные рекомендации для всех ваших подписок.  Вы можете применить фильтры, чтобы рекомендации отображались для определенных подписок и типов ресурсов.  Рекомендации делятся на пять категорий: 

* **Высокий уровень доступности** — предназначены обеспечить и улучшить непрерывную работу критически важных бизнес-приложений. Дополнительные сведения см. в разделе [Рекомендации Azure Advisor по высокой доступности](advisor-high-availability-recommendations.md).
* **Безопасность** — предназначены для выявления угроз и уязвимостей, которые могут привести к нарушениям безопасности. Дополнительные сведения см. в разделе [Рекомендации Azure Advisor по безопасности](advisor-security-recommendations.md).
* **Производительность** — предназначены для повышения скорости работы ваших приложений. Дополнительные сведения см. в разделе [Рекомендации Azure Advisor по производительности](advisor-performance-recommendations.md).
* **Стоимость** — предназначены оптимизировать и сократить общие издержки на Azure. Дополнительные сведения см. в разделе [Рекомендации Azure Advisor по затратам](advisor-cost-recommendations.md).
* **Эффективное эксплуатация**: помогает достичь эффективности процессов и рабочих процессов, рекомендации по управлению ресурсами и развертыванию. . Дополнительные сведения см. в разделе [рекомендации по оперативной работе с помощником](advisor-operational-excellence-recommendations.md).

  ![Типы рекомендаций Advisor](./media/advisor-overview/advisor-dashboard.png)

Вы можете щелкнуть категорию, чтобы отобразился список рекомендаций по этой категории, а затем выбрать определенную рекомендацию и узнать о ней больше.  Можно также узнать о действиях, которые можно выполнить, чтобы воспользоваться преимуществами предлагаемой возможности или устранить проблему.

![Категория рекомендаций Помощника](./media/advisor-overview/advisor-ha-category-example.png) 

Выберите рекомендуемое действие, чтобы применить эту рекомендацию.  Откроется простой интерфейс, позволяющий применить рекомендацию или отсылающий к документации, которая поможет ее применить.  После применения рекомендации Помощнику может потребоваться до одного дня, чтобы обновить эти сведения.

Если вы не собираетесь немедленно реагировать на рекомендацию, можно отложить ее на определенное время или закрыть.  Если вы не хотите получать рекомендации для определенной подписки или группы ресурсов, то можете настроить Помощник таким образом, чтобы он формировал рекомендации только для указанных подписок и групп ресурсов.

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

### <a name="how-do-i-access-advisor"></a>Как воспользоваться Advisor?
Вы можете получить доступ к помощнику через [портал Azure](https://aka.ms/azureadvisordashboard). Войдите на [портал](https://portal.azure.com), в меню навигации найдите **Помощник** или выполните поиск в меню **Все службы**.

Рекомендации Помощника можно также просмотреть через интерфейс ресурса виртуальной машины. Выберите виртуальную машину и прокрутите меню до рекомендаций Advisor. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Какие разрешения требуются для использования Advisor?
 
Рекомендации Помощника могут просматривать *владельцы*, *участники* или *читатели* подписки.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Для каких ресурсов Advisor предоставляет рекомендации?

Advisor предоставляет рекомендации для шлюза приложений, служб приложений, групп доступности, кэша Azure, фабрики данных Azure, базы данных Azure для MySQL, базы данных Azure для PostgreSQL, базы данных Azure для MariaDB, Azure ExpressRoute, Azure Cosmos DB, общедоступных IP-адресов Azure, хранилища данных SQL, серверов SQL, учетных записей хранения, профилей диспетчера трафика и виртуальных машин.

Помощник Azure также включает рекомендации из [центра безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-recommendations) , которые могут включать рекомендации для дополнительных типов ресурсов.

### <a name="can-i-postpone-or-dismiss-a-recommendation"></a>Можно ли отложить или закрыть рекомендацию?

Чтобы отложить или закрыть рекомендацию, щелкните ссылку **Отложить**. Можно указать период времени, на который откладывается рекомендация, или выбрать значение **Никогда**, чтобы закрыть ее.

## <a name="next-steps"></a>Дальнейшие шаги

Чтобы узнать больше о рекомендациях Помощника, ознакомьтесь с приведенными ниже материалами.

* [Начало работы с Помощником по Azure](advisor-get-started.md)
* [Рекомендации Azure Advisor по высокой доступности](advisor-high-availability-recommendations.md)
* [Рекомендации Azure Advisor по безопасности](advisor-security-recommendations.md)
* [Рекомендации Azure Advisor по производительности](advisor-performance-recommendations.md)
* [Рекомендации Azure Advisor по затратам](advisor-cost-recommendations.md)
