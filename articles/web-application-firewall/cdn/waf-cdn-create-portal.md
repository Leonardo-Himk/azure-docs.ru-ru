---
title: Руководство по созданию политики WAF для Azure CDN при помощи портала Azure
description: Из этого руководства вы узнаете, как создать политику брандмауэра веб-приложения в Azure CDN с помощью портала Azure.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 03/18/2020
ms.author: victorh
ms.openlocfilehash: 7a9e0cc3977892fd899b4a25e17ad72f13481506
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82608819"
---
# <a name="tutorial-create-a-waf-policy-on-azure-cdn-using-the-azure-portal"></a>Руководство по созданию политики WAF в Azure CDN при помощи портала Azure

В этом руководстве показано, как создать простейшую политику брандмауэра веб-приложения (WAF) и применить ее к конечной точке в Azure CDN.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Создание политики WAF
> * Связывание ее с конечной точкой CDN. Политику WAF можно связать только с конечными точками, размещенными на SKU **Azure CDN уровня "Стандартный" от Майкрософт**.
> * настройка правил WAF.

## <a name="prerequisites"></a>Предварительные требования

Создайте профиль Azure CDN и конечную точку, следуя инструкциям в статье [Краткое руководство по созданию профиля и конечной точки Azure CDN](../../cdn/cdn-create-new-endpoint.md). 

## <a name="create-a-web-application-firewall-policy"></a>Создание политики брандмауэра веб-приложения

Сначала создайте на портале базовую политику WAF с управляемым набором правил по умолчанию (DRS).

1. В левой верхней части экрана выберите **Создать ресурс**, выполните поиск по запросу **WAF**, выберите элемент **Брандмауэр веб-приложения** и щелкните **Создать**.
2. На вкладке **Основные сведения** страницы **Создание политики WAF** введите или выберите указанные ниже сведения, примите значения по умолчанию для остальных параметров и нажмите кнопку **Проверить и создать**.

    | Параметр                 | Значение                                              |
    | ---                     | ---                                                |
    | Политика для следующего:            |Выберите Azure CDN (предварительная версия)|
    | Подписка            |Выберите имя своей подписки Front Door.|
    | Группа ресурсов          |Выберите имя группы ресурсов Front Door.|
    | Имя политики             |Введите уникальное имя для политики WAF.|

   ![Создание политики WAF](../media/waf-cdn-create-portal/basic.png)

3. На вкладке **Ассоциация** страницы **Создание политики WAF** выберите **Добавить конечную точку CDN**, введите указанные ниже сведения, а затем нажмите кнопку **Добавить**.

    | Параметр                 | Значение                                              |
    | ---                     | ---                                                |
    | Профиль сети CDN              | Выберите имя профиля CDN.|
    | Конечная точка           | Выберите имя конечной точки и нажмите кнопку **Добавить**.|
    
    > [!NOTE]
    > Если конечная точка связана с политикой WAF, она отображается серым цветом. Сначала необходимо удалить конечную точку из связанной политики, а затем повторно связать ее с новой политикой WAF.
1. Выберите **Проверить и создать**, а затем выберите **Создать**.

## <a name="configure-web-application-firewall-policy-optional"></a>Настройка политики брандмауэра веб-приложения (необязательно)

### <a name="change-mode"></a>Изменение режима

По умолчанию при создании политики WAF она находится в режиме *Обнаружение*. В режиме *Обнаружение* WAF не блокирует запросы. Вместо этого запросы, соответствующие правилам WAF, регистрируются в журналах WAF.

Чтобы увидеть WAF в действии, можно изменить режим с *Обнаружение* на *Предотвращение*. В режиме *предотвращения* запросы, соответствующие правилам, которые определены в наборе правил по умолчанию (DRS), блокируются и регистрируются в журналах WAF.

 ![Изменение режима политики WAF](../media/waf-cdn-create-portal/policy.png)

### <a name="custom-rules"></a>Настраиваемые правила

Чтобы создать настраиваемое правило, выберите команду **Добавить настраиваемое правило** в разделе **Настраиваемые правила**. Откроется страница настройки настраиваемого правила. Существует два типа настраиваемых правил: **правило сопоставления** и **ограничение скорости**.

На следующем снимке экрана показано настраиваемое правило сопоставления для блокировки запроса, если строка запроса содержит значение **blockme**.

![Изменение режима политики WAF](../media/waf-cdn-create-portal/custommatch.png)

Для правил ограничения скорости требуются два дополнительных поля: **Период ограничения скорости** и **Пороговое значение ограничения скорости (запросов)** , как показано в следующем примере:

![Изменение режима политики WAF](../media/waf-cdn-create-portal/customrate.png)

### <a name="default-rule-set-drs"></a>Набор правил по умолчанию (DRS)

Набор правил по умолчанию, управляемый Azure, включен по умолчанию. Чтобы отключить отдельное правило в группе правил, разверните правила в этой группе, установите флажок перед номером правила и щелкните **Отключить** на вкладке выше. Чтобы изменить типы действий для отдельных правил в наборе правил, установите флажок перед номером правила, а затем выберите вкладку **Изменение действия** выше.

 ![Изменение набора правил WAF](../media/waf-cdn-create-portal/managed2.png)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Что такое брандмауэр веб-приложения Azure?](../overview.md)
