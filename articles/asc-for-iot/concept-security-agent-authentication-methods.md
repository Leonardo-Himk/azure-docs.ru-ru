---
title: Методы проверки подлинности агента безопасности
description: Узнайте о различных методах проверки подлинности, доступных при использовании центра безопасности Azure для службы IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 0d9d51292c3cae9634af917819b558cdfd2fa04b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81311514"
---
# <a name="security-agent-authentication-methods"></a>Методы проверки подлинности агента безопасности

В этой статье описываются различные методы проверки подлинности, которые можно использовать с агентом Азуреиотсекурити для проверки подлинности в центре Интернета вещей.

Для каждого устройства, подключенного к центру безопасности Azure для Интернета вещей в центре Интернета вещей, требуется модуль безопасности. Для проверки подлинности устройства в центре безопасности Azure для Интернета вещей можно использовать один из двух способов. Выберите метод, который лучше подходит для существующего решения IoT.

> [!div class="checklist"]
> * Секуритимодуле, параметр
> * Параметр устройства

## <a name="authentication-methods"></a>Методы проверки подлинности

Для проверки подлинности агент Азуреиотсекурити выполняет следующие два метода:

- Режим проверки подлинности **секуритимодуле**<br>
Проверка подлинности агента выполняется с помощью удостоверения модуля безопасности независимо от удостоверения устройства.
Используйте этот тип проверки подлинности, если вы хотите, чтобы агент безопасности использовал выделенный метод проверки подлинности через модуль безопасности (только симметричный ключ).

- Режим проверки подлинности **устройства**<br>
В этом методе агент безопасности сначала выполняет проверку подлинности с удостоверением устройства. После первоначальной проверки подлинности центр безопасности Azure для агента IoT выполняет вызов **неактивных** данных в центре Интернета вещей, используя REST API с данными проверки подлинности устройства. Затем центр безопасности Azure для агента IoT запрашивает метод проверки подлинности модуля безопасности и данные из центра Интернета вещей. На последнем шаге центр безопасности Azure для агента IoT выполняет проверку подлинности в центре безопасности Azure для модуля IoT.

Используйте этот тип проверки подлинности, если вы хотите, чтобы агент безопасности повторно использовал существующий метод проверки подлинности устройства (самозаверяющий сертификат или симметричный ключ).

Дополнительные сведения о настройке см. в разделе [Параметры установки агента безопасности](#security-agent-installation-parameters) .

## <a name="authentication-methods-known-limitations"></a>Известные ограничения методов проверки подлинности

- Режим проверки подлинности **секуритимодуле** поддерживает только проверку подлинности с симметричным ключом.
- Сертификат, подписанный ЦС, не поддерживается режимом проверки подлинности **устройства** .

## <a name="security-agent-installation-parameters"></a>Параметры установки агента безопасности

При [развертывании агента безопасности](how-to-deploy-agent.md)в качестве аргументов должны быть предоставлены сведения о проверке подлинности.
Эти аргументы описаны в следующей таблице.

|Имя параметра Linux | Имя параметра Windows | Сокращенный параметр |Описание|Элемент Options|
|---------------------|---------------|---------|---------------|---------------|
|Проверка подлинности — удостоверение|аусентикатионидентити|ауи|Удостоверение проверки подлинности| **Секуритимодуле** или **устройство**|
|authentication-method|AuthenticationMethod|аум|Метод проверки подлинности|**SymmetricKey** или **SelfSignedCertificate**|
|путь к файлу|FilePath|f|Абсолютный полный путь к файлу, содержащему сертификат или симметричный ключ| |
|имя узла|HostName|hn|Полное доменное имя центра Интернета вещей|Пример: ContosoIotHub.azure-devices.net|
|идентификатор устройства|DeviceId|Di|Идентификатор устройства.|Пример: MyDevice1|
|тип сертификата-Location|цертификателокатионкинд|CL|Расположение хранилища сертификатов|**Локальный_файл** или **магазин**|
|

При использовании скрипта установки агента безопасности следующая конфигурация выполняется автоматически. Чтобы изменить проверку подлинности агента безопасности вручную, измените файл конфигурации.

## <a name="change-authentication-method-after-deployment"></a>Изменение метода проверки подлинности после развертывания

При развертывании агента безопасности с помощью скрипта установки автоматически создается файл конфигурации.

Чтобы изменить методы проверки подлинности после развертывания, необходимо вручную изменить файл конфигурации.

### <a name="c-based-security-agent"></a>Агент безопасности на основе C#

Измените _файл Authentication. config_ со следующими параметрами:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>Агент безопасности на основе C

Измените _локалконфигуратион. JSON_ со следующими параметрами:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>См. также

- [Обзор агентов безопасности](security-agent-architecture.md)
- [Развертывание агента безопасности](how-to-deploy-agent.md)
- [Доступ к необработанным данным безопасности](how-to-security-data-access.md)
