---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/08/2019
ms.author: tamram
ms.openlocfilehash: 2ed88d8abb7cbe96093b68d89030e6e464a35541
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "75392250"
---
| Ресурс | Назначение        |
|----------|---------------|
| Максимальный размер одного контейнера больших двоичных объектов | То же, что и максимальная емкость учетной записи хранения |
| Максимальное число блоков в блочном BLOB-объекте или добавочном большом двоичном объекте | 50 000 блоков |
| Максимальный размер блока в блочном BLOB-объекте | 100 МиБ |
| Максимальный размер блочного BLOB-объекта | 50 000 X 100 MiB (приблизительно 4,75 Тиб) |
| Максимальный размер блока в добавочном большом двоичном объекте | 4 МиБ |
| Максимальный размер добавочного большого двоичного объекта | 50 000 x 4 MiB (приблизительно 195 гиб) |
| Максимальный размер страничного BLOB-объекта | 8 ТиБ |
| Максимальное число хранимых политик доступа на контейнер больших двоичных объектов | 5 |
|Частота целевых запросов для одного большого двоичного объекта | До 500 запросов в секунду |
|Целевая пропускная способность для одного страничного BLOB-объекта | До 60 МиБ в секунду |
|Целевая пропускная способность для одного блочного большого двоичного объекта |Пределы входящих и исходящих данных учетной записи хранения<sup>1</sup> |

<sup>1</sup> пропускная способность для одного большого двоичного объекта зависит от нескольких факторов, включая, но не ограничиваясь: параллелизм, размер запроса, уровень производительности, скорость источника для отправки и назначение для загрузки. Чтобы воспользоваться преимуществами повышения производительности [блочных BLOB-объектов с высокой пропускной способностью](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/), отправьте большие двоичные объекты или блоки. В частности, вызовите операцию [помещения большого двоичного объекта](/rest/api/storageservices/put-blob) или [размещения блока](/rest/api/storageservices/put-block) с большим двоичным объектом или размером блока, который больше 4 MiB для учетных записей хранения уровня "Стандартный". Для блочного BLOB-объекта уровня "Премиум" или для учетных записей хранения Data Lake Storage 2-го поколения используйте размер блока или большого двоичного объекта, превышающий 256 КИБ.
