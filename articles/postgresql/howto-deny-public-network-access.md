---
title: Запрет доступа к общедоступной сети — портал Azure — "база данных Azure для PostgreSQL — один сервер"
description: Узнайте, как настроить запрет доступа к общедоступной сети с помощью портал Azure для базы данных Azure для PostgreSQL одного сервера.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 4dff2321414721dbd415b468e59aea0ab4b3acee
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79375126"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>Запрет доступа к общедоступной сети в базе данных Azure для PostgreSQL с помощью портал Azure

В этой статье описывается, как настроить односерверную базу данных Azure для PostgreSQL, чтобы запретить все общедоступные конфигурации и разрешить подключения только через частные конечные точки для дальнейшего повышения безопасности сети.

## <a name="prerequisites"></a>Предварительные условия

Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:

* [Один сервер базы данных Azure для PostgreSQL](quickstart-create-PostgreSQL Single server-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Установка запрета доступа к общедоступной сети

Выполните следующие действия, чтобы настроить PostgreSQL Single Server для запрета доступа к общедоступной сети.

1. В [портал Azure](https://portal.azure.com/)выберите существующую базу данных Azure для PostgreSQL Single Server.

1. На странице PostgreSQL Single Server в разделе **Параметры**щелкните **Безопасность подключения** , чтобы открыть страницу Настройка безопасности подключения.

1. В списке **запретить доступ к общедоступной сети**выберите **Да** , чтобы разрешить запретить общий доступ для PostgreSQL одного сервера.

    ![Сервер базы данных Azure для PostgreSQL запрещает доступ к сети](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. Чтобы сохранить изменения, нажмите кнопку **Сохранить**.

1. Уведомление подтверждает успешное включение параметра безопасности подключения.

    ![Служба "база данных Azure для PostgreSQL" запрещает доступ к сети](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте [, как создавать оповещения по метрикам](howto-alert-on-metric.md).