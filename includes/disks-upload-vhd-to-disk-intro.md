---
title: Включить имя файла
description: включить файл
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e96d205ef1a8f94baa3a0cfe6c5127b6cf570e5a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80420958"
---
В этой статье объясняется, как отправить VHD с локального компьютера на управляемый диск Azure или скопировать управляемый диск в другой регион с помощью AzCopy. Этот процесс, прямая отправка, также позволяет передавать VHD вплоть до 32 Тиб в размере непосредственно на управляемый диск. В настоящее время прямая отправка поддерживается для дисков уровня "Стандартный", "Стандартный SSD" и "Премиум", управляемых на SSD. Она еще не поддерживается для дисков Ultra.

Если вы предоставляете решение для резервного копирования для виртуальных машин IaaS в Azure, мы рекомендуем использовать прямую отправку для восстановления резервных копий клиентов на управляемые диски. При отправке виртуального жесткого диска из внешнего источника в Azure скорость будет зависеть от локальной пропускной способности. При отправке или копировании из виртуальной машины Azure пропускная способность будет такой же, как и для стандартных дисков.