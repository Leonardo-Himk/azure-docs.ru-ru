---
title: Устранение незначительного резервного копирования файлов и папок
description: В этой статье представлены инструкции по устранению неполадок, которые помогут вам диагностировать причины проблем с производительностью службы архивации Azure.
ms.reviewer: saurse
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.openlocfilehash: 5e669a68794a8622bb4a2fa55b206153717fd772
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187908"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Устранение неполадок, связанных с низкой производительностью архивации файлов и папок в службе архивации Azure

В этой статье представлены инструкции по устранению неполадок, которые помогут вам диагностировать причины низкой производительности при архивации файлов и папок при использовании службы архивации Azure. Если архивация файлов выполняется с помощью агента службы архивации Azure, этот процесс может занять больше времени, чем ожидалось. Ниже приведены причины возникновения задержек.

* [На компьютере, на котором выполняется резервное копирование, имеются узкие места в производительности.](#cause1)
* [На работу службы архивации Azure влияет другой процесс или антивирусная программа](#cause2)
* [Агент службы архивации запущен на виртуальной машине Azure](#cause3)  
* [Архивация большого количества файлов (нескольких миллионов)](#cause4)

Прежде чем приступить к устранению неполадок, рекомендуется скачать и установить [последнюю версию агента службы архивации Azure](https://aka.ms/azurebackup_agent). Мы периодически обновляем агент службы архивации, чтобы устранить различные проблемы, добавить новые возможности и улучшить производительность.

Кроме того, настоятельно рекомендуется изучить статью [Служба архивации Azure: часто задаваемые вопросы](backup-azure-backup-faq.md) , чтобы убедиться, что проблемы c производительностью не вызваны общими ошибками конфигурации.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause-backup-job-running-in-unoptimized-mode"></a>Причина: задание резервного копирования выполняется в неоптимизированном режиме

* Агент MARS может запускать задание резервного копирования в **оптимизированном режиме** с помощью журнала изменений USN (порядковый номер обновления) или **неоптимизированного режима** путем проверки наличия изменений в каталогах или файлах путем сканирования всего тома.
* Неоптимизированный режим работает слишком долго, так как агент должен сканировать каждый файл на томе и сравнить его с метаданными для определения измененных файлов.
* Чтобы проверить это, откройте **сведения о задании** в консоли агента MARS и проверьте состояние, чтобы узнать, сообщает ли он о **передаче данных (не оптимизировать, может занять больше времени)** , как показано ниже.

    ![Выполняется в неоптимизированном режиме](./media/backup-azure-troubleshoot-slow-backup-performance-issue/unoptimized-mode.png)

* Выполнение задания резервного копирования в неоптимизированном режиме может быть вызвано следующими причинами.
  * Первая резервная копия (также известная как начальная репликация) всегда будет выполняться в неоптимизированном режиме
  * Если предыдущее задание резервного копирования завершится сбоем, следующее запланированное задание архивации будет выполняться как неоптимизированное.

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Причина: наличие узких мест производительности на компьютере

Наличие узких мест производительности на компьютере, для которого выполняется архивация, может приводить к задержкам. Например, причиной узких мест на компьютере может быть возможность чтения с диска или записи на него либо пропускная способность, доступная для отправки данных по сети.

Windows предоставляет встроенное средство, называемое [системным монитором](https://techcommunity.microsoft.com/t5/ask-the-performance-team/windows-performance-monitor-overview/ba-p/375481) (PerfMon) для обнаружения этих узких мест.

Ниже приведены некоторые счетчики производительности и диапазоны, которые могут оказаться полезными при обнаружении узких мест производительности для оптимизации архивации.

| Счетчик | Состояние |
| --- | --- |
| Логический диск (физический диск) — % времени простоя |* 100% времени бездействия в 50% Idle = исправен</br>* 49% времени бездействия в 20% Idle = предупреждение или монитор</br>* 19% бездействие в 0% Idle = критическое или не хватает спецификации |
| Логический диск (физический диск)--% AVG. время чтения или записи на диск (с) |от * 0,001 мс до 0,015 МС = работоспособное</br>* 0,015 МС 0,025 МС = предупреждение или монитор</br>* 0,026 МС или более длинный = критический или исходящий из спецификации |
| Логический диск (физический диск) — текущая длина очереди диска (для всех экземпляров) |80 запросов в течение более 6 минут |
| Память — байт в невыгружаемом пуле |* Менее 60% использования пула = работоспособное состояние<br>от * 61% до 80% использования пула = предупреждение или монитор</br>* Более 80% использования пула = критическое или не из спецификации |
| Память — байт в выгружаемом пуле |* Менее 60% использования пула = работоспособное состояние</br>от * 61% до 80% использования пула = предупреждение или монитор</br>* Более 80% использования пула = критическое или не из спецификации |
| Доступная память (МБ) |* 50% свободной памяти или более: исправен</br>* 25% доступной свободной памяти = монитор</br>* 10% доступной свободной памяти = предупреждение</br>* Доступно менее 100 МБ или 5% свободной памяти = критическое или не используемое спецификацией |
| Процессор — \% загруженности процессора (все экземпляры) |* Потреблено менее 60% = работоспособное</br>* с 61% на 90% потреблено = отслеживание или предостережение</br>* 91% на 100% потреблено = критическое |

> [!NOTE]
> Если выяснилось, что причина в инфраструктуре, то рекомендуется регулярно выполнять дефрагментацию дисков, чтобы повысить производительность.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Причина: на работу службы архивации Azure влияет другой процесс или антивирусная программа

Мы уже сталкивались с несколькими случаями, когда на производительность агента службы архивации Azure отрицательно влияли другие процессы, запущенные в системе Windows. Например, если для архивации данных используется как агент службы архивации Azure, так и другая программа, или если выполняется архивация файлов, заблокированных антивирусной программой. В таких ситуациях архивация может завершиться сбоем или занять больше времени, чем ожидалось.

Для решения этой проблемы рекомендуется отключить другие программы архивации и отследить время резервного копирования, выполняемого с помощью агента службы архивации Azure. Как правило, чтобы предотвратить эти конфликтные ситуации, достаточно не выполнять несколько заданий архивации одновременно.

Для антивирусных программ необходимо исключить следующие файлы и расположения:

* процесс C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe;
* папки C:\Program Files\Microsoft Azure Recovery Services Agent\;
* вспомогательное расположение (если не используется стандартное расположение).

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Причина: агент службы архивации запущен на виртуальной машине Azure

Если агент службы архивации запущен на виртуальной машине Azure, а не на физическом компьютере, это может привести к снижению производительности. Это ожидаемое поведение, так как максимальное количество операций ввода-вывода ограничено.  Чтобы оптимизировать производительность, рекомендуется переместить данные, для которых выполняется архивация, в службу хранилища Azure уровня "Премиум". Мы работаем над этой проблемой, и она будет устранена в следующей версии.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Причина: архивация большого количества файлов (нескольких миллионов)

Перемещение большого объема данных занимает больше времени, чем перемещение меньшего объема данных. В некоторых случаях длительность архивации обуславливается не только объемом данных, но количеством файлов или папок. Это особенно актуально при архивации миллионов небольших файлов (размером от нескольких байтов до нескольких килобайтов).

Причина этого состоит в том, что при архивации и переносе данных в Azure одновременно выполняется каталогизация файлов. Иногда операция каталогизации может занять больше времени, чем ожидалось.

Ниже приведены признаки, которые помогут выявить узкие места и принять соответствующие меры.

* **В пользовательском интерфейсе отображается ход выполнения передачи данных**. Идет перенос данных. Возможны задержки из-за пропускной способности сети или объема данных.
* **В пользовательском интерфейс не отображается ход выполнения передачи данных**. В этом случае необходимо открыть файлы журналов в папке C:\Program Files\Microsoft Azure Recovery Services Agent\Temp и проверить в них наличие записи FileProvider::EndData. Наличие этой записи означает, что передача данных завершена, и выполняется операция каталогизации. Не отменяйте задание архивации. Вместо этого подождите еще немного, пока не завершится операция каталогизации. Если проблема будет повторяться, обратитесь в [службу поддержки Azure](https://portal.azure.com/#create/Microsoft.Support).

## <a name="next-steps"></a>Следующие шаги

* [Распространенные вопросы о резервном копировании файлов и папок](backup-azure-file-folder-backup-faq.md)
