---
title: Использование создания и использования ключей среды выполнения с LUIS
titleSuffix: Azure Cognitive Services
description: LUIS использует два ключа — ключ разработки для создания модели и ключ среды выполнения для запроса конечной точки прогнозирования с пользовательским фразы продолжительностью.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 954e7a22ae6b242c6221119c688259e4ce629a2a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82101065"
---
# <a name="authoring-and-runtime-keys"></a>Ключи разработки и среды выполнения

Language Understanding (LUIS) содержит две службы и наборы API:

* Создание (ранее известное как _программное_)
* Среда выполнения прогнозирования

В зависимости от того, с какой службой вы хотите работать, и с каким способом работать с ней необходимо использовать несколько типов ключей.

## <a name="non-azure-resources-for-luis"></a>Ресурсы, не относящиеся к Azure, для LUIS

### <a name="starter-key"></a>Начальный ключ

При первом запуске с помощью LUIS создается **начальный ключ** . Этот ресурс предоставляет следующие сведения:

* Бесплатные запросы на обслуживание с помощью портала LUIS или API-интерфейсов (включая пакеты SDK)
* Бесплатные запросы к конечной точке прогнозирования 1 000 в месяц с помощью браузера, API или пакетов SDK

## <a name="azure-resources-for-luis"></a>Ресурсы Azure для LUIS

<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

LUIS позволяет выполнять три типа ресурсов Azure:

