---
title: Мониторинг целостности файлов в центре безопасности Azure
description: Узнайте, как сравнивать базовые показатели с мониторингом целостности файлов в центре безопасности Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c8a2a589-b737-46c1-b508-7ea52e301e8f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: memildin
ms.openlocfilehash: bb45e1d1ee17a6daf16bd688982f79fda986bde5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "73664405"
---
# <a name="compare-baselines-using-file-integrity-monitoring-fim"></a>Сравнение базовых показателей с помощью мониторинга целостности файлов

Наблюдение за целостностью файлов (FIM) информирует о том, что изменения происходят в конфиденциальных областях ресурсов, что позволяет исследовать и устранять несанкционированные действия. FIM наблюдает за файлами Windows, реестрами Windows и файлами Linux.

В этом разделе объясняется, как включить FIM для файлов и реестров. Дополнительные сведения о FIM см. [в статье мониторинг целостности файлов в центре безопасности Azure](security-center-file-integrity-monitoring.md).

## <a name="why-use-fim"></a>Зачем использовать FIM?

Операционная система, приложения и связанные конфигурации управляют поведением и состоянием безопасности ресурсов. Таким образом, злоумышленники нацелены на файлы, управляющие ресурсами, чтобы овертаке операционную систему ресурса и (или) выполнять действия без обнаружения.

Фактически, для многих стандартов соответствия нормативным требованиям, таких как PCI-DSS & ISO 17799, требуется реализация элементов управления FIM.  

## <a name="enable-built-in-recursive-registry-checks"></a>Включить встроенные рекурсивные проверки реестра

Параметры Hive реестра FIM предоставляют удобный способ отслеживания рекурсивных изменений в общих областях безопасности.  Например, злоумышленник может настроить сценарий для выполнения в LOCAL_SYSTEM контексте, настроив выполнение при запуске или завершении работы.  Чтобы отслеживать изменения этого типа, включите встроенную проверку.  

![Реестр](./media/security-center-file-integrity-monitoring-baselines/baselines-registry.png)

>[!NOTE]
> Рекурсивные проверки применяются только к рекомендуемым кустам безопасности, а не к пользовательским путям реестра.  

## <a name="adding-a-custom-registry-check"></a>Добавление пользовательской проверки реестра

Базовые показатели FIM начинаются с определения характеристик известного состояния операционной системы и поддерживающего приложения.  В этом примере мы рассмотрим конфигурации политики паролей для Windows Server 2008 и более поздних версий.


|Имя политики                 | Параметр реестра|
|---------------------------------------|-------------|
|Контроллер домена: запретить изменение пароля учетных записей компьютера.| Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\рефусепассвордчанже|
|Член домена: всегда требуется цифровая подпись или шифрование потока данных безопасного канала.|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\рекуиресигнорсеал|
|Член домена: шифрование данных безопасного канала, когда это возможно|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\сеалсекуречаннел|
|Член домена: цифровая подпись данных безопасного канала, когда это возможно|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\сигнсекуречаннел|
|Член домена: отключить изменение пароля учетных записей компьютера.|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\дисаблепассвордчанже|
|Член домена: максимальный срок действия пароля учетных записей компьютера|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\максимумпассвордаже|
|Член домена: требовать стойкий сеансовый ключ (Windows 2000 или выше)|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\рекуирестронгкэй|
|Сетевая безопасность: ограничение NTLM: проверка подлинности NTLM в этом домене|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\рестриктнтлминдомаин|
|Сетевая безопасность: ограничения NTLM: добавить исключения для серверов в этом домене.|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\дкалловеднтлмсерверс|
|Сетевая безопасность: ограничения NTLM — аудит проверки подлинности NTLM на этом домене|Мачине\систем\куррентконтролсет\сервицес \Нетлогон\параметерс\аудитнтлминдомаин|

> [!NOTE]
> Дополнительные сведения о параметрах реестра, поддерживаемых различными версиями операционных систем, см. в [справочной таблице по параметрам групповая политика](https://www.microsoft.com/download/confirmation.aspx?id=25250).

*Настройка FIM для мониторинга базовых показателей реестра:*

1. В окне **Добавление реестра Windows для отслеживание изменений** в текстовом поле раздел **реестра Windows** введите раздел реестра.

    <code>

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters
    </code>

      ![Включение FIM в реестре](./media/security-center-file-integrity-monitoring-baselines/baselines-add-registry.png)

## <a name="tracking-changes-to-windows-files"></a>Отслеживание изменений в файлах Windows

1. В окне **Добавление файла Windows для отслеживание изменений** в текстовом поле **введите путь** введите папку, содержащую файлы, которые необходимо отвестить. В примере на следующем рисунке **веб-приложение Contoso** находится в D:\ диск в структуре папок **контосвебапп** .  
1. Создайте настраиваемую запись файла Windows, указав имя класса параметров, включив рекурсию и указав верхнюю папку с суффиксом-шаблоном (*).

    ![Включение FIM для файла](./media/security-center-file-integrity-monitoring-baselines/baselines-add-file.png)

## <a name="retrieving-change-data"></a>Получение данных изменений

Данные мониторинга целостности файлов находятся в наборе таблиц Azure Log Analytics/ConfigurationChange.  

 1. Задайте диапазон времени, чтобы получить сводку изменений по ресурсам.
В следующем примере мы извлекаем все изменения за последние четырнадцать дней в категориях реестра и файлов:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry', 'Files')

    > | summarize count() by Computer, ConfigChangeType

    </code>

1. Чтобы просмотреть сведения об изменениях в реестре, выполните следующие действия.

    1. Удалите **файлы** из предложения **WHERE** , 
    1. Удалите строку формирования сводных данных и замените ее предложением упорядочивания:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry')

    > | order by Computer, RegistryKey

    </code>

Отчеты можно экспортировать в CSV для архивирования и (или) передачи в Power BI отчет.  

![Данные FIM](./media/security-center-file-integrity-monitoring-baselines/baselines-data.png)