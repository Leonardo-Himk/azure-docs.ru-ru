---
title: Типы содержимого — QnA Maker
description: Типы содержимого включают множество стандартных структурированных документов, таких как PDF, DOC и TXT.
services: cognitive-services
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: e2569832f7f87ed7b8fccbc44e431fa45495ba67
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996114"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Типы содержимого документов, которые можно добавить в базу знаний
Типы содержимого включают множество стандартных структурированных документов, таких как PDF, DOC и TXT.

## <a name="file-and-url-data-types"></a>Типы данных File и URL

В приведенной ниже таблице представлены типы содержимого и форматы файлов, поддерживаемые QnA Maker.

|Тип источника|Тип содержимого| Примеры|
|--|--|--|
|URL-адрес|Часто задаваемые вопросы<br> (неструктурированные, с разделами или темами на домашней странице)<br>Страницы поддержки <br> (одностраничные руководства, устранение неполадок и т. д.)|[Простая страница вопросов и ответов](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs) <br>[Часто задаваемые вопросы о со ссылками](https://www.microsoft.com/en-us/software-download/faq)<br> [Вопросы и ответы с домашней страницей разделов](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Статья технической поддержки](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|Часто задаваемые вопросы и ответы<br> Руководство по продукту<br> Брошюры<br> Бумажные документы<br> Политика в отношении флаеров<br> Руководство по поддержке<br> Файл структурированных вопросов и ответов (QnA)<br> и т. д.|**Без множественной переворачивания**<br>[Файл структурированных вопросов и ответов (QnA).doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx)<br> [Пример руководства по продукту.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf)<br> [Пример слабоструктурированного раздела.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx)<br> [Образец технического документа. PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br><br>**Множественная перевращение**:<br>[Surface Pro (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Преимущества Contoso (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Преимущества Contoso (PDF)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|* Excel|Файл структурированных вопросов и ответов (QnA)<br> (включая поддержку RTF, HTML)|[Пример часто задаваемых вопросов QnA.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|* TXT/TSV|Файл структурированных вопросов и ответов (QnA)|[Пример беседы.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Если для источника данных требуется проверка подлинности, рассмотрите следующие методы получения содержимого в QnA Maker:

* Загрузить файл вручную и импортировать в QnA Maker
* Добавить файл из расположения, прошедшего проверку подлинности [SharePoint](../how-to/add-sharepoint-datasources.md)

## <a name="url-content"></a>Содержимое URL-адреса

С помощью **URL-адреса** в QnA Maker можно импортировать два типа документов:

* URL-адреса вопросов и ответов
* Поддержка URL-адресов

Каждый тип указывает ожидаемый формат.

## <a name="file-based-content"></a>Содержимое на основе файлов

Вы можете добавить файлы в базу знаний из общедоступного источника или локальной файловой системы на [портале QnA Maker](https://www.qnamaker.ai).

## <a name="content-format-guidelines"></a>Рекомендации по формату содержимого

Узнайте больше о [рекомендациях по формату](../reference-document-format-guidelines.md) для различных файлов.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, какие сведения хранятся в [паре вопросов и ответов (QnA)](question-answer-set.md).