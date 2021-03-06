---
title: Перенос развертываний обновлений OMS в Azure
description: В этой статье описывается, как перенести существующие развертывания обновлений OMS в Azure
services: automation
ms.subservice: update-management
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 910f284eedbf50be5b58b6c18f02e50adda35e9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81680004"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>Перенос развертываний обновлений OMS в Azure

Портал Operations Management Suite (OMS) объявлен [устаревшим](../azure-monitor/platform/oms-portal-transition.md). Все функции, доступные на портале OMS для Управление обновлениями, доступны в портал Azure с помощью журналов Azure Monitor. В этой статье содержатся сведения, необходимые для перехода на портал Azure.

## <a name="key-information"></a>Основные сведения

* Это не повлияет на работоспособность имеющихся развертываний. После воссоздания развертывания в Azure, старую версию можно удалить из OMS.
* Все существующие компоненты OMS доступны в Azure. Для дополнительных сведений об управлении обновлениями см. [Обзор управления обновлениями](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Доступ к порталу Azure

В рабочей области OMS щелкните **открыть в Azure**. Этот выбор переходит к рабочей области Log Analytics, которую использует OMS.

![Открыть в Azure — портал OMS](media/migrate-oms-update-deployments/link-to-azure-portal.png)

На портале Azure щелкните **учетную запись службы автоматизации**

![Журналы Azure Monitor](media/migrate-oms-update-deployments/log-analytics.png)

В учетной записи службы автоматизации щелкните **Управление обновлениями**.

![Управление обновлениями](media/migrate-oms-update-deployments/azure-automation.png)

В портал Azure выберите **учетные записи службы автоматизации** в разделе **все службы**. 

В разделе **средства управления**выберите нужную учетную запись службы автоматизации и нажмите кнопку **Управление обновлениями**.

## <a name="recreate-existing-deployments"></a>Воссоздание существующих развертываний

У всех развертываний обновлений, созданных на портале OMS есть [сохраненный поиск](../azure-monitor/platform/computer-groups.md), известный как группа компьютеров, с тем же именем что и существующее развертывание обновлений. Сохраненный поиск содержит список компьютеров, запланированных в развертывании обновлений.

![Управление обновлениями](media/migrate-oms-update-deployments/oms-deployment.png)

Для использования существующего сохраненного поиска, выполните перечисленные ниже действия.

Для создания нового развертывания обновления, перейдите на портал Azure, выберите используемую учетную запись службы автоматизации, и щелкните **Управление обновлениями**. Щелкните **Запланировать развертывание обновлений**.

![Запланировать развертывание обновлений](media/migrate-oms-update-deployments/schedule-update-deployment.png)

Откроется панель Новое развертывание обновления. Введите значения для свойств, описанных в следующей таблице и нажмите **Создать**:

Для **обновляемых компьютеров**выберите сохраненный поиск, используемый существующим развертыванием OMS.

| Свойство | Описание |
| --- | --- |
|Имя |Уникальное имя для идентификации развертывания обновлений. |
|Операционная система| Выберите **Linux** или **Windows**.|
|Компьютеры, на которые нужно установить обновления |Щелкните "Сохраненные условия поиска", "Импортировать группу" или выберите "Компьютер" в раскрывающемся списке и выберите нужные компьютеры. Если выберете **Компьютеры**, готовность к обновлению будет показана в столбце **Готовность к обновлению агента**.</br> Дополнительные сведения о различных способах создания групп компьютеров в журналах Azure Monitor см. в разделе [группы компьютеров в журналах Azure Monitor](../azure-monitor/platform/computer-groups.md) . |
|Классификации обновлений|Выберите все необходимые категории обновления. CentOS не поддерживает это без дополнительной настройки.|
|Исключаемые обновления|Введите обновления для исключения. Для Windows введите номер статьи базы знаний без префикса **KB**. Для Linux введите имя пакета или используйте подстановочный знак.  |
|Параметры расписания|Выберите время запуска, а затем — **однократное** применение или **повторяющееся**. | 
| Период обслуживания |Количество минут, установленное для обновлений. Значение должно составлять не менее 30 минут, но не более 6 часов. |
| Перезагрузка элемента управления| Определяет, как следует выполнять перезагрузку.</br>Доступные параметры:</br>Перезагрузка при необходимости (по умолчанию)</br>Всегда выполнять перезагрузку</br>Никогда не перезагружать</br>Только перезагрузка без установки обновлений.|

Нажмите **Запланированные развертывания обновлений**, чтобы просмотреть статус новосозданного развертывания обновлений.

![Развертывание нового обновления](media/migrate-oms-update-deployments/new-update-deployment.png)

Как уже упоминалось ранее, после того как вы настроили через портал Azure новые развертывания, можете удалить существующие развертывания с портала OMS.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о Управление обновлениями в Azure см. в разделе [Управление обновлениями](automation-update-management.md).
