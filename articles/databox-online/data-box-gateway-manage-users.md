---
title: Управление пользователями в Шлюзе Azure Data Box | Документация Майкрософт
description: В этой статье описывается управление пользователями в Шлюзе Azure Data Box с помощью портала Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 137531ab0d96c24a3ce19e429a5765d6e4a13c93
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82561238"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-gateway"></a>Управление пользователями в Шлюзе Azure Data Box с помощью портала Azure

В этой статье описывается управление пользователями в Шлюзе Azure Data Box. Вы можете управлять Шлюзом Azure Data Box на портале Azure или с помощью локального пользовательского веб-интерфейса. Используйте портал Azure, чтобы добавлять, изменять или удалять пользователей. 

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Добавление пользователей
> * Изменение пользователя
> * Удаление пользователя

## <a name="about-users"></a>Сведения о пользователях

У пользователей может быть доступ только для чтения или полный доступ. Как указано в определении, пользователи с доступом только для чтения могут только просматривать данные в общем файловом ресурсе. Пользователи с полным доступом могут просматривать данные в общем файловом ресурсе, выполнять запись в общие папки, а также изменять или удалять данные из общей папки.

 - **Пользователь с правами полного доступа** — это локальный пользователь, у которого есть полный доступ.
 - **Пользователь с правами только для чтения** — это локальный пользователь, у которого есть доступ только для чтения. Эти пользователи связаны с общими папками, которые позволяют выполнять операции только для чтения.

Сначала разрешения пользователя определяются при его создании во время создания общей папки. Изменение разрешений на уровне общего ресурса сейчас не поддерживается.

## <a name="add-a-user"></a>Добавление пользователей

Выполните на портале Azure шаги ниже, чтобы добавить пользователя.

1. На портале Azure выберите ресурс Шлюза Data Box и перейдите к разделу **Обзор**. На панели команд щелкните **+ Add user** (+ Добавить пользователя).

    ![Нажатие кнопки "Добавить пользователя"](media/data-box-gateway-manage-users/add-user-1.png)

2. Укажите имя и пароль для пользователя, которого вы хотите добавить. Подтвердите пароль и щелкните **Добавить**.

    ![Нажатие кнопки "Добавить пользователя"](media/data-box-gateway-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Эти пользователи зарезервированы системой и не должны использоваться: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Вы получите уведомление о начале и завершении процесса создания пользователя. После создания пользователя щелкните **Обновить** на панели команд, чтобы просмотреть обновленный список пользователей.


## <a name="modify-user"></a>Изменение пользователя

Вы можете изменить пароль, связанный с пользователем, после создания пользователя. Выберите и щелкните пользователя в списке пользователей. Укажите и подтвердите новый пароль. Сохраните изменения.
 
![Изменение пользователя](media/data-box-gateway-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Удаление пользователя

Чтобы удалить пользователя, выполните следующие действия на портале Azure.

1. Выберите пользователя из списка пользователей, а затем щелкните **Удалить**.  

   ![Удаление пользователя](media/data-box-gateway-manage-users/delete-user-1.png)

2. При появлении запроса подтвердите удаление. 

   ![Удаление пользователя](media/data-box-gateway-manage-users/delete-user-2.png)

Список пользователей обновляется с учетом удаления.

![Удаление пользователя](media/data-box-gateway-manage-users/delete-user-3.png)


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте об [управлении пропускной способностью](data-box-gateway-manage-bandwidth-schedules.md).
