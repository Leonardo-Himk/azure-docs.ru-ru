---
title: Ознакомьтесь с рекомендациями помощника по Azure, которые важны для вас
description: Просмотрите и отфильтруйте рекомендации помощника по Azure, чтобы уменьшить шум.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 10d7b16864f8e449dc51e870c5ff9f20d8c0dc87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75422375"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Ознакомьтесь с рекомендациями помощника по Azure, которые важны для вас

Помощник по Azure предоставляет рекомендации, которые помогут вам оптимизировать развертывания Azure. В рамках помощника вы получите доступ к некоторым функциям, которые помогут вам сократить ваши рекомендации только теми, которые важны для вас.

## <a name="configure-subscriptions-and-resource-groups"></a>Настройка подписок и групп ресурсов

Помощник предоставляет возможность выбора подписок и групп ресурсов, которые важны для вас и вашей организации. Вы увидите только рекомендации по выбранным подпискам и группам ресурсов. По умолчанию выбраны все. Параметры конфигурации применяются к подписке или группе ресурсов, поэтому одни и те же параметры применяются ко всем, у кого есть доступ к этой подписке или группе ресурсов. Параметры конфигурации можно изменить в портал Azure или программно.

Чтобы внести изменения в портал Azure, выполните следующие действия.

1. Откройте [Помощник по Azure](https://aka.ms/azureadvisordashboard) в портал Azure.

1. В меню выберите пункт **Конфигурация** .

   ![Меню конфигурации Advisor](./media/view-recommendations/configuration.png)

1. Установите флажок в столбце **включить** для всех подписок или групп ресурсов, чтобы получать рекомендации помощника. Если флажок отключен, возможно, у вас нет разрешения на внесение изменений в конфигурацию этой подписки или группы ресурсов. Дополнительные сведения о [разрешениях см. в статье помощник по Azure](permissions.md).

1. После внесения изменений нажмите кнопку **Применить** в нижней части.

## <a name="filtering-your-view-in-the-azure-portal"></a>Фильтрация представления в портал Azure

Параметры конфигурации остаются активными до изменения. Если требуется ограничить просмотр рекомендаций для одного просмотра, можно воспользоваться раскрывающимся списком, представленным в верхней части панели Advisor. На панелях обзор, высокая доступность, безопасность, производительность, затраты и все рекомендации можно выбрать подписки, типы ресурсов и состояние рекомендаций, которые нужно просмотреть.

   ![Меню фильтрации Advisor](./media/view-recommendations/filtering.png)

## <a name="dismissing-and-postponing-recommendations"></a>Отклонение и отсрочка рекомендаций

Помощник Azure позволяет отклонять или откладывать рекомендации на одном ресурсе. Если вы откроете рекомендацию, вы не увидите ее повторно, пока она не будет активирована вручную. Однако отсрочка рекомендации позволяет указать длительность, после которой рекомендация будет автоматически активирована снова. Откладывание можно выполнять в портал Azure или программно.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Отложить одну рекомендацию в портал Azure 

1. Откройте [Помощник по Azure](https://aka.ms/azureadvisordashboard) в портал Azure.
1. Выберите категорию рекомендаций, чтобы просмотреть рекомендации
1. Выберите рекомендацию из списка рекомендаций.
1. Выберите отложить или отклонить для рекомендации, которую нужно отложить или отклонить

     ![Меню фильтрации Advisor](./media/view-recommendations/postpone-dismiss.png)

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Отложить или отклонить несколько рекомендаций в портал Azure

1. Откройте [Помощник по Azure](https://aka.ms/azureadvisordashboard) в портал Azure.
1. Выберите категорию рекомендации, чтобы просмотреть рекомендации.
1. Выберите рекомендацию из списка рекомендаций.
1. Установите флажок слева от строки для всех ресурсов, которые необходимо отложить или отклонить.
1. Выберите **отложить** или **Закрыть** в верхнем левом углу таблицы.

     ![Меню фильтрации Advisor](./media/view-recommendations/postpone-dismiss-multiple.png)

> [!NOTE]
> Чтобы отклонить или отложить рекомендацию, требуется разрешение участника или владельца. Дополнительные сведения о разрешениях см. в статье помощник по Azure.

> [!NOTE]
> Если поля выбора отключены, рекомендации по-прежнему могут быть загружены. Дождитесь завершения загрузки всех рекомендаций, прежде чем пытаться отложить или отклонить.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Повторная активация отложенной или отклоненной рекомендации

Вы можете активировать рекомендацию, которая была отложена или отклонена. Это действие можно выполнить в портал Azure или программно. На портале Azure выполните следующие действия.

1. Откройте [Помощник по Azure](https://aka.ms/azureadvisordashboard) в портал Azure.

1. Измените фильтр на панели Обзор, чтобы она была **отложена**. Затем помощник отображает отложенные или Отклоненные рекомендации.

    ![Меню фильтрации Advisor](./media/view-recommendations/activate-postponed.png)

1. Выберите категорию, чтобы просмотреть **отложенные** и **отклоненные** рекомендации.

1. Выберите рекомендацию из списка рекомендаций. Откроется вкладка **отложенная &** отключена, чтобы отобразить ресурсы, для которых эта рекомендация была отложена или отклонена.

1. Щелкните **активировать** в конце строки. После нажатия рекомендации для этого ресурса активна и поэтому удаляются из этой таблицы. Теперь рекомендация отображается на **активной** вкладке.
 
     ![Меню фильтрации Advisor](./media/view-recommendations/activate-postponed-2.png)

## <a name="next-steps"></a>Дальнейшие действия

В этой статье объясняется, как можно просмотреть рекомендации, которые важны для вас в помощнике по Azure. Дополнительные сведения о Помощнике см. в таких разделах. 

- [Что такое Помощник по Azure?](advisor-overview.md)
- [начало работы с помощником](advisor-get-started.md)
- [Разрешения в Azure Advisor](permissions.md)



