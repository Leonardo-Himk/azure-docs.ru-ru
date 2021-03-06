---
title: Управление Отслеживание изменений и инвентаризацией в службе автоматизации Azure
description: В этой статье рассказывается, как использовать Отслеживание изменений и инвентаризацию для контроля изменений программного обеспечения и служб Майкрософт, происходящих в вашей среде.
services: automation
ms.subservice: change-inventory-management
ms.date: 07/03/2018
ms.topic: conceptual
ms.openlocfilehash: 8ca1bd7a724d3256bc2e171ce39fd6a06e2e5935
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2020
ms.locfileid: "82779303"
---
# <a name="manage-change-tracking-and-inventory"></a>Управление решением для отслеживания изменений и инвентаризации

При добавлении нового файла или раздела реестра для отслеживания служба автоматизации Azure включает его для функции [Отслеживание изменений и инвентаризации](change-tracking.md) . Эта статья содержит процедуры для работы с этой функцией.

## <a name="enable-the-full-change-tracking-and-inventory-feature"></a>Включение функции полного Отслеживание изменений и инвентаризации

Если вы включили [мониторинг целостности файлов центра безопасности Azure (FIM)](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring), вы можете использовать полную отслеживание изменений и функцию инвентаризации, как описано ниже. Эти параметры не удаляются этим процессом.

