---
title: Резервное копирование фермы SharePoint в Azure с помощью DPM
description: В этой статье описывается защита фермы SharePoint в Azure с помощью DPM или сервера службы архивации Azure
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 0199495e3b0eb002e58c096ed9abf05d46f43f97
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80054108"
---
# <a name="back-up-a-sharepoint-farm-to-azure-with-dpm"></a>Резервное копирование фермы SharePoint в Azure с помощью DPM

Архивация SharePoint в Microsoft Azure с помощью System Center Data Protection Manager (DPM) во многом напоминает архивацию других источников данных. Служба архивации Azure позволяет гибко планировать архивацию, задавая ежедневные, еженедельные, ежемесячные или ежегодные точки архивации, и предоставляет параметры политики хранения для любой из этих точек. DPM дает возможность сохранять копии локальных дисков для краткосрочных целей времени восстановления, а также сохранять копии в Azure для экономичного и длительного хранения.

Резервное копирование SharePoint в Azure с помощью DPM — очень похожий процесс резервного копирования SharePoint в DPM локально. Конкретные рекомендации для Azure будут указаны в этой статье.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>Поддерживаемые версии SharePoint и соответствующие сценарии защиты

Список поддерживаемых версий SharePoint и версий DPM, необходимых для их резервного копирования, см. в статье [Данные, поддерживаемые для резервного копирования с помощью DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2019#applications-backup).

## <a name="before-you-start"></a>Перед началом работы

Перед архивацией фермы SharePoint в Azure необходимо выполнить некоторые действия.

### <a name="prerequisites"></a>Предварительные условия

Прежде чем продолжить, выполните все [предварительные требования по использованию службы Microsoft Azure Backup](backup-azure-dpm-introduction.md#prerequisites-and-limitations) для защиты рабочих нагрузок. В частности, необходимо создать хранилище резервных копий, скачать учетные данные хранилища, установить агент службы Azure Backup и зарегистрировать DPM и Azure Backup Server в хранилище.

Дополнительные предварительные требования и ограничения можно найти в статье [резервное копирование SharePoint с помощью DPM](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#prerequisites-and-limitations) .

## <a name="configure-backup"></a>Настройка резервного копирования

Для реализации резервного копирования фермы SharePoint необходимо настроить защиту для SharePoint с помощью средства ConfigureSharePoint.exe, а затем создать группу защиты в DPM. Инструкции см. в разделе [Настройка резервного копирования](https://docs.microsoft.com//system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#configure-backup) в документации по DPM.

## <a name="monitoring"></a>Мониторинг

Чтобы отслеживать задание резервного копирования, следуйте инструкциям в разделе [мониторинг резервного копирования DPM](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#monitoring) .

## <a name="restore-sharepoint-data"></a>Восстановление данных SharePoint

Сведения о восстановлении элемента SharePoint с диска с помощью DPM см. в разделе [RESTORE SharePoint Data](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#restore-sharepoint-data).

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>Восстановление базы данных SharePoint из Azure с помощью DPM

1. Чтобы восстановить базу данных контента SharePoint, просмотрите различные точки восстановления (как было показано ранее) и выберите желаемую точку восстановления.

    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Дважды щелкните точку восстановления SharePoint, чтобы отобразить доступные регистрационные сведения SharePoint.

   > [!NOTE]
   > Так как для фермы SharePoint предусмотрен длительный срок хранения в Azure, на сервере DPM нет доступных сведений о каталоге (метаданных). В результате каждый раз, когда базу данных контента SharePoint требуется вернуть в состояние на определенный момент, необходимо каталогизировать ферму SharePoint заново.
   >
   >
3. Нажмите кнопку **Повторить каталогизацию**.

    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    Откроется окно состояния **Повторная каталогизация облака** .

    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    После завершения каталогизации состояние изменится на *Успешно*. Нажмите кнопку **Закрыть**.

    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. Щелкните объект SharePoint на вкладке DPM **Восстановление** , чтобы получить структуру базы данных контента. Щелкните элемент правой кнопкой мыши и выберите **Восстановить**.

    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Выполните процедуру восстановления, описанную ранее в этой статье, чтобы восстановить базу данных содержимого SharePoint с диска.

## <a name="switching-the-front-end-web-server"></a>Изменение интерфейсного веб-сервера

Если у вас есть несколько интерфейсных веб-серверов и вы хотите переключить сервер, используемый DPM для защиты фермы, следуйте инструкциям в разделе [Переключение внешнего веб-сервера](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#switching-the-front-end-web-server).

## <a name="next-steps"></a>Дальнейшие шаги

* [Azure Backup Server и DPM — часто задаваемые вопросы](backup-azure-dpm-azure-server-faq.md)
* [Устранение неполадок при работе с System Center Data Protection Manager](backup-azure-scdpm-troubleshooting.md)
