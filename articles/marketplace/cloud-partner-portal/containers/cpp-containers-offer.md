---
title: Предложение образа контейнеров Azure | Azure Marketplace
description: Общие сведения о процессе публикации предложения контейнера в Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: dsindona
ms.openlocfilehash: 81908de5c2fb3960684ed6cf37952e8815d8a5d5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82148298"
---
# <a name="containers"></a>Контейнеры

> [!IMPORTANT]
> Начиная с 13 апреля 2020 г. Мы начинаем перемещаться по управлению предложениями контейнеров Azure в центр партнеров. После миграции вы создадите предложения в центре партнеров и будете управлять ими. Следуйте инструкциям в разделе [Создание предложения контейнера Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer) для управления перенесенными предложениями.

<table> <tr> <td>В этом разделе объясняется, как опубликовать образ контейнера в <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a>.  
Тип предложения контейнера поддерживает образы контейнера Docker, подготовленные в качестве экземпляров <a href="https://docs.microsoft.com/azure/aks/index">Azure Kubernetes Service</a> или <a href="https://docs.microsoft.com/azure/container-instances/container-instances-overview">экземпляров контейнера Azure</a> и размещенные в репозитории <a href="https://docs.microsoft.com/azure/container-registry">Azure Container Registry</a>. </td> <td><img src="./media/container-icon.png"  alt="Azure container icon" /></td> </tr> </table>

## <a name="offer-components"></a>Компоненты предложения

В этом разделе описываются элементы процесса публикации контейнера. Он является руководством для издателя Azure Marketplace. Публикация состоит из следующих основных частей:

- [Предварительные требования.](./cpp-prerequisites.md) Перечислены технические и бизнес-требования, которые должны быть выполнены перед созданием или публикацией предложения контейнера.
- [Создание предложения.](./cpp-create-offer.md) Перечислены шаги по созданию записи предложения контейнера с использованием Портала Cloud Partner.
- [Подготовка технических средств.](./cpp-create-technical-assets.md) Создание технических средств для решения контейнера в качестве предложения в Azure Marketplace.
- [Публикация предложения контейнера.](./cpp-publish-offer.md) Отправка предложения для публикации в Azure Marketplace.

## <a name="container-publishing-process"></a>Процесс публикации контейнера

На следующей схеме показаны основные действия публикации предложения виртуальной машины.
![Шаги публикации предложения](./media/containers-offer-process.png)

Ниже перечислены основные этапы для публикации предложения контейнера.

1. Создание предложения. Укажите подробные сведения о предложении. К этим сведениям относятся: описание предложения, маркетинговые материалы, сведения о поддержке и характеристики ресурса.
2. Создание бизнес- и технических ресурсов. Создайте бизнес-ресурсы (юридические документы и маркетинговые материалы) и технические ресурсы для связанного решения (образы контейнеров, размещенные в Реестре контейнеров Azure).
3. Создание номера SKU. Создайте номера SKU, связанные с предложением. Уникальный номер SKU нужен для каждого публикуемого образа.
4. Сертификация и публикация предложения. Оформив предложение и выполнив технические требования, вы можете отправить предложение на публикацию. В ходе отправки начинается процесс публикации. Во время этого процесса решение тестируется, проверяется, сертифицируется и активируется в Azure Marketplace.

## <a name="next-steps"></a>Дальнейшие шаги

Прежде чем следовать этим действиям, необходимо выполнить [технические и бизнес-требования](./cpp-prerequisites.md) для публикации контейнера в Microsoft Azure Marketplace.
