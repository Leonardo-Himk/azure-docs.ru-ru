---
title: Операционная безопасность Azure | Документация Майкрософт
description: Узнайте о журналах Microsoft Azure Monitor, его службах и принципах работы.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 34c0c52945abc6e0ab74b1cb180581c76464bee8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75749959"
---
# <a name="azure-operational-security"></a>Операционная безопасность Azure
## <a name="introduction"></a>Введение

### <a name="overview"></a>Обзор
Мы понимаем, что в облаке безопасность имеет наивысший приоритет и вам крайне важно получать точные и своевременные сведения о безопасности Azure. Одним из наиболее весомых доводов в пользу использования Azure для приложений и служб является доступ к обширному ассортименту различных средств и возможностей обеспечения безопасности. Эти средства и возможности мы предлагаем вам для создания безопасных решений на безопасной платформе Azure. Служба Microsoft Azure должна обеспечивать конфиденциальность, целостность и доступность данных клиента, а также прозрачный учет.

Этот технический документ, "Операционная безопасность Azure", написан, чтобы помочь клиентам лучше понять массив средств управления безопасностью, реализованных в Microsoft Azure. В нем представлен взгляд как с точки зрения эксплуатации клиента, так и корпорации Майкрософт, а также исчерпывающие сведения о безопасности работы в Microsoft Azure.

### <a name="azure-platform"></a>Платформа Azure
Azure — общедоступная облачная платформа, которая поддерживает широкий выбор операционных систем, языков программирования, платформ, инструментов, баз данных и устройств. На ней можно запускать контейнеры Linux с интеграцией Docker, создавать приложения на языках JavaScript, Python, .NET, PHP, Java и Node.js, разрабатывать серверные решения для устройств под управлением iOS, Android и Windows. Облачная служба Azure поддерживает технологии, которым доверяют миллионы разработчиков и ИТ-специалистов.

Создавая ресурсы ИТ или перенося их в поставщик общедоступных облачных служб, вы полагаетесь на возможности организации по обеспечению защиты приложений и данных с помощью предоставляемых решений для управления безопасностью облачных ресурсов.

Инфраструктура устройств и приложений Azure предназначена для размещения миллионов пользователей одновременно. Это надежная база, которая удовлетворяет корпоративным требованиям к обеспечению безопасности. Кроме того, Azure предоставляет самые разные настраиваемые решения для обеспечения безопасности, а также возможность управления ими. Все это позволит настроить защиту в соответствии с уникальными особенностями развернутых в организации служб. Данный документ поможет вам понять, каким образом функции безопасности Azure позволяют удовлетворить эти требования.

### <a name="abstract"></a>Аннотация
Под операционной безопасностью Azure понимаются службы, элементы управления и функции, которые пользователи могут использовать для защиты своих данных, приложений и других ресурсов в Microsoft Azure. Операционная безопасность Azure основывается на знаниях, полученных в процессе эксплуатации ряда уникальных систем корпорации Майкрософт, включая жизненный цикл разработки защищенных приложений (Майкрософт) (SDL) и программу Microsoft Security Response Center, а также на глубокой осведомленности в области угроз кибербезопасности.

В этом техническом документе представлен подход корпорации Майкрософт к операционной безопасности Azure, применяемый на облачной платформе Microsoft Azure, и описаны следующие службы:
1.  [Azure Monitor](../../azure-monitor/index.yml)

2.  [Центр безопасности Azure](../../security-center/security-center-intro.md)

3.  [Azure Monitor](../../azure-monitor/overview.md)

4.  [Наблюдатель за сетями Azure;](../../network-watcher/network-watcher-monitoring-overview.md)

5.  [Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md)


## <a name="microsoft-azure-monitor-logs"></a>Журналы мониторинга Microsoft Azure

Журналы мониторинга Microsoft Azure — это решение по управлению ИТ-средой для гибридного облака. Azure Monitor журналы, которые используются отдельно или для расширения существующего развертывания System Center, обеспечивают максимальную гибкость и контроль над облачным управлением инфраструктуры.

