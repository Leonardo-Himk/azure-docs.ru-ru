---
title: Отслеживание входящих запросов в Azure Application Insights с помощью Опенценсус Python | Документация Майкрософт
description: Отслеживайте вызовы запросов для приложений Python через Опенценсус Python.
ms.topic: conceptual
author: lzchen
ms.author: lechen
ms.date: 10/15/2019
ms.openlocfilehash: 0396bd8d150c6145a39f36e7be9e6e2dcacef2c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77669953"
---
# <a name="track-incoming-requests-with-opencensus-python"></a>Мониторинг входящих запросов с помощью Опенценсус Python

Данные входящего запроса собираются с помощью Опенценсус Python и различных интеграций. Отследите данные входящего запроса `django`, отправляемые в веб-приложения, созданные на основе популярных веб `flask` - `pyramid`платформ, и. Затем данные отправляются в Application Insights под Azure Monitor в `requests` качестве телеметрии.

Сначала выполните инструментирование приложения Python с помощью последнего [пакета SDK для Опенценсус Python](../../azure-monitor/app/opencensus-python.md).

## <a name="tracking-django-applications"></a>Отслеживание Django приложений

1. Скачайте и установите `opencensus-ext-django` из [PyPI](https://pypi.org/project/opencensus-ext-django/) и выполните инструментирование приложения с `django` помощью по промежуточного слоя. Входящие запросы, отправляемые `django` в ваше приложение, будут относиться к отслеживанию.

2. Включите `opencensus.ext.django.middleware.OpencensusMiddleware` в `settings.py` файл в `MIDDLEWARE`.

    ```python
    MIDDLEWARE = (
        ...
        'opencensus.ext.django.middleware.OpencensusMiddleware',
        ...
    )
    ```

3. Убедитесь, что Азурикспортер правильно настроен в вашей `settings.py` среде `OPENCENSUS`.

    ```python
    OPENCENSUS = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>"
            )''',
        }
    }
    ```

4. Кроме того, в можно добавлять `settings.py` URL `BLACKLIST_PATHS` -адреса для запросов, которые не следует отслеживанию.

    ```python
    OPENCENSUS = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=0.5)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>",
            )''',
            'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent from it.
        }
    }
    ```

## <a name="tracking-flask-applications"></a>Отслеживание Flask приложений

1. Скачайте и установите `opencensus-ext-flask` из [PyPI](https://pypi.org/project/opencensus-ext-flask/) и выполните инструментирование приложения с `flask` помощью по промежуточного слоя. Входящие запросы, отправляемые `flask` в ваше приложение, будут относиться к отслеживанию.

    ```python
    
    from flask import Flask
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.ext.flask.flask_middleware import FlaskMiddleware
    from opencensus.trace.samplers import ProbabilitySampler
    
    app = Flask(__name__)
    middleware = FlaskMiddleware(
        app,
        exporter=AzureExporter(connection_string="InstrumentationKey=<your-ikey-here>"),
        sampler=ProbabilitySampler(rate=1.0),
    )
    
    @app.route('/')
    def hello():
        return 'Hello World!'
    
    if __name__ == '__main__':
        app.run(host='localhost', port=8080, threaded=True)
    
    ```

2. По `flask` промежуточного слоя можно настроить непосредственно в коде. Для запросов с URL-адресов, которые не нужно отслеживанию, добавьте `BLACKLIST_PATHS`их в.

    ```python
    app.config['OPENCENSUS'] = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1.0)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>",
            )''',
            'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent to it.
        }
    }
    ```

## <a name="tracking-pyramid-applications"></a>Отслеживание пирамидальных приложений

1. Скачайте и установите `opencensus-ext-django` из [PyPI](https://pypi.org/project/opencensus-ext-pyramid/) и выполните инструментирование приложения с `pyramid` помощью анимации. Входящие запросы, отправляемые `pyramid` в ваше приложение, будут относиться к отслеживанию.

    ```python
    def main(global_config, **settings):
        config = Configurator(settings=settings)
    
        config.add_tween('opencensus.ext.pyramid'
                         '.pyramid_middleware.OpenCensusTweenFactory')
    ```

2. Вы можете настроить `pyramid` анимацию непосредственно в коде. Для запросов с URL-адресов, которые не нужно отслеживанию, добавьте `BLACKLIST_PATHS`их в.

    ```python
    settings = {
        'OPENCENSUS': {
            'TRACE': {
                'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1.0)',
                'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                    connection_string="InstrumentationKey=<your-ikey-here>",
                )''',
                'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent to it.
            }
        }
    }
    config = Configurator(settings=settings)
    ```

## <a name="next-steps"></a>Дальнейшие шаги

* [Схема приложений](../../azure-monitor/app/app-map.md)
* [Доступность](../../azure-monitor/app/monitor-web-app-availability.md)
* [Поиск](../../azure-monitor/app/diagnostic-search.md)
* [Запрос к журналу (Analytics)](../../azure-monitor/log-query/log-query-overview.md)
* [Диагностика транзакции](../../azure-monitor/app/transaction-diagnostics.md)
