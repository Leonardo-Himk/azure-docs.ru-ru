---
title: Восстановление файлов и папок из резервной копии виртуальной машины Azure
description: Из этой статьи вы узнаете, как восстанавливать файлы и папки из точки восстановления виртуальной машины Azure.
ms.topic: conceptual
ms.date: 03/01/2019
ms.openlocfilehash: 0e3061ea8fc26adcf39fe415cd9a662de739543a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79273310"
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Восстановление файлов из резервной копии виртуальной машины Azure

Azure Backup предоставляет возможность восстановления [виртуальных машин и дисков Azure](./backup-azure-arm-restore-vms.md) из резервных копий виртуальных машин Azure, также известных как точки восстановления. В этой статье описывается восстановление файлов и папок из резервной копии виртуальной машины Azure. Восстановление файлов и папок доступно для всех виртуальных машин Azure, развернутых по модели с помощью Resource Manager и защищенных в хранилище служб восстановления.

> [!NOTE]
> Эта функция доступна для всех виртуальных машин Azure, развернутых посредством модели с помощью Resource Manager и защищенных в хранилище служб восстановления.
> Восстановление файлов из зашифрованной резервной копии виртуальной машины не поддерживается.
>

## <a name="mount-the-volume-and-copy-files"></a>Подключение тома и копирование файлов

Чтобы восстановить файлы и папки из точки восстановления, перейдите на виртуальную машину и выберите нужную точку восстановления.