![Журналы Azure Monitor](./media/operational-security/azure-operational-security-fig1.png)

С помощью журналов Azure Monitor можно управлять любыми экземплярами в любом облаке, включая локальные, Azure, AWS, Windows Server, Linux, VMware и OpenStack, с более низкой стоимостью, чем конкурентоспособные решения. Azure Monitor журналы, разработанные для облачного использования в мире, предлагают новый подход к управлению предприятием, который является самым быстрым и экономичным способом удовлетворения новых бизнес-задач и размещения новых рабочих нагрузок, приложений и облачных сред.

### <a name="azure-monitor-services"></a>Службы Azure Monitor

Основные функциональные возможности журналов Azure Monitor предоставляются набором служб, работающих в Azure. Каждая служба предоставляет определенную функцию управления, благодаря чему можно комбинировать службы для получения различных сценариев управления.

| Служба  | Описание|
| :------------- | :-------------|
| Журналы Azure Monitor | Отслеживает и анализирует доступность и производительность различных ресурсов, включая физические компьютеры и виртуальные машины. |
|Служба автоматизации | Автоматизирует ручные процессы и обеспечивает принудительное применение конфигураций для физических компьютеров и виртуальных машин. |
| Резервное копирование | Архивирует и восстанавливает критически важные данные. |
| Site Recovery | Обеспечивает высокую доступность критически важных приложений. |

### <a name="azure-monitor-logs"></a>Журналы Azure Monitor

