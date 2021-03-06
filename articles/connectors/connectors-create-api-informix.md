---
title: Подключение к базе данных IBM Informix
description: Автоматизируйте задачи и рабочие процессы, которые управляют ресурсами, хранящимися в IBM Informix, с помощью Azure Logic Apps
services: logic-apps
ms.suite: integration
author: gplarsen
ms.author: plarsen
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/07/2020
tags: connectors
ms.openlocfilehash: dccb715c974037b4e3080f3e51576feae34c03df
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76757974"
---
# <a name="manage-ibm-informix-database-resources-by-using-azure-logic-apps"></a>Управление ресурсами базы данных IBM Informix с помощью Azure Logic Apps

С помощью [Azure Logic Apps](../logic-apps/logic-apps-overview.md) и [соединителя Informix](/connectors/informix/)можно создавать автоматизированные задачи и рабочие процессы, которые управляют ресурсами в базе данных IBM Informix. Этот соединитель включает клиент Майкрософт, который обменивается данными с удаленными серверами Informix через сеть TCP/IP, включая облачные базы данных, такие как IBM Informix для Windows, работающие в виртуализации Azure и локальные базы данных при использовании [локального шлюза](../logic-apps/logic-apps-gateway-connection.md). Вы можете подключиться к этим платформам и версиям Informix, если они настроены для поддержки клиентских подключений архитектуры распределенной реляционной базы данных (DRDA):

* IBM Informix 12.1
* IBM Informix 11.7

