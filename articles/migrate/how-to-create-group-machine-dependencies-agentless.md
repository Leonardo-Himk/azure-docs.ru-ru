---
title: Настройка анализа зависимостей без агента в службе "миграция Azure — Оценка сервера"
description: Настройка анализа зависимостей без агента в службе "миграция Azure" для оценки серверов.
ms.topic: how-to
ms.date: 2/24/2020
ms.openlocfilehash: af767bf73a3b9a6f2a91298987f11974499fd694
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79455712"
---
# <a name="set-up-agentless-dependency-visualization"></a>Настройка визуализации зависимостей без агента 

В этой статье описывается, как настроить анализ зависимостей без агента в службе "миграция Azure": Оценка сервера. [Анализ зависимостей](concepts-dependency-visualization.md) помогает определить и понять зависимости между компьютерами, которые вы хотите оценить и перенести в Azure.


> [!IMPORTANT]
> Визуализация зависимостей без агента сейчас доступна в предварительной версии только для виртуальных машин VMware, обнаруженных с помощью средства Azure Migrate: Server.
> Возможности могут быть ограничены или неполными.
> Эта предварительная версия охватывается службой поддержки клиентов и может использоваться для рабочих нагрузок.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



## <a name="before-you-start"></a>Перед началом работы

- [Сведения об](concepts-dependency-visualization.md#agentless-analysis) анализе зависимостей без агента.
- [Проверка](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) предварительных требований и поддержки для настройки визуализации зависимостей без агента для виртуальных машин VMware
- Убедитесь, что вы [создали](how-to-add-tool-first-time.md) проект "миграция Azure".
- Если вы уже создали проект, убедитесь, что вы [добавили](how-to-assess.md) средство Azure Migrate: Server для оценки серверов.
- Убедитесь, что вы настроили [устройство "миграция Azure](migrate-appliance.md) " для обнаружения локальных компьютеров. Узнайте, как настроить устройство для виртуальных машин [VMware](how-to-set-up-appliance-vmware.md) . Устройство обнаруживает локальные компьютеры и отправляет метаданные и данные производительности в службу "миграция Azure": Оценка сервера.


## <a name="current-limitations"></a>Текущие ограничения

- Сейчас вы не можете добавить или удалить сервер из группы в представлении "анализ зависимостей".
- Схема зависимостей для группы серверов сейчас недоступна.
- В настоящее время данные зависимостей нельзя скачать в табличном формате.

## <a name="create-a-user-account-for-discovery"></a>Создание учетной записи пользователя для обнаружения

Настройте учетную запись пользователя, чтобы оценка сервера могла получить доступ к виртуальной машине для обнаружения. [Сведения](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) о требованиях к учетной записи.


## <a name="add-the-user-account-to-the-appliance"></a>Добавление учетной записи пользователя в устройство

Добавьте учетную запись пользователя в устройство.

1. Откройте приложение управления устройством. 
2. Перейдите на панель **Укажите сведения о vCenter** .
3. В окне **обнаружение приложения и зависимостей на виртуальных машинах**нажмите кнопку **Добавить учетные данные** .
3. Выберите **операционную систему**, укажите понятное имя учетной записи и**пароль** **пользователя**/.
6. Нажмите кнопку **Сохранить**.
7. Нажмите кнопку **сохранить и начать обнаружение**.

    ![Добавление учетной записи пользователя виртуальной машины](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Запустить обнаружение зависимостей

Выберите компьютеры, на которых необходимо включить обнаружение зависимостей.

1. В **службе "миграция Azure": Оценка сервера**щелкните **обнаруженные серверы**.
2. Щелкните значок **анализ зависимостей** .
3. Нажмите кнопку **Добавить серверы**.
3. На странице **Добавление серверов** выберите устройство, которое будет обнаруживать соответствующие компьютеры.
4. В списке компьютер выберите компьютеры.
5. Нажмите кнопку **Добавить серверы**.

    ![Запустить обнаружение зависимостей](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Можно визуализировать зависимости около шести часов после запуска обнаружения зависимостей.

## <a name="visualize-dependencies"></a>Визуализация зависимостей

1. В **службе "миграция Azure": Оценка сервера**щелкните **обнаруженные серверы**.
2. Найдите компьютер, который вы хотите просмотреть.
3. В столбце **зависимости** щелкните **Просмотр зависимостей** .
4. Измените период времени, для которого необходимо просмотреть карту, с помощью раскрывающегося списка **Длительность времени** .
5. Разверните группу **клиентов** , чтобы получить список компьютеров с зависимостью от выбранной машины.
6. Разверните группу **портов** , чтобы получить список компьютеров, имеющих зависимость от выбранного компьютера.
7. Чтобы перейти к представлению карт зависимых компьютеров, щелкните имя компьютера > **загрузить карту сервера** .

    ![Развертывание группы портов сервера и загрузка схемы сервера](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Развернуть группу клиентов ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Разверните выбранный компьютер, чтобы просмотреть сведения об уровне процесса для каждой зависимости.

    ![Развертывание сервера для отображения процессов](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Сведения о процессе для зависимости не всегда доступны. Если он недоступен, зависимость отображается с процессом, помеченным как "Неизвестный процесс".

## <a name="stop-dependency-discovery"></a>Отключить обнаружение зависимостей

Выберите компьютеры, на которых требуется отключить обнаружение зависимостей.

1. В **службе "миграция Azure": Оценка сервера**щелкните **обнаруженные серверы**.
2. Щелкните значок **анализ зависимостей** .
3. Нажмите кнопку **удалить серверы**.
3. На странице **Удаление серверов** выберите **устройство** , которое обнаруживает виртуальные машины, на которых вы хотите отключить обнаружение зависимостей.
4. В списке компьютер выберите компьютеры.
5. Нажмите кнопку **удалить серверы**.


## <a name="next-steps"></a>Дальнейшие шаги

[Сгруппируйте компьютеры](how-to-create-a-group.md) для оценки.