[Журналы Azure Monitor](https://azure.microsoft.com/documentation/services/log-analytics) предоставляют службы мониторинга, собирая данные из управляемых ресурсов в центральный репозиторий. Эти данные могут содержать события, данные о производительности или пользовательские данные, полученные с помощью API. Собранные данные доступны для оповещения, анализа и экспорта.


Этот метод позволяет консолидировать данные из различных источников таким образом, чтобы вы могли объединять данные из служб Azure с существующей локальной средой. Он также явно отделяет сбор данных от действия над этими данными, чтобы все действия были доступны для всех видов данных.


![Журналы Azure Monitor](./media/operational-security/azure-operational-security-fig2.png)

Служба Azure Monitor безопасно управляет облачными данными с помощью следующих методов.
-   разделение данных;
-   хранение данных;
-   обеспечение физической безопасности;
-   управление инцидентами;
-   обеспечение соответствия требованиям;
-   соответствие сертификатам и стандартам безопасности.

### <a name="azure-backup"></a>Azure Backup

[Azure Backup](https://azure.microsoft.com/documentation/services/backup) предоставляет службы резервного копирования и восстановления данных и входит в состав Azure Monitor набора продуктов и служб.
Она защищает данные приложений и хранит их в течение нескольких лет без каких-либо капитальных затрат и с минимальными операционными затратами. Она может архивировать данные с физических и виртуальных серверов Windows в дополнение к рабочим нагрузкам приложений, таких как SQL Server и SharePoint. Кроме того, она может использоваться с [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) для репликации защищенных данных в Azure для обеспечения избыточности и долговременного хранения.


Защищенные данные в службе резервного копирования Azure хранятся в хранилище архивации, расположенном в определенном географическом регионе. Как правило, данные реплицируются в том же регионе, однако (в зависимости от типа хранилища) репликация может выполняться в другом регионе для обеспечения дополнительной устойчивости.

### <a name="management-solutions"></a>Решения для управления
[Azure Monitor](../../security-center/security-center-intro.md) — это облачное решение Майкрософт для управления ИТ-средой, которое помогает управлять локальной и облачной инфраструктурой и защищать ее.


[Решения по управлению](../../monitoring/monitoring-solutions.md) — это предварительно упакованные наборы логики, реализующие определенный сценарий управления с использованием одной или нескольких служб Azure Monitor. Различные решения доступны корпорации Майкрософт и партнерам, которые можно легко добавить в подписку Azure, чтобы увеличить ценность инвестиций в Azure Monitor. Как партнер Вы можете создавать собственные решения для поддержки приложений и служб, а также предоставлять их пользователям с помощью шаблонов быстрого запуска Azure Marketplace или быстрого запуска.


![Решения для управления](./media/operational-security/azure-operational-security-fig4.png)

Хорошим примером решения, которое использует несколько служб для обеспечения дополнительных функциональных возможностей, является [решение для управления обновлениями](../../automation/automation-update-management.md). В этом решении используется агент [журналов Azure Monitor](../../log-analytics/log-analytics-queries.md) для Windows и Linux, который собирает сведения о необходимых обновлениях на каждом агенте. Он записывает эти данные в репозиторий журналов Azure Monitor, где их можно проанализировать с помощью входящей панели мониторинга.

При создании развертывания модули Runbook в [службе автоматизации Azure](../../automation/automation-intro.md) используются для установки необходимых обновлений. Всем процессом можно управлять на портале, не заботясь о деталях.

## <a name="azure-security-center"></a>Центр безопасности Azure

Центр безопасности Azure помогает защитить ваши ресурсы Azure. Он включает в себя встроенные функции мониторинга безопасности и управления политиками для подписок Azure. С помощью службы можно определить политики не только для подписок Azure, но и для [групп ресурсов](../../azure-resource-manager/management/overview.md#resource-groups), что сделает ваши настройки более точными.

### <a name="security-policies-and-recommendations"></a>политики безопасности и рекомендации по ее обеспечению;

Политика безопасности определяет набор элементов управления, которые рекомендуются для ресурсов в указанной подписке или группе ресурсов.

В центре безопасности вы можете настраивать политики в соответствии с требованиями безопасности вашей компании, типом приложений и конфиденциальностью данных.

![политики безопасности и рекомендации по ее обеспечению;](./media/operational-security/azure-operational-security-fig5.png)


Политики, включенные на уровне подписки, автоматически распространяются на все группы ресурсов в рамках подписки, как показано на схеме справа.


### <a name="data-collection"></a>Сбор данных

Центр безопасности собирает с виртуальных машин данные, на основе которых оценивает состояние их безопасности, предоставляет рекомендации по безопасности и оповещает об угрозах. Когда вы впервые обращаетесь в центр безопасности, сбор данных включается на всех виртуальных машинах, входящих в вашу подписку. Мы рекомендуем использовать сбор данных, но вы можете отказаться от него, отключив этот параметр в политике центра безопасности.

### <a name="data-sources"></a>Источники данных

- С целью отслеживания состояния безопасности, определения уязвимостей системы и их устранения в соответствии с рекомендациями, а также обнаружения активных угроз центр безопасности Azure анализирует данные из следующих источников:

-   Службы Azure. Из развернутых служб Azure считываются сведения о конфигурации путем взаимодействия с поставщиком ресурсов соответствующей службы.

- Сетевой трафик. Из инфраструктуры Майкрософт считывается выборка метаданных сетевого трафика, например IP-адрес и порт источника и назначения, размер пакета и сетевой протокол.

-   Решения партнеров. Из интегрированных решений партнеров, например брандмауэров и решений по защите от вредоносных программ, собираются оповещения системы безопасности.

-   Ваши виртуальные машины. С виртуальных машин собираются сведения о конфигурации и событиях безопасности, например события Windows, данные журналов аудита и журналов IIS, а также сообщения системного журнала и файлы аварийного дампа.

### <a name="data-protection"></a>Защита данных

Чтобы помочь клиентам предотвращать и выявлять угрозы, а также реагировать на них, центр безопасности Azure собирает и обрабатывает данные о безопасности, в том числе сведения о конфигурации, метаданные, журналы событий, файлы аварийных дампов и многое другое. Корпорация Майкрософт следует строгим нормативным требованиям и указаниям по безопасности — от создания кода до эксплуатации служб.

-   **Разделение данных.** Данные логическим образом отделяются для каждого компонента службы. Все данные отмечаются тегами по организациям. Эти теги существуют в течение всего жизненного цикла данных и используются на каждом уровне службы.

-   **Доступ к данным**. С целью предоставления рекомендаций по обеспечению безопасности и обнаружения потенциальных угроз безопасности сотрудники корпорации Майкрософт могут получать доступ к сведениям, которые собираются или анализируются службами Azure. В частности, к таким данным относятся файлы аварийных дампов, сведения о событиях создания процесса, моментальные снимки диска, артефакты виртуальной машины и другие сведения, которые могут непреднамеренно содержать данные клиента или персональные данные из виртуальных машин. Мы соблюдаем [условия использования и заявление о конфиденциальности Microsoft Online Services](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), которые гласят, что корпорация Майкрософт не использует данные клиента и не извлекает из них сведения для рекламных или аналогичных коммерческих целей.

-   **Использование данных.** Корпорация Майкрософт использует шаблоны и данные анализа угроз, которые наблюдались у нескольких клиентов, чтобы улучшить возможности предотвращения и обнаружения. При этом соблюдаются обязательства по безопасности, описанные в [заявлении о конфиденциальности](https://www.microsoft.com/en-us/privacystatement/OnlineServices/).

### <a name="data-location"></a>Расположение данных

Центр безопасности Azure собирает временные копии файлов аварийного дампа и анализирует их на признаки попыток компрометации и удачной компрометации. Этот анализ выполняется в том географическом регионе, к которому привязана рабочая область. После завершения анализа Центр безопасности Azure удаляет временные копии. Артефакты виртуальной машины хранятся централизованно в том же регионе, что и виртуальная машина.

-   **Учетные записи хранения**. Учетная запись хранения указывается для каждого региона, в котором работают виртуальные машины. Таким образом, данные можно хранить в том же регионе, где расположена виртуальная машина, из которой выполняется сбор данных.

-   **Хранилище центра безопасности Azure**. Здесь хранятся сведения об оповещениях системы безопасности (в том числе оповещения от партнеров), состоянии работоспособности системы безопасности и рекомендации. В настоящее время указанные данные хранятся централизованно в США. Эти сведения могут включать в себя связанные данные конфигурации и события безопасности, собранные из виртуальных машин для предоставления оповещений безопасности, состояния работоспособности системы безопасности и рекомендаций.


## <a name="azure-monitor"></a>Azure Monitor

Решение "безопасность и аудит" [Azure Monitor журналов](../../security-center/security-center-monitoring.md) позволяет ИТ активно отслеживать все ресурсы, что может помочь в уменьшении влияния инцидентов безопасности. Журналы Azure Monitor Безопасность и аудит имеют домены безопасности, которые можно использовать для мониторинга ресурсов. Домен безопасности предоставляет быстрый доступ к параметрам. Мы более подробно рассмотрим следующие домены, которые имеют отношение к мониторингу безопасности:

-   Оценка вредоносных программ
-   Оценка обновлений
-   Идентификация и доступ

Служба Azure Monitor содержит указатели на сведения для определенных типов ресурсов. Она предлагает возможности визуализации, создания запросов, маршрутизации, оповещения, автомасштабирования и автоматизации на основе данных инфраструктуры Azure (журнал действий) и каждого отдельного ресурса Azure (журналы диагностики).

![Azure Monitor](./media/operational-security/azure-operational-security-fig6.png)


Облачные приложения являются сложными и содержат множество подвижных частей. Мониторинг дает возможность отслеживать данные, чтобы обеспечить работоспособность приложения, а также позволяет предотвратить потенциальные проблемы или устранить неполадки.

Кроме того, данные мониторинга можно использовать для получения подробных сведений о приложении. Эти знания могут помочь повысить его производительность и улучшить возможности обслуживания, а также автоматизировать действия, которые в противном случае выполнялись бы вручную.

### <a name="azure-activity-log"></a>Журнал действий Azure


Это журнал с информацией об операциях, которые выполнялись с ресурсами в подписке. Журнал действий раньше назывался журналом аудита или операционным журналом, так как он содержит связанные с подписками события на уровне управления.

![Журнал действий Azure](./media/operational-security/azure-operational-security-fig7.png)

С помощью журнала изменений можно ответить на вопросы "что? кто? когда?" о любой операции записи (PUT, POST, DELETE) с ресурсами в вашей подписке. Вы также можете отслеживать состояние операции и другие ее свойства. Журнал действий не содержит операции чтения (GET) или операции с ресурсами, которые используют классическую модель.

### <a name="azure-diagnostic-logs"></a>Журналы диагностики Azure

Эти журналы выдаются ресурсом. Они содержат подробные и своевременные данные о работе этого ресурса. Содержимое этих журналов зависит от типа ресурса.

Например, системные журналы событий Windows — это одна из категорий журналов диагностики для виртуальных машин, а журналы больших двоичных объектов, таблиц и очередей — категории журналов диагностики для учетных записей хранения.

Журналы диагностики отличаются от [журналов действий (прежнее название — журналы аудита или операционные журналы)](../../azure-monitor/platform/platform-logs-overview.md). Журнал действий Azure — это журнал с информацией об операциях, которые выполнялись с ресурсами в подписке. Журналы диагностики дают представление об операциях, выполняемых самим ресурсом.

### <a name="metrics"></a>Метрики

Azure Monitor позволяет использовать данные телеметрии, чтобы получать сведения о производительности и работоспособности рабочих нагрузок в Azure. Наиболее важным типом данных телеметрии Azure являются метрики (также называемые счетчиками производительности), создаваемые большинством ресурсов Azure. Azure Monitor предоставляет несколько способов настройки и использования этих [метрик](../../monitoring/monitoring-data-collection.md) для мониторинга и устранения неполадок. Метрики — ценный источник данных телеметрии. С помощью метрик можно решать следующие задачи:

-   **Отслеживать производительность** ресурса (например, виртуальной машины, веб-сайта или приложения логики), создав на портале диаграмму соответствующих метрик. Эту диаграмму можно закрепить на панели мониторинга.

-   **Получать уведомления о проблемах**, влияющих на производительность ресурсов, когда значения метрик достигают определенных пороговых значений.

-   **Настраивать автоматизированные действия**, например автомасштабирование ресурса или запуск модуля Runbook, когда значение метрики достигает определенного порогового значения.

-   **Выполнять расширенную аналитику** или создавать подробные отчеты о производительности ресурсов или тенденциях их использования.

-   **Архивировать** журналы производительности и работоспособности ресурсов для соответствия требованиям или проведения аудита.

### <a name="azure-diagnostics"></a>Диагностика Azure

Эта функция Azure позволяет выполнять сбор диагностических данных в развернутом приложении. Можно использовать расширение диагностики из нескольких различных источников. В настоящее время поддерживаются [веб-роли и рабочие роли облачной службы Azure](/visualstudio/azure/vs-azure-tools-configure-roles-for-cloud-service), [виртуальные машины Azure](../../virtual-machines/windows/overview.md) под управлением Microsoft Windows и [Service Fabric](../../azure-monitor/platform/diagnostics-extension-overview.md). Другие службы Azure располагают собственной диагностикой.

## <a name="azure-network-watcher"></a>Наблюдатель за сетями Azure

Аудит безопасности сети критически важен для выявления уязвимостей сети и обеспечения соответствия вашей модели управления и нормативным требованиям ИТ-безопасности. Представление группы безопасности позволяет получить настроенную группу безопасности сети и правила безопасности, а также просмотреть действующие правила безопасности. С помощью списка примененных правил можно определить открытые порты и оценить уязвимость сети.

[Наблюдатель за сетями](../../network-watcher/network-watcher-monitoring-overview.md) — это региональная служба, обеспечивающая мониторинг и диагностику условий на уровне сети на платформе Azure. Инструменты диагностики сети и визуализации, доступные в Наблюдателе за сетями, помогают понять, как работает сеть в Azure, диагностировать ее и получить ценную информацию. Эта служба включает в себя запись пакетов, определение следующего прыжка, проверку IP-потока, представление групп безопасности и журналы потоков NSG. В отличие от мониторинга отдельных сетевых ресурсов, мониторинг на уровне сценария обеспечивает комплексное представление сетевых ресурсов.

![Наблюдатель за сетями Azure](./media/operational-security/azure-operational-security-fig8.png)

В настоящее время Наблюдатель за сетями имеет следующие возможности.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Журналы аудита</a>**. Регистрируются операции, выполняемые при настройке сетей. Эти журналы можно просмотреть на портале Azure или получить с помощью инструментов Майкрософт, таких как Power BI, или сторонних инструментов. Журналы аудита можно получить с помощью портала, PowerShell, интерфейса командной строки и REST API. Дополнительные сведения о журналах аудита см. в статье "Просмотр журналов действий для аудита действий с ресурсами". Доступны журналы аудита для операций, выполненных со всеми сетевыми ресурсами.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">Проверка IP-потока</a>**. Позволяет проверить, разрешен или запрещен пакет, на основе информации о пакете, указанной в пяти кортежах (IP-адрес назначения, исходный IP-адрес, порт назначения, исходный порт и протокол). Если пакет отклонен группой безопасности сети, то возвращаются правило и группа безопасности сети, которые отклонили этот пакет.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Следующий прыжок</a>** — определяет следующий прыжок для пакетов, направляемых в структуре сети Azure, что позволяет диагностировать все неправильно настроенные определяемые пользователем маршруты.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Представление группы безопасности</a>** — получает действующие и Примененные правила безопасности, которые применяются к виртуальной машине.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">Ведение журнала потоков NSG</a>**. Журналы потоков для групп безопасности сети позволяют записывать журналы, относящиеся к трафику, который разрешают или запрещают правила безопасности в группе. Поток определяется данными пяти кортежей: исходный IP-адрес, конечный IP-адрес, исходный порт, конечный порт и протокол.

## <a name="azure-storage-analytics"></a>Аналитика службы хранилища Azure

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) может хранить метрики, включающие сводную статистику по транзакциям и данные о емкости запросов к службе хранилища. Сообщения о транзакциях предоставляются как на уровне операций API, так и на уровне службы хранилища, а сообщения о емкости предоставляются на уровне службы хранилища. Данные метрик можно использовать для анализа использования службы хранилища, диагностики проблем с запросами к службе хранилища и повышения производительности приложений, использующих службу.

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) ведет журнал и предоставляет данные метрик для учетной записи хранения. Эти данные можно использовать для трассировки запросов, анализа тенденций использования и диагностики проблем учетной записи хранения. Ведение журнала Storage Analytics доступно для [служб BLOB-объектов, очередей и таблиц](../../storage/common/storage-introduction.md). В аналитике хранилища регистрируется подробная информация об успешных и неудачных запросах к службе хранилища.

Эта информация может использоваться для мониторинга отдельных запросов и диагностики неполадок в службе хранилища. Запросы вносятся в журнал наилучшим возможным образом. Записи журнала создаются только при получении запроса к конечной точке службы. Например, если обнаруживается действие учетной записи хранения в конечной точке службы BLOB-объектов, но не в ее конечных точках служб таблиц или очередей, то создаются журналы, которые относятся только к службе BLOB-объектов.

Для использования аналитики хранилища ее необходимо включить отдельно для каждой из отслеживаемых служб. Это можно сделать на [портале Azure](https://portal.azure.com/). Дополнительные сведения см. в статье [Мониторинг учетной записи хранения на портале Azure](../../storage/common/storage-monitor-storage-account.md). Аналитику хранилища также можно включить программно через REST API или клиентскую библиотеку. Используйте операции Set Service Properties, чтобы отдельно включить Storage Analytics для каждой службы.

Объединенные данные хранятся в известном большом двоичном объекте (для ведения журнала) и в известных таблицах (для метрик), доступ к которым можно получить с помощью API службы BLOB-объектов и службы таблиц.

В Storage Analytics установлено ограничение в 20 ТБ для объема хранимых данных, не зависящее от общего ограничения на вашу учетную запись хранения. Все журналы хранятся в [блочных BLOB-объектах](../../storage/common/storage-analytics.md) в контейнере $logs, который автоматически создается при включении Storage Analytics для учетной записи хранения.

Плата взимается за следующие действия, выполняемые аналитикой хранилища:

-   запросы на создание больших двоичных объектов для ведения журнала;
-   запросы на создание сущностей таблиц для метрик.

> [!Note]
> Дополнительную информацию о выставлении счетов и политиках хранения данных см. в разделе [Storage Analytics and Billing](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing) (Storage Analytics и выставление счетов).
> Для оптимальной производительности следует ограничить количество интенсивно используемых дисков, присоединенных к виртуальной машине, во избежание возможного регулирования. Если интенсивного одновременного использования дисков нет, то учетная запись хранения может поддерживать большее количество дисков.

> [!Note]
> Дополнительные сведения об ограничениях учетной записи хранения см. в разделе [целевые показатели масштабируемости для стандартных учетных записей хранения](../../storage/common/scalability-targets-standard-account.md).


Регистрируются аутентифицированные и анонимные запросы следующих типов.

| Аутентифицированные  | Анонимные|
| :------------- | :-------------|
| Успешные запросы. | Успешные запросы. |
|Неудачные запросы, в том числе из-за ошибок, связанных с временем ожидания, регулированием, сетью, авторизацией и др. | Запросы, в которых используется подписанный URL-адрес (SAS), в том числе неудачные и успешные запросы. |
| Запросы, в которых используется подписанный URL-адрес (SAS), в том числе неудачные и успешные запросы. |Ошибки времени ожидания для клиента и сервера. |
|   Запросы к данным аналитики. |    Неудачные запросы GET с кодом ошибки 304 (не изменено). |
| Запросы, выполненные в самой аналитике хранилища, такие как запросы на создание или удаление журнала, не регистрируются. Полный список регистрируемых данных приведен в разделах [Операции с протоколированием и сообщения о состоянии аналитики хранилища](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) и [Формат журналов аналитики хранилища](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | Остальные неудачные анонимные запросы не регистрируются. Полный список регистрируемых данных приведен в разделах [Операции с протоколированием и сообщения о состоянии аналитики хранилища](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) и [Формат журналов аналитики хранилища](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD также включает полный набор возможностей управления удостоверениями, в том числе многофакторную идентификацию, регистрацию устройств, самостоятельное управление паролями, самостоятельное управление группами, управление привилегированной учетной записью, управление доступом на основе ролей, отслеживание использования приложений, расширенный аудит, мониторинг безопасности и предупреждения.

-   Повышение безопасности приложений с помощью многофакторной проверки подлинности и условного доступа Azure AD.

-   Мониторинг использования приложений и защита от дополнительных угроз с отчетами и мониторингом безопасности.

Azure Active Directory (Azure AD) формирует отчеты о безопасности, активности и аудите каталога. [Отчет аудита Azure Active Directory](../../active-directory/active-directory-reporting-azure-portal.md) позволяет клиентам определить привилегированные действия, выполненные в их каталогах Azure Active Directory. К привилегированным действиям относятся изменение повышения (например, создание роли или сброс пароля), изменение конфигурации политики (например, политики паролей) или изменение конфигурации каталога (например, изменение параметров федерации домена).

Отчеты содержат такие сведения: имя события, выполнивший действие субъект, подвергшийся изменению целевой ресурс, дата и время (в формате UTC). Список событий аудита для службы Azure Active Directory можно получить на [портале Azure](https://portal.azure.com/). Дополнительные сведения см. в статье [Что такое отчеты в Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md). Ниже приведен список включенных отчетов.

| Отчеты о безопасности  | Отчеты об активности| Отчеты об аудите |
| :------------- | :-------------| :-------------|
|Попытки входа из неизвестных источников | Использование приложения: сводка | Отчет об аудите каталога |
|"Операции входа после нескольких неудачных попыток"; | Использование приложения: подробности |   |
|"Операции входа из нескольких географических регионов". | Панель мониторинга приложений |  |
|Попытки входа с IP-адресов с подозрительными действиями |Ошибки подготовки учетной записи |  |
|Нестандартные действия при входе |Устройства отдельного пользователя |  |
|Попытки входа с возможно инфицированных устройств |Активность отдельного пользователя |   |
|Пользователи с аномальными событиями при входе |Отчет о действиях групп |   |
| |Отчет о событиях регистрации для сброса пароля |   |
| |Действие сброса пароля |   |



Данные этих отчетов могут быть полезными для приложений, например систем SIEM, а также инструментов аудита и бизнес-аналитики. [Интерфейсы API](../../active-directory/active-directory-reporting-api-getting-started-azure-portal.md) отчетов Azure AD предоставляют программный доступ к данным с помощью набора интерфейсов API на базе REST. Эти интерфейсы API можно вызвать, используя различные языки программирования и инструменты.

События в отчете аудита Azure AD хранятся на протяжении 180 дней.

> [!Note]
> Дополнительные сведения о хранении отчетов см. в разделе [Azure Active Directory политики хранения отчетов](../../active-directory/reports-monitoring/reference-reports-data-retention.md).

Для клиентов, заинтересованных в хранении [событий аудита](../../active-directory/active-directory-reporting-activity-audit-logs.md) в течение более длительного периода хранения, API отчетов можно использовать для регулярного извлечения событий аудита в отдельное хранилище данных.

## <a name="summary"></a>Сводка

Эта статья содержит сводные данные о защите конфиденциальности и безопасности данных. Корпорация Майкрософт выпускает программное обеспечение и службы, которые помогают вам управлять ИТ-инфраструктурой своей организации. Мы понимаем, что когда вы доверяете свои данные сторонним организациям, они требуют надежной защиты. Корпорация Майкрософт следует строгим нормативным требованиям и указаниям по безопасности — от создания кода до эксплуатации служб.  Безопасность и защита данных — главный приоритет корпорации Майкрософт.

Содержание статьи

-   Как собираются, обрабатываются и защищаются данные в Azure Monitor Suite.

-   Данные нескольких источников теперь можно анализировать очень быстро. Выявляйте риски безопасности и прогнозируйте влияние угроз и атак, чтобы уменьшить риск получения ущерба от брешей в системе безопасности.

-   Определяйте шаблоны атак, используя визуализацию исходящего IP-трафика злоумышленников и типов угроз. Изучите состояние безопасности всей среды, на какой бы платформе она не работала.

-   Получите все данные журналов и событий, необходимые для аудита безопасности и соответствия требованиям. Тратьте меньше времени и ресурсов на проверку безопасности благодаря исчерпывающему набору данных журналов и событий, который можно найти и экспортировать.

<ul>
<li>Собирайте события безопасности и выполняйте аудит и расширенный анализ, чтобы внимательно следить за своими ресурсами:</li>
<ul>
<li>состояние безопасности;</li>
<li>важные проблемы;</li>
<li>сводные данные по угрозам.</li>
</ul>
</ul>

## <a name="next-steps"></a>Дальнейшие действия

- [Безопасность проектирования и работы](https://www.microsoft.com/trustcenter/security/designopsecurity)

Корпорация Майкрософт разрабатывает свои службы и программное обеспечение с учетом требований безопасности, что позволяет обеспечить отказоустойчивость облачной инфраструктуры и надежную защиту от атак.

- [Журналы Azure Monitor | Соответствие & безопасности](https://www.microsoft.com/cloud-platform/security-and-compliance)

Используйте функции сбора и анализа данных безопасности корпорации Майкрософт для более "разумного" и эффективного обнаружения угроз.

- [Руководство по планированию использования центра безопасности Azure и работе в нем](../../security-center/security-center-planning-and-operations-guide.md). Набор инструкций и задач, которые помогут оптимизировать использование центра безопасности с учетом требований к безопасности и модели управления облаком в вашей организации.

