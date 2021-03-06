---
title: Учебник по управлению конфигурацией виртуальных машин Windows в Azure
description: В этом учебнике описано, как выявлять пакетные обновления и управлять ими на виртуальных машинах Windows.
author: cynthn
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ed36dc669c8b89ba4a2b7831c6eb6f8742e73730
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82100419"
---
# <a name="tutorial-monitor-changes-and-update-a-windows-virtual-machine-in-azure"></a>Руководство по Мониторинг изменений и обновление виртуальных машин Windows в Azure

С помощью решений [Отслеживание изменений](../../automation/change-tracking.md) и [Управление обновлениями](../../automation/automation-update-management.md) Azure вы можете легко определять изменения в виртуальных машинах Windows в Azure и управлять обновлениями операционной системы для этих виртуальных машин.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * управление обновлениями Windows;
> * мониторинг изменений и инвентаризация.

## <a name="open-azure-cloud-shell"></a>Открытие Azure Cloud Shell

Azure Cloud Shell — это бесплатная интерактивная оболочка, с помощью которой можно выполнять действия, описанные в этой статье. Она включает предварительно установленные общие инструменты Azure и настроена для использования с вашей учетной записью Azure.

Чтобы открыть любой блок кода в Cloud Shell, просто выберите **Попробовать** в правом верхнем углу этого блока кода.

