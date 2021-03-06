---
title: Сброс паролей для виртуальных машин лаборатории в службах лаборатории Azure | Документация Майкрософт
description: Узнайте, как сбросить пароли для виртуальных машин в учебных лабораториях служб Azure Labs.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: 7c757ef8508f9364a46116e6ddf19207f23a4b6f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "73583701"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs-students"></a>Установка или сброс пароля для виртуальных машин в учебных лабораториях (студенты)
В этой статье показано, как студенты могут задать или сбросить пароль для своих виртуальных машин. 

## <a name="enable-resetting-of-passwords"></a>Включить сброс паролей
На момент создания лаборатории владелец лаборатории может включить или отключить **использование одного и того же пароля для всех виртуальных машин**. Если этот параметр включен, студенты не смогут сбросить пароль. Все виртуальные машины в лабораториях будут иметь тот же пароль, который задается преподавателем. 

Если этот параметр отключен, пользователям необходимо будет задать пароль при попытке подключения к виртуальной машине в первый раз. Студенты также могут сбросить пароль позже в любое время, как показано в последнем разделе этой статьи. 

## <a name="reset-password-for-the-first-time"></a>Сбросить пароль в первый раз
Если параметр **использовать один и тот же пароль для всех виртуальных машин** был отключен, то при нажатии кнопки **Подключиться** на плитке лаборатории на странице **мои виртуальные машины** пользователь увидит следующее диалоговое окно, чтобы задать пароль для виртуальной машины: 

![Сброс пароля для учащегося](../media/how-to-set-virtual-machine-passwords/student-set-password.png)

## <a name="reset-password-later"></a>Сбросить пароль позже
Студент также может задать пароль, щелкнув меню переполнения (**вертикально три точки**) на плитке лаборатории и выбрав **Сброс пароля**. 

![Сбросить пароль позже](../media/how-to-set-virtual-machine-passwords/student-set-password-2.png)


## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о других вариантах использования учащихся, которые может настроить владелец лаборатории, см. в следующей статье: [Настройка использования учащихся](how-to-configure-student-usage.md).
