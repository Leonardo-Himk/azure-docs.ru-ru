---
title: Архив новых возможностей центра безопасности Azure
description: Описание новых возможностей и изменений в центре безопасности Azure за шесть месяцев назад и более ранних версий.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2020
ms.author: memildin
ms.openlocfilehash: c0883e91d5e806fb166c3ddeafc4ce130ff3f66f
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83210847"
---
# <a name="archive-for-whats-new-in-azure-security-center"></a>Архив для новых возможностей центра безопасности Azure

На странице "заметки о выпуске" [Azure Active Directory в разделе "Основные новые возможности](release-notes.md) " содержатся обновления за последние шесть месяцев, а на этой странице содержатся старые элементы.

На этой странице представлены сведения о:

- Новые функции
- Исправления ошибок
- Нерекомендуемые функции.

## <a name="november-2019"></a>Ноябрь 2019 г.

### <a name="threat-protection-for-azure-key-vault-in-public-preview-in-north-america-regions"></a>Защита от угроз для Azure Key Vault в общедоступной предварительной версии в Северная Америка регионах

Azure Key Vault является важной службой для защиты данных и повышения производительности облачных приложений за счет предоставления возможности централизованного управления ключами, секретами, криптографическими ключами и политиками в облаке. Поскольку Azure Key Vault хранит конфиденциальные и критически важные данные, она требует максимальной безопасности для хранилищ ключей и хранящихся в них данных.

Поддержка защиты от угроз в центре безопасности Azure для Azure Key Vault предоставляет дополнительный уровень логики безопасности, который обнаруживает необычные и потенциально опасные попытки доступа или использования хранилищ ключей. Этот новый уровень защиты позволяет клиентам устранять угрозы в своих хранилищах ключей без эксперта по безопасности или управления системами мониторинга безопасности. Эта функция доступна в общедоступной предварительной версии в Северная Америка регионах.


### <a name="threat-protection-for-azure-storage-includes-malware-reputation-screening"></a>Защита от угроз для службы хранилища Azure включает в себя отбор репутации вредоносных программ

Защита от угроз для службы хранилища Azure предлагает новые обнаружения, реализованные в Microsoft Threat Intelligence, для обнаружения передачи вредоносных программ в службу хранилища Azure с помощью анализа репутации хэша и подозрительного доступа из активного узла выхода Tor (анонимизирующие proxy). Теперь вы можете просматривать обнаруженные вредоносные программы в учетных записях хранения с помощью центра безопасности Azure.


### <a name="workflow-automation-with-logic-apps-preview"></a>Автоматизация рабочих процессов с помощью Logic Apps (Предварительная версия)

Организации, имеющие централизованное управление безопасностью, а также ИТ-операции, реализуют внутренние процессы рабочего процесса, чтобы обеспечить необходимое действие в Организации при обнаружении расхождений в своих средах. Во многих случаях эти рабочие процессы являются повторяемыми процессами, и автоматизация может значительно упростить процессы в Организации.

Сегодня мы представляем новую возможность в центре безопасности, которая позволяет клиентам создавать конфигурации автоматизации, использующие Azure Logic Apps и создавать политики, которые будут автоматически запускать их на основе конкретных результатов ASC, таких как рекомендации или оповещения. Приложение логики Azure можно настроить для выполнения любого настраиваемого действия, поддерживаемого обширным сообществом соединителей приложений логики, или использовать один из шаблонов, предоставляемых центром безопасности, например отправить сообщение электронной почты или открыть билет™ ServiceNow.

Дополнительные сведения о возможностях автоматического и ручного центра безопасности для запуска рабочих процессов см. в статье [Автоматизация рабочих](workflow-automation.md)процессов.

Дополнительные сведения о создании Logic Apps см. в разделе [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).


### <a name="quick-fix-for-bulk-resources-generally-available"></a>Быстрое исправление для небольшого количества ресурсов в общем доступе

При большом количестве задач, которые пользователь получает в рамках безопасной оценки, возможность эффективного устранения проблем в большом парке может стать сложной задачей.

Чтобы упростить исправление ошибок в настройках системы безопасности и быстро исправлять рекомендации для большого количества ресурсов и улучшить безопасность, используйте быстрое исправление исправлений.

Эта операция позволяет выбрать ресурсы, к которым необходимо применить исправление, и запустить действие исправления, которое будет настраивать этот параметр от вашего имени.

Быстрое исправление общедоступно для клиентов в рамках страницы рекомендаций центра безопасности.

Ознакомьтесь с рекомендациями по безопасности в разделе рекомендации по быстрому исправлению, которое включено в [справочном руководстве](recommendations-reference.md).


### <a name="scan-container-images-for-vulnerabilities-preview"></a>Проверять образы контейнеров на наличие уязвимостей (Предварительная версия)

Центр безопасности Azure теперь может проверять образы контейнеров в реестре контейнеров Azure на наличие уязвимостей.

Сканирование изображений выполняется путем анализа файла образа контейнера и проверки на наличие известных уязвимостей (на платформе Qualys).

Само сканирование автоматически запускается при помещении новых образов контейнеров в реестр контейнеров Azure. Обнаруженные уязвимости будут выдержаться в виде рекомендаций центра безопасности и включены в показатель безопасности Azure вместе со сведениями об их исправлении для уменьшения разрешенной уязвимой зоны.


### <a name="additional-regulatory-compliance-standards-preview"></a>Дополнительные стандарты соответствия нормативным требованиям (Предварительная версия)

Панель мониторинга соответствия нормативным требованиям предоставляет аналитические сведения о соответствии вашей оценке на основе оценок центра безопасности. На панели мониторинга показано, как ваша среда соответствует элементам управления и требованиям, определенным нормативным стандартам и отраслевым производительности, и предоставляет рекомендации по решению этих требований.

На панели мониторинга соответствия нормативным требованиям поддерживались четыре встроенных стандарта: Azure CIS 1.1.0, PCI-DSS, ISO 27001 и SOC-TSP. Теперь мы сообщаем о общедоступной предварительной версии дополнительных поддерживаемых стандартов: NIST SP 800-53 R4, SWIFT CSP КСКФ v2020, федерального правительства США ПБММ и Великобритании, а также в Великобритании отделения. Мы также выпустили обновленную версию Azure CIS 1.1.0, охватывающую больше элементов управления из стандартной и улучшенной расширяемости.

[Узнайте больше о настройке набора стандартов на панели мониторинга соответствия нормативным требованиям](update-regulatory-compliance-packages.md).


### <a name="threat-protection-for-azure-kubernetes-service-preview"></a>Защита от угроз для службы Kubernetes Azure (Предварительная версия)

Kubernetes быстро становится новым стандартом для развертывания и управления программным обеспечением в облаке. Некоторые люди имеют обширный опыт работы с Kubernetes, и многие из них сосредоточены только на общем проектировании и администрировании и не заявляют аспекты безопасности. Чтобы обеспечить безопасность среды Kubernetes, необходимо тщательно настроить ее, убедившись, что для злоумышленников не осталось открыть дверцы уязвимой зоны. Центр безопасности расширяет свою поддержку в пространстве контейнера до одной из самых быстрых растущих служб в Azure — Azure Kubernetes Service (AKS).

Новые возможности в этой общедоступной предварительной версии включают:

- **Обнаружение & видимость** — непрерывное обнаружение УПРАВЛЯЕМЫХ экземпляров AKS в зарегистрированных подписках центра безопасности.
- **Рекомендации по обеспечению** безопасности — действия, которые помогут клиентам соответствовать рекомендациям по обеспечению безопасности в AKS в рамках оценки безопасности клиента, например "Управление доступом на основе ролей", чтобы ограничить доступ к кластеру службы Kubernetes.
- **Обнаружение угроз** — аналитика на основе узла и кластера, например "обнаружен привилегированный контейнер".


### <a name="virtual-machine-vulnerability-assessment-preview"></a>Оценка уязвимости виртуальной машины (Предварительная версия)

Приложения, установленные на виртуальных машинах, часто могут иметь уязвимости, которые могут привести к нарушению работы виртуальной машины. Мы сообщаем, что уровень "Стандартный" центра безопасности включает в себя встроенную оценку уязвимостей для виртуальных машин без дополнительной платы. Оценка уязвимостей, предоставляемая Qualys в общедоступной предварительной версии, позволяет непрерывно сканировать все установленные приложения на виртуальной машине, чтобы найти уязвимые приложения и показать результаты работы портала центра безопасности. Центр безопасности следит за всеми операциями развертывания, чтобы не требовалось никаких дополнительных действий от пользователя. Теперь мы планируем предоставить варианты оценки уязвимостей для удовлетворения уникальных бизнес-потребностей клиентов.

[Узнайте больше об оценке уязвимостей для виртуальных машин Azure](security-center-vulnerability-assessment-recommendations.md).


### <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Расширенная защита данных для серверов SQL Server на виртуальных машинах Azure (Предварительная версия)

Поддержка центра безопасности Azure для защиты от угроз и оценки уязвимостей для SQL баз данных, выполняемых на виртуальных машинах IaaS, теперь доступна в предварительной версии.

[Оценка уязвимостей](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) — это простая настройка службы, которая может обнаруживать, записывать и помогать устранять потенциальные уязвимости базы данных. Он обеспечивает видимость уровня безопасности в рамках оценки безопасности Azure и включает действия по устранению проблем безопасности и улучшению фортификатионс базы данных.

[Расширенная защита от угроз](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) обнаруживает аномальные действия, указывающие на необычные и потенциально опасные попытки доступа или использования сервера SQL Server. Она постоянно отслеживает подозрительные действия в базе данных и предоставляет оповещения системы безопасности, ориентированные на действия, для аномальных шаблонов доступа к базам данных. Эти оповещения предоставляют сведения о подозрительном мероприятии и рекомендуемые действия для изучения и устранения угрозы.


### <a name="support-for-custom-policies-preview"></a>Поддержка настраиваемых политик (Предварительная версия)

Центр безопасности Azure теперь поддерживает пользовательские политики (в предварительной версии).

Наши клиенты хотят расширить свое текущее покрытие системы безопасности в центре безопасности с помощью собственных средств оценки безопасности на основе политик, создаваемых в политике Azure. Теперь это возможно благодаря поддержке пользовательских политик.

Эти новые политики будут включены в рекомендации центра безопасности, оценить безопасность и панель мониторинга стандартов соответствия нормативным требованиям. Благодаря поддержке пользовательских политик теперь можно создать пользовательскую инициативу в политике Azure, а затем добавить ее в качестве политики в центре безопасности и визуализировать ее как рекомендацию.


### <a name="extending-azure-security-center-coverage-with-platform-for-community-and-partners"></a>Расширение покрытия центра безопасности Azure с помощью платформы для сообщества и партнеров

Используйте центр безопасности, чтобы получать рекомендации не только от корпорации Майкрософт, но и из существующих решений партнеров, таких как Check Point, Тенабле и Циберарк с гораздо более интеграцией.  Простой поток адаптации центра безопасности позволяет подключать существующие решения к центру безопасности, позволяя просматривать рекомендации по обеспечению безопасности в одном месте, запускать единые отчеты и использовать все возможности центра безопасности в отношении встроенных и партнерских рекомендаций. Вы также можете экспортировать рекомендации центра безопасности в продукты партнеров.

[Дополнительные сведения о интеллектуальной сопоставлении безопасности Майкрософт](https://www.microsoft.com/security/partnerships/intelligent-security-association).



### <a name="advanced-integrations-with-export-of-recommendations-and-alerts-preview"></a>Расширенные возможности интеграции с экспортом рекомендаций и оповещений (Предварительная версия)

Чтобы включить сценарии уровня предприятия на основе центра безопасности, теперь можно использовать оповещения и рекомендации центра безопасности в дополнительных местах, кроме портал Azure или API. Их можно экспортировать непосредственно в концентратор событий и в Log Analytics рабочих областей. Ниже приведены несколько рабочих процессов, которые можно создать вокруг этих новых возможностей:

- С помощью экспорта в Log Analytics рабочую область можно создавать настраиваемые панели мониторинга с Power BI.
- При экспорте в концентратор событий вы сможете экспортировать оповещения и рекомендации центра безопасности для решения Siem сторонних разработчиков в стороннее решение в режиме реального времени или Azure обозреватель данных.


### <a name="onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview"></a>Подключение локальных серверов к центру безопасности из центра администрирования Windows (Предварительная версия)

Центр администрирования Windows — это портал управления для серверов Windows, которые не развертываются в Azure и предлагают несколько возможностей управления Azure, таких как резервное копирование и обновление системы. Недавно мы добавили возможность подключения этих серверов, не относящихся к Azure, для защиты с помощью ASC непосредственно из центра администрирования Windows.

С этим новым интерфейсом пользователи смогут подключить сервер ВАК к центру безопасности Azure и включить Просмотр оповещений системы безопасности и рекомендаций непосредственно в центре администрирования Windows.


## <a name="september-2019"></a>Сентябрь 2019 г.

### <a name="managing-rules-with-adaptive-application-controls-improvements"></a>Улучшенное управление правилами с помощью адаптивных элементов управления приложениями

Улучшены возможности управления правилами для виртуальных машин с использованием адаптивных элементов управления приложениями. Адаптивные элементы управления приложениями в центре безопасности Azure помогают контролировать, какие приложения могут выполняться на виртуальных машинах. Помимо общего улучшения управления правилами, новое преимущество позволяет контролировать, какие типы файлов будут защищены при добавлении нового правила.

Дополнительные [сведения о адаптивных элементах управления приложениями](security-center-adaptive-application.md).


### <a name="control-container-security-recommendation-using-azure-policy"></a>Управление рекомендациями по безопасности контейнера с помощью политики Azure

Рекомендации центра безопасности Azure по исправлению уязвимостей в безопасности контейнеров теперь можно включить или отключить с помощью политики Azure.

Чтобы просмотреть включенные политики безопасности, в центре безопасности откройте страницу Политика безопасности.


## <a name="august-2019"></a>Август 2019 г.

### <a name="just-in-time-jit-vm-access-for-azure-firewall"></a>JIT-доступ к виртуальной машине для брандмауэра Azure 

Доступ к виртуальной машине по требованию (JIT) для брандмауэра Azure теперь общедоступен. Используйте его для защиты защищенных сред брандмауэра Azure в дополнение к защищенным средам NSG.

JIT-доступ к виртуальным машинам снижает уязвимость сетевых объемные, предоставляя контролируемый доступ к виртуальным машинам только при необходимости, используя правила брандмауэра NSG и Azure.

При включении JIT для виртуальных машин создается политика, определяющая защищаемые порты, период времени, в течение которого порты остаются открытыми, и IP-адреса, с которых можно получить доступ к этим портам. Эта политика позволяет контролировать, какие действия пользователи могут выполнять при запросе доступа.

Запросы регистрируются в журнале действий Azure, что позволяет легко отслеживать и контролировать доступ. JIT-страница также помогает быстро выявить существующие виртуальные машины с включенным JIT и виртуальными машинами, где рекомендуется использовать JIT.

[Дополнительные сведения о брандмауэре Azure](https://docs.microsoft.com/azure/firewall/overview).


### <a name="single-click-remediation-to-boost-your-security-posture-preview"></a>Единое нажатие кнопки "исправление" для повышения безопасности (Предварительная версия)

Оценка безопасности —это средство, с помощью которого можно оценить состояние безопасности рабочей нагрузки. Он изучает рекомендации по безопасности и расставит приоритеты для вас, поэтому вы узнаете, какие рекомендации следует выполнить первыми. Это помогает найти наиболее серьезные уязвимости системы безопасности для определения приоритета расследования.

Чтобы упростить исправление ошибок настройки безопасности и помочь вам быстро повысить уровень безопасности, мы добавили новую возможность, которая позволяет исправлять рекомендацию для большого количества ресурсов одним щелчком мыши.

Эта операция позволяет выбрать ресурсы, к которым необходимо применить исправление, и запустить действие исправления, которое будет настраивать этот параметр от вашего имени.

Ознакомьтесь с рекомендациями по безопасности в разделе рекомендации по быстрому исправлению, которое включено в [справочном руководстве](recommendations-reference.md).


### <a name="cross-tenant-management"></a>Управление в нескольких клиентах

Центр безопасности теперь поддерживает сценарии управления между клиентами в составе Azure Лигхсаусе. Это позволяет получить видимость и управлять безопасностью нескольких клиентов в центре безопасности. 

[Дополнительные сведения о процессах управления между клиентами](security-center-cross-tenant-management.md).


## <a name="july-2019"></a>Июль 2019 г.

### <a name="updates-to-network-recommendations"></a>Обновления для сетевых рекомендаций

В центре безопасности Azure (ASC) были выпущены новые рекомендации по работе с сетью и улучшены некоторые из существующих. Теперь использование центра безопасности обеспечивает еще более надежную защиту ресурсов сети. 

[Дополнительные сведения о сетевых рекомендациях](recommendations-reference.md#recs-network).


## <a name="june-2019"></a>Июнь 2019 г.

### <a name="adaptive-network-hardening---generally-available"></a>Адаптивное усиление защиты сети — общедоступная версия

Одним из крупнейших областей для атаки рабочих нагрузок, работающих в общедоступном облаке, являются подключения к общедоступному Интернету и из него. Наши клиенты узнают, какие правила группы безопасности сети (NSG) должны быть установлены, чтобы убедиться, что рабочие нагрузки Azure доступны только для требуемых исходных диапазонов. С помощью этой функции центр безопасности узнает о шаблонах сетевого трафика и подключения рабочих нагрузок Azure и предоставляет рекомендации по правилам NSG для виртуальных машин, подключенных к Интернету. Это помогает нашим клиентам лучше настраивать политики доступа к сети и ограничивать их воздействие на атаки. 

Дополнительные [сведения о адаптивной усилении защиты сети](security-center-adaptive-network-hardening.md).
