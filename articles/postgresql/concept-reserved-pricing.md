---
title: Зарезервированные цены на вычисления — база данных Azure для PostgreSQL — один сервер
description: Предоплата за базу данных Azure для PostgreSQL. Вычисление ресурсов с зарезервированной емкостью
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/02/2020
ms.openlocfilehash: 7f671e2a77a0a00fd1cc4338e29c14f7b8fca4f2
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2020
ms.locfileid: "82734728"
---
# <a name="prepay-for-azure-database-for-postgresql-compute-resources-with-reserved-capacity"></a>Предоплата за базу данных Azure для PostgreSQL. Вычисление ресурсов с зарезервированной емкостью

Служба "база данных Azure для PostgreSQL" теперь позволяет экономить деньги за счет предоплаты за ресурсы вычислений по сравнению с ценами на оплату по мере использования. С зарезервированной емкостью базы данных Azure для PostgreSQL вы выполняете предварительное обязательство на сервере PostgreSQL в течение одного или трех лет, чтобы получить существенную скидку на затраты на вычисление. Чтобы приобрести базу данных Azure для зарезервированной емкости PostgreSQL, необходимо указать регион Azure, тип развертывания, уровень производительности и термин. </br>

Вам не нужно назначать резервирование конкретным серверам базы данных Azure для PostgreSQL. Уже запущенная база данных Azure для PostgreSQL или только что развернутая служба автоматически получит преимущества зарезервированных цен. Приобретая резервирование, вы предварительно оплачиваете стоимость вычислений на один или три года. Как только вы купите резервирование, стоимость вычислений для базы данных Azure для PostgreSQL, которая соответствует атрибутам резервирования, больше не взимается по тарифам с оплатой по мере использования. Резервирование не охватывает программное обеспечение, сети или расходы на хранение, связанные с серверами базы данных PostgreSQL. По окончании срока резервирования срок выставления счетов истекает, а база данных Azure для PostgreSQL оплачивается по цене оплаты по мере использования. Резервирования не возобновляются автоматически. Сведения о ценах см. в [предложении "база данных Azure для зарезервированных ресурсов PostgreSQL](https://azure.microsoft.com/pricing/details/postgresql/)". </br>

> [!IMPORTANT]
> Цены на зарезервированные ресурсы доступны только для развертывания с [одним сервером](https://docs.microsoft.com/azure/postgresql/overview#azure-database-for-postgresql---single-server) в базе данных Azure для PostgreSQL, а не для развертывания [Цитус](https://docs.microsoft.com/azure/postgresql/overview#azure-database-for-postgresql---hyperscale-citus) .

Вы можете купить зарезервированную емкость базы данных Azure для PostgreSQL в [портал Azure](https://portal.azure.com/). Платите за резервирование [наперед или ежемесячными платежами](../cost-management-billing/reservations/monthly-payments-reservations.md). Чтобы приобрести зарезервированную емкость, сделайте следующее:

* Необходимо быть в роли владельца по крайней мере для одной корпоративной или отдельной подписки с тарифами с оплатой по мере использования.
* Для подписок с соглашением Enterprise параметр **Добавить зарезервированные экземпляры** следует включить на [портале EA](https://ea.azure.com/). Или, если этот параметр отключен, необходимо быть администратором подписки EA.
* Для программы поставщика облачных решений (CSP) только агенты администратора или агенты по продажам могут приобрести зарезервированную емкость базы данных Azure для PostgreSQL. </br>

Сведения о том, как корпоративные клиенты и клиенты с оплатой по мере использования оплачиваются по покупкам резервирования, см. в статье [Использование резервирования Azure для регистрации на предприятии](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) и [сведения об использовании резервирования Azure для подписки с оплатой по мере использования](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).


## <a name="determine-the-right-server-size-before-purchase"></a>Определение правильного размера сервера перед покупкой

Размер резервирования должен основываться на общем объеме вычислений, используемом существующими или вскоре развернутыми серверами в определенном регионе, с использованием того же уровня производительности и поколения оборудования.</br>

Например, предположим, что вы используете одну общую цель, го поколения – 32 Виртуальное ядро PostgreSQL Database и две оптимизированные для памяти базы данных го поколения – 16 Виртуальное ядро PostgreSQL. Кроме того, предположим, что вы планируете развертывание в течение следующего месяца, а не в общем, го поколения – 32 Виртуальное ядро Database Server и один оптимизированный для памяти сервер базы данных го поколения – 16 Виртуальное ядро. Предположим, вам известно, что эти ресурсы понадобятся не менее 1 года. В этом случае необходимо приобрести 64 (2x32) виртуальных ядер, зарезервированное 1 год для одной базы данных общего назначения — го поколения и 48 (2x16 + 16) Виртуальное ядро 1 год для одной базы данных, оптимизированной для памяти — го поколения


## <a name="buy-azure-database-for-postgresql-reserved-capacity"></a>Купить зарезервированную емкость базы данных Azure для PostgreSQL

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Выберите **все службы** > **резервирования**.
3. Выберите **Добавить** , а затем на панели резервирования покупки выберите **база данных Azure для PostgreSQL** , чтобы приобрести новое резервирование для баз данных PostgreSQL.
4. Заполните поля, необходимые для заполнения полей. Существующие или новые базы данных, соответствующие выбранным атрибутам, будут получать зарезервированную скидку емкости. Фактическое число серверов базы данных Azure для PostgreSQL, которые получают скидку, зависит от выбранной области и количества.


![Общие сведения о зарезервированных ценах](media/concepts-reserved-pricing/postgresql-reserved-price.png)


В следующей таблице описываются обязательные поля.

| Поле | Описание |
| :------------ | :------- |
| Подписка   | Подписка, используемая для оплаты резервирования зарезервированных ресурсов в базе данных Azure для PostgreSQL. В качестве способа оплаты по подписке взимается предварительные затраты для резервирования зарезервированной емкости базы данных Azure для PostgreSQL. Тип подписки должен быть соглашением Enterprise (номера предложения: MS-AZR-0017P или MS-AZR-0148P) или отдельное соглашение с ценами с оплатой по мере использования (номера предложений: MS-AZR-0003P или MS-AZR-0023P). Для подписки с соглашением Enterprise плата вычитается из баланса денежных обязательств или относится к избыточным расходам. Для отдельной подписки с оплатой по мере использования плата взимается по методу оплаты кредитной карты или счета-фактуры в подписке.
| Область | Область резервирования виртуальных ядер может охватывать одну или несколько подписок (общая область). Если выбрать: </br></br> В **общем**, скидка на резервирование Виртуальное ядро применяется к серверам базы данных Azure для PostgreSQL, работающих в любой подписке в контексте выставления счетов. Для клиентов с соглашением Enterprise общая область действует в рамках регистрации и включает в себя все подписки, заданные при регистрации. Для клиентов с подпиской с оплатой по мере использования общая область включает в себя все подписки с оплатой по мере использования, созданные администратором учетной записи.</br></br> **Одна подписка**, скидка на резервирование Виртуальное ядро применяется к серверам базы данных Azure для PostgreSQL в этой подписке. </br></br> **Одна группа ресурсов**. скидка на резервирование применяется к серверам базы данных Azure для PostgreSQL в выбранной подписке и выбранной группе ресурсов в этой подписке.
| Регион | Регион Azure, который охватывает зарезервированную емкость резервирования базы данных Azure для PostgreSQL.
| Тип развертывания | Тип ресурса базы данных Azure для PostgreSQL, для которого требуется приобрести резервирование.
| Уровень производительности | Уровень служб для серверов базы данных Azure для PostgreSQL.
| Термин | Один год
| Количество | Объем ресурсов вычислений, приобретаемых в базе данных Azure для резервирования зарезервированной емкости PostgreSQL. Количество — это число виртуальных ядер в выбранном регионе Azure и уровне производительности, которые будут зарезервированы и получат скидку при выставлении счетов. Например, если вы используете или планируете запускать базу данных Azure для серверов PostgreSQL с общей емкостью вычислений го поколения 16 виртуальных ядер в регионе "Восточная часть США", укажите для параметра Quantity значение 16, чтобы максимально увеличить преимущество для всех серверов.

## <a name="cancel-exchange-or-refund-reservations"></a>Отмена, обмен резервирования, возмещение средств за резервирование

Вы можете отменить и обменять резервирования, а также вернуть вложенные в резервирование средства, но при этом применяются определенные ограничения. Дополнительные сведения см. в статье [самостоятельная служба Exchange и возмещение для резервирования Azure](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="vcore-size-flexibility"></a>Гибкость размеров для виртуального ядра

Гибкость размеров виртуального ядра позволяет увеличивать или уменьшать масштаб в пределах уровня производительности и региона, не теряя преимуществ приобретенной резервной мощности. 

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Дальнейшие действия

Скидка на резервирование Виртуальное ядро автоматически применяется к числу серверов базы данных Azure для PostgreSQL, которые соответствуют области резервирования и атрибутам базы данных Azure для зарезервированной емкости PostgreSQL. Вы можете обновить область базы данных Azure для резервирования зарезервированных ресурсов PostgreSQL с помощью портал Azure, PowerShell, CLI или API. </br></br>
Чтобы узнать, как управлять зарезервированной емкостью базы данных Azure для PostgreSQL, см. статью Управление зарезервированной емкостью базы данных Azure для PostgreSQL.

Дополнительные сведения о резервировании в Azure см. по следующим ссылкам:

* [Что такое резервирование Azure](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)?
* [Управление Azure Reserved VM Instances](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
* [Сведения о скидках на резервирование Azure](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
* [Общие сведения об использовании резервирования Azure для подписки с оплатой по мере использования](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-postgresql)
* [Общие сведения об использовании зарезервированных экземпляров Azure с Соглашением о регистрации Enterprise](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
* [Приобретение зарезервированных экземпляров Azure](https://docs.microsoft.com/partner-center/azure-reservations)
