---
title: Метаданные OpenAPI в функциях Azure
description: Обзор поддержки OpenAPI в Функциях Azure
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: cbfd0e36307210851070c22e74acb0a858446ce1
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866718"
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Поддержка метаданных OpenAPI 2.0 в Функциях Azure (предварительная версия)
Поддержка метаданных OpenAPI 2.0 (прежнее название — Swagger) в Функциях Azure — это предварительная версия функции, которая предназначена для записи определения OpenAPI 2.0 в приложении-функции. Затем можно разместить этот файл с помощью приложения-функции.

> [!IMPORTANT]
> Предварительная версия OpenAPI доступна сейчас только в среде выполнения версии 1.x. Сведения о том, как создать приложение-функцию 1.x, см. [здесь](./functions-versions.md#creating-1x-apps).

[Метаданные OpenAPI](https://swagger.io/) позволяют использовать функцию, на которой размещен REST API во всевозможном программном обеспечении. Это программное обеспечение включает предложения Майкрософт, такие как PowerApps и [функцию "Приложения API" службы приложений Azure](../app-service/overview.md), средства сторонних разработчиков, например [Postman](https://www.getpostman.com/docs/importing_swagger), и [многие дополнительные пакеты](https://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>Мы рекомендуем начать с [учебника по началу работы](./functions-api-definition-getting-started.md), а затем вернуться к этому документу, чтобы узнать больше о конкретных функциях.

## <a name="enable-openapi-definition-support"></a><a name="enable"></a>Включение поддержки определения OpenAPI
Все параметры OpenAPI можно настроить на странице **Определение API** в приложении-функции **Функции платформы**.

> [!NOTE]
> Определение API функций сейчас не поддерживается для бета-версии среды выполнения.

Чтобы включить создание размещенного определения OpenAPI и кратких определений, задайте для параметра **Источник определения API** значение **Функция (предварительная версия)**. **Внешний URL-адрес** позволяет функции применять определение OpenAPI, размещенное в другом месте.

## <a name="generate-a-swagger-skeleton-from-your-functions-metadata"></a><a name="generate-definition"></a>Создание схемы Swagger на основе метаданных функции
Шаблон поможет вам приступить к написанию первого определения OpenAPI. Функция шаблона определения создает разреженное определение OpenAPI, используя все метаданные в файле function.json для каждой функции "Триггер HTTP". Вам необходимо будет указать дополнительные сведения об API из [спецификации OpenAPI](https://swagger.io/specification/), например шаблоны запросов и ответов.

Пошаговые инструкции см. в статье [Создание метаданных OpenAPI 2.0 (Swagger) для приложения-функции (предварительная версия)](./functions-api-definition-getting-started.md).

### <a name="available-templates"></a><a name="templates"></a>Доступные шаблоны

|Имя| Описание |
|:-----|:-----|
|Созданное определение|Определение OpenAPI с максимальным объемом информации, которую можно извлечь из имеющихся метаданных функции.|

### <a name="included-metadata-in-the-generated-definition"></a><a name="quickstart-details"></a>Метаданные, включенные в созданное определение

В следующей таблице показано соответствие параметров портала Azure и данных в function.json согласно созданной схеме Swagger.

|Swagger.json|Пользовательский интерфейс портала|Function.json|
|:----|:-----|:-----|
|[Узел](https://swagger.io/specification/#fixed-fields-15)|**Настройки** > приложения**приложения App Service настройки** > **Обзор** > **URL**|*Не присутствует*
|[Пути](https://swagger.io/specification/#paths-object-29)|**Интеграция** > **выбранных методов HTTP**|Bindings: Route
|[Path Item](https://swagger.io/specification/#path-item-object-32)|**Шаблон интеграции** > **маршрута**|Bindings: Methods
|[Безопасность](https://swagger.io/specification/#security-scheme-object-112)|**Ключи**|*Не присутствует*|
|operationID*|**Маршрут + допустимые команды**|Маршрут + допустимые команды|

\*Идентификатор операции необходим только для интеграции с PowerApps и Flow.
> [!NOTE]
> Расширение x-ms-summary предоставляет отображаемое имя в Logic Apps, PowerApps и Flow.
>
> Дополнительные сведения см. в статье [Customize your Swagger definition for PowerApps](https://docs.microsoft.com/connectors/custom-connectors/openapi-extensions) (Настройка определения Swagger для PowerApps).

## <a name="use-cicd-to-set-an-api-definition"></a><a name="CICD"></a>Создание определения API с помощью процесса непрерывной интеграции и доставки

 Прежде чем включать систему управления версиями для изменения из нее определения API, необходимо включить размещение определения API на портале. Выполните следующие действия:

1. Перейдите к **API Definition (preview)** (Определение API (предварительная версия)) в параметрах приложения-функции.
   1. Задайте для параметра **API definition source** (Источник определения API) значение **Функция**.
   1. Щелкните **Generate API definition template** (Создать шаблон определения API), а затем **Сохранить**, чтобы создать определение шаблона, которое можно будет изменить позже.
   1. Обратите внимание на URL-адрес и ключ определения API.
1. [Настройте непрерывную интеграцию и развертывание](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#requirements-for-continuous-deployment).
2. Измените файл swagger.json в системе управления исходным кодом по адресу \site\wwwroot\.azurefunctions\swagger\swagger.json.

Теперь изменения, внесенные в файл swagger.json в репозитории, размещаются приложением-функцией с использованием URL-адреса и ключа определения API, которые указаны на шаге 1.в.

## <a name="next-steps"></a>Дальнейшие действия
* [Начало начала учебник](functions-api-definition-getting-started.md). Попробуйте воспользоваться нашим пошаговым руководством, чтобы увидеть определения OpenAPI в действии.
* [Репозиторий Azure Функции GitHub](https://github.com/Azure/Azure-Functions/). Ознакомьтесь со сведениями о Функциях в репозитории и оставьте свои отзывы о предварительной версии средства поддержки определений API. Добавьте на GitHub сведения обо всех проблемах, которые необходимо устранить.
* [Ссылка разработчика Azure Functions](functions-reference.md). Сведения о написании кода функций и определении триггеров и привязок.
