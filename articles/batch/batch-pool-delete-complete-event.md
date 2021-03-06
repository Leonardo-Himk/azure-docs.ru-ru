---
title: Событие завершения удаления пула пакетной службы Azure
description: Справочник по событию завершения удаления пула пакетной службы. Это событие создается при завершении операции удаления пула.
ms.topic: article
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: d28223c79f96f35a6ee11f98e0f09f21d7db4451
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115947"
---
# <a name="pool-delete-complete-event"></a>Событие завершения удаления пула

 Это событие создается при завершении операции удаления пула.

 Ниже приведен пример текста для события завершения удаления пула.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Элемент|Тип|Примечания|
|-------------|----------|-----------|
|`id`|Строка|Идентификатор пула.|
|`startTime`|DateTime|Время, когда было начато удаление пула.|
|`endTime`|DateTime|Время, когда удаление пула было завершено.|

## <a name="remarks"></a>Remarks
Дополнительные сведения о состояниях и кодах ошибок для операции изменения размера пула см. в статье [Удаление пула из учетной записи](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).
