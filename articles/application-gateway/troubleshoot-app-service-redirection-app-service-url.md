---
title: Устранение неполадок перенаправления в URL-адрес службы приложений
titleSuffix: Azure Application Gateway
description: В этой статье содержатся сведения о том, как устранить проблему перенаправления при использовании шлюза приложений Azure со службой приложений Azure.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.openlocfilehash: 961ed17bcef19b445c2546a557725bb6bd8653cb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80293540"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>Устранение неполадок службы приложений в шлюзе приложений

Узнайте, как диагностировать и разрешать проблемы, которые могут возникнуть при использовании службы приложений Azure в качестве серверного целевого сервера с помощью шлюза приложений Azure.

## <a name="overview"></a>Обзор

В этой статье вы узнаете, как устранить следующие проблемы.

> [!div class="checklist"]
> * URL-адрес службы приложений предоставляется в браузере при перенаправлении.
> * В качестве домена cookie службы приложений Арраффинити используется имя узла службы приложений example.azurewebsites.net, а не исходный узел.

Когда серверное приложение отправляет ответ перенаправления, может потребоваться перенаправить клиента на URL-адрес, отличный от того, который указан в серверном приложении. Это может потребоваться, если служба приложений размещена за шлюзом приложений и требует от клиента перенаправления на его относительный путь. Примером является перенаправление с contoso.azurewebsites.net/path1 на contoso.azurewebsites.net/path2. 

Когда служба приложений отправляет ответ перенаправления, она использует то же имя узла в заголовке Location своего ответа, что и в запросе, полученном от шлюза приложений. Например, клиент делает запрос напрямую contoso.azurewebsites.net/path2, а не через contoso.com/path2 шлюза приложений. Вы не хотите обходить шлюз приложений.

Эта проблема может возникать по следующим основным причинам.

- У вас есть перенаправление, настроенное в службе приложений. Перенаправление может быть простым добавлением завершающей косой черты в запрос.
- Проверка подлинности Azure Active Directory, что приводит к перенаправлению.

Кроме того, при использовании служб приложений, расположенных за шлюзом приложений, доменное имя, связанное с шлюзом приложений (example.com), отличается от доменного имени службы приложений (скажем, example.azurewebsites.net). Значение домена для файла cookie Арраффинити, заданное службой приложений, содержит имя домена example.azurewebsites.net, которое нежелательно. Исходное имя узла, example.com, должно быть значением доменного имени в файле cookie.

## <a name="sample-configuration"></a>Пример конфигурации

- Прослушиватель HTTP: базовый или несколько сайтов
- Пул адресов серверной части: служба приложений
- Параметры HTTP: **выбрать имя узла из серверного адреса** включено
- Проба: **Выбор имени узла из параметров HTTP** включено

## <a name="cause"></a>Причина

Служба приложений является многоклиентской службой, поэтому она использует заголовок узла в запросе для маршрутизации запроса в правильную конечную точку. Доменное имя по умолчанию для служб приложений, *. azurewebsites.net (скажем, contoso.azurewebsites.net), отличается от доменного имени шлюза приложений (скажем, contoso.com). 

Исходный запрос клиента содержит доменное имя шлюза приложений contoso.com в качестве имени узла. Необходимо настроить шлюз приложений, чтобы изменить имя узла в исходном запросе на имя узла службы приложений при маршрутизации запроса в серверную части службы приложений. Используйте параметр **выбрать имя узла из серверного адреса** в конфигурации параметров HTTP шлюза приложений. Используйте параметр **выбрать имя узла в параметрах HTTP серверной части** в конфигурации зонда работоспособности.



