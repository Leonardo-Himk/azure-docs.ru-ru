---
title: Настройка балансировки нагрузки и правил исходящего трафика с помощью портал Azure
titleSuffix: Azure Load Balancer
description: В этой статье показано, как настроить балансировку нагрузки и правила исходящего трафика в Load Balancer (цен. категория "Стандартный") с помощью портал Azure.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: b75f49155991bfc71f788ad88f166c0bec281841
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77590017"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-the-azure-portal"></a>Настройка балансировки нагрузки и правил исходящего трафика в Load Balancer (цен. категория "Стандартный") с помощью портал Azure

В этой статье показано, как настроить правила исходящего трафика в Load Balancer (цен. категория "Стандартный") с помощью портал Azure.  

Ресурс балансировщика нагрузки содержит две внешние интерфейсы и связанные с ними правила. У вас есть один внешний интерфейс для входящего трафика и другой внешний интерфейс для исходящего трафика.  

Каждый внешний интерфейс ссылается на общедоступный IP-адрес. В этом сценарии общедоступный IP-адрес для входящего трафика отличается от адреса для исходящего трафика.   Правило балансировки нагрузки обеспечивает только входящую балансировку нагрузки. Правило исходящего трафика управляет преобразованием исходящего сетевого адреса (NAT) для виртуальной машины.  

В сценарии используются два внутренних пула: один для входящего трафика и один для исходящего трафика. Эти пулы иллюстрируют возможности и обеспечивают гибкость для сценария.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу. 

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-load-balancer"></a>Создание балансировщика нагрузки

В этом разделе вы создадите подсистему балансировки нагрузки, которая выполнит балансировку нагрузки виртуальных машин. Можно создать общедоступную подсистему балансировки нагрузки или внутреннюю подсистему балансировки нагрузки. При создании общедоступной подсистемы балансировки нагрузки создается новый общедоступный IP-адрес, настроенный в качестве внешнего интерфейса для балансировщика нагрузки. Интерфейс по умолчанию будет называться **лоадбаланцерфронтенд** .

1. В верхней левой части экрана выберите **создать ресурс** > **Сетевые подключения** > **Load Balancer**.
2. На вкладке **основы** страницы **Создание балансировщика нагрузки** введите или выберите следующие сведения.

    | Параметр                 | Значение                                              |
    | ---                     | ---                                                |
    | Подписка               | Выберите свою подписку.    |    
    | Группа ресурсов         | Выберите **Создать** и в текстовом поле введите **myResourceGroupSLB**.|
    | Имя                   | **myLoadBalancer**                                   |
    | Регион         | Выберите **Западная Европа**.                                        |
    | Тип          | Щелкните **Общедоступный**.                                        |
    | номер SKU           | Выберите **Стандартный** или **Базовый**. Для производственных рабочих нагрузок компания Майкрософт рекомендует использовать цен. категорию "Стандартный". |
    | Общедоступный IP-адрес | Выберите **Создать**. Если у вас есть общедоступный IP-адрес, который вы хотите использовать, выберите **использовать существующий**.  Существующий общедоступный IP-адрес должен быть SKU " **стандартный** ".  Базовые общедоступные IP-адреса несовместимы с подсистемой балансировки нагрузки " **стандартный** ".  |
    | Имя общедоступного IP-адреса              | Введите **myPublicIP** в текстовом поле.|
    | Зона доступности | Выберите **избыточная в зону** , чтобы создать устойчивую Load Balancer. Чтобы создать зональный Load Balancer, выберите определенную зону 1, 2 или 3. |

3. Примите значения по умолчанию для оставшейся части конфигурации.
4. Выберите **Проверка и создать** .

    > [!IMPORTANT]
    > В оставшейся части этого краткого руководства предполагается, что в процессе выбора номера SKU выбирается **стандартный** номер SKU.

