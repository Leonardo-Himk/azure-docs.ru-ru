---
title: Запросы данных о производительности на стороне сервера
description: Как выполнять запросы производительности на стороне сервера через вызовы API
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: 9a28dee2d1e6d1355b729a56e8eeb8447e4ed8c8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80682030"
---
# <a name="server-side-performance-queries"></a>Запросы данных о производительности на стороне сервера

Правильная производительность отрисовки на сервере крайне важна для стабильной частоты кадров и хорошей работы пользователей. Важно тщательно отслеживать характеристики производительности сервера и при необходимости оптимизировать их. Запросы к данным производительности можно запрашивать с помощью выделенных функций API.

Наиболее влиянием на производительность отрисовки является входные данные модели. Вы можете настроить входные данные, как описано в разделе [Настройка преобразования модели](../../how-tos/conversion/configure-model-conversion.md).

Производительность приложений на стороне клиента также может быть узким местом. Для глубокого анализа производительности на стороне клиента рекомендуется использовать [трассировку производительности](../../how-tos/performance-tracing.md).

## <a name="clientserver-timeline"></a>Временная шкала клиента/сервера

Прежде чем углубляться в подробные сведения о различных значениях задержки, стоит взглянуть на точки синхронизации между клиентом и сервером на временной шкале:

![Временная шкала конвейера](./media/server-client-timeline.png)

На рисунке показано, как:

* *Оценка* выдается клиентом по постоянной частоте кадров 60-Гц (каждые 16,6 мс).
* Затем сервер начинает подготовку к просмотру на основе объекта
* сервер отправляет обратно закодированное видео изображение
* Клиент декодирует изображение, выполняет некоторые ЦП и GPU на поверх него, а затем отображает изображение.

## <a name="frame-statistics-queries"></a>Запросы статистики кадров

Статистика кадра предоставляет некоторые сведения высокого уровня для последнего кадра, например задержки. Данные, предоставленные в `FrameStatistics` структуре, измеряются на стороне клиента, поэтому API является синхронным вызовом:

````c#
void QueryFrameData(AzureSession session)
{
    FrameStatistics frameStatistics;
    if (session.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        // do something with the result
    }
}
````

Извлеченный `FrameStatistics` объект содержит следующие члены:

| Участник | Объяснение |
|:-|:-|
| латенципосеторецеиве | Задержка от оценки типа камеры на клиентском устройстве, пока серверный кадр для этой объекта не будет полностью доступен клиентскому приложению. Это значение включает обмен данными по сети, время визуализации сервера, декодирование видео и компенсацию колебаний. См **. интервал 1 на рисунке выше.**|
| латенцирецеиветопресент | Задержка от доступности полученной удаленной рамки до тех пор, пока клиентское приложение не вызовет Пресентфраме на ЦП. |
| латенципресенттодисплай  | Задержка показа кадра на ЦП до отображения индикатора. Это значение включает время GPU клиента, любые кадры, выполняемые операционной системой, перепроецирование оборудования и время просмотра с аппаратным обеспечением. См **. интервал 2 на рисунке выше.**|
| тимесинцеластпресент | Время между последовательными вызовами Пресентфраме в ЦП. Значения, превышающие длительность отображения (например, 16,6 мс на клиентском устройстве 60-Гц), указывают на проблемы, вызванные тем, что клиентское приложение не завершило рабочую нагрузку ЦП вовремя. См **. интервал 3 на рисунке выше.**|
| видеофрамесрецеивед | Число кадров, полученных с сервера за последнюю секунду. |
| видеофрамереуседкаунт | Число полученных кадров за последнюю секунду, которые использовались на устройстве несколько раз. Ненулевые значения указывают на то, что фреймы пришлось повторно использовать и перепроецироваться из-за колебаний сети или чрезмерного времени на визуализацию сервера. |
| видеофрамесскиппед | Число полученных кадров за последнюю секунду, которые были декодированы, но не показаны при отображении, поскольку пришел новый кадр. Ненулевые значения указывают на то, что разрыв сети привел к задержке нескольких кадров и последующему поступлении на клиентское устройство. |
| видеофрамесдискардед | Очень похоже на **видеофрамесскиппед**, но причина, по которой она была удалена, заключается в том, что кадр поступил так, что он даже не может быть связан с любым ожидающим набором. В этом случае возникает серьезное сетевое состязание.|
| видеофрамеминделта | Минимальное количество времени между двумя последовательными кадрами, поступающими за последнюю секунду. Вместе с Видеофрамемаксделта этот диапазон дает указание на нарушение, вызванное либо сетевым, либо видеокодеком. |
| видеофрамемаксделта | Максимальный промежуток времени между двумя последовательными кадрами, поступающими за последнюю секунду. Вместе с Видеофрамеминделта этот диапазон дает указание на нарушение, вызванное либо сетевым, либо видеокодеком. |

Сумма всех значений задержки обычно гораздо больше, чем доступное время кадра с частотой 60 Гц. Это нормально, так как несколько кадров находятся в режиме параллельного полета, а новые запросы к кадрам запускаются с требуемой частотой кадров, как показано на рисунке. Однако если задержка станет слишком большой, она повлияет на качество [перепроекции с поздним этапом](../../overview/features/late-stage-reprojection.md)и может нарушить общую работу.

