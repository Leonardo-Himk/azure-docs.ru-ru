---
title: Что такое Azure Data Lake Storage 1-го поколения? | Документы Майкрософт
description: Обзор Data Lake Storage 1-го поколения (ранее назывался Azure Data Lake Store) и значения, предоставляемого другими хранилищами данных
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: twooley
ms.openlocfilehash: 99384374226fd89cfd672c6b4f851a1743db0764
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "67118805"
---
# <a name="what-is-azure-data-lake-storage-gen1"></a>Что такое Azure Data Lake Storage 1-го поколения?

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage 1-го поколения — это крупномасштабный репозиторий корпоративного уровня для рабочих нагрузок анализа больших данных. Azure Data Lake позволяет сохранять данные с любым размером, типом и скоростью приема в одном месте для эксплуатационной и исследовательской аналитики.

Data Lake Storage 1-го поколения доступно из Hadoop (имеется в кластере HDInsight) с помощью интерфейсов REST API, совместимых с WebHDFS. Он предназначен для включения анализа хранимых данных и настроен для обеспечения производительности сценариев анализа данных. Data Lake Storage 1-го поколения включает все возможности корпоративного уровня: безопасность, управляемость, масштабируемость, надежность и доступность.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

## <a name="key-capabilities"></a>Ключевые возможности

Ниже перечислены некоторые основные возможности Data Lake Storage 1-го поколения.

### <a name="built-for-hadoop"></a>Поддержка Hadoop

Data Lake Storage 1-го поколения — это Apache Hadoopная файловая система, совместимая с Hadoop распределенная файловая система (HDFS) и работающая с экосистемой Hadoop. Существующие приложения и службы HDInsight, использующие API-интерфейс WebHDFS, могут легко интегрироваться с Data Lake Storage 1-го поколения. Data Lake Storage 1-го поколения также предоставляет для приложений интерфейс REST, совместимый с WebHDFS.

Вы можете легко анализировать данные, хранящиеся в Data Lake Storage 1-го поколения, используя аналитические платформы Hadoop, такие как MapReduce или Hive. Вы можете подготавливать кластеры Azure HDInsight и настраивать их для прямого доступа к данным, хранящимся в Data Lake Storage 1-го поколения.

### <a name="unlimited-storage-petabyte-files"></a>Неограниченное пространство хранения, файлы петабайтного размера

Data Lake Storage 1-го поколения предоставляет неограниченное хранилище и может хранить разнообразные данные для аналитики. Он не накладывает никаких ограничений на размеры учетных записей, размеры файлов или объем данных, которые могут храниться в Data Lake. Размер отдельных файлов может варьироваться от килобайта до петабайтов. Данные хранятся надежно путем создания нескольких копий. Ограничение времени, в течение которого данные могут храниться в Data Lake, не ограничено.

### <a name="performance-tuned-for-big-data-analytics"></a>Настройки производительности для анализа больших данных

Data Lake Storage 1-го поколения создается для выполнения крупномасштабных аналитических систем, требующих большой пропускной способности для запроса и анализа больших объемов данных. В озере данных фрагменты файлов распределяются по нескольким отдельным серверам хранилища. Это повышает пропускную способность при параллельном чтении файла для проведения анализа данных.

### <a name="enterprise-ready-highly-available-and-secure"></a>Готовность к работе в Организации: высокая доступность и безопасность

Data Lake Storage 1-го поколения обладает доступностью и надежностью, соответствующими отраслевым стандартам. Надежность хранения данных обеспечивается созданием избыточных копий для защиты от любых непредвиденных сбоев.