5. На вкладке **Отзыв и создание** выберите **Создать**.   

    ![Создание подсистемы балансировки нагрузки уровня "Стандартный"](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Создание ресурсов подсистемы балансировки нагрузки

В этом разделе описано, как настроить параметры подсистемы балансировки нагрузки для внутреннего пула адресов, пробы работоспособности, а также указать правила балансировки нагрузки.

### <a name="create-a-backend-pool"></a>Создание внутреннего пула.

Внутренний пул адресов содержит IP-адреса виртуальных сетевых адаптеров во внутреннем пуле. Создайте внутренний пул адресов **myBackendPool**, чтобы включить виртуальные машины для балансировки нагрузки интернет-трафика.

1. В меню слева щелкните **Все службы**, выберите **Все ресурсы**, а затем из списка ресурсов выберите **myLoadBalancer**.
2. В разделе **Параметры** выберите **Серверные пулы**, затем щелкните **Добавить**.
3. На странице **Добавление серверного пула** введите имя **myBackEndPool** для серверного пула и выберите **Добавить**.

### <a name="create-a-health-probe"></a>Создание пробы работоспособности

Проба работоспособности используется для отслеживания состояния приложения. Зонд работоспособности добавляет или удаляет виртуальные машины из балансировщика нагрузки в соответствии с их ответом на проверки работоспособности. Создайте зонд работоспособности **myHealthProbe**, чтобы отслеживать работоспособность виртуальных машин.

1. В меню слева щелкните **Все службы**, выберите **Все ресурсы**, а затем из списка ресурсов выберите **myLoadBalancer**.
2. В разделе **Параметры** выберите **Пробы работоспособности**, а затем щелкните **Добавить**.
    
    | Параметр | Значение |
    | ------- | ----- |
    | Имя | Введите **myHealthProbe**. |
    | Протокол | Выберите **HTTP**. |
    | Порт | Введите **80**.|
    | Интервал | В качестве **интервала** между попытками выполнения пробы (в секундах) введите **15**. |
    | Пороговое значение сбоя | Выберите **2** в качестве значения **порога состояния неработоспособности** или количества последовательных сбоев пробы, после которых виртуальная машина будет считаться неработоспособной.|
    | | |
4. Нажмите кнопку **OK**.

### <a name="create-a-load-balancer-rule"></a>Создание правила балансировщика нагрузки
Правило балансировщика нагрузки позволяет определить распределение трафика между виртуальными машинами. 

Вы определяете:
 - IP-конфигурация внешнего интерфейса для входящего трафика.
 - Внутренний пул IP-адресов для получения трафика.
 - Требуемый исходный и конечный порты. 

В следующем разделе вы создадите:
 - Правило балансировщика нагрузки **myHTTPRule** для прослушивания порта 80.
 - Интерфейсный **лоадбаланцерфронтенд**.
 - Серверный пул адресов **myBackEndPool** также использует порт 80. 

1. В меню слева щелкните **Все службы**, выберите **Все ресурсы**, а затем из списка ресурсов выберите **myLoadBalancer**.
2. В разделе **Параметры** выберите **Правила балансировки нагрузки**, а затем щелкните **Добавить**.
3. Для настройки правила балансировки нагрузки используйте следующие значения.
    
    | Параметр | Значение |
    | ------- | ----- |
    | Имя | Введите **myHTTPRule**. |
    | Протокол | Выберите **TCP**. |
    | Порт | Введите **80**.|
    | Серверный порт | Введите **80**. |
    | Серверный пул | Выберите **myBackendPool**.|
    | Проверка работоспособности | Выберите **myHealthProbe**. |
    | Создание неявных правил исходящего трафика | Выберите вариант **Нет**. Правила для исходящего трафика будут созданы в более позднем разделе с использованием выделенного общедоступного IP-адреса. |
4. Сохраните остальные значения по умолчанию и нажмите кнопку **ОК**.

## <a name="create-outbound-rule-configuration"></a>Создание конфигурации исходящего правила
Исходящие правила балансировщика нагрузки настройте исходящий SNAT для виртуальных машин в внутреннем пуле. 

### <a name="create-an-outbound-public-ip-address-and-frontend"></a>Создание исходящего общедоступного IP-адреса и внешнего интерфейса

1. В меню слева щелкните **Все службы**, выберите **Все ресурсы**, а затем из списка ресурсов выберите **myLoadBalancer**.

2. В разделе **Параметры**выберите **IP-конфигурация внешнего**интерфейса, а затем нажмите кнопку **добавить**.

3. Используйте эти значения, чтобы настроить интерфейсную IP-конфигурацию для исходящего трафика:

    | Параметр | Значение |
    | ------- | ----- |
    | Имя | Введите **лоадбаланцерфронтендаутбаунд**. |
    | Версия IP | Выберите значение **IPv4**. |
    | Тип IP-адреса | Выберите **IP-адрес**.|
    | Общедоступный IP-адрес | Выберите **Создать**. В поле **добавить общедоступный IP-адрес**введите **мипублиЦипаутбаунд**.  Нажмите кнопку **OK**. |

4. Выберите **Добавить**.

### <a name="create-an-outbound-backend-pool"></a>Создание исходящего внутреннего пула

1. В меню слева щелкните **Все службы**, выберите **Все ресурсы**, а затем из списка ресурсов выберите **myLoadBalancer**.

2. В разделе **Параметры** выберите **Серверные пулы**, затем щелкните **Добавить**.

3. На странице **Добавление внутреннего пула** в поле Имя введите **мибаккендпулаутбаунд**в качестве имени для серверного пула и нажмите кнопку **добавить**.

### <a name="create-outbound-rule"></a>Создание правила для исходящего трафика

1. В меню слева щелкните **Все службы**, выберите **Все ресурсы**, а затем из списка ресурсов выберите **myLoadBalancer**.

2. В разделе **Параметры**выберите **правила для исходящих подключений**, а затем нажмите кнопку **добавить**.

3. Используйте эти значения для настройки правил исходящего трафика:

    | Параметр | Значение |
    | ------- | ----- |
    | Имя | Введите **мйоутбаундруле**. |
    | Интерфейсный IP-адрес | Выберите **лоадбаланцерфронтендаутбаунд**. |
    | Время ожидания простоя (в минутах) | Переместите ползунок в положение * * 15 минут.|
    | Сброс TCP | Выберите **Включено**.|
    | Серверный пул | Выбор **мибаккендпулаутбаунд** |
    | Выделение портов — выделение порта > | Выберите выбрать **количество исходящих портов вручную** |
    | Исходящие порты — > выберите по | Выбор **портов на экземпляр** |
    | Исходящие порты-> портов на экземпляр | Введите **10 000**. |

4. Выберите **Добавить**.

## <a name="clean-up-resources"></a>Очистка ресурсов

Ставшие ненужными группу ресурсов, подсистему балансировки нагрузки и все связанные ресурсы можно удалить. Выберите группу ресурсов **myResourceGroupSLB** , содержащую подсистему балансировки нагрузки, и нажмите кнопку **Удалить**.

## <a name="next-steps"></a>Дальнейшие шаги

В этой статье:
 - Вы создали стандартную подсистему балансировки нагрузки.
 - Настроены как входящие, так и исходящие правила трафика балансировщика нагрузки.
 - Настроена проверка работоспособности для виртуальных машин в пуле серверной части. 

Чтобы узнать больше, перейдите к [руководствам для Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md).
