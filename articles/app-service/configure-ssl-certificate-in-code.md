---
title: Использование сертификата TLS/SSL в коде
description: Узнайте, как использовать сертификаты клиента в коде. Проверяйте подлинность с помощью удаленных ресурсов с помощью сертификата клиента или выполните с ними задачи шифрования.
ms.topic: article
ms.date: 11/04/2019
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: d76bac60bae11f0843d81de523030154af62a373
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80811696"
---
# <a name="use-a-tlsssl-certificate-in-your-code-in-azure-app-service"></a>Использование TLS/SSL-сертификата в коде в Службе приложений Azure

В коде приложения можно получить доступ к [общедоступным или частным сертификатам, добавляемым в службу приложений](configure-ssl-certificate.md). Код приложения может действовать как клиент и обращаться к внешней службе, для которой требуется проверка подлинности сертификата, или же может потребоваться выполнить криптографические задачи. В этом пошаговом руководство показано, как использовать общедоступные или частные сертификаты в коде приложения.

Этот подход к использованию сертификатов в коде использует функции TLS в службе приложений, которая требует, чтобы ваше приложение было на уровне **Basic** или выше. Если приложение находится на уровне **Free** или **Shared** , можно [включить файл сертификата в репозиторий приложения](#load-certificate-from-file).

Если служба приложений позволяет управлять сертификатами TLS/SSL, вы можете поддерживать сертификаты и код приложения отдельно и защищать конфиденциальные данные.

## <a name="prerequisites"></a>Предварительные требования

Ознакомьтесь со следующими статьями:

- [Создано приложение службы приложений](/azure/app-service/).
- [Добавление сертификата в приложение](configure-ssl-certificate.md)

## <a name="find-the-thumbprint"></a>Найти отпечаток

На <a href="https://portal.azure.com" target="_blank">портале Azure</a> в меню слева выберите **Службы приложений** >  **\<имя_приложения>** .

В левой области навигации приложения выберите **Параметры TLS/SSL**, а затем выберите **Сертификаты закрытого ключа (PFX)** или **Сертификаты открытого ключа (CER)**.

Найдите сертификат, который вы хотите использовать, и скопируйте отпечаток.

![Копирование отпечатка сертификата](./media/configure-ssl-certificate/create-free-cert-finished.png)

## <a name="make-the-certificate-accessible"></a>Предоставление доступа к сертификату

Чтобы получить доступ к сертификату в коде приложения, добавьте его отпечаток в параметр `WEBSITE_LOAD_CERTIFICATES` приложения, выполнив следующую команду в <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Чтобы сделать все сертификаты доступными, задайте значение `*`.

## <a name="load-certificate-in-windows-apps"></a>Загрузка сертификата в приложениях Windows

Параметр `WEBSITE_LOAD_CERTIFICATES` приложения делает указанные сертификаты доступными для размещенного в Windows приложения в хранилище сертификатов Windows, и расположение зависит от [ценовой](overview-hosting-plans.md)категории:

- **Изолированный** уровень в [локальной мачине\ми](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores). 
- Все остальные уровни — в [текущем усер\ми](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores).

В коде C# вы обращаетесь к сертификату по отпечатку сертификата. Следующий код загружает сертификат с отпечатком `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Linq;
using System.Security.Cryptography.X509Certificates;

string certThumbprint = "E661583E8FABEF4C0BEF694CBC41C28FB81CD870";
bool validOnly = false;

using (X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser))
{
  certStore.Open(OpenFlags.ReadOnly);

  X509Certificate2Collection certCollection = certStore.Certificates.Find(
                              X509FindType.FindByThumbprint,
                              // Replace below with your certificate's thumbprint
                              certThumbprint,
                              validOnly);
  // Get the first cert with the thumbprint
  X509Certificate2 cert = certCollection.OfType<X509Certificate>().FirstOrDefault();

  if (cert is null)
      throw new Exception($"Certificate with thumbprint {certThumbprint} was not found");

  // Use certificate
  Console.WriteLine(cert.FriendlyName);
  
  // Consider to call Dispose() on the certificate after it's being used, avaliable in .NET 4.6 and later
}
```

В коде Java вы получите доступ к сертификату из хранилища Windows-MY, используя поле общего имени субъекта (см. раздел [сертификат открытого ключа](https://en.wikipedia.org/wiki/Public_key_certificate)). В следующем коде показано, как загрузить сертификат закрытого ключа:

```java
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import java.security.KeyStore;
import java.security.cert.Certificate;
import java.security.PrivateKey;

