---
title: Поддерживаемые хранилища данных в общей папке данных Azure
description: Сведения о хранилищах данных, которые поддерживаются для использования общей папки данных Azure.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 624bb45de3e2ff184326949611d437f71f3e2def
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79501808"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Поддерживаемые хранилища данных в общей папке данных Azure

Общая папка данных Azure предоставляет доступ к открытым и гибким данным, включая возможность совместного использования с различными хранилищами данных. Поставщики данных могут обмениваться данными из одного типа хранилища данных, и их потребители данных могут выбрать хранилище данных для получения данных. 

В этой статье вы узнаете о обширном наборе хранилищ данных Azure, которые поддерживаются в общем ресурсе данных Azure. Также можно найти сведения о комбинациях хранилищ данных, которые можно использовать поставщиками данных и потребителями данных. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Какие хранилища данных поддерживаются в общем ресурсе данных Azure? 

В таблице ниже приведены поддерживаемые источники данных для общей папки данных Azure. 

| Хранилище данных | Общий доступ на основе моментальных снимков | Общий доступ на месте 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Хранилище BLOB-объектов Azure |✓ | |
| Хранилище Azure Data Lake Storage 1-го поколения |✓ | |
| Azure Data Lake Storage 2-го поколения |✓ ||
| База данных SQL Azure |Общедоступная предварительная версия | |
| Azure синапсе Analytics (ранее — хранилище данных SQL Azure) |Общедоступная предварительная версия | |
| Azure Data Explorer | |Общедоступная предварительная версия |

## <a name="data-store-support-matrix"></a>Матрица поддержки хранилища данных

Общая папка данных Azure предоставляет потребителям данных гибкие возможности при выборе хранилища данных для приема данных в. Например, данные, общие для базы данных SQL Azure, можно получить в Azure Data Lake Store Gen2, базе данных SQL Azure или Azure синапсе Analytics. Клиенты могут выбрать формат для получения данных при настройке полученного общего ресурса данных. 

В таблице ниже приведены различные сочетания и варианты, которые пользователи данных имеют при принятии и настройке общей папки данных. Дополнительные сведения о настройке сопоставлений наборов данных см. в разделе [Настройка сопоставлений наборов данных](how-to-configure-mapping.md).

|  | хранилище BLOB-объектов Azure | Хранилище Azure Data Lake Storage 1-го поколения | Azure Data Lake Storage 2-го поколения | База данных SQL Azure | Azure Synapse Analytics 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Хранилище BLOB-объектов Azure | ✓ || ✓|
| Хранилище Azure Data Lake Storage 1-го поколения | ✓ | | ✓|
| Azure Data Lake Storage 2-го поколения | ✓ | | ✓|
| База данных SQL Azure | ✓ | | ✓| ✓| ✓|
| Azure синапсе Analytics (ранее — хранилище данных SQL Azure) | ✓ | | ✓| ✓| ✓|

## <a name="share-from-a-storage-account"></a>Общий доступ из учетной записи хранения
Общая папка данных Azure поддерживает общий доступ к файлам, папкам и файловым системам Azure Data Lake Gen1 и Azure Data Lake Gen2. Он также поддерживает общий доступ к BLOB-объектам, папкам и контейнерам из хранилища BLOB-объектов Azure. В настоящее время поддерживается только блочный BLOB-объект. Когда папки используются совместно с общим доступом на основе моментальных снимков, потребитель данных может создать полную копию общих данных или использовать функцию добавочного моментального снимка для копирования только новых или обновленных файлов. Существующие файлы с тем же именем будут перезаписаны.

## <a name="share-from-a-sql-based-source"></a>Общий доступ из источника на основе SQL
Общий ресурс данных Azure поддерживает общий доступ к таблицам и представлениям из базы данных SQL Azure и Azure синапсе Analytics (ранее — хранилище Azure SQL). Потребители данных могут принять данные в Azure Data Lake Store Gen2 или хранилище BLOB-объектов Azure в формате CSV или Parquet. Обратите внимание, что по умолчанию форматы файлов — CSV. Потребитель данных может по желанию получить данные в формате Parquet. Это можно сделать в параметрах сопоставления набора данных при получении данных. 

При принятии данных в Azure Data Lake Store Gen2 или хранилище BLOB-объектов Azure полные моментальные снимки перезаписывают содержимое целевого файла. 

Потребитель данных может выбрать получение данных в таблицу по своему усмотрению. В этом случае, если Целевая таблица еще не существует, Общая папка данных Azure создает таблицу SQL с исходной схемой. Если целевая таблица с таким именем уже существует, она будет удалена и переписана с использованием последнего полного моментального снимка. При сопоставлении целевой таблицы можно указать альтернативную схему и имя таблицы. Добавочные моментальные снимки в настоящее время не поддерживаются. 

Общий доступ к источникам на основе SQL имеет предварительные требования, связанные с правилами брандмауэра и разрешениями. Дополнительные сведения см. в разделе "предварительные требования" руководства " [общий доступ к данным](share-your-data.md) ".

## <a name="share-from-azure-data-explorer"></a>Предоставление общего доступа к данным из Azure Data Explorer
Общий ресурс данных Azure поддерживает возможность совместного использования баз данных на месте из кластеров Azure обозреватель данных. Поставщик данных может совместно использоваться на уровне базы данных или кластера. При общем доступе на уровне базы данных потребитель данных может получить доступ только к тем базам данных, которые совместно используются поставщиком данных. При совместном использовании на уровне кластера потребитель данных может получить доступ ко всем базам данных из кластера поставщика, включая все будущие базы данных, созданные поставщиком данных.

Чтобы получить доступ к общим базам данных, потребитель данных должен иметь собственный кластер Azure обозреватель данных. Кластер Azure обозреватель данных для потребителя данных должен находиться в том же центре обработки данных Azure, что и кластер Azure обозреватель данных поставщика данных. Если установлено отношение общего доступа, Общая папка данных Azure создает символьную связь между поставщиком и кластерами Azure обозреватель данных потребителя.

Обозреватель данных Azure поддерживает два режима приема данных: пакетная обработка и потоковая передача. Данные, полученные из пакетной службы в общей базе данных, появятся между несколькими секундами на стороне потребителя данных. Для отображения данных, полученных из потоковой передачи, на стороне потребителя данных может потребоваться до 24 часов. 

## <a name="next-steps"></a>Дальнейшие шаги

Чтобы узнать, как приступить к обмену данными, перейдите к [этому](share-your-data.md) руководству.
