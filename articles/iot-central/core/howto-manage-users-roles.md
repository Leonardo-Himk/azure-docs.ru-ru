---
title: Управление пользователями и ролями в IoT Central приложении Azure | Документация Майкрософт
description: Как администратор, как управлять пользователями и ролями в приложении IoT Central Azure;
author: lmasieri
ms.author: lmasieri
ms.date: 12/05/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: c00f9d8baa55ef0d0cf6322ee71f22e739e6acdc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80365504"
---
# <a name="manage-users-and-roles-in-your-iot-central-application"></a>Управление пользователями и ролями в приложении IoT Central

В этой статье описывается, как администратор может добавлять, изменять и удалять пользователей в приложении IoT Central Azure. В статье также описывается, как управлять ролями в приложении IoT Central Azure.

Чтобы получить доступ к разделу **Администрирование**, вам должна быть назначена роль **Администратор** приложения Azure IoT Central. При создании приложения IoT Central Azure автоматически добавляется к роли **администратора** этого приложения.

## <a name="add-users"></a>Добавление пользователей

Каждый пользователь должен иметь учетную запись, чтобы войти в систему и получить доступ к приложению Azure IoT Central. Учетные записи Майкрософт и Azure Active Directory поддерживаются в IoT Central Azure. Группы Azure Active Directory сейчас не поддерживаются в Azure IoT Central.