> [!NOTE]
> Включение функции полного Отслеживание изменений и инвентаризации может привести к дополнительным расходам. См. раздел [цены на автоматизацию](https://azure.microsoft.com/pricing/details/automation/).

1. Удалите решение для мониторинга, перейдя в рабочую область и найдя его в [списке установленных решений мониторинга](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions).
2. Щелкните имя решения, чтобы открыть его страницу сводки, а затем щелкните **Удалить**, как описано в статье [Удаление решения мониторинга](../azure-monitor/insights/solutions.md#remove-a-monitoring-solution).
3. Чтобы повторно включить Отслеживание изменений и инвентаризацию, перейдите к учетной записи службы автоматизации и выберите **Отслеживание изменений** в разделе **Управление конфигурацией**.
4. Выберите рабочую область Log Analytics и учетную запись автоматизации, подтвердите параметры рабочей области и нажмите кнопку **включить**.

## <a name="onboard-machines-to-change-tracking-and-inventory"></a><a name="onboard"></a>Подключение компьютеров к Отслеживание изменений и инвентаризации

Чтобы начать отслеживание изменений, необходимо включить Отслеживание изменений и инвентаризацию в службе автоматизации Azure. Ниже приведены рекомендуемые и поддерживаемые способы подключения компьютеров к этой функции. 

* [Подключение из виртуальной машины](automation-onboard-solutions-from-vm.md)
* [Подключение из обзора нескольких компьютеров](automation-onboard-solutions-from-browse.md)
* [Подключение из учетной записи службы автоматизации](automation-onboard-solutions-from-automation-account.md)
* [Подключение в модуле Runbook службы автоматизации Azure](automation-onboard-solutions.md)

## <a name="track-files"></a>Файлы трассировки

### <a name="configure-file-tracking-on-windows"></a>Настройка отслеживания файлов в Windows

Чтобы настроить отслеживание файлов на компьютерах Windows, выполните следующие действия.

1. В учетной записи службы автоматизации выберите **Отслеживание изменений** в разделе **Управление конфигурацией**. 
2. Щелкните **Изменить параметры** (символ шестеренки).
3. На странице Конфигурация рабочей области выберите **файлы Windows**, а затем нажмите кнопку **+ Добавить** , чтобы добавить новый файл для трассировки.
4. В области добавить файл Windows для Отслеживание изменений введите сведения о файле для трассировки и нажмите кнопку **сохранить**. В следующей таблице определены свойства, которые можно использовать для получения сведений.

    |Свойство  |Описание  |
    |---------|---------|
    |Активировано     | Значение true, если параметр применен, и false в противном случае.        |
    |Имя элемента     | Понятное имя файла для отслеживания.        |
    |Группа     | Имя группы для логического группирования файлов.        |
    |Укажите путь     | Путь для проверки файла, например **\\\*c:\temp. txt**. Можно также использовать переменные среды, такие как `%winDir%\System32\\\*.*`.       |
    |Тип пути     | Тип пути. Возможными значениями являются File и Directory.        |    
    |Рекурсия     | Значение true, если рекурсия используется при поиске элемента для отслеживания, и false в противном случае.        |    
    |Отправить содержимое файла | Значение true, чтобы передать содержимое файла при отслеживании изменений, и false в противном случае.|

5. Убедитесь, что указано значение true для **отправки содержимого файла**. Этот параметр включает отслеживание содержимого файлов для указанного пути к файлу.

### <a name="configure-file-tracking-on-linux"></a>Настройка отслеживания файлов в Linux

Чтобы настроить отслеживание файла на компьютере с Linux, сделайте следующее:

1. В учетной записи службы автоматизации выберите **Отслеживание изменений** в разделе **Управление конфигурацией**. 
2. Щелкните **Изменить параметры** (символ шестеренки).
3. На странице Конфигурация рабочей области выберите **файлы Linux**, а затем нажмите кнопку **+ Добавить** , чтобы добавить новый файл для трассировки.
4. На панели добавить файл Linux для Отслеживание изменений введите сведения о файле или каталоге для трассировки и нажмите кнопку **сохранить**. В следующей таблице определены свойства, которые можно использовать для получения сведений.

    |Свойство  |Описание  |
    |---------|---------|
    |Активировано     | Значение true, если параметр применен, и false в противном случае.        |
    |Имя элемента     | Понятное имя файла для отслеживания.        |
    |Группа     | Имя группы для логического группирования файлов.        |
    |Укажите путь     | Путь для проверки файла, например **/etc/*. conf**.       |
    |Тип пути     | Тип пути. Возможными значениями являются File и Directory.        |
    |Рекурсия     | Значение true, если рекурсия используется при поиске элемента для отслеживания, и false в противном случае.        |
    |Использовать sudo     | Значение true, если необходимо использовать sudo при проверке элемента, и false в противном случае.         |
    |Ссылки     | Параметр, определяющий, как обрабатывать символьные ссылки при просмотре каталогов. Возможны следующие значения:<br> Пропустить — игнорирование символических ссылок без включения ссылочных файлов и каталогов.<br>Далее следуют символические ссылки во время рекурсии, а также включает файлы и каталоги, на которые имеются ссылки.<br>Управлять — переход по символическим ссылкам и разрешение изменения полученного содержимого. **Примечание** . Этот вариант не рекомендуется, так как он не поддерживает извлечение содержимого файлов.    |
    |Отправить содержимое файла | Значение true, чтобы передать содержимое файла при отслеживании изменений, и false в противном случае. |

5. Убедитесь, что указано значение true для **отправки содержимого файла**. Этот параметр включает отслеживание содержимого файлов для указанного пути к файлу.

   ![Добавить файл Linux](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="track-file-contents"></a>Запись содержимого файла

Отслеживание содержимого файлов позволяет просматривать содержимое файла до и после отслеженного изменения. Эта функция сохраняет содержимое файла в учетной записи хранения после каждого изменения. Ниже приведены некоторые правила, которые необходимо выполнить для отслеживания содержимого файла.

* Для хранения содержимого файла требуется стандартная учетная запись хранения с моделью развертывания Resource Manager. 

* Не используйте учетные записи хранения Premium и классической модели развертывания. См. раздел [об учетных записях хранения Azure](../storage/common/storage-create-storage-account.md).

* Используемая учетная запись хранения может быть подключена только к одной учетной записи службы автоматизации.

* [Отслеживание изменений и Инвентаризация](change-tracking.md) включены в учетной записи службы автоматизации.

### <a name="enable-tracking-for-file-content-changes"></a>Включить отслеживание изменений содержимого файлов

1. В портал Azure откройте учетную запись службы автоматизации, а затем выберите **Отслеживание изменений** в разделе **Управление конфигурацией**.
2. Щелкните **Изменить параметры** (символ шестеренки).
3. Выберите **Содержимое файла** и нажмите **Ссылка**. При выборе этого варианта откроется диалоговое окно Добавление расположения содержимого для Отслеживание изменений.

   ![Включить расположение содержимого](./media/change-tracking-file-contents/enable.png)

4. Выберите подписку и учетную запись хранения, которые следует использовать для хранения содержимого файлов. 

5. Если вы хотите включить отслеживание содержимого файла для всех существующих отслеживаемых файлов, выберите **Вкл.** для пункта **Отправка содержимого файла для всех параметров**. Этот параметр можно изменить для каждого пути к файлу позже.

   ![Задать учетную запись хранения](./media/change-tracking-file-contents/storage-account.png)

6. Отслеживание изменений и Inventory отображает учетную запись хранения и URI подписанного URL-адреса (SAS), когда он включает отслеживание изменений содержимого файлов. Срок действия подписей истекает через 365 дней. их можно повторно создать, нажав кнопку **повторно создать**.

   ![Вывод ключей учетной записи](./media/change-tracking-file-contents/account-keys.png)

### <a name="view-the-contents-of-a-tracked-file"></a>Просмотр содержимого записанного файла

После того, как Отслеживание изменений и Инвентаризация обнаружит изменение для отслеживающего файла, содержимое файла можно просмотреть на панели сведения об изменении.  

![Список изменений](./media/change-tracking-file-contents/change-list.png)

1. В портал Azure откройте учетную запись службы автоматизации, а затем выберите **Отслеживание изменений** в разделе **Управление конфигурацией**.

2. Выберите файл в списке изменений и щелкните **Просмотреть изменения содержимого файла** , чтобы просмотреть содержимое файла. В области сведения об изменении отображаются стандартные сведения о файле до и после.

   ![Сведения об изменении](./media/change-tracking-file-contents/change-details.png)

3. Содержимое файла просматривается в параллельном представлении. Вы можете выбрать **встроенный** , чтобы увидеть встроенное представление изменений.

## <a name="track-registry-keys"></a>Трассировка разделов реестра

Чтобы настроить разделы реестра для отслеживания на компьютерах с Windows, сделайте следующее:

1. В учетной записи службы автоматизации выберите **Отслеживание изменений** в разделе **Управление конфигурацией**. 
2. Щелкните **Изменить параметры** (символ шестеренки).
3. На странице Конфигурация рабочей области выберите **реестр Windows**.
4. Нажмите кнопку **+ Добавить** , чтобы добавить новый раздел реестра для трассировки.
5. В области добавить реестр Windows для Отслеживание изменений введите сведения о ключе для трассировки и нажмите кнопку **сохранить**. В следующей таблице определены свойства, которые можно использовать для получения сведений.

    |Свойство  |Описание  |
    |---------|---------|
    |Активировано     | Значение true, если параметр применен, и false в противном случае.        |
    |Имя элемента     | Понятное имя раздела реестра для трассировки.        |
    |Группа     | Имя группы для логического группирования разделов реестра.        |
    |Раздел реестра Windows   | Имя ключа с путем, например **HKEY_LOCAL_MACHINE \Софтваре\микрософт\виндовс\куррентверсион\експлорер\усер Shell Folders\Common Startup**.      |

## <a name="search-logs-for-change-records"></a>Поиск записей об изменениях в журналах

Можно выполнять различные поисковые запросы к журналам Azure Monitor для записей об изменениях. Откройте страницу отслеживание изменений и нажмите кнопку **log Analytics** , чтобы открыть страницу журналы. В следующей таблице приведены примеры поисков по журналам для записей об изменениях.

|Запрос  |Описание  |
|---------|---------|
|ConfigurationData<br>&#124;, где Конфигдататипе = = "службы Майкрософт" и Свкстартуптипе = = "Auto"<br>&#124; where SvcState == "Stopped"<br>&#124; summarize arg_max(TimeGenerated, *) by SoftwareName, Computer         | Показывает последние записи инвентаризации для служб Майкрософт, для которых было задано значение Авто, но при этом они были переданы как останавливаемые. Результаты ограничены самой последней записью для указанного имени программного обеспечения и компьютера.    |
|ConfigurationChange<br>&#124; where ConfigChangeType == "Software" and ChangeCategory == "Removed"<br>&#124; order by TimeGenerated desc|Показывает записи об изменениях для удаленного программного обеспечения.|

## <a name="create-alerts-on-changes"></a>Создание оповещений об изменениях

В следующем примере показано, что файл **C:\windows\system32\drivers\etc\hosts** был изменен на компьютере. Этот файл важен, так как Windows использует его для разрешения имен узлов в IP-адреса. Эта операция имеет приоритет над DNS и может привести к проблемам с подключением. Это также может привести к перенаправлению трафика на вредоносные или другие опасные веб-сайты.

![Диаграмма, отображающая изменение файла hosts](./media/change-tracking-file-contents/changes.png)

Рассмотрим этот пример, чтобы обсудить шаги по созданию предупреждений при изменении.

1. В учетной записи службы автоматизации выберите **Отслеживание изменений** в разделе **Управление конфигурацией**, а затем выберите **log Analytics**. 
2. В поиске по журналам найдите изменения содержимого в файле **hosts** с помощью запроса `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`. Этот запрос ищет изменение содержимого для файлов с полным путем, содержащим слово "hosts". Можно также запросить определенный файл, изменив часть пути на полную форму, например с помощью `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`.

3. После того как запрос вернет нужные результаты, щелкните **создать правило генерации оповещений** в поиске по журналам, чтобы открыть страницу создания предупреждения. Также можно переходить к этой странице с помощью **Azure Monitor** в портал Azure. 

4. Снова проверьте запрос и измените логику оповещения. В этом случае вы хотите, чтобы оповещение активировалось, если на всех машинах среды обнаружено хотя бы одно изменение.

    ![Изменение запроса для отслеживания изменений в файле hosts](./media/change-tracking-file-contents/change-query.png)

5. После установки логики оповещения назначьте группам действий выполнение действий в ответ на срабатывание оповещения. В этом случае мы настраиваете отправляемые сообщения электронной почты и билет управления ИТ-службами (ITSM). 

    ![Настройка группы действий для предупреждения об изменении](./media/change-tracking/action-groups.png)

## <a name="next-steps"></a>Дальнейшие действия

* Основные сведения о Отслеживание изменений и инвентаризации см. в разделе [обзор отслеживание изменений и инвентаризации](change-tracking.md).
* Устранение неполадок, связанных с изменениями виртуальной машины Azure, см. в статье [Устранение неполадок отслеживание изменений и инвентаризации](troubleshoot/change-tracking.md).
* Используйте [Поиск по журналам в Azure Monitor журналов](../log-analytics/log-analytics-log-searches.md) для просмотра подробных данных отслеживания изменений.