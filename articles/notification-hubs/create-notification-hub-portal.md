---
title: Создание центра уведомлений Azure с помощью портала Azure | Документация Майкрософт
description: В этом руководстве вы можете узнать, как создать центр уведомлений Azure с помощью портала Azure.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 3aeeb989d15dc74849c85fa58cbefa891809f3c5
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "80347091"
---
# <a name="quickstart-create-an-azure-notification-hub-in-the-azure-portal"></a>Краткое руководство. Создание центра уведомлений Azure с помощью портала Azure 
Центры уведомлений Azure обеспечивают простой в использовании и масштабируемый механизм отправки push-уведомлений, который позволяет отправлять уведомления на любую платформу (iOS, Android, Windows, Kindle, Baidu и т. д.) из любой серверной части (облачной или локальной). Дополнительные сведения о службе см. в статье [Что такое Центры уведомлений Azure?](notification-hubs-push-notification-overview.md).

В этом кратком руководстве вы создадите центр уведомлений на портале Azure. В первом разделе приведены шаги по созданию Центров уведомлений и концентратора в этом пространстве имен. Во втором разделе приведены пошаговые инструкции по созданию центра уведомлений в имеющемся пространстве имен Центров уведомлений. 

## <a name="create-a-namespace-and-a-notification-hub"></a>Создание пространства имен и центра уведомлений
В этом разделе вы создадите пространство имен и центр в нем. 

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Создание центра уведомлений в имеющемся пространстве имен
В этом разделе создается центр уведомлений в имеющемся пространстве имен. 

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все службы** в меню слева, выполните поиск **центра уведомлений**, выберите **звездочку** (`*`) рядом с полем **Пространства имен концентратора уведомлений**, чтобы добавить его в раздел **Избранное** в меню слева. Выберите **Пространства имен концентратора уведомлений**. 

      ![Портал Azure. Выбор параметра "Пространства имен концентратора уведомлений".](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. На странице **Пространства имен концентратора уведомлений** выберите нужное пространство имен из списка. 

      ![Выбор нужного пространства имен из списка](./media/create-notification-hub-portal/select-namespace.png)
1. На странице **Пространство имен концентратора уведомлений** выберите **Добавить концентратор** на панели инструментов. 

      ![Пространства имен концентратора уведомлений. Кнопка "Добавить концентратор"](./media/create-notification-hub-portal/add-hub-button.png)
4. На странице **Пространство имен концентратора уведомлений** введите имя для концентратора уведомлений и выберите **ОК**.

      ![Страница "Пространство имен концентратора уведомлений" -> введите имя для концентратора](./media/create-notification-hub-portal/new-notification-hub-page.png)
4. Выберите **Уведомления** (значок колокольчика) в верхней части, чтобы просмотреть состояние развертывания нового центра. Выберите **X** в правом верхнем углу, чтобы закрыть окно уведомлений. 

      ![Развертывание уведомления](./media/create-notification-hub-portal/deployment-notification.png)
5. Обновите веб-страницу **Пространство имен концентратора уведомлений** для просмотра нового центра в списке. 

      ![Портал Azure — "Уведомления" -> Go to resource (Перейти к ресурсу)](./media/create-notification-hub-portal/new-hub-in-list.png)
6. Чтобы просмотреть домашнюю страницу центра уведомлений, выберите свой **центр уведомлений**. 

      ![Портал Azure — "Уведомления" -> Go to resource (Перейти к ресурсу)](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Дальнейшие действия
В этом кратком руководстве вы создали центр уведомлений. Чтобы узнать, как настроить центр с помощью системы уведомлений платформы, ознакомьтесь с [этой статьей](configure-notification-hub-portal-pns-settings.md). 