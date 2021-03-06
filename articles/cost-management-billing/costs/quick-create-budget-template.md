---
title: Краткое руководство. Создание бюджета с помощью шаблона Azure Resource Manager
description: Краткое руководство по созданию бюджета с помощью шаблона Azure Resource Manager.
author: bandersmsft
ms.author: banders
tags: azure-resource-manager
ms.service: cost-management-billing
ms.topic: quickstart
ms.date: 04/22/2020
ms.custom: subject-armqs
ms.openlocfilehash: de24895334ec4c864e6daae84a6aab47a47d7b9b
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2020
ms.locfileid: "82103639"
---
# <a name="quickstart-create-a-budget-with-an-azure-resource-manager-template"></a>Краткое руководство. Создание бюджета с помощью шаблона Azure Resource Manager

Бюджеты в службе "Управление затратами" помогают планировать и отслеживать отчетность на уровне организации. С бюджетами вы можете учитывать службы Azure, которые используете или на которые подписываетесь в течение определенного периода. Они помогают сообщать другим пользователям о своих расходах, чтобы эффективно управлять затратами и контролировать то, как они возрастают с течением времени. Когда созданные вами пороговые значения бюджета превышены, активируются уведомления. Ни один из ваших ресурсов не затронут а потребление не остановлено. Анализируя затраты, можно использовать бюджеты для их сравнения и отслеживания. В этом кратком руководстве описано, как создать бюджет с помощью шаблона Resource Manager.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Предварительные требования

Шаблон Azure Resource Manager поддерживает только подписки Azure для Соглашений Enterprise (EA). Другие типы подписок этот шаблон не поддерживает.

А для создания и администрирования бюджетов требуется разрешение уровня участника. Вы можете создать отдельные бюджеты для подписок EA и групп ресурсов. Однако вы не можете создать бюджеты для учетных записей выставления счетов EA. Для подписок Azure EA вам необходимо иметь доступ на чтение для просмотра данных о бюджете.

После создания бюджета для его просмотра нужен как минимум доступ на чтение для учетной записи Azure.

Если вы используете новую подписку, вы не сможете сразу создать бюджет или использовать другие функции службы "Управление затратами". Полная активация всех функций Управления затратами может потребовать до 48 часов.

На подписку для бюджетов по пользователям и группам поддерживаются следующие разрешения или области Azure. См. [основные сведения об областях и работе с ними](understand-work-scopes.md).

- Владелец может создавать, изменять или удалять бюджеты для подписки.
- Участник и участник службы "Управление затратами" может создавать, изменять или удалять свои собственные бюджеты. Можно изменить сумму бюджета для бюджетов, созданных другими пользователями.
- Читатель и читатель службы "Управление затратами" может просматривать бюджеты, к которым имеет доступ.

Дополнительные сведения о назначении разрешений на доступ к данным службы "Управление затратами" см. в [этой статье](assign-access-acm-data.md).

## <a name="review-the-template"></a>Изучение шаблона