В этой статье показано, как использовать соединитель в приложении логики для обработки операций базы данных.

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас еще нет подписки Azure, [зарегистрируйтесь для получения бесплатной учетной записи Azure](https://azure.microsoft.com/free/).

* Для локальных баз данных [скачайте и установите локальный шлюз данных](../logic-apps/logic-apps-gateway-install.md) на локальном компьютере, а затем [Создайте ресурс шлюза данных Azure на портал Azure](../logic-apps/logic-apps-gateway-connection.md).

* Приложение логики, в котором требуется доступ к базе данных Informix. Этот соединитель предоставляет только действия, поэтому приложение логики уже должно начинаться с триггера, например [триггера повторения](../connectors/connectors-native-recurrence.md). 

## <a name="add-an-informix-action"></a>Добавление действия Informix

1. Откройте приложение логики в конструкторе приложений логики на [портале Azure](https://portal.azure.com), если оно еще не открыто.

1. На шаге, где нужно добавить действие Informix, выберите **новый шаг**.

   Чтобы добавить действие между существующими шагами, наведите указатель мыши на соединяющую стрелку. Выберите отображаемый знак плюса (**+**), а затем щелкните **Добавить действие**.

1. В поле поиска введите `informix` в качестве фильтра. В списке действия выберите нужное действие, например:

   ![Выберите действие Informix для запуска](./media/connectors-create-api-informix/select-informix-connector-action.png)

   Соединитель предоставляет следующие действия, которые выполняют соответствующие операции с базой данных:

   * Получение таблиц — вывод списка таблиц базы данных с `CALL` помощью инструкции
   * Получение строк — чтение всех строк с помощью `SELECT *` инструкции
   * Получение строки — чтение строки с помощью `SELECT WHERE` оператора
   * Добавление строки с помощью `INSERT` оператора
   * Изменение строки с помощью `UPDATE` оператора
   * Удаление строки с помощью `DELETE` оператора

1. Если появится запрос на ввод сведений о подключении для базы данных Informix, выполните [действия по созданию подключения](#create-connection), а затем перейдите к следующему шагу.

1. Укажите сведения для выбранного действия:

   | Действие | Описание | Свойства и описания |
   |--------|-------------|-----------------------------|
   | **Получение таблиц** | Перечислите таблицы базы данных, выполнив инструкцию CALL Informix. | Нет |
   | **Получение строк** | Получение всех строк в указанной таблице путем выполнения оператора Informix `SELECT *` . | **Имя таблицы**: имя нужной таблицы Informix <p><p>Чтобы добавить другие свойства в это действие, выберите их в списке **Добавить новый параметр** . Дополнительные сведения см. в [справочном разделе по соединителю](/connectors/informix/). |
   | **Получение строки** | Получить строку из указанной таблицы, выполнив инструкцию Informix `SELECT WHERE` . | - **Имя таблицы**: имя нужной таблицы Informix <br>- **Идентификатор строки**: уникальный идентификатор строки, например`9999` |
   | **Вставка строки** | Добавьте строку в указанную таблицу Informix, выполнив инструкцию `INSERT` Informix. | - **Имя таблицы**: имя нужной таблицы Informix <br>- **Item**: строка со значениями для добавления. |
   | **Обновление строки** | Измените строку в указанной таблице Informix, выполнив инструкцию Informix `UPDATE` . | - **Имя таблицы**: имя нужной таблицы Informix <br>- **Идентификатор строки**: уникальный идентификатор обновляемой строки, например`9999` <br>- **Row**: строка с обновленными значениями, например`102` |
   | **Удалить строку** | Удалите строку из указанной таблицы Informix, выполнив инструкцию Informix `DELETE` . | - **Имя таблицы**: имя нужной таблицы Informix <br>- **Идентификатор строки**: уникальный идентификатор удаляемой строки, например`9999` |
   ||||

1. Сохраните приложение логики. Теперь либо [протестируйте приложение логики](#test-logic-app) , либо продолжите создание приложения логики.

<a name="create-connection"></a>

## <a name="connect-to-informix"></a>Подключение к Informix

1. Если приложение логики подключается к локальной базе данных, выберите **подключиться через локальный шлюз данных**.

1. Укажите сведения о подключении и нажмите кнопку **создать**.

   | Свойство | Свойство JSON | Обязательный | Пример значения | Описание |
   |----------|---------------|----------|---------------|-------------|
   | Имя соединения | `name` | Да | `informix-demo-connection` | Имя, используемое для подключения к базе данных Informix. |
   | Server (Сервер) | `server` | Да | Вычислений`informixdemo.cloudapp.net:9089` <br>Локальная среда:`informixdemo:9089` | Адрес или псевдоним TCP/IP, который находится в формате IPv4 или IPv6, за которым следует двоеточие и номер порта TCP/IP. |
   | База данных | `database` | Да | `nwind` | Имя DRDA реляционной базы данных (RDBNAM) или имя базы данных Informix (dbname). Informix принимает 128-байтовую строку. |
   | Проверка подлинности | `authentication` | Только в локальной среде | **Basic** или **Windows** (Kerberos) | Тип проверки подлинности, необходимый для базы данных Informix. Это свойство отображается только при выборе **подключения через локальный шлюз данных**. |
   | Имя пользователя | `username` | Нет | <*база данных-пользователь-имя*> | Имя пользователя для базы данных |
   | Пароль | `password` | Нет | <*база данных — пароль*> | Пароль для базы данных |
   | Шлюз | `gateway` | Только в локальной среде | -<*Azure-Subscription*> <br>-<*Azure в локальной среде — Data-Gateway-Resource*> | Подписка Azure и имя ресурса Azure для локального шлюза данных, созданного в портал Azure. Свойство и вложенные свойства **шлюза** отображаются только при выборе **подключения через локальный шлюз данных**. |
   ||||||

   Пример:

   * **Облачная база данных**

     ![Сведения о подключении к облачной базе данных](./media/connectors-create-api-informix/informix-cloud-connection.png)

   * **Локальная база данных**

     ![Сведения о подключении к локальной базе данных](./media/connectors-create-api-informix/informix-on-premises-connection.png)

1. Сохраните приложение логики.

<a name="test-logic-app"></a>

## <a name="test-your-logic-app"></a>Тестирование приложения логики

1. На панели инструментов конструктора приложений логики выберите **выполнить**. После запуска приложения логики можно просмотреть выходные данные этого запуска.

1. В меню приложения логики выберите **Обзор**. В области Обзор в разделе**Журнал выполнений** **сводки** > выберите последний запуск.

1. В разделе **Запуск приложения логики**выберите **сведения о выполнении**.

1. В списке действия выберите действие с выходными данными, которые необходимо просмотреть, например **Get_tables**.

   Если действие выполнено успешно, его свойство **Status** помечается как **успешное**.

1. Чтобы просмотреть входные данные, в разделе **входные данные**выберите ссылку URL-адрес. Чтобы просмотреть выходные данные, в разделе **выходные данные** ссылки щелкните ссылку URL-адрес. Ниже приведено несколько примеров выходных данных:

   * **Get_tables** отображает список таблиц:

     ![Выходные данные действия "получение таблиц"](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

   * **Get_rows** отображает список строк:

     ![Выходные данные действия "Получение строк"](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

   * **Get_row** показывает указанную строку:

     ![Выходные данные действия "получить строку"](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

   * **Insert_row** отображает новую строку:

     ![Выходные данные из действия "Вставка строки"](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

   * В **Update_row** отображается обновленная строка:

     ![Выходные данные действия "обновить строку"](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

   * **Delete_row** показывает удаленную строку:

     ![Выходные данные действия "удалить строку"](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="connector-specific-details"></a>Сведения о соединителях

Технические сведения о триггерах, действиях и ограничениях, которые описаны в описании Swagger соединителя, см. на [странице справочника по соединителю](/connectors/informix/).

## <a name="next-steps"></a>Дальнейшие шаги

* См. дополнительные сведения о других [соединителях Logic Apps](apis-list.md).
