---
title: Сведения о скидках на зарезервированные экземпляры Выделенного узла Azure
description: Узнайте, как скидка на зарезервированные экземпляры применяется к Выделенному узлу Azure.
author: yashesvi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: banders
ms.openlocfilehash: a39cd7aa2c15fedeaf69408d8c8ae8c6b0848fed
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "80677389"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-azure-dedicated-hosts"></a>Как скидка на резервирование применяется к Выделенному узлу Azure

После того, как вы купили зарезервированный экземпляр Выделенного узла Azure, скидка резервирования автоматически применяется к выделенным узлам, атрибуты и количество которых совпадают с резервированием. Резервирование покрывает затраты стоимости выделенных узлов.

## <a name="how-reservation-discount-is-applied"></a>Применение скидки на резервирование

Скидка на резервирование *сгорает, если не используется*. Поэтому, если у вас не существует подходящих ресурсов в течение некоторого часа, вы потеряете количество ресурсов, зарезервированных на этот час. Вы не можете переносить неиспользуемые зарезервированные часы.

При удалении выделенного узла скидка на резервирование автоматически применяется к другому подходящему ресурсу в указанной области резервирования. Если в указанной области не найдены подходящие ресурсы, зарезервированные часы *теряются*.

## <a name="reservation-discount-for-dedicated-hosts"></a>Скидка на резервирование для выделенных узлов

На зарезервированные экземпляры Выделенного узла Azure предоставляется скидка в размере стоимости вычислительной инфраструктуры, используемой для выделенных узлов. Скидка применяется к выделенным узлам независимо от того, используют их виртуальные машины или нет. Резервирование не охватывает дополнительные затраты, такие как лицензирование, использование сети или потребление хранилища виртуальными машинами, развернутыми на выделенном узле.

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами

Если у вас есть вопросы или вам нужна помощь,  [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о резервировании в Azure см. по следующим ссылкам:

- [Сведения о резервированиях для Azure](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)

- [Использование выделенных узлов](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)

- [Цены на службу "Выделенный узел Azure"](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Управление резервированиями для Azure](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)

- [Общие сведения об использовании резервирования Azure для подписки с оплатой по мере использования](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)

- [Общие сведения об использовании зарезервированных экземпляров Azure с Соглашением о регистрации Enterprise](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)

- [Продажа Microsoft Azure Reserved VM Instances](https://docs.microsoft.com/partner-center/azure-reservations)