Шаблон, используемый в этом кратком руководстве, взят из [шаблонов быстрого запуска Azure](https://azure.microsoft.com/resources/templates/create-budget).

:::code language="json" source="~/quickstart-templates/create-budget/azuredeploy.json":::

В этом шаблоне определяется один ресурс Azure.

* [Microsoft.Consumption/budgets](/azure/templates/microsoft.consumption/budgets): создание бюджета Azure.

## <a name="deploy-the-template"></a>Развертывание шаблона

1. Выберите следующее изображение, чтобы войти на портал Azure и открыть шаблон. Шаблон создаст бюджет.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json"><img src="./media/quick-create-budget-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Введите или выберите следующие значения.

   [![Шаблон Resource Manager, создание бюджета, развертывание на портале](./media/quick-create-budget-template/create-budget-using-template-portal.png)](./media/quick-create-budget-template/create-budget-using-template-portal.png#lightbox)

    * **Подписка**. Выберите нужную подписку Azure.
    * **Группа ресурсов**. Щелкните **Создать**, введите уникальное имя группы ресурсов и нажмите кнопку **ОК** либо выберите имеющуюся группу ресурсов.
    * **Расположение**. Выберите расположение. Например, **центральная часть США**.
    * **Имя бюджета**. Введите имя для этого бюджета. Оно должно быть уникальным в пределах группы ресурсов. Допустимы только буквы, цифры, знаки подчеркивания и дефисы.
    * **Сумма**. Введите общий объем затрат или потребления, который будет отслеживаться этим бюджетом.
    * **Категория бюджета**. В качестве категории бюджета выберите, будет ли в нем отслеживаться **Стоимость** или **Потребление**.
    * **Интервал времени**. Введите период времени, охватываемый этим бюджетом. Допустимые значения: ежемесячно, ежеквартально или ежегодно. По истечении интервала времени сумма бюджета сбрасывается.
    * **Дата начала**. Введите дату начала, которая соответствует первому дню месяца в формате ГГГГ-ММ-ДД. Дата начала в будущем должна быть не позже, чем через три месяца от текущей даты. Вы можете указать дату начала в прошлом в сочетании с интервалом времени.
    * **Дата окончания**. Введите дату окончания учета бюджета в формате ГГГГ-ММ-ДД. Если эта дата не указана, по умолчанию устанавливается окончание через 10 лет после даты начала.
    * **Оператор**. Выберите оператор сравнения. Допустимые варианты: EqualTo, GreaterThan и GreaterThanOrEqualTo.
    * **Пороговое значение**. Введите пороговое значение для уведомлений. Уведомление отправляется, когда накопленная стоимость превысит пороговое значение. Это значение всегда выражается в процентах и должно находиться в диапазоне от 0 до 1000.
    * **Contact Emails** (Адреса электронной почты для связи). Введите список адресов электронной почты, на которые будет отправлено уведомление о превышении порогового значения для бюджета. Ожидается формат `["user1@domain.com","user2@domain.com"]`.
    * **Contact Roles** (Роли для связи). Введите список ролей для связи, которым будет отправлено уведомление о превышении порогового значения для бюджета. По умолчанию поддерживаются роли владельца, участника и читателя. Ожидается формат `["Owner","Contributor","Reader"]`.
    * **Contact Groups** (Группы для связи). Введите список групп действий, в которые будет отправлено уведомление о превышении порогового значения для бюджета. Здесь принимается массив строк. Ожидается формат `["Action Group Name1","Action Group Name2"]`. Если вы не хотите использовать группы действий, введите значение `[]`.
    * **Resources Filter** (Фильтр ресурсов). Введите список фильтров для ресурсов. Ожидается формат `["Resource Filter Name1","Resource Filter Name2"]`. Если вы не хотите применять фильтр, введите значение `[]`. Если вы ввели фильтр ресурсов, обязательно укажите и значения **фильтров показателей**.
    * **Meters Filter** (Фильтр показателей). Введите список фильтров по показателям, который является обязательным для бюджетов с категорией **Потребление**. Ожидается формат `["Meter Filter Name1","Meter Filter Name2"]`. Если вы не указали **фильтр ресурсов**, введите значение `[]`.
    * **I agree to the terms and conditions state above** (Я принимаю указанные выше условия). Установите этот флажок.

3. Щелкните **Приобрести**. После успешного развертывания бюджета вы получите уведомление:

   ![Шаблон Resource Manager: уведомление о развертывании бюджета на портале](./media/quick-create-budget-template/resource-manager-template-portal-deployment-notification.png)

Для развертывания шаблона используется портал Azure. Помимо портала Azure можно также использовать Azure PowerShell, Azure CLI и REST API. Сведения о других шаблонах развертывания см. в [этой статье](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="validate-the-deployment"></a>Проверка развертывания

С помощью портала Azure вы можете убедиться, что бюджет успешно развернут. Для этого выберите **Управление затратами и выставление счетов** > область действия > **Бюджеты**. Также для просмотра бюджета вы можете использовать следующие скрипты в Azure CLI или Azure PowerShell.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
az consumption budget list
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Get-AzConsumptionBudget
```

---

## <a name="next-steps"></a>Дальнейшие действия

Из этого краткого руководства вы узнали, как создать бюджет Azure для развертывания. Дополнительные сведения об управлении затратами и выставлением счетов в Azure и Azure Resource Manager см. в статьях ниже.

- Изучите обзорные сведения об [управлении затратами и выставлении счетов](../cost-management-billing-overview.md).
- [Создайте бюджеты](tutorial-acm-create-budgets.md) на портале Azure.
- Сведения об [Azure Resource Manager](../../azure-resource-manager/management/overview.md)