`videoFramesReceived`, `videoFrameReusedCount`и `videoFramesDiscarded` могут использоваться для оценки производительности сети и сервера. Если `videoFramesReceived` имеет значение Low `videoFrameReusedCount` и имеет значение High, это может указывать на перегрузку сети или низкую производительность сервера. Высокое `videoFramesDiscarded` значение также указывает на перегрузку сети.

Наконец,`timeSinceLastPresent` `videoFrameMinDelta`, и `videoFrameMaxDelta` Представьте себе дисперсию входящих кадров видео и локальных вызовов. Высокая дисперсия означает нестабильную частоту кадров.

Ни одно из приведенных выше значений не дает четкого указания о чистой задержке в сети (красные стрелки на рисунке), так как точное время, когда сервер занят отрисовкой, необходимо вычесть из `latencyPoseToReceive`двунаправленного значения. Часть общей задержки на стороне сервера представляет собой информацию, недоступную для клиента. Однако в следующем абзаце объясняется приближение этого значения через дополнительные входные данные с сервера и предоставляется через это `networkLatency` значение.

## <a name="performance-assessment-queries"></a>Запросы оценки производительности

*Запросы оценки производительности* предоставляют более подробные сведения о рабочей нагрузке ЦП и GPU на сервере. Поскольку данные запрашиваются с сервера, запрос моментального снимка производительности соответствует обычной асинхронной модели:

``` cs
PerformanceAssessmentAsync _assessmentQuery = null;

void QueryPerformanceAssessment(AzureSession session)
{
    _assessmentQuery = session.Actions.QueryServerPerformanceAssessmentAsync();
    _assessmentQuery.Completed += (PerformanceAssessmentAsync res) =>
    {
        // do something with the result:
        PerformanceAssessment result = res.Result;
        // ...

        _assessmentQuery = null;
    };
}
```

В `PerformanceAssessment` отличие от `FrameStatistics` объекта, объект содержит сведения на стороне сервера:

| Участник | Объяснение |
|:-|:-|
| тимекпу | Среднее время ЦП сервера на кадр в миллисекундах |
| тимегпу | Среднее время GPU сервера на кадр в миллисекундах |
| утилизатионкпу | Общее использование ЦП сервера в процентах |
| утилизатионгпу | Общее использование GPU сервера в процентах |
| меморикпу | Общее использование памяти сервера в процентах |
| меморигпу | Общее использование выделенной видеопамяти в процентах от графического процессора сервера |
| нетворклатенци | Приблизительная средняя задержка сети обмена данными в миллисекундах. На рисунке выше это соответствует сумме красной стрелки. Значение вычисляется путем вычитания фактического времени визуализации сервера из `latencyPoseToReceive` значения. `FrameStatistics` Хотя это приближение является неточным, оно дает указание о задержке сети, изолированной от значений задержки, вычисленных на клиенте. |
| полигонсрендеред | Число треугольников, отображаемых в одном кадре. Это число также включает треугольники, которые исключаются позже во время подготовки к просмотру. Это означает, что это число не зависит от количества различных позиций камеры, но производительность может радикально варьироваться в зависимости от частоты исключающих треугольников.|

Чтобы помочь вам оценить значения, каждая из них имеет классификацию качества, например " **отличная**", " **хорошая**", " **заурядного**" или " **плохо**".
Эта метрика оценки дает приблизительную индикацию работоспособности сервера, но не должна рассматриваться как абсолютная. Например, предположим, что для времени GPU отображается оценка "заурядного". Он считается заурядного, так как он приближается к ограничению общего бюджетного времени. Однако в вашем случае это может быть хорошей ценностью, так как вы готовите к просмотру сложную модель.

## <a name="statistics-debug-output"></a>Выходные данные отладки статистики

Класс `ARRServiceStats` охватывает как статистику кадров, так и запросы оценки производительности и предоставляет удобные функции для возврата статистики в виде статистических значений или в виде предварительно построенной строки. Следующий код является самым простым способом отображения статистики на стороне сервера в клиентском приложении.

``` cs
ARRServiceStats _stats = null;

void OnConnect()
{
    _stats = new ARRServiceStats();
}

void OnDisconnect()
{
    _stats = null;
}

void Update()
{
    if (_stats != null)
    {
        // update once a frame to retrieve new information and build average values
        _stats.Update(Service.CurrentActiveSession);

        // retrieve a string with relevant stats information
        InfoLabel.text = _stats.GetStatsString();
    }
}
```

Приведенный выше код заполняет текстовую метку следующим текстом:

![Вывод строки Аррсервицестатс](./media/arr-service-stats.png)

`GetStatsString` API форматирует строку всех значений, но каждое отдельное значение также может быть запрашиваться программно из `ARRServiceStats` экземпляра.

Существуют также варианты членов, которые объединяют значения с течением времени. См. раздел члены `*Avg`с `*Max`суффиксом `*Total`, или. Элемент `FramesUsedForAverage` указывает, сколько кадров использовалось для этого агрегата.

## <a name="next-steps"></a>Дальнейшие шаги

* [Создание трассировок производительности](../../how-tos/performance-tracing.md)
* [Настройка преобразования модели](../../how-tos/conversion/configure-model-conversion.md)
