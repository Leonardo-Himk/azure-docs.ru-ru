---
title: Устранение неполадок с входными подключениями для Azure Stream Analytics
description: В этой статье описаны методы устранения неполадок с входными подключениями в заданиях Azure Stream Analytics.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/01/2020
ms.custom: seodec18
ms.openlocfilehash: 920755e128f10a79a056d47813b1b65d8633c937
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628748"
---
# <a name="troubleshoot-input-connections"></a>Устранение неполадок с входными подключениями

В этой статье описаны распространенные проблемы, связанные с Azure Stream Analytics входными подключениями, способы устранения неполадок ввода и устранения проблем. Во многих действиях по устранению неполадок необходимо включить журналы ресурсов для задания Stream Analytics. Если журналы ресурсов не включены, см. раздел [Устранение неполадок Azure Stream Analytics с помощью журналов ресурсов](stream-analytics-job-diagnostic-logs.md).

## <a name="input-events-not-received-by-job"></a>Входные события, не полученные заданием 

1.  Протестируйте входные и выходные подключения. Проверьте подключение к портам ввода и вывода с помощью кнопки **Проверить подключение** для всех входных и выходных данных.

2.  Проверьте входные данные.

    1. Используйте кнопку [**Выбор данных**](stream-analytics-sample-data-input.md) для каждого входного аргумента. Скачайте входные образцы данных.
        
    1. Изучите образец данных, чтобы понять схему и [типы данных](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics).
    
    1. Проверьте [метрики концентратора событий](../event-hubs/event-hubs-metrics-azure-monitor.md) , чтобы убедиться, что события отправляются. Метрики сообщений должны быть больше нуля, если концентраторы событий получают сообщения.

3.  Убедитесь, что в предварительной версии входных данных выбран временной диапазон. Выберите **выбрать диапазон времени**, а затем введите длительность выборки перед тестированием запроса.

## <a name="malformed-input-events-causes-deserialization-errors"></a>Неправильный формат входных событий, который приводит к ошибкам десериализации 

Если входной поток задания Stream Analytics содержит сообщения неправильного формата, возникают проблемы десериализации. Например, неправильно сформированное сообщение может быть вызвано отсутствием круглой скобки или фигурной скобкой в объекте JSON или неверным форматом метки времени в поле Time. 
 
Когда задание Stream Analytics получает сообщение неправильного формата из входного набора данных, это сообщение отклоняется, а пользователь получает предупреждение. На плитке **входные данные** задания Stream Analytics отображается символ предупреждения. Следующий предупреждающий символ существует, пока задание находится в состоянии выполняется:

![Плитка "Входные данные" Azure Stream Analytics](media/stream-analytics-malformed-events/stream-analytics-inputs-tile.png)

