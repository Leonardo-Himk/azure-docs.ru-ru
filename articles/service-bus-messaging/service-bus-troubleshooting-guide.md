---
title: Руководство по устранению неполадок в служебной шине Azure | Документация Майкрософт
description: В этой статье представлен список исключений обмена сообщениями служебной шины Azure и предлагаемых действий, которые необходимо выполнить при возникновении исключения.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2020
ms.author: aschhab
ms.openlocfilehash: 63bf035d4e19cc1d64998a6ad533812e71ee71b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80887779"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Руководство по устранению неполадок в служебной шине Azure
В этой статье содержатся советы и рекомендации по устранению некоторых проблем, которые могут возникнуть при использовании служебной шины Azure. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Проблемы с подключением, сертификатом или временем ожидания
Следующие шаги могут помочь при устранении неполадок с подключением, сертификатами и временем ожидания для всех служб в каталоге *. servicebus.windows.net. 

- Перейдите к или [wget](https://www.gnu.org/software/wget/) `https://<yournamespace>.servicebus.windows.net/`. Он помогает проверять наличие IP-фильтрации, виртуальной сети или цепочки сертификатов (чаще всего при использовании пакета SDK для Java).

    Пример успешного сообщения:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Пример сообщения об ошибке сбоя:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Выполните следующую команду, чтобы проверить, заблокирован ли порт в брандмауэре. Используются порты 443 (HTTPS), 5671 (AMQP) и 9354 (NET Messaging/SBMP). В зависимости от используемой библиотеки также используются другие порты. Ниже приведен пример команды, которая проверяет, заблокирован ли порт 5671. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    В Linux

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- При наличии периодических проблем с подключением выполните следующую команду, чтобы проверить наличие пропущенных пакетов. Эта команда попытается установить 25 разных TCP-подключений каждые 1 секунду со службой. После этого можно проверить, сколько из них прошло успешное выполнение и завершилось сбоем, а также увидеть задержку подключения TCP. Это `psping` средство можно загрузить [отсюда](/sysinternals/downloads/psping).

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    Аналогичные команды можно использовать, если вы используете другие средства, такие как `tnc`, `ping`и т. д. 
- Найдите трассировку сети, если предыдущие шаги не помогают и не анализируют их с помощью таких средств, как [Wireshark](https://www.wireshark.org/). При необходимости обратитесь в [Служба поддержки Майкрософт](https://support.microsoft.com/) . 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Проблемы, которые могут возникнуть при обновлении или перезапуске службы
Обновление и перезапуск серверной службы могут привести к следующим последствиям для приложений:

- Запросы могут быть продолжены с немедленным регулированием.
- Может существовать перетаскивание входящих сообщений или запросов.
- Файл журнала может содержать сообщения об ошибках.
- Приложения могут быть отключены от службы в течение нескольких секунд.

Если код приложения использует пакет SDK, то политика повторных попыток уже встроена и активна. Приложение будет повторно подключено без значительного влияния на приложение или рабочий процесс.

## <a name="unauthorized-access-send-claims-are-required"></a>Несанкционированный доступ: требуется отправка утверждений
Эта ошибка может возникать при попытке доступа к разделу служебной шины из Visual Studio на локальном компьютере с помощью назначенного пользователем управляемого удостоверения с разрешениями на отправку.

```bash
Service Bus Error: Unauthorized access. 'Send' claim\(s\) are required to perform this operation.
```

Чтобы устранить эту ошибку, установите библиотеку [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) .  Дополнительные сведения см. в статье [Проверка подлинности локальной разработки](..\key-vault\service-to-service-authentication.md#local-development-authentication). 

Сведения о назначении разрешений ролям см. в статье [Проверка подлинности управляемого удостоверения с Azure Active Directory для доступа к ресурсам служебной шины Azure](service-bus-managed-service-identity.md).

## <a name="next-steps"></a>Дальнейшие шаги
См. следующие статьи: 

- [Azure Resource Manager исключения](service-bus-resource-manager-exceptions.md). В нем перечислены исключения, созданные при взаимодействии с служебной шиной Azure с помощью Azure Resource Manager (через шаблоны или прямые вызовы).
- [Исключения обмена сообщениями](service-bus-messaging-exceptions.md). Он предоставляет список исключений, созданных .NET Framework для служебной шины Azure. 

