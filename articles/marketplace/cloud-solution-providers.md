---
title: Поставщики облачных решений | Azure Marketplace
description: Теперь издатели могут продавать свои предложения через канал партнера поставщика решений (CSP) Microsoft Cloud.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/16/2020
ms.author: dsindona
ms.openlocfilehash: b962610c585df288a9cb3297ed8e09c8abc5ac0a
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2020
ms.locfileid: "82160653"
---
# <a name="cloud-solution-providers"></a>Поставщики облачных решений

Предложения программного обеспечения могут достигать миллионов квалифицированных клиентов Майкрософт, обслуживаемых партнерами в программе поставщика облачных решений (CSP), а также общедоступных предложений через [Интернет-магазины Майкрософт](https://docs.microsoft.com/azure/marketplace/comparing-appsource-azure-marketplace).

Издатели настраивают предложения для обеспечения доступности в программе CSP на основе согласия, нового предложения или существующего, позволяя партнерам продавать ваши продукты и создавать Объединенные решения для клиентов.

Издатели отвечают за предоставление поддержки по устранению неисправностей для конечных клиентов и на предоставление партнерам программы CSP и/или клиентов для связи с вами. Рекомендуется предоставлять партнерам в программе CSP сведения о пользовательской документации, обучении, а также уведомления о работоспособности и сбоях службы (как применимо), чтобы партнеры в программе CSP могли обслуживать запросы на поддержку уровня 1 от клиентов.  

Следующие предложения могут быть включены для продажи партнерами в программе CSP:

- Предложения по языку "программное обеспечение как услуга" (SaaS)
- Виртуальные машины
- Шаблоны решений
- Управляемые приложения

> [!NOTE]
> Контейнеры и виртуальные машины с собственными лицензиями (BYOL) по умолчанию участвуют в участии в программе CSP.

## <a name="how-to-configure-an-offering"></a>Настройка предложения

Параметр "участие в программе CSP" настраивается в центре партнеров или Портал Cloud Partner предлагает возможность создания. Дополнительные [сведения об изменении взаимодействия с издателем](https://www.microsoftpartnercommunity.com/t5/Azure-Marketplace-and-AppSource/Cloud-Marketplace-In-Partner-Center/m-p/9738#M293).

### <a name="partner-center-opt-in"></a>Согласие на участие в центре партнеров

В центре партнеров вы найдете сведения о функции участия в модуле "аудитория торгового посредника CSP".

![Аудитория торгового посредника CSP](media/marketplace-publishers-guide/csp-reseller-audience.png)

В модуле "аудитория торгового посредника CSP" можно выбрать один из трех вариантов:

- Вариант 1. любой партнер в программе CSP
- Вариант 2. конкретные партнеры в выбранном программном процессе CSP
- Вариант 3. нет партнеров в программе CSP

#### <a name="option-one-any-partner-in-the-csp-program"></a>Вариант 1. любой партнер в программе CSP

![Любой партнер в программе CSP](media/marketplace-publishers-guide/csp-reseller-option-one.png)

 При выборе этого параметра все партнеры в программе CSP могут перепродавать свое предложение своим клиентам.

#### <a name="option-two-specific-partners-in-the-csp-program-i-select"></a>Вариант 2. конкретные партнеры в выбранном программном процессе CSP

![Выбранные партнеры в программе CSP, которую я выберу](media/marketplace-publishers-guide/csp-reseller-option-two.png)

Если выбрать этот параметр, вы решите, какие партнеры в программе CSP могут перепродать свое предложение.

Чтобы авторизовать партнеров, щелкните **выбрать партнеров CSP** и появится меню, позволяющее выполнять поиск по имени партнера или идентификатору клиента Azure Active Directory CSP (AAD).

![Выбрать меню CSP](media/marketplace-publishers-guide/csp-pop-up-module.png)

Можно применять фильтры поиска, такие как **страна**, **компетенция**или **навык**.

![Фильтры по странам, компетенции и навыкам для поиска партнеров](media/marketplace-publishers-guide/csp-add-resellers.png)

Выбрав список партнеров, нажмите кнопку **Добавить**.

![Пример списка полномочных партнеров в программе CSP](media/marketplace-publishers-guide/csp-add-resellers-details.png)

В таблице, отображающей выбранный список партнеров, отображается страница "аудитория поставщика CSP".

![Таблица со списком партнеров на странице "аудитория торгового посредника CSP"](media/marketplace-publishers-guide/csp-option-two-add-reseller.png)

Выберите **Сохранить черновик** , чтобы зарегистрировать изменения.

Если это предложение не опубликовано, необходимо опубликовать предложение, чтобы оно было доступно выбранным партнерам.

>[!NOTE]
>Если вы авторизуете партнера в программе CSP в определенном регионе, он может продать предложение любому клиенту, который относится к определенному региону. Дополнительные сведения о том, как предложения CSP классифицируются в регионах, см. в разделе [региональные рынки и валюта поставщика облачных решений](https://docs.microsoft.com/partner-center/regional-authorization-overview) .

Если вы обновляете список CSP уже опубликованного предложения, добавьте дополнительных партнеров и выберите **Sync CSP**.

Если у вас есть предложение, у которого уже есть список полномочных партнеров, и вы хотите использовать один и тот же список для другого предложения, используйте функцию **импорта и экспорта**. Перейдите к предложению со списком CSP и выберите **Экспорт CSP**. Функция разрабатывает CSV-файл, который можно импортировать в другое предложение.

#### <a name="option-three-no-partners-in-the-csp-program"></a>Вариант 3. нет партнеров в программе CSP

![Нет партнеров в программе CSP](media/marketplace-publishers-guide/csp-reseller-option-three.png)

Если выбрать этот параметр, предложение будет отказаться от программы CSP. Этот выбор можно изменить в любое время.

### <a name="cloud-partner-portal-opt-in"></a>Портал Cloud Partner согласие

В Портал Cloud Partner вы задаете согласие на вкладку Marketplace или витрину. Возможность выбора конкретных партнеров в программе CSP доступна только в центре партнеров.

![Возможности использования CSP в CPP](media/marketplace-publishers-guide/csp-opt-in.png)

## <a name="deauthorize-partners-in-the-csp-program"></a>Отменять авторизацию партнеров в программе CSP

Если вы уполномочены партнеру в программе CSP и этот партнер уже перепродажу продукт своим клиентам, вы не сможете отменить авторизацию этого партнера.

Если партнер в программе CSP не продавал ваш продукт клиентам и вы хотите удалить CSP после публикации предложения, выполните следующие действия.

1. Перейдите на [страницу Поддержка](https://partner.microsoft.com/support/v2/?stage=1). Первые несколько раскрывающихся меню заполняются автоматически.

   > [!NOTE]
   > Не изменяйте предварительно заполненные элементы раскрывающегося меню.

2. В **поле выберите версию продукта**выберите **Управление Live Offer**.
3. Для **выбора категории, наилучшим образом описывающей эту ошибку**, выберите категорию, которая относится к вашему предложению.
4. Для **выбора проблемы, которая наилучшим образом описывает проблему**, выберите **Обновить существующее предложение**.
5. Нажмите кнопку **Далее** , чтобы перейти на **страницу сведения о проблемах** , чтобы ввести дополнительные сведения о возникшей ошибке.
6. Используйте параметр отменять **авторизацию CSP** в качестве названия проблемы и заполните остальные необходимые разделы.




## <a name="navigate-between-options"></a>Переход между параметрами

Используйте этот раздел для перехода между тремя параметрами торгового посредника CSP.

### <a name="navigate-from-option-one-any-partner-in-the-csp-program"></a>Переход от параметра один: любой партнер в программе CSP

Если ваше предложение сейчас имеет **значение 1: любой партнер в программе CSP** и вы хотите переходить к любому из двух других вариантов, выполните следующие инструкции, чтобы создать запрос:

1. Перейдите на [страницу Поддержка](https://partner.microsoft.com/support/v2/?stage=1). Первые несколько раскрывающихся меню заполняются автоматически.

   > [!NOTE]
   > Не изменяйте предварительно заполненные элементы раскрывающегося меню.

2. В **поле выберите версию продукта**выберите **Управление Live Offer**.
3. Для **выбора категории, наилучшим образом описывающей эту ошибку**, выберите категорию, которая относится к вашему предложению.
4. Для **выбора проблемы, которая наилучшим образом описывает проблему**, выберите **Обновить существующее предложение**.
5. Нажмите кнопку **Далее** , чтобы перейти на **страницу сведения о проблемах** , чтобы ввести дополнительные сведения о возникшей ошибке.
6. Используйте параметр отменять **авторизацию CSP** в качестве названия проблемы и заполните остальные необходимые разделы.

> [!NOTE]
> Если вы пытаетесь выбрать вариант два, а партнер в программе CSP уже перепродажу предложение своим клиентам, этот партнер по умолчанию уже включен в список разрешенных партнеров.  

### <a name="navigate-from-option-two-specific-partners-in-the-csp-program-i-select"></a>Переход из параметра 2. конкретные партнеры в выбранном программном процессе CSP

Если в настоящее время выбран **вариант 2: конкретные партнеры в выбранной программе CSP** и вы хотите выбрать **вариант один: любой партнер в программе CSP**, выполните следующие инструкции, чтобы создать запрос:

1. Перейдите на [страницу Поддержка](https://partner.microsoft.com/support/v2/?stage=1). Первые несколько раскрывающихся меню заполняются автоматически.

   > [!NOTE]
   > Не изменяйте предварительно заполненные элементы раскрывающегося меню.

2. В **поле выберите версию продукта**выберите **Управление Live Offer**.
3. Для **выбора категории, наилучшим образом описывающей эту ошибку**, выберите категорию, которая относится к вашему предложению.
4. Для **выбора проблемы, которая наилучшим образом описывает проблему**, выберите **Обновить существующее предложение**.
5. Нажмите кнопку **Далее** , чтобы перейти на **страницу сведения о проблемах** , чтобы ввести дополнительные сведения о возникшей ошибке.
6. Используйте параметр отменять **авторизацию CSP** в качестве названия проблемы и заполните остальные необходимые разделы.

 Если в настоящее время выбран **вариант 2: отдельные партнеры в выбранной программе CSP** и вы хотите переходить к **варианту 3: нет партнеров в программе CSP**, вы сможете использовать этот вариант только в том случае, если партнеры в программе CSP, которые вы ранее разрешили, не перепродажу ваше предложение клиентам. Чтобы создать запрос, выполните следующие инструкции:

1. Перейдите на [страницу Поддержка](https://partner.microsoft.com/support/v2/?stage=1). Первые несколько раскрывающихся меню заполняются автоматически.

   > [!NOTE]
   > Не изменяйте предварительно заполненные элементы раскрывающегося меню.

2. В **поле выберите версию продукта**выберите **Управление Live Offer**.
3. Для **выбора категории, наилучшим образом описывающей эту ошибку**, выберите категорию, которая относится к вашему предложению.
4. Для **выбора проблемы, которая наилучшим образом описывает проблему**, выберите **Обновить существующее предложение**.
5. Нажмите кнопку **Далее** , чтобы перейти на **страницу сведения о проблемах** , чтобы ввести дополнительные сведения о возникшей ошибке.
6. Используйте параметр отменять **авторизацию CSP** в качестве названия проблемы и заполните остальные необходимые разделы.

### <a name="navigate-from-option-3-no-partners-in-the-csp-program"></a>Переход из варианта 3. нет партнеров в программе CSP

Если ваше предложение сейчас имеет **значение 3: нет партнеров в программе CSP**, вы можете в любое время выбрать любой из двух других вариантов.

## <a name="sharing-sales-and-support-materials-with-partners-in-the-csp-program"></a>Совместное использование материалов по продажам и поддержке с партнерами в программе CSP

Чтобы помочь партнерам в программе поставщика облачных решений наиболее эффективно представить ваше предложение и привлечься к Организации, необходимо отправить материалы по продажам и поддержке, которые будут доступны торговым посредникам. Эти ресурсы не будут доступны клиентам в витринах Marketplace.

### <a name="partner-center-csp-channel"></a>Канал поставщика CSP центра партнеров

Если вы выбрали канал CSP в центре партнеров, издатели должны ввести URL-адрес, который содержит релевантные маркетинговые материалы и контактные данные канала, в канал CSP в модуле списка предложений:

![Информационные материалы CSP центра партнеров](media/marketplace-publishers-guide/pc-csp-channel.png)

### <a name="cloud-partner-portal-csp-channel"></a>Канал Портал Cloud Partner CSP

Если вы выбрали канал CSP в Портал Cloud Partner, издатели должны ввести URL-адрес, который содержит соответствующие маркетинговые материалы и контактные данные канала, в канал CSP:

![Информационные материалы по Портал Cloud Partner CSP](media/marketplace-publishers-guide/cpp-csp-information.png)

## <a name="next-steps"></a>Следующие шаги

Посетите страницу с [руководством по издателю Azure Marketplace и AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

Дополнительные сведения о службах Marketplace GTM Services см. в разделе [услуги переходят на рынок](https://partner.microsoft.com/reach-customers/gtm).

Войдите в [Центр партнеров](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) , чтобы создать и настроить предложение.
