---
title: Поддержка оценки VMware в службе "миграция Azure"
description: Узнайте о поддержке оценки виртуальных машин VMware с помощью Azure Migrate Server.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: a0d05c56670c54aca25232a86b5a0e89d2f0bcfd
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82983658"
---
# <a name="support-matrix-for-vmware-assessment"></a>Матрица поддержки для оценки VMware 

В этой статье перечислены требования к предварительным требованиям и поддержке при оценке виртуальных машин VMware для миграции в Azure с помощью средства " [Миграция Azure": Серверная Оценка](migrate-services-overview.md#azure-migrate-server-assessment-tool) . Если вы хотите перенести виртуальные машины VMware в Azure, изучите [таблицу поддержки миграции](migrate-support-matrix-vmware-migration.md).

Чтобы оценить виртуальные машины VMware, создайте проект службы "миграция Azure", а затем добавьте в проект средство оценки сервера. После добавления средства вы развертываете [устройство "миграция Azure](migrate-appliance.md)". Устройство постоянно обнаруживает локальные компьютеры и отправляет метаданные и данные о производительности компьютера в Azure. После завершения обнаружения вы собираете обнаруженные компьютеры в группы и выполняете оценку группы.

## <a name="limitations"></a>Ограничения

**Поддержка** | **Сведения**
--- | ---
**Ограничения проекта** | В подписке Azure можно создать несколько проектов.<br/><br/> Вы можете обнаружить и оценить до 35 000 виртуальных машин VMware в одном [проекте](migrate-support-matrix.md#azure-migrate-projects). Проект также может включать физические серверы и виртуальные машины Hyper-V вплоть до ограничений оценки для каждой из них.
**Обнаружение** | Устройство для миграции Azure может обнаруживать до 10 000 виртуальных машин VMware на vCenter Server.
**Оценка** | В одну группу можно добавить до 35 000 компьютеров.<br/><br/> Вы можете оценить до 35 000 виртуальных машин за одну оценку.

Дополнительные [сведения](concepts-assessment-calculation.md) об оценках.


## <a name="application-discovery"></a>Обнаружение приложений

Помимо обнаружения компьютеров, средство оценки серверов может обнаруживать приложения, роли и функции, работающие на компьютерах. Обнаружение инвентаризации приложений позволяет выявление и планирование пути миграции, предназначенного для локальных рабочих нагрузок. 

**Поддержка** | **Сведения**
--- | ---
**Поддерживаемые компьютеры** | В настоящее время Обнаружение приложений поддерживается только для виртуальных машин VMware.
**Обнаружение** | Обнаружение приложений — без агента. Он использует гостевые учетные данные компьютера и удаленный доступ к компьютерам с помощью WMI и вызовов SSH.
**Поддержка виртуальных машин** | Обнаружение приложений поддерживается для всех версий Windows и Linux.
**учетные данные vCenter** | Для обнаружения приложений требуется учетная запись vCenter Server с доступом только для чтения, а для виртуальных машин, > гостевых операций, доступны права.
**Учетные данные виртуальной машины** | В настоящее время Обнаружение приложений поддерживает использование одного учетного данных для всех серверов Windows и один учетные данные для всех серверов Linux.<br/><br/> Создайте учетную запись гостевого пользователя для виртуальных машин Windows, а также обычную или нормальную учетную запись пользователя (без доступа sudo) для всех виртуальных машин Linux.
**Средства VMware** | Средства VMware должны быть установлены и запускаться на виртуальных машинах, которые вы хотите обнаружить. <br/> Версия инструментов VMware должна быть более поздней, чем 10.2.0.
**PowerShell** | На виртуальных машинах должна быть установлена оболочка PowerShell версии 2,0 или более поздней.
**Доступ к портам** | На узлах ESXi, на которых нужно обнаружить виртуальные машины, устройство миграции Azure должно иметь возможность подключения к TCP-порту 443.
**Ограничения** | Для обнаружения приложений можно обнаружить до 10000 виртуальных машин на каждом устройстве миграции Azure.



## <a name="vmware-requirements"></a>Требования к VMware

**VMware** | **Сведения**
--- | ---
**Виртуальные машины VMware** | Оценка поддерживается для всех операционных систем Windows и Linux.
**vCenter Server** | Компьютеры, которые требуется обнаружить и оценивать, должны управляться с помощью vCenter Server версии 5,5, 6,0, 6,5 или 6,7.
**Разрешения (оценка)** | vCenter Server учетную запись только для чтения.
**Разрешения (Обнаружение приложений)** | Учетная запись vCenter Server с доступом только для чтения, а привилегии для **виртуальных машин > гостевых операций**.
**Разрешения (Визуализация зависимостей)** | Учетная запись vCenter Server с доступом только для чтения и привилегиями, включенными для**гостевых операций** **виртуальных машин** > .


## <a name="azure-migrate-appliance-requirements"></a>Требования к устройству Миграции Azure

Служба "миграция Azure" использует [устройство "миграция Azure](migrate-appliance.md) " для обнаружения и оценки. Вы можете развернуть устройство как виртуальную машину VMWare с помощью шаблона OVA, импортированного в vCenter Server или с помощью [скрипта PowerShell](deploy-appliance-script.md).

- Сведения о [требованиях к устройству](migrate-appliance.md#appliance---vmware) для VMware.
- Узнайте о URL-адресах, которые требуются устройству в [общедоступных](migrate-appliance.md#public-cloud-urls) и [правительственных](migrate-appliance.md#government-cloud-urls) облаках.
- В Azure для государственных организаций необходимо развернуть устройство [с помощью скрипта](deploy-appliance-script-government.md).


## <a name="port-access"></a>Доступ к портам

**Устройство** | **Соединен**
--- | ---
прибор. | Входящие подключения через TCP-порт 3389 для разрешения подключения удаленного рабочего стола к устройству.<br/><br/> Входящие подключения через порт 44368 для удаленного доступа к приложению управления устройством с помощью URL-адреса:```https://<appliance-ip-or-name>:44368``` <br/><br/>Исходящие подключения через порт 443 (HTTPS) для отправки метаданных обнаружения и производительности в службу "миграция Azure".
Сервер vCenter | Входящие подключения через TCP-порт 443, чтобы разрешить устройству получать метаданные конфигурации и производительности для оценки. <br/><br/> По умолчанию устройство подключается к vCenter через порт 443. Если сервер vCenter Server прослушивает другой порт, можно изменить порт при настройке обнаружения.
Узлы ESXi (Обнаружение приложений и анализ зависимостей без агента) | Если вы хотите выполнять [Обнаружение](how-to-discover-applications.md) приложений или [безагентный анализ зависимостей](concepts-dependency-visualization.md#agentless-analysis), устройство подключается к узлам ESXi через TCP-порт 443, чтобы обнаружить приложения и запустить визуализацию зависимостей без агента на виртуальных машинах.

## <a name="application-discovery"></a>Обнаружение приложений

Помимо обнаружения компьютеров, средство оценки серверов может обнаруживать приложения, роли и функции, работающие на компьютерах. Обнаружение инвентаризации приложений позволяет выявление и планирование пути миграции, предназначенного для локальных рабочих нагрузок. 

**Поддержка** | **Сведения**
--- | ---
**Поддерживаемые компьютеры** | В настоящее время Обнаружение приложений поддерживается только для виртуальных машин VMware.
**Обнаружение** | Обнаружение приложений — без агента. Он использует гостевые учетные данные компьютера и удаленный доступ к компьютерам с помощью WMI и вызовов SSH.
**Поддержка виртуальных машин** | Обнаружение приложений поддерживается для всех версий Windows и Linux.
**учетные данные vCenter** | Для обнаружения приложений требуется учетная запись vCenter Server с доступом только для чтения, а для виртуальных машин, > гостевых операций, доступны права.
**Учетные данные виртуальной машины** | В настоящее время Обнаружение приложений поддерживает использование одного учетного данных для всех серверов Windows и один учетные данные для всех серверов Linux.<br/><br/> Создайте учетную запись гостевого пользователя для виртуальных машин Windows, а также обычную или нормальную учетную запись пользователя (без доступа sudo) для всех виртуальных машин Linux.
**Средства VMware** | Средства VMware должны быть установлены и запускаться на виртуальных машинах, которые вы хотите обнаружить. <br/> Версия инструментов VMware должна быть более поздней, чем 10.2.0.
**PowerShell** | На виртуальных машинах должна быть установлена оболочка PowerShell версии 2,0 или более поздней.
**Доступ к портам** | На узлах ESXi, на которых нужно обнаружить виртуальные машины, устройство миграции Azure должно иметь возможность подключения к TCP-порту 443.
**Ограничения** | Для обнаружения приложений можно обнаружить до 10000 виртуальных машин на каждом устройстве миграции Azure.


## <a name="agentless-dependency-analysis-requirements"></a>Требования к анализу зависимостей без агента

[Анализ зависимостей](concepts-dependency-visualization.md) помогает определить зависимости между локальными компьютерами, которые вы хотите оценить и перенести в Azure. В таблице перечислены требования для настройки анализа зависимостей без агента. 

**Требование** | **Сведения**
--- | --- 
**Перед развертыванием** | У вас должен быть проект "миграция Azure" на месте с добавлением к проекту средства оценки сервера.<br/><br/>  Визуализация зависимостей развертывается после настройки устройства миграции Azure для обнаружения локальных компьютеров VMWare.<br/><br/> [Узнайте, как](create-manage-projects.md) создать проект в первый раз.<br/> [Узнайте, как](how-to-assess.md) добавить средство оценки в существующий проект.<br/> [Узнайте, как](how-to-set-up-appliance-vmware.md) настроить устройство "миграция Azure" для оценки виртуальных машин VMware.
**Поддержка виртуальных машин** | Сейчас поддерживается только для виртуальных машин VMware.
**Виртуальные машины Windows** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64-разрядная версия).
**Учетная запись Windows** |  Для анализа зависимостей устройству "миграция Azure" требуется учетная запись администратора домена или учетная запись локального администратора для доступа к виртуальным машинам Windows.
**Виртуальные машины Linux** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14,04, 16,04<br/> Debian 7, 8.<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.
**Учетная запись Linux** | Для анализа зависимостей на компьютерах Linux устройству "миграция Azure" требуется учетная запись пользователя с правами root.<br/><br/> Кроме того, учетной записи пользователя требуются следующие разрешения для файлов/бин/нетстат и/бин/ЛС: CAP_DAC_READ_SEARCH и CAP_SYS_PTRACE.
**Обязательные агенты** | На компьютерах, которые вы хотите проанализировать, не требуется агент.
**Средства VMware** | Средства VMware (более поздние версии, чем 10,2) должны быть установлены и запускаться на каждой виртуальной машине, которую необходимо проанализировать.
**учетные данные vCenter Server** | Для визуализации зависимостей требуется учетная запись vCenter Server с доступом только для чтения, а для виртуальных машин, > гостевых операций, доступны права. 
**PowerShell** | На виртуальных машинах должна быть установлена оболочка PowerShell версии 2,0 или более поздней.
**Доступ к портам** | На узлах ESXi, на которых выполняются анализируемые виртуальные машины, устройство миграции Azure должно иметь возможность подключения к TCP-порту 443.


## <a name="agent-based-dependency-analysis-requirements"></a>Требования к анализу зависимостей на основе агентов

[Анализ зависимостей](concepts-dependency-visualization.md) помогает определить зависимости между локальными компьютерами, которые вы хотите оценить и перенести в Azure. В таблице приведены требования к настройке анализа зависимостей на основе агентов. 

**Требование** | **Сведения** 
--- | --- 
**Перед развертыванием** | У вас должен быть проект "миграция Azure" на месте с помощью средства "миграция Azure", добавленного в проект.<br/><br/>  Вы развертываете визуализацию зависимостей после настройки устройства миграции Azure для обнаружения локальных компьютеров.<br/><br/> [Узнайте, как](create-manage-projects.md) создать проект в первый раз.<br/> [Узнайте, как](how-to-assess.md) добавить средство оценки в существующий проект.<br/> Узнайте, как настроить устройство "миграция Azure" для оценки [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md)или физических серверов.
**Azure для государственных организаций** | Визуализация зависимостей недоступна в Azure для государственных организаций.
**Log Analytics** | Служба "миграция Azure" использует решение [сопоставление служб](../operations-management-suite/operations-management-suite-service-map.md) в [журналах Azure Monitor](../log-analytics/log-analytics-overview.md) для визуализации зависимостей.<br/><br/> Вы связываете новую или существующую рабочую область Log Analytics с проектом службы "миграция Azure". Рабочую область для проекта службы "миграция Azure" нельзя изменить после добавления. <br/><br/> Рабочая область должна находиться в той же подписке, что и проект службы "миграция Azure".<br/><br/> Рабочая область должна находиться в регионах восточной части США, Юго-Восточной Азии или Западной Европы. Рабочие области в других регионах не могут быть связаны с проектом.<br/><br/> Рабочая область должна находиться в регионе, в котором [поддерживается сопоставление служб](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites).<br/><br/> В Log Analytics рабочей области, связанной с миграцией Azure, помечается ключ проекта миграции и имя проекта.
**Обязательные агенты** | На каждом компьютере, который необходимо проанализировать, установите следующие агенты:<br/><br/> [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).<br/> [Агент зависимостей](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Если локальные компьютеры не подключены к Интернету, необходимо скачать и установить шлюз Log Analytics.<br/><br/> Дополнительные сведения об установке [агента зависимостей](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) и [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Рабочая область Log Analytics** | Рабочая область должна находиться в той же подписке, что и проект службы "миграция Azure".<br/><br/> Служба "миграция Azure" поддерживает рабочие области в регионах "Восточная часть США", "Юго-Восточная Азия" и "Западная Европа".<br/><br/>  Рабочая область должна находиться в регионе, в котором [поддерживается сопоставление служб](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites).<br/><br/> Рабочую область для проекта службы "миграция Azure" нельзя изменить после добавления.
**Держек** | На Сопоставление служб решение не взимается плата за первые 180 дней (с дня, когда вы связываете рабочую область Log Analytics с проектом "миграция Azure")/<br/><br/> По истечении 180 дней применяются стандартные тарифы Log Analytics.<br/><br/> При использовании любого решения, кроме Сопоставление служб в связанной Log Analytics рабочей области, будет взиматься [стандартная плата](https://azure.microsoft.com/pricing/details/log-analytics/) за log Analytics.<br/><br/> При удалении проекта "Миграция Azure" рабочая область не будет удалена. После удаления проекта Сопоставление служб использование не освобождается, и каждый узел будет оплачиваться согласно платному уровню Log Analytics рабочей области/<br/><br/>Если у вас есть проекты, созданные до того, как Azure перейдет в общедоступную версию (от-28 февраля 2018 г.), может взиматься дополнительная плата за Сопоставление служб. Чтобы обеспечить оплату только через 180 дней, рекомендуется создать новый проект, так как существующие рабочие области будут по-прежнему взиматься.
**Управление** | При регистрации агентов в рабочей области вы используете идентификатор и ключ, предоставленные проектом "миграция Azure".<br/><br/> Рабочую область Log Analytics можно использовать за пределами службы "Миграция Azure".<br/><br/> При удалении связанного проекта службы "миграция Azure" Рабочая область не удаляется автоматически. [Удалите его вручную](../azure-monitor/platform/manage-access.md).<br/><br/> Не удаляйте рабочую область, созданную службой "миграция Azure", если вы не удалите проект службы "миграция Azure". В противном случае функция визуализации зависимостей не будет работать должным образом.
**Подключение к Интернету** | Если компьютеры не подключены к Интернету, необходимо установить на них шлюз Log Analytics.
**Azure для государственных организаций** | Анализ зависимостей на основе агента не поддерживается.


## <a name="next-steps"></a>Дальнейшие действия

- [Ознакомьтесь](best-practices-assessment.md) с рекомендациями по созданию оценок.
- [Подготовка к оценке VMware](tutorial-prepare-vmware.md) .