Включите журналы ресурсов для просмотра сведений об ошибке и сообщения (полезных данных), вызвавшего ошибку. Есть несколько причин, по которым могут возникать ошибки десериализации. Дополнительные сведения о конкретных ошибках десериализации см. в разделе [ошибки входных данных](data-errors.md#input-data-errors). Если журналы ресурсов не включены, на портал Azure будет доступно Краткое уведомление.

![Предупреждение о вводе сведений о входе](media/stream-analytics-malformed-events/warning-message-with-offset.png)

В случаях, когда полезная нагрузка сообщения превышает 32 КБ или находится в двоичном формате, запустите код CheckMalformedEvents.cs, доступный в [репозитории примеров GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). При помощи этого кода считываются идентификатор раздела и смещение, а затем выводятся данные для этого смещения. 

## <a name="job-exceeds-maximum-event-hub-receivers"></a>Задание превышает максимальное число получателей концентратора событий

Для использования концентраторов событий рекомендуется использовать несколько групп потребителей для масштабируемости заданий. Число модулей чтения в задании Stream Analytics для определенных входных данных влияет на число модулей чтения в одной группе потребителей. Точное число приемников зависит от сведений о внутренней реализации логики топологии развертывания и не предоставляется извне. Число модулей чтения можно изменить во время запуска или обновления задания.

При превышении максимального числа получателей отображаются следующие сообщения об ошибках. Сообщение об ошибке содержит список существующих соединений, сделанных в концентраторе событий в группе потребителей. Тег `AzureStreamAnalytics` указывает, что подключения относятся к службе потоковой передачи Azure.

```
The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.

The following information may be helpful in identifying the connected receivers: Exceeded the maximum number of allowed receivers per partition in a consumer group which is 5. List of connected receivers – 
AzureStreamAnalytics_c4b65e4a-f572-4cfc-b4e2-cf237f43c6f0_1, 
AzureStreamAnalytics_c4b65e4a-f572-4cfc-b4e2-cf237f43c6f0_1, 
AzureStreamAnalytics_c4b65e4a-f572-4cfc-b4e2-cf237f43c6f0_1, 
AzureStreamAnalytics_c4b65e4a-f572-4cfc-b4e2-cf237f43c6f0_1, 
AzureStreamAnalytics_c4b65e4a-f572-4cfc-b4e2-cf237f43c6f0_1.
```

> [!NOTE]
> При изменении числа модулей чтения во время обновления задания временные предупреждения записываются в журналы аудита. Задания Stream Analytics восстанавливаются от этих временных сбоев автоматически.

### <a name="add-a-consumer-group-in-event-hubs"></a>Добавление группы потребителей в Центры событий

Чтобы добавить новую группу потребителей в экземпляр Центра событий, сделайте следующее.

1. Войдите на портал Azure.

2. Нахождение концентратора событий.

3. Выберите **Центры событий** под заголовком **Сущности**.

4. Выберите имя концентратора событий.

5. На странице **Экземпляр Центров событий** под заголовком **Сущности** установите флажок **Группы потребителей**. Указана группа потребителей с именем **$Default**.

6. Выберите **+Группа потребителей**, чтобы добавить новую группу потребителей. 

   ![Добавление группы потребителей в Центры событий](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Вы указали группу потребителей, когда создавали входные данные в задании Stream Analytics для концентратора событий. **$Default** используется, если не указано ни одного. После создания группы потребителей измените входные данные концентратора событий в задании Stream Analytics и укажите имя новой группы потребителей.

## <a name="readers-per-partition-exceeds-event-hubs-limit"></a>Превышение предельно допустимого числа модулей чтения каждого раздела для Центров событий

Если синтаксис запроса потоковой передачи ссылается на один и тот же входной ресурс концентратора событий несколько раз, обработчик заданий может использовать несколько читателей для каждого запроса из этой же группы потребителей. При наличии слишком большого количества ссылок в одной и той же группе потребителей задание может превысить установленный предел (пять ссылок) и вызвать ошибку. В таких случаях можно выполнять разделение с помощью нескольких групп входных данных в нескольких группах потребителей, используя решения, описанные в следующем разделе. 

Ниже представлены сценарии, в которых число модулей чтения каждого раздела превышает предельно допустимое число для Центров событий (пять).

* Несколько инструкций SELECT. При использовании нескольких инструкций SELECT, ссылающихся на **один** набор входных данных концентратора событий, каждая из них создает по приемнику.

* UNION. При использовании UNION несколько наборов входных данных могут ссылаться на **один** концентратор событий и на одну группу потребителей.

* SELF JOIN. При использовании операции SELF JOIN можно создать несколько ссылок на **один** концентратор событий.

Следующие рекомендации могут помочь справиться со сценариями, в которых число модулей чтения каждого раздела превышает предельно допустимое число для Центров событий (пять).

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Разбиение запроса на несколько шагов с помощью предложения WITH

Предложение WITH задает временный именованный результирующий набор, на который можно создать ссылку в предложении FROM в запросе. Предложение WITH определяется в области выполнения одной инструкции SELECT.

Например, вместо этого запроса:

```SQL
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Используйте этот:

```SQL
WITH data AS (
   SELECT * FROM inputEventHub
)

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Привязка входных данных к разным группам потребителей

Создайте отдельные группы потребителей для запросов, в которых три или более наборов входных данных подключены к одной группе потребителей Центров событий. Для этого нужно создать дополнительные наборы входных данных Stream Analytics.

## <a name="get-help"></a>Получить справку

Для получения дополнительной помощи посетите наш [Azure Stream Analytics Форум](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Дальнейшие действия

* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
