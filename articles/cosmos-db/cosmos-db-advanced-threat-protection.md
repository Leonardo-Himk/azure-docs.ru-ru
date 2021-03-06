---
title: Расширенная защита от угроз для Azure Cosmos DB
description: Узнайте, как Azure Cosmos DB обеспечивает шифрование неактивных данных и их реализацию.
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: seodec18
ms.author: memildin
author: memildin
manager: rkarlin
ms.openlocfilehash: bcc1c6ffe7cdec4aed325a67969235ae993a5109
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77614838"
---
# <a name="advanced-threat-protection-for-azure-cosmos-db-preview"></a>Расширенная защита от угроз для Azure Cosmos DB (Предварительная версия)

Расширенная защита от угроз для Azure Cosmos DB предоставляет дополнительный уровень логики безопасности, который обнаруживает необычные и потенциально опасные попытки доступа или использования учетных записей Azure Cosmos DB. Этот уровень защиты позволяет решать угрозы даже без эксперта по безопасности и интегрировать их с системами мониторинга централизованной безопасности.

Оповещения системы безопасности активируются при возникновении аномалий в действии. Эти оповещения системы безопасности интегрируются с [центром безопасности Azure](https://azure.microsoft.com/services/security-center/), а также отправляются по электронной почте администраторам подписки с подробными сведениями о подозрительных действиях и рекомендациями по исследованию и исправлению угроз.

> [!NOTE]
>
> * Расширенная защита от угроз для Azure Cosmos DB в настоящее время доступна только для API SQL.
> * Расширенная защита от угроз для Azure Cosmos DB в настоящее время недоступна в регионах Azure для государственных организаций и независимых.

Для полного исследования оповещений системы безопасности рекомендуется включить [ведение журнала диагностики в Azure Cosmos DB, в](https://docs.microsoft.com/azure/cosmos-db/logging)котором регистрируются операции с самой базой данных, включая операции CRUD для всех документов, контейнеров и баз данных.

## <a name="threat-types"></a>Типы угроз

Расширенная защита от угроз для Azure Cosmos DB обнаруживает аномальные действия, указывающие на необычные и потенциально опасные попытки доступа к базам данных или их использования. В настоящее время он может активировать следующие оповещения:

- **Доступ из необычных расположений**. это оповещение активируется при изменении шаблона доступа к учетной записи Azure Cosmos, когда кто-то подключается к конечной точке Azure Cosmos DB из необычного географического расположения. В некоторых случаях предупреждение обнаруживает допустимое действие, означающее новую операцию обслуживания приложения или разработчика. В других случаях оповещение обнаруживает вредоносные действия от предыдущего сотрудника, внешнего злоумышленника и т. д.

- **Необычное извлечение данных**. это оповещение активируется, когда клиент извлекает необычное количество данных из учетной записи Azure Cosmos DB. Это может быть симптомом некоторых утечка данных, выполняемых для пересылки всех данных, хранящихся в учетной записи, во внешнее хранилище данных.

## <a name="set-up-advanced-threat-protection"></a>Настройка расширенной защиты от угроз

### <a name="set-up-atp-using-the-portal"></a>Настройка ATP с помощью портала

1. Запустите портал Azure по адресу [https://portal.azure.com](https://portal.azure.com/).

2. В учетной записи Azure Cosmos DB в меню **Параметры** выберите пункт **Расширенная безопасность**.

    ![Настройка ATP](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp.png)

3. В колонке **Расширенная конфигурация безопасности** :

    * Выберите параметр **Advanced Threat protection** ( **вкл**.), чтобы включить его.
    * Щелкните **Сохранить**, чтобы сохранить новую или обновленную политику Расширенной защиты от угроз.   

### <a name="set-up-atp-using-rest-api"></a>Настройка ATP с помощью REST API

Используйте команды API-интерфейса для создания, обновления или получения параметра Advanced Threat Protection для конкретной учетной записи Azure Cosmos DB.

* [Расширенная защита от угроз — создание](https://go.microsoft.com/fwlink/?linkid=2099745)
* [Расширенная защита от угроз — получение](https://go.microsoft.com/fwlink/?linkid=2099643)

### <a name="set-up-atp-using-azure-powershell"></a>Настройка ATP с помощью Azure PowerShell

Используйте следующие командлеты PowerShell:

* [Включение службы "Расширенная защита от угроз"](https://go.microsoft.com/fwlink/?linkid=2099607&clcid=0x409)
* [Получить расширенную защиту от угроз](https://go.microsoft.com/fwlink/?linkid=2099608&clcid=0x409)
* [Отключить Advanced Threat protection](https://go.microsoft.com/fwlink/?linkid=2099709&clcid=0x409)

### <a name="using-azure-resource-manager-templates"></a>Использование шаблонов диспетчера ресурсов Azure

Используйте шаблон Azure Resource Manager, чтобы настроить Cosmos DB с включенной Advanced Threat protection.
Дополнительные сведения см. [в статье Создание учетной записи CosmosDB с помощью Advanced Threat protection](https://azure.microsoft.com/resources/templates/201-cosmosdb-advanced-threat-protection-create-account/).

### <a name="using-azure-policy"></a>Использование политики Azure

Используйте политику Azure, чтобы включить расширенную защиту от угроз для Cosmos DB.

1. Откройте страницу " **политики Azure — определения** " и выполните поиск по запросу политика **развертывания Advanced Threat Protection для Cosmos DB** .

    ![Поиск политики](./media/cosmos-db-advanced-threat-protection/cosmos-db.png) 

1. Щелкните **развертывание Advanced Threat Protection для CosmosDB** , а затем нажмите кнопку **назначить**.

    ![Выберите подписку или группу](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-policy.png)


1. В поле **область** щелкните три точки, выберите подписку Azure или группу ресурсов и нажмите кнопку **выбрать**.

    ![Страница "определения политик"](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-details.png)


1. Введите другие параметры и нажмите кнопку **назначить**.

## <a name="manage-atp-security-alerts"></a>Управление оповещениями системы безопасности ATP

При возникновении Azure Cosmos DB аномалий действий создается оповещение системы безопасности с информацией о подозрительной событии безопасности. 

 В центре безопасности Azure можно просматривать текущие [оповещения системы безопасности](../security-center/security-center-alerts-overview.md)и управлять ими.  Щелкните определенное оповещение в [центре безопасности](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0) , чтобы просмотреть возможные причины и рекомендуемые действия для изучения и устранения потенциальных угроз. На следующем рисунке показан пример сведений о предупреждении, предоставленных в центре безопасности.

 ![Сведения об угрозе](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert-details.png)

Также отправляется уведомление по электронной почте с подробными сведениями о предупреждении и рекомендуемыми действиями. На следующем рисунке показан пример электронного сообщения с оповещением.

 ![Сведения об оповещении](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert.png)

## <a name="cosmos-db-atp-alerts"></a>Оповещения Cosmos DB ATP

 Список предупреждений, созданных при наблюдении за учетными записями Azure Cosmos DB, см. в разделе [Cosmos DB Alerts](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurecosmos) в документации по центру безопасности Azure.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о [ведении журналов диагностики в Azure Cosmos DB](cosmosdb-monitor-resource-logs.md)
* Дополнительные сведения о [центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
