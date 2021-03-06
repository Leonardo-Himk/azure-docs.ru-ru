---
title: Azure CDN необработанных журналов HTTP
description: В этой статье описываются необработанные журналы HTTP Azure CDN.
services: cdn
author: sohamnchatterjee
manager: danielgi
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2020
ms.author: sohamnc
ms.openlocfilehash: c6e8570746ae3dd0051dbec084c89d90580d28b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80371635"
---
# <a name="azure-cdn-http-raw-logs"></a>Azure CDN необработанных журналов HTTP
Необработанные журналы содержат подробные сведения об операциях и ошибках, которые важны для аудита и устранения неполадок. Необработанные журналы отличаются от журналов действий. Журналы действий позволяют видеть операции, выполняемые в ресурсах Azure. Необработанные журналы содержат записи об операциях ресурса.

> [!IMPORTANT]
> Функция "журналы HTTP RAW" доступна для Azure CDN от Майкрософт.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу. 

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="configuration"></a>Конфигурация

Чтобы настроить необработанные журналы для Azure CDN из профиля Майкрософт, выполните следующие действия. 

1. В меню портал Azure выберите **ALL Resources** >> **\<a-CDN-Profile>**.

2. В разделе **мониторинг**выберите **параметры диагностики**.

3. Выберите **+ Добавить параметр диагностики**.

    ![Параметр диагностики CDN](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > Необработанные журналы доступны только на уровне профиля, а агрегированные журналы кода состояния HTTP доступны на уровне конечной точки.

4. В разделе **параметры диагностики**введите имя параметра диагностики в разделе **имя параметров диагностики**.

5. Выберите **Журнал** и задайте срок хранения в днях.

6. Выберите **сведения о назначении**. Параметры назначения:
    * **Отправка в Log Analytics**
        * Выберите **подписку** и **log Analytics рабочую область**.
    * **Архивация в учетную запись хранения**
        * Выберите **подписку** и **учетную запись хранения**.
    * **Поток в концентратор событий**
        * Выберите **подписку**, **пространство имен концентратора событий**, **имя концентратора событий (необязательно)** и **имя политики концентратора событий**.

    ![Параметр диагностики CDN](./media/cdn-raw-logs/raw-logs-02.png)

7. Нажмите кнопку **Сохранить**.

## <a name="raw-logs-properties"></a>Свойства необработанных журналов

Azure CDN из службы Майкрософт в настоящее время предоставляют необработанные журналы. Необработанные журналы предоставляют отдельные запросы API с каждой записью, имеющей следующую схему: 

| Свойство              | Описание                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| траккингреференце     | Уникальная строка ссылки, определяющая запрос, обслуживаемый передней дверцей, который также отправляется клиенту в виде заголовка X-Azure-ref. Требуется для поиска сведений в журналах доступа для конкретного запроса. |
| HttpMethod            | Метод HTTP, используемый для запроса.                                                                                                                                                                     |
| HttpVersion           | Тип запроса или соединения.                                                                                                                                                                   |
| RequestUri            | URI полученного запроса.                                                                                                                                                                         |
| рекуестбитес          | Размер сообщения HTTP-запроса в байтах, включая заголовки запроса и текст запроса.                                                                                                   |
| респонсебитес         | Байт, отправленный внутренним сервером в качестве ответа.                                                                                                                                                    |
| UserAgent             | Тип браузера, который использовался клиентом.                                                                                                                                                               |
| ClientIp              | IP-адрес отправившего запрос клиента.                                                                                                                                                  |
| TimeTaken             | Продолжительность времени, затраченного на выполнение действия, в миллисекундах.                                                                                                                                            |
| Экземпляр SecurityProtocol      | Версия протокола TLS/SSL, используемая запросом, или значение null, если шифрование отсутствует.                                                                                                                           |
| Конечная точка              | Узел конечной точки CDN настроен в родительском профиле CDN.                                                                                                                                   |
| Имя узла внутреннего сервера     | Имя узла внутреннего сервера или источника, куда отправляются запросы.                                                                                                                                |
| Отправлено на начальную панель | Если значение — true, это означает, что на запрос был получен ответ из кэша экранирования источника, а не из точки подключения к границе. Точка защиты источника — это родительский кэш, используемый для улучшения коэффициента попадания в кэш.                                       |
| HttpStatusCode        | Код состояния HTTP, возвращенный прокси-сервером.                                                                                                                                                        |
| хттпстатусдетаилс     | Итоговое состояние запроса. Значение этого строкового значения можно найти в таблице ссылок на состояние.                                                                                              |
| Рор                   | POP ребра, которая ответила на запрос пользователя. Аббревиатуры POP — это коды аэропорта соответствующих Metro.                                                                                   |
| Состояние кэша          | Указывает, был ли объект возвращен из кэша или получен из источника.                                                                                                             |
> [!IMPORTANT]
> Функция "необработанные журналы HTTP" доступна автоматически для всех профилей, созданных или обновленных после **25 февраля 2020**. Для профилей CDN, созданных ранее, необходимо обновить конечную точку CDN после настройки ведения журнала. Например, можно переместиться в географическую фильтрацию в конечных точках CDN и заблокировать любую страну, не относящуюся к рабочей нагрузке, и нажать кнопку Сохранить. 

> [!NOTE]
> Журналы можно просмотреть в профиле Log Analytics, выполнив запрос. Пример запроса будет выглядеть как AzureDiagnostics | где Category = = "Азурекднакцесслог"

## <a name="next-steps"></a>Дальнейшие шаги
В этой статье вы включили необработанные журналы HTTP для службы Microsoft CDN.

Дополнительные сведения о Azure CDN и других службах Azure, упомянутых в этой статье, см. в следующих статьях:

* [Анализ](cdn-log-analysis.md) Azure CDN шаблонов использования.

* Дополнительные сведения о [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview).

* Настройте [log Analytics в Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).
