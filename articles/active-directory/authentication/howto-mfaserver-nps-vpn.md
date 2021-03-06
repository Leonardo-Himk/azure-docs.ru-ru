---
title: Сервер Azure MFA и VPN сторонних производителей Azure Active Directory
description: Пошаговые руководства по настройке сервера Azure MFA для интеграции с Cisco, Citrix и Juniper.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01decb99a9eb24ae60250f83f1f961b4c1690bc0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80652850"
---
# <a name="advanced-scenarios-with-azure-mfa-server-and-third-party-vpn-solutions"></a>Расширенные сценарии с сервером Azure MFA и VPN-решениями сторонних производителей

Сервер Многофакторной идентификации Azure (сервер Azure MFA) можно использовать для беспроблемного подключения к различным сторонним решениям VPN. Эта статья посвящена&reg; VPN-устройству Cisco ASA, VPN-устройству Citrix NetScaler SSL и Juniper Networks Secure Access/Pulse Secure Connect Secure SSL VPN-устройство. Мы разработали руководства по настройке для этих трех распространенных устройств. Сервер Azure MFA также может интегрироваться с большинством других систем, использующих RADIUS, LDAP, IIS или проверку подлинности на основе утверждений, для AD FS. Дополнительные сведения см. в [настройках сервера Azure MFA](howto-mfaserver-deploy.md#next-steps).

> [!IMPORTANT]
> По состоянию на 1 июля 2019 Корпорация Майкрософт больше не будет предлагать сервер MFA для новых развертываний. Новые клиенты, желающие требовать многофакторную проверку подлинности пользователей, должны использовать службу многофакторной идентификации Azure на основе облачных служб. Существующие клиенты, которые активировали сервер MFA до 1 июля, смогут скачать последнюю версию, будущие обновления и создать учетные данные активации обычным образом.

## <a name="cisco-asa-vpn-appliance-and-azure-mfa-server"></a>VPN-устройство Cisco ASA и сервер Azure MFA
Сервер Azure MFA интегрируется с VPN-&reg; устройством Cisco ASA, чтобы обеспечить дополнительную безопасность для входа&reg; в VPN Cisco AnyConnect и доступа к порталу.  Вы можете использовать протокол LDAP или RADIUS.  Щелкните одну из следующих ссылок и загрузите подробную пошаговую инструкцию по настройке.

| Руководство по настройке | Описание |
| --- | --- |
| [Настройка Cisco ASA с конфигурацией Anyconnect VPN и многофакторной проверки подлинности Azure для LDAP](https://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Интеграция устройства VPN Cisco ASA с Azure MFA с помощью LDAP |
| [Настройка Cisco ASA с конфигурацией Anyconnect VPN и многофакторной проверки подлинности Azure для RADIUS](https://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Интеграция устройства VPN Cisco ASA с Azure MFA с помощью RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-mfa-server"></a>Citrix NetScaler SSL VPN и сервер Azure MFA
Сервер Azure MFA интегрируется с VPN-устройством Citrix NetScaler SSL для обеспечения дополнительной безопасности для имен входа VPN Citrix NetScaler SSL и доступа к порталу.  Вы можете использовать протокол LDAP или RADIUS.  Щелкните одну из следующих ссылок и загрузите подробную пошаговую инструкцию по настройке.

| Руководство по настройке | Описание |
| --- | --- |
| [Настройка VPN Citrix NetScaler SSL и многофакторной проверки подлинности Azure для LDAP](https://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Интеграция устройства VPN Citrix NetScaler SSL с Azure MFA с помощью LDAP |
| [Настройка VPN Citrix NetScaler SSL и многофакторной проверки подлинности Azure для RADIUS](https://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Интеграция устройства VPN Citrix NetScaler SSL с Azure MFA с помощью RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-mfa-server"></a>Juniper/Pulse Secure SSL VPN-устройство и сервер Azure MFA
Сервер Azure MFA интегрируется с VPN-устройством Juniper/Pulse Secure SSL для обеспечения дополнительной безопасности для учетных записей VPN Juniper/Pulse Secure SSL и доступа к порталу.  Вы можете использовать протокол LDAP или RADIUS.  Щелкните одну из следующих ссылок и загрузите подробную пошаговую инструкцию по настройке.

| Руководство по настройке | Описание |
| --- | --- |
| [Настройка VPN Juniper/Pulse Secure SSL и многофакторной проверки подлинности Azure для LDAP](https://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Интеграция устройства VPN Juniper/Pulse Secure SSL с Azure MFA с помощью LDAP |
| [Настройка VPN Juniper/Pulse Secure SSL и многофакторной проверки подлинности Azure для RADIUS](https://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Интеграция устройства VPN Juniper/Pulse Secure SSL с Azure MFA с помощью RADIUS |

## <a name="next-steps"></a>Дальнейшие шаги

- [Внедрение расширения NPS для Многофакторной идентификации Azure в существующую инфраструктуру проверки подлинности.](howto-mfa-nps-extension.md)

- [Настройка параметров многофакторной идентификации Azure](howto-mfa-mfasettings.md)
