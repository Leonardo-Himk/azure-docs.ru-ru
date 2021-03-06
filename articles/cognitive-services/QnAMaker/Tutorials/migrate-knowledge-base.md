---
title: Перенос баз знаний с помощью QnA Maker
description: Чтобы перенести базу знаний, необходимо экспортировать одну базы знаний, а затем импортировать ее в другую базу знаний.
ms.topic: article
ms.date: 03/25/2020
ms.openlocfilehash: 13e5e79bf4eaf6ec59e41b3e12aa1bb23f2c1578
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80258096"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Миграция базы знаний с помощью экспорта и импорта

Миграция — это процесс создания новой базы знаний из существующей базы знаний. Это можно сделать по нескольким причинам.

* процесс резервного копирования и восстановления
* Конвейер CI/CD
* переместить регионы

Для миграции базы знаний необходимо экспортировать из существующей базы знаний, а затем импортировать в другую.

## <a name="prerequisites"></a>Предварительные требования

* Перед началом работы создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Настройте новую [службу QnA Maker](../How-To/set-up-qnamaker-service-azure.md).

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Перенос базы знаний из QnA Maker
1. Войдите на [портал QnA Maker](https://qnamaker.ai).
1. Выберите базу знаний источника, которую необходимо перенести.

1. На странице **Параметры** выберите **Экспорт базы знаний** , чтобы скачать TSV-файл, содержащий содержимое базы знаний источника — вопросы, ответы, метаданные, приглашения к исполнению, и имена источников данных, из которых они были извлечены.

1. Выберите **создать базу знаний** в верхнем меню, а затем создайте _пустую_ базу знаний. Он пуст, так как при создании он не добавляется никаких URL-адресов или файлов. Они добавляются на этапе импорта после создания.

    Настройка базы знаний. Задать только новое имя базы знаний. Поддерживаются повторяющиеся имена, а также специальные знаки в имени.

    Не выбирайте ничего из шага 4, так как эти значения будут перезаписаны при импорте файла.

1. На шаге 5 Выберите **создать**.

1. В новой базе знаний откройте вкладку **Settings** (Параметры) и выберите **Import knowledge base** (Импортировать базу знаний). При этом будут импортированы вопросы, ответы, метаданные, приглашения к исполнению и сохранены имена источников данных, из которых они были извлечены.

   > [!div class="mx-imgBorder"]
   > [![Импорт базы знаний](../media/qnamaker-how-to-migrate-kb/Import.png)](../media/qnamaker-how-to-migrate-kb/Import.png#lightbox)

1. **Проверьте** новую базу знаний с помощью панели "Тестирование". Узнайте, как [проверить базу знаний](../How-To/test-knowledge-base.md).

1. **Опубликуйте** базу знаний и создайте робот чата. Узнайте, как [опубликовать базу знаний](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="programmatically-migrate-a-knowledge-base-from-qna-maker"></a>Программная миграция базы знаний из QnA Maker

Процесс миграции программным путем можно получить с помощью следующих интерфейсов API:

**Экспорт**

* [Скачать API базы знаний](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/download)

**Импорт**

* [Reload API (перезагрузка с тем же ИДЕНТИФИКАТОРом базы знаний)](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/replace)
* [Создать API (загрузить с новым ИДЕНТИФИКАТОРом базы знаний)](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)


## <a name="chat-logs-and-alterations"></a>Журналы чатов и варианты
Преобразования (синонимы) без учета регистра не импортируются автоматически. Используйте [API v4](https://go.microsoft.com/fwlink/?linkid=2092179) для перемещения изменений в новой базе знаний.

Возможность переноса журналов чатов не предусмотрена, так как новая база знаний использует для хранения журналов чатов Application Insights.

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Редактирование базы знаний](../How-To/edit-knowledge-base.md)
