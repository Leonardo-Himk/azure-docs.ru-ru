---
title: Рекомендации по публикации шаблонов решений для приложений Azure в Azure Marketplace
description: В этой статье описываются требования к публикации шаблонов решений в Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: f62b3478c5c711423913b5918886b43b79ac691d
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858327"
---
# <a name="publishing-guide-for-azure-applications-solution-template-offers"></a>Рекомендации по публикации для предложений шаблонов решений для приложений Azure

В этой статье описываются требования к публикации предложений шаблонов решений, которые являются одним из способов публикации предложений приложений Azure в Azure Marketplace. Для типа предложения шаблона решения требуется [шаблон Azure Resource Manager (шаблон ARM)](../azure-resource-manager/templates/overview.md) для автоматического развертывания инфраструктуры решения.

Используйте *шаблон решения* для приложений Azure в следующих случаях:

- Для решения требуется дополнительная автоматизация развертывания и настройки за пределами одной виртуальной машины (ВМ), например сочетание виртуальных машин, сети и ресурсов хранилища.
- Ваши клиенты будут управлять самим решением.

Обращение к действию, которое клиент видит для этого типа предложения, — *получить его сейчас*.

## <a name="requirements-for-solution-template-offers"></a>Требования к предложениям шаблонов решений

| **Требования** | **Сведения**  |
| ---------------  | -----------  |
|Выставление счетов и ценообразование    |  Предложения шаблонов решений не являются предложениями по транзакциям, но их можно использовать для развертывания платных предложений виртуальных машин, которые выставляются через коммерческий магазин Майкрософт. Ресурсы, развертываемые с помощью шаблона ARM решения, настраиваются в подписке Azure клиента. Виртуальные машины с оплатой по мере использования преобразуются в транзакции с клиентом через корпорацию Майкрософт и выставляются через подписку Azure клиента.<br/> Для выставления счетов за использование собственной лицензии (BYOL), несмотря на то, что стоимость инфраструктуры выставляется по подписке клиента, вы самостоятельно проставляете платежи по лицензированию программного обеспечения непосредственно клиенту.   |
|Совместимый с Azure виртуальный жесткий диск (VHD)  |   Виртуальные машины должны быть созданы на платформе Windows или Linux. Дополнительные сведения можно найти в разделе  <ul> <li>[Создайте предложение приложения Azure](./partner-center-portal/create-new-azure-apps-offer.md) (для виртуальных жестких дисков Windows).</li><li>[Дистрибутивы Linux, одобренные в Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) (для виртуальных жестких дисков Linux).</li></ul> |
| Определение потребления услуг клиентами | Для всех шаблонов решений, опубликованных в Azure Marketplace, требуется включить атрибуты использования клиента. Дополнительные сведения о соотношении использования клиентов и о том, как ее включить, см. в статье о соотношении [использования клиентов в Azure](./azure-partner-customer-usage-attribution.md).  |
| Использование управляемых дисков | По умолчанию для материализованных виртуальных машин "инфраструктура как услуга" (IaaS) в Azure используется параметр " [управляемые диски](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) ". В шаблонах решений необходимо использовать управляемые диски. <ul><li>Чтобы обновить шаблоны решений, следуйте указаниям в статье [Использование управляемых дисков в Azure Resource Manager шаблонах](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments)и используйте предоставленные [образцы](https://github.com/Azure/azure-quickstart-templates).<br><br> </li><li>Чтобы опубликовать VHD как образ в Azure Marketplace, импортируйте базовый виртуальный жесткий диск управляемых дисков в учетную запись хранения, используя один из следующих методов.<ul><li>[Azure PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [Интерфейс командной строки Azure](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul></ul> |

## <a name="next-steps"></a>Дальнейшие шаги

Если вы еще не сделали этого, Узнайте, как [расширить свой облачный бизнес с помощью Azure Marketplace](https://azuremarketplace.microsoft.com/sell).

Чтобы зарегистрироваться и начать работу в центре партнеров, выполните следующие действия.

- [Войдите в центр партнеров](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) , чтобы создать или завершить свое предложение.
- Дополнительные сведения см. [в разделе Создание предложения приложения Azure](./partner-center-portal/create-new-azure-apps-offer.md) .
