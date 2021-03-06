---
title: Краткое руководство. Публикация базы знаний, остальное, C# — QnA Maker
description: В этом кратком руководстве по началу работы с C# и REST описывается, как опубликовать базу знаний и создать конечную точку, которую можно вызвать в приложении или чат-боте.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 0a2f64795d82928e33a10c7e9d162f2333cdf7bf
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "78851710"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Краткое руководство. Публикация базы знаний в QnA Maker с помощью C#

В этом кратком руководстве по REST описано, как программным способом опубликовать базу знаний. В процессе публикации последняя версия базы знаний передается в индекс выделенной службы "Когнитивный поиск Azure" и создается конечная точка, которую можно вызывать в приложении или чат-боте.

В этом кратком руководстве вызываются API службы QnA Maker:
* [Публикация.](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish) При использовании этого API в тексте запроса не нужно указывать какие-либо сведения.

## <a name="prerequisites"></a>Предварительные требования

* Последний [**выпуск Visual Studio Community**](https://www.visualstudio.com/downloads/).
* У вас должна быть [служба QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Чтобы получить ключ и конечную точку (включая имя ресурса), щелкните **Быстрый запуск** для ресурса на портале Azure.
* Идентификатор базы знаний QnA Maker, который находится в URL-адресе в параметре строки запроса `kbid`, как показано ниже.

    ![Идентификатор базы знаний QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Если у вас еще нет базы знаний, создайте пример для этого краткого руководства по инструкциям из [этой статьи](create-new-kb-csharp.md).

> [!NOTE]
> Полный файл решения можно найти в [репозитории **Azure-Samples/qnamaker-Services--CSharp** GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-knowledge-base-project"></a>Создание проекта базы знаний

1. Откройте Visual Studio 2019 Community Edition.
1. Создайте проект **Консольное приложение (.NET Core)** и присвойте ему имя `QnaMakerQuickstart`. Для остальных параметров оставьте значения по умолчанию.

## <a name="add-required-dependencies"></a>Добавление необходимых зависимостей

В начале файла Program.cs замените одиночный оператор using следующими строками, чтобы добавить необходимые зависимости в проект:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=1-2 "Add the required dependencies")]

## <a name="add-required-constants"></a>Добавление необходимых констант

В классе **Program** добавьте необходимые константы для доступа к QnA Maker.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=8-34 "Add the required constants")]

## <a name="add-the-main-method-to-publish-the-knowledge-base"></a>Добавление метода Main для публикации базы знаний

Включив константы, добавьте следующий код, который отправляет HTTPS-запрос к API службы QnA Maker, чтобы опубликовать базу знаний и получить ответ:

[!code-csharp[Add HTTP Post request and response](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=36-56 "Add HTTP Post request and response")]

Вызов API возвращает состояние 204 для успешной публикации без содержимого в тексте ответа.

## <a name="build-and-run-the-program"></a>Сборка и запуск проекта

Выполните сборку и запуск программы. Эта программа автоматически отправляет запрос к API службы QnA Maker на публикацию базы знаний. Ответ выводится в окно консоли.

После публикации базы знаний вы можете запросить ее из конечной точки с помощью клиентского приложения или чат-бота.

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Следующие шаги

Опубликовав базу знаний, вам нужно использовать [URL-адрес конечной точки для создания ответа](./get-answer-from-knowledge-base-csharp.md).

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API Reference](https://go.microsoft.com/fwlink/?linkid=2092179) (Справочник по API REST QnA Maker (V4))