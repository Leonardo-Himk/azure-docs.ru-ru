---
title: Аутентификация пользователей — Java с Data Lake Storage 1-го поколения — Azure
description: Узнайте, как реализовать аутентификацию пользователей в Azure Data Lake Storage 1-го поколения с помощью Azure Active Directory и Java.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 1e03ad657fd40dce22a17f2fff5b67a65eb3eb52
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691766"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-java"></a>Аутентификация пользователей в Azure Data Lake Storage 1-го поколения с помощью Java
> [!div class="op_single_selector"]
> * [Использование Java](data-lake-store-end-user-authenticate-java-sdk.md)
> * [Использование пакета SDK для .NET](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Использование Python](data-lake-store-end-user-authenticate-python.md)
> * [Использование REST API](data-lake-store-end-user-authenticate-rest-api.md)
> 
>   

В этой статье описывается, как использовать пакет SDK для Java для аутентификации пользователей в Azure Data Lake Storage 1-го поколения. См. дополнительные сведения об [аутентификации между службами в Data Lake Storage 1-го поколения с помощью Java](data-lake-store-service-to-service-authenticate-java.md).

## <a name="prerequisites"></a>Предварительные условия
* **Подписка Azure**. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Создание собственного приложения Active Directory** Вам нужно выполнить инструкции по [аутентификации пользователей в Data Lake Storage 1-го поколения с помощью Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). В этом руководстве это средство используется для создания зависимостей проекта. Хотя зависимости можно создать и без использования таких систем, как Maven или Gradle, они существенно упрощают управление ими.

* Интегрированная среда разработки, например [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) или аналогичная (необязательно).

## <a name="end-user-authentication"></a>Проверка подлинности конечных пользователей
1. Создайте проект Maven с использованием [архетипа mvn](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) с помощью командной строки или интегрированной среды разработки (IDE). Инструкции по созданию проекта Java с использованием IntelliJ см. [здесь](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Инструкции по созданию проекта с использованием Eclipse см. [здесь](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).

2. Добавьте приведенные ниже зависимости в файл Maven **pom.xml**. Перед тегом ** \<>/Project** добавьте следующий фрагмент:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.2.3</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    Первая зависимость предназначена для использования пакета SDK для Data Lake Storage 1-го поколения (`azure-data-lake-store-sdk`) из репозитория Maven. Вторая зависимость нужна, чтобы указать, какие платформы ведения журналов (`slf4j-nop`) будут использоваться для этого приложения. Пакет SDK для Data Lake Storage 1-го поколения использует [SLF4J](https://www.slf4j.org/) Logging фасадной, который позволяет выбирать из ряда популярных платформ ведения журналов, таких как log4j, ведение журнала Java, Logback и т. д., а также без ведения журнала. В этом примере мы отключим ведение журнала, так как используем привязку **slf4j-nop**. Сведения о других вариантах ведения журнала в приложении см. [здесь](https://www.slf4j.org/manual.html#projectDep).

3. Добавьте следующие инструкции импорта в приложение.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.DeviceCodeTokenProvider;

4. Используйте следующий фрагмент кода в приложении Java, чтобы получить маркер для собственного приложения Active Directory, созданного ранее с помощью `DeviceCodeTokenProvider`. Замените часть **FILL-IN-HERE** фактическими значениями для собственного приложения Azure Active Directory.

        private static String nativeAppId = "FILL-IN-HERE";
            
        AccessTokenProvider provider = new DeviceCodeTokenProvider(nativeAppId);   

Пакет SDK для Data Lake Storage 1-го поколения предоставляет удобные методы управления маркерами безопасности, которые нужны для подключения к учетной записи Data Lake Storage 1-го поколения. Тем не менее пакет SDK не требует использования только этих методов. Можно использовать любые другие средства получения маркера, например [пакет SDK для Azure Active Directory](https://github.com/AzureAD/azure-activedirectory-library-for-java) или пользовательский код.

## <a name="next-steps"></a>Дальнейшие действия
В этой статье вы узнали, как использовать аутентификацию пользователей, чтобы реализовать аутентификацию в Data Lake Storage 1-го поколения с помощью пакета SDK для Java. Дополнительные сведения об использовании пакета SDK для Java для работы с Azure Data Lake Storage 1-го поколения см. в следующих статьях.

* [Операции с данными в Data Lake Storage 1-го поколения c использованием SDK для Java](data-lake-store-get-started-java-sdk.md)


