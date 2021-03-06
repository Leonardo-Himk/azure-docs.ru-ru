---
title: 'Виртуальная глобальная сеть: клиент Azure AD для разных групп пользователей: проверка подлинности Azure AD'
description: VPN-подключение P2S можно использовать для подключения к виртуальной сети с помощью проверки подлинности Azure AD.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: alzam
ms.openlocfilehash: af5ff5817ee9ae7e6d7432fe281ecb440bf25b9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80060716"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>Создание клиента Azure Active Directory для подключений по протоколу P2S Опенвпн

При подключении к виртуальной сети можно использовать проверку подлинности на основе сертификатов или аутентификацию RADIUS. Однако при использовании открытого протокола VPN можно также использовать проверку подлинности Azure Active Directory. Если вы хотите, чтобы другой набор пользователей мог подключаться к разным шлюзам, можно зарегистрировать несколько приложений в AD и связать их с разными шлюзами.

Эта статья поможет вам настроить клиент Azure AD для проверки подлинности P2S Опенвпн, а также создать и зарегистрировать несколько приложений в Azure AD, чтобы разрешить разные права доступа для разных пользователей и групп.

> [!NOTE]
> Аутентификация Azure AD поддерживается только для подключений&reg; по протоколу опенвпн.
>

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="6-create-a-new-p2s-configuration"></a><a name="site"></a>6. Создание новой конфигурации P2S

Конфигурации подключения "точка — сеть" определяет параметры для подключения удаленных клиентов.

1. Установите следующие переменные, заменив по мере необходимости значения в вашей среде.

   ```azurepowershell-interactive
   $aadAudience = "00000000-abcd-abcd-abcd-999999999999"
   $aadIssuer = "https://sts.windows.net/00000000-abcd-abcd-abcd-999999999999/"
   $aadTenant = "https://login.microsoftonline.com/00000000-abcd-abcd-abcd-999999999999"    
   ```

2. Чтобы создать конфигурацию, выполните следующие команды:

   ```azurepowershell-interactive
   $aadConfig = New-AzVpnServerConfiguration -ResourceGroupName <ResourceGroup> -Name newAADConfig -VpnProtocol OpenVPN -VpnAuthenticationType AAD -AadTenant $aadTenant -AadIssuer $aadIssuer -AadAudience $aadAudience -Location westcentralus
   ```

   > [!NOTE]
   > Не используйте идентификатор приложения клиента VPN Azure в приведенных выше командах. он предоставит всем пользователям доступ к шлюзу. Используйте идентификатор зарегистрированного приложения.

## <a name="7-edit-hub-assignment"></a><a name="hub"></a>7. изменение назначения концентратора

1. Перейдите в колонку **концентраторы** в виртуальной глобальной сети.

2. Выберите концентратор, с которым вы хотите связать конфигурацию vpn-сервера, и нажмите кнопку с многоточием (...).

    ![новый сайт](media/openvpn-azure-ad-tenant-multi-app/p2s4.jpg)

3. Нажмите **Изменение виртуального концентратора**.

4. Установите флажок **Включить шлюз "точка — сеть"** и выберите нужную **единицу масштабирования шлюза**.

    ![новый сайт](media/openvpn-azure-ad-tenant-multi-app/p2s2.jpg)

5. Введите **пул адресов**, с которого VPN клиентам будут назначаться IP адреса.

6. Нажмите кнопку **подтвердить**.

7. Выполнение операции может занять до 30 минут.

## <a name="8-download-vpn-profile"></a><a name="device"></a>8. скачивание профиля VPN

Используйте профиль VPN для настройки клиентов.

1. На странице Виртуальной глобальной сети щелкните **Конфигурация VPN пользователя**.

2. В верхней части страницы щелкните **Загрузить конфигурацию пользователя VPN**.

3. После завершения создания файла вы можете скачать его, щелкнув ссылку.

4. Используйте файл профиля для настройки VPN-клиента.

5. Извлеките скачанный ZIP-файл.

6. Перейдите к распакованной папке "AzureVPN".

