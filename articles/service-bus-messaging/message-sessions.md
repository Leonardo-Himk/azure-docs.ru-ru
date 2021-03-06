---
title: Сеансы обмена сообщениями служебной шины Azure | Документация Майкрософт
description: В этой статье объясняется, как использовать сеансы для обеспечения совместной и упорядоченной обработки непривязанных последовательностей связанных сообщений.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2020
ms.author: aschhab
ms.openlocfilehash: a4bc2dcfd1826623516a40be0aff7688d0b6168c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82116695"
---
# <a name="message-sessions"></a>Сеансы обмена сообщениями
Сеансы службы "Служебная шина Microsoft Azure" обеспечивают согласованную и упорядоченную обработку несвязанных последовательностей связанных сообщений. Сеансы можно использовать в шаблонах «первым поступил», «первым вышел» (FIFO) и «запрос-ответ». В этой статье показано, как использовать сеансы для реализации этих шаблонов при использовании служебной шины. 

## <a name="first-in-first-out-fifo-pattern"></a>Шаблон «первым поступил, первым обслужен» (FIFO)
Чтобы реализовать гарантию FIFO в служебной шине, используйте сеансы. Служебная шина не имеет нормативных сведений о характере связи между сообщениями, а также не определяет определенную модель для определения места начала или окончания последовательности сообщений.

