---
title: Развертывание службы управления сертификатами хранилища OPC в Azure | Документация Майкрософт
description: Развертывание службы управления сертификатами хранилища OPC с нуля.
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 7ee186684b702a42335c6e1a7832cc5c761a69d0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81686942"
---
# <a name="build-and-deploy-the-opc-vault-certificate-management-service"></a>Создание и развертывание службы управления сертификатами хранилища OPC

В этой статье объясняется, как развернуть службу управления сертификатами хранилища OPC в Azure.

> [!NOTE]
> Дополнительные сведения см. в [репозитории хранилища GitHub OPC](https://github.com/Azure/azure-iiot-opc-vault-service).

## <a name="prerequisites"></a>Предварительные условия

### <a name="install-required-software"></a>Установка необходимого программного обеспечения

В настоящее время операция сборки и развертывания ограничена Windows.
Примеры написаны для .NET Standard C#, для которых необходимо создать службу и примеры для развертывания.
Все средства, необходимые для .NET Standard, входят в состав средств .NET Core. См. статью Начало [работы с .NET Core](https://docs.microsoft.com/dotnet/articles/core/getting-started).

1. [Установите .NET Core 2.1 +][dotnet-install].
2. [Установите DOCKER][docker-url] (необязательно, только если требуется локальная сборка DOCKER).
4. Установите [средства командной строки Azure для PowerShell][powershell-install].
5. Подпишитесь на [подписку Azure][azure-free].

### <a name="clone-the-repository"></a>Клонирование репозитория

Если вы еще не сделали этого, клонировать этот репозиторий GitHub. Откройте командную строку или терминал и выполните следующую команду:

```bash
git clone https://github.com/Azure/azure-iiot-opc-vault-service
cd azure-iiot-opc-vault-service 
```

Кроме того, репозиторий можно клонировать непосредственно в Visual Studio 2017.

### <a name="build-and-deploy-the-azure-service-on-windows"></a>Создание и развертывание службы Azure в Windows

Сценарий PowerShell предоставляет простой способ развертывания микрослужбы хранилища OPC и приложения.

1. Откройте окно PowerShell в корне репозитория. 
3. Перейдите в папку `cd deploy`Deploy.
3. Выберите имя `myResourceGroup` , которое вряд ли приведет к конфликту с другими развернутыми веб-страницами. См. раздел "имя веб-сайта уже используется" Далее в этой статье.
5. Запустите развертывание с помощью `.\deploy.ps1` для интерактивной установки или введите полную командную строку:  
`.\deploy.ps1  -subscriptionName "MySubscriptionName" -resourceGroupLocation "East US" -tenantId "myTenantId" -resourceGroupName "myResourceGroup"`
7. Если планируется разработка с использованием этого развертывания, добавьте `-development 1` , чтобы включить пользовательский интерфейс Swagger и развернуть отладочные сборки.
6. Следуйте инструкциям в сценарии, чтобы войти в подписку и предоставить дополнительные сведения.
9. После успешной операции сборки и развертывания должно отобразиться следующее сообщение:
   ```
   To access the web client go to:
   https://myResourceGroup.azurewebsites.net

   To access the web service go to:
   https://myResourceGroup-service.azurewebsites.net

   To start the local docker GDS server:
   .\myResourceGroup-dockergds.cmd

   To start the local dotnet GDS server:
   .\myResourceGroup-gds.cmd
   ```

   > [!NOTE]
   > В случае возникновения проблем см. раздел "Устранение неполадок при развертывании" Далее в этой статье.

8. Откройте свой любимый браузер и откройте страницу приложения:`https://myResourceGroup.azurewebsites.net`
8. Присвойте веб-приложению и микрослужбе хранилища OPC несколько минут, чтобы развернуть после развертывания. Домашняя страница веб-сайта может зависнуть при первом использовании, до минуты, пока вы не получите первые ответы.
11. Чтобы ознакомиться с API Swagger, откройте:`https://myResourceGroup-service.azurewebsites.net`
13. Чтобы запустить локальный сервер GDS с помощью DotNet, запустите `.\myResourceGroup-gds.cmd`. С помощью DOCKER запустите `.\myResourceGroup-dockergds.cmd`.

Можно повторно развернуть сборку с теми же параметрами. Имейте в виду, что такая операция обновляет секреты всех приложений и может сбрасывать некоторые параметры в Azure Active Directory регистрации приложений (Azure AD).

Кроме того, можно повторно развернуть только двоичные файлы веб-приложения. С помощью параметра `-onlyBuild 1`новые ZIP-пакеты службы и приложения развертываются в веб приложения.

После успешного развертывания можно приступить к использованию служб. См. [руководство по управлению сертификатами хранилища OPC](howto-opc-vault-manage.md).

## <a name="delete-the-services-from-the-subscription"></a>Удаление служб из подписки

Это делается так.

1. Войдите на [портал Azure](https://portal.azure.com).
2. Перейдите к группе ресурсов, в которой была развернута служба.
3. Выберите **Удалить группу ресурсов** и подтвердите выбор.
4. Через некоторое время все развернутые компоненты службы будут удалены.
5. Перейдите в **Azure Active Directory** > **Регистрация приложений**.
6. Для каждой развернутой группы ресурсов должно быть указано три регистрации. Регистрации имеют следующие имена: `resourcegroup-client`, `resourcegroup-module`,. `resourcegroup-service` Удалите каждую регистрацию отдельно.

Теперь все развернутые компоненты будут удалены.

## <a name="troubleshooting-deployment-failures"></a>Устранение неполадок при развертывании

### <a name="resource-group-name"></a>Имя группы ресурсов

Используйте короткое и простое имя группы ресурсов. Имя также используется для именования ресурсов и префикса URL-адреса службы. Таким образом, он должен соответствовать требованиям к именованию ресурсов.  

### <a name="website-name-already-in-use"></a>Имя веб-сайта уже используется

Возможно, имя веб-сайта уже используется. Необходимо использовать другое имя группы ресурсов. В скрипте развертывания используются имена узлов: https:\//resourcegroupname.azurewebsites.NET и HTTPS:\//resourgroupname-Service.azurewebsites.NET.
Другие имена служб создаются с помощью сочетания хэшей коротких имен и вряд ли конфликтуют с другими службами.

### <a name="azure-ad-registration"></a>регистрация в Azure AD. 

Сценарий развертывания пытается зарегистрировать три приложения Azure AD в Azure AD. В зависимости от ваших разрешений в выбранном клиенте Azure AD эта операция может завершиться ошибкой. Существует два варианта.

- Если вы выбрали клиент Azure AD из списка клиентов, перезапустите сценарий и выберите другой из списка.
- Кроме того, можно развернуть частный клиент Azure AD в другой подписке. Перезапустите сценарий и выберите его использование.

## <a name="deployment-script-options"></a>Параметры скрипта развертывания

Скрипт принимает следующие параметры:


```
-resourceGroupName
```

Это может быть имя существующей или новой группы ресурсов.

```
-subscriptionId
```


Это идентификатор подписки, в которой будут развернуты ресурсы. Это необязательно.

```
-subscriptionName
```


Кроме того, можно использовать имя подписки.

```
-resourceGroupLocation
```


Это расположение группы ресурсов. Если этот параметр указан, он пытается создать новую группу ресурсов в этом расположении. Этот параметр также является необязательным.


```
-tenantId
```


Это клиент Azure AD для использования. 

```
-development 0|1
```

Это развертывание для разработки. Используйте отладочную сборку и задайте среду ASP.NET для разработки. Создайте `.publishsettings` для импорта в Visual Studio 2017, чтобы разрешить ему развертывание приложения и службы напрямую. Этот параметр также является необязательным.

```
-onlyBuild 0|1
```

Это необходимо для перестроения и повторного развертывания только веб-приложений, а также для перестроения контейнеров DOCKER. Этот параметр также является необязательным.

[azure-free]:https://azure.microsoft.com/free/
[powershell-install]:https://azure.microsoft.com/downloads/#powershell
[docker-url]: https://www.docker.com/
[dotnet-install]: https://www.microsoft.com/net/learn/get-started

## <a name="next-steps"></a>Дальнейшие шаги

Теперь, когда вы узнали, как развертывать хранилище OPC с нуля, вы можете:

> [!div class="nextstepaction"]
> [Управление хранилищем OPC](howto-opc-vault-manage.md)
