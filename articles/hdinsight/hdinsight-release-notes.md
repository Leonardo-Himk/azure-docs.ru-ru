---
title: Заметки о выпуске Azure HDInsight
description: Последние заметки о выпуске Azure HDInsight. Получите советы по разработке и подробные сведения о Hadoop, Spark, R Server, Hive и других.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: 458925dd9f7f7386a9159256fdb024d027f7016c
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82929321"
---
# <a name="release-notes"></a>Заметки о выпуске

Эта статья содержит сведения о **последних** обновлениях выпуска Azure HDInsight. Дополнительные сведения о предыдущих выпусках см. в статье [Заметки о выпуске для компонентов Hadoop в Azure HDInsight (архив)](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Сводка

Azure HDInsight — одна из самых популярных служб среди корпоративных клиентов для аналитики с открытым кодом в Azure.

## <a name="release-date-01092020"></a>Дата выпуска: 01/09/2020

Этот выпуск применим как для HDInsight 3,6, так и для 4,0. Выпуск HDInsight предоставляется для всех регионов в течение нескольких дней. Дата выпуска указывает дату выпуска первого региона. Если вы не видите приведенных ниже изменений, подождите, пока выпуск выполнится в вашем регионе в течение нескольких дней.

> [!IMPORTANT]  
> Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в [статье об управлении версиями HDInsight](hdinsight-component-versioning.md).

## <a name="new-features"></a>Новые функции
### <a name="tls-12-enforcement"></a>Обязательное использование TLS 1.2
TLS и SSL являются протоколами шифрования, которые обеспечивает безопасность передачи данных по сети. Дополнительные сведения о [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0). HDInsight использует TLS 1,2 на общедоступных конечных точках HTTPs, но TLS 1,1 по-прежнему поддерживается для обеспечения обратной совместимости. 

В этом выпуске клиенты могут использовать TLS 1,2 только для всех подключений через конечную точку общедоступного кластера. Для этого новое свойство **минсуппортедтлсверсион** введено и может быть указано во время создания кластера. Если свойство не задано, кластер по-прежнему поддерживает TLS 1,0, 1,1 и 1,2, что соответствует текущему поведению. Клиенты могут задать для этого свойства значение "1,2". Это означает, что кластер поддерживает только TLS 1,2 и более поздние версии. Дополнительные сведения см. в разделе [безопасность транспортного уровня](./transport-layer-security.md).

### <a name="bring-your-own-key-for-disk-encryption"></a>Приведите собственный ключ к шифрованию диска
Все управляемые диски в HDInsight защищены с помощью шифрования службы хранилища Azure (SSE). По умолчанию данные на этих дисках шифруются ключами, управляемыми корпорацией Майкрософт. Начиная с этого выпуска, можно создание собственных ключей (BYOK) для шифрования дисков и управлять им с помощью Azure Key Vault. Шифрование BYOK — это одноэтапная конфигурация при создании кластера без дополнительных затрат. Просто зарегистрируйте HDInsight как управляемое удостоверение с Azure Key Vault и добавьте ключ шифрования при создании кластера. Дополнительные сведения см. в разделе [Шифрование диска, управляемого клиентом](https://docs.microsoft.com/azure/hdinsight/disk-encryption).

## <a name="deprecation"></a>Устаревшее
Нет устаревших версий для этого выпуска. Чтобы подготовиться к предстоящим устаревшим параметрам, см. статью [предстоящие изменения](#upcoming-changes).

## <a name="behavior-changes"></a>Изменения в поведении
Нет изменений в поведении для этого выпуска. Чтобы подготовиться к предстоящим изменениям, см. статью [предстоящие изменения](#upcoming-changes).

## <a name="upcoming-changes"></a>Предстоящие изменения
В будущих выпусках будут внесены следующие изменения. 

### <a name="deprecate-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>Устаревшие Spark 2,1 и 2,2 в кластере Spark HDInsight 3,6
Начиная с июля 1 2020 клиенты не смогут создавать новые кластеры Spark с Spark 2,1 и 2,2 в HDInsight 3,6. Существующие кластеры будут работать как без поддержки корпорации Майкрософт. Рассмотрите возможность перехода в Spark 2,3 с HDInsight 3,6 на Июнь 30 2020, чтобы избежать потенциальных прерываний системы и поддержки.

### <a name="deprecate-spark-23-in-hdinsight-40-spark-cluster"></a>Прекращение использования Spark 2,3 в кластере HDInsight 4,0 Spark
Начиная с июля 1 2020 клиенты не смогут создавать новые кластеры Spark с помощью Spark 2,3 в HDInsight 4,0. Существующие кластеры будут работать как без поддержки корпорации Майкрософт. Рассмотрите возможность перехода в Spark 2,4 в HDInsight 4,0 на Июнь 30 2020, чтобы избежать потенциальных прерываний системы и поддержки.

### <a name="deprecate-kafka-11-in-hdinsight-40-kafka-cluster"></a>Нерекомендуемый Kafka 1,1 в кластере HDInsight 4,0 Kafka
Начиная с июля 1 2020 клиенты не смогут создавать новые кластеры Kafka с Kafka 1,1 в HDInsight 4,0. Существующие кластеры будут работать как без поддержки корпорации Майкрософт. Рассмотрите возможность перехода на Kafka 2,1 в HDInsight 4,0 на Июнь 30 2020, чтобы избежать потенциальных прерываний системы и поддержки.

### <a name="hbase-20-to-216"></a>HBase 2,0 на 2.1.6
В предстоящем выпуске HDInsight 4,0 версия HBase будет обновлена с версии 2,0 до 2.1.6

### <a name="spark-240-to-244"></a>Spark 2.4.0 в 2.4.4
В предстоящем выпуске HDInsight 4,0 версия Spark будет обновлена с версии 2.4.0 до 2.4.4.

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Для головного узла требуется минимальная 4-ядерная виртуальная машина 
Для головного узла требуется минимальная 4-ядерная виртуальная машина, чтобы обеспечить высокий уровень доступности и надежность кластеров HDInsight. Начиная с апреля 6 2020 клиенты могут выбрать только 4-ядерную или более позднюю виртуальную машину в качестве головного узла для новых кластеров HDInsight. Существующие кластеры продолжат работать должным образом. 

### <a name="esp-spark-cluster-node-size-change"></a>Изменение размера узла кластера ESP Spark 
В следующем выпуске минимальный разрешенный размер узла для кластера ESP Spark изменится на Standard_D13_V2. Виртуальные машины серии A могут вызвать проблемы с кластером ESP из-за относительно нехватки ресурсов ЦП и памяти. Виртуальные машины серии A не рекомендуются для создания новых кластеров ESP.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Переход к масштабируемым наборам виртуальных машин Azure
Теперь HDInsight использует виртуальные машины Azure для инициализации кластера. В предстоящем выпуске HDInsight будет использовать масштабируемые наборы виртуальных машин Azure. См. Дополнительные сведения о масштабируемых наборах виртуальных машин Azure.

## <a name="bug-fixes"></a>Исправления ошибок
HDInsight продолжит повысить надежность и производительность кластера. 

## <a name="component-version-change"></a>Изменение версии компонента
Для этого выпуска не изменилась версия компонента. Текущие версии компонентов для HDInsight 4,0 AD HDInsight 3,6 можно найти здесь.