Дополнительные сведения см. на странице [справки по учетной записи Майкрософт](https://support.microsoft.com/products/microsoft-account?category=manage-account) и в статье [Краткое руководство по добавлению новых пользователей в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Чтобы добавить пользователя в приложение IoT Central, в разделе **Администрирование** перейдите на страницу **Пользователи**.
    
    > [!div class="mx-imgBorder"]
    >![Управление пользователями](media/howto-manage-users-roles/manage-users-pnp.png)

1. На странице **Пользователи** выберите **+Добавить пользователя**.

1. Выберите роль в раскрывающемся списке **Роль**. Дополнительные сведения о ролях см. в разделе [Управление ролями](#manage-roles) этой статьи.

    > [!div class="mx-imgBorder"]
    >![Добавление пользователя и выбор роли](media/howto-manage-users-roles/add-user-pnp.png)

    > [!NOTE]
    > Пользователь, который находится в пользовательской роли, предоставляющей им разрешение на добавление других пользователей, может добавлять пользователей только к роли с тем же или меньшим числом разрешений, чем их собственная роль.

Если идентификатор пользователя IoT Central удаляется из Azure Active Directory и затем добавляется повторно, он не сможет войти в приложение IoT Central. Чтобы повторно включить доступ, администратор IoT Central должен удалить и прочесть пользователя в приложении.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Изменение ролей, назначенных пользователям

Роли нельзя изменить после назначения. Чтобы изменить роль, назначенную пользователю, удалите учетную запись и повторно добавьте пользователя с другой ролью.

> [!NOTE]
> Назначенные роли относятся только к IoT Centralному приложению и не могут управляться с помощью портала Azure.

## <a name="delete-users"></a>Удаление пользователей

Чтобы удалить пользователей, установите один или несколько флажков на странице **Пользователи**. Затем выберите **Удалить**.

## <a name="manage-roles"></a>Управление ролями

Роли позволяют контролировать, кто в вашей организации может выполнять различные задачи в IoT Central. Существует три встроенных роли, которые можно назначить пользователям приложения. Кроме того, можно [создавать пользовательские роли](#create-a-custom-role) , если требуется более детализированный контроль.

> [!div class="mx-imgBorder"]
> ![Управление выбором ролей](media/howto-manage-users-roles/manage-roles-pnp.png)

### <a name="administrator"></a>Администратор

Пользователи с ролью **администратора** могут управлять и контролировать каждую часть приложения, включая выставление счетов.

Пользователю, создавшему приложение, автоматически назначается роль **администратора**. В роли **администратора** должен быть хотя бы один пользователь.

### <a name="builder"></a>Конструктор

Пользователи в роли **построителя** могут управлять каждой частью приложения, но не могут вносить изменения на вкладках администрирование или непрерывное экспорт данных.

### <a name="operator"></a>Оператор

Пользователи в роли **оператора** могут отслеживать работоспособность и состояние устройства. Они не могут вносить изменения в шаблоны устройств или администрировать приложение. Операторы могут добавлять и удалять устройства, управлять наборами устройств и выполнять аналитику и задания. 

## <a name="create-a-custom-role"></a>Создание пользовательской роли

Если для решения требуются детализированные элементы управления доступом, можно создать пользовательские роли с пользовательскими наборами разрешений. Чтобы создать пользовательскую роль, перейдите на страницу **роли** в разделе **Администрирование** приложения. Затем выберите **+ создать роль**и добавьте имя и описание для своей роли. Выберите разрешения, которые требуются для роли, и нажмите кнопку **сохранить**.

Пользователи могут добавлять пользователей к пользовательской роли так же, как и к встроенной роли.

> [!div class="mx-imgBorder"]
> ![Создание пользовательской роли](media/howto-manage-users-roles/create-custom-role-pnp.png)

### <a name="custom-role-options"></a>Параметры настраиваемой роли

При определении пользовательской роли выбирается набор разрешений, предоставленных пользователю, если он является членом роли. Некоторые разрешения зависят от других. Например, при добавлении к роли разрешения на **Обновление панелей мониторинга** приложений автоматически добавляется разрешение **Просмотр панелей мониторинга приложений** . В следующих таблицах перечислены доступные разрешения и их зависимости, которые можно использовать при создании пользовательских ролей.

#### <a name="managing-devices"></a>Управление устройствами

**Разрешения для шаблона устройства**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Управление | Просмотр <br/> Другие зависимости: просмотр экземпляров устройств  |
| Полный доступ | Просмотр, управление <br/> Другие зависимости: просмотр экземпляров устройств |

**Разрешения для экземпляра устройства**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет <br/> Другие зависимости: Просмотр шаблонов устройств и групп устройств |
| Обновление: | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств и групп устройств  |
| Создание | Представление <br/> Другие зависимости: Просмотр шаблонов устройств и групп устройств  |
| Удалить | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств и групп устройств  |
| Выполнение команд | Обновление, просмотр <br/> Другие зависимости: Просмотр шаблонов устройств и групп устройств  |
| Полный доступ | Просмотр, обновление, создание, удаление, выполнение команд <br/> Другие зависимости: Просмотр шаблонов устройств и групп устройств  |

**Разрешения для групп устройств**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет <br/> Другие зависимости: Просмотр шаблонов устройств и экземпляров устройств |
| Обновление: | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств и экземпляров устройств   |
| Создание | Просмотр, обновление <br/> Другие зависимости: Просмотр шаблонов устройств и экземпляров устройств   |
| Удалить | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств и экземпляров устройств   |
| Полный доступ | Просмотр, обновление, создание, удаление <br/> Другие зависимости: Просмотр шаблонов устройств и экземпляров устройств |

**Разрешения на управление подключением устройств**

| Имя | Зависимости |
| ---- | -------- |
| Чтение экземпляра | Нет <br/> Другие зависимости: Просмотр шаблонов устройств, групп устройств, экземпляров устройств |
| Управление экземпляром | Нет |
| Чтение глобального | Нет   |
| Управление глобальными | Чтение глобального |
| Полный доступ | Чтение экземпляра, управление экземплярами, чтение глобальных, Управление глобальными. <br/> Другие зависимости: Просмотр шаблонов устройств, групп устройств, экземпляров устройств |

**Разрешения заданий**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств и групп устройств |
| Обновление: | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств и групп устройств |
| Создание | Просмотр, обновление <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств и групп устройств |
| Удалить | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств и групп устройств |
| Execute | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств и групп устройств; Обновление экземпляров устройств; Выполнять команды в экземплярах устройств |
| Полный доступ | Просмотр, обновление, создание, удаление, выполнение <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств и групп устройств; Обновление экземпляров устройств; Выполнять команды в экземплярах устройств |

**Разрешения правил**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет <br/> Другие зависимости: Просмотр шаблонов устройств |
| Обновление: | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств |
| Создание | Просмотр, обновление <br/> Другие зависимости: Просмотр шаблонов устройств |
| Удалить | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств |
| Полный доступ | Просмотр, обновление, создание, удаление <br/> Другие зависимости: Просмотр шаблонов устройств |

#### <a name="managing-the-app"></a>Управление приложением

**Разрешения для параметров приложения**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Обновление: | Просмотр   |
| Копировать | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств, групп устройств, панелей мониторинга, экспорта данных, фирменной символики, ссылок на справку, пользовательских ролей, правил |
| Удалить | Просмотр   |
| Полный доступ | Просмотр, обновление, копирование, удаление <br/> Другие зависимости: Просмотр шаблонов устройств, групп устройств, панелей мониторинга приложений, экспорт данных, фирменная символика, ссылки справки, пользовательские роли, правила |

**Разрешения на экспорт шаблона приложения**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Экспортировать | Просмотр <br/> Другие зависимости: Просмотр шаблонов устройств, экземпляров устройств, групп устройств, панелей мониторинга, экспорта данных, фирменной символики, ссылок на справку, пользовательских ролей, правил |
| Полный доступ | Просмотр, экспорт <br/> Другие зависимости: Просмотр шаблонов устройств, групп устройств, панелей мониторинга приложений, экспорт данных, фирменная символика, ссылки справки, пользовательские роли, правила |

**Разрешения на выставление счетов**

| Имя | Зависимости |
| ---- | -------- |
| Управление | Нет     |
| Полный доступ | Управление |

#### <a name="managing-users-and-roles"></a>Управление пользователями и ролями

**Разрешения пользовательских ролей**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет |
| Обновление: | Просмотр |
| Создание | Просмотр, обновление |
| Удалить | Просмотр |
| Полный доступ | Просмотр, обновление, создание, удаление |

**Разрешения на управление пользователями**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет <br/> Другие зависимости: Просмотр пользовательских ролей |
| Add | Просмотр <br/> Другие зависимости: Просмотр пользовательских ролей |
| Удалить | Просмотр <br/> Другие зависимости: Просмотр пользовательских ролей |
| Полный доступ | Просмотр, добавление, удаление <br/> Другие зависимости: Просмотр пользовательских ролей |

> [!NOTE]
> Пользователь, который находится в пользовательской роли, предоставляющей им разрешение на добавление других пользователей, может добавлять пользователей только к роли с тем же или меньшим числом разрешений, чем их собственная роль.

#### <a name="customizing-the-app"></a>Настройка приложения

**Разрешения панели мониторинга приложения**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Обновление: | Просмотр   |
| Создание | Просмотр, обновление |
| Удалить | Просмотр   |
| Полный доступ | Просмотр, обновление, создание, удаление |

**Разрешения личных панелей мониторинга**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Обновление: | Просмотр   |
| Создание | Просмотр, обновление   |
| Удалить | Просмотр   |
| Полный доступ | Просмотр, обновление, создание, удаление |

**Разрешения на фирменную символику, фавикон и цвета**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Обновление: | Просмотр   |
| Полный доступ | Просмотр, обновление |

**Разрешения ссылок на справку**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Обновление: | Просмотр   |
| Полный доступ | Просмотр, обновление |

#### <a name="extending-the-app"></a>Расширение приложения

**Разрешения на экспорт данных**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Обновление: | Просмотр   |
| Создание | Просмотр, обновление  |
| Удалить | Просмотр   |
| Полный доступ | Просмотр, обновление, создание, удаление |

**Разрешения токена API**

| Имя | Зависимости |
| ---- | -------- |
| Просмотр | Нет     |
| Создание | Представление   |
| Удалить | Просмотр   |
| Полный доступ | Просмотр, создание, удаление |

## <a name="next-steps"></a>Дальнейшие шаги

Теперь, когда вы узнали, как управлять пользователями и ролями в приложении IoT Central Azure, рекомендуем следующий шаг — узнать, как [управлять счетом](howto-view-bill.md).
