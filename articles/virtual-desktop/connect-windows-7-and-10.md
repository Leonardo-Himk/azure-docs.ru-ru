---
title: Подключение к виртуальному рабочему столу Windows 10 или 7 в Azure
description: Как подключиться к виртуальному рабочему столу Windows с помощью настольного клиента Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2b16818856ca8196b82eb8f618cf22b5fc1b6854
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612696"
---
# <a name="connect-with-the-windows-desktop-client"></a>Подключение к клиенту Windows Desktop

> Область применения: Windows 7, Windows 10 и Windows 10 IoT Корпоративная

Вы можете получить доступ к ресурсам виртуальных рабочих столов Windows на устройствах с Windows 7, Windows 10 и Windows 10 IoT Корпоративная с помощью настольного клиента Windows.

>[!NOTE]
>Клиент Windows по умолчанию будет выдавать значение 2019 для виртуальных рабочих столов Windows. Однако если клиент обнаруживает, что у пользователя также есть Azure Resource Manager ресурсы, он автоматически добавляет ресурсы или уведомляет пользователя о том, что они доступны. 

> [!IMPORTANT]
> Виртуальный рабочий стол Windows не поддерживает клиент Подключения к удаленным рабочим столам и приложениям RemoteApp (RADC) или клиент Подключения к удаленному рабочему столу (MSTSC).

> [!IMPORTANT]
> Виртуальный рабочий стол Windows в настоящее время не поддерживает клиент Удаленного рабочего стола из Магазина Windows. Поддержка этого клиента будет добавлена в следующем выпуске.

## <a name="install-the-windows-desktop-client"></a>Установка настольного клиента Windows

Выберите клиент, который соответствует вашей версии Windows:

- [Windows (64-разрядная версия)](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows (32-разрядная версия)](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows для ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Можно установить клиент для текущего пользователя, что не требует прав администратора. Кроме того, администратор может установить и настроить клиент, чтобы все пользователи устройства могли получить к нему доступ.

После установки клиент можно будет запустить из меню "Пуск", выполнив поиск фразы **Удаленный рабочий стол**.

## <a name="subscribe-to-a-feed"></a>Подписка на веб-канал

Получите список управляемых ресурсов, доступных для вас, подписавшись на веб-канал, предоставленный администратором. Подписка делает ресурсы доступными на локальном ПК.

Для подписки на веб-канал:

1. Откройте классическое клиент Windows.
2. На главной странице выберите **Подпишитесь** , чтобы подключиться к службе и получить ресурсы.
3. При появлении запроса войдите в систему со своей учетной записью.

После успешного входа вы увидите список ресурсов, к которым у вас есть доступ.

Ресурсы можно запускать одним из двух способов.

- На главной странице клиента дважды щелкните ресурс, чтобы запустить его.
- Запустите ресурс, как обычно другие приложения из меню "Пуск".
  - Вы также можете искать приложения на панели поиска.

После подписки на веб-канал содержимое веб-канала автоматически обновляется на регулярной основе. Ресурсы можно добавлять, изменять или удалять в зависимости от изменений, внесенных администратором.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об использовании настольного клиента Windows см. в статье Начало [работы с настольным клиентом Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
