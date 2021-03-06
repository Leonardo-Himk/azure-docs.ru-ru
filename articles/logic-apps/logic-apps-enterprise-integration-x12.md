---
title: Отправка и получение сообщений X12 для B2B
description: Обмен сообщениями X12 для сценариев интеграции B2B Enterprise с помощью Azure Logic Apps с Пакет интеграции Enterprise
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/29/2020
ms.openlocfilehash: 8ec20e03544ba54b83130ae41244dcdb186252d0
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82613090"
---
# <a name="exchange-x12-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Обмен сообщениями X12 для интеграции с предприятием B2B в Azure Logic Apps с помощью Пакета интеграции Enterprise

Для работы с сообщениями X12 в Azure Logic Apps можно использовать соединитель X12, который предоставляет триггеры и действия для управления связью X12. Дополнительные сведения о EDIFACT сообщениях см. в разделе [Exchange EDIFACT messages](logic-apps-enterprise-integration-edifact.md).

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас еще нет подписки Azure, [получите бесплатную учетную запись Azure](https://azure.microsoft.com/free/).

* Приложение логики, из которого вы хотите использовать соединитель X12, и триггер, который запускает рабочий процесс приложения логики. Соединитель X12 предоставляет только действия, а не триггеры. Если вы не работали с приложениями логики, см. руководства по [Azure Logic Apps](../logic-apps/logic-apps-overview.md) и [созданию первого приложения логики](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* [Учетная запись интеграции](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , связанная с подпиской Azure и связанная с приложением логики, где вы планируете использовать соединитель X12. Приложение логики и учетная запись интеграции должны находиться в одном расположении или регионе Azure.

* По крайней мере два [торговых партнера](../logic-apps/logic-apps-enterprise-integration-partners.md) , которые уже определены в учетной записи интеграции с помощью квалификатора удостоверения X12.

* [Схемы](../logic-apps/logic-apps-enterprise-integration-schemas.md) , используемые для проверки XML, уже добавленные в учетную запись интеграции. Если вы работаете со схемами акту о переносе и защите здравоохранения, см. статью [схемы HIPAA](#hipaa-schemas).

* Прежде чем можно будет использовать соединитель X12, необходимо создать [соглашение](../logic-apps/logic-apps-enterprise-integration-agreements.md) X12 между торговыми партнерами и сохранить это соглашение в учетной записи интеграции. Если вы работаете с схемами закона о защите и переносе страхового обеспечения здравоохранения, вам нужно добавить `schemaReferences` раздел в соглашение. Дополнительные сведения см. в разделе [схемы HIPAA](#hipaa-schemas).

<a name="receive-settings"></a>

## <a name="receive-settings"></a>"Receive Settings" (Параметры получения)

После задания свойств соглашения можно настроить, как это соглашение идентифицирует и обрабатывает входящие сообщения, получаемые от партнера с помощью данного соглашения.

1. В колонке **Добавить** выберите **Настройки получения**.

1. Настройте эти свойства в соответствии с вашим соглашением с партнером, с которым вы будете обмениваться сообщениями. **Параметры получения** организованы в следующие разделы:

   * [Идентификаторы](#inbound-identifiers)
   * [Синхронизации](#inbound-acknowledgement)
   * [Схемы](#inbound-schemas)
   * [Envelopes (Оболочка)](#inbound-envelopes)
   * [Control Numbers (Контрольные номера)](#inbound-control-numbers)
   * [Validations](#inbound-validations)
   * [Internal Settings (Внутренние параметры)](#inbound-internal-settings)

   Ниже приведены таблицы с описанием свойств раздела настроек отправки.

1. Когда все будет готово, сохраните параметры, нажав **кнопку ОК**.

<a name="inbound-identifiers"></a>

### <a name="receive-settings---identifiers"></a>Параметры получения — идентификаторы

![Свойства идентификатора для входящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-receive-settings-identifiers.png)

| Свойство | Описание |
|----------|-------------|
| **ISA1 (Authorization Qualifier) (ISA1 (квалификатор авторизации))** | Значение квалификатора авторизации, которое вы хотите использовать. Значение по умолчанию — **00 — сведения об авторизации**отсутствуют. <p>**Примечание**. Если выбрать другие значения, укажите значение для свойства **ISA2** . |
| **ISA2** | Значение данных авторизации, используемое, если свойство **ISA1** не равно **00. сведения об авторизации**отсутствуют. Значение этого свойства должно содержать не менее одного алфавитно-цифрового символа и не больше 10. |
| **ISA3 (Security Qualifier) (ISA3 (квалификатор безопасности))** | Значение квалификатора безопасности, которое вы хотите использовать. Значение по умолчанию — **00 — сведения о безопасности отсутствуют**. <p>**Примечание**. Если выбрать другие значения, укажите значение для свойства **ISA4** . |
| **ISA4** | Значение сведений о безопасности, используемое, если свойство **ISA3** не равно **00, сведения о безопасности**отсутствуют. Значение этого свойства должно содержать не менее одного алфавитно-цифрового символа и не больше 10. |
|||

<a name="inbound-acknowledgement"></a>

### <a name="receive-settings---acknowledgement"></a>Параметры получения — подтверждение

![Подтверждение входящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-receive-settings-acknowledgement.png)

| Свойство | Описание |
|----------|-------------|
| **Ожидается TA1** | Возвращает техническое подтверждение (TA1) отправителю сообщения. |
| **Ожидается FA** | Возвращает функциональное подтверждение (FA) отправителю сообщения. <p>Для свойства **версия ОС** в зависимости от версии схемы выберите подтверждения 997 или 999. <p>Чтобы включить создание циклов AK2 в функциональных подтверждениях для принятых наборов транзакций, выберите **включить цикл AK2/IK2**. |
||||

<a name="inbound-schemas"></a>

### <a name="receive-settings---schemas"></a>Параметры получения — схемы

![Схемы для входящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-receive-settings-schemas.png)

В этом разделе Выберите [схему](../logic-apps/logic-apps-enterprise-integration-schemas.md) из [учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-accounts.md) для каждого типа транзакции (ST01) и приложения отправителя (GS02). Конвейер получения EDI обрабатывает входящее сообщение, сопоставляя значения и схему, заданные в этом разделе, со значениями для ST01 и GS02 во входящем сообщении и со схемой входящего сообщения. После завершения каждой строки автоматически появится новая пустая строка.

| Свойство | Описание |
|----------|-------------|
| **Версия** | Версия X12 для схемы |
| **Transaction Type (ST01) (Тип транзакции (ST01))** | Тип транзакции |
| **Sender Application (GS02) (Приложение отправителя (GS02))** | Приложение отправителя |
| **Схемы** | Файл схемы, который вы хотите использовать. |
|||

<a name="inbound-envelopes"></a>

### <a name="receive-settings---envelopes"></a>Параметры получения — конверты

![Разделители, используемые в наборах транзакций для входящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-receive-settings-envelopes.png)

| Свойство | Описание |
|----------|-------------|
| **ISA11 Usage (Использование ISA11)** | Разделитель, используемый в наборе транзакций: <p>- **Стандартный идентификатор**: используйте точку (.) для десятичной нотации, а не десятичную нотацию входящего документа в конвейере получения EDI. <p>- **Разделитель повторений**: Укажите разделитель для повторяющихся вхождений простого элемента данных или повторяющейся структуры данных. Например, в качестве разделителя повторений обычно используется карет (^). Для схем HIPPA можно использовать только карет. |
|||

<a name="inbound-control-numbers"></a>

### <a name="receive-settings---control-numbers"></a>Параметры получения — контрольные номера

![Обработка дубликатов контрольных номеров для входящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-receive-settings-control-numbers.png) 

| Свойство | Описание |
|----------|-------------|
| **Запретить дубликаты контрольного номера обмена** | Запрещает повторяющиеся операции обмена. Проверьте контрольный номер обмена (ISA13) для полученного контрольного номера обмена. При обнаружении совпадения конвейер получения EDI не обрабатывает обмен. <p><p>Чтобы указать число дней для выполнения проверки, введите значение для параметра **проверять наличие ПОВТОРЯЮЩИХСЯ ISA13 каждые (Days)** . |
| **Disallow Group control number duplicates (Запретить повторяющиеся контрольные номера групп)** | Блокировка взаимоизменений с повторяющимися контрольными номерами групп. |
| **Disallow Transaction set control number duplicates (Запретить повторяющиеся контрольные номера наборов транзакций)** | Блокировка взаимоизменений с повторяющимися контрольными номерами наборов транзакций. |
|||

<a name="inbound-validations"></a>

### <a name="receive-settings---validations"></a>Параметры получения — проверки

![Проверки входящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-receive-settings-validations.png)

В строке **по умолчанию** показаны правила проверки, используемые для типа сообщений EDI. Если необходимо определить другие правила, выберите каждое поле, для которого должно быть задано **значение true**. После завершения каждой строки автоматически появится новая пустая строка.

| Свойство | Описание |
|----------|-------------|
| **Тип сообщения** | Тип сообщений EDI |
| **EDI Validation (Проверка EDI)** | Выполняет проверку EDI для типов данных в соответствии со свойствами EDI схемы, ограничениями длины, пустыми элементами данных и конечными разделителями. |
| **Extended Validation (Расширенная проверка)** | Если тип данных не является EDI, проверка осуществляется в соответствии с требованием к элементу данных и разрешенным повторением, перечислениями и проверкой длины элемента данных (min или Max). |
| **Allow Leading/Trailing Zeroes (Разрешить начальные и конечные нули)** | Оставляйте любые дополнительные начальные или конечные нули и пробелы. Не удаляйте эти символы. |
| **Обрезать начальные и конечные нули** | Удалите все начальные или конечные нули и пробелы. |
| **Trailing Separator Policy (Политика конечных разделителей)** | Создает конечные разделители. <p>- **Не допускается**: запретить конечные разделители и разделитель во входящем обмене. Если обмен содержит конечные разделители, он будет объявлен недопустимым. <p>- **Необязательно**: принимать изменения с конечными разделителями и разделительами или без них. <p>- **Обязательный**: входящий обмен должен содержать конечные разделители и разделитель. |
|||

<a name="inbound-internal-settings"></a>

### <a name="receive-settings---internal-settings"></a>Параметры получения — внутренние параметры

![Внутренние параметры для входящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-receive-settings-internal-settings.png)

| Свойство | Описание |
|----------|-------------|
| **Преобразование неявного десятичного формата nn в базовое 10 числовое значение** | Преобразование номера EDI, указанного в формате "NN", в числовое значение с основанием 10. |
| **Create empty XML tags if trailing separators are allowed (Создать пустые теги XML, если конечные разделители разрешены)** | Отправитель обмена содержит пустые XML-теги для конечных разделителей. |
| **Разделение документа Interchange на наборы транзакций — заблокировать наборы транзакций при ошибке** | Проанализируйте каждый набор транзакций, находящихся в обмене, в отдельный XML-документ, применив соответствующий конверт к набору транзакций. Приостановите только те транзакции, в которых проверка не удалась. |
| **Разделение документа Interchange на наборы транзакций — заблокировать документ Interchange при ошибке** | Проанализируйте каждый набор транзакций, находящихся в обмене, в отдельный XML-документ, применив соответствующий конверт. Если один или несколько наборов транзакций, входящих в обмен, не проходят проверку, обработка останавливается для всего обмена. |
| **Сохранение документа Interchange — заблокировать наборы транзакций при ошибке** | Оставьте обмен неизменным и создайте XML-документ для всего пакетного обмена. Приостановите только те наборы транзакций, которые не прошли проверку, но продолжают обрабатывать все остальные наборы транзакций. |
| **Сохранение документа Interchange — заблокировать операцию обмена при ошибке** |Сохраняет операцию обмена неделимой, создавая XML-документ для всего пакетного обмена. Если один или несколько наборов транзакций, входящих в обмен, не проходят проверку, обработка останавливается для всего обмена. |
|||

<a name="send-settings"></a>

## <a name="send-settings"></a>"Send Settings" (Параметры отправки)

После задания свойств соглашения можно настроить, как это соглашение идентифицирует и обрабатывает исходящие сообщения, отправляемые партнеру по этому соглашению.

1. В колонке **Добавить** выберите **Настройки отправки**.

1. Настройте эти свойства в соответствии с вашим соглашением с партнером, с которым вы будете обмениваться сообщениями. Ниже приведены таблицы с описанием свойств раздела настроек отправки.

   **Параметры отправки** организованы в следующие разделы:

   * [Идентификаторы](#outbound-identifiers)
   * [Синхронизации](#outbound-acknowledgement)
   * [Схемы](#outbound-schemas)
   * [Envelopes (Оболочка)](#outbound-envelopes)
   * [Номер версии элемента управления](#outbound-control-version-number)
   * [Control Numbers (Контрольные номера)](#outbound-control-numbers)
   * [Character Sets and Separators (Наборы символов и разделители)](#outbound-character-sets-separators)
   * [Проверка](#outbound-validation)

1. Когда все будет готово, сохраните параметры, нажав **кнопку ОК**.

<a name="outbound-identifiers"></a>

### <a name="send-settings---identifiers"></a>Параметры отправки — идентификаторы

![Свойства идентификатора для исходящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-send-settings-identifiers.png)

| Свойство | Описание |
|----------|-------------|
| **ISA1 (Authorization Qualifier) (ISA1 (квалификатор авторизации))** | Значение квалификатора авторизации, которое вы хотите использовать. Значение по умолчанию — **00 — сведения об авторизации**отсутствуют. <p>**Примечание**. Если выбрать другие значения, укажите значение для свойства **ISA2** . |
| **ISA2** | Значение данных авторизации, используемое, если свойство **ISA1** не равно **00. сведения об авторизации**отсутствуют. Значение этого свойства должно содержать не менее одного алфавитно-цифрового символа и не больше 10. |
| **ISA3 (Security Qualifier) (ISA3 (квалификатор безопасности))** | Значение квалификатора безопасности, которое вы хотите использовать. Значение по умолчанию — **00 — сведения о безопасности отсутствуют**. <p>**Примечание**. Если выбрать другие значения, укажите значение для свойства **ISA4** . |
| **ISA4** | Значение сведений о безопасности, используемое, если свойство **ISA3** не равно **00, сведения о безопасности**отсутствуют. Значение этого свойства должно содержать не менее одного алфавитно-цифрового символа и не больше 10. |
|||

<a name="outbound-acknowledgement"></a>

### <a name="send-settings---acknowledgement"></a>Параметры отправки — подтверждение

![Свойства подтверждения исходящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-send-settings-acknowledgement.png)

| Свойство | Описание |
|----------|-------------|
| **Ожидается TA1** | Возвращает техническое подтверждение (TA1) отправителю сообщения. <p>Этот параметр указывает, что главный партнер, отправляющий сообщение, запрашивает подтверждение от гостевого партнера в соглашении. Эти подтверждения ожидаются ведущим партнером на основе параметров получения соглашения. |
| **Ожидается FA** | Возвращает функциональное подтверждение (FA) отправителю сообщения. Для свойства **версия ОС** в зависимости от версии схемы выберите подтверждения 997 или 999. <p>Этот параметр указывает, что главный партнер, отправляющий сообщение, запрашивает подтверждение от гостевого партнера в соглашении. Эти подтверждения ожидаются ведущим партнером на основе параметров получения соглашения. |
|||

<a name="outbound-schemas"></a>

### <a name="send-settings---schemas"></a>Параметры отправки — схемы

![Схемы для исходящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-send-settings-schemas.png)

В этом разделе Выберите [схему](../logic-apps/logic-apps-enterprise-integration-schemas.md) из [учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-accounts.md) для каждого типа транзакции (ST01). После завершения каждой строки автоматически появится новая пустая строка.

| Свойство | Описание |
|----------|-------------|
| **Версия** | Версия X12 для схемы |
| **Transaction Type (ST01) (Тип транзакции (ST01))** | Тип транзакции для схемы |
| **Схемы** | Файл схемы, который вы хотите использовать. Если сначала выбрать схему, то версия и тип транзакции будут установлены автоматически. |
|||

<a name="outbound-envelopes"></a>

### <a name="send-settings---envelopes"></a>Параметры отправки — конверты

![Разделители в наборе транзакций, используемом для исходящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-send-settings-envelopes.png)

| Свойство | Описание |
|----------|-------------|
| **ISA11 Usage (Использование ISA11)** | Разделитель, используемый в наборе транзакций: <p>- **Стандартный идентификатор**: используйте точку (.) для десятичной нотации, а не десятичную нотацию исходящего документа в конвейере отправки EDI. <p>- **Разделитель повторений**: Укажите разделитель для повторяющихся вхождений простого элемента данных или повторяющейся структуры данных. Например, в качестве разделителя повторений обычно используется карет (^). Для схем HIPPA можно использовать только карет. |
|||

<a name="outbound-control-version-number"></a>

### <a name="send-settings---control-version-number"></a>Параметры отправки — номер версии элемента управления

![Контрольный номер версии для исходящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-send-settings-control-version-number.png)

В этом разделе Выберите [схему](../logic-apps/logic-apps-enterprise-integration-schemas.md) из [учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-accounts.md) для каждого обмена. После завершения каждой строки автоматически появится новая пустая строка.

| Свойство | Описание |
|----------|-------------|
| **Control Version Number (ISA12) (Контрольный номер версии (ISA12))** | Версия X12 Standard |
| **Usage Indicator (ISA15) (Индикатор использования (ISA15))** | Контекст обмена, который является либо **тестовыми** данными, **информационными** данными, либо **рабочими** данными. |
| **Схемы** | Схема, используемая для создания сегментов GS и ST для X12ного обмена, отправляемого в конвейер отправки EDI. |
| **GS1** | (Необязательно) выберите функциональный код. |
| **GS2** | (Необязательно) укажите отправителя приложения. |
| **GS3** | Необязательно, укажите приемник приложения. |
| **GS4** | Необязательно, выберите **CCYYMMDD** или **YYMMDD**. |
| **GS5** | Необязательно, выберите **ЧЧММ**, **ЧЧММСС**или **ххммссдд**. |
| **GS7** | (Необязательно) выберите значение для ответственного агентства. |
| **GS8** | (Необязательно) укажите версию документа схемы. |
|||

<a name="outbound-control-numbers"></a>

### <a name="send-settings---control-numbers"></a>Параметры отправки — контрольные номера

![Контрольные номера исходящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-send-settings-control-numbers.png)

| Свойство | Описание |
|----------|-------------|
| **Interchange Control Number (ISA13) (Контрольный номер обмена (ISA13))** | Диапазон значений для контрольного номера обмена, который может иметь минимальное значение, равное 1, и максимальное значение 999999999 |
| **Group Control Number (GS06) (Контрольный номер группы (GS06))** | Диапазон значений для контрольного номера группы, который может иметь минимальное значение, равное 1, и максимальное значение 999999999 |
| **Transaction Set Control Number (ST02) (Контрольный номер набора транзакций (ST02))** | Диапазон значений для контрольного номера набора транзакций, который может иметь минимальное значение, равное 1, и максимальное значение 999999999 <p>- **Prefix**: необязательное, буквенно-цифровое значение <br>- **Суффикс**: необязательное, буквенно-цифровое значение |
|||

<a name="outbound-character-sets-separators"></a>

### <a name="send-settings---character-sets-and-separators"></a>Параметры отправки — наборы символов и разделители

![Разделители для типов сообщений в исходящих сообщениях](./media/logic-apps-enterprise-integration-x12/x12-send-settings-character-sets-separators.png)

В строке **по умолчанию** показан набор символов, используемый в качестве разделителей для схемы сообщений. Если вы не хотите использовать кодировку **по умолчанию** , можно ввести другой набор разделителей для каждого типа сообщений. После завершения каждой строки автоматически появится новая пустая строка.

> [!TIP]
> Чтобы ввести значения со специальными знаками, откройте соглашение в редакторе как JSON-файл и укажите значение ASCII для специального знака.

| Свойство | Описание |
|----------|-------------|
| **Character Set to be used (Используемый набор символов)** | X12 кодировка — **Basic**, **Extended**или **UTF8**. |
| **Схемы** | Схема, которую вы хотите использовать. После выбора схемы выберите нужную кодировку на основе приведенных ниже описаний разделителя. |
| **Тип входных данных** | Тип входных данных для кодировки |
| **Разделитель компонентов** | Отдельный символ, разделяющий составные элементы данных |
| **Разделитель элементов данных** | Отдельный символ, разделяющий простые элементы данных в составных данных |
| **Разделитель символов замены** | Символ замены, заменяющий все символы-разделители в полезных данных при создании исходящего сообщения X12 |
| **Признак конца сегмента** | Одиночный символ, указывающий конец сегмента EDI |
| **Суффикс** | Символ, используемый с идентификатором сегмента. Если указать суффикс, элемент данных конца сегмента может быть пустым. Если терминатор сегмента оставить пустым, необходимо указать суффикс. |
|||

<a name="outbound-validation"></a>

### <a name="send-settings---validation"></a>Параметры отправки — проверка

![Свойства проверки для исходящих сообщений](./media/logic-apps-enterprise-integration-x12/x12-send-settings-validation.png) 

В строке **по умолчанию** показаны правила проверки, используемые для типа сообщений EDI. Если необходимо определить другие правила, выберите каждое поле, для которого должно быть задано **значение true**. После завершения каждой строки автоматически появится новая пустая строка.

| Свойство | Описание |
|----------|-------------|
| **Тип сообщения** | Тип сообщений EDI |
| **EDI Validation (Проверка EDI)** | Выполняет проверку EDI для типов данных в соответствии со свойствами EDI схемы, ограничениями длины, пустыми элементами данных и конечными разделителями. |
| **Extended Validation (Расширенная проверка)** | Если тип данных не является EDI, проверка осуществляется в соответствии с требованием к элементу данных и разрешенным повторением, перечислениями и проверкой длины элемента данных (min или Max). |
| **Allow Leading/Trailing Zeroes (Разрешить начальные и конечные нули)** | Оставляйте любые дополнительные начальные или конечные нули и пробелы. Не удаляйте эти символы. |
| **Обрезать начальные и конечные нули** | Удалите все начальные или конечные нули и пробелы. |
| **Trailing Separator Policy (Политика конечных разделителей)** | Создает конечные разделители. <p>- **Не допускается**: запретить конечные разделители и разделитель в исходящем обмене. Если обмен содержит конечные разделители, он будет объявлен недопустимым. <p>- **Необязательно**: отправка взаимоизменений с конечными разделителями или без них. <p>- **Обязательный**: исходящий обмен должен содержать конечные разделители и разделитель. |
|||

<a name="hipaa-schemas"></a>

## <a name="hipaa-schemas-and-message-types"></a>Схемы и типы сообщений HIPAA

При работе со схемами HIPAA и типами сообщений 277 или 837 необходимо выполнить несколько дополнительных действий. [Номера версий документов (GS8)](#outbound-control-version-number) для этих типов сообщений содержат более 9 символов, например "005010X222A1". Кроме того, некоторые номера версий документа сопоставляются с типами сообщений Variant. Если вы не ссылаетесь на правильный тип сообщений в схеме и в вашем соглашении, отобразится следующее сообщение об ошибке:

`"The message has an unknown document type and did not resolve to any of the existing schemas configured in the agreement."`

В этой таблице перечислены затронутые типы сообщений, любые варианты и номера версий документов, которые сопоставляются с этими типами сообщений:

| Тип или вариант сообщения |  Описание | Номер версии документа (GS8) |
|-------------------------|--------------|-------------------------------|
| 277 | Уведомление о состоянии сведений о карьере работоспособности | 005010X212 |
| 837_I | Дентал утверждений о работоспособности | 004010X096A1 <br>005010X223A1 <br>005010X223A2 |
| 837_D | Заявка на здравоохранение в отношении здравоохранения | 004010X097A1 <br>005010X224A1 <br>005010X224A2 |
| 837_P | Специалист по здравоохранения здравоохранения | 004010X098A1 <br>005010X222 <br>005010X222A1 |
|||

Также необходимо отключить проверку EDI при использовании этих номеров версий документа, так как они приводят к ошибке, что Недопустимая длина символа.

Чтобы указать эти номера версий документа и типы сообщений, выполните следующие действия.

1. В схеме HIPAA замените текущий тип сообщения на тип Variant для номера версии документа, который вы хотите использовать.

   Например, предположим, что вы хотите использовать номер `005010X222A1` версии документа с типом `837` сообщения. В схеме вместо этого замените каждое `"X12_00501_837"` значение на `"X12_00501_837_P"` значение.

   Чтобы обновить схему, выполните следующие действия.

   1. В портал Azure перейдите к учетной записи интеграции. Найдите и скачайте схему. Замените тип сообщения и переименуйте файл схемы и отправьте измененную схему в учетную запись интеграции. Дополнительные сведения см. в разделе [изменение схем](../logic-apps/logic-apps-enterprise-integration-schemas.md#edit-schemas).

   1. В параметрах сообщений вашего соглашения выберите измененную схему.

1. В `schemaReferences` объекте соглашения добавьте еще одну запись, указывающую тип сообщения Variant, соответствующий номеру версии документа.

   Например, предположим, что вы хотите использовать номер `005010X222A1` версии документа для типа `837` сообщений. В соглашении есть `schemaReferences` раздел со следующими свойствами и значениями:

   ```json
   "schemaReferences": [
      {
         "messageId": "837",
         "schemaVersion": "00501",
         "schemaName": "X12_00501_837"
      }
   ]
   ```

   В этом `schemaReferences` разделе добавьте еще одну запись со следующими значениями:

   * `"messageId": "837_P"`
   * `"schemaVersion": "00501"`
   * `"schemaName": "X12_00501_837_P"`

   Когда все будет готово, `schemaReferences` раздел будет выглядеть следующим образом:

   ```json
   "schemaReferences": [
      {
         "messageId": "837",
         "schemaVersion": "00501",
         "schemaName": "X12_00501_837"
      },
      {
         "messageId": "837_P",
         "schemaVersion": "00501",
         "schemaName": "X12_00501_837_P"
      }
   ]
   ```

1. В параметрах сообщения в соглашении отключите проверку EDI, сняв флажок **проверки EDI** для каждого типа сообщений или для всех типов сообщений, если используются значения **по умолчанию** .

   ![Отключить проверку для всех типов сообщений или каждого типа сообщений](./media/logic-apps-enterprise-integration-x12/x12-disable-validation.png) 

## <a name="connector-reference"></a>Справочник по соединителям

Дополнительные технические сведения об этом соединителе, такие как действия и ограничения, описанные в файле Swagger соединителя, см. на [странице справочника по соединителю](https://docs.microsoft.com/connectors/x12/).

> [!NOTE]
> Для приложений логики в [среде службы интеграции (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)эта версия этого соединителя использует [ограничения сообщений B2B для интегрированной среды сценариев](../logic-apps/logic-apps-limits-and-config.md#b2b-protocol-limits).

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о других [соединителях для Logic Apps](../connectors/apis-list.md)