![Имя узла для изменения шлюза приложений](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Когда служба приложений выполняет перенаправление, она использует переопределенное имя узла contoso.azurewebsites.net в заголовке Location вместо исходного имени узла contoso.com, если не указано иное. Проверьте следующие примеры заголовков запросов и ответов.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://contoso.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=contoso.azurewebsites.net

X-Powered-By: ASP.NET
```
Обратите внимание, что в предыдущем примере заголовок ответа имеет код состояния 301 для перенаправления. Заголовок location содержит имя узла службы приложений вместо исходного имени `www.contoso.com`узла.

## <a name="solution-rewrite-the-location-header"></a>Решение. Перепишите заголовок Location

Задайте имя узла в заголовке Location в качестве имени домена шлюза приложений. Для этого создайте [правило перезаписи](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) с условием, которое оценивает, содержит ли заголовок Location в ответе azurewebsites.NET. Кроме того, необходимо выполнить действие для перезаписи заголовка Location, чтобы он имел имя узла шлюза приложений. Дополнительные сведения см. в разделе инструкции по [перезаписи заголовка Location](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#modify-a-redirection-url).

> [!NOTE]
> Поддержка переопределения HTTP-заголовков доступна только для [Standard_v2 и WAF_V2 SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) шлюза приложений. При использовании номера SKU v1 рекомендуется [выполнить миграцию с версии v1 на версию 2](https://docs.microsoft.com/azure/application-gateway/migrate-v1-v2). Вы хотите использовать функцию перезаписи и другие [Расширенные возможности](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#feature-comparison-between-v1-sku-and-v2-sku) , доступные в версии v2.

## <a name="alternate-solution-use-a-custom-domain-name"></a>Альтернативное решение. Используйте имя пользовательского домена.

Если вы используете номер SKU v1, вы не сможете переписать заголовок Location. Эта возможность доступна только для SKU версии 2. Чтобы устранить проблему перенаправления, передайте имя узла, которое шлюз приложений получит в службу приложений, вместо того, чтобы переопределять узел.

Служба приложений теперь выполняет перенаправление (если таковое имеется) в том же исходном заголовке узла, который указывает на шлюз приложений, а не на его собственный.

Необходимо быть владельцем личного домена и выполнить следующую процедуру:

- Зарегистрируйте домен в списке личный домен службы приложений. В пользовательском домене должна быть запись CNAME, указывающая на полное доменное имя службы приложений. Дополнительные сведения см. [в статье сопоставьте существующее настраиваемое DNS-имя со службой приложений Azure](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

    ![Список пользовательских доменов службы приложений](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Ваша служба приложений готова принять имя `www.contoso.com`узла. Измените запись CNAME в DNS, чтобы она указывала на полное доменное имя шлюза приложений, например `appgw.eastus.cloudapp.azure.com`.

- Убедитесь, что домен `www.contoso.com` разрешается в полное доменное имя шлюза приложений при выполнении запроса DNS.

- Настройте пользовательскую пробу, чтобы отключить **Выбор имени узла из серверных параметров HTTP**. В портал Azure снимите флажок в параметрах зонда. В PowerShell не используйте параметр **-пиккхостнамефромбаккендхттпсеттингс** в команде **Set-азаппликатионгатевайпробеконфиг** . В поле имя узла зонда введите полное доменное имя службы приложений, example.azurewebsites.net. Запросы пробы, отправленные из шлюза приложений, содержат полное доменное имя в заголовке узла.

  > [!NOTE]
  > Для следующего шага убедитесь, что пользовательская проверка не связана с параметрами HTTP серверной части. В этом месте параметры HTTP по-прежнему имеют параметр " **выбрать имя узла из серверного** коммутатора адресов".

- Задайте параметры HTTP для шлюза приложений, чтобы отключить **Выбор имени узла из серверного адреса**. В портал Azure снимите флажок. В PowerShell не используйте параметр **-пиккхостнамефромбаккендаддресс** в команде **Set-азаппликатионгатевайбаккендхттпсеттингс** .

- Свяжите пользовательскую пробу с параметрами HTTP серверной части и убедитесь, что серверная части работоспособна.

- Шлюз приложений теперь должен перенаправлять одно и то же имя `www.contoso.com`узла в службу приложений. Перенаправление происходит в том же имени узла. Проверьте следующие примеры заголовков запросов и ответов.

Чтобы реализовать предыдущие шаги с помощью PowerShell для существующей установки, используйте приведенный ниже пример сценария PowerShell. Обратите внимание, что мы не использовали параметры **-пиккхостнаме** в конфигурации проверки и HTTP-параметров.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Дальнейшие шаги

Если описанные выше действия не помогли устранить проблему, отправьте запрос в [службу поддержки](https://azure.microsoft.com/support/options/).
