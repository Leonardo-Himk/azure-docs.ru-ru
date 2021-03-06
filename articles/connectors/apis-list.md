---
title: Соединители для Azure Logic Apps
description: Автоматизируйте рабочие процессы с соединителями для Azure Logic Apps, таких как встроенная, управляемая, локальная учетная запись интеграции, интегрированная среда сценариев ISE и корпоративные соединители.
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 04/24/2020
ms.openlocfilehash: f2a2ee7a2806a753ffd159c91ed782634e74c704
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838685"
---
# <a name="connectors-for-azure-logic-apps"></a>Соединители для Azure Logic Apps

Соединители обеспечивают быстрый доступ из Azure Logic Apps к событиям, данным и действиям в других приложениях, службах, системах, протоколах, а также на других платформах. Используя соединители в приложениях логики, вы можете расширить возможности своих облачных и локальных приложений, чтобы выполнять задачи с создаваемыми и существующими данными.

Хотя Logic Apps предлагает [сотни соединителей](https://docs.microsoft.com/connectors), в этой статье описываются *популярные и более часто используемые* соединители, которые успешно использовались тысячами приложений и миллионы выполнений для обработки данных и информации. Чтобы найти полный список соединителей и справочные сведения о каждом соединителе, такие как триггеры, действия и ограничения, ознакомьтесь со страницами Справочника по соединителям в разделе [Общие сведения о](https://docs.microsoft.com/connectors)соединительных линиях. Кроме того, Узнайте больше о [триггерах и действиях](#triggers-actions), [Logic Apps модели ценообразования](../logic-apps/logic-apps-pricing.md), а также [о Logic Apps ценах](https://azure.microsoft.com/pricing/details/logic-apps/).

> [!TIP]
> Для интеграции со службой или API, которые не имеют соединителя, можно либо напрямую вызвать службу через протокол, например HTTP, либо создать [настраиваемый соединитель](#custom).

## <a name="connector-types"></a>Типы соединителей

Соединители доступны в виде встроенных триггеров и действий или управляемых соединителей.

<a name="built-in"></a>

* [**Встроенные**](#built-ins): встроенные триггеры и действия являются "собственными", Azure Logic Apps и позволяют выполнять эти задачи в приложениях логики:

  * Запускаются в настраиваемых и расширенных расписаниях.

  * Упорядочивать и контролировать рабочий процесс приложения логики, например циклы и условия, а также работать с переменными и операциями с данными.

  * Обмен данными с другими конечными точками.

  * Получение запросов и реагирование на них.

  * Вызывайте функции Azure, приложения API Azure (веб-приложения), собственные API, управляемые и опубликованные с помощью службы управления API Azure, а также вложенные приложения логики, которые могут получать запросы.

<a name="managed-connectors"></a>

* [**Управляемые соединители**](#managed-api-connectors): развернутые и управляемые корпорацией Майкрософт. эти соединители предоставляют триггеры и действия для доступа к облачным службам, локальным системам или обоим, включая Office 365, хранилище BLOB-объектов Azure, SQL Server, Dynamics, Salesforce, SharePoint и т. д. Некоторые соединители специально поддерживают сценарии связи "бизнес-бизнес" (B2B) и нуждаются в [учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , связанной с приложением логики. Перед использованием определенных соединителей может потребоваться создать подключения, управляемые с помощью Azure Logic Apps.

  Например, если вы используете Microsoft BizTalk Server, приложения логики могут подключаться к BizTalk Server и взаимодействовать с ними с помощью [BizTalk Server локального соединителя](#on-premises-connectors). Затем вы можете расширить или выполнить операции, подобные BizTalk, в своих приложениях логики с помощью [соединителей для учетной записи интеграции](#integration-account-connectors).

  Соединители классифицируются как Standard или Enterprise. [Корпоративные соединители](#enterprise-connectors) предоставляют доступ к корпоративным системам, таким как SAP, IBM MQ и IBM 3270, за дополнительную плату. Чтобы определить, является ли соединитель стандартным или корпоративным, ознакомьтесь с техническими сведениями на справочной странице каждого соединителя в разделе " [Общие сведения о соединителях](https://docs.microsoft.com/connectors)".

  Можно также выявление соединителей с помощью этих категорий, хотя некоторые соединители могут пересекаться с несколькими категориями. Например, SAP — это соединитель предприятия и локальный Соединитель:

  |   |   |
  |---|---|
  | [**Управляемые соединители**](#managed-api-connectors) | Создавайте приложения логики, которые используют такие службы, как хранилище BLOB-объектов, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online и многие другие. |
  | [**Локальные соединители**](#on-premises-connectors) | После установки и настройки [локального шлюза данных][gateway-doc] эти соединители помогают приложениям логики получать доступ к локальным системам, таким как SQL Server, SharePoint Server, Oracle DB, общие файловые ресурсы и другие. |
  | [**Соединители для учетной записи интеграции**](#integration-account-connectors) | Доступные при создании и оплате учетной записи интеграции. Эти соединители преобразуют и проверяют XML, кодируют и декодируют неструктурированные файлы, а также обрабатывают сообщения типа "бизнес-бизнес" (B2B) с помощью протоколов AS2, EDIFACT и X12. |
  |||

  > [!IMPORTANT]
  > Если вы хотите использовать соединитель Gmail, только бизнес-учетные записи G-Suite могут использовать этот соединитель без ограничений в приложениях логики. Если у вас есть учетная запись потребителя Gmail, вы можете использовать этот соединитель только с конкретными утвержденными Google службами или [создать клиентское приложение Google для проверки подлинности с помощью соединителя Gmail](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application). Дополнительные сведения см. [в разделе политики безопасности и конфиденциальности данных для соединителей Google в Azure Logic Apps](../connectors/connectors-google-data-security-privacy-policy.md).

<a name="integration-service-environment"></a>

### <a name="connect-from-an-integration-service-environment"></a>Подключение из среды службы интеграции

Для приложений логики, которым необходим прямой доступ к ресурсам в виртуальной сети Azure, можно создать изолированную [среду службы интеграции (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) , в которой можно создавать, развертывать и запускать приложения логики на выделенных ресурсах. В конструкторе приложений логики при просмотре соединителей, которые вы хотите использовать для приложений логики в интегрированной среде сценариев, на встроенных триггерах и действиях появляется **Основная** метка, а в некоторых соединителях отображается метка **ISE** :

* **Core**: встроенные триггеры и действия с этой меткой запускаются в той же интегрированной среде сценариев, что и приложения логики, например:

  ![Пример соединителя ISE](./media/apis-list/example-core-connector.png)

* **ISE**. управляемые соединители с этой меткой выполняются в той же интегрированной среде сценариев, что и приложения логики, например:

  ![Пример соединителя ISE](./media/apis-list/example-ise-connector.png)

  При наличии локальной системы, подключенной к виртуальной сети Azure, интегрированная среда сценариев позволяет приложениям логики напрямую обращаться к этой системе без [локального шлюза данных](../logic-apps/logic-apps-gateway-connection.md). Вместо этого можно использовать соединитель **ISE** системы, если он доступен, HTTP-действие или [пользовательский соединитель](#custom). Для локальных систем, у которых нет соединителей **ISE** , используйте локальный шлюз данных. Чтобы просмотреть доступные соединители ISE, см. раздел [соединители ISE](#ise-connectors).

* Все остальные соединители без метки **Core** или **ISE** , которые можно продолжать использовать в глобальной службе Logic Apps с несколькими клиентами, например:

  ![Пример соединителя с несколькими клиентами](./media/apis-list/example-multi-tenant-connector.png)

Приложения логики, работающие в интегрированной среде сценариев и их соединители, независимо от того, где работают эти соединители, соответствуют фиксированному тарифному плану и тарифному плану на основе потребления. Дополнительные сведения см. на следующих страницах:

* [Модель ценообразования приложений логики](../logic-apps/logic-apps-pricing.md)
* [Logic Apps сведения о ценах](https://azure.microsoft.com/pricing/details/logic-apps/)
* [Подключение к виртуальным сетям Azure из Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="built-ins"></a>

## <a name="built-in"></a>Встроено

Logic Apps предоставляет встроенные триггеры и действия, позволяющие создавать рабочие процессы на основе расписания, помогать приложениям логики взаимодействовать с другими приложениями и службами, управлять рабочим процессом через приложения логики, управлять данными и работать с ними.

|   |   |   |   |
|---|---|---|---|
| [![][schedule-icon]<br>**Расписание** значков API][schedule-doc] | — Запуск приложения логики в указанное повторение, начиная от базового до расширенных расписаний с [триггером **повторения** ][schedule-recurrence-doc]. <p>— Запустите приложение логики, которое должно выполнять обработку данных в непрерывных блоках с помощью [триггера **скользящего окна** ][schedule-sliding-window-doc]. <p>— Приостановка приложения логики в течение указанного времени с помощью [действия **delay** ][schedule-delay-doc]. <p>— Приостановка приложения логики до указанной даты и времени с [ **задержкой до** действия][schedule-delay-until-doc]. | [![][batch-icon]<br>**Пакет** значков API][batch-doc] | — Обрабатывайте сообщения в пакетах с помощью триггера **Пакетные сообщения**. <p>— Вызывайте приложения логики, имеющие пакеты триггеров, с помощью действия **Send messages to batch** (Отправка сообщений в пакет). |
| [![Значок][http-icon]<br>API**http**][http-doc] | Вызывайте конечные точки HTTP или HTTPS с триггерами и действиями для HTTP. Другие встроенные триггеры и действия HTTP включают [http + Swagger][http-swagger-doc] и [HTTP + веб-перехватчик][http-webhook-doc]. | [![][http-request-icon]<br>**Запрос** значка API][http-request-doc] | — Сделайте свое приложение логики доступным для вызовов от других приложений или служб, активируйте события ресурса Event Grid или ответ на предупреждения Azure Security Center с помощью триггера **Запрос**. <p>— Отправляйте ответы в приложение или службу с помощью действия **Ответ**. |
| [![Значок][azure-api-management-icon]<br>API**Управление API <br>Azure**][azure-api-management-doc] | Вызывайте триггеры и действия, определенные API-интерфейсами, которые вы публикуете и контролируете с помощью управления API Azure. | [![Значок][azure-app-services-icon]<br>API**службы приложений <br>Azure**][azure-app-services-doc] | Вызывайте приложения API Azure или веб-приложения, размещенные в Службе приложений Azure. Если включен Swagger, триггеры и действия, определенные этими приложениями, отображаются как любые другие триггеры и действия первого класса.|
| [![Значок][azure-logic-apps-icon]<br>API**Azure Logic <br>Apps**][nested-logic-app-doc] | Вызов других приложений логики, начинающихся с триггера **запроса** . |
|||||

### <a name="run-code-from-logic-apps"></a>Выполнение кода из приложений логики

Logic Apps предоставляет встроенные действия для выполнения собственного кода в рабочем процессе приложения логики:

|   |   |   |   |
|---|---|---|---|
| [![Значок][azure-functions-icon]<br>API**функции Azure**][azure-functions-doc] | Вызывайте функции Azure, которые запускают настраиваемые фрагменты кода (C# или Node.js) из ваших приложений логики. | [![][inline-code-icon]<br>**Встроенный код** значка API][azure-functions-doc] | Добавление и запуск фрагментов кода JavaScript из приложений логики. |
|||||

### <a name="control-workflow"></a>Рабочий процесс управления

Logic Apps предоставляет встроенные действия для структурирования и управления действиями в рабочем процессе приложения логики.

|   |   |   |   |
|---|---|---|---|
| [![][condition-icon]<br>**Условие** встроенного значка][condition-doc] | Оценка условия и выполнение различных действий на основе значения условия: True или False. | [![Встроенный значок][for-each-icon]<br>**для каждого**][for-each-doc] | Выполнение одинаковых действий для каждого элемента в массиве. |
| [![][scope-icon]<br>**Область** встроенных значков][scope-doc] | Группировка действий в *области*, которые получают свое состояние после завершения действий в области. | [![][switch-icon]<br>**Переключатель** встроенного значка][switch-doc] | Группируйте действия в *варианты*, которым присваиваются уникальные значения, за исключением варианта по умолчанию. Запускайте только тот вариант, присвоенное значение которого соответствует результату выражения, объекта или токена. Если совпадений не существует, запустите вариант по умолчанию. |
| [![][terminate-icon]<br>**Завершение** встроенного значка][terminate-doc] | Остановка выполнения активного рабочего процесса приложения логики. | [![Встроенный значок][until-icon]<br>**до**][until-doc] | Повторяйте действия до тех пор, пока значение указанного условия не будет равно True или какое-нибудь из состояний не изменится. |
|||||

### <a name="manage-or-manipulate-data"></a>Управление данными или их обработка

Logic Apps предоставляет встроенные действия для работы с выходными данными и их форматами:

|   |   |
|---|---|
| [![][data-operations-icon]<br>**Операции с данными** встроенных значков][data-operations-doc] | Выполнение операций с данными: <p>- **Создание**. Создает один вывод из нескольких входных данных с различными типами. <br>- **Создание таблицы CSV**. Создает таблицу с разделителями-запятыми (CSV) из массива с объектами JSON. <br>- **Создание таблицы HTML**. Создает таблицу HTML из массива с объектами JSON. <br>- **Фильтрация массива**. Создает массив из элементов другого массива, соответствующих вашим критериям. <br>- **Объединение**. Создает строку из всех элементов массива и разделяет эти элементы указанным разделителем. <br>- **Анализ JSON**: создание понятных пользователю маркеров из свойств и их значений в содержимом JSON, чтобы можно было использовать эти свойства в рабочем процессе. <br>- **Выбор**. Создает массив с объектами JSON, преобразуя элементы или значения в другой массив и сопоставив эти элементы с указанными свойствами. |
| ![Значок встроенного действия][date-time-icon]<br>**Дата и время** | Выполнение операций с метками времени: <p>- **Добавление интервала к указанному времени**. Добавляет указанное количество единиц в метку времени. <br>- **Преобразование часового пояса**. Преобразовывает метку времени из исходного часового пояса в целевой. <br>- **Текущее время**. Возвращает текущую метку времени в виде строки. <br>- **Получение будущего времени**. Возвращает текущую метку времени, а также указанные единицы времени. <br>- **Получение времени в прошлом**. Возвращает текущую метку времени, вычитая указанные единицы времени. <br>- **Вычитание из указанного времени**. Вычитает количество единиц времени из метки времени. |
| [![Встроенные][variables-icon]<br>**переменные** значков][variables-doc] | Выполнение операций с переменными: <p>- **Добавление значения в переменную массива**. Вставляет значение в качестве последнего элемента в массив, хранящийся в переменной. <br>- **Добавление в переменную строки**. Вставляет значение в качестве последнего символа в строку, хранящуюся в переменной. <br>- **Переменная декремента**. Уменьшает переменную на фиксированное значение. <br>- **Увеличение переменной**. Увеличивает переменную на фиксированное значение. <br>- **Инициализация переменной**. Создает переменную и объявляет ее тип данных и начальное значение. <br>- **Задание переменной**. Назначает другое значение имеющейся переменной. |
|  |  |

<a name="managed-api-connectors"></a>

## <a name="managed-connectors"></a>Управляемые соединители

Logic Apps предоставляет эти популярные стандартные соединители для автоматизации задач, процессов и рабочих процессов с этими службами или системами:

|   |   |   |   |
|---|---|---|---|
| [![Значок][azure-service-bus-icon]<br>API**служебная шина Azure**][azure-service-bus-doc] | Управление асинхронными сообщениями, сеансами и подписками на темы с помощью наиболее часто используемого соединителя в системе Logic Apps. | [![Значок][sql-server-icon]<br>API**SQL Server**][sql-server-doc] | Подключитесь SQL Server к локальной службе или базе данных SQL Azure в облаке, чтобы можно было управлять записями, выполнять хранимые процедуры или выполнять запросы. |
| [![Значок][azure-blob-storage-icon]<br>API**хранилище BLOB<br>-объектов Azure**][azure-blob-storage-doc] | Подключитесь к учетной записи хранения, чтобы можно было создавать и управлять содержимым больших двоичных объектов. | [![Значок][office-365-outlook-icon]<br>API**Office 365<br>Outlook**][office-365-outlook-doc] | Подключитесь к учетной записи электронной почты Office 365, чтобы вы могли создавать и администрировать сообщения электронной почты, задачи, события календаря и собрания, контакты, запросы и многое другое. |
| [![Значок][sftp-ssh-icon]<br>API**SFTP-SSH**][sftp-ssh-doc] | Подключитесь к SFTP-серверам, к которым можно получить доступ из Интернета с помощью SSH, чтобы можно было работать с файлами и папками. | [![Значок][sharepoint-online-icon]<br>API**SharePoint<br>Online**][sharepoint-online-doc] | Подключитесь к SharePoint Online, чтобы можно было управлять файлами, вложениями, папками и т. д. | 
| [![Значок][dynamics-365-icon]<br>API**Dynamics 365<br> **][dynamics-365-doc] | Подключитесь к учетной записи Dynamics 365, чтобы можно было создавать и администрировать записи, элементы и многое другое. | [![Значок][azure-queues-icon]<br>API**очереди <br>Azure**][azure-queues-doc] | Подключитесь к учетной записи хранения Azure, чтобы можно было создавать очереди и сообщения и управлять ими. |
| [![Значок][ftp-icon]<br>API**FTP**][ftp-doc] | Подключитесь к FTP-серверам, к которым можно получить доступ из Интернета, чтобы можно было работать с файлами и папками. | [![Значок][file-system-icon]<br>API**Файловая <br>система**][file-system-doc] | Подключитесь к локальной общей папке, чтобы можно было создавать файлы и управлять ими. |
| [![Значок][azure-event-hubs-icon]<br>API**концентраторы событий Azure**][azure-event-hubs-doc] | Использование событий и их публикация в концентраторе событий. Например, получите выходные данные из приложения логики с помощью соединителя Центров событий, а затем отправьте эти сведения любому поставщику аналитики в реальном времени. | [![Значок][azure-event-grid-icon]<br>API служба "<br>**Сетка** **событий Azure**"][azure-event-grid-doc] | Мониторинг событий, публикуемых службой "Сетка событий Azure", например при изменении ресурсов Azure или сторонних ресурсов. |
| [![Значок][salesforce-icon]<br>API**Salesforce**][salesforce-doc] | Подключитесь к учетной записи Salesforce, чтобы вы могли создавать такие элементы, как записи, задания, объекты и многое другое, и управлять ими. | [![Значок][twitter-icon]<br>API**Twitter**][twitter-doc] | Подключитесь к учетной записи Twitter, чтобы вы могли управлять твитами, подписчиков, временной шкалой и многое другое. Хранение твитов в SQL, Excel или SharePoint. |
|||||

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Локальные соединители

Ниже приведено несколько часто используемых стандартных соединителей, которые Logic Apps обеспечивают доступ к данным и ресурсам в локальных системах. Прежде чем создавать подключение к локальной системе, необходимо [загрузить, установить и настроить локальный шлюз данных ][gateway-doc]. Этот шлюз предоставляет безопасный коммуникационный канал без необходимости настройки обязательной сетевой инфраструктуры.

|   |   |   |   |   |
|---|---|---|---|---|
| [![Значок][biztalk-server-icon]<br>API**BizTalk** <br> **Server**][biztalk-server-doc] | [![Значок][file-system-icon]<br>API**Файловая <br>система**][file-system-doc] | [![Значок][ibm-db2-icon]<br>API**IBM DB2**][ibm-db2-doc] | [![Значок][ibm-informix-icon]<br>API**IBM** <br> **Informix**][ibm-informix-doc] | [![Значок][mysql-icon]<br>API**MySQL**][mysql-doc] |
| [![Значок][oracle-db-icon]<br>API**Oracle DB**][oracle-db-doc] | [![Значок][postgre-sql-icon]<br>API**PostgreSQL**][postgre-sql-doc] | [![Значок][sharepoint-server-icon]<br>API**SharePoint <br>Server**][sharepoint-server-doc] | [![Значок][sql-server-icon]<br>API**SQL <br>Server**][sql-server-doc] | [![Значок][teradata-icon]<br>API**Teradata**][teradata-doc] |
|||||

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Соединители для учетной записи интеграции

Logic Apps предоставляет стандартные соединители для создания решений B2B с помощью приложений логики при создании и оплате [учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), которая доступна в пакет интеграции Enterprise (EIP) в Azure. С помощью этой учетной записи вы можете создавать и хранить артефакты B2B, такие как деловые партнеры, соглашения, карты, схемы, сертификаты и т.д. Чтобы использовать эти артефакты, свяжите свои приложения логики с учетной записью интеграции. Если в настоящее время вы используете BizTalk Server, эти соединители могут быть вам знакомы.

|   |   |   |   |
|---|---|---|---|
| [![Значок][as2-icon]<br>API**декодирование AS2 <br>**][as2-doc] | [![Значок][as2-icon]<br>API**кодирование <br>AS2**][as2-doc] | [![Значок][edifact-icon]<br>API**декодирование EDIFACT <br>**][edifact-decode-doc] | [![Значок][edifact-icon]<br>API**EDIFACT <br>Encoding**][edifact-encode-doc] |
| [![Значок][flat-file-decode-icon]<br>API**декодирование <br>неструктурированного файла**][flat-file-decode-doc] | [![Значок][flat-file-encode-icon]<br>API**кодировка <br>неструктурированного файла**][flat-file-encode-doc] | [![Значок][integration-account-icon]<br>API**учетная запись интеграции <br>**][integration-account-doc] | [![Значок][liquid-icon]<br>API **преобразования** **жидкостей** <br>][json-liquid-transform-doc] |
| [![Значок][x12-icon]<br>API**декодирование X12 <br>**][x12-decode-doc] | [![Значок][x12-icon]<br>API**X12 <br>Encoding**][x12-encode-doc] | [![Значок][xml-transform-icon]<br>API **преобразования** **XML-кода** <br>][xml-transform-doc] | [![Значок][xml-validate-icon]<br>API**Проверка <br>XML**][xml-validate-doc] |  
|||||

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Корпоративные соединители

Logic Apps предоставляет эти соединители предприятия для доступа к корпоративным системам, таким как SAP и IBM MQ:

|   |   |   |
|---|---|---|
| [![Значок][ibm-3270-icon]<br>API**IBM 3270**][ibm-3270-doc] | [![Значок][ibm-mq-icon]<br>API**IBM MQ**][ibm-mq-doc] | [![Значок][sap-icon]<br>API**SAP**][sap-connector-doc] |
||||

<a name="ise-connectors"></a>

## <a name="ise-connectors"></a>Соединители ISE

Для приложений логики, которые создают и запускают изолированную [среду службы интеграции (ISE)](#integration-service-environment), конструктор приложений логики определяет встроенные триггеры и действия, выполняемые в интегрированной среде разработки с помощью **основной** метки. Управляемые соединители, работающие в интегрированной среде сценариев, отображают метку **ISE** , а соединители, работающие в глобальной многоклиентской Logic Apps службе, не отображают ни одну метку. В этом списке показаны соединители, которые в настоящее время имеют версии ISE:

|   |   |   |   |   |
|---|---|---|---|---|
[![Значок][as2-icon]<br>API**AS2**][as2-doc] | [![Значок][azure-automation-icon]<br>API Служба**автоматизации Azure <br>**][azure-automation-doc] | [![Значок][azure-blob-storage-icon]<br>API**хранилище BLOB<br>-объектов Azure**][azure-blob-storage-doc] | [![Значок][azure-cosmos-db-icon]<br>API**Azure Cosmos <br> DB**][azure-cosmos-db-doc] | [![Значок][azure-event-hubs-icon]<br>API**концентраторы <br>событий Azure**][azure-event-hubs-doc] |
[![Значок][azure-event-grid-icon]<br>API служба "**Сетка событий <br>Azure** "][azure-event-grid-doc] | [![Значок][azure-file-storage-icon]<br>API**хранилище файлов<br>Azure**][azure-file-storage-doc] | [![Значок][azure-key-vault-icon]<br>API**хранилище ключей <br>Azure**][azure-key-vault-doc] | [![Значок][azure-monitor-logs-icon]<br>API**Azure Monitor <br>журналы**][azure-monitor-logs-doc] | [![Значок][azure-service-bus-icon]<br>API**служебная <br>шина Azure**][azure-service-bus-doc] |
| [![Значок][azure-sql-data-warehouse-icon]<br>API**хранилище <br>данных SQL Azure**][azure-sql-data-warehouse-doc] | [![Значок][azure-table-storage-icon]<br>API**хранилище таблиц <br>Azure**][azure-table-storage-doc] | [![Значок][azure-queues-icon]<br>API**очереди <br>Azure**][azure-queues-doc] | [![Значок][edifact-icon]<br>API**EDIFACT**][edifact-doc] | [![Значок][file-system-icon]<br>API**Файловая <br>система**][file-system-doc] |
| [![Значок][ftp-icon]<br>API**FTP**][ftp-doc] | [![Значок][ibm-3270-icon]<br>API**IBM 3270**][ibm-3270-doc] | [![Значок][ibm-db2-icon]<br>API**IBM DB2**][ibm-db2-doc] | [![Значок][ibm-mq-icon]<br>API**IBM MQ**][ibm-mq-doc] | [![Значок][sap-icon]<br>API**SAP**][sap-connector-doc] |
| [![Значок][sftp-ssh-icon]<br>API**SFTP-SSH**][sftp-ssh-doc] | [![Значок][smtp-icon]<br>API**SMTP**][smtp-doc] | [![Значок][sql-server-icon]<br>API**SQL <br>Server**][sql-server-doc] | [![Значок][x12-icon]<br>API**X12**][x12-doc] |
||||||

Дополнительные сведения см. в следующих статьях:

* [Доступ к ресурсам виртуальной сети Azure из Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Модель ценообразования приложений логики](../logic-apps/logic-apps-pricing.md)
* [Подключение к виртуальным сетям Azure из Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="triggers-actions"></a>

## <a name="triggers-and-action-types"></a>Триггеры и типы действий

Соединители могут предоставлять *триггеры*, *действия*или и то, и другое. *Триггер* — это первый шаг в любом приложении логики, который обычно указывает на событие, которое вызывает срабатывание триггера, и начинает выполнение приложения логики. Например, соединитель FTP имеет триггер, который запускает приложение логики при добавлении или изменении файла. Некоторые триггеры регулярно проверяют указанное событие или данные, а затем срабатывают при обнаружении указанного события или данных. Другие триггеры ожидают, но сразу же срабатывают, когда происходит определенное событие или когда новые данные доступны. Триггеры также передают все необходимые данные в приложение логики. Приложение логики может считывать и использовать эти данные в рамках рабочего процесса. Например, соединитель Twitter имеет триггер «при публикации нового твита», который передает содержимое твита в рабочий процесс приложения логики.

После срабатывания триггера Azure Logic Apps создает экземпляр приложения логики и запускает выполнение *действий* в рабочем процессе приложения логики. Действия — это шаги, которые следуют за триггером и выполняют задачи в рабочем процессе приложения логики. Например, можно создать приложение логики, которое получает данные клиента из базы данных SQL и обрабатывает эти данные в последующих действиях.

Ниже приведены общие типы триггеров, которые Azure Logic Apps предоставляет:

* *Триггер повторения*. Этот триггер запускается по указанному расписанию и не тесно связан с определенной службой или системой.

* *Триггер опроса*. Этот триггер регулярно опрашивает определенную службу или систему на основе указанного расписания, проверяет наличие новых данных или указывает, произошло ли определенное событие. Если новые данные доступны или произошло определенное событие, триггер создает и запускает новый экземпляр приложения логики, который теперь может использовать данные, передаваемые в качестве входных данных.

* *Триггер push-уведомлений*. Этот триггер ожидает и прослушивает новые данные или событие. Когда новые данные доступны или событие происходит, триггер создает и запускает новый экземпляр приложения логики, который теперь может использовать данные, передаваемые в качестве входных данных.

<a name="connections"></a>

## <a name="connector-configuration"></a>Конфигурация соединителя

Триггеры и действия каждого соединителя предоставляют собственные свойства для настройки. Во многих соединителях также требуется сначала создать *Подключение* к целевой службе или системе и предоставить учетные данные для проверки подлинности или другие сведения о конфигурации, прежде чем можно будет использовать триггер или действие в приложении логики. Например, необходимо авторизовать подключение к учетной записи Twitter для доступа к данным или для публикации от вашего имени.

Для соединителей, использующих Azure Active Directory (Azure AD) OAuth, создание подключения означает вход в службу, например Office 365, Salesforce или GitHub, где маркер доступа [шифруется](../security/fundamentals/encryption-overview.md) и безопасно хранится в хранилище секретов Azure. Для других соединителей, таких как FTP и SQL, требуется подключение с подробными сведениями о конфигурации, например адрес сервера, имя пользователя и пароль. Эти сведения о конфигурации подключения также безопасно хранятся в зашифрованном виде. Дополнительные сведения о [шифровании в Azure](../security/fundamentals/encryption-overview.md).

Подключения могут обращаться к целевой службе или системе в течение времени, в течение которого эта служба или система допускает. Для служб, использующих подключения OAuth Azure AD, такие как Office 365 и Dynamics, Azure Logic Apps обновляет маркеры доступа бесконечно. Другие службы могут иметь ограничения на время, в течение которого Azure Logic Apps может использовать маркер без обновления. Как правило, некоторые действия делают недействительными все маркеры доступа, например изменение пароля.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Пользовательские API и соединители

Чтобы вызывать API-интерфейсы, которые запускают пользовательский код или недоступны в качестве соединителей, можно расширить платформу Logic Apps, [создавая настраиваемые приложения API](../logic-apps/logic-apps-create-api-app.md). Кроме того, вы можете [создавать пользовательские соединители](../logic-apps/custom-connector-overview.md) для *любых* API на основе REST или SOAP, которые делают эти API доступными для любого приложения логики в вашей подписке Azure. Чтобы пользовательские приложения API или соединители были доступными для любого пользователя в Azure, вы можете [отправить соединители для сертификации Майкрософт](../logic-apps/custom-connector-submit-certification.md).

> [!NOTE]
> Приложения логики, развертываемые и выполняемые в [среде службы интеграции (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) , могут напрямую обращаться к ресурсам в виртуальной сети Azure. Если у вас есть пользовательские соединители, для которых требуется локальный шлюз данных, и вы создали эти соединители за пределами интегрированной среды сценариев, то приложения логики в интегрированной среде сценариев также могут использовать эти соединители.
>
> Настраиваемые соединители, созданные в интегрированной среде сценариев, не работают с локальным шлюзом данных. Однако эти соединители могут напрямую обращаться к локальным источникам данных, подключенным к виртуальной сети Azure, в которой размещена интегрированная среда сценариев. Таким образом, приложения логики в интегрированной среде сценариев, скорее всего, не нуждаются в шлюзе данных при взаимодействии с этими ресурсами.
>
> Дополнительные сведения о создании Исес см. в статье [Подключение к виртуальным сетям Azure из Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

## <a name="next-steps"></a>Дальнейшие действия

* Просмотр [полного списка соединителей](https://docs.microsoft.com/connectors)
* [Создание первого приложения логики](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Обзор пользовательских соединителей](https://docs.microsoft.com/connectors/custom-connectors/)
* [Создание настраиваемых API для приложений логики](../logic-apps/logic-apps-create-api-app.md)

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[inline-code-icon]: ./media/apis-list/inline-code.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed connector icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-cosmos-db-icon]: ./media/apis-list/azure-cosmos-db.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-file-storage-icon]: ./media/apis-list/azure-file-storage.png
[azure-key-vault-icon]: ./media/apis-list/azure-key-vault.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-monitor-logs-icon]: ./media/apis-list/azure-monitor-logs.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[azure-sql-data-warehouse-icon]: ./media/apis-list/azure-sql-data-warehouse.png
[azure-table-storage-icon]: ./media/apis-list/azure-table-storage.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-3270-icon]: ./media/apis-list/ibm-3270.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-ssh-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png

<!--Other doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Подключение к локальным источникам данных из приложений логики с помощью локального шлюза данных"

<!--Built-in doc links-->
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Создание экземпляра службы управления API Azure для управления и публикации API-интерфейсов"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Интеграция приложений логики с приложениями API службы приложений"
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Интеграция приложений логики с помощью функций Azure"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Обработка сообщений в группах или в качестве пакетов"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Оценка условия и выполнение различных действий в зависимости от того, имеет ли условие значение true или false"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Выполнение одних и тех же действий для каждого элемента в массиве"
[http-doc]: ./connectors-native-http.md "Вызов конечных точек HTTP или HTTPS из приложений логики"
[http-request-doc]: ./connectors-native-reqres.md "Получение HTTP-запросов в приложениях логики"
[http-response-doc]: ./connectors-native-reqres.md "Реагирование на HTTP-запросы из приложений логики"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Вызов конечных точек RESTFUL из приложений логики"
[http-webhook-doc]: ./connectors-native-webhook.md "Ожидание конкретных событий от конечных точек HTTP или HTTPS"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Интеграция приложений логики с вложенным рабочим процессом."
[query-doc]: ../logic-apps/logic-apps-perform-data-operations.md#filter-array-action "Выбор и фильтрации массивов с помощью действия "Запрос""
[schedule-doc]: ../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md "Запускать приложения логики на основе расписания"
[schedule-delay-doc]: ./connectors-native-delay.md "Задержка выполнения следующего действия"
[schedule-delay-until-doc]: ./connectors-native-delay.md "Задержка выполнения следующего действия"
[schedule-recurrence-doc]:  ./connectors-native-recurrence.md "Запуск приложений логики по повторяющемуся расписанию"
[schedule-sliding-window-doc]: ./connectors-native-sliding-window.md "Запуск приложений логики, требующих обработки данных в смежных фрагментах"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Упорядочение действий в группы, которые получают свой статус после окончания выполнения действий в группе"
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Упорядочите действия в варианты, которым присваиваются уникальные значения. Выполните только тот случай, значение которого соответствует результату выражения, объекта или токена. Если совпадений не существует, выполните вариант по умолчанию."
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Остановка или отмена активного рабочего процесса для приложения логики"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Повторять действия до тех пор, пока заданное условие не будет равно true или изменилось некоторое состояние"
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Выполнение операций с данными, таких как фильтрация массивов или создание таблиц CSV и HTML"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Выполнение операций с переменными, такими как инициализация, набор, увеличение, декремент и добавление к строке или переменной массива"

<!--Managed connector doc links-->
[azure-automation-doc]: https://docs.microsoft.com/connectors/azureautomation/ "Создание заданий службы автоматизации для облачной и локальной инфраструктуры и управление ими"
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Управление файлами в контейнере больших двоичных объектов с помощью соединителя хранилища BLOB-объектов Azure"
[azure-cosmos-db-doc]: https://docs.microsoft.com/connectors/documentdb/ "Подключение к Azure Cosmos DB для доступа к документам и хранимым процедурам"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Наблюдение за событиями, опубликованными службой "Сетка событий", например при изменении ресурсов Azure или ресурсов сторонних производителей"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Подключение к концентраторам событий Azure для получения и отправки событий между приложениями логики и концентраторами событий"
[azure-file-storage-doc]: https://docs.microsoft.com/connectors/azurefile/ "Подключитесь к учетной записи хранения Azure, чтобы можно было создавать, обновлять, получать и удалять файлы."
[azure-key-vault-doc]: https://docs.microsoft.com/connectors/keyvault/ "Подключитесь к Azure Key Vault, чтобы можно было управлять секретами и ключами."
[azure-monitor-logs-doc]: https://docs.microsoft.com/connectors/azuremonitorlogs/ "Выполнение запросов к журналам Azure Monitor в Log Analytics рабочих областях и компонентах Application Insights"
[azure-queues-doc]: https://docs.microsoft.com/connectors/azurequeues/ "Подключитесь к учетной записи хранения Azure, чтобы можно было создавать очереди и сообщения и управлять ими."
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Отправка сообщений из очередей и разделов служебной шины, а также получение сообщений из очередей и подписок служебной шины"
[azure-sql-data-warehouse-doc]: https://docs.microsoft.com/connectors/sqldw/ "Подключение к хранилищу данных SQL Azure для просмотра данных"
[azure-table-storage-doc]: https://docs.microsoft.com/connectors/azuretables/ "Подключитесь к учетной записи хранения Azure, чтобы можно было создавать, обновлять и запрашивать таблицы и многое другое."
[biztalk-server-doc]: https://docs.microsoft.com/connectors/biztalk/ "Подключитесь к BizTalk Server, чтобы можно было запускать приложения на основе BizTalk параллельно с Azure Logic Apps"
[box-doc]: ./connectors-create-api-box.md "Подключиться к Box. Отправка, получение, удаление, перечисление файлов и многое другое"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Подключитесь к Dropbox. Отправка, получение, удаление, перечисление файлов и многое другое"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Подключение к Dynamics CRM Online для работы с данными CRM Online"
[facebook-doc]: ./connectors-create-api-facebook.md "Подключитесь к Facebook. Публикация на временной шкале, получение веб-канала страницы и многое другое"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Подключение к локальной файловой системе"
[ftp-doc]: ./connectors-create-api-ftp.md "Подключение к FTP и FTPS-серверу для выполнения разных FTP-задач, включая отправку, получение, удаление файлов и другие действия."
[github-doc]: ./connectors-create-api-github.md "Подключение к GitHub и отслеживание проблем"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Подключается к Google календарю и может управлять календарем"
[google-drive-doc]: ./connectors-create-api-googledrive.md "Подключение к GoogleDrive, чтобы можно было работать с данными"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Подключение к Google таблицам, чтобы можно было изменять листы"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Подключается к задачам Google, чтобы вы могли управлять своими задачами."
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "Подключение к приложениям 3270 на мэйнфреймах IBM"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Подключитесь к IBM DB2 в облаке или в локальной среде. Обновление строки, получение таблицы и многое другое"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Подключитесь к Informix в облаке или в локальной среде. Чтение строки, перечисление таблиц и многое другое"
[ibm-mq-doc]: ./connectors-create-api-mq.md "Подключение к IBM MQ в локальной среде или Azure для отправки и получения сообщений"
[instagram-doc]: ./connectors-create-api-instagram.md "Подключитесь к Instagram. Активировать или исдействовать события"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Подключитесь к учетной записи MailChimp. Управление почтой и ее Автоматизация"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Подключение к Mandrill для обмена данными"
[mysql-doc]: https://docs.microsoft.com/connectors/mysql/ "Подключитесь к локальной базе данных MySQL, чтобы можно было читать и записывать данные."
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Подключитесь к учетной записи Office 365, чтобы вы могли отправлять и получать сообщения электронной почты, управлять календарем и контактами и многое другое."
[office-365-users-doc]: ./connectors-create-api-office365-users.md
[onedrive-doc]: ./connectors-create-api-onedrive.md "Подключитесь к личному приложению Microsoft OneDrive, чтобы вы могли отправлять, удалять, перечислять файлы и многое другое."
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Подключитесь к Microsoft OneDrive для бизнеса, чтобы вы могли отправлять, удалять, перечислять файлы и многое другое."
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Подключение к базе данных Oracle, чтобы можно было добавлять, вставлять, удалять строки и многое другое."
[outlook.com-doc]: ./connectors-create-api-outlook.md "Подключение к почтовому ящику Outlook для управления электронной почтой, календарями, контактами и т. д."
[postgre-sql-doc]: https://docs.microsoft.com/connectors/postgresql/ "Подключитесь к базе данных PostgreSQL, чтобы можно было считывать данные из таблиц."
[project-online-doc]: ./connectors-create-api-projectonline.md "Подключение к Microsoft Project Online для управления проектами, задачами, ресурсами и т. д."
[rss-doc]: ./connectors-create-api-rss.md "Публикация и получение элементов канала, активация операций при публикации нового элемента в RSS-канале"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Подключитесь к учетной записи Salesforce. Управление учетными записями, интересами, возможностями и др."
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Подключение к локальному серверу SAP"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Подключитесь к SendGrid. Отправка электронной почты и управление списками получателей"
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "Подключитесь к учетной записи SFTP с помощью SSH. Отправка, получение, удаление файлов и многое другое"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Подключитесь к локальному серверу SharePoint. Управление документами, элементами списков и т. д."
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Подключитесь к SharePoint Online. Управление документами, элементами списков и т. д."
[slack-doc]: ./connectors-create-api-slack.md "Подключение к резервному времени и отправка сообщений в каналы временного резерва"
[smtp-doc]: ./connectors-create-api-smtp.md "Подключение к SMTP-серверу и отправка электронных сообщений с вложениями"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Подключение к SparkPost для обмена данными"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Подключитесь к базе данных SQL Azure или SQL Server. Создание, обновление, получение и удаление записей в таблице базы данных SQL"
[teradata-doc]: https://docs.microsoft.com/connectors/teradata/ "Подключение к базе данных Teradata для чтения данных из таблиц"
[trello-doc]: ./connectors-create-api-trello.md "Подключитесь к Trello. Управляйте своими проектами и организуйте что угодно"
[twilio-doc]: ./connectors-create-api-twilio.md "Подключитесь к Twilio. Отправка и получение сообщений, получение доступных номеров, управление входящими телефонными номерами и многое другое"
[twitter-doc]: ./connectors-create-api-twitter.md "Подключитесь к Twitter. Получение временных шкал, публикация твитов и многое другое"
[yammer-doc]: ./connectors-create-api-yammer.md "Подключитесь к Yammer. Публикация сообщений, получение новых сообщений и многое другое"
[youtube-doc]: ./connectors-create-api-youtube.md "Подключитесь к YouTube. Управляйте своими видео и каналами"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Кодирование и декодирование сообщений, использующих протокол AS2"
[edifact-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "Кодирование и декодирование сообщений, использующих протокол EDIFACT"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Декодирование сообщений, использующих протокол EDIFACT"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Кодирование сообщений, использующих протокол EDIFACT"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Сведения о неструктурированном файле интеграции Enterprise"
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Сведения о неструктурированном файле интеграции Enterprise"
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Управление метаданными для артефактов учетной записи интеграции"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Преобразование JSON с шаблонами жидкостей"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Кодирование и декодирование сообщений, использующих протокол X12"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Декодирование сообщений, использующих протокол X12"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Кодирование сообщений, использующих протокол X12"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Преобразование XML-сообщений"
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Проверка XML-сообщений"

