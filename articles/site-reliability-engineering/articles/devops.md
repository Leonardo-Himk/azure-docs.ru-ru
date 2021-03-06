---
title: 'Часто задаваемые вопросы: выполняются и DevOps | Документация Майкрософт'
titleSuffix: Azure
description: 'Часто задаваемые вопросы: основные сведения о связи между выполняются и DevOps'
author: dnblankedelman
manager: efreeman
ms.service: site-reliability-engineering
ms.topic: article
ms.date: 04/22/2020
ms.author: dnb
ms.openlocfilehash: e917c609b484b1a58377fea2f6cdd75dde30ca6c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82196417"
---
# <a name="frequently-asked-questions-whats-the-relationship-between-sre-and-devops"></a>Часто задаваемые вопросы: что такое отношение между выполняются и DevOps?

Существует ряд часто встречающихся вопросов, связанных со связью между проектированием надежности сайта и DevOps, включая «как они одинаковы? Чем они отличаются? Есть ли у нас и в нашей Организации?». В этой статье вы попытаетесь поделиться некоторыми из ответов, предлагаемых сообществом выполняются и DevOps, которые поближе нас к пониманию этой связи.

## <a name="how-are-they-the-same"></a>Как они одинаковы?

ВЫПОЛНЯЮТСЯ и DevOps — это современные методики работы, которые были созданы и разработаны в ответ на проблемы, которые включают в себя:

- растет сложность производственных сред и процессов разработки
- повышение бизнес-зависимости на постоянной работе этих сред
- невозможность масштабирования сотрудников линейно с учетом размера этих сред
- необходимость в быстром переходе, сохраняя при этом операционную стабильность

Оба практических решения приводят внимание на темы, которые важны для решения таких задач, как мониторинг/наблюдаемость, Автоматизация, документация и средства совместной разработки программного обеспечения.

Существует существенное перекрытие средств и областей работы между выполняются и DevOps. По мере того, как _Книга надежности сайта_ помещает ее, "выполняются считает, что это так, как DevOps, но по слегка разным причинам".

## <a name="three-different-ways-to-compare-the-two-operations-practices"></a>Три разных способа сравнения двух операций эксплуатации

Сходства между выполняются и DevOps очевидны. Там, где это интересно, это то, как два отличия или расходятся. В этой статье мы предлагаем три способа подумать о взаимосвязих как способ придумать свои особенности к этому вопросу. Вы не можете согласиться с этими ответами, но каждый из них предоставит хорошее начало обсуждения.

### <a name="class-sre-implements-interface-devops"></a>"класс выполняются реализует интерфейс DevOps"

_Книга надежности сайта_ (упомянутая в нашем [списке книг ресурсов](../resources/books.md)) обсуждает выполняются и DevOps в первой главе. В этой главе используется фраза "класс выполняются реализует интерфейс DevOps" в качестве подзаголовка. Это необходимо для предложения (используя фразу, предназначенную для разработчиков), которую выполняются можно рассматривать как конкретную реализацию философии DevOps. Как говорится в главе, «DevOps сравнительно незаметно для выполнения операций на подробном уровне», в то время как выполняются значительно более подробно описывает его методики. Один из возможных ответов на вопрос о том, как две связи являются выполняются, могут считаться одной из многих возможных реализаций DevOps.

### <a name="sre-is-to-reliability-as-devops-is-to-delivery"></a>ВЫПОЛНЯЮТСЯ — это надежность, так как DevOps — Доставка

Это сравнение является небольшим муддиедм, так как существует несколько определений для выполняются и DevOps, но они все еще потенциально полезны. Он начинается с вопроса «если вам пришлось преносить каждую операцию в одно или два слова, отражающие его основные проблемы, что бы оно было?»

Если мы используем это определение выполняются из [центра проектирования надежности сайта](../index.yml):

> Обеспечение надежности информационных систем — инженерная дисциплина, направленная на устойчивое достижение организациями необходимого уровня надежности в их системах, службах и продуктах.

Затем было бы легко сказать, что слово для выполняются — «надежность». Если он находится в середине имени, также предлагается несколько отличных доказательств для этого утверждения.

Если мы используем это определение DevOps из [центра ресурсов Azure DevOps](https://docs.microsoft.com/azure/devops/learn/):

> DevOps — это объединение людей, процессов и продуктов для непрерывной поставки ценности конечным пользователям.

Таким же образом, DevOps может быть "Доставка".

Следовательно, «выполняются — это надежность, так как DevOps — доставка».

### <a name="direction-of-attention"></a>Направление внимания

Этот ответ заключен в кавычки или несколько слов из вклада Томас Лимонцелли в книгу по _поиску выполняются_ , упомянутую в нашем [списке книг ресурсов](../resources/books.md). Он отмечает, что инженеры DevOps во многом сосредоточены на конвейере жизненного цикла разработки программного обеспечения с редкими обязанностями в рабочей среде, в то время как Срес сосредоточиться на производственных операциях с помощью случайного SDLCа

Но, что более важно, он также рисует схему, которая начинается с процесса разработки программного обеспечения на одной стороне, и производственные операции работают на другом. Эти два типа соединены обычным конвейером, созданным для получения кода от разработчика, Шеферд его с помощью требуемого количества тестов и этапа, а затем перемещая этот код в рабочую среду.

Лимонцелли заметок о том, что инженеры DevOps начинают работу в среде разработки и автоматизируют шаги в производстве. После завершения они возвращаются для оптимизации узких мест.

Срес, с другой стороны, сосредоточьтесь на производственных операциях и достигайте глубокой части в конвейере, что позволяет улучшить конечный результат (в основном работает в обратном направлении).

Это различие в направлении фокуса выполняются и DevOps, которое помогает отличать их.

## <a name="coexistence-in-the-same-organization"></a>Сосуществование в одной организации

Последний вопрос, который нам хотелось бы решить: "возможно, у вас есть и выполняются, и DevOps в одной организации?"

Ответ на этот вопрос — заметным "Да!".

Мы надеемся, что в предыдущих ответах мы предложим понять, как перекрываются два практических опыта, и, когда они не перекрываются, как они могут быть дополнены фокусом. Организации с установленной практикой DevOps могут поэкспериментировать с методиками выполняются на небольшом масштабе (например, попытаться выполнить Слис и SLO) без необходимости зафиксироваться в создании выполняются позиций или команд. Это довольно распространенный шаблон внедрения выполняются.

## <a name="next-steps"></a>Дальнейшие действия

Хотите узнать больше о проектировании надежности сайта или DevOps? Ознакомьтесь с сайтом по обеспечению [надежности сайта](../index.yml) и [центром ресурсов Azure DevOps](https://docs.microsoft.com/azure/devops/learn/).
