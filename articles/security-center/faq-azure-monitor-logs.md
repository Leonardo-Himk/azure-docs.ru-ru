---
title: Вопросы и ответы об имеющихся агентах Log Analytics в центре безопасности Azure
description: Это часто задаваемые вопросы о клиентах, которые уже используют агент Log Analytics и рассмотрены центр безопасности Azure, продукт, который помогает предотвращать, обнаруживать угрозы и реагировать на них.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 2fe306cf7d17f0789c5e134c3fcad3f8f07a0b80
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612832"
---
# <a name="faq-for-customers-already-using-azure-monitor-logs"></a>Вопросы и ответы для клиентов, уже использующих журналы Azure Monitor<a name="existingloganalyticscust"></a>

## <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Переопределяет ли центр безопасности все установленные подключения между виртуальными машинами и рабочими областями?

Если на виртуальной машине уже установлен агент Log Analytics в качестве расширения Azure, центр безопасности не переопределит существующее подключение к рабочей области. Он использует существующую рабочую область. Виртуальная машина будет защищена при условии, что решение "безопасность" или "SecurityCenterFree" установлено в рабочей области, на которую он сообщает. 

Решение центра безопасности устанавливается в рабочей области, выбранной на экране сбора данных, если она еще не существует, а решение применяется только к соответствующим виртуальным машинам. Когда вы добавляете решение, оно по умолчанию автоматически развертывается на всех агентах Windows и Linux, подключенных к рабочей области Log Analytics. Функция [нацеливания решений](../operations-management-suite/operations-management-suite-solution-targeting.md) позволяет применять к решениям область действия.

> [!TIP]
> Если агент Log Analytics устанавливается непосредственно на виртуальной машине (а не в виде расширения Azure), центр безопасности не устанавливает агент Log Analytics, а мониторинг безопасности ограничен.

## <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Устанавливает ли центр безопасности решения в моих существующих рабочих областях Log Analytics? Как это влияет на выставление счетов?
Центр безопасности определяет, что виртуальная машина уже подключена к рабочей области, которую вы создали. Затем центр безопасности включает решения в этой рабочей области в соответствии с ценовой категорией. Решения применяются только к релевантным виртуальным машинам Azure с помощью функции [нацеливания решений](../operations-management-suite/operations-management-suite-solution-targeting.md). Поэтому счета выставляются без изменений.

- **Уровень "Бесплатный"**  — центр безопасности устанавливает в рабочей области решение SecurityCenterFree. Плата за бесплатный уровень не взимается.
- **Уровень "Стандартный"** — центр безопасности устанавливает в рабочей области решение Security.

   ![Решения в рабочей области по умолчанию](./media/security-center-platform-migration-faq/solutions.png)

## <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>В моей среде уже есть рабочие области. Можно ли использовать их для сбора данных безопасности?
Если на виртуальной машине уже установлен агент Log Analytics в качестве расширения Azure, центр безопасности использует имеющуюся подключенную рабочую область. Центр безопасности устанавливается в рабочей области (если он еще не установлен), и решение применяется только к релевантным виртуальным машинам при помощи функции [нацеливания решений](../operations-management-suite/operations-management-suite-solution-targeting.md).

Когда центр безопасности устанавливает агент Log Analytics на виртуальных машинах, он использует рабочие области по умолчанию, созданные центром безопасности, если центр безопасности не указывает на существующую рабочую область.

## <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>В моих рабочих областях уже есть решение для обеспечения безопасности. Как это влияет на выставление счетов?
Решение аудита & безопасности используется для включения функций уровня "Стандартный" центра безопасности для виртуальных машин Azure. Если решение "Безопасность и аудит" уже установлено в рабочей области, центр безопасности использует существующее решение. Счета будут выставляться без изменений.
