---
title: Настройка постоянного туннеля VPN-пользователя
titleSuffix: Azure VPN Gateway
description: В этой статье описывается, как настроить туннель VPN пользователя Always On для VPN-шлюза.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 56934dd13661d8f623e673e2817e87618675c7ed
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79502274"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Настройка туннеля Always On VPN для пользователя

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Настройка шлюза

 Используйте инструкции из статьи [Настройка VPN-подключения типа "точка — сеть](vpn-gateway-howto-point-to-site-resource-manager-portal.md) ", чтобы настроить VPN-шлюз для использования проверки подлинности IKEv2 и на основе сертификата.

## <a name="configure-a-user-tunnel"></a>Настройка пользовательского туннеля

[!INCLUDE [user configuration](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Удаление профиля

Чтобы удалить профиль, выполните следующие действия.

1. Выполните следующую команду:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Отключите подключение и снимите флажок **подключиться автоматически** .

   ![Очистка](./media/vpn-gateway-howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Дальнейшие шаги

Сведения об устранении проблем с подключением, которые могут возникнуть, см. в статье [проблемы с подключением "точка — сеть](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)" в Azure.
