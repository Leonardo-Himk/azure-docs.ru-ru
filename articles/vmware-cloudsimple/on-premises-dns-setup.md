---
title: Решение VMware для Azure от Клаудсимпле — Настройка DNS для частного облака Клаудсимпле
description: Описывается, как настроить разрешение имен DNS для доступа к vCenter Server в частном облаке Клаудсимпле с локальных рабочих станций.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c2d69d21eb46d502a45c9df1dfaaa947d26ef7c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79246114"
---
# <a name="configure-dns-for-name-resolution-for-private-cloud-vcenter-access-from-on-premises-workstations"></a>Настройка DNS для разрешения имен для доступа к службе v-Center для частного облака с локальных рабочих станций

Чтобы получить доступ к серверу vCenter в частном облаке Клаудсимпле с локальных рабочих станций, необходимо настроить разрешение адресов DNS, чтобы сервер vCenter мог быть адресован по имени узла, а также по IP-адресу.

## <a name="obtain-the-ip-address-of-the-dns-server-for-your-private-cloud"></a>Получение IP-адреса DNS-сервера для частного облака

1. Войдите на [портал клаудсимпле](access-cloudsimple-portal.md).

2. Перейдите к разделу **ресурсы** > **частные облака** и выберите частное облако, к которому нужно подключиться.

3. На странице **Сводка** частного облака в разделе **Основные сведения**скопируйте IP-адрес DNS-сервера частного облака.

    ![DNS-серверы частного облака](media/private-cloud-dns-server.png)


Используйте любой из этих параметров для конфигурации DNS.

* [Создание зоны на DNS-сервере для *. cloudsimple.io](#create-a-zone-on-a-microsoft-windows-dns-server)
* [Создание сервера условной пересылки на локальном DNS-сервере для разрешения *. cloudsimple.io](#create-a-conditional-forwarder)

## <a name="create-a-zone-on-the-dns-server-for-cloudsimpleio"></a>Создание зоны на DNS-сервере для *. cloudsimple.io

Вы можете настроить зону в качестве зоны-заглушки и указать DNS-серверы в частном облаке для разрешения имен. В этом разделе приводятся сведения об использовании привязки DNS-сервера или DNS-сервера Microsoft Windows.

### <a name="create-a-zone-on-a-bind-dns-server"></a>Создание зоны на DNS-сервере привязки

Конкретный файл и параметры для настройки могут зависеть от конкретной настройки DNS.

Например, для конфигурации сервера привязки по умолчанию измените файл/ЕТК/намед.конф на DNS-сервере и добавьте следующие сведения о зоне.

```
zone "az.cloudsimple.io"
{
    type stub;
    masters { IP address of DNS servers; };
    file "slaves/cloudsimple.io.db";
};
```

### <a name="create-a-zone-on-a-microsoft-windows-dns-server"></a>Создание зоны на DNS-сервере Microsoft Windows

1. Щелкните правой кнопкой мыши DNS-сервер и выберите пункт **создать зону**. 
  
    ![Новая зона](media/DNS01.png)
2. Выберите **зона-заглушка** и нажмите кнопку **Далее**.

    ![Новая зона](media/DNS02.png)
3. Выберите подходящий вариант в зависимости от среды и нажмите кнопку **Далее**.

    ![Новая зона](media/DNS03.png)
4. Выберите **зону прямого просмотра** и нажмите кнопку **Далее**.

    ![Новая зона](media/DNS01.png)
5. Введите имя зоны и нажмите кнопку **Далее**.

    ![Новая зона](media/DNS05.png)
6. Введите IP-адреса DNS-серверов для частного облака, полученного на портале Клаудсимпле.

    ![Новая зона](media/DNS06.png)
7. Нажмите кнопку **Далее** , если необходимо завершить установку мастера.

## <a name="create-a-conditional-forwarder"></a>Создание сервера условной пересылки

Сервер условной пересылки перенаправляет все запросы разрешения DNS-имен на указанный серверу. При такой настройке любой запрос к *. cloudsimple.io перенаправляется на DNS-серверы, расположенные в частном облаке. В следующих примерах показано, как настроить серверы пересылки на различных типах DNS-серверов.

### <a name="create-a-conditional-forwarder-on-a-bind-dns-server"></a>Создание сервера условной пересылки на DNS-сервере привязки

Конкретный файл и параметры для настройки могут зависеть от конкретной настройки DNS.

Например, для конфигурации сервера привязки по умолчанию измените файл/ЕТК/намед.конф на DNS-сервере и добавьте следующие данные условной пересылки.

```
zone "az.cloudsimple.io" {
    type forward;
    forwarders { IP address of DNS servers; };
};
```

### <a name="create-a-conditional-forwarder-on-a-microsoft-windows-dns-server"></a>Создание сервера условной пересылки на DNS-сервере Microsoft Windows

1. Откройте диспетчер DNS на DNS-сервере.
2. Щелкните правой кнопкой мыши пункт **серверы условной пересылки** и выберите пункт Добавить новый условный сервер пересылки.

    ![Сервер условной пересылки 1 Windows DNS](media/DNS08.png)
3. Введите домен DNS и IP-адрес DNS-серверов в частном облаке и нажмите кнопку **ОК**.
