---
title: Оптимизация затрат на Azure Databricks с помощью предварительного приобретения
description: Узнайте, как можно предварительно оплачивать расходы Azure Databricks с помощью резервной мощности,чтобы сэкономить средства.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: b000934bb218c21a454ed04afe4e9a4495e808ab
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "77200390"
---
# <a name="optimize-azure-databricks-costs-with-a-pre-purchase"></a>Оптимизация затрат на Azure Databricks с помощью предварительного приобретения

Вы можете сэкономить затраты на единицу Azure Databricks (DBU), если предварительно приобретете зафиксированные единицы Azure Databricks (DBCU) на один или три года. Предварительно приобретенные DBCU можно использовать в любое время в течение срока покупки. В отличие от виртуальных машин, срок действия предварительно приобретенных единиц не истекает каждый час, а также их можно использовать в любое время в течение срока покупки.

Все Azure Databricks автоматически используют вычеты из предварительно приобретенных DBU. Вам не нужно повторно развертывать или назначать предварительно приобретенный план для рабочих областей Azure Databricks для потребления DBU, чтобы получить скидки, связанные с предварительной покупкой.

Скидка на предварительную покупку применяется только к использованию DBU. Другие расходы, такие как вычисления, хранение и сетевое подключение, взимаются отдельно.

## <a name="determine-the-right-size-to-buy"></a>Определение правильного размера для приобретения

Предварительная покупка Databricks применяется ко всем рабочим нагрузкам и уровням Databricks. Предварительную покупку можно рассматривать как пул предварительно оплаченных зафиксированных единиц Databricks. Потребление вычитается из пула, независимо от рабочей нагрузки или уровня. Потребление вычитается в следующем соотношении:

| **Рабочая нагрузка** | **Соотношение приложений DBU — уровень Standard** | **Соотношение приложений DBU — уровень Premium** |
| --- | --- | --- |
| Аналитика данных | 0,4 | 0,55 |
| Инжиниринг данных | 0,15 | 0,30 |
| Инжиниринг данных с облегченной нагрузкой | 0,07 | 0,22 |

Например, если используется уровень аналитики данных "Standard", то фиксированные единицы предварительной покупки Databricks вычитаются на 0,4 единицы.

Перед покупкой вычислите общее используемое количество DBU для различных рабочих нагрузок и уровней. Используйте приведенные выше соотношения для нормализации до DBCU, а затем запустите проекцию общего использования за один или три года.

## <a name="purchase-databricks-commit-units"></a>Фиксированные единицы покупки Databricks (DBCU)

Планы Databricks можно приобрести на [портале Azure](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D). Чтобы приобрести резервную мощность, необходимо иметь роль "Владелец" по крайней мере для одной подписки Enterprise.

- Необходимо иметь роль "Владелец" по крайней мере для одного Соглашения Enterprise (номера предложений: MS-AZR-0017P или MS-AZR-0148P) или Клиентского соглашения Майкрософт либо отдельную подписку с оплатой по мере использования (номера предложений: MS-AZR-0003P или MS-AZR 0023P).
- Для подписок с Соглашением Enterprise параметр "Добавить зарезервированные экземпляры" следует включить на портале EA. Если этот параметр отключен, необходимо быть администратором подписки EA.
- Для подписок с соглашением Enterprise параметр **Добавить зарезервированные экземпляры** следует включить на [портале EA](https://ea.azure.com/). Или, если этот параметр отключен, необходимо быть администратором подписки EA.

**Для покупки:**

1. Перейдите на [портал Azure](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D).
1. Выберите подписку. Используйте список **Подписка** для выбора подписки, которая используется для оплаты резервной мощности. Резервная мощность оплачивается предварительно с помощью метода оплаты, указанного для подписки. Плата вычитается из баланса денежных обязательств или относится к избыточным расходам.
1. Выберите область. Используйте список **Область** для выбора диапазона подписки:
    - **Одна группа ресурсов** — скидка по резервированию применяется к подходящим ресурсам только в выбранной группе ресурсов.
    - **Одна подписка** — скидка по резервированию применяется к подходящим ресурсам только в выбранной подписке.
    - **Общая область резервирования** — скидка по резервированию применяется к подходящим ресурсам во всех допустимых подписках в контексте выставления счетов. Для клиентов Соглашения Enterprise контекстом выставления счетов считается регистрация.
1. Выберите количество единиц измерения Azure Databricks, которые требуется приобрести, и выполните покупку.


![Пример покупки Azure Databricks на портале Azure](./media/prepay-databricks-reserved-capacity/data-bricks-pre-purchase.png)

## <a name="change-scope-and-ownership"></a>Изменение области резервирования и владения

После приобретения в резервирование можно внести следующие типы изменений:

- Обновить область резервирования
- Доступ на основе ролей

Вы не можете разделить или объединить предварительную покупку фиксированной единицы Databricks. Дополнительные сведения об управлении резервированиями см. в статье[Manage reservations after purchase](manage-reserved-vm-instance.md) (Управление резервированиями после покупки).

## <a name="cancellations-and-exchanges"></a>Отмена и обмен

Отмена и обмен не поддерживаются для планов предварительной покупки Databricks. Все покупки являются окончательными.

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о резервировании в Azure см. по следующим ссылкам:
  - [Основные сведения о резервировании в Azure](save-compute-costs-reservations.md)
  - [Understand how an Azure Databricks pre-purchase DBCU discount is applied](reservation-discount-databricks.md) (О применении скидки предварительной покупки Azure Databricks)
  - [Общие сведения об использовании зарезервированных экземпляров Azure с Соглашением о регистрации Enterprise](understand-reserved-instance-usage-ea.md)
