---
title: Настройка лаборатории для скриптов оболочки Linux в Службах лабораторий Azure | Документация Майкрософт
description: Сведения о том, как настроить лабораторию для курсов по скриптам оболочки Linux.
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
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 66b325eb1d268fdd5b1052a0da84c603186edf65
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589505"
---
# <a name="set-up-a-lab-to-teach-shell-scripting-on-linux"></a>Настройка лаборатории для курсов по скриптам оболочки Linux
В этой статье показано, как настроить лабораторию для курсов по скриптам оболочки Linux. Скрипты — это полезная часть системного администрирования, которая позволяет администраторам избежать повторяющихся задач. В этом примере сценария класс охватывает традиционные bash-скрипты и расширенные скрипты. Расширенные скрипты — это скрипты, сочетающие bash-команды и Ruby. Этот подход позволяет Ruby передавать данные и bash-команды для взаимодействия с оболочкой. 

Студенты, посещающие эти занятия по написанию сценариев, получают виртуальную машину Linux для изучения основ Linux, а также знакомятся со скриптами оболочки bash. Виртуальная машина Linux поставляется с включенным доступом к удаленному рабочему столу и установленными текстовыми редакторами [gedit](https://help.gnome.org/users/gedit/stable/) и [ Visual Studio Code](https://code.visualstudio.com/).

## <a name="lab-configuration"></a>Настройка лаборатории
Для настройки этой лаборатории вам прежде всего потребуется подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу. Создав подписку Azure, вы можете создать новую учетную запись лаборатории в Службах лабораторий Azure или использовать существующую учетную запись лаборатории. Чтобы создать учетную запись лаборатории, ознакомьтесь со следующим руководством: [Руководство. Настройка учетной записи лаборатории с помощью Служб лабораторий Azure](tutorial-setup-lab-account.md).

После создания учетной записи лаборатории включите следующие параметры в учетной записи лаборатории. 

| Настройки учетной записи лаборатории | Instructions |
| ----------- | ------------ |  
| Образы Marketplace | Разрешите использование образа Ubuntu Server 18.04 LTS в учетной записи лаборатории. Дополнительные сведения см. в статье [Выбор образов Marketplace, доступных для создателей лаборатории](specify-marketplace-images.md). | 

Следуйте рекомендациям в [этом руководстве](tutorial-setup-classroom-lab.md), чтобы создать лабораторию и применить следующие параметры:

| Параметры лаборатории | Значение или инструкции | 
| ------------ | ------------------ |
| Размер виртуальной машины | Малый  |
| Образ виртуальной машины | Ubuntu Server 18.04 LTS |
| Разрешение подключения к удаленному рабочему столу | Включить. <p>Включение этого параметра позволит преподавателям и учащимся подключаться к виртуальным машинам с помощью удаленного рабочего стола (RDP). Дополнительные сведения см. в статье [Включение удаленного рабочего стола для виртуальных машин Linux в лаборатории в Службах лабораторий Azure](how-to-enable-remote-desktop-linux.md#connect-to-the-template-vm). </p>|


## <a name="install-desktop-and-xrdp"></a>Установка рабочего стола и xrdp
В образе Ubuntu Server 18.04 LTS по умолчанию отсутствует сервер удаленного рабочего стола. Следуйте инструкциям из статьи [Установка и настройка удаленного рабочего стола для подключения к виртуальной машине Linux в Azure](../../virtual-machines/linux/use-remote-desktop.md), чтобы установить на шаблоне виртуальной машины пакеты, необходимые для подключения через протокол удаленного рабочего стола.

## <a name="install-ruby"></a>Установка Ruby
Ruby — это динамический язык с открытым кодом, который можно сочетать со скриптами bash. В этом разделе описано, как с помощью `apt-get` установить последнюю версию [Ruby](https://www.ruby-lang.org/).

1. Для установки обновлений выполните следующие команды:

    ```bash
    sudo apt-get update 
    sudo apt-get upgrade 
    ```
2.  Установите [Ruby](https://www.ruby-lang.org/).  Ruby — это динамический язык с открытым кодом, который можно сочетать со скриптами bash. 
    
    ```bash
    sudo apt-get install ruby-full
    ```

## <a name="install-development-tools"></a>Установка средств разработки
В этом разделе показано, как установить несколько текстовых редакторов. Gedit является текстовым редактором по умолчанию в среде рабочего стола GNOME. Это типовой текстовый редактор общего назначения. Текстовый редактор Visual Studio Code поддерживает интеграцию с системами отладки и управления версиями.

> [!NOTE]
> Существует много доступных текстовых редакторов. Visual Studio Code и gedit — это лишь два примера.

1. Установите [gedit](https://help.gnome.org/users/gedit/stable/).

    ```bash
    sudo apt-get install gedit
    ```
1. Установите [Visual Studio Code](https://code.visualstudio.com/).  Visual Studio Code можно установить из Snap Store.  Другие варианты установки представлены на [этой странице](https://code.visualstudio.com/#alt-downloads).

    ```bash
    sudo snap install vscode --classic 
    ```

    Итак, вы обновили шаблон и теперь у вас есть и язык программирования, и средства разработки для создаваемой лаборатории. Теперь образ шаблона можно опубликовать в лаборатории. Нажмите кнопку **Опубликовать** на странице шаблона, чтобы опубликовать этот шаблон в лаборатории.  

## <a name="cost"></a>Стоимость 
Если вы хотите оценить стоимость этой лаборатории, можно использовать следующий пример:
 
Для класса из 25 учащихся с 20 часами запланированного времени обучения и 10 часами квоты для домашних заданий или назначений цена за лабораторию будет такой: 

25 учащихся * (20 + 10) часов * 20 единиц лаборатории * 0,01 долл. США в час = 150 долл. США.

Дополнительные сведения о ценах см. в следующем документе: [Цены на Службы лабораторий Azure](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Заключение
Из этой статьи вы узнали, как создать лабораторию для занятий по скриптам. В ней рассматривается настройка на компьютере Linux средств создания скриптов на языке Ruby, но этот же подход можно применить и для классов по скриптам на других языках, таких как Python в Linux.

## <a name="next-steps"></a>Дальнейшие действия
Следующие шаги являются общими для настройки любой лаборатории.

- [Добавление пользователей](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Настройка квоты](how-to-configure-student-usage.md#set-quotas-for-users)
- [Настройка расписания](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Отправка по электронной почте ссылок для регистрации учащимся](how-to-configure-student-usage.md#send-invitations-to-users) 





