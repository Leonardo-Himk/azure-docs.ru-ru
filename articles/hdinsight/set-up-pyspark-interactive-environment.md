---
title: PySpark интерактивной среды с помощью средств Azure HDInsight
description: Сведения о создании и отправке запросов и скриптов с помощью средств Azure HDInsight для Visual Studio Code.
keywords: VScode,средства Azure HDInsight,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/23/2020
ms.openlocfilehash: 47f774f7b848e34aa7dc219ee4b08fb083cbd813
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188214"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Настройка интерактивной среды PySpark для Visual Studio Code

Следующие шаги показывают, как настроить интерактивную среду PySpark в VS Code.

Мы используем команду **python или pip** для сборки виртуального окружения в основной папке. Если вы хотите использовать другую версию, вам нужно вручную изменить версию по умолчанию команды **python или pip**. Дополнительные сведения см. в статье о команде [update-alternatives](https://linux.die.net/man/8/update-alternatives).

1. Установите [Python](https://www.python.org/downloads/) и [PIP](https://pip.pypa.io/en/stable/installing/).

   * Установите Python из [https://www.python.org/downloads/](https://www.python.org/downloads/).
   * Установите PIP с [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) (если он не установлен из установки Python).
   * Убедитесь, что Python и PIP установлены успешно с помощью следующих команд. (необязательно)

        ![Проверка команды версии для Python PIP](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > Рекомендуется вручную установить Python вместо использования версии macOS по умолчанию.

2. Установите **virtualenv**, выполнив команду, приведенную ниже.

   ```bash
   pip install virtualenv
   ```

## <a name="other-packages"></a>другие пакеты.

Если вы поступаете через сообщение об ошибке, установите необходимые пакеты, выполнив следующие команды:

   ![Установка пакета libkrb5 для Python](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```bash
sudo apt-get install libkrb5-dev
```

```bash
sudo apt-get install python-dev
```

Перезапустите VS Code, а затем снова перейдите к редактору сценариев, который работает на базе **интерактивной среды PySpark службы HDInsight**.

## <a name="next-steps"></a>Дальнейшие действия

### <a name="demo"></a>Демонстрация

* HDInsight для VS Code: [видео](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Средства и расширения

* [Использование средств Azure HDInsight для Visual Studio Code](hdinsight-for-vscode.md)
* [Создание приложений Apache Spark для кластера HDInsight с помощью Azure Toolkit for IntelliJ](spark/apache-spark-intellij-tool-plugin.md)
* [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](spark/apache-spark-jupyter-notebook-install-locally.md)
