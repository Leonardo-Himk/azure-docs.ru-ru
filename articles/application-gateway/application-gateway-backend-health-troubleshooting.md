---
title: Устранение проблем работоспособности серверной части в шлюзе приложений Azure
description: В этой статье описывается, как устранять проблемы работоспособности серверной части для шлюза приложений Azure.
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: article
ms.date: 08/30/2019
ms.author: surmb
ms.openlocfilehash: a16120194b1b8015466005f42336828c2b4ace6c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80983846"
---
<a name="troubleshoot-backend-health-issues-in-application-gateway"></a>Устранение проблем работоспособности серверной части в шлюзе приложений
==================================================

<a name="overview"></a>Обзор
--------

По умолчанию шлюз приложений Azure проверяет состояние работоспособности внутренних серверов и проверяет, готовы ли они к обработке запросов. Пользователи также могут создать пользовательские зонды, указав имя узла, путь для проверки и коды состояния, которые должны приниматься как работоспособные. В каждом случае, если внутренний сервер не отвечает, шлюз приложений помечает сервер как неработоспособный и останавливает пересылку запросов на сервер. После успешного ответа сервера Шлюз приложений возобновляет запросы.

### <a name="how-to-check-backend-health"></a>Проверка работоспособности серверной части

Чтобы проверить работоспособность серверного пула, можно использовать страницу **работоспособность серверной части** на портал Azure. Также можно использовать [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhealth?view=azps-2.6.0), [CLI](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-show-backend-health)или [REST API](https://docs.microsoft.com/rest/api/application-gateway/applicationgateways/backendhealth).

Состояние, полученное любым из этих методов, может быть одним из следующих:

- Работоспособно

- Unhealthy

- Неизвестно

Если состояние работоспособности серверной части работоспособно, это означает, что шлюз приложений перенаправит запросы на этот сервер. Но если работоспособность серверной части для всех серверов в серверном пуле неработоспособна или неизвестна, при попытке доступа к приложениям могут возникнуть проблемы. В этой статье описаны признаки, причины и способы их устранения для каждой из отображаемых ошибок.

<a name="backend-health-status-unhealthy"></a>Состояние работоспособности серверной части: неработоспособность
-------------------------------

Если состояние работоспособности серверной части является неработоспособным, представление портала будет выглядеть следующим образом:

![Работоспособность серверной части шлюза приложений — неработоспособность](./media/application-gateway-backend-health-troubleshooting/appgwunhealthy.png)

Если вы используете Azure PowerShell, CLI или Azure REST API Query, вы получите ответ, похожий на следующий:
```azurepowershell
PS C:\Users\testuser\> Get-AzApplicationGatewayBackendHealth -Name "appgw1" -ResourceGroupName "rgOne"
BackendAddressPools :
{Microsoft.Azure.Commands.Network.Models.PSApplicationGatewayBackendHealthPool}
BackendAddressPoolsText : [
{
                              "BackendAddressPool": {
                                "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appgw1/b
                          ackendAddressPools/appGatewayBackendPool"
                              },
                              "BackendHttpSettingsCollection": [
                                {
                                  "BackendHttpSettings": {
                                    "TrustedRootCertificates": [],
                                    "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appg
                          w1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
                                  },
                                  "Servers": [
                                    {
                                      "Address": "10.0.0.5",
                                      "Health": "Healthy"
                                    },
                                    {
                                      "Address": "10.0.0.6",
                                      "Health": "Unhealthy"
                                    }
                                  ]
                                }
                              ]
                            }
                        ]
```
После получения состояния неработоспособного внутреннего сервера для всех серверов в серверном пуле запросы не пересылаются на серверы, а шлюз приложений возвращает запрашивающему клиенту ошибку "502 неверный шлюз". Чтобы устранить эту проблему, просмотрите столбец **сведения** на вкладке **работоспособность серверной части** .

Сообщение, отображаемое в столбце **сведения** , содержит более подробные сведения о данной неполадке, и на их основе можно приступить к устранению проблемы.

> [!NOTE]
> Запрос проверки по умолчанию отправляется в \<формате протокола\>://127.0.0.1:\<Port\>/. Например, http://127.0.0.1:80 для проверки HTTP на порте 80. Только коды состояния HTTP от 200 до 399 считаются работоспособными. Протокол и порт назначения наследуются от параметров HTTP. Если требуется, чтобы шлюз приложений проверит наличие другого протокола, имени узла или пути и распознало другой код состояния как работоспособный, настройте пользовательскую проверку и свяжите ее с параметрами HTTP.

<a name="error-messages"></a>Сообщения об ошибках
------------------------
#### <a name="backend-server-timeout"></a>Время ожидания внутреннего сервера

**Сообщение:** Время, затраченное серверной частью на реагирование\'на проверку работоспособности шлюза приложений, превышает пороговое значение времени ожидания в параметре проверки.

**Причина:** После того как шлюз приложений отправит запрос проверки HTTP (S) на внутренний сервер, он ждет ответа от внутреннего сервера в течение заданного периода. Если внутренний сервер не отвечает в течение заданного периода (значение времени ожидания), он помечается как неработоспособный, пока не начнет отвечать в течение заданного периода ожидания.

**Решение:** Проверьте, почему внутренний сервер или приложение не отвечают в течение заданного периода ожидания, а также проверьте зависимости приложения. Например, проверьте, есть ли у базы данных проблемы, которые могут вызвать задержку в ответе. Если вам известно о поведении приложения и он должен отвечать только после значения времени ожидания, увеличьте значение времени ожидания в параметрах пользовательской проверки. Для изменения значения времени ожидания необходимо иметь пользовательскую пробу. Сведения о настройке пользовательской пробы [см. на странице документации](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal).

Чтобы увеличить значение времени ожидания, выполните следующие действия.

1.  Непосредственное обращение к внутреннему серверу и проверка времени, затраченного на ответ сервера на эту страницу. Для доступа к внутреннему серверу можно использовать любое средство, включая браузер с помощью средств разработчика.

1.  Определив время ответа приложения, перейдите на вкладку **зонды работоспособности** и выберите проверку, связанную с параметрами HTTP.

1.  Введите любое значение времени ожидания, превышающее время ответа приложения в секундах.

1.  Сохраните пользовательские параметры проверки и проверьте, отображается ли работоспособность серверной части как работоспособная.

#### <a name="dns-resolution-error"></a>Ошибка разрешения DNS

**Сообщение:** Шлюзу приложений не удалось создать зонд для этой серверной части. Обычно это происходит, когда полное доменное имя внутренней службы введено неправильно. 

**Причина:** Если серверный пул имеет тип IP-адреса/FQDN или служба приложений, шлюз приложений разрешается в IP-адрес полного доменного имени, введенного с помощью службы доменных имен (DNS) (настраиваемое или Azure Default), и пытается подключиться к серверу по TCP-порту, упомянутому в параметрах HTTP. Но если это сообщение отображается, это означает, что шлюзу приложений не удалось успешно разрешить IP-адрес указанного полного доменного имени.

**Решение.**

1.  Убедитесь, что полное доменное имя, указанное во внутреннем пуле, указано правильно и является общедоступным доменом, а затем попробуйте разрешить его с локального компьютера.

1.  Если вы можете разрешить IP-адрес, возможно, в виртуальной сети возникли проблемы с конфигурацией DNS.

1.  Проверьте, настроена ли для виртуальной сети настраиваемый DNS-сервер. Если это так, проверьте DNS-сервер о том, почему он не может быть разрешен, на IP-адрес указанного FQDN.

1.  Если вы используете DNS Azure по умолчанию, обратитесь к регистратору доменных имен, чтобы узнать, завершено ли правильное сопоставление записи или записи CNAME.

1.  Если домен является частным или внутренним, попробуйте разрешить его из виртуальной машины в той же виртуальной сети. Если вы можете устранить проблему, перезапустите шлюз приложений и снова проверьте его. Чтобы перезапустить шлюз приложений, необходимо приступить [к его](https://docs.microsoft.com/powershell/module/azurerm.network/stop-azurermapplicationgateway?view=azurermps-6.13.0) [запуску](https://docs.microsoft.com/powershell/module/azurerm.network/start-azurermapplicationgateway?view=azurermps-6.13.0) с помощью команд PowerShell, описанных в этих связанных ресурсах.

#### <a name="tcp-connect-error"></a>Ошибка TCP-подключения

**Сообщение:** Шлюзу приложений не удалось подключиться к серверной части.
Убедитесь, что серверная часть отвечает на порт, используемый для пробы.
Также проверьте, блокирует ли любой NSG/UDR/Firewall доступ к IP-адресу и порту этой серверной части.

**Причина:** После фазы разрешения DNS шлюз приложений пытается подключиться к внутреннему серверу через TCP-порт, настроенный в параметрах HTTP. Если шлюзу приложений не удается установить сеанс TCP в указанном порте, проба помечается как неработоспособное с этим сообщением.

**Решение:** При возникновении этой ошибки выполните следующие действия.

1.  Проверьте, можно ли подключиться к внутреннему серверу через порт, упомянутый в параметрах HTTP, с помощью браузера или PowerShell. Например, выполните следующую команду:`Test-NetConnection -ComputerName
    www.bing.com -Port 443`

1.  Если порт, который указан, не является нужным, введите правильный номер порта шлюза приложений для подключения к внутреннему серверу.

1.  Если вы не можете подключиться к порту с локального компьютера, выполните следующие действия.

    a.  Проверьте параметры группы безопасности сети (NSG) сетевого адаптера и подсети внутреннего сервера и убедитесь, что разрешены входящие подключения к настроенному порту. Если это не так, создайте новое правило, разрешающее подключения. Сведения о создании правил NSG [см. на странице документации](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-security-rules).

    b.  Проверьте, допускают ли параметры NSG подсети шлюза приложений исходящий общий и частный трафик, чтобы можно было установить соединение. Дополнительные сведения о создании правил NSG см. на странице документа, приведенной в действии 3a.
    ```azurepowershell
            $vnet = Get-AzVirtualNetwork -Name "vnetName" -ResourceGroupName "rgName"
            Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
    ```

    c.  Проверьте параметры определяемых пользователем маршрутов (UDR) шлюза приложений и подсети внутреннего сервера для всех аномалий маршрутизации. Убедитесь, что UDR не направляет трафик вне внутренней подсети. Например, проверьте маршруты к виртуальным сетевым устройствам или маршруты по умолчанию, объявляемые в подсети шлюза приложений через Azure ExpressRoute или VPN.

    d.  Чтобы проверить действующие маршруты и правила для сетевого адаптера, можно использовать следующие команды PowerShell:
    ```azurepowershell
            Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
            Get-AzEffectiveRouteTable -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
    ```
1.  Если проблем с NSG или UDR не обнаружено, проверьте наличие проблем, связанных с приложением, на внутреннем сервере, которые не позволяют клиентам устанавливать сеанс TCP на настроенных портах. Необходимо проверить следующие моменты:

    a.  Откройте командную строку (Win + R-\> cmd), введите `netstat`и нажмите клавишу ВВОД.

    b.  Проверьте, прослушивает ли сервер порт, который настроен. Пример:
    ```
            Proto Local Address Foreign Address State PID
            TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 4
    ```
    c.  Если он не прослушивает настроенный порт, проверьте параметры веб-сервера. Например: привязки сайта в IIS, серверный блок в NGINX и виртуальном узле в Apache.

    d.  Проверьте параметры брандмауэра ОС, чтобы убедиться, что входящий трафик к порту разрешен.

#### <a name="http-status-code-mismatch"></a>Несоответствие кода состояния HTTP

**Сообщение:** Код состояния HTTP-ответа\'серверной части не соответствует параметру пробы. Ожидалось: {HTTPStatusCode0} получено: {HTTPStatusCode1}.

**Причина:** После установки подключения TCP и выполнения подтверждения TLS (если включено TLS) шлюз приложений отправит зонд в качестве HTTP-запроса GET на внутренний сервер. Как было сказано ранее, проба по умолчанию \<будет\>соответствовать Протоколу:/\</\>127.0.0.1: Port/и считает коды состояния ответа в Rage 200 – 399 как работоспособные. Если сервер возвращает любой другой код состояния, он будет помечен как неработоспособный с помощью этого сообщения.

**Решение:** В зависимости от кода ответа внутреннего сервера можно выполнить следующие действия. Ниже перечислены некоторые общие коды состояния.

| **Ошибка** | **Действия** |
| --- | --- |
| Несоответствие кода состояния зонда: получено 401 | Проверьте, требуется ли проверка подлинности на внутреннем сервере. На этом этапе зонды шлюза приложений не могут передавать учетные данные для проверки подлинности. Либо \"разрешите\" HTTP 401 в коде состояния зонда, либо выполнит проверку по пути, где сервер не требует проверки подлинности. | |
| Несоответствие кода состояния зонда: получено 403 | Доступ запрещен. Проверьте, разрешен ли доступ к пути на внутреннем сервере. | |
| Несоответствие кода состояния зонда: получено 404 | Страница не найдена. Проверьте, доступен ли путь к имени узла на внутреннем сервере. Измените параметр имени узла или пути на доступное значение. | |
| Несоответствие кода состояния зонда: получено 405 | В запросах пробы для шлюза приложений используется метод HTTP GET. Проверьте, разрешает ли сервер этот метод. | |
| Несоответствие кода состояния зонда: получено 500 | Внутренняя ошибка сервера. Проверьте работоспособность внутреннего сервера и убедитесь, что службы работают. | |
| Несоответствие кода состояния зонда: получено 503 | Служба недоступна. Проверьте работоспособность внутреннего сервера и убедитесь, что службы работают. | |

Или, если вы считаете, что ответ является допустимым и вы хотите, чтобы шлюз приложений принимал другие коды состояния как работоспособные, можно создать пользовательскую проверку. Этот подход полезен в ситуациях, когда веб-сайт внутреннего сервера нуждается в проверке подлинности. Так как запросы пробы не содержат учетных данных пользователя, они завершатся ошибкой и код состояния HTTP 401 будет возвращен внутренним сервером.

Чтобы создать пользовательскую проверку, выполните следующие [действия](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal).

#### <a name="http-response-body-mismatch"></a>Несоответствие тела ответа HTTP

**Сообщение:** Текст HTTP-ответа\'серверной части не соответствует параметру зонда. Полученный текст ответа не содержит {String}.

**Причина:** При создании пользовательской пробы можно пометить внутренний сервер как работоспособный, сопоставляя строку из текста ответа. Например, можно настроить шлюз приложений таким образом, чтобы он принимал "несанкционированные" в качестве строки для сопоставления. Если ответ внутреннего сервера для запроса пробы содержит строку с **несанкционированным**выполнением, он будет помечен как работоспособный. В противном случае оно будет помечено как неработоспособное с помощью этого сообщения.

**Решение:** Чтобы устранить эту проблему, выполните следующие действия.

1.  Получите доступ к внутреннему серверу локально или с клиентского компьютера на пути пробы и проверьте текст ответа.

1.  Убедитесь, что текст ответа в настраиваемой конфигурации проверки шлюза приложений совпадает с настроенным.

1.  Если они не совпадают, измените конфигурацию зонда, чтобы для параметра было указано правильное строковое значение.

Узнайте больше о [сопоставлении проверки шлюза приложений](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#probe-matching).

#### <a name="backend-server-certificate-invalid-ca"></a>Недопустимый центр сертификации внутреннего сервера

**Сообщение:** Сертификат сервера, используемый серверной частью, не подписан известным центром сертификации (ЦС). Список разрешений серверную часть шлюза приложений, загружая корневой сертификат сертификата сервера, используемого серверной частью.

**Причина:** Сквозной протокол SSL с шлюзом приложений версии 2 требует проверки сертификата внутреннего сервера, чтобы считать сервер работоспособным.
Чтобы сертификат TLS/SSL был доверенным, этот сертификат должен выдаваться центром сертификации, который входит в состав доверенного хранилища шлюза приложений. Если сертификат не был выдан доверенным центром сертификации (например, если использовался самозаверяющий сертификат), пользователи должны передать сертификат поставщика в шлюз приложений.

**Решение:** Выполните следующие действия, чтобы экспортировать и передать доверенный корневой сертификат в шлюз приложений. (Эти действия предназначены для клиентов Windows.)

1.  Войдите на компьютер, где размещено ваше приложение.

1.  Выберите Win + R или щелкните правой кнопкой мыши кнопку **Пуск** и выберите команду **выполнить**.

1.  Введите `certmgr.msc` и нажмите клавишу ВВОД. Также можно выполнить поиск диспетчера сертификатов в меню " **Пуск** ".

1.  Выберите сертификат, обычно в `\Certificates - Current User\\Personal\\Certificates\`и откройте его.

1.  Выберите корневой сертификат и нажмите кнопку **Просмотр сертификата**.

1.  В свойствах сертификата перейдите на вкладку **сведения** .

1.  На вкладке **сведения** выберите параметр **Копировать в файл** и сохраните файл в Base-64 Encoded X. 509 (. CER).

1.  Откройте страницу **параметров** HTTP шлюза приложений в портал Azure.

1. Откройте параметры HTTP, щелкните **Добавить сертификат**и выберите только что сохраненный файл сертификата.

1. Нажмите кнопку **сохранить** , чтобы сохранить параметры HTTP.

Кроме того, можно экспортировать корневой сертификат с клиентского компьютера, напрямую обратившись к серверу (минуя шлюз приложений) через браузер и экспортировав корневой сертификат из браузера.

Дополнительные сведения о том, как извлечь и передать Доверенные корневые сертификаты в шлюзе приложений, см. в статье [Экспорт доверенного корневого сертификата (для SKU v2)](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).

#### <a name="trusted-root-certificate-mismatch"></a>Несоответствие доверенного корневого сертификата

**Сообщение:** Корневой сертификат сертификата сервера, используемый серверной частью, не совпадает с доверенным корневым сертификатом, добавленным в шлюз приложений. Убедитесь, что вы добавили правильный корневой сертификат для список разрешений серверной части.

**Причина:** Сквозной протокол SSL с шлюзом приложений версии 2 требует проверки сертификата внутреннего сервера, чтобы считать сервер работоспособным.
Чтобы сертификат TLS/SSL был доверенным, сертификат внутреннего сервера должен выдаваться центром сертификации, который входит в состав доверенного хранилища шлюза приложений. Если сертификат не был выдан доверенным центром сертификации (например, использовался самозаверяющий сертификат), пользователи должны передать сертификат поставщика в шлюз приложений.

Сертификат, который был отправлен в параметры HTTP шлюза приложений, должен соответствовать корневому сертификату сертификата внутреннего сервера.

**Решение:** Если получено это сообщение об ошибке, то существует несоответствие между сертификатом, который был отправлен в шлюз приложений, и переданным на внутренний сервер.

Выполните шаги 1-11 в предыдущем методе, чтобы отправить правильный доверенный корневой сертификат в шлюз приложений.

Дополнительные сведения о том, как извлечь и передать Доверенные корневые сертификаты в шлюзе приложений, см. в статье [Экспорт доверенного корневого сертификата (для SKU v2)](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).
> [!NOTE]
> Эта ошибка также может возникать, если внутренний сервер не обменивается всей цепочкой сертификата, включая корневой > промежуточный (если применимо) > конечный в течение подтверждения TLS. Чтобы проверить, можно использовать команды OpenSSL с любого клиента и подключиться к внутреннему серверу с помощью настроенных параметров в зонде шлюза приложений.

Пример:
```
OpenSSL> s_client -connect 10.0.0.4:443 -servername www.example.com -showcerts
```
Если в выходных данных не отображается полная цепочка возвращаемого сертификата, экспортируйте сертификат повторно с помощью полной цепочки, включая корневой сертификат. Настройте этот сертификат на внутреннем сервере. 

```
  CONNECTED(00000188)\
  depth=0 OU = Domain Control Validated, CN = \*.example.com\
  verify error:num=20:unable to get local issuer certificate\
  verify return:1\
  depth=0 OU = Domain Control Validated, CN = \*.example.com\
  verify error:num=21:unable to verify the first certificate\
  verify return:1\
  \-\-\-\
  Certificate chain\
   0 s:/OU=Domain Control Validated/CN=*.example.com\
     i:/C=US/ST=Arizona/L=Scottsdale/O=GoDaddy.com, Inc./OU=http://certs.godaddy.com/repository//CN=Go Daddy Secure Certificate Authority - G2\
  \-----BEGIN CERTIFICATE-----\
  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\
  \-----END CERTIFICATE-----
```

#### <a name="backend-certificate-invalid-common-name-cn"></a>Неправильное общее имя сертификата внутреннего сервера (CN)

**Сообщение:** Общее имя (CN) внутреннего сертификата не соответствует заголовку узла зонда.

**Причина:** Шлюз приложений проверяет, соответствует ли имя узла, указанное в параметрах HTTP серверной части, значению CN, представленному в сертификате TLS/SSL внутреннего сервера. Это Standard_v2 и WAF_v2 поведения SKU. Указание имени сервера номера SKU Standard и WAF (SNI) задаются в качестве полного доменного имени в адресе внутреннего пула.

Если в версии v2 имеется проверка по умолчанию (не настроена и не сопоставлена пользовательская пробная версия), SNI будет установлен из имени узла, указанного в параметрах HTTP. Или, если параметр "выбрать имя узла из серверного адреса" упоминается в параметрах HTTP, в которых серверный пул адресов содержит допустимое полное доменное имя, то применяется эта настройка.

При наличии пользовательской проверки, связанной с параметрами HTTP, SNI будет установлен из имени узла, указанного в пользовательской конфигурации зонда. Или, если в пользовательской зонде выбран **параметр выбрать имя узла из серверных параметров HTTP** , SNI будет установлен из имени узла, указанного в параметрах HTTP.

Если параметр **выбрать имя узла из серверного адреса** задан в настройках HTTP, серверный пул адресов должен содержать допустимое полное доменное имя.

Если получено это сообщение об ошибке, то CN внутреннего сертификата не соответствует имени узла, настроенному в пользовательской проверке, или параметрах HTTP (если выбран параметр **выбрать имя узла из серверной части HTTP** ). Если вы используете проверку по умолчанию, имя узла будет иметь значение **127.0.0.1**. Если это значение не требуется, создайте пользовательскую пробу и свяжите его с параметрами HTTP.

**Решение**.

Устранить проблему можно так.

Для Windows:

1.  Войдите на компьютер, где размещено ваше приложение.

1.  Выберите Win + R или щелкните правой кнопкой мыши кнопку **Пуск** и выберите команду **выполнить**.

1.  Введите **CertMgr. msc** и нажмите клавишу ВВОД. Также можно выполнить поиск диспетчера сертификатов в меню " **Пуск** ".

1.  Выберите сертификат (обычно в `\Certificates - Current User\\Personal\\Certificates`) и откройте сертификат.

1.  На вкладке **сведения** проверьте **субъект**сертификата.

1.  Проверьте общее имя сертификата из сведений и введите его в поле имени узла пользовательской пробы или в параметрах HTTP (если выбран параметр **выбрать имя узла из серверной части HTTP** ). Если это не требуемое имя узла для веб-сайта, необходимо получить сертификат для этого домена или ввести правильное имя узла в конфигурации пользовательской пробы или настройки HTTP.

Для Linux с использованием OpenSSL:

1.  Выполните следующую команду в OpenSSL:
    ```
    openssl x509 -in certificate.crt -text -noout
    ```

2.  В отображаемых свойствах найдите общее имя сертификата и введите его в поле Host Name (название узла) в параметрах HTTP. Если это не требуемое имя узла для веб-сайта, необходимо получить сертификат для этого домена или ввести правильное имя узла в конфигурации пользовательской пробы или настройки HTTP.

#### <a name="backend-certificate-is-invalid"></a>Недопустимый внутренний сертификат

**Сообщение:** Недопустимый внутренний сертификат. \"Текущая дата находится за пределами допустимого диапазона\" \"\" дат и срока действия сертификата.

**Причина:** Каждый сертификат поставляется с диапазоном допустимых значений, и HTTPS-подключение не будет безопасным, если только сертификат TLS или SSL сервера не действителен. Текущие данные должны находиться в пределах **допустимого** диапазона от **до допустимого** значения. Если это не так, сертификат считается недопустимым и создает ошибку безопасности, в которой шлюз приложений помечает внутренний сервер как неработоспособный.

**Решение:** Если срок действия сертификата TLS/SSL истек, продлите сертификат у поставщика и обновите параметры сервера с помощью нового сертификата. Если это самозаверяющий сертификат, необходимо создать действительный сертификат и передать корневой сертификат в параметры HTTP шлюза приложений. Для этого выполните следующие действия:

1.  Откройте параметры HTTP шлюза приложений на портале.

1.  Выберите параметр с истекшим сроком действия сертификат, нажмите кнопку **Добавить сертификат**и откройте новый файл сертификата.

1.  Удалите старый сертификат с помощью значка **удаления** рядом с сертификатом, а затем нажмите кнопку **сохранить**.

#### <a name="certificate-verification-failed"></a>Проверка сертификата не пройдена

**Сообщение:** Не удалось проверить допустимость серверного сертификата. Чтобы узнать причину, проверьте сведения о диагностике OpenSSL для сообщения, связанного с кодом ошибки {errorCode}

**Причина:** Эта ошибка возникает, когда шлюз приложений не может проверить допустимость сертификата.

**Решение:** Чтобы устранить эту проблему, убедитесь, что сертификат на сервере создан правильно. Например, с помощью [OpenSSL](https://www.openssl.org/docs/man1.0.2/man1/verify.html) можно проверить сертификат и его свойства, а затем попытаться повторно отправить сертификат в параметры HTTP шлюза приложений.

<a name="backend-health-status-unknown"></a>Состояние работоспособности серверной части: неизвестно
-------------------------------
Если работоспособность серверной части отображается как неизвестная, представление портала будет выглядеть следующим образом:

![Работоспособность серверной части шлюза приложений — неизвестно](./media/application-gateway-backend-health-troubleshooting/appgwunknown.png)

Такое поведение может возникать по одной или нескольким из следующих причин.

1.  NSG в подсети шлюза приложений блокирует входящий доступ к портам 65503-65534 (номер SKU v1) или 65200-65535 (SKU v2) из Интернета.
1.  Для UDR в подсети шлюза приложений задан маршрут по умолчанию (0.0.0.0/0), а следующий прыжок не указан как "Интернет".
1.  Маршрут по умолчанию объявляется подключением ExpressRoute или VPN к виртуальной сети по протоколу BGP.
1.  Пользовательский DNS-сервер настраивается в виртуальной сети, которая не может разрешать общедоступные доменные имена.
1.  Шлюз приложений находится в неработоспособном состоянии.

**Решение**.

1.  Проверьте, не блокирует ли NSG доступ к портам 65503-65534 (номер SKU v1) или 65200-65535 (v2 SKU) из **Интернета**:

    a.  На вкладке **Обзор** шлюза приложений выберите ссылку **Виртуальная сеть или подсеть** .

    b.  На вкладке **подсети** виртуальной сети выберите подсеть, в которой развернут шлюз приложений.

    c.  Проверьте, настроены ли какие-либо NSG.

    d.  Если NSG настроен, найдите этот ресурс NSG на вкладке **Поиск** или в разделе **все ресурсы**.

    д)  В разделе **правила для входящих** подключений добавьте правило входящего трафика, чтобы разрешить диапазон портов назначения 65503-65534 для SKU v1 или SKU 65200-65535 v2 с **исходным** набором или **Интернетом**. **Any**

    е)  Выберите **сохранить** и убедитесь, что серверная часть находится в работоспособном состоянии. Кроме того, это можно сделать с помощью [PowerShell или CLI](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

1.  Проверьте, имеет ли UDR маршрут по умолчанию (0.0.0.0/0) со следующим прыжком, не равным **Internet**:
    
    a.  Выполните шаги 1a и 1B, чтобы определить подсеть.

    b.  Проверьте, настроены ли UDR. Если это так, выполните поиск ресурса на панели поиска или в разделе **все ресурсы**.

    c.  Проверьте наличие маршрутов по умолчанию (0.0.0.0/0) со следующим прыжком, не равным **Internet**. Если параметр имеет значение **виртуальное устройство** или **шлюз виртуальной сети**, необходимо убедиться, что виртуальное устройство или локальное устройство могут правильно маршрутизировать пакет в Интернет-адрес, не изменяя пакет.

    d.  В противном случае измените следующий прыжок на **Интернет**, выберите **сохранить**и проверьте работоспособность серверной части.

1.  Маршрут по умолчанию, объявленный с помощью подключения ExpressRoute или VPN к виртуальной сети по протоколу BGP:

    a.  При наличии подключения ExpressRoute или VPN к виртуальной сети по протоколу BGP и при объявлении маршрута по умолчанию необходимо убедиться, что пакет перенаправляется обратно в Интернет, не изменяя его. Проверить можно с помощью параметра **Устранение неполадок подключения** на портале шлюза приложений.

    b.  Выберите назначение вручную, как любой IP-адрес, поддерживающий маршрутизацию в Интернете, например 1.1.1.1. Задайте порт назначения как угодно и проверьте подключение.

    c.  Если следующий прыжок является шлюзом виртуальной сети, то может существовать маршрут по умолчанию, объявленный через ExpressRoute или VPN.

1.  Если в виртуальной сети настроен настраиваемый DNS-сервер, убедитесь, что сервер (или серверы) может разрешать общедоступные домены. Разрешение общедоступного доменного имени может потребоваться в сценариях, где шлюз приложений должен быть доступен внешним доменам, например серверам OCSP, или проверять состояние отзыва сертификата.

1.  Чтобы убедиться, что шлюз приложений работоспособен и работает, перейдите к параметру **работоспособность ресурсов** на портале и убедитесь, что состояние **работоспособно**. Если вы видите **неработоспособное** или **пониженное** состояние, обратитесь в [службу поддержки](https://azure.microsoft.com/support/options/).

<a name="next-steps"></a>Дальнейшие шаги
----------

Дополнительные сведения о [диагностике и ведении журнала шлюза приложений](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
