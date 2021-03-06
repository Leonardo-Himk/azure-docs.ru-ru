---
title: Управление инцидентами безопасности в центре безопасности Azure | Документация Майкрософт
description: Этот документ поможет вам использовать центр безопасности Azure для управления инцидентами безопасности.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: 98fc339e473ffb2bf54e7119634e93046cca1ef3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79415665"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Управление инцидентами безопасности в центре безопасности Azure

Рассмотрение и исследование оповещений системы безопасности может занимать много времени даже для самых опытных аналитиков безопасности, и для многих из них трудно даже понять, с чего начать. Используя [аналитику](security-center-detection-capabilities.md) для связывания данных из разных [оповещений системы безопасности](security-center-managing-and-responding-alerts.md), центр безопасности может предоставить общую картину кампании атак и все соответствующие оповещения. Это позволит вам быстро понять, какие действия выполнил злоумышленник и какие ресурсы затронуты.

В этом разделе содержатся сведения о инцидентах в центре безопасности, а также об исправлении их оповещений.

## <a name="what-is-a-security-incident"></a>Что такое инцидент?

В центре безопасности инцидентом считается совокупность всех оповещений для ресурса, которые соответствуют схемам [этапов нарушения безопасности](alerts-reference.md#intentions) . Инциденты отображаются в списке [оповещения системы безопасности](security-center-managing-and-responding-alerts.md) . Щелкните инцидент, чтобы просмотреть связанные с ним предупреждения, которые позволяют получить дополнительные сведения о каждом вхождении.

## <a name="managing-security-incidents"></a>Управление инцидентами

1. На панели мониторинга центра безопасности щелкните плитку **оповещения системы безопасности** . Будут перечислены инциденты и оповещения. Обратите внимание, что в описании инцидентов используется не такой значок, как в других оповещениях.

    ![Просмотр инцидентов безопасности](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Чтобы просмотреть сведения, щелкните инцидент. В колонке **обнаруженные инциденты безопасности** отображаются дополнительные сведения. В разделе **Общие сведения** можно получить представление о том, что активировало оповещение системы безопасности. Он отображает такие сведения, как целевой ресурс, исходный IP-адрес (если применимо), если оповещение по-прежнему активно, и рекомендации по исправлению.  

    ![Реагирование на инциденты безопасности в центре безопасности Azure](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. Чтобы получить дополнительные сведения о каждом оповещении, щелкните оповещение. Исправления, предлагаемые центром безопасности Azure, зависят от оповещения системы безопасности.

   > [!NOTE]
   > Одно и то же предупреждение может существовать как часть инцидента, а также быть видимым как отдельное оповещение.

    ![Сведения об оповещении](./media/security-center-incident/security-center-incident-alert.png)

1. Выполните действия по исправлению, заданные для каждого оповещения.


## <a name="see-also"></a>См. также
Этот документ содержит подробные сведения о функции инцидентов в центре безопасности Azure. Дополнительные сведения см. в следующих разделах:

* [Защита от угроз с помощью Центра безопасности Azure](threat-protection.md)
* [Оповещения безопасности в Центре безопасности Azure](security-center-alerts-overview.md)
* [Управление оповещениями системы безопасности](security-center-managing-and-responding-alerts.md)