1. Войдите на [портал Azure](https://portal.Azure.com) и в области слева щелкните **Виртуальные машины**. В списке виртуальных машин выберите виртуальную машину, чтобы открыть ее панель мониторинга.

2. В меню виртуальной машины щелкните **Архивация**, чтобы открыть панель мониторинга архивации.

    ![Открытие архивируемого элемента в хранилище служб восстановления](./media/backup-azure-restore-files-from-vm/open-vault-for-vm.png)

3. В меню панели мониторинга резервной копии щелкните **Восстановление файлов**.

    ![Кнопка восстановления файлов](./media/backup-azure-restore-files-from-vm/vm-backup-menu-file-recovery-button.png)

    Открывается меню **Восстановление файла**.

    ![Меню восстановления файлов](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

4. В раскрывающемся меню **Выберите точку восстановления** найдите и выберите точку восстановления, которая содержит требуемые файлы. По умолчанию здесь выбрана последняя из созданных точек восстановления.

5. Чтобы скачать программное обеспечение, используемое для копирования файлов из точки восстановления, щелкните **скачать исполняемый файл** (для виртуальных машин Windows Azure) или **загрузить скрипт** (для виртуальных машин Linux Azure создается скрипт Python).

    ![Созданный пароль](./media/backup-azure-restore-files-from-vm/download-executable.png)

    Azure скачает исполняемый файл или сценарий на локальный компьютер.

    ![сообщение о скачивании исполняемого файла или сценария](./media/backup-azure-restore-files-from-vm/run-the-script.png)

    Для запуска исполняемого файла или сценария от имени администратора рекомендуется сохранить скачанный файл на компьютере.

6. Исполняемый файл или сценарий защищен, и для него требуется ввести пароль. В меню **Восстановление файла** нажмите кнопку "Копировать" для загрузки пароля в память.

    ![Созданный пароль](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

7. Из расположения скачивания (обычно папка "Загрузки") щелкните исполняемый файл или сценарий правой кнопкой мыши и запустите его с учетными данными администратора. При появлении запроса введите пароль или вставьте пароль из памяти и нажмите клавишу **Ввод**. После ввода пароля сценарии подключаются к точке восстановления.

    ![Меню восстановления файлов](./media/backup-azure-restore-files-from-vm/executable-output.png)

8. Для компьютеров Linux создается скрипт Python. Один из них должен загрузить сценарий и скопировать его на соответствующий или совместимый сервер Linux. Может потребоваться изменить разрешения, чтобы выполнить его с помощью ```chmod +x <python file name>```. Затем запустите файл Python с ```./<python file name>```.

Чтобы убедиться, что сценарий выполнен успешно, обратитесь к разделу [требования к доступу](#access-requirements) .

### <a name="identifying-volumes"></a>Определение томов

#### <a name="for-windows"></a>Для Windows

При выполнении исполняемого файла операционная система подключает новые тома и назначает им буквы дисков. Для просмотра этих дисков вы можете использовать проводник Windows или проводник. Буквы дисков, назначенные томам, могут отличаться от букв исходной виртуальной машины. Однако имя тома сохраняется. Например, если том на исходной виртуальной машине имел значение "диск данных (E:`\`)", этот том можно подключить к локальному компьютеру как "диск данных (любая буква":`\`). Просмотрите все тома, указанные в выходных данных сценария, пока не найдете файлы или папки.  

   ![Меню восстановления файлов](./media/backup-azure-restore-files-from-vm/volumes-attached.png)

#### <a name="for-linux"></a>Для Linux

В Linux тома точки восстановления подключаются к папке, в которой выполняет сценарий. Отображаются подключенные диски, тома и соответствующие пути подключения. Эти пути подключения видимы для пользователей с доступом на корневом уровне. Просмотрите тома, указанные в выходных данных сценария.

  ![Меню восстановления файлов Linux](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)

## <a name="closing-the-connection"></a>Закрытие подключения

Когда вы найдете все нужные файлы и скопируете их в локальное хранилище, удалите или отключите дополнительные диски. Чтобы отключить диски, щелкните **Unmount Disks** (Отключить диски) в меню **Восстановление файлов** на портале Azure.

![Отключение дисков](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Когда диски будут отключены, появится сообщение об успешном завершении операции. На обновление состояния подключения может потребоваться несколько минут, после чего диски можно удалить.

В Linux после разрыва подключения к точке восстановления операционная система не удаляет соответствующие пути подключения автоматически. Пути подключения существуют как "потерянные" тома и видимы, но при доступе к ним и записи в них возникает ошибка. Их можно удалить вручную. При запуске сценарий определяет такие тома, оставшиеся от любой предыдущей точки восстановления, и очищает их после согласия пользователя.

## <a name="special-configurations"></a>Специальные конфигурации

### <a name="dynamic-disks"></a>Динамические диски

Если на защищенной виртуальной машине Azure есть тома, обладающие одной или всеми следующими характеристиками, то запустить на ней исполняемый сценарий не удастся.

- Тома, охватывающие несколько дисков (составные и чередующиеся тома)
- Отказоустойчивые тома (зеркальные тома и тома RAID-5) на динамических дисках

Вместо этого запустите исполняемый сценарий на другом компьютере с совместимой операционной системой.

### <a name="windows-storage-spaces"></a>Дисковое пространство Windows

Дисковые пространства Windows — это технология Windows, которая позволяет виртуализировать хранилище. С помощью дисковых пространств Windows можно сгруппировать стандартные отраслевые диски в пулы носителей. Затем доступное пространство в этих пулах носителей можно использовать для создания виртуальных дисков, называемых дисковыми пространствами.

Если защищенная виртуальная машина Azure использует дисковые пространства Windows, то запустить на ней исполняемый сценарий не удастся. Вместо этого запустите его на другом компьютере с совместимой операционной системой.

### <a name="lvmraid-arrays"></a>Массивы LVM/RAID

В Linux для управления логическими томами на нескольких дисках используется диспетчер логических томов (LVM) и (или) программные RAID-массивы. Если защищенная виртуальная машина Linux использует LVM и/или RAID-массивы, на ней нельзя запустить сценарий. Вместо этого запустите сценарий на любом другом компьютере с совместимой операционной системы, поддерживающей файловую систему защищенной виртуальной машины.

В следующих выходных данных сценария отображаются диски и тома LVM или RAID-массивов с типом раздела.

   ![Меню выходных данных для LVM в Linux](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)

Чтобы подключить эти разделы, выполните команды в следующих разделах.

#### <a name="for-lvm-partitions"></a>Для секций LVM

Чтобы вывести список имен групп томов в физическом томе:

```bash
#!/bin/bash
pvs <volume name as shown above in the script output>
```

Чтобы получить список всех логических томов, имен и их путей в группе томов, выполните следующие действия.

```bash
#!/bin/bash
lvdisplay <volume-group-name from the pvs command's results>
```

Чтобы подключить логические тома к выбранному пути, сделайте следующее:

```bash
#!/bin/bash
mount <LV path> </mountpath>
```

#### <a name="for-raid-arrays"></a>Для массивов RAID

Следующая команда отображает сведения обо всех дисках RAID:

```bash
#!/bin/bash
mdadm –detail –scan
```

 Соответствующий диск RAID будет отображен как `/dev/mdm/<RAID array name in the protected VM>`.

Используйте команду mount, если на диске RAID есть физические тома:

```bash
#!/bin/bash
mount [RAID Disk Path] [/mountpath]
```

Если на диске RAID настроен другой LVM, используйте предыдущую процедуру для разделов LVM, но вместо имени диска RAID используйте имя тома.

## <a name="system-requirements"></a>Требования к системе

### <a name="for-windows-os"></a>Windows 10

Следующая таблица содержит информацию о совместимости операционных систем сервера и компьютера. Нельзя восстановить файлы до предыдущей или будущей версии операционной системы. Например, нельзя восстановить файл с виртуальной машины Windows Server 2016 на компьютер Windows Server 2012 или Windows 8. Файлы можно восстановить с виртуальной машины в ту же серверную операционную систему или в совместимую клиентскую операционную систему.

|ОС сервера | Совместимые ОС клиента  |
| --------------- | ---- |
| Windows Server 2019    | Windows 10 |
| Windows Server 2016    | Windows 10 |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

### <a name="for-linux-os"></a>Для ОС Linux

В Linux операционная система компьютера, используемого для восстановления файлов, должна поддерживать файловую систему защищенной виртуальной машины. При выборе компьютера для выполнения сценария убедитесь, что он имеет совместимую ОС и использует одну из версий, приведенных в следующей таблице:

|ОС Linux | Версии  |
| --------------- | ---- |
| Ubuntu | 12.04 и выше |
| CentOS | 6.5 и выше  |
| RHEL | 6.7 и выше |
| Debian | 7 и выше |
| Oracle Linux | 6.4 и выше |
| SLES | 12 и выше |
| openSUSE | 42.2 и выше |

> [!NOTE]
> Мы обнаружили некоторые проблемы, связанные с запуском сценария восстановления файлов на компьютерах с ОС SLES 12 SP4, и мы изучаем группу SLES.
> Сейчас запуск скрипта восстановления файлов работает на компьютерах с версиями ОС SLES 12 SP2 и SP3.
>

Сценарию также требуется компоненты python и bash для выполнения операций и безопасного подключения к точке восстановления.

|Компонент | Версия  |
| --------------- | ---- |
| bash | 4 и выше |
| python | 2.6.6 и выше  |
| TLS | Должна поддерживаться версия 1.2  |

## <a name="access-requirements"></a>Требования для доступа

Если скрипт выполняется на компьютере с ограниченным доступом, убедитесь в наличии доступа к следующим ресурсам:

- `download.microsoft.com`
- URL-адреса служб восстановления (geo-name относится к региону, в котором расположено хранилище служб восстановления):
  - `https://pod01-rec2.geo-name.backup.windowsazure.com` (для общедоступных геообъектов Azure);
  - `https://pod01-rec2.geo-name.backup.windowsazure.cn`(Для Azure Китая 21Vianet)
  - `https://pod01-rec2.geo-name.backup.windowsazure.us` (Azure для государственных организаций США);
  - `https://pod01-rec2.geo-name.backup.windowsazure.de` (Azure для Германии).
- Исходящие порты 53 (DNS), 443, 3260

> [!NOTE]
>
> - Имя скачанного файла скрипта будет содержать **географическое имя** , которое будет заполнено URL-адресом. Для примере: имя скачанного скрипта начинается \'с\'\_\'VMname\'геоимени\'_\'GUID, например *ContosoVM_wcus_12345678*
> - URL-адрес будет <https://pod01-rec2.wcus.backup.windowsazure.com>"
>

Для Linux сценарию требуются компоненты open-iscsi и lshw для подключения к точке восстановления. Если компоненты не существуют на компьютере, где выполняется сценарий, сценарий запрашивает разрешение на установку компонентов. Согласитесь на установку необходимых компонентов.

Доступ к `download.microsoft.com` необходим для загрузки компонентов, используемых для создания безопасного канала между компьютером, на котором выполняется сценарий, и данными в точке восстановления.

Сценарий можно запустить на любом компьютере, операционная система на котором совпадает или совместима с операционной системой архивной виртуальной машины. Условия совместимости можно проверить в [таблице совместимых ОС](backup-azure-restore-files-from-vm.md#system-requirements). Если на защищенной виртуальной машине Azure используются дисковые пространства Windows (для виртуальных машин Windows в Azure) либо LVM или RAID-массивы (для виртуальных машин Linux), то запустить на ней исполняемый файл или сценарий не удастся. Вместо этого запустите его на другом компьютере с совместимой операционной системой.

## <a name="file-recovery-from-virtual-machine-backups-having-large-disks"></a>Восстановление файлов из резервных копий виртуальных машин с большими дисками

В этом разделе объясняется, как выполнять восстановление файлов из резервных копий виртуальных машин Azure с более чем 16 дисками, а размер каждого диска превышает 32 ТБ.

Так как процесс восстановления файлов присоединяет все диски из резервной копии, при использовании большого числа дисков (>16) или больших дисков (каждый > 32 ТБ) рекомендуется использовать следующие точки действий:

- Для восстановления файлов используйте отдельный сервер восстановления (виртуальные машины Azure D2v3 VM). Его можно использовать только для восстановления файлов, а затем завершать его работу, когда это не требуется. Восстановление на исходном компьютере не рекомендуется, так как оно оказывает значительное влияние на саму виртуальную машину.
- Затем запустите сценарий один раз, чтобы проверить успешность операции восстановления файла.
- Если процесс восстановления файла зависает (диски не подключены или подключены, но тома не отображаются), выполните следующие действия.
  - Если сервер восстановления является виртуальной машиной Windows:
    - Убедитесь, что ОС имеет значение WS 2012 или более поздней версии.
    - Убедитесь, что разделы реестра заданы на сервере восстановления, как показано ниже, и перезагрузите сервер. Число рядом с GUID может быть в диапазоне от 0001-0005. В следующем примере это 0004. Перейдите по пути к разделу реестра до раздела параметров.

    ![искси-рег-Кэй-чанжес. png](media/backup-azure-restore-files-from-vm/iscsi-reg-key-changes.png)

```registry
- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Disk\TimeOutValue – change this from 60 to 1200
- HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e97b-e325-11ce-bfc1-08002be10318}\0003\Parameters\SrbTimeoutDelta – change this from 15 to 1200
- HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e97b-e325-11ce-bfc1-08002be10318}\0003\Parameters\EnableNOPOut – change this from 0 to 1
- HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e97b-e325-11ce-bfc1-08002be10318}\0003\Parameters\MaxRequestHoldTime - change this from 60 to 1200
```

- Если сервер восстановления является виртуальной машиной Linux, выполните следующие действия.
  - В файле/ЕТК/искси/исксид.конф измените значение параметра с:
    - Node. conn [0]. Тимео. noop_out_timeout = 5 для Node. conn [0]. Тимео. noop_out_timeout = 30
- После внесения приведенных выше изменений запустите сценарий еще раз. С этими изменениями очень вероятно, что восстановление файлов завершится с ошибкой.
- Каждый раз, когда пользователь скачивает сценарий, Azure Backup инициирует процесс подготовки точки восстановления к скачиванию. Для больших дисков этот процесс займет значительное время. При наличии последовательных пакетов запросов целевая подготовка будет идти в спирали. Поэтому рекомендуется скачать скрипт с портала, PowerShell или интерфейса командной строки, подождать 20-30 минут (эвристика) и запустить ее. На этот раз целевой объект должен быть готов к подключению из скрипта.
- После восстановления файлов убедитесь, что вы вернитесь на портал и щелкните **отключить диски** для точек восстановления, в которых не удалось подключить тома. По сути, этот шаг позволяет очистить существующие процессы и сеансы и повысить вероятность восстановления.

## <a name="troubleshooting"></a>Устранение неполадок

Если при восстановлении файлов из виртуальных машин у вас возникнут проблемы, проверьте сведения, приведенные в следующей таблице.

| Сообщение об ошибке или ситуация | Возможные причины | Рекомендованное действие |
| ------------------------ | -------------- | ------------------ |
| Выходные данные exe: *перехвачено исключение при подключении к целевому объекту* | Сценарий не может получить доступ к точке восстановления    | Проверьте, удовлетворяет ли компьютер [предыдущим требованиям доступа](#access-requirements). |  
| Исполняемый файл возвращает сообщение об ошибке *The target has already been logged in via an ISCSI session* (Вход на целевое устройство уже выполнен через сеанс iSCSI). | Этот скрипт уже запущен на этом компьютере и диски уже подключены | Тома точки восстановления уже подключены. Их НЕЛЬЗЯ подключить с теми же буквами дисков, что и на исходной виртуальной машине. Просмотрите все доступные тома в обозревателе файлов для файла. |
| Выходные данные exe: *этот скрипт является недопустимым, так как диски были отключены через портал/превышен предел в 12 ч. Скачайте новый скрипт с портала.* |    Диски были отключены от портала или превышено 12-часовое ограничение | Этот исполняемый файл теперь является недопустимым и не может быть запущен. Если вы хотите получить доступ к файлам этой точки восстановления, посетите портал для получения нового exe-файла.|
| На компьютере, где выполняется exe-файл: новые тома не будут отключены после нажатия кнопки отключения | Инициатор iSCSI на компьютере не отвечает на запросы и не обновляет подключение к целевому объекту и сохраняет кэш. |  После нажатия кнопки **Отключить**, подождите несколько минут. Если новые тома не будут отключены, просмотрите все тома. Просмотр всех томов заставляет инициатора обновить подключение, а том отключается с сообщением об ошибке, что диск недоступен.|
| Выходные данные exe: сценарий успешно выполняется, но в выходных данных скрипта не отображается "новые тома подключены". |    Это временная проблема    | Тома будут уже присоединены. Попробуйте открыть их через проводник. Если вы используете один и тот же компьютер для выполнения сценариев каждый раз, попробуйте перезапустить компьютер, после чего список должен отобразиться в последующих запусках исполняемого файла. |
| В Linux: не удается просмотреть нужные тома. | Операционная система компьютера, на котором выполняется сценарий, не может распознать базовую файловую систему защищенной виртуальной машины. | Проверьте, является ли точка восстановления отказоустойчивой или совместимой с файлами. Если файл является совместимым, запустите скрипт на другом компьютере, ОС которого распознает файловую систему защищенной виртуальной машины. |
| В Windows: не удается просмотреть нужные тома | Возможно, диски подключены, но тома не настроены | На экране управления дисками определите дополнительные диски, относящиеся к точке восстановления. Если какой-либо из этих дисков находится в автономном состоянии, попробуйте подключить их к сети, щелкнув диск правой кнопкой мыши и выбрав пункт в **сети**.|

## <a name="security"></a>Безопасность

В этом разделе обсуждаются различные меры безопасности, предпринятые для реализации восстановления файлов из резервных копий виртуальных машин Azure.

### <a name="feature-flow"></a>Поток функций

Эта функция была создана для доступа к данным виртуальной машины без необходимости восстановления всей виртуальной машины или дисков виртуальной машины, а также минимального числа шагов. Доступ к данным виртуальной машины обеспечивается сценарием (который подключает том восстановления при запуске, как показано ниже), и служит основой всех реализаций безопасности.

  ![Поток функций безопасности](./media/backup-azure-restore-files-from-vm/vm-security-feature-flow.png)

### <a name="security-implementations"></a>Реализации безопасности

#### <a name="select-recovery-point-who-can-generate-script"></a>Выбор точки восстановления (которая может создавать скрипт)

Сценарий предоставляет доступ к данным виртуальной машины, поэтому важно контролировать, кто может создать ее в первую очередь. Для создания скрипта необходимо войти в портал Azure и обеспечить [разрешение RBAC](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) .

Для восстановления файлов требуется тот же уровень авторизации, что и для восстановления виртуальной машины и восстановления дисков. Иными словами, только полномочные пользователи могут просматривать данные виртуальной машины, чтобы создать скрипт.

Созданный скрипт подписывается официальным сертификатом Майкрософт для службы Azure Backup. Любое изменение сценария означает, что подпись нарушена, а любая попытка выполнить сценарий выделена как потенциальное воздействие операционной системы.

#### <a name="mount-recovery-volume-who-can-run-script"></a>Подключение тома восстановления (который может выполнять скрипт)

Только администратор может запустить сценарий, и он должен работать в режиме с повышенными правами. Сценарий запускает только предварительно сформированный набор шагов и не принимает входные данные из какого-либо внешнего источника.

Чтобы выполнить сценарий, требуется пароль, который отображается только для пользователя, который является полномочным, во время создания скрипта в портал Azure или PowerShell/CLI. Это необходимо для того, чтобы полномочный пользователь, загружающий скрипт, также отвечал за выполнение сценария.

#### <a name="browse-files-and-folders"></a>Обзор файлов и папок

Для просмотра файлов и папок скрипт использует инициатор iSCSI на компьютере и подключается к точке восстановления, настроенной в качестве цели iSCSI. Здесь можно представить сценарии, в которых одна из них пытается имитировать или подменить все компоненты.

Мы используем механизм взаимной проверки подлинности CHAP, чтобы каждый компонент проходил проверку подлинности другого. Это означает, что очень трудно имитировать подключение фиктивного инициатора к цели iSCSI и присоединить фиктивный целевой объект к компьютеру, на котором выполняется сценарий.

Поток данных между службой восстановления и компьютером защищается путем создания защищенного туннеля TLS по протоколу TCP ([TLS 1,2 должен поддерживаться](#system-requirements) на компьютере, где выполняется сценарий).

Любой список управления доступом к файлам (ACL), имеющийся на виртуальной машине (родительской или резервной), также сохраняется в подключенной файловой системе.

Скрипт предоставляет доступ только для чтения к точке восстановления и действует только в течение 12 часов. Если вы хотите удалить доступ ранее, войдите на портал Azure, PowerShell или CLI и выполните **Отключение дисков** для этой точки восстановления. Скрипт будет недействителен немедленно.

## <a name="next-steps"></a>Дальнейшие шаги

- Сведения о проблемах при восстановлении файлов см. в разделе [Устранение неполадок](#troubleshooting) .
- Сведения о [восстановлении файлов с помощью PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#restore-files-from-an-azure-vm-backup)
- Узнайте, как [восстановить файлы с помощью Azure CLI](https://docs.microsoft.com/azure/backup/tutorial-restore-files)
- После восстановления виртуальной машины Узнайте, как [управлять резервным копированием](https://docs.microsoft.com/azure/backup/backup-azure-manage-vms) .
