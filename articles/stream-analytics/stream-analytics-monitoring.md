---
title: Основные сведения о мониторинге заданий в Azure Stream Analytics
description: В этой статье описывается мониторинг заданий Stream Analytics на портале Azure.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2018
ms.custom: seodec18
ms.openlocfilehash: 54bff88e9650240a3703e18d583f603cafeb3022
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611897"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Основные сведения о мониторинге заданий Stream Analytics и порядок мониторинга запросов

## <a name="introduction-the-monitor-page"></a>Введение: страница мониторинга
Портал Azure отображает ключевые метрики производительности, которые можно использовать для наблюдения за производительностью запросов и заданий и устранения неполадок. Для просмотра метрик перейдите к нужному заданию Stream Analytics и просмотрите раздел **Мониторинг** на странице "Обзор".  

![Ссылка на страницу мониторинга заданий Stream Analytics](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Откроется окно, как показано ниже:

![Панель мониторинга заданий Stream Analytics](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Метрики, доступные в Stream Analytics
| Метрика                 | Определение                               |
| ---------------------- | ---------------------------------------- |
| Необработанные входные события       | Количество необработанных входных событий. Ненулевое значение для этой метрики подразумевает, что задание не может справиться с числом входящих событий. Если это значение медленно увеличивается или постоянно равно ненулевому значению, нужно расширить ваше задание. Дополнительные сведения см. в статье, посвященной [обзору и настройке единиц потоковой передачи](stream-analytics-streaming-unit-consumption.md). |
| Ошибки преобразования данных | Количество выходных событий, которые не удалось преобразовать в ожидаемую схему выходных данных. Политику обработки ошибок можно изменить на "Drop", чтобы удалить события, по которым этот сценарий возникает. |
| Ранние входные события       | События, у которых отметка времени приложения предшествует времени прибытия больше, чем на 5 минут. |
| Неудачные запросы функций | Количество неудачных вызовов функции машинного обучения Azure (при наличии). |
| События функций        | Количество событий, отправленных в функцию машинного обучения Azure (при наличии). |
| Запросы функций      | Количество вызовов функции машинного обучения Azure (при наличии). |
| Ошибки десериализации входа       | Количество входных событий, которые не удалось десериализировать.  |
| Байты входного события      | Объем данных, получаемых заданием Stream Analytics, измеряемый в байтах. Эти данные можно использовать, чтобы проверить, отправляются ли события в источник входных данных. |
| Входные события           | Число записей, десериализованных из входных событий. Это число не включает входящие события, которые приводят к ошибкам десериализации. Одни и те же события могут быть приняты Stream Analytics несколько раз в таких сценариях, как внутренние операции восстановления и самосоединения. Поэтому не рекомендуется рассчитывать метрики входных и выходных событий, если у задания есть простой запрос "Pass-". |
| Полученные входные источники       | Количество сообщений, полученных заданием. Для концентратора событий сообщение является одним из событий EventData. Для большого двоичного объекта сообщение — это один большой двоичный объект. Обратите внимание, что источники входных данных подсчитываются до десериализации. При возникновении ошибок десериализации источники входных данных могут быть больше, чем входные события. В противном случае он может быть меньше или равен входным событиям, так как каждое сообщение может содержать несколько событий. |
| События позднего ввода      | События, полученные позже настроенного значения допустимого интервала поступления с задержкой. Сведения о рассмотрении порядка событий Azure Stream Analytics см. [здесь](stream-analytics-out-of-order-and-late-events.md). |
| Неупорядоченные события    | Количество событий, полученных в неактуальное время, которые были удалены или получили откорректированную метку времени в соответствии с политикой упорядочения событий. Эта метрика может зависеть от настройки параметра «Интервал для событий, полученных в неправильном порядке». |
| Выходные события          | Объем данных, отправляемых заданием Stream Analytics в место назначения выходных данных, измеряемый количеством событий. |
| Ошибки среды выполнения         | Общее число ошибок, связанных с обработкой запросов (за исключением ошибок, обнаруженных при приеме событий или выводе результатов) |
| Использование единиц потоковой передачи (%)       | Использование единиц потоковой передачи, назначенных заданию на вкладке «Масштаб» соответствующего задания. Как только этот индикатор достигнет 80 % и более существует высокая вероятность приостановки или отсрочки обработки события. |
| Предельная задержка       | Предельная задержка для всех разделов выходных данных в задании. |

Можно использовать эти метрики для [мониторинга выполнения задания Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Настройка мониторинга на портале Azure
Вы можете изменить тип диаграммы, отображаемые метрики и временной диапазон на экране "Редактирование диаграммы". Дополнительные сведения см. в статье [Настройка мониторинга](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Диаграмма значений времени в мониторе запросов Stream Analytics](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Последние выходные данные
Еще одна интересная точка для мониторинга задания — это время последнего вывода, показанное на странице "Обзор".
Это время является временем приложения (т. е. временем, используемым меткой времени на основе данных события) последних выходных данных задания.

## <a name="get-help"></a>Получить справку
Дополнительную помощь и поддержку вы можете получить на нашем [форуме Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
