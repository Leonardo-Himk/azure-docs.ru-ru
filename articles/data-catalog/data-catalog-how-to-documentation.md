---
title: Как создать документацию по источникам данных в Каталоге данных Azure
description: Эта статья представляет собой практическое руководство, в котором объяснено, как создать документацию по ресурсам данных в каталоге данных Azure.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: e9e9013d354585d04f205feb93a84d94c0f05905
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "68950191"
---
# <a name="how-to-document-data-sources-in-azure-data-catalog"></a>Как создать документацию по источникам данных в Каталоге данных Azure

## <a name="introduction"></a>Введение
**Каталог данных Microsoft Azure** — это полностью управляемая облачная служба, выполняющая функции систем регистрации и обнаружения корпоративных источников данных. Проще говоря, **каталог данных Azure** помогает пользователям находить, *оценивать*и использовать источники данных, что, в свою очередь, повышает для организаций ценность существующей информации.

Если источник данных зарегистрирован в **каталоге данных Azure**, его метаданные копируются и индексируются службой, но на этом работа с ними не заканчивается. **Каталог данных Azure** позволяет пользователям также предоставлять их собственную полную документацию, в которой описаны использование и стандартные сценарии, предназначенные для источника данных.

Из статьи о [создании заметок к источникам данных](data-catalog-how-to-annotate.md)вы можете узнать, что специалисты, владеющие информацией об источнике данных, могут добавлять к нему теги и описания. Портал **каталога данных Azure** содержит текстовый редактор, с помощью которого пользователи могут документировать ресурсы и контейнеры данных. Редактор предусматривает возможности форматирования, включая работу с заголовками, маркированными списками, нумерованными списками и таблицами, а также форматированием текста.

Теги и описания прекрасно подходят для простых заметок. Но чтобы пользователи могли соотнести бизнес-сценарии с имеющимися источниками данных, а также оценили возможности использования этих источников, специалисты могут предоставить исчерпывающую документацию. Создать документацию по источнику данных легко. Выберите ресурс данных или контейнер и щелкните **Документация**.

![Вкладка "Документация" в каталоге данных](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a>Создание документации по ресурсам данных
Вы можете использовать документацию **каталога данных Azure** в качестве репозитория содержимого, создавая с его помощью полное описание ресурсов данных. Можно изучать подробные описания контейнеров и таблиц. Если у вас уже есть содержимое в другом репозитории содержимого, например в SharePoint или общей папке, вы можете добавить ссылки на это содержимое в документацию по ресурсу. Эта возможность облегчает поиск существующих документов.

> [!NOTE]
> Документация не включена в индекс поиска.
>

![Вкладка "Документация" и гиперссылка на веб-канал](media/data-catalog-documentation/data-catalog-documentation2.png)

Уровень документации может быть разным: от описания характеристик и значений контейнера ресурса данных до подробного описания схемы таблицы, которую содержит контейнер. Уровень документации следует выбирать с учетом ваших бизнес-потребностей. Создание документации по ресурсам данных сопряжено с такими основными преимуществами и недостатками.

* Документация только по контейнеру: все содержимое находится в одном расположении, но содержимое может быть недостаточно подробным, следовательно, некоторые пользователи не смогут принять обоснованные решения.
* Документация только по таблицам: содержимое описывает конкретный объект, но документы находятся в нескольких расположениях.
* Документация по контейнерам и таблицам: комплексный подход, который не исключает необходимость дополнительной работы с документами.

## <a name="summary"></a>Сводка
Создавая документацию по источникам данных с помощью **каталога данных Azure** , можно описать свои ресурсы данных максимально подробно.  Вы можете добавить ссылки на существующий репозиторий содержимого, чтобы использовать уже имеющиеся документы и ресурсы данных. А ваши пользователи, обнаружив определенные ресурсы данных, могут пользоваться полным набором документации.
