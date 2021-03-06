---
title: Необходимые условия для Azure AD Connect подготовки облака в Azure AD
description: В этой статье описываются необходимые условия и требования к оборудованию, необходимые для подготовки облачных сред.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 553ecc971235b5ba7d55a2dcb6963200919a3480
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82853456"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Предварительные требования для подготовки облачных Azure AD Connect
В этой статье приводятся рекомендации по выбору и использованию Azure Active Directory (Azure AD) для подключения облака в качестве решения для идентификации.



## <a name="cloud-provisioning-agent-requirements"></a>Требования к агенту подготовки облака
Для использования Azure AD Connect подготовки облака необходимо следующее:
    
- Учетная запись глобального администратора для клиента Azure AD, который не является гостевым пользователем.
- Локальный сервер для агента подготовки с Windows 2012 R2 или более поздней версии.
- Локальные конфигурации брандмауэра.

>[!NOTE]
>В настоящее время агент подготовки можно установить только на англоязычных языковых серверах. Установка английского языкового пакета на сервере, отличном от английского, не является допустимым решением и приведет к сбою установки агента. 

В оставшейся части документа приводятся пошаговые инструкции по выполнению этих предварительных требований.

### <a name="in-the-azure-active-directory-admin-center"></a>В центре администрирования Azure Active Directory

1. Создайте облачную учетную запись глобального администратора в клиенте Azure AD. Таким образом вы можете управлять конфигурацией клиента, если локальные службы завершаются сбоем или становятся недоступными. Узнайте [, как добавить облачную учетную запись глобального администратора](../active-directory-users-create-azure-portal.md). Завершение этого шага важно для того, чтобы не блокировать клиент.
1. Добавьте одно [имя личного домена](../active-directory-domains-add-azure-portal.md) (или несколько) в клиент Azure AD. Пользователи могут выполнить вход с помощью одного из этих доменных имен.

### <a name="in-your-directory-in-active-directory"></a>В каталоге в Active Directory

Запустите [средство идфикс](https://docs.microsoft.com/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) , чтобы подготовить атрибуты каталога для синхронизации.

### <a name="in-your-on-premises-environment"></a>В локальной среде

1. Выявление присоединенного к домену сервера узла под управлением Windows Server 2012 R2 или более поздней версии с 4 ГБ ОЗУ и .NET 4.7.1 + Runtime.

1. Политика выполнения PowerShell на локальном сервере должна быть установлена в значение undefine или RemoteSigned.

1. Если между вашими серверами и Azure AD есть брандмауэр, настройте следующие параметры:
   - Убедитесь, что агенты могут передавать *исходящие* запросы в Azure AD через приведенные ниже порты.

        | Номер порта | Как он используется |
        | --- | --- |
        | **80** | Скачивает списки отзыва сертификатов (CRL) при проверке сертификата TLS/SSL.  |
        | **443** | Обрабатывает все исходящие связи со службой. |
        | **8080** (необязательно) | Агенты передают данные о своем состоянии каждые 10 минут через порт 8080, если порт 443 недоступен. Это состояние отображается на портале Azure AD. |
     
   - Если брандмауэр применяет правила в соответствии с отправляющими трафик пользователями, откройте эти порты для трафика, поступающего от служб Windows, которые работают как сетевая служба.
   - Если брандмауэр или прокси-сервер позволяет указать надежные суффиксы, добавьте подключения к \*. msappproxy.NET и \*. servicebus.Windows.NET. Если нет, разрешите доступ к [диапазонам IP-адресов центра обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653). Список диапазонов IP-адресов обновляется еженедельно.
   - Агентам требуется доступ к адресам login.windows.net и login.microsoftonline.com для первоначальной регистрации. Откройте эти URL-адреса в брандмауэре.
   - Для проверки сертификата Разблокируйте следующие URL-адреса: mscrl.microsoft.com:80, crl.microsoft.com:80, ocsp.msocsp.com:80 и\.www Microsoft.com:80. Эти URL-адреса используются для проверки сертификата с другими продуктами Майкрософт, поэтому эти URL-адреса могут быть уже заблокированы.

### <a name="verify-the-port"></a>Проверка порта
Чтобы убедиться, что Azure прослушивает порт 443 и что агент может взаимодействовать с ним, используйте следующий URL-адрес:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Этот тест проверяет, могут ли агенты обмениваться данными с Azure через порт 443. Откройте браузер и перейдите к предыдущему URL-адресу с сервера, на котором установлен агент.

![Проверка достижимости порта](media/how-to-install/verify2.png)

### <a name="additional-requirements"></a>Дополнительные требования
- [Платформа Microsoft .NET 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>Требования к TLS

>[!NOTE]
>TLS — это протокол, обеспечивающий безопасность обмена данными. Изменение параметров TLS влияет на весь лес. Дополнительные сведения см. в разделе [Обновление для включения tls 1,1 и tls 1,2 в качестве защищенных протоколов по умолчанию в WinHTTP в Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

Перед установкой на сервере Windows, на котором размещен агент подготовки Azure AD Connect Cloud, должен быть включен протокол TLS 1,2.

Чтобы включить TLS 1,2, выполните следующие действия.

1. Настройте следующие разделы реестра:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Перезапустите сервер.


## <a name="next-steps"></a>Дальнейшие действия 

- [Что собой представляет подготовка?](what-is-provisioning.md)
- [Что такое облачная подготовка Azure AD Connect?](what-is-cloud-provisioning.md)

