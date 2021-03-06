---
title: Создание центра Интернета вещей на портале Azure | Документация Майкрософт
description: Сведения о том, как с помощью портала Azure создавать и удалять Центры Интернета вещей Azure, а также управлять ими. Содержит сведения о ценовых категориях, масштабировании, безопасности, а также о настройке обмена сообщениями.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: robinsh
ms.openlocfilehash: c43c142b22709d42416b2dd14dfc78812970916a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79284737"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Создание Центра Интернета вещей с помощью портала Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

В этой статье описывается создание Центров Интернета вещей и управление ими с помощью [портала Azure](https://portal.azure.com).

Для выполнения шагов в этом руководстве вам потребуется подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="change-the-settings-of-the-iot-hub"></a>Изменение параметров Центра Интернета вещей

Вы можете изменить параметры Центра Интернета вещей после его создания, используя область этого центра.

![Снимок экрана, показывающий параметры для Центра Интернета вещей](./media/iot-hub-create-through-portal/iot-hub-settings-panel.png)

Ниже приведены некоторые свойства, которые можно задать для центра Интернета вещей:

**Цены и масштабирование**. Это свойство можно использовать для миграции на другой уровень или для того, чтобы задать количество единиц Центра Интернета вещей. 

**Мониторинг операций**. Включение и отключение различных категорий мониторинга, таких как ведение журнала событий, связанных с сообщениями, отправляемыми с устройства в облако и принимаемыми из облака на устройство.

**Фильтрация IP-адресов**. Здесь можно указать диапазон IP-адресов, который будет принят или отклонен центром Интернета вещей.

**Свойства**. Содержит список свойств, например идентификатор ресурса, группу ресурсов, расположение и т. д., которые можно скопировать и использовать в другом месте.

### <a name="shared-access-policies"></a>Политики общего доступа

Можно также просмотреть или изменить список политик общего доступа, выбрав **Политики общего доступа** в разделе **Параметры**. Эти политики определяют разрешения для подключения устройств и служб к Центру Интернета вещей. 

Выберите **Добавить**, чтобы открыть колонку **Добавление политики общего доступа**.  Вы можете ввести новое имя политики и указать разрешения, которые нужно связать с ней. Эта колонка показана на следующем рисунке:

![Снимок экрана добавления политики общего доступа](./media/iot-hub-create-through-portal/iot-hub-add-shared-access-policy.png)

* Политики **Чтение реестра** и **Запись в реестр** предоставляют права на чтение и запись в реестре удостоверений. Эти разрешения используются внутренними облачными службами для управления удостоверениями устройств. Если вы выберете разрешение на запись, разрешение на чтение добавляется автоматически.

* Политика **подключения к службе** предоставляет разрешение на доступ к конечным точкам службы. Это разрешение используется внутренними облачными службами для отправки и получения сообщений с устройств, а также для обновления и чтения двойникаов устройств и данных двойника модуля.

* Политика **Подключение устройства** предоставляет разрешения на отправку и получение сообщений через конечные точки Центра Интернета вещей на стороне устройства. Это разрешение используется устройствами для отправки и получения сообщений из центра Интернета вещей, обновления и чтения двойникаов устройств и данных двойника модуля и выполнения отправки файлов.

Чтобы добавить новую политику к списку уже существующих, нажмите кнопку **Создать** .

Более подробные сведения о доступе, предоставляемом конкретными разрешениями, см. в разделе [разрешения центра Интернета вещей](./iot-hub-devguide-security.md#iot-hub-permissions).

## <a name="register-a-new-device-in-the-iot-hub"></a>Регистрация нового устройства в центре Интернета вещей

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="message-routing-for-an-iot-hub"></a>Маршрутизация сообщений для центра Интернета вещей

В разделе **Маршрутизация сообщений** выберите **Обмен сообщениями**, чтобы просмотреть область "Маршрутизация сообщений", где можно определить пути и пользовательские конечные точки для центра. [Маршрутизация сообщений](iot-hub-devguide-messages-d2c.md) дает возможность управлять способом отправки данных с устройств к конечным точкам. Первым шагом является добавление нового маршрута. Затем можно добавить в маршрут существующую конечную точку или создать новую конечную точку одного из поддерживаемых типов, например хранилище BLOB-объектов. 

![Область "Маршрутизация сообщений"](./media/iot-hub-create-through-portal/iot-hub-message-routing.png)

### <a name="routes"></a>Маршруты

"Маршруты" — это первая вкладка в области "Маршрутизация сообщений". Чтобы добавить новый маршрут, щелкните **+ Добавить**. Вы увидите приведенный ниже экран. 

![Снимок экрана добавления нового маршрута](./media/iot-hub-create-through-portal/iot-hub-add-route-storage-endpoint.png)

Укажите имя центра. Имя должно быть уникальным в пределах списка маршрутов для этого центра. 

Для параметра **Конечная точка** можно выбрать существующую конечную точку из раскрывающегося списка или добавить новую. В этом примере учетная запись хранения и контейнер уже доступны. Чтобы добавить их в качестве конечной точки, нажмите кнопку **+ Добавить** рядом с раскрывающимся списком конечной точки и выберите **Хранилище BLOB-объектов**. На следующем экране показано, где заданы учетная запись хранения и контейнер.

![Снимок экрана, на котором показано добавление конечной точки хранилища для правила маршрутизации](./media/iot-hub-create-through-portal/iot-hub-routing-add-storage-endpoint.png)

Щелкните **Выберите контейнер**, чтобы выбрать учетную запись хранения и контейнер. Если выбрать эти поля, произойдет переход в область конечной точки. Используйте значения по умолчанию для остальных полей и нажмите кнопку **Создать**, чтобы создать конечную точку для учетной записи хранения и добавить ее в правила маршрутизации.

Для параметра **Источник данных** выберите "Сообщения телеметрии устройства". 

Затем добавьте запрос на маршрутизацию. В этом примере сообщения, для которых задано свойство приложения `level` со значением `critical`, маршрутизируются в учетную запись хранения.

![Снимок экрана, где показано сохранение нового правила маршрутизации](./media/iot-hub-create-through-portal/iot-hub-add-route.png)

Нажмите кнопку **Сохранить**, чтобы сохранить правило маршрутизации. Вы вернетесь к области "Маршрутизация сообщений", где отображается новое правило маршрутизации.

### <a name="custom-endpoints"></a>Пользовательские конечные точки

Перейдите на вкладку **пользовательские конечные точки** . Вы увидите уже созданные пользовательские конечные точки. На этой странице можно добавить новые конечные точки или удалить существующие. 

> [!NOTE]
> При удалении маршрута присвоенные ему конечные точки не удаляются. Чтобы удалить конечную точку, выберите вкладку "Пользовательские конечные точки", а затем выберите соответствующую конечную точку и нажмите кнопку "Удалить".
>

Дополнительные сведения о пользовательских конечных точках см. в статье [Руководство. Конечные точки Центра Интернета вещей](iot-hub-devguide-endpoints.md).

Для центра Интернета вещей можно определить до 10 пользовательских конечных точек. 

Полный пример использования конечных точек с маршрутизацией см. в статье [Руководство. Настройка маршрутизации сообщений с Центром Интернета вещей](tutorial-routing.md).

## <a name="find-a-specific-iot-hub"></a>Поиск определенного центра Интернета вещей

Ниже приведено два способа поиска определенного центра Интернета вещей в подписке:

1. Если вы знаете группу ресурсов, к которой принадлежит центр Интернета вещей, щелкните **Группы ресурсов**, а затем выберите группу ресурсов из списка. На экране группы ресурсов показаны все ресурсы в этой группе, включая центры Интернета вещей. Щелкните центр, который вы искали.

2. Выберите **Все ресурсы**. В области **Все ресурсы** есть раскрывающийся список, который по умолчанию имеет значение `All types`. Щелкните раскрывающийся список и снимите флажок `Select all`. Найдите `IoT Hub` и установите напротив него флажок. Щелкните раскрывающийся список, чтобы закрыть его. Записи будут отфильтрованы так, чтобы отображались только ваши центры Интернета вещей.

## <a name="delete-the-iot-hub"></a>Удаление Центра Интернета вещей

Чтобы удалить центр Интернета вещей, найдите его, а затем нажмите кнопку **Удалить** под именем центра Интернета вещей.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения об управлении Центром Интернета вещей в Azure см. по следующим ссылкам:

* [Руководство. Настройка маршрутизации сообщений с Центром Интернета вещей](tutorial-routing.md)
* [Метрики Центра Интернета вещей](iot-hub-metrics.md)
* [Мониторинг операций](iot-hub-operations-monitoring.md)