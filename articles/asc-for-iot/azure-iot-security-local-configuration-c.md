---
title: Локальная конфигурация агента безопасности (C)
description: Сведения о центре безопасности Azure для локальных конфигураций агента для C.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: cd344b9bebb69af210c482f46af6b2dd7edf7816
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81311710"
---
# <a name="understanding-the-localconfigurationjson-file---c-agent"></a>Основные сведения о файле LocalConfiguration.json — агент C

В центре безопасности Azure для агента безопасности IoT используются конфигурации из локального файла конфигурации.
Агент безопасности считывает конфигурацию один раз при запуске агента.
Конфигурация, найденная в локальном файле конфигурации, содержит конфигурацию проверки подлинности и другие конфигурации, связанные с агентом.
Файл содержит конфигурации в парах "ключ-значение" в нотации JSON, а конфигурации заполняются при установке агента.

По умолчанию файл находится в папке:/Вар/асЦиотажент/локалконфигуратион.жсон

Изменения в файле конфигурации происходят при перезапуске агента.

## <a name="security-agent-configurations-for-c"></a>Конфигурации агента безопасности для C

| Имя конфигурации | Возможные значения | Сведения |
|:-----------|:---------------|:--------|
| AgentId | Идентификатор GUID | Уникальный идентификатор агента |
| тригжердевентсинтервал | Строка ISO8601 | Интервал планировщика для коллекции активированных событий |
| ConnectionTimeout | Строка ISO8601 | Истекло время ожидания подключения к IoThub |
| Проверка подлинности | JsonObject | Конфигурация проверки подлинности. Этот объект содержит все сведения, необходимые для проверки подлинности в IoTHub |
| Идентификация | "DPS", "Секуритимодуле", "Device" | Удостоверение проверки подлинности — DP, если проверка подлинности выполняется с помощью DPS, Секуритимодуле, если проверка подлинности выполняется через учетные данные модуля безопасности или устройства, если проверка подлинности |
| AuthenticationMethod | "SasToken", "SelfSignedCertificate" | Секрет пользователя для проверки подлинности. Выберите SasToken, если секретный ключ используется как симметричный, выберите самозаверяющий сертификат, если секретный сертификат является самозаверяющим.  |
| FilePath | Путь к файлу (строка) | Путь к файлу, содержащему секрет проверки подлинности |
| HostName | строка | Имя узла центра Интернета вещей Azure. обычно <My-Hub>. azure-devices.net |
| DeviceId | строка | ИДЕНТИФИКАТОР устройства (зарегистрированный в центре Интернета вещей Azure) |
| DPS | JsonObject | Конфигурации, связанные с DP |
| IDScope | строка | Область ИДЕНТИФИКАТОРов DPS |
| ИД регистрации | строка  | ИДЕНТИФИКАТОР регистрации устройства DPS |
| Logging | JsonObject | Конфигурации, связанные с регистратором агентов |
| системлогжерминимумсеверити | 0 <= число <= 4 | сообщения журнала, равные и превышающие эту серьезность, будут зарегистрированы в/Вар/лог/сислог (0 — наименьшая серьезность) |
| диагностицевентминимумсеверити | 0 <= число <= 4 | сообщения журнала, равные и превышающие эту серьезность, будут отправляться как диагностические события (0 — наименьшая серьезность) |

## <a name="security-agent-configurations-code-example"></a>Пример кода конфигураций агента безопасности

```JSON
{
    "Configuration" : {
        "AgentId" : "b97faf0a-0f57-471f-9dab-46a8e1764946",
        "TriggerdEventsInterval" : "PT2M",
        "ConnectionTimeout" : "PT30S",
        "Authentication" : {
            "Identity" : "Device",
            "AuthenticationMethod" : "SasToken",
            "FilePath" : "/path/to/my/SymmetricKey",
            "DeviceId" : "my-device",
            "HostName" : "my-iothub.azure-devices.net",
            "DPS" : {
                "IDScope" : "",
                "RegistrationId" : ""
            }
        },
        "Logging": {
            "SystemLoggerMinimumSeverity": 0,
            "DiagnoticEventMinimumSeverity": 2
        }
    }
}
```

## <a name="next-steps"></a>Дальнейшие шаги

- Ознакомьтесь с [обзором](overview.md) центра безопасности Azure для службы IOT
- Узнайте больше о центре безопасности Azure для [архитектуры](architecture.md) IOT
- Включение центра безопасности Azure для [службы](quickstart-onboard-iot-hub.md) IOT
- [Вопросы и ответы](resources-frequently-asked-questions.md) о центре безопасности Azure для службы IOT
- Узнайте, как получить доступ к [необработанным данным безопасности](how-to-security-data-access.md).
- Общие сведения о [рекомендациях](concept-recommendations.md).
- Общие сведения об [оповещениях](concept-security-alerts.md) системы безопасности
