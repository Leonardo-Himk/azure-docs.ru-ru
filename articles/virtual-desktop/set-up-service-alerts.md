---
title: Настройка оповещений служб для Виртуального рабочего стола Windows — Azure
description: Как настроить Работоспособность служб Azure, чтобы получать уведомления служб для Виртуального рабочего стола Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: ad25ab219cdb83227d39f86109d18b2c8402c38f
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612356"
---
# <a name="tutorial-set-up-service-alerts"></a>Руководство по Настройка оповещений служб

>[!IMPORTANT]
>Это содержимое применимо к обновлению за весну 2020 года с объектами Azure Resource Manager для Виртуального рабочего стола Windows. Если вы используете выпуск Виртуального рабочего стола Windows за осень 2019 года без объектов Azure Resource Manager, см. [эту статью](./virtual-desktop-fall-2019/set-up-service-alerts-2019.md).
>
> Обновление Виртуального рабочего стола Windows за весну 2020 года пока предоставляется как общедоступная предварительная версия. Эта предварительная версия предоставляется без соглашений об уровне обслуживания и не рекомендована для выполнения производственных рабочих нагрузок. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. 
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Вы можете использовать Работоспособность служб Azure, чтобы отслеживать проблемы служб и рекомендации по работоспособности для Виртуального рабочего стола Windows. Служба "Работоспособность служб Azure" может информировать вас с помощью разных типов оповещений (например, сообщение электронной почты или SMS) о возникновении проблем и их масштабе, а также уведомлять вас после их устранения. Служба "Работоспособность служб Azure" также помогает предотвращать простой и подготовиться к плановому обслуживанию и изменениям, которые могут повлиять на доступность ваших ресурсов.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание и настройка оповещений служб.

Подробные сведения о службе "Работоспособность служб Azure" см. [здесь](https://docs.microsoft.com/azure/service-health/).

## <a name="create-service-alerts"></a>Создание оповещений служб

В этой статье показано, как настроить Работоспособность служб и уведомления, к которым у вас есть доступ на портале Azure. Вы можете настроить разные типы оповещений и запланировать их, чтобы своевременно получать уведомления.

### <a name="recommended-service-alerts"></a>Рекомендуемые оповещения служб

Мы рекомендуем создать оповещения службы для следующих типов событий работоспособности.

- **Проблема со службой.** Получение уведомлений по основным проблемам, которые влияют на возможность подключения пользователей, с помощью службы или с возможностью управления клиентом Виртуального рабочего стола Windows.
- **Рекомендации по работоспособности.** Получение уведомлений, которые требуют внимания. Ниже приводятся некоторые примеры такого типа уведомления:
    - Виртуальные машины настроены небезопасно из-за открытого порта 3389.
    - Прекращение поддержки функциональности.

### <a name="configure-service-alerts"></a>Настройка оповещений служб

Чтобы настроить оповещения служб:

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Выберите **Работоспособность служб**.
3. Выполните инструкции в разделе [Создание оповещения журнала действий по уведомлениям службы](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) для настройки оповещений и уведомлений.

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике вы узнали, как настраивать и использовать Работоспособность служб Azure, чтобы отслеживать проблемы служб и рекомендации по работоспособности для Виртуального рабочего стола Windows. Чтобы узнать о том, как можно выполнить вход в Виртуальный рабочий стол Windows, перейдите к учебникам по подключению к Виртуальному рабочему столу Windows.

> [!div class="nextstepaction"]
> [Connect to the Remote Desktop client on Windows 7 and Windows 10](./connect-windows-7-and-10.md) (Подключение к клиенту удаленного рабочего стола в Windows 7 и Windows 10)
