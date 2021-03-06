---
title: Журнал обращений в службу поддержки для Azure Stack ребра, Шлюз Azure Data Box | Документация Майкрософт
description: Узнайте, как заносить в журнал запросы на поддержку для вопросов, связанных с Azure Stack пограничным или Шлюз Data Box заказами.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 3839fb325b1ed0c052f7a4e8955e9a9fda51fc5f
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82569658"
---
# <a name="open-a-support-ticket-for-azure-stack-edge-and-azure-data-box-gateway"></a>Отправьте запрос в службу поддержки для Azure Stack пограничных Шлюз Azure Data Box

Эта статья относится к Azure Stack границе и Шлюз Azure Data Box, которые управляются Azure Stack пограничным или Шлюз Azure Data Box службой. При возникновении проблем со службой можно создать запрос на обслуживание для получения технической поддержки. В этой статье описаны следующие операции.

* Создание запроса на техническую поддержку.
* Управление жизненным циклом запроса на техническую поддержку на портале.

## <a name="create-a-support-request"></a>Создание запроса на обслуживание

Для создания запроса на обслуживание выполните следующие действия.

1. Перейдите к Azure Stack границе или Шлюз Data Box порядке. Перейдите в раздел **Поддержка и устранение неполадок** , а затем выберите **новый запрос в службу поддержки**.
   
2. В **новом запросе на поддержку**на вкладке **Основные сведения** выполните следующие действия.
    
    1. В раскрывающемся списке **Тип проблемы** выберите **Техническая**.
    2. Выберите свою **подписку**.
    3. В разделе **Служба** установите флажок **Мои службы**. В раскрывающемся списке выберите **Azure Stack ребра и шлюз Data Box**.
    4. Выберите **ресурс**. Соответствует имени вашего заказа.
    5. Укажите краткую **сводку** возникшей проблемы. 
    6. Выберите **тип проблемы**.
    7. В зависимости от выбранного типа проблемы выберите соответствующий подтип **проблемы**.
    8. Нажмите кнопку **Далее: solutions >>**.

        ![Основы](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)

3. На вкладке **сведения** выполните следующие действия.
    
    1. Укажите дату и время начала для проблемы.
    2. Укажите **Описание** проблемы.
    3. В поле **Отправка файла**выберите значок папки, чтобы просмотреть другие файлы, которые нужно передать.
    4. Установите флажок **Предоставить диагностическую информацию**.
    5. На основе вашей подписки **план поддержки** заполняется автоматически.
    6. В раскрывающемся списке выберите **серьезность**.
    7. Укажите **предпочтительный способ связи**.
    8. **Часы отклика** выбираются автоматически на основе плана подписки.
    9. Укажите предпочтительный язык для поддержки.
    10. В **контактной информации**укажите свое имя, адрес электронной почты, номер телефона, необязательный контакт, страну или регион. Служба технической поддержки Майкрософт будет использовать эту информацию, чтобы связаться с вами для получения дополнительных сведений, диагностики и устранения проблемы. 
    11. Выберите **Далее: Проверка + создать >>**.

        ![Проблема](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-2.png)

4. На вкладке " **Проверка и создание** " Проверьте сведения, связанные с запросом в службу поддержки. Щелкните **Создать**. 

    ![Проблема](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-3.png)

    После создания запроса в службу поддержки специалист по технической поддержке свяжется с вами как можно скорее, чтобы продолжить работу с запросом.

## <a name="get-hardware-support"></a>Получение поддержки оборудования

Эти сведения применимы только к устройствам Azure Stack. Процесс сообщения о проблемах оборудования выглядит следующим образом:

1. Отправьте запрос в службу поддержки из портал Azure для устранения проблем с оборудованием. В разделе **тип проблемы**выберите **Azure Stack оборудование**. Выберите **подтип проблемы** как **сбой оборудования**. 

    ![Проблемы с оборудованием](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-hardware-issue-1.png)

    После создания запроса в службу поддержки специалист по технической поддержке свяжется с вами как можно скорее, чтобы продолжить работу с запросом. 

2. Если служба поддержки Майкрософт определяет, что это проблема оборудования, выполняется одно из следующих действий. 

    - Отправляется единица замены поля (FRU) для неисправной части оборудования. В настоящее время мощностью блока питания является единственной поддерживаемой FRU. 
    - В случае любого другого сбоя Корпорация Майкрософт выполняет полную замену системы (ФСР) или замену устройства.

3. Если запрос в службу поддержки выдается до 4:30 по местному времени (с понедельника по пятницу), специалист по локализации отправляет следующий рабочий день в ваше местоположение для выполнения FRU или полной замены устройства.

## <a name="manage-a-support-request"></a>Управление запросом в службу поддержки

После создания запроса вы можете управлять его жизненным циклом, не покидая портал.

#### <a name="to-manage-your-support-requests"></a>Управление запросами в службу поддержки

1. Чтобы открыть страницу справки и поддержки, последовательно щелкните **Обзор > Справка и поддержка**.

    ![Управление запросами на поддержку](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-manage-support-request-1.png)   

2. Список **недавних запросов в службу поддержки** в виде таблицы отображается в разделе **Справка и поддержка**.

    <!--[Manage support requests](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)--> 

3. Выберите и щелкните запрос на поддержку. Для запроса можно просмотреть состояние и подробные сведения. Щелкните **+ Новое сообщение**, если нужно следить за этим запросом.

   
## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [устранять проблемы, связанные с Azure Stack пограничными](azure-stack-edge-troubleshoot.md).
Узнайте, как [устранять проблемы, связанные с шлюз Data Box](data-box-gateway-troubleshoot.md).