7. Запишите расположение файла "азуревпнконфиг. XML". Азуревпнконфиг. XML содержит параметр для VPN-подключения и может быть импортирован непосредственно в клиентское приложение VPN Azure. Этот файл также можно передать всем пользователям, которым требуется подключение по электронной почте или другим средствам. Для успешного подключения пользователю понадобятся действительные учетные данные Azure AD.

## <a name="9-configure-user-vpn-clients"></a>9. Настройка VPN-клиентов пользователей

Для подключения необходимо скачать VPN-клиент Azure и импортировать профиль VPN-клиента, скачанный на предыдущих шагах, на каждый компьютер, который нужно подключить к виртуальной сети.

> [!NOTE]
> Аутентификация Azure AD поддерживается только для подключений&reg; по протоколу опенвпн.
>

#### <a name="to-download-the-azure-vpn-client"></a>Загрузка VPN-клиента Azure

Используйте эту [ссылку](https://go.microsoft.com/fwlink/?linkid=2117554), чтобы скачать VPN-клиент Azure.

#### <a name="to-import-a-client-profile"></a><a name="import"></a>Импорт профиля клиента

1. На странице выберите **Import** (Импорт).

    ![импорт](./media/openvpn-azure-ad-tenant-multi-app/import/import1.jpg)

2. Перейдите к XML-файлу профиля и выберите его. Выбрав файл, выберите **Open** (Открыть).

    ![импорт](./media/openvpn-azure-ad-tenant-multi-app/import/import2.jpg)

3. Укажите имя профиля и выберите **Save** (Сохранить).

    ![импорт](./media/openvpn-azure-ad-tenant-multi-app/import/import3.jpg)

4. Выберите **Connect** (Подключиться), чтобы подключиться к VPN.

    ![импорт](./media/openvpn-azure-ad-tenant-multi-app/import/import4.jpg)

5. После подключения значок станет зеленым и выдаст **Connected** (Подключено).

    ![импорт](./media/openvpn-azure-ad-tenant-multi-app/import/import5.jpg)

#### <a name="to-delete-a-client-profile"></a><a name="delete"></a>Удаление профиля клиента

1. Нажмите кнопку с многоточием (...) рядом с удаляемым профилем клиента. Затем щелкните **Remove** (Удалить).

    !["Удалить"](./media/openvpn-azure-ad-tenant-multi-app/delete/delete1.jpg)

2. Выберите **Remove** (Удалить), чтобы выполнить удаление.

    !["Удалить"](./media/openvpn-azure-ad-tenant-multi-app/delete/delete2.jpg)

#### <a name="to-diagnose-connection-issues"></a><a name="diagnose"></a>Диагностика проблем с подключением

1. Для диагностики проблем с подключением можно использовать средство **Diagnose** (Диагностика). Нажмите кнопку с многоточием (...) рядом с VPN-подключением, которое нужно диагностировать, чтобы открыть меню. Затем выберите **Diagnose** (Диагностика).

    ![diagnose](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose1.jpg)

2. На странице **Connection Properties** (Свойства подключения) выберите **Run Diagnosis** (Выполнить диагностику).

    ![diagnose](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose2.jpg)

3. Войдите с помощью своих учетных данных.

    ![diagnose](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose3.jpg)

4. Просмотр результатов диагностики.

    ![diagnose](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose4.jpg)

## <a name="10-view-your-virtual-wan"></a><a name="viewwan"></a>10. Просмотр виртуальной глобальной сети

1. Перейдите к виртуальной глобальной сети.

2. На странице обзора каждая точка на карте представляет собой концентратор.

3. В разделе концентраторов и подключений можно просмотреть сведения о состоянии концентратора, сайте, регионе, состоянии VPN-подключения, приеме и передаче байтов.

## <a name="clean-up-resources"></a><a name="cleanup"></a>Очистка ресурсов

Вы можете удалить ненужную группу ресурсов и все содержащиеся в ней ресурсы с помощью командлета [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). Замените myResourceGroup на имя вашей группы ресурсов и выполните следующую команду PowerShell:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Виртуальной глобальной сети см. в [этой статье](virtual-wan-about.md).
