---
title: Управление IoT Central на портале Azure | Документы Майкрософт
description: В этой статье описывается создание приложений IoT Central и управление ими из портал Azure.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: e7efda8efa27044168386e3ebbc557bf7fb74e8c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80157931"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Управление IoT Central на портале Azure

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Вместо создания приложений IoT Central и управления ими на веб-сайте [диспетчера приложений Azure IOT Central](https://aka.ms/iotcentral) можно использовать [портал Azure](https://portal.azure.com) для управления приложениями.

## <a name="create-iot-central-applications"></a>Создание приложений IoT Central

Чтобы создать приложение, перейдите к [портал Azure](https://ms.portal.azure.com) и выберите **создать ресурс**.

В **поле Поиск на панели Marketplace** введите *IOT Central*:

![Портал управления: поиск](media/howto-manage-iot-central-from-portal/image0a1.png)

Выберите плитку **приложения IOT Central** в результатах поиска:

![Портал управления: результаты поиска](media/howto-manage-iot-central-from-portal/image0b1.png)

Теперь выберите **создать**:

![Портал управления: ресурс IoT Central](media/howto-manage-iot-central-from-portal/image0c1.png)

Заполните все поля в форме. Эта форма аналогична форме, которую вы заполнили для создания приложений на веб-сайте [Azure IOT Central Application Manager](https://aka.ms/iotcentral) . Дополнительные сведения см. в кратком руководстве [Создание приложения Azure IoT Central](quick-deploy-iot-central.md).

![Создание IoT Central формы](media/howto-manage-iot-central-from-portal/image6a.png)

**Расположение** — [географический регион](https://azure.microsoft.com/global-infrastructure/geographies/), где вы хотите создать приложение. Как правило, следует выбирать расположение как можно ближе к устройствам, чтобы обеспечить оптимальную производительность. В настоящее время IoT Central Azure доступна в **странах Австралии**, **Азиатско-Тихоокеанский регион**, **Европе**, США **, Великобритании**и **Японии** . **United States** Выбрав расположение, вы не сможете переместить приложение в другое расположение.

После заполнения всех полей выберите **создать**.

## <a name="manage-existing-iot-central-applications"></a>Управление существующими приложениями IoT Central

Если у вас уже есть приложение IoT Central, его можно удалить, а также перенести в другую подписку или группу ресурсов на портале Azure.

> [!NOTE]
> В портал Azure не отображаются приложения, созданные в рамках плана бесплатного ценообразования, так как они не связаны с вашей подпиской.

Чтобы начать работу, выберите **все ресурсы** на портале. Установите флажок **Показывать скрытые типы** и начните вводить имя приложения в поле **Фильтр по имени** , чтобы найти его. Затем выберите приложение IoT Central, которым вы хотите управлять.

Для перехода к приложению выберите **IOT Central URL-адрес приложения**:

![Портал управления: управление ресурсами](media/howto-manage-iot-central-from-portal/image3.png)

Чтобы переместить приложение в другую группу ресурсов, выберите **изменить** рядом с группой ресурсов. На странице **Перемещение ресурсов** выберите группу ресурсов, в которую вы хотите переместить это приложение:

![Портал управления: управление ресурсами](media/howto-manage-iot-central-from-portal/image4a.png)

Чтобы переместить приложение в другую подписку, выберите **изменить** рядом с подпиской. На странице **Перемещение ресурсов** выберите подписку, в которую вы хотите переместить это приложение:

![Портал управления: управление ресурсами](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Дальнейшие шаги

Вы узнали, как управлять приложениями Azure IoT Central на портале Azure, а значит, вы готовы к следующему шагу:

> [!div class="nextstepaction"]
> [Администрирование приложения](howto-administer.md)