---
title: Управление доступом к данным удаленного мониторинга в Azure | Документы Майкрософт
description: Эта статья содержит сведения о настройке управления доступом для обозревателя телеметрии Аналитики временных рядов в акселераторе решения для удаленного мониторинга
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/06/2018
ms.topic: conceptual
ms.openlocfilehash: 9d5d572c3e32e3645e65ba8d6fc28b567b3c1e9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "65827212"
---
# <a name="configure-access-controls-for-the-time-series-insights-telemetry-explorer"></a>Настройка управления доступом для обозревателя телеметрии Аналитики временных рядов

Эта статья содержит сведения о настройке управления доступом для обозревателя Аналитики временных рядов в акселераторе решения для удаленного мониторинга. Если требуется предоставить пользователям акселератора решений доступ к службе "Аналитика временных рядов", необходимо предоставить каждому пользователю доступ к данным.

Политики доступа к данным предоставляют разрешения для отправки запросов данных, обработки ссылочных данных в среде, а также предоставляют другим пользователям среды доступ к сохраненным запросам и перспективам.

## <a name="grant-data-access"></a>Предоставление доступа к данным

Выполните следующие действия, чтобы предоставить доступ к данным для субъекта-пользователя.

1. Войдите на [портал Azure](https://portal.azure.com).

2. Найдите среду "Аналитика временных рядов". Введите **Временные ряды** в поле **поиска**. Выберите **Среда временных рядов** в результатах поиска. 

3. Выберите среду Time Series Insights в списке.

4. Выберите **Политики доступа к данным**, а затем нажмите кнопку **+Добавить**.
    ![Управление исходной средой "Аналитика временных рядов"](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access1.png)

5. Щелкните **Выбор пользователя**.  Выполните поиск по имени пользователя или адресу электронной почты, чтобы найти пользователя, которого следует добавить. Нажмите **Выбрать**, чтобы подтвердить выбор. 

    ![Управление источником Time Series Insights — "Добавить"](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access2.png)

6. Щелкните **Выбор ролей**. Выберите соответствующую роль доступа для пользователя.
   - Выберите **участник** , если хотите разрешить пользователю изменять эталонные данные и предоставить общий доступ к сохраненным запросам и перспективам другим пользователям среды. 
   - В противном случае выберите **читатель** , чтобы разрешить пользователям запрашивать данные в среде и сохранять личные (не общие) запросы в среде.

     Нажмите кнопку **ОК** , чтобы подтвердить выбор роли.

     ![Управление источником Time Series Insights — "Выбор пользователя"](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access3.png)

7. На странице **Выбор роли пользователя** нажмите кнопку **ОК**.

    ![Управление источником Time Series Insights — "Выбор ролей"](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access4.png)

8. На странице **Политики доступа к данным** перечислены пользователи и соответствующие роли.

    ![Управление источником Time Series Insights — результаты](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали, как обеспечивается управление доступом для обозревателя Аналитики временных рядов в акселераторе решения для удаленного мониторинга.

Дополнительные сведения об ускорителе решений для удаленного мониторинга см. в разделе [архитектура удаленного мониторинга](iot-accelerators-remote-monitoring-sample-walkthrough.md) .

Дополнительные сведения о настройке решения для удаленного мониторинга см. в статье [Настройка и повторное развертывание микрослужбы](iot-accelerators-microservices-example.md).
<!-- Next tutorials in the sequence -->