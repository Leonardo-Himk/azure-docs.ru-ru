---
title: Запрет доступа к общедоступной сети — портал Azure — база данных Azure для MariaDB
description: Узнайте, как настроить запрет доступа к общедоступной сети с помощью портал Azure для базы данных Azure для MariaDB.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 87b6033e486e9009573436628d0183c8a022aced
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79375243"
---
# <a name="deny-public-network-access-in-azure-database-for-mariadb-using-azure-portal"></a>Запрет доступа к общедоступной сети в базе данных Azure для MariaDB с помощью портал Azure

В этой статье описывается, как настроить сервер базы данных Azure для MariaDB для запрета всех общедоступных конфигураций и разрешить только подключения через частные конечные точки для дальнейшего повышения безопасности сети.

## <a name="prerequisites"></a>Предварительные условия

Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:

* [База данных Azure для MariaDB](quickstart-create-MariaDB-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Установка запрета доступа к общедоступной сети

Выполните следующие действия, чтобы настроить MariaDB Server для запрета доступа к общедоступной сети.

1. В [портал Azure](https://portal.azure.com/)выберите существующий сервер базы данных Azure для MariaDB.

1. На странице сервер MariaDB в разделе **Параметры**щелкните **Безопасность подключения** , чтобы открыть страницу Настройка безопасности подключения.

1. В списке запретить доступ к общедоступной сети выберите **Да** , чтобы включить запрет общего доступа для сервера MariaDB.

    ![Служба "база данных Azure для MariaDB" запрещает доступ к сети](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. Чтобы сохранить изменения, нажмите кнопку **Сохранить**.

1. Уведомление подтверждает успешное включение параметра безопасности подключения.

    ![Служба "база данных Azure для MariaDB" запретила доступ к сети](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте [, как создавать оповещения по метрикам](howto-alert-metric.md).