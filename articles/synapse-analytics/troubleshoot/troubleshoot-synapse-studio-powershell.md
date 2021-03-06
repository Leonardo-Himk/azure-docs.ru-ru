---
title: Устранение неполадок подключения PowerShell для Azure синапсе Studio (Предварительная версия)
description: Устранение неполадок с подключением Azure синапсе Studio с помощью PowerShell
author: julieMSFT
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: bbc985407a6cb56f4f1b539f514ab092b5f7d0de
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81431479"
---
# <a name="diagnose-azure-synapse-studio-preview-connectivity-issues-with-powershell-script"></a>Диагностика проблем подключения Azure синапсе Studio (Предварительная версия) с помощью сценария PowerShell

Azure синапсе Studio (Предварительная версия) зависит от набора конечных точек веб-API для правильной работы. Это руководство поможет определить причины проблем с подключением:
- Настройка локальной сети (например, сети за корпоративным брандмауэром) для доступа к Azure синапсе Studio.
- возникли проблемы с подключением с помощью Azure синапсе Studio.

## <a name="prerequisite"></a>Предварительные требования

* PowerShell 5,0 или более поздней версии в Windows или
* PowerShell Core 6,0 или более поздней версии в Windows или Linux.

## <a name="troubleshooting-steps"></a>Действия по устранению неполадок

Щелкните следующую ссылку правой кнопкой мыши и выберите команду "Сохранить объект как":

- [Тест-азуресинапсе. ps1](https://go.microsoft.com/fwlink/?linkid=2119734)

Кроме того, вы можете открыть ссылку напрямую и сохранить открытый файл скрипта. Не сохраняйте адрес приведенной выше ссылки, так как он может измениться в будущем.

В проводнике щелкните правой кнопкой мыши скачанный файл скрипта и выберите команду "запустить с помощью PowerShell".

![Запуск скачанного файла скрипта с помощью PowerShell](media/troubleshooting-synapse-studio-powershell/run-with-powershell.png)

При появлении запроса введите имя рабочей области Azure синапсе, в которой возникла проблема или вы хотите проверить подключение, и нажмите клавишу ВВОД.

![Введите имя рабочей области](media/troubleshooting-synapse-studio-powershell/enter-workspace-name.png)

Будет запущен диагностический сеанс. Дождитесь завершения проверки.

![Дождитесь завершения диагностики](media/troubleshooting-synapse-studio-powershell/wait-for-diagnosis.png)

В итоге будет показана сводка диагностики. Если компьютеру не удается подключиться к одной или нескольким конечным точкам, в разделе "Сводка" будут показаны некоторые рекомендации.

![Обзор диагностики](media/troubleshooting-synapse-studio-powershell/diagnosis-summary.png)

Кроме того, файл журнала диагностики для этого сеанса будет создан в той же папке, что и сценарий устранения неполадок. Его расположение показано в разделе "Общие рекомендации" (`D:\TestAzureSynapse_2020....log`). При необходимости вы можете отправить этот файл в службу технической поддержки.

Если вы являетесь администратором сети и настраиваете конфигурацию брандмауэра для Azure синапсе Studio, дополнительные сведения см. в разделе "Сводка".

* Все тестовые элементы (запросы), помеченные как "пройденные", означают, что они прошли проверку подключения, независимо от кода состояния HTTP.
 Для неудачных запросов причина отображается желтым цветом, например `NamedResolutionFailure` или. `ConnectFailure` Эти причины могут помочь определить, есть ли в вашей сетевой среде недопустимые конфигурации.


## <a name="next-steps"></a>Дальнейшие шаги
Если предыдущие шаги не помогли устранить проблему, создайте запрос [в службу поддержки](../../sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket.md).