|Ключ|Назначение|Служба для переприятия`kind`|Служба для переприятия`type`|
|--|--|--|--|
|[Создание ключа](#programmatic-key)|Доступ к данным приложения и управление ими с помощью разработки, обучения, публикации и тестирования. Создайте ключ создания LUIS, если планируется программное создание приложений LUIS.<br><br>Цель этого `LUIS.Authoring` ключа — предоставить вам следующие возможности:<br>* Программное управление Language Understanding приложениями и моделями, включая обучение и публикацию<br> * Управление разрешениями на создание ресурсов путем назначения пользователям [роли участника](#contributions-from-other-authors).|`LUIS.Authoring`|`Cognitive Services`|
|[Ключ прогнозирования](#prediction-endpoint-runtime-key)| Запросы конечной точки прогнозирования запросов. Создайте ключ прогнозирования LUIS, прежде чем клиентское приложение запросит прогнозы за пределами 1 000 запросов, предоставленных начальным ресурсом. |`LUIS`|`Cognitive Services`|
|[Ключ ресурса с несколькими службами для перекрывающего службы](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|Запросы конечной точки прогнозирования запросов, используемые совместно с LUIS и другими поддерживаемыми Cognitive Services.|`CognitiveServices`|`Cognitive Services`|

По завершении процесса создания ресурса [назначьте ключ](luis-how-to-azure-subscription.md) приложению на портале Luis.

Важно создавать приложения LUIS в [регионах](luis-reference-regions.md#publishing-regions) , где вы хотите публиковать и запрашивать.

> [!CAUTION]
> Для удобства многие из примеров используют [начальный ключ](#starter-key) , так как он предоставляет несколько свободных вызовов конечной точки прогнозирования в своей [квоте](luis-limits.md#key-limits).


### <a name="query-prediction-resources"></a>Ресурсы прогнозирования запросов

* Ключ среды выполнения можно использовать для всех приложений LUIS или для конкретных приложений LUIS.
* Не используйте ключ среды выполнения для создания приложений LUIS.

Конечная точка среды выполнения LUIS принимает два стиля запроса: используется ключ среды выполнения прогнозирующего Endpoint, но в разных местах.

Конечная точка, используемая для доступа к среде выполнения, использует поддомен, уникальный для региона ресурса, который указан `{region}` в следующей таблице.

## <a name="assignment-of-the-key"></a>Назначение ключа

Вы можете [назначить](luis-how-to-azure-subscription.md) ключ среды выполнения на [портале Luis](https://www.luis.ai) или через соответствующие API.

## <a name="key-limits"></a>Ограничения ключа

Вы можете создать до 10 ключей создания для каждого региона на подписку.

См. [раздел ограничения ключей](luis-limits.md#key-limits) и [регионы Azure](luis-reference-regions.md).

Регионы публикации отличаются от регионов разработки. Убедитесь, что вы создали приложение в области разработки, соответствующей региону публикации, в котором должно располагаться клиентское приложение.

## <a name="key-limit-errors"></a>Ошибки ограничений ключей
При превышении квоты на количество транзакций в секунду (TPS) возникает ошибка HTTP 429. Если вы превысили квоту на транзакцию за месяц (TPS), вы получаете ошибку HTTP 403.

## <a name="contributions-from-other-authors"></a>Вклады других авторов

**Для [создания переносимых ресурсов](luis-migration-authoring.md) приложения**: _участники_ управляются в портал Azure для ресурса разработки с помощью страницы **управления доступом (IAM)** . Узнайте, [как добавить пользователя](luis-how-to-collaborate.md), используя адрес электронной почты участника совместной работы и роль _участника_ .

**Для приложений, которые еще не перенесены**: все _Участники совместной работы_ управляются на портале Luis на странице **совместной работы "Управление >** ".

## <a name="move-transfer-or-change-ownership"></a>Перемещение, перемещение или изменение владельца

Приложение определяется ресурсами Azure, которое определяется подпиской владельца.

Вы можете переместить приложение LUIS. Используйте следующие ресурсы документации в портал Azure или Azure CLI.

* [Перемещение приложения между ресурсами по разработке LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Перемещение ресурса в новую группу ресурсов или подписку](../../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Переместить ресурс в пределах одной подписки или в рамках подписок](../../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)

Чтобы передавать [владение](../../cost-management-billing/manage/billing-subscription-transfer.md) подпиской, выполните следующие действия.

**Для пользователей, которые выполнили миграцию перенесенных [ресурсов](luis-migration-authoring.md) приложения**: в качестве владельца ресурса можно добавить. `contributor`

**Для пользователей, которые еще не выполнили миграцию**: экспортируйте приложение как JSON-файл. Другой пользователь LUIS может импортировать приложение, тем самым преставя его в владельца. Новое приложение будет иметь другой идентификатор приложения.

## <a name="access-for-private-and-public-apps"></a>Доступ для частных и общедоступных приложений

Для **частного** приложения доступ к среде выполнения предоставляется владельцам и участникам. Для **общедоступного приложения доступ** к среде выполнения предоставляется всем, у кого есть собственный ресурс [службы](../cognitive-services-apis-create-account.md) "Поиск Azure" или "Среда выполнения [Luis](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) ", а также идентификатор общедоступного приложения.

В настоящее время каталог общедоступных приложений отсутствует.

### <a name="authoring-access"></a>Доступ для разработки
Доступ к приложению с портала [Luis](luis-reference-regions.md#luis-website) или [API-интерфейсов разработки](https://go.microsoft.com/fwlink/?linkid=2092087) управляется ресурсом разработки Azure.

Владелец и все участники имеют доступ для создания приложения.

|Доступ к разработке включает в себя:|Примечания|
|--|--|
|Добавление или удалений ключей конечных точек||
|Экспорт версии||
|Экспорт журналов конечных точек||
|Импорт версии||
|Предоставление к приложению общего доступа|Если приложение является общедоступным, запросить его может любой пользователь с ключом разработки или ключом конечной точки.|
|Изменение модели|
|Публикация|
|Проверка высказываний конечной точки для [активного обучения](luis-how-to-review-endpoint-utterances.md)|
|Обучение|

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Доступ к среде выполнения для конечной точки прогнозирования

Доступ к запросу конечной точки прогнозирования управляется параметром на странице **сведения о приложении** в разделе **Управление** .

|[Частная конечная точка](#runtime-security-for-private-apps)|[Общедоступная конечная точка](#runtime-security-for-public-apps)|
|:--|:--|
|Доступно для владельца и участников|Доступно для владельца, участников и других пользователей, которым известен идентификатор приложения|

Вы можете контролировать, кто видит ключ среды выполнения LUIS, вызывая его в среде "сервер-сервер". Если вы используете LUIS из бота, соединение между ботом и LUIS уже защищено. При вызове конечной точки LUIS напрямую следует создать серверный API (такой как [функция](https://azure.microsoft.com/services/functions/) Azure) с контролируемым доступом (таким как [AAD](https://azure.microsoft.com/services/active-directory/)). При вызове API на стороне сервера и проверке подлинности и авторизации, передайте вызов в LUIS. Хотя эта стратегия не мешает атакам типа "злоумышленник в середине", она скрывает ключ и URL-адрес конечной точки от пользователей, позволяет контролировать доступ и позволяет добавлять журнал ответов на конечные точки (например, [Application Insights](https://azure.microsoft.com/services/application-insights/)).

#### <a name="runtime-security-for-private-apps"></a>Безопасность среды выполнения для частных приложений

Среда выполнения частного приложения доступна только для следующих приложений:

|Ключ и пользователь|Объяснение|
|--|--|
|Ключ разработки владельца| До 1000 обращений к конечной точке|
|Ключи для совместной работы и создания участников| До 1000 обращений к конечной точке|
|Любой ключ, назначенный LUIS автору или участнику совместной работы|В зависимости от уровня использования ключей|

#### <a name="runtime-security-for-public-apps"></a>Безопасность среды выполнения для общедоступных приложений

После этого _любой_ допустимый ключ разработки или ключ конечной точки LUIS может выполнять запросы к приложению до тех пор, пока он не израсходует всю квоту конечной точки.

Пользователь, не являющийся владельцем или участником, может получить доступ к среде выполнения общедоступного приложения только при наличии идентификатора приложения. У LUIS нет общедоступного _магазина_ или других вариантов поиска общедоступных приложений.

Общедоступное приложение публикуется во всех регионах, чтобы пользователь с ключом ресурса LUIS для определенного региона смог получить доступ к приложению в том регионе, который связан с ключом ресурса.

## <a name="transfer-of-ownership"></a>Передача права владения

LUIS не имеет концепции передачи владения ресурсом.

## <a name="securing-the-endpoint"></a>Защита конечной точки

Вы можете контролировать, кто может просматривать ключ конечной точки среды выполнения LUIS PREDICTION, вызывая его в среде "сервер-сервер". Если вы используете LUIS из бота, соединение между ботом и LUIS уже защищено. При вызове конечной точки LUIS напрямую следует создать серверный API (такой как [функция](https://azure.microsoft.com/services/functions/) Azure) с контролируемым доступом (таким как [AAD](https://azure.microsoft.com/services/active-directory/)). После вызова серверного API и прохождения авторизации и проверки подлинности нужно передать вызов в LUIS. Хотя эта стратегия не предотвращает распространение атак с перехватом, она маскирует конечную точку от пользователей, позволяет отслеживать доступ и добавлять функцию ведения журнала ответов конечной точки (такую как [Application Insights](https://azure.microsoft.com/services/application-insights/)).

## <a name="next-steps"></a>Следующие шаги

* Поймите принципы [управления версиями](luis-concept-version.md).
* Узнайте [, как создавать ключи](luis-how-to-azure-subscription.md).
