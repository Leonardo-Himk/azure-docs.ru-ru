---
title: Настройка предпочтительного варианта сетевой маршрутизации (предварительная версия)
titleSuffix: Azure Storage
description: Настройте предпочтительный вариант сетевой маршрутизации (предварительная версия) для учетной записи хранения Azure, чтобы определить способ передачи сетевого трафика в учетную запись от клиентов через Интернет.
services: storage
author: santoshc
ms.service: storage
ms.topic: article
ms.date: 05/12/2020
ms.author: santoshc
ms.reviewer: tamram
ms.subservice: common
ms.openlocfilehash: bdb33ebfb1ca37772a5b0db96acdbddd422578af
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595243"
---
# <a name="configure-network-routing-preference-for-azure-storage-preview"></a>Настройка предпочтительного варианта сетевой маршрутизации для службы хранилища Azure (предварительная версия)

Вы можете настроить [предпочтительный вариант сетевой маршрутизации](../../virtual-network/routing-preference-overview.md) (предварительная версия) для учетной записи хранения Azure, чтобы определить способ передачи сетевого трафика в учетную запись от клиентов через Интернет. По умолчанию трафик из Интернета направляется в общедоступную конечную точку учетной записи хранения через [глобальную сеть Майкрософт](../../networking/microsoft-global-network.md). Служба хранилища Azure предоставляет дополнительные возможности для настройки способа маршрутизации трафика в учетную запись хранения.

Настройка предпочтительного варианта маршрутизации позволяет оптимизировать передачу трафика в соответствии со стоимостью или производительностью сети уровня "Премиум". При настройке предпочтительного варианта маршрутизации вы определяете, как по умолчанию будет направляться трафик в общедоступную конечную точку для вашей учетной записи хранения. Вы также можете опубликовать конечные точки учетной записи хранения для конкретного маршрута.

## <a name="microsoft-global-network-versus-internet-routing"></a>Маршрутизация через глобальную сеть Майкрософт или через Интернет

По умолчанию клиенты за пределами среды Azure обращаются к учетной записи хранения через глобальную сеть Майкрософт. Глобальная сеть Майкрософт оптимизирована для выбора пути с низкой задержкой, что обеспечивает высокую надежность и оптимальную производительность сети. Входящий и исходящий трафик направляется через ближайшую к клиенту точку присутствия. Стандартная конфигурация маршрутизации гарантирует, что входящий и исходящий трафик учетной записи хранения большую часть пути проходит через глобальную сеть Майкрософт, что повышает производительность.

Вы можете изменить для учетной записи хранения конфигурацию маршрутизации так, чтобы входящий и исходящий трафик направлялся к клиентам и от клиентов за пределами среды Azure через точку присутствия, которая находится ближе всего к учетной записи хранения. Такой маршрут минимизирует перемещение трафика по глобальной сети Майкрософт, передавая его транзитному поставщику услуг Интернета при первой же возможности. Использование этой конфигурации маршрутизации снижает затраты на сеть.

На следующей схеме показано, как проходит трафик между клиентом и учетной записью хранения при использовании этих двух предпочтительных вариантов маршрутизации:

![Обзор вариантов маршрутизации для службы хранилища Azure](media/network-routing-preference/routing-options-diagram.png)

Дополнительные сведения о предпочтительном варианте маршрутизации (предварительная версия) в Azure см. в [этой статье](../../virtual-network/routing-preference-overview.md).

## <a name="routing-configuration"></a>Конфигурация маршрутизации

Вы можете выбрать для общедоступной конечной точки учетной записи хранения один из двух предпочтительных вариантов маршрутизации по умолчанию: через глобальную сеть Майкрософт или через Интернет. Настройка предпочтительного варианта маршрутизации по умолчанию применяется ко всему трафику от клиентов за пределами сети Azure и влияет на конечные точки Azure Data Lake Storage 2-го поколения, хранилища BLOB-объектов, Файлов Azure и статических сайтов. Настройка предпочтительного варианта маршрутизации не поддерживается для очередей Azure и таблиц Azure.

Вы также можете опубликовать конечные точки учетной записи хранения для конкретного маршрута. При публикации конечных точек для конкретного маршрута служба хранилища Azure создает в учетной записи хранения новые общедоступные конечные точки, которые направляют трафик по нужному пути. Такая гибкость позволяет направлять трафик учетной записи хранения по конкретному маршруту, не меняя предпочтительный вариант маршрутизации по умолчанию.

Например, публикация конечной точки для маршрута через Интернет в учетной записи StorageAccountA приведет к публикации следующих конечных точек для этой учетной записи хранения:

| Служба хранилища        | Конечная точка для конкретного маршрута                                  |
| :--------------------- | :------------------------------------------------------- |
| Служба больших двоичных объектов           | `StorageAccountA-internetrouting.blob.core.windows.net`  |
| Data Lake Storage 2-го поколения | `StorageAccountA-internetrouting.dfs.core.windows.net`   |
| Служба файлов           | `StorageAccountA-internetrouting.file.core.windows.net`  |
| Статические веб-сайты        | `StorageAccountA-internetrouting.web.core.windows.net`   |

Если у вас есть хранилище RA-GRS (геоизбыточное с доступом на чтение) или RA-GZRS (геоизбыточное между зонами с доступом на чтение), публикация конечных точек для конкретного маршрута автоматически создает дополнительные конечные точки в дополнительном регионе с доступом на чтение.

| Служба хранилища        | Вторичная конечная точка для конкретного маршрута с доступом только для чтения                        |
| :--------------------- | :----------------------------------------------------------------- |
| Служба больших двоичных объектов           | `StorageAccountA-internetrouting-secondary.blob.core.windows.net`  |
| Data Lake Storage 2-го поколения | `StorageAccountA-internetrouting-secondary.dfs.core.windows.net`   |
| Служба файлов           | `StorageAccountA-internetrouting-secondary.file.core.windows.net`  |
| Статические веб-сайты        | `StorageAccountA-internetrouting-secondary.web.core.windows.net`   |

Строки подключения для опубликованных конечных точек для конкретного маршрута можно скопировать на [портале Azure](https://portal.azure.com). Эти строки подключения можно использовать для авторизации с общим ключом во всех существующих пакетах SDK и API службы хранилища Azure.

## <a name="about-the-preview"></a>Сведения о предварительной версии

Предпочтительный вариант маршрутизации для службы хранилища Azure доступен в следующих регионах:

- Южная Франция
- Центрально-северная часть США
- центрально-западная часть США

Следующие известные проблемы влияют на работу предварительной версии функции выбора предпочтительного варианта маршрутизации для службы хранилища Azure.

- Запросы на доступ к конечной точке для конкретного маршрута, направляемого через глобальную сеть Майкрософт, возвращают ошибку HTTP 404 или что-то похожее. Маршрутизация через глобальную сеть Майкрософт работает нормально, только если она настроена как предпочтительный вариант по умолчанию для общедоступной конечной точки.

## <a name="pricing-and-billing"></a>Цены и выставление счетов

Сведения о ценах и выставлении счетов см. в **разделе цен** статьи о [предпочтительном варианте маршрутизации (предварительная версия)](../../virtual-network/routing-preference-overview.md#pricing).

## <a name="next-steps"></a>Дальнейшие действия

- [Что собой представляет функция выбора предпочтительного варианта маршрутизации (предварительная версия)?](../../virtual-network/routing-preference-overview.md)
- [Настройка брандмауэров службы хранилища Azure и виртуальных сетей](storage-network-security.md)
- [Рекомендации по обеспечению безопасности для хранилища BLOB-объектов](../blobs/security-recommendations.md)