Кроме того, Cloud Shell можно открыть в отдельной вкладке браузера. Для этого перейдите на страницу [https://shell.azure.com/powershell](https://shell.azure.com/powershell). Выберите **Копировать**, чтобы скопировать блоки кода, вставьте их на вкладку Cloud Shell и нажмите клавишу ВВОД, чтобы запустить код.

## <a name="create-a-virtual-machine"></a>Создание виртуальной машины

Чтобы настроить мониторинг Azure и администрировать обновления для работы с этим руководством, вам понадобится виртуальная машина Windows в Azure.

Сначала укажите имя и пароль администратора для виртуальной машины с помощью командлета [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Теперь создайте виртуальную машину с помощью команды [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). В следующем примере создается виртуальная машина с именем `myVM` в расположении `East US`. Если они еще не существуют, создаются поддерживающие сетевые ресурсы и группа ресурсов `myResourceGroupMonitor`:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "East US" `
    -Credential $cred
```

Создание этих ресурсов и виртуальной машины может занять несколько минут.

## <a name="manage-windows-updates"></a>Управление обновлениями Windows

Решение "Управление обновлениями" помогает управлять обновлениями и исправлениями виртуальных машин Azure под управлением Windows. Непосредственно со своей виртуальной машины вы можете быстро:

- оценить состояние доступных обновлений;
- запланировать установку необходимых обновлений;
- просмотреть результаты развертывания, чтобы убедиться, что обновления были успешно применены к виртуальной машине.

Сведения о ценах см. на странице [цен на службу автоматизации](https://azure.microsoft.com/pricing/details/automation/).

### <a name="enable-update-management"></a>Включение управления обновлениями

Чтобы включить решение "Управление обновлениями" для виртуальной машины, сделайте следующее:

1. В левой части окна выберите **Виртуальные машины**.
1. Выберите виртуальную машину из списка.
1. В области **Операции** окна виртуальной машины выберите **Управление обновлениями**.
1. Откроется окно **Включение управления обновлениями**.

Выполняется проверка, чтобы определить, включено ли решение "Управление обновлениями" для этой виртуальной машины. При этом проверяется рабочая область Log Analytics и связанная учетная запись службы автоматизации, а также наличие решения в рабочей области.

Рабочая область [Log Analytics](../../log-analytics/log-analytics-overview.md) используется для сбора данных, созданных компонентами и службами, такими как "Управление обновлениями". Рабочая область предоставляет единое расположение для проверки и анализа данных из нескольких источников.

Для выполнения дополнительных действий на виртуальных машинах, требующих обновления, можно использовать службу автоматизации Azure, чтобы включить на этих машинах модули runbook. Это могут быть такие действия, как скачивание или применение обновлений.

В процессе проверки также определяется, была ли виртуальная машина подготовлена с помощью Microsoft Monitoring Agent (MMA) и гибридной рабочей роли Runbook службы автоматизации. Агент используется для взаимодействия с виртуальной машиной и получения сведений о состоянии обновления.

В окне **Включение управления обновлениями** выберите рабочую область Log Analytics и учетную запись службы автоматизации, а затем щелкните **Включить**. Процесс включения решения может занять до 15 минут.

Если во время подключения отсутствуют какие-либо из следующих компонентов, они будут автоматически добавлены.

* [Рабочая область Log Analytics](../../log-analytics/log-analytics-overview.md).
* [Служба автоматизации](../../automation/automation-offering-get-started.md)
* [Гибридная рабочая роль runbook](../../automation/automation-hybrid-runbook-worker.md), которая включена на виртуальной машине.

После включения решения открывается окно **Управление обновлениями**. Настройте расположение, рабочую область Log Analytics и учетную запись службы автоматизации, а затем щелкните **Включить**. Если эти параметры отключены, для этой виртуальной машины настроено другое решение автоматизации. Это значит, что необходимо использовать рабочую область решения и учетную запись службы автоматизации.

![Включение решения по управлению обновлениями](./media/tutorial-monitoring/manageupdates-update-enable.png)

Процесс включения решения "Управление обновлениями" может занять до 15 минут. В течение этого времени не закрывайте окно браузера. После того как решение включено, сведения об отсутствующих обновлениях на виртуальной машине передаются в журналы Azure Monitor. На получение данных для анализа может уйти от 30 минут до 6 часов.

### <a name="view-an-update-assessment"></a>Просмотр оценки обновлений

После включения решения "Управление обновлениями" отобразится окно **Управление обновлениями**. После оценки обновлений вы увидите список отсутствующих обновлений на вкладке **Отсутствующие обновления**.

 ![Просмотр состояния обновления](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Планирование развертывания обновлений

Чтобы установить обновления, составьте план развертывания в рамках графика выпуска и периода обслуживания. Вы выбираете типы обновлений, которые будут включены в развертывание. Например, можно включить только критические обновления или обновления для системы безопасности и исключить накопительные пакеты обновления.

Чтобы запланировать новое развертывание обновления для виртуальной машины, щелкните **Запланировать развертывание обновлений** в верхней части окна **Управление обновлениями**. В окне **Новое развертывание обновления** укажите следующие сведения:

| Параметр | Описание |
| --- | --- |
| **имя**; |Введите уникальное имя для идентификации развертывания обновлений. |
|**Операционная система**| Выберите **Linux** или **Windows**.|
| **Группы для обновления** |Для виртуальных машин, размещенных в Azure, определите запрос на основе подписки, групп ресурсов, расположений и тегов. Этот запрос создает динамическую группу виртуальных машин, размещенных в Azure, которые будут добавлены в развертывание. </br></br>Для виртуальных машин, не размещенных в Azure, выберите имеющиеся сохраненные поисковые запросы. С помощью этих запросов вы можете выбрать группу этих виртуальных машин для включения в развертывание. </br></br> Дополнительные сведения см. в статье [Использование динамических групп с Управлением обновлениями](../../automation/automation-update-management-groups.md).|
| **Обновляемые компьютеры** |Выберите **Сохраненные условия поиска**, **Imported group** (Импортированная группа) или **Компьютеры**.<br/><br/>Если вы выбираете **Компьютеры**, вы можете выбрать отдельные компьютеры из раскрывающегося списка. Сведения о готовности каждого компьютера отображаются в столбце **Готовность к обновлению агента** таблицы.</br></br> Дополнительные сведения о различных способах создания групп компьютеров в журналах Azure Monitor см. в статье [Группы компьютеров в запросах к журналам Azure Monitor](../../azure-monitor/platform/computer-groups.md). |
|**Классификации обновлений**|Выберите все необходимые классификации обновлений.|
|**Включение или исключение обновлений**|Выберите этот параметр, чтобы открыть область **Включить или исключить**. Обновления, которые можно включить или исключить, находятся на отдельных вкладках. Дополнительные сведения об обработке включений см. в разделе [Schedule an update deployment](../../automation/automation-tutorial-update-management.md#schedule-an-update-deployment) (Планирование развертывания обновлений). |
|**Параметры расписания**|Выберите время запуска, а затем — **Однократно** или **Повторение**.|
| **Сценарии предварительного и последующего выполнения**|Выберите сценарии, которые будут выполняться до и после развертывания.|
| **Период обслуживания** | Введите количество минут для установки обновлений. Допустимые значения: от 30 до 360 минут. |
| **Reboot control** (Управление перезагрузкой)| Выберите способ обработки перезагрузок. Доступные варианты:<ul><li>**Перезагрузка при необходимости**;</li><li>**Всегда перезагружать**;</li><li>**Никогда не перезагружать**;</li><li>**Only reboot** (Только перезагрузка).</li></ul>По умолчанию выбран вариант **Перезагрузка при необходимости**. Если вы выберете **Only reboot** (Только перезагрузка), обновления не будут установлены.|

Завершив настройку расписания, щелкните **Создать**, чтобы вернуться на панель мониторинга состояния. В таблице **Запланированные** отобразится созданное расписание развертываний.

Развертывания обновлений также можно создать программно. Чтобы узнать, как создать развертывание обновлений с помощью REST API, ознакомьтесь со статьей [Software Update Configurations — Create](/rest/api/automation/softwareupdateconfigurations/create) (Конфигурации обновления программного обеспечения. Создание). Кроме того, имеется пример модуля runbook, который можно использовать для создания развертывания еженедельного обновления. Дополнительные сведения об этом модуле runbook см. в статье [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Создание развертывания еженедельного обновления для одной или нескольких виртуальных машин в группе ресурсов).

### <a name="view-results-of-an-update-deployment"></a>Просмотр результатов развертывания обновлений

После запуска запланированного развертывания вы можете просмотреть его состояние на вкладке **Развертывания обновлений** окна **Управление обновлениями**.

Если развертывание выполняется в этот момент, отображается состояние "Выполняется". После успешного выполнения состояние меняется на "Успешно". Однако в случае сбоя какого-то из обновлений при развертывании для состояния отобразится значение Partially failed (Частичный сбой).

Выберите завершенное развертывание обновлений, чтобы просмотреть панель мониторинга для этого развертывания.

![Панель мониторинга состояния развертывания обновлений конкретного развертывания](./media/tutorial-monitoring/manageupdates-view-results.png)

Плитка **Update results** (Результаты обновления) является сводкой общего количества обновлений и результатов развертывания на виртуальной машине. В таблице справа представлена подробная информация о каждом обновлении и результатах установки. Для каждого результата необходимо установить одно из следующих значений.

* **Попытка не предпринималась**. Обновление не установлено. Недостаточно времени на основе определенной длительности окна обслуживания.
* **Выполнено**. Обновление выполнено.
* **Ошибка**. Обновление не выполнено.

Чтобы просмотреть все записи журнала, созданные при этом развертывании, щелкните **Все журналы**.

Щелкните плитку **Вывод**, чтобы просмотреть поток заданий для модуля runbook, который управляет развертыванием обновления на целевой виртуальной машине.

Чтобы просмотреть подробные сведения об ошибках развертывания, выберите **Ошибки**.

## <a name="monitor-changes-and-inventory"></a>Мониторинг изменений и инвентаризация

Вы можете собирать и просматривать сведения об установленных на компьютерах программном обеспечении, файлах, управляющих программах Linux, службах и разделах реестра Windows. Отслеживание конфигураций поможет вам локализовать операционные проблемы в любом сегменте среды и получить сведения о текущем состоянии компьютеров.

### <a name="enable-change-and-inventory-management"></a>Включение управления изменениями и инвентаризацией

Чтобы включить управление изменениями и инвентаризацией для виртуальной машины, сделайте следующее:

1. В левой части окна выберите **Виртуальные машины**.
1. Выберите виртуальную машину из списка.
1. В разделе **Операции** в окне виртуальной машины выберите **Инвентаризация** или **Отслеживание изменений**.
1. Откроется область **Включение отслеживания изменений и инвентаризации**.

Настройте расположение, рабочую область Log Analytics и учетную запись службы автоматизации, а затем щелкните **Включить**. Если параметры являются отключенными, решение для автоматизации уже включено для виртуальной машины. В этом случае необходимо использовать уже включенную рабочую область и учетную запись автоматизации.

Эти решения представлены в меню отдельно, но по сути являются одним решением. Включение любого из них активирует для виртуальной машины оба решения.

![Включение отслеживания для изменений и инвентаризации](./media/tutorial-monitoring/manage-inventory-enable.png)

После включения решения потребуется некоторое время, пока будут собраны и отображены данные инвентаризации для виртуальной машины.

### <a name="track-changes"></a>Отслеживание изменений

На виртуальной машине в разделе **ОПЕРАЦИИ** выберите **Отслеживание изменений**, а затем щелкните **Изменить параметры**. Откроется область **Отслеживание изменений**. Выберите тип настроек, которые вы намерены отслеживать, и щелкните **+ Добавить** для настройки параметров.

Доступные параметры для Windows:

* реестр Windows;
* файлы Windows.

Подробные сведения об Отслеживании изменений см. в статье [Устранение неполадок при изменениях в среде](../../automation/automation-tutorial-troubleshoot-changes.md).

### <a name="view-inventory"></a>Просмотр данных инвентаризации

На виртуальной машине выберите **Инвентаризация** в разделе **Операции**. На вкладке **Программное обеспечение** вы найдете таблицу с обнаруженным программным обеспечением. В таблице отображаются обобщенные сведения о каждой строке с информацией о программном обеспечении. Здесь для программного обеспечения указываются: имя, версия, издатель, время последнего обновления.

![Просмотр данных инвентаризации](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Мониторинг журналов действий и изменений

На странице **Отслеживание изменений** на виртуальной машине выберите **Управление подключением журнала действий**, чтобы открыть область **Журнал действий Azure**. Выберите **Подключить**, чтобы подключить журнал действий Azure для виртуальной машины к службе "Отслеживание изменений".

Включив параметр "Отслеживание изменений", перейдите к области **Обзор** для виртуальной машины и выберите действие **Остановить**, чтобы остановить виртуальную машину. Подтвердите остановку, выбрав **Да** при появлении соответствующего запроса. После освобождения виртуальной машины выберите **Запустить**, чтобы снова запустить ее.

Остановка и повторный запуск виртуальной машины регистрируются как события в журнале действий. Вернитесь к области **Отслеживание изменений** и выберите вкладку **События** в нижней части области. Через некоторое время события появятся на диаграмме и в таблице. Вы можете выбрать каждое событие, чтобы просмотреть подробные сведения о нем.

![Просмотр изменений в журнале действий](./media/tutorial-monitoring/manage-activitylog-view-results.png)

На предыдущей диаграмме отображаются изменения, произошедшие за определенное время. После добавления подключения к журналу действий Azure график в верхней части отобразит события из журнала действий Azure.

Каждая строка гистограммы соответствует определенному типу отслеживаемых изменений. К этим типам относятся управляющие программы Linux, файлы, разделы реестра Windows, программное обеспечение и службы Windows. На вкладке **Изменение** отображаются сведения об изменениях. Изменения отображаются в том порядке, в котором они произошли, причем самое последнее изменение отображается первым.

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике вы настроили решения "Отслеживание изменений" и "Управление обновлениями" на виртуальной машине и проверили их. Вы ознакомились с выполнением следующих задач:

> [!div class="checklist"]
> * создание группы ресурсов и виртуальной машины;
> * управление обновлениями Windows;
> * мониторинг изменений и инвентаризация.

Перейдите к следующему учебнику, чтобы узнать о мониторинге виртуальных машин.

> [!div class="nextstepaction"]
> [Мониторинг виртуальных машин](tutorial-monitor.md)
