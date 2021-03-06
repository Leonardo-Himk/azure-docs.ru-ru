---
title: Заметки о выпуске Шлюз Azure Data Box общей доступности | Документация Майкрософт
description: Описание критических открытых проблем и способов их устранения для Шлюз Azure Data Box, в котором выводится общедоступная версия.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/26/2019
ms.author: alkohli
ms.openlocfilehash: 1a8a9840cc6e1f3627c5fbd30e0b7432db0f16e4
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82561047"
---
# <a name="azure-data-box-edgeazure-data-box-gateway-general-availability-release-notes"></a>Заметки о выпуске общей доступности Azure Data Box Edge/Шлюз Azure Data Box

## <a name="overview"></a>Обзор

В следующих заметках о выпуске указываются критические открытые проблемы и разрешенные проблемы для выпуска общедоступной версии для Azure Data Box Edge и Шлюз Azure Data Box. 

Заметки о выпуске постоянно обновляются: обнаруживаются и добавляются критические проблемы, требующие временного решения. Перед развертыванием Data Box Edge или Шлюз Data Box внимательно проверьте сведения, содержащиеся в заметках о выпуске.

Общедоступный выпуск соответствует версиям программного обеспечения:

- **Шлюз Data Box 1903 (1.5.814.447)**
- **Data Box Edge 1903 (1.5.814.447)**


## <a name="whats-new"></a>Новые возможности

- **Новые образы виртуальных дисков** . теперь в портал Azure доступны новые VHDX-файлы и VMDK. Загрузите эти образы, чтобы подготавливать, настраивать и развертывать новые Шлюз Data Box общедоступные устройства. Устройства шлюза Data Box, созданные в более ранних предварительных выпусках, не удается обновить до этой версии. Дополнительные сведения см. в разделе [Подготовка к развертыванию шлюза Azure Data Box](data-box-gateway-deploy-prep.md).
- **Поддержка NFS** . Поддержка NFS доступна в предварительной версии и доступна для клиентов версии 3.0 и v 4.1, обращающихся к устройствам Data Box Edge и шлюз Data Box.
- **Устойчивость хранилища** . устройство Data Box Edge может выдержать сбой одного диска данных с функцией устойчивости хранилища. Эта функция в настоящее время находится на стадии предварительной версии. Чтобы включить устойчивость хранилища, выберите параметр **отказоустойчивость** в **параметрах хранилища** в локальном веб-интерфейсе.


## <a name="known-issues-in-ga-release"></a>Известные проблемы в общедоступном выпуске

В следующей таблице приведены сводные сведения об известных проблемах, возникающих при работе с Шлюз Data Boxным выпуском.

| Нет. | Компонент | Проблема | Временное решение и комментарии |
| --- | --- | --- | --- |
| **1.** |Типы файлов | Следующие типы файлов не поддерживаются: символьные файлы, блочные файлы, сокеты, каналы, символические ссылки.  |Копирование этих файлов приведет к созданию пустых файлов в общей папке NFS. Эти файлы находятся в состоянии ошибки, а данные о них передаются в файл *error.xml*. <br> Символические ссылки на каталоги приводят к тому, что каталоги никогда не помечаются в автономном режиме. И как результат — вы можете не увидеть серый крест на каталогах, который указывает на то, что каталоги отключены и все связанное содержимое полностью отправлено в Azure. |
| **2.** |Удаление | Из-за ошибки в этом выпуске при удалении общего ресурса NFS, он может быть не удален. Состояние общего ресурса отобразится как *Удаление*.  |Это происходит только в том случае, если общий ресурс использует имя файла, которое не поддерживается. |
| **3.** |Копировать | Копирование данных завершается с ошибкой: запрошенная операция не может быть завершена из-за ограничения файловой системы.  |Альтернативный поток данных (ADS), связанный с размером файла более 128 КБ, не поддерживается.   |


## <a name="next-steps"></a>Дальнейшие действия

- [Подготовка к развертыванию шлюза Azure Data Box](data-box-gateway-deploy-prep.md).
- [Подготовка к развертыванию Azure Data Box Edge](azure-stack-edge-deploy-prep.md).
