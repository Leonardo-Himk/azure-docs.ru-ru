---
title: Краткое руководство. Развертывание приложения на портале LUIS
description: В этом кратком руководстве показано, как развернуть приложение путем создания ресурса конечной точки прогнозирования, назначения ресурса, обучения и публикации приложения.
ms.topic: quickstart
ms.date: 05/06/2020
ms.openlocfilehash: 9b22f1eda822b5354f0b434a04c2ea03e4c0ff2a
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585104"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Краткое руководство. Развертывание приложения на портале LUIS

Когда приложение LUIS будет готово возвращать прогнозы касательно речевых фрагментов в клиентское приложение (например, в чат-бот), его необходимо развернуть в конечной точке прогнозирования.

В рамках этого краткого руководства вы узнаете, как развернуть приложение. Вы создадите ресурс конечной точки прогнозирования, назначите его приложению, обучите и опубликуете это приложение.

## <a name="prerequisites"></a>Предварительные требования

* Оформите [подписку Azure](https://azure.microsoft.com/free).
* Выполните указания из [предыдущего краткого руководства по порталу](get-started-portal-build-app.md) или [скачайте и импортируйте приложение](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json).
* Если у вас есть приложения с более устаревшей проверкой подлинности, чем проверка подлинности ресурсов Azure, [перейдите на ресурс Azure](luis-migration-authoring.md). Некоторые страницы портала отличаются, когда действует проверка подлинности по электронной почте.

<a name="create-the-endpoint-resource"></a>

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>Назначение ключа ресурса для приложения LUIS на портале LUIS

Каждый раз при создании ресурса прогнозирующего запроса или разработки для LUIS необходимо назначить его для приложения LUIS. После этого вам не нужно будет еще раз выполнять этот шаг до создания следующего ресурса. Вы можете создать ресурс для расширения регионов приложения или для поддержки большего количества запросов прогнозирования.

1. Войдите на [портал LUIS](https://www.luis.ai) и выберите приложение **myEnglishApp** из списка приложений.

1. В верхнем правом меню выберите **Manage** (Управление), **Azure Resources** (Ресурсы Azure).

1. Чтобы добавить LUIS, выберите **Add prediction resource** (Добавить ресурс прогнозирования).

    ![Чтобы добавить ресурс прогнозирования LUIS, выберите Add prediction resource (Добавить ресурс прогнозирования).](./media/get-started-portal-deploy-app/azure-resources-add-prediction-resource.png)

1. Выберите клиент, подписку и группу ресурсов. Выберите **Assign resource** (Присвоить ресурс).

   ![Присвоение ресурса приложению](./media/get-started-portal-deploy-app/assign-resource.png)

1. Выполните те же действия, чтобы добавить в приложение ключ разработки.

1. Найдите в таблице добавленную строку для нового ресурса приложения и скопируйте URL-адрес конечной точки. Он построен правильно для того, чтобы отправлять запрос `HTTP GET` в конечную точку API LUIS для прогнозирования.

> [!TIP]
> Если вы планируете использовать активное обучение для улучшения приложения LUIS, выберите **Change query parameters** (Изменить параметры запроса), а затем выберите **Сохранить журналы**. Это действие изменяет URL-адрес примера, добавляя параметр `log=true` querystring. Скопируйте и используйте URL-адрес измененного примера запроса при выполнении прогнозирующих запросов к конечной точке среды выполнения.

## <a name="train-the-app"></a>Обучение приложения

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-the-prediction-endpoint"></a>Публикация приложения в конечной точке прогнозирования

[!INCLUDE [LUIS How to Train steps](includes/howto-publish.md)]

## <a name="prediction-endpoint-request"></a>Запрос конечной точки прогнозирования

На портале `query=` в конце URL-адреса обозначает место добавления речевого фрагмента пользователя в запрос GET. После `query=` введите речевой фрагмент пользователя, используемый в конце предыдущего краткого руководства:

```Is there a form named hrf-234098```

Убедитесь, что строка запроса содержит следующие пары:

* `show-all-intents=true`
* `verbose=true`

В браузере отобразится ответ:

```JSON
{
    "query": "Is there a form named hrf-234098",
    "prediction": {
        "topIntent": "FindForm",
        "intents": {
            "FindForm": {
                "score": 0.9768753
            },
            "None": {
                "score": 0.0216071177
            }
        },
        "entities": {
            "Human Resources Form Number": [
                "hrf-234098"
            ],
            "$instance": {
                "Human Resources Form Number": [
                    {
                        "type": "Human Resources Form Number",
                        "text": "hrf-234098",
                        "startIndex": 22,
                        "length": 10,
                        "modelTypeId": 8,
                        "modelType": "Regex Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    }
}
```

Чтобы просмотреть на панели проверки сведения такого же уровня, необходимо выполнить публикацию приложения. После этого на панели проверки выберите **Compare with published** (Сравнить с опубликованным). На панели проверки опубликованного приложения щелкните **Show JSON view** (Просмотреть представление JSON), чтобы просмотреть данные JSON, отображаемые при выполнении предыдущего шага. Таким образом вы сможете найти различия между текущим приложением, над которым работаете, и приложением, которое опубликовано в конечной точке.

[![Сравнение текущего приложения, в которое вносятся изменения, и опубликованной версии приложения](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)

## <a name="clean-up-resources"></a>Очистка ресурсов

Изучив это краткое руководство, щелкните **Мои приложения** в меню навигации вверху. Установите флажок возле имени приложения в списке, а затем щелкните **Удалить** в контекстном меню над списком.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Определение общих намерений и сущностей](luis-tutorial-prebuilt-intents-entities.md)
