---
title: Работа с источниками данных для профилирования в Каталоге данных Azure
description: В статье приведены инструкции по добавлению профилей данных уровня таблиц и столбцов при регистрации источников данных в каталоге данных Azure. В статье также объясняется, как профили данных помогают анализировать имеющиеся источники.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 04ac6c2bf0137289221a4ae6ed58d5a71ad21739
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "68950222"
---
# <a name="how-to-data-profile-data-sources-in-azure-data-catalog"></a>Как источники данных профиля данных в каталоге данных Azure

## <a name="introduction"></a>Введение

**Каталог данных Microsoft Azure** — это полностью управляемая облачная служба, выполняющая функции систем регистрации и обнаружения корпоративных источников данных. Другими словами, **Каталог данных Azure** помогает пользователям находить, изучать и использовать источники данных, а также помогать организациям получать больше возможностей из существующих данных. Если источник данных зарегистрирован в **каталоге данных Azure**, его метаданные копируются и индексируются службой, но на этом работа с ними не заканчивается.

Функция **профилирования данных****каталога данных Azure** проверяет данные из поддерживаемых источников данных в каталоге и собирает статистику и информацию об этих данных. Включить профиль ресурсов данных — очень легко. При регистрации ресурса данных выберите пункт **Включить профиль данных** в средстве регистрации источника данных.

## <a name="what-is-data-profiling"></a>Что такое профилирование данных

Профилирование данных — это проверка данных в регистрируемом источнике данных, а также сбор статистики и информации об этих данных. Во время поиска источника данных эта статистика может помочь вам определить пригодность данных для решения той или иной бизнес-задачи.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Профилирование данных поддерживают такие источники данных:

* таблицы и представления SQL Server (включая базы данных SQL Azure и хранилище данных SQL Azure);
* таблицы и представления Oracle;
* таблицы и представления Teradata;
* таблицы Hive.

Включение профилей данных при регистрации ресурсов данных помогает пользователям ответить на следующие вопросы об источниках данных:

* Могу ли я с помощью этого источника данных решить свою бизнес-проблему?
* Соответствуют ли данные определенным стандартам или шаблонам?
* Каковы аномалии этого источника данных?
* Каковы возможные проблемы интеграции этих данных в мое приложение?

> [!NOTE]
> Кроме того, вы можете добавить документацию в ресурс, чтобы описать, как данные можно интегрировать в приложение. Ознакомьтесь со статьей [Как создать документацию по источникам данных](data-catalog-how-to-documentation.md).
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Как включить профиль данных при регистрации источника данных

Включить профиль источника данных очень легко. Когда вы регистрируете источник данных, на панели **Объекты для регистрации** средства регистрации источника данных выберите **Включить профиль данных**.

![Флажок включения профиля данных](media/data-catalog-data-profile/data-catalog-register-profile.png)

Дополнительные сведения о том, как регистрировать источники данных, см. в статьях [Как регистрировать источники данных](data-catalog-how-to-register.md) и [Начало работы с каталогом данных Azure](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Фильтрация по ресурсам данных, которые включают в себя профили данных

Чтобы обнаружить ресурсы данных, которые включают в себя профили данных, вы можете использовать условие поиска `has:tableDataProfiles` или `has:columnsDataProfiles`.

> [!NOTE]
> Если в инструменте регистрации источников данных выбрать **Включить профиль данных** , то будут включены данные профиля уровня таблицы и столбца. Тем не менее API каталога данных позволяет регистрировать ресурсы данных, содержащие только один набор данных профиля.
>

## <a name="viewing-data-profile-information"></a>Просмотр сведений о профиле данных

Найдя подходящий источник данных с профилем, вы можете просмотреть сведения об этом профиле данных. Для этого выберите ресурс данных и щелкните **Профиль данных** в окне портала каталога данных.

![Вкладка «Профиль данных»](media/data-catalog-data-profile/data-catalog-view.png)

Профиль данных в **каталоге данных Azure** отображает сведения о профиле столбца и таблицы, в том числе приведенную ниже информацию.

### <a name="object-data-profile"></a>Профиль данных объекта

* Число строк
* Размер таблицы
* Время последнего обновления объекта

### <a name="column-data-profile"></a>Профиль данных столбца

* Тип данных столбца
* Количество уникальных значений
* Количество строк со значением NULL
* Минимальное, максимальное, среднее и стандартное отклонение значений столбца

## <a name="summary"></a>Сводка

Профилирование данных дает возможность пользоваться информацией о зарегистрированных ресурсах данных и связанной с ними статистикой. Это позволяет определить, насколько те или иные данные подходят для решения бизнес-задач. С помощью профилей данных можно не только аннотировать и документировать источники данных, но и предоставлять пользователям более точное описание своих данных.

## <a name="see-also"></a>См. также:

* [Как регистрировать источники данных](data-catalog-how-to-register.md)
* [Начало работы с Каталогом данных Azure](data-catalog-get-started.md)