Data Lake Storage 1-го поколения также обеспечивает безопасность корпоративного уровня для сохраненных данных. Дополнительные сведения см. в статье о [защите данных в Data Lake Storage 1-го поколения](#DataLakeStoreSecurity).

### <a name="all-data"></a>Все данные

Data Lake Storage 1-го поколения могут хранить любые данные в собственном формате, не требуя никаких предыдущих преобразований. Data Lake Storage 1-го поколения не требует определять схему перед загрузкой данных. Интерпретацию данных и определение схемы осуществляет конкретная аналитическая платформа во время анализа. Возможность хранить файлы произвольных размеров и форматов позволяет Data Lake Storage 1-го поколения управлять структурированными, частично структурированными и неструктурированными данными.

В Data Lake Storage 1-го поколения хранятся контейнеры для данных — папки и файлы. Вы действуете с хранимыми данными с помощью пакетов SDK, портал Azure и Azure PowerShell. Если данные помещаются в хранилище с помощью этих интерфейсов и используются соответствующие контейнеры, можно хранить данные любого типа. Data Lake Storage 1-го поколения обрабатывает сохраняемые данные без учета их типа.

## <a name="securing-data"></a><a name="DataLakeStoreSecurity"></a>Защита данных

Data Lake Storage 1-го поколения использует Azure Active Directory (Azure AD) для проверки подлинности и списки управления доступом (ACL) для управления доступом к данным.

| Компонент | Описание |
| --- | --- |
| Аутентификация |Data Lake Storage 1-го поколения интегрируется с Azure AD для управления удостоверениями и доступом для всех данных, хранящихся в Data Lake Storage 1-го поколения. Из-за интеграции Data Lake Storage 1-го поколения преимущества всех функций Azure AD, таких как многофакторная идентификация, условный доступ, управление доступом на основе ролей, мониторинг использования приложений, мониторинг безопасности и предупреждения и т. д. Data Lake Storage 1-го поколения поддерживает протокол OAuth 2.0 для аутентификации в интерфейсе REST. См. раздел [Проверка подлинности Data Lake Storage 1-го поколения](data-lakes-store-authentication-using-azure-active-directory.md).|
| Управление доступом |Data Lake Storage 1-го поколения обеспечивает контроль доступа за счет поддержки разрешений POSIX, предоставляемых протоколом WebHDFS. Можно включить списки управления доступом для корневой папки, вложенных папок и отдельных файлов. Дополнительные сведения о работе списков управления доступом в контексте Data Lake Storage 1-го поколения см. в разделе [Контроль доступа в Data Lake Storage 1-го поколения](data-lake-store-access-control.md). |
| Шифрование |Data Lake Storage 1-го поколения также обеспечивает шифрование данных, хранящихся в учетной записи. Параметры шифрования можно задать во время создания учетной записи Data Lake Storage 1-го поколения. Шифрование данных можно как включить, так и отключить. Дополнительные сведения см. в статье [Шифрование данных в Data Lake Storage 1-го поколения](data-lake-store-encryption.md). Инструкции по предоставлению конфигурации, связанной с шифрованием, см. в статье [Приступая к работе с Data Lake Storage 1-го поколения помощью портал Azure](data-lake-store-get-started-portal.md). |

Инструкции по защите данных в Azure Data Lake Storage 1-го поколения см. в [этой статье](data-lake-store-secure-data.md).

## <a name="application-compatibility"></a>Совместимость приложений

Data Lake Storage 1-го поколения совместимо с большинством компонентов с открытым исходным кодом в экосистеме Hadoop. Он также интегрируется с другими службами Azure. Чтобы узнать больше о том, как можно использовать Data Lake Storage 1-го поколения с компонентами с открытым кодом и другими службами Azure, используйте следующие ссылки:

- Ознакомьтесь со [списком приложений с открытым кодом, совместимых с Data Lake Storage 1-го поколения](data-lake-store-compatible-oss-other-applications.md).
- См. статью [Интеграция с другими службами Azure](data-lake-store-integrate-with-other-services.md) , чтобы понять, как использовать Data Lake Storage 1-го поколения с другими службами Azure для реализации более широкого спектра сценариев.
- См. статью о [сценариях работы с Data Lake Storage 1-го поколения](data-lake-store-data-scenarios.md), включая прием, обработку, загрузку и визуализацию данных.

## <a name="data-lake-storage-gen1-file-system"></a>Data Lake Storage 1-го поколения файловая система

Доступ к Data Lake Storage 1-го поколения можно получить с помощью файловой системы Азуредаталакефилесистем (adl://) в средах Hadoop (доступно в кластере HDInsight). Приложения и службы, использующие adl://, могут использовать другие возможности оптимизации производительности, которые в настоящее время недоступны в HDFS. В результате Data Lake Storage 1-го поколения предоставляет гибкие возможности для использования лучшей производительности с рекомендуемым вариантом использования adl://или поддержки существующего кода, продолжая использовать API-интерфейсы HDFS напрямую. Azure HDInsight использует все возможности AzureDataLakeFilesystem для обеспечения максимальной производительности в Data Lake Storage 1-го поколения.

Для доступа к данным в Data Lake Storage 1-го поколения можно использовать `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Дополнительные сведения о доступе к данным в Data Lake Storage 1-го поколения см. в разделе [Просмотр свойств сохраненных данных](data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Следующие шаги

- [Приступая к работе с Data Lake Storage 1-го поколения с помощью портал Azure](data-lake-store-get-started-portal.md)
- [Приступая к работе с Data Lake Storage 1-го поколения с помощью пакета SDK для .NET](data-lake-store-get-started-net-sdk.md)
- [Создание кластеров HDInsight, использующих Data Lake Store, с помощью портала Azure](data-lake-store-hdinsight-hadoop-use-portal.md)