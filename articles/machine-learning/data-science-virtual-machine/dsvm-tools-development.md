---
title: Инструменты разработки
titleSuffix: Azure Data Science Virtual Machine
description: Сведения о средствах и интегрированных средах разработки, доступных на виртуальной машине для обработки и анализа данных.
keywords: средства анализа и обработки данных, виртуальная машина для анализа и обработки данных, средства для анализа и обработки данных, анализ и обработка данных Linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: c48cf6a7a82e90d3c9d8dc4c35e37dfb944af99f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80282687"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Средства разработки на виртуальной машине Azure для обработки и анализа данных

Виртуальная машина для обработки и анализа данных (DSVM) объединяет несколько популярных средств в высокопроизводительную интегрированную среду разработки (IDE). Ниже приведены некоторые средства, предоставляемые на виртуальной машине для обработки и анализа данных.

## <a name="visual-studio-community-edition"></a>выпуск Visual Studio Community;

|    |           |
| ------------- | ------------- |
| Что это такое?   | Среда IDE общего назначения      |
| Поддерживаемые версии DSVM      | Windows: Visual Studio 2017, Windows 2019: Visual Studio 2019      |
| Распространенные способы применения      | Разработка программного обеспечения    |
| Как она настраивается и устанавливается на DSVM?      | Рабочая нагрузка обработки и анализа данных (средства Python и R), рабочая нагрузка Azure (Hadoop, Data Lake), средства Node.js и SQL Server, [Машинное обучение Azure для Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai)    |
| Использование и запуск      | Ярлык рабочего стола`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`(). Графически откройте Visual Studio с помощью значка рабочего стола или меню " **Пуск** ". Откройте окно поиска программ (кнопки Windows + S) и введите **Visual Studio**. Здесь можно создавать проекты на разных языках, например C#, Python, R и Node.js.   |
| Связанные средства на DSVM      |     Visual Studio Code, RStudio, Juno  |

> [!NOTE]
> Может появиться сообщение о том, что период ознакомления завершился. Введите учетные данные для учетной записи Майкрософт. Вы можете создать новую бесплатную учетную запись, чтобы получить доступ к Visual Studio Community.

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Что это такое?   | Среда IDE общего назначения      |
| Поддерживаемые версии DSVM      | Windows, Linux     |
| Распространенные способы применения      | Редактор кода и интеграция Git   |
| Использование и запуск      | Ярлык рабочего стола`C:\Program Files (x86)\Microsoft VS Code\Code.exe`() в Windows, ярлык рабочего стола или`code`терминал () в Linux    |
| Связанные средства на DSVM      |     Visual Studio, RStudio, Juno  |

## <a name="rstudio-desktop"></a>RStudio Desktop;

|    |           |
| ------------- | ------------- |
| Что это такое?   | Интегрированная среда разработки клиента для языка R   |
| Поддерживаемые версии DSVM      | Windows, Linux      |
| Распространенные способы применения      |  Разработка на языке R     |
| Использование и запуск      | Ярлык рабочего стола`C:\Program Files\RStudio\bin\rstudio.exe`() в Windows, ярлык на`/usr/bin/rstudio`рабочем столе () в Linux      |
| Связанные средства на DSVM      |   Visual Studio, Visual Studio Code, Juno      |

## <a name="rstudio-server"></a>RStudio Server

|    |           |
| ------------- | ------------- |
| Что это такое?   | Интегрированная среда разработки клиента для языка R   |
| Что это такое?   | Интегрированная веб-среда разработки для R    |
| Поддерживаемые версии DSVM      | Linux      |
| Распространенные способы применения      |  Разработка на языке R     |
| Использование и запуск      | Включите службу с помощью _systemctl Enable Rstudio-Server_, а затем запустите службу с помощью _systemctl Start Rstudio-Server_. Затем войдите на сервер RStudio по адресу HTTP\/:/йоур-ВМ-ИП: 8787.       |
| Связанные средства на DSVM      |   Visual Studio, Visual Studio Code, RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Что это такое?   | Интегрированная среда разработки клиента для языка Julia   |
| Поддерживаемые версии DSVM      | Windows, Linux      |
| Распространенные способы применения      |  Разработка на языке Julia     |
| Использование и запуск      | Ярлык рабочего стола`C:\JuliaPro-0.5.1.1\Juno.bat`() в Windows, ярлык на`/opt/JuliaPro-VERSION/Juno`рабочем столе () в Linux      |
| Связанные средства на DSVM      |   Visual Studio, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>PyCharm

|    |           |
| ------------- | ------------- |
| Что это такое?   | Интегрированная среда разработки клиента для языка Python    |
| Поддерживаемые версии DSVM      | Windows 2019, Linux      |
| Распространенные способы применения      |  Разработка на Python     |
| Использование и запуск      | Ярлык рабочего стола`C:\Program Files\tk`() в Windows. Ярлык на рабочем столе (`/usr/bin/pycharm`) в Linux      |
| Связанные средства на DSVM      |   Visual Studio, Visual Studio Code, RStudio      |