...
KeyStore ks = KeyStore.getInstance("Windows-MY");
ks.load(null, null); 
Certificate cert = ks.getCertificate("<subject-cn>");
PrivateKey privKey = (PrivateKey) ks.getKey("<subject-cn>", ("<password>").toCharArray());

// Use the certificate and key
...
```

Дополнительные сведения о языках, которые не поддерживают или предлагают недостаточную поддержку хранилища сертификатов Windows, см. [в разделе Загрузка сертификата из файла](#load-certificate-from-file).

## <a name="load-certificate-in-linux-apps"></a>Загрузка сертификата в приложениях Linux

Параметры `WEBSITE_LOAD_CERTIFICATES` приложения делают указанные сертификаты доступными для размещенных в Linux приложений (включая пользовательские приложения-контейнеры) в виде файлов. Файлы находятся в следующих каталогах:

- Частные сертификаты `/var/ssl/private` ( `.p12` файлы)
- Общедоступные `/var/ssl/certs` сертификаты `.der` — (файлы)

Имена файлов сертификатов — это отпечатки сертификата. В следующем коде C# показано, как загрузить открытый сертификат в приложение Linux.

```csharp
using System;
using System.IO;
using System.Security.Cryptography.X509Certificates;

...
var bytes = File.ReadAllBytes("/var/ssl/certs/<thumbprint>.der");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Чтобы узнать, как загрузить сертификат TLS/SSL из файла в Node. js, PHP, Python, Java или Ruby, см. документацию по соответствующему языку или веб-платформе.

## <a name="load-certificate-from-file"></a>Загрузить сертификат из файла

Если вам нужно загрузить файл сертификата, который вы перегрузили вручную, лучше передать сертификат с помощью [FTPS](deploy-ftp.md) вместо [Git](deploy-local-git.md), например. Конфиденциальные данные следует учитывать как закрытый сертификат из системы управления версиями.

> [!NOTE]
> ASP.NET и ASP.NET Core в Windows должны получить доступ к хранилищу сертификатов, даже если вы загрузили сертификат из файла. Чтобы загрузить файл сертификата в приложение Windows .NET, загрузите профиль текущего пользователя с помощью следующей команды в <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:
>
> ```azurecli-interactive
> az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
> ```
>
> Этот подход к использованию сертификатов в коде использует функции TLS в службе приложений, которая требует, чтобы ваше приложение было на уровне **Basic** или выше.

Следующий пример на C# загружает открытый сертификат из относительного пути в приложении:

```csharp
using System;
using System.IO;
using System.Security.Cryptography.X509Certificates;

...
var bytes = File.ReadAllBytes("~/<relative-path-to-cert-file>");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Чтобы узнать, как загрузить сертификат TLS/SSL из файла в Node. js, PHP, Python, Java или Ruby, см. документацию по соответствующему языку или веб-платформе.

## <a name="more-resources"></a>Дополнительные ресурсы

* [Защита пользовательского доменного имени с помощью привязки TLS/SSL в Службе приложений Azure](configure-ssl-bindings.md)
* [Принудительное использование HTTPS](configure-ssl-bindings.md#enforce-https)
* [Принудительное применение TLS 1.1/1.2](configure-ssl-bindings.md#enforce-tls-versions)
* [FAQ: Configuration and management FAQs for Web Apps in Azure](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/) (Часто задаваемые вопросы о настройке и управлении для функции "Веб-приложения" в Azure)