> [!NOTE]
> Базовый уровень служебной шины не поддерживает сеансы. а категории "Стандартный" и "Премиум" — поддерживают. Различия между этими уровнями см. в разделе [цены на служебную шину](https://azure.microsoft.com/pricing/details/service-bus/).

Любой отправитель может создать сеанс при отправке сообщений в очередь или раздел, задав свойству [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) какой-либо определяемый приложением идентификатор, уникальный в рамках сеанса. На уровне протокола AMQP 1.0 это значение соответствует свойству *group-id*.

В очередях или подписках, использующих сеанс, сеансы возникают, когда существует хотя бы одно сообщение с [идентификатором](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId)сеанса. Когда сеанс существует, не определено время или API-интерфейс, когда сеанс истечет или исчезнет. Теоретически, сообщение для сеанса может быть получено сегодня, а следующее — через год, и если значение **SessionId** совпадает, то с точки зрения служебной шины это тот же самый сеанс.

Обычно в приложении четко определено, где начинается и заканчивается набор связанных сообщений. Служебная шина не устанавливает никаких особых правил.

Например, чтобы разграничить последовательность для передачи файла, можно задать для свойства **Label** первого сообщения значение **start**, для промежуточных сообщений задать для этого свойства значение **content**, а для последнего сообщения — значение **end**. Относительное положение сообщений с содержимым может быть вычислено как разница между значением *SequenceNumber* текущего сообщения и значением *SequenceNumber* сообщения **start**.

Функция сеансов в служебной шине позволяет выполнить специальную операцию получения посредством [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) в интерфейсах API C# и Java. Для включения этой функции нужно задать свойство [requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) для очереди или подписки с помощью Azure Resource Manager или установить соответствующий флаг на портале. Это необходимо, прежде чем пытаться использовать связанные операции API.

На портале флаг устанавливается с помощью следующего флажка:

![][2]

> [!NOTE]
> Если сеансы включены в очереди или подписке, клиентские приложения ***больше не*** могут отправлять и получать обычные сообщения. Все сообщения должны быть отправлены в рамках сеанса (путем установки идентификатора сеанса) и получены путем получения сеанса.

Интерфейсы API для сеансов существуют в клиентах очереди и подписки. Существует императивная модель, управляющая получением сеансов и сообщений, а также модель на основе обработчиков, аналогичная *onmessage*, которая скрывает сложность управления циклом получения.

### <a name="session-features"></a>Функции сеансов

Сеансы обеспечивают параллельное демультиплексирование потоков сообщений с сохранением и гарантией порядка доставки.

![][1]

Получатель [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) создается клиентом, принимающим сеанс. Клиент вызывает [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) или [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) в C#. В реактивной модели обратного вызова он регистрирует обработчик сеанса.

Когда объект [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) принимается и удерживается клиентом, он удерживает монопольную блокировку для всех сообщений с [идентификатором](/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) сеанса, которые существуют в очереди или подписке, а также на всех сообщениях с этим **SessionID** , которые по-прежнему приходят во время сеанса.

Блокировка освобождается при вызове **Close** или **CloseAsync** или при истечении срока действия блокировки в случаях, когда приложению не удается выполнить операцию закрытия. Блокировка сеанса должна рассматриваться как монопольная блокировка файла. Это означает, что приложение должно закрыть сеанс, как только оно больше не требуется и/или не ждет дальнейших сообщений.

Когда несколько параллельных получателей извлекают сообщения из очереди, сообщения, относящиеся к определенному сеансу, отправляются в конкретный получатель, который в настоящее время наложил блокировку для этого сеанса. С этой операцией поток сообщений с чередованием в одной очереди или подписке очищается без мультиплексирования между разными получателями, и эти получатели могут также находиться на разных клиентских компьютерах, так как Управление блокировкой происходит на стороне службы внутри служебной шины.

На предыдущем рисунке показано три параллельных приемника сеансов. У одного сеанса с `SessionId` = 4 нет активного владеющего клиента. Это означает, что сообщения не будут доставлены из этого конкретного сеанса. Сеанс работает по-разному, как и подочередь.

Блокировка сеанса, накладываемая получателем сеанса, — это "зонтик" для блокировки сообщений, используемый для режима согласования *PeekLock*. Только один получатель может блокировать сеанс. Получатель может иметь много поступающих сообщений, но сообщения будут получены по порядку. Если сообщение отбрасывается, то это же сообщение обслуживается повторно при следующей операции получения.

### <a name="message-session-state"></a>Состояние сеанса обмена сообщениями

Когда рабочие процессы обрабатываются в высокомасштабируемых облачных системах с высоким уровнем доступности, обработчик рабочего процесса, связанный с определенным сеансом, должен иметь возможность восстановления после непредвиденных сбоев и может возобновить частично завершенную работу на другом процессе или компьютере, где началась работа.

Функция состояния сеанса позволяет добавлять определяемые приложением заметки для сеанса обмена сообщениями внутри брокера, чтобы записанное состояние обработки, относящееся к этому сеансу, становилось мгновенно доступным при получении этого сеанса новым обработчиком.

С точки зрения служебной шины состояние сеанса обмена сообщениями — непрозрачный двоичный объект, который может содержать данные, размер которых равен размеру одного сообщения, что составляет 256 КБ для служебной шины категории "Стандартный" и 1 МБ для служебной шины категории "Премиум". Состояние обработки относительно сеанса может сохраняться в состоянии сеанса, или состояние сеанса может указывать на некоторое место хранения либо запись базы данных, где содержатся эти сведения.

Интерфейсы API для управления состоянием сеанса, [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) и [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), можно найти в объекте [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) в интерфейсах API C# и Java. Сеанс, для которого ранее не было задано состояние сеанса, возвращает в интерфейс API **GetState****пустую** ссылку. Очистка заданного ранее состояния сеанса выполняется с помощью [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Состояние сеанса остается неизменным (возвращается **значение NULL**), даже если все сообщения в сеансе используются.

Все существующие сеансы в очереди или подписке можно перечислить с помощью метода **сессионбровсер** в API Java и с помощью [жетмессажесессионс](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) в [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) и [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) в клиенте .NET Framework.

Состояние сеанса, хранящееся в очереди или подписке, учитывается при подсчете квоты хранилища этой сущности. Поэтому, когда приложение завершает работу с сеансом, рекомендуется очищать его сохраненное состояние, чтобы избежать затрат на внешнее управление.

### <a name="impact-of-delivery-count"></a>Влияние счетчика доставки

Определение числа доставок для каждого сообщения в контексте сеансов немного отличается от определения в случае отсутствия сеансов. Ниже приведена таблица, в которой приводится сводка по увеличению числа доставок.

| Сценарий | Увеличивается счетчик доставки сообщений |
|----------|---------------------------------------------|
| Сеанс принят, но блокировка сеанса истекает (из-за истечения времени ожидания) | Да |
| Сеанс принят, сообщения в сеансе не завершены (даже если они заблокированы) и сеанс закрыт. | Нет |
| Сеанс принят, сообщения завершаются, а затем сеанс явным образом закрывается | Н/д (это стандартный поток. Здесь сообщения удаляются из сеанса. |

## <a name="request-response-pattern"></a>Шаблон "запрос-ответ"
[Шаблон "запрос-ответ](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) " — это хорошо установленный шаблон интеграции, который позволяет приложению отправителя отправить запрос и предоставить получателю возможность правильно отправить ответ обратно в приложение отправителя. Для этого шаблона обычно требуется кратковременная очередь или раздел, в котором приложение отправляет ответы. В этом сценарии сеансы предоставляют простое альтернативное решение с сравнимой семантикой. 

Несколько приложений могут отправить свои запросы в одну очередь запросов с заданным параметром заголовков для уникальной идентификации приложения-отправителя. Приложение-получатель может обрабатывать запросы, поступающие в очередь, и отправлять ответы в очередь с включенными сеансами, устанавливая для идентификатора сеанса уникальный идентификатор, отправленный отправителем в сообщении запроса. Приложение, которое отправило запрос, может затем принимать сообщения по определенному ИДЕНТИФИКАТОРу сеанса и правильно обрабатывать ответы.

> [!NOTE]
> Приложение, которое отправляет первоначальные запросы, должно быть осведомлено об ИДЕНТИФИКАТОРе `SessionClient.AcceptMessageSession(SessionID)` сеанса и использовать для блокировки сеанса, в котором он ожидает ответа. Рекомендуется использовать идентификатор GUID, который уникальным образом идентифицирует экземпляр приложения в качестве идентификатора сеанса. Для того чтобы ответы были заблокированы `AcceptMessageSession(timeout)` и обработаны конкретными получателями, в очереди не должно быть обработчиков сеансов или.

## <a name="next-steps"></a>Дальнейшие шаги

- Пример использования клиента .NET Framework для обработки сообщений, поддерживающих сеанс, см. в [статье примеры Microsoft. Azure. servicebus](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/Sessions) или [Microsoft. servicebus. Messaging](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) . 

Дополнительные сведения об обмене сообщениями через служебную шину см. в следующих статьях:

* [Очереди, разделы и подписки служебной шины](service-bus-queues-topics-subscriptions.md)
* [Начало работы с очередями служебной шины](service-bus-dotnet-get-started-with-queues.md)
* [Как использовать разделы и подписки служебной шины](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png
