---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 3e1f9c225a57e7d41f85c2a92dac989453057c51
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2020
ms.locfileid: "82736983"
---
Ниже приведены ограничения для размера данных, копируемых в учетную запись хранения. Убедитесь, что отправляемые данные соответствуют этим ограничениям. Самые последние сведения об этих ограничениях см. в разделе [целевые показатели масштабируемости и производительности для хранилища BLOB-объектов](../articles/storage/blobs/scalability-targets.md) и [целевые показатели масштабируемости и производительности файлов Azure](../articles/storage/files/storage-files-scale-targets.md).

| Размер данных, копируемых в учетную запись хранения Azure                      | Ограничение по умолчанию          |
|---------------------------------------------------------------------|------------------------|
| Блочный BLOB-объект и страничный BLOB-объект                                            | 2 ПБ для США и Европы.<br>500 ТБ для всех других регионов, включая Великобритания.  <br> Сюда входят данные из всех источников, включая Data Box.|
| Файлы Azure                                                          | Максимальный размер стандартных файловых ресурсов 100TiB *, 5 ТБ, общих файловых ресурсов уровня "Премиум" 100TiB на общую папку.<br> Все папки в *StorageAccount_AzureFiles* должны соответствовать этому ограничению.       |
