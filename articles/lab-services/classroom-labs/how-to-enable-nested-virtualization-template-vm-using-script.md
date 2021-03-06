---
title: Включение вложенной виртуализации на виртуальной машине шаблона в службах лаборатории Azure (скрипт) | Документация Майкрософт
description: Узнайте, как создать шаблон виртуальной машины с несколькими виртуальными машинами в.  Иными словами, включите вложенную виртуализацию на виртуальной машине шаблона в службах лаборатории Azure.
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
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: 56e5ad21f94521565b4df193b2450a1c994b66f8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79503040"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services-using-a-script"></a>Включение вложенной виртуализации на виртуальной машине шаблона в службах лаборатории Azure с помощью скрипта

Вложенная виртуализация позволяет создать среду с несколькими виртуальными машинами в виртуальной машине шаблона лаборатории. Публикация шаблона предоставит каждому пользователю в лаборатории виртуальную машину, настроенную с несколькими виртуальными машинами в ней.  Дополнительные сведения о вложенной виртуализации и службах лаборатории Azure см. [в статье Включение вложенной виртуализации на виртуальной машине шаблона в службах лаборатории Azure](how-to-enable-nested-virtualization-template-vm.md).

Действия, описанные в этой статье, касаются настройки вложенной виртуализации для Windows Server 2016 или Windows Server 2019. Для настройки компьютера шаблона с помощью Hyper-V будет использоваться сценарий.  Ниже приведены инструкции по использованию [сценариев Hyper-V в службах лаборатории](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV).

>[!IMPORTANT]
>Выберите **крупный (вложенная виртуализация)** или **средняя (вложенная виртуализация)** для размера виртуальной машины при создании лаборатории.  В противном случае вложенная виртуализация не будет работать.  

## <a name="run-script"></a>Запуск сценария

1. Если вы используете Internet Explorer, может потребоваться добавить `https://github.com` в список надежных сайтов.
    1. Откройте обозреватель Internet Explorer.
    1. Щелкните значок шестеренки и выберите **Свойства браузера**.  
    1. Когда откроется диалоговое окно **Свойства обозревателя** , выберите **Безопасность**, выберите **Надежные сайты**, нажмите кнопку **сайты** .
    1. Когда появится диалоговое окно **Надежные сайты** , `https://github.com` добавьте в список Доверенные веб-сайты и нажмите кнопку **Закрыть**.

        ![Надежные сайты](../media/how-to-enable-nested-virtualization-template-vm-using-script/trusted-sites-dialog.png)
1. Скачайте файлы репозитория Git, как описано в следующих шагах.
    1. Перейдите по [https://github.com/Azure/azure-devtestlab/](https://github.com/Azure/azure-devtestlab/)адресу.
    1. Нажмите кнопку **клонировать или скачать** .
    1. Щелкните **скачать ZIP-файл**.
    1. Извлеките ZIP-файл

    >[!TIP]
    >Репозиторий Git также можно клонировать по адресу [https://github.com/Azure/azure-devtestlab.git](https://github.com/Azure/azure-devtestlab.git).

1. Запустите **PowerShell** в режиме **администратора** .
1. В окне PowerShell перейдите к папке с скачанным скриптом. Если вы выполняете переход из верхней папки файлов репозитория, сценарий находится по адресу `azure-devtestlab\samples\ClassroomLabs\Scripts\HyperV\`.
1. Для успешного выполнения скрипта может потребоваться изменить политику выполнения. Выполните следующую команду:

    ```powershell
    Set-ExecutionPolicy bypass -force
    ```

1. Выполните скрипт:

    ```powershell
    .\SetupForNestedVirtualization.ps1
    ```

    > [!NOTE]
    > Для выполнения сценария может потребоваться перезапустить компьютер. Следуйте инструкциям скрипта и повторно запустите скрипт, пока в выходных данных не появится элемент **script завершен** .
1. Не забудьте сбросить политику выполнения. Выполните следующую команду:

    ```powershell
    Set-ExecutionPolicy default -force
    ```

## <a name="conclusion"></a>Заключение

Теперь ваш компьютер шаблона готов к созданию виртуальных машин Hyper-V. Инструкции по созданию виртуальных машин Hyper-V см. [в статье Создание виртуальной машины в Hyper-v](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v) . См. также раздел [Центр оценки Майкрософт](https://www.microsoft.com/evalcenter/) для извлечения доступных операционных систем и программного обеспечения.  

## <a name="next-steps"></a>Дальнейшие шаги

Дальнейшие действия являются общими для настройки любой лаборатории.

- [Добавление пользователей](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Задать квоту](how-to-configure-student-usage.md#set-quotas-for-users)
- [Задание расписания](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Ссылки для регистрации электронной почты учащихся](how-to-configure-student-usage.md#send-invitations-to-users)