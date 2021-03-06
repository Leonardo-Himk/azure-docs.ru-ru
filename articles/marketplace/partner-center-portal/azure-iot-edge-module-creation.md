---
title: Создание предложения модуля Azure IoT Edge с помощью центра партнеров в Azure Marketplace
description: Узнайте, как создать, настроить и опубликовать предложение модуля IoT Edge в Azure Marketplace с помощью центра партнеров.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: d69090eb07159c2c188c54499a167f127269df24
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857658"
---
# <a name="create-configure-and-publish-an-iot-edge-module-offer-in-azure-marketplace"></a>Создание, Настройка и публикация предложения модуля IoT Edge в Azure Marketplace

В этой статье описывается создание и публикация предложения модуля пограничной службы "Интернет вещей" (IoT) для Azure Marketplace. Прежде чем начать, [Создайте учетную запись коммерческого магазина в центре партнеров](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) , если вы еще этого не сделали. Убедитесь, что ваша учетная запись зарегистрирована в программе коммерческого рынка.

## <a name="create-a-new-offer"></a>Создание нового предложения

1. Войдите в [Центр партнеров](https://partner.microsoft.com/dashboard/home).
2. В меню слева выберите пункт**Обзор** **коммерческого рынка** > .
3. На странице Обзор выберите **+ New предложение** > **IOT Edge модуль**.

    ![Показывает меню навигации слева.](./media/new-offer-iot-edge.png)

> [!IMPORTANT]
> После публикации предложения изменения, внесенные в него в центре партнеров, отображаются только в витринах после повторной публикации предложения. Убедитесь, что после внесения изменений всегда выполняется повторная публикация.

### <a name="offer-id-and-alias"></a>Идентификатор предложения и псевдоним

Введите **идентификатор предложения**. Это уникальный идентификатор для каждого предложения в вашей учетной записи.

- Этот идентификатор отображается для клиентов в веб-адресе для предложения Marketplace и шаблонов Azure Resource Manager, если это применимо.
- Вы можете использовать только строчные буквы и цифры. Он может содержать дефисы и знаки подчеркивания, но не содержит пробелов и не должен превышать 50 символов. Например, если ввести **Test-предложение-1**, веб-адрес предложения будет иметь `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`значение.
- Идентификатор предложения нельзя изменить после нажатия **создать**.

Введите **псевдоним предложения**. Это имя, используемое для предложения в центре партнеров.

- Это имя не используется в Marketplace и отличается от имени предложения и других значений, отображаемых для клиентов.
- Этот параметр нельзя изменить после нажатия **создать**.

Выберите **создать** , чтобы создать предложение и продолжить.

## <a name="offer-overview"></a>Обзор предложения

На странице **Обзор предложения** отображается визуальное представление шагов, необходимых для публикации этого предложения (как завершенное, так и предстоящее), а также время, необходимое для выполнения каждого шага.

На этой странице содержатся ссылки для выполнения операций с этим предложением на основе сделанного выбора. Пример.

- Если это предложение черновика- [Delete](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Если предложение активно, то не допуская [продажи предложения](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Если предложение находится в режиме предварительной версии — [Go-Live](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Если вы еще не завершили выход из Publisher, [Отмените публикацию.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="offer-setup"></a>Настройка предложения

Выполните следующие действия, чтобы настроить предложение.

### <a name="connect-lead-management"></a>Подключение управления интересами

При публикации предложения в Marketplace с помощью центра партнеров можно подключить его к системе управления отношениями с клиентами (CRM). Это позволяет получить контактную информацию клиента, как только кто-то выражает интерес или использует ваш продукт.

1. Выберите назначение интереса, в которое будут отправляться данные о потенциальных клиентах. Центр партнеров поддерживает следующие системы CRM:

    - [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics) для участия клиентов
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > Если ваша система CRM не указана выше, используйте [таблицу Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) или [конечную точку HTTPS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) для хранения данных интереса клиента, а затем экспортируйте данные в систему CRM.

2. Подключайте свое предложение к целевому месту при публикации в центре партнеров.
3. Убедитесь, что подключение к цели интереса настроено правильно. После публикации в центре партнеров мы пропроверяем подключение и отправим вам тестового руководителя. Пока вы проведете предварительный просмотр предложения перед его продолжением, вы также можете проверить подключение интереса, пытаясь приобрести предложение самостоятельно в предварительной версии среды.
4. Убедитесь, что подключение к месту назначения не изменится, так что вы не потеряли никаких интересов.

Ниже приведены некоторые дополнительные ресурсы по управлению интересами.

- [Обзор управления интересами](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Раздел часто задаваемых вопросов об управлении потенциальными клиентами](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Распространенные ошибки конфигурации интересов во время публикации на Портале Cloud Partner](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#publishing-config-errors)
- [Обзор управления интересами](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (убедитесь, что блокирование всплывающих окон отключено).

Прежде чем продолжить, выберите **Сохранить черновик** .

### <a name="properties"></a>Свойства

Эта страница позволяет определить категории, используемые для группировки предложения в Marketplace и юридических контрактах, поддерживающих ваше предложение.

#### <a name="category"></a>Категория

Выберите минимум одну и не более пяти категорий. Эти категории используются для размещения предложения в соответствующих областях поиска Marketplace и отображаются на странице сведений о предложении. В описании предложения Объясните, как ваше предложение поддерживает эти категории. На страницах "Обзор" все модули IOT Edge отображаются в категории  **модуля "Интернет вещей" > IOT Edge**.

#### <a name="legal"></a>Юридическая информация

Для предложения необходимо указать условия. Имеются две возможности.

- Используйте стандартный контракт для коммерческого рынка Майкрософт.
- Укажите собственные условия.

##### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Стандартный контракт для коммерческого рынка Майкрософт

Мы предлагаем шаблон стандартного контракта, который поможет упростить транзакции в коммерческом рынке. Вы можете предложить свое решение в стандартном контракте, который клиенты должны проверять и принимать только один раз. Это хороший вариант, если вы не хотите создавать собственные условия.

Дополнительные сведения о стандартном контракте см. в статье [стандартный контракт для коммерческого рынка Майкрософт](https://docs.microsoft.com/azure/marketplace/standard-contract). Вы также можете скачать [стандартный контракт](https://go.microsoft.com/fwlink/?linkid=2041178) PDF (убедитесь, что блокирование всплывающих окон отключено).

Чтобы использовать стандартный контракт, установите флажок **использовать стандартный контракт для коммерческого рынка Майкрософт** , а затем нажмите кнопку **принять**.

> [!NOTE]
> После публикации предложения с помощью стандартного контракта для коммерческого рынка Майкрософт вы не сможете использовать собственные условия. Либо Предложите свое решение в соответствии со стандартным контрактом, либо в соответствии с собственными условиями.

![Демонстрирует использование стандартного контракта для коммерческого рынка корпорации Майкрософт.](./media/iot-edge-module-creation/iot-edge-module-standard-contract-checkbox.png)

##### <a name="your-own-terms-and-conditions"></a>Собственные условия

Чтобы предоставить собственные пользовательские условия, введите их в поле **условия** . В этом поле можно ввести неограниченное количество символов текста. Клиенты должны принять эти условия, прежде чем они смогут попробовать ваше предложение.

Выберите **Сохранить черновик** , прежде чем перейти к следующему разделу, список предложений.

## <a name="offer-listing"></a>Список предложений

Здесь вы определите сведения о предложении, которые будут отображаться в Marketplace. Сюда входят название предложения, описание, изображения и т. д. При настройке этого предложения обязательно следуйте политикам, описанным на странице политики корпорации Майкрософт.

> [!NOTE]
> Сведения о предложении не обязательно должны располагаться на английском языке, если описание предложения начинается с фразы «это приложение доступно только на языке, отличном от английского языка]». Также можно предоставить полезную ссылку для предложения содержимого на языке, отличном от того, который используется в сведениях о предложении.

### <a name="name"></a>Имя

Введенное здесь имя отображается в качестве заголовка вашего предложения. Это поле заполняется текстом, введенным в поле **псевдоним предложения** при создании предложения. Это имя можно изменить.

Имя:

- Может быть охраняемым товаром (и может содержать символы товарных знаков или авторских прав).
- Длина не может превышать 50 символов.
- Не может содержать эмодзи.

### <a name="search-results-summary"></a>Сводка по результатам поиска

Укажите краткое описание предложения. Это может быть до 100 символов и использоваться в результатах поиска Marketplace.

### <a name="long-summary"></a>Длинная сводка

Введите более подробное описание предложения. Это может быть до 256 символов и использоваться в результатах поиска Marketplace.

### <a name="description"></a>Описание:

Введите более длинное описание предложения, не более 3 000 символов. Это отображается для клиентов в обзоре списка Marketplace.

Включите в описание одну или несколько следующих элементов:

- Ценность и ключевые преимущества, предоставляемые вашим предложением
- Категории или отраслевые связи;
- Возможности приобретения в приложении
- Все необходимые раскрытия

В конце описания предложения модуля IoT Edge должны быть включены минимальные требования к оборудованию. Пример.

*Минимальные требования к оборудованию: Linux x64 и ARM32 OS, 1 ГБ ОЗУ, 500 МБ хранилища*

Ниже приведены некоторые советы по написанию описания.

- Ясно Опишите ценность вашего предложения в первых нескольких предложениях вашего описания. Включите следующие элементы:
    - Описание предложения.
    - Тип пользователя, который имеет преимущества от предложения.
    - Клиентам требуется или выдает адреса предложения.
- Помните, что первые несколько предложений могут отображаться в результатах поиска.
- Не полагайтесь на возможности и функции для продажи продукта. Вместо этого сосредоточьтесь на ценности, предоставляемой вашим предложением.
- Попробуйте использовать словарь, зависящий от отрасли, или слова на основе преимуществ.

Чтобы сделать **Описание** предложения более привлекательным, используйте редактор форматированного текста для форматирования описания. Редактор форматированного текста позволяет добавлять числа, маркеры, жирные курсивы и отступы, чтобы сделать описание более удобочитаемым.

:::image type="content" source="media/text-editor2.png" alt-text="Демонстрирует редактор форматированного текста." border="false":::

- Чтобы изменить формат содержимого, выделите текст, который нужно отформатировать, и выберите стиль текста, как показано на следующем снимке экрана:

     :::image type="content" source="media/text-editor3.png" alt-text="Демонстрирует элемент управления "стиль текста" в редакторе форматированного текста." border="false":::

- Чтобы добавить маркированный или нумерованный список в текст, используйте параметры, показанные на этом снимке экрана:
  
    :::image type="content" source="media/text-editor4.png" alt-text="Демонстрирует элементы управления маркированным списком и нумерованным спискам в редакторе форматированного текста." border="false":::

- Чтобы добавить или удалить отступы в тексте, используйте параметры, показанные на этом снимке экрана:

    :::image type="content" source="media/text-editor5.png" alt-text="Демонстрирует элементы управления отступами в редакторе форматированного текста." border="false":::

#### <a name="privacy-policy-url"></a>URL-адрес с политикой конфиденциальности

Введите веб-адрес политики конфиденциальности вашей организации. Вы несете ответственность за то, что ваше предложение соответствует законодательству и законодательству в отношении конфиденциальности. Вы также отвечаете за публикацию действительной политики конфиденциальности на веб-сайте.

#### <a name="useful-links"></a>Полезные ссылки

Предоставление дополнительных онлайн-документов о предложении. Можно добавить до 25 ссылок. Чтобы добавить ссылку, выберите **+ Добавить ссылку** , а затем заполните следующие поля:

- **Заголовок** — клиенты увидят заголовок на странице сведений о предложении.
- **Ссылка (URL-адрес)** — введите ссылку для клиентов, чтобы просмотреть документ в Интернете. Ссылка должна начинаться с http://или https://.

Обязательно добавьте по крайней мере одну ссылку на документацию и одну ссылку на совместимые IoT Edge устройства из [каталога устройств Azure IOT](https://catalog.azureiotsolutions.com/).

### <a name="contact-information"></a>Контактные данные

Необходимо указать имя, адрес электронной почты и номер телефона для **контакта службы поддержки** и **контактного лица по проектированию.** Эти сведения не отображаются для клиентов. Она доступна корпорации Майкрософт и может быть предоставлена партнерам поставщика облачных решений (CSP).

- Контакт службы поддержки (обязательно): для получения общих вопросов о поддержке.
- Контактное лицо по проектированию (обязательно). технические вопросы и проблемы с сертификацией.
- Контакт программы CSP (необязательно). вопросы торгового посредника, связанные с программой CSP.

В разделе **контактные сведения о службе поддержки** укажите веб-адрес **сайта поддержки** , где партнеры могут найти поддержку вашего предложения в зависимости от того, доступно ли предложение в глобальной среде Azure, Azure для государственных организаций или в обоих случаях.

В разделе **Contact программы CSP** укажите ссылку (**маркетинговые материалы по программе CSP**), где партнеры CSP могут найти маркетинговые материалы для вашего предложения.

#### <a name="additional-marketplace-listing-resources"></a>Материалы по дополнительным материалам Marketplace

Дополнительные сведения о создании списков предложений см. в разделе [предложения с перечнем рекомендаций](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices).

### <a name="marketplace-images"></a>Образы Marketplace

Предоставление логотипов и изображений для использования с вашим предложением. Все изображения должны быть в формате PNG. Размытые изображения будут отклонены.

>[!Note]
>Если при отправке файлов возникла ошибка, убедитесь, что ваша локальная сеть не блокирует https://upload.xboxlive.com службу, используемую центром партнеров.

#### <a name="store-logos"></a>Логотипы Store

Предоставьте PNG-файлы логотипа вашего предложения в каждом из следующих четырех размеров пикселей:

- **Малый (48 x 48)**
- **Средний (90 x 90)**
- **Крупный (216 x 216)**
- **Широкие (255 x 115)**

Все четыре логотипа являются обязательными и используются в разных местах в списке Marketplace.

#### <a name="screenshots-optional"></a>Снимки экрана (необязательно)

Добавьте до пяти снимков экрана, показывающих, как работает Ваше предложение. Размер каждого из них должен составлять 1280 x 720 пикселей и в формате PNG.

#### <a name="videos-optional"></a>Видео (необязательно)

Добавьте до пяти видеороликов, демонстрирующих ваше предложение. Введите имя видео, его веб-адрес и изображение эскиза видео размером 1280 x 720 пикселей.

#### <a name="offer-examples"></a>Примеры предложения

В следующих примерах показано, как поля списка предложений отображаются в разных местах предложения.

На этом снимке экрана показана страница **со списком предложений** в Azure Marketplace.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-offer-listing-page.png" alt-text="Иллюстрация страницы со списком предложений в Azure Marketplace.":::

На этом снимке экрана показаны результаты поиска в Azure Marketplace:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-search-results.png" alt-text="Демонстрирует результаты поиска в Azure Marketplace.":::

На этом снимке экрана показана страница **со списком предложений** в портал Azure.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-listing-page-azure-portal.png" alt-text="Демонстрирует страницу списка предложений в портал Azure.":::

На этом снимке экрана показаны результаты поиска в портал Azure.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-search-results.png" alt-text="Демонстрирует страницу списка предложений в портал Azure.":::

Выберите **Сохранить черновик** , прежде чем перейти к следующему разделу (Предварительная версия).

## <a name="preview"></a>Предварительный просмотр

На **вкладке Предварительный просмотр**можно выбрать ограниченную **аудиторию предварительной версии** для проверки вашего предложения перед его публикацией в более широкой аудитории Marketplace.

> [!IMPORTANT]
> После просмотра предложения в предварительной версии необходимо выбрать **Go Live** , чтобы опубликовать предложение в общедоступном.

Укажите аудиторию предварительной версии, используя ИДЕНТИФИКАТОРы GUID подписки Azure, а также необязательное описание для каждого из них. Клиенты не могут видеть ни одно из этих полей.

> [!NOTE]
> Идентификатор подписки Azure можно найти на странице "подписки" в портал Azure.

Добавьте по крайней мере один идентификатор подписки Azure (по отдельности (до 10) или отправку CSV-файла (до 100). Добавляя эти идентификаторы подписки, вы определяете, кто может просматривать предложение, прежде чем публиковать его в реальном времени. Если ваше предложение уже активно, вы можете определить аудиторию предварительного просмотра, чтобы проверить изменения или обновления вашего предложения.

> [!NOTE]
> Аудитория предварительного просмотра отличается от личной аудитории. Аудитория **предварительного просмотра** может просмотреть и подтвердить все планы предложений, прежде чем они будут опубликованы в Marketplace, включая те, которые будут публиковаться только для **закрытой** аудитории (задается на вкладке доступность).

Выберите **Сохранить черновик** , прежде чем перейти к следующему разделу Обзор плана.

### <a name="plan-overview"></a>Общие сведения о плане

Эта вкладка позволяет указать различные параметры плана в рамках одного предложения в центре партнеров. Эти планы ранее назывались номерами SKU или единицами хранения. Планы могут различаться с точки зрения доступных облаков, таких как глобальные облака, государственные облака и образ, на который ссылается план. Чтобы получить список предложений в Marketplace, необходимо настроить по крайней мере один план.

После создания планов на вкладке **Обзор плана** отображается следующее:

- Имена планов
- Модель ценообразования
- Доступность облака (Глобальная или правительственная)
- Текущее состояние публикации
- Все доступные действия

Действия, доступные в обзоре плана, зависят от текущего состояния плана. в том числе:

- **Удалить черновик**. значение, если состояние плана — черновик.
- **Прерывать план продаж**: Если состояние плана Опубликовано в реальном времени.

#### <a name="create-new-plan"></a>Создание плана

Выберите **создать новый план**. Откроется диалоговое окно **новый план** .

В поле **идентификатор плана** Создайте уникальный идентификатор плана для каждого плана в этом предложении. Этот идентификатор будет отображаться для клиентов в веб-адресе продукта. Используйте только строчные буквы и цифры, тире или символы подчеркивания и не более 50 символов.

В поле **имя плана** введите имя для этого плана. Клиенты видят это имя при принятии решения о том, какой план выбрать в предложении. Создайте уникальное имя для каждого плана в этом предложении. Например, вы можете использовать название предложения **Windows Server** с планами **Windows Server 2016** и **Windows Server 2019**.

> [!NOTE]
> Невозможно изменить идентификатор плана после нажатия на кнопку " **создать**".

Щелкните **Создать**.

### <a name="plan-setup"></a>Настройка плана

На этой вкладке можно указать, в каких облаках доступен план. Ваши ответы на этой вкладке влияют на то, какие поля отображаются на других вкладках.

#### <a name="cloud-availability"></a>Доступность облака

Ваш план должен быть доступен по крайней мере в одном облаке с помощью центра Интернета вещей Azure.

Выберите параметр **Azure Global (Глобальный** ), чтобы клиенты могли использовать ваш план во всех глобальных регионах Azure, использующих Marketplace. Дополнительные сведения см. в разделе [географическая доступность и поддержка валюты](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Выберите вариант [облако Azure для государственных организаций](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) , чтобы сделать ваше решение отображаемым здесь. Это облачное облако сообщества с управляемым доступом для клиентов от федеральных, штатных и местных или уровня общины государственных учреждений США, а также от партнеров, которые могут их обслуживать. Как издатель, вы несете ответственность за любые элементы управления соответствием, меры безопасности и рекомендации для этого облачного сообщества. Azure для государственных организаций использует физически изолированные центры обработки данных и сети (которые находятся только в США). Перед [публикацией](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners) в Azure для государственных организаций протестируйте и подтвердите решение в этой области, так как результаты могут отличаться. Чтобы разместить и протестировать решение, запросите пробную учетную запись от [Microsoft Azure для государственных организаций пробной версии](https://azure.microsoft.com/global-infrastructure/government/request/).

> [!NOTE]
> После публикации и доступности плана в конкретном облаке вы не сможете удалить это облако.

#### <a name="azure-government-cloud-certifications"></a>Сертификация облачных служб Azure для государственных организаций

Этот параметр отображается только в том случае, если выбрано **облако Azure для государственных организаций** в разделе **доступность в облаке**.

Службы Azure для государственных организаций обработают данные, подлежащие определенным нормам и требованиям государственных организаций. Например, FedRAMP, NIST 800,171 (DIB), ITAR, IRS 1075, DoD на уровне 4 и CJIS. Чтобы обеспечить осведомленность о сертификации для этих программ, можно предоставить до 100 ссылок, описывающих сертификаты. Они могут быть ссылками на списки непосредственно в программе или на свой собственный веб-сайт. Эти ссылки видны только клиентам Azure для государственных организаций.

## <a name="plan-listing"></a>Список планов

На этой вкладке отображаются конкретные сведения для каждого плана в рамках одного предложения.

### <a name="plan-name"></a>Имя плана

Оно заполняется именем, которое вы присвоили плану при его создании. При необходимости это имя можно изменить. Длина может составлять до 50 символов. Это имя отображается в качестве заголовка этого плана в Azure Marketplace и портал Azure. Он используется в качестве имени модуля по умолчанию после того, как план будет готов к использованию.

### <a name="plan-summary"></a>Сводка плана

Укажите краткую сводку по вашему плану (не предложение). Эта сводка отображается в результатах поиска Azure Marketplace и может содержать до 100 символов.

### <a name="plan-description"></a>Описание плана

Опишите, что делает этот план уникальным, а также различия между планами в рамках вашего предложения. Не Опишите предложение, а только план. Это описание будет отображаться в Azure Marketplace и в портал Azure на странице со списком предложений. Это может быть то же содержимое, которое вы указали в сводке плана и содержало до 2 000 символов.

После завершения этих полей выберите **Сохранить черновик** .

#### <a name="plan-examples"></a>Примеры плана

В следующих примерах показано, как поля списка плана отображаются в разных представлениях.

Это поля в Azure Marketplace при просмотре сведений о плане:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-plan-details.png" alt-text="Здесь показаны поля, отображаемые при просмотре сведений о плане в Azure Marketplace.":::

Это сведения о плане на портал Azure:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-plan-details.png" alt-text="Иллюстрирует сведения о плане для портал Azure.":::

## <a name="availability"></a>Доступность

Если вы хотите скрыть опубликованное предложение, чтобы клиенты не могли искать, просматривать или покупать его в Marketplace, установите флажок **Скрыть план** на вкладке доступность.

Это поле часто используется в следующих случаях:

- Предложение предназначено для использования только косвенным образом при ссылке на другое приложение.
- Предложение не следует покупать по отдельности.
- План использовался для первоначального тестирования и больше не является релевантным.
- План использовался для временных или сезонных предложений и больше не должен предлагаться.

## <a name="technical-configuration"></a>Техническая конфигурация

Тип предложения **модуля IOT Edge** — это контейнер определенного типа, который выполняется на устройстве IOT Edge. На вкладке **Техническая конфигурация** вы получите справочные сведения о репозитории образов контейнеров в [реестре контейнеров Azure](https://azure.microsoft.com/services/container-registry/), а также параметры конфигурации, позволяющие пользователям легко использовать этот модуль.

После публикации предложения образ контейнера IoT Edge копируется в Azure Marketplace в определенном общедоступном реестре контейнеров. Все запросы пользователей Azure на использование вашего модуля обслуживаются в общедоступном реестре контейнеров Azure Marketplace, а не в частном реестре контейнеров.

Вы можете ориентироваться на несколько платформ и предоставить несколько версий образа контейнера модулей с помощью тегов. Дополнительные сведения о тегах и управлении версиями см. в разделе [Подготовка технических ресурсов модуля IOT Edge](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-iot-edge-module-asset).

### <a name="image-repository-details"></a>Сведения о репозитории изображений

На вкладке **сведения о репозитории образов** представлены следующие сведения.

**Выберите источник образа**: выберите параметр **реестра контейнеров Azure** .

**Идентификатор подписки Azure**. Укажите идентификатор подписки, в которой сообщается об использовании ресурсов, и счета за службы для реестра контейнеров Azure, включающие образ контейнера. Этот идентификатор можно найти на странице " [подписки](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) " в портал Azure.

**Имя группы ресурсов Azure**. Укажите имя [группы ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal) , содержащей реестр контейнеров Azure, в образе контейнера. Группа ресурсов должна быть доступна в ИДЕНТИФИКАТОРе подписки (см. выше). Имя можно найти на странице " [группы ресурсов](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) " в портал Azure.

**Имя реестра контейнеров Azure**: укажите имя [реестра контейнеров Azure](https://docs.microsoft.com/azure/container-registry/container-registry-intro) , в котором находится образ контейнера. Реестр контейнеров должен присутствовать в указанной ранее группе ресурсов Azure. Укажите только имя реестра, а не полное имя сервера входа. Не забудьте опустить **azurecr.IO** из имени. Имя реестра можно найти на [странице реестров контейнеров](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerRegistry%2Fregistries) в портал Azure.

**Имя администратора для реестра контейнеров Azure**: укажите [имя администратора](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account) , связанное с реестром контейнеров Azure, в котором находится образ контейнера. Имя пользователя и пароль необходимы, чтобы предоставить компании доступ к реестру. Чтобы получить имя пользователя и пароль администратора, задайте для свойства с **поддержкой администрирования** **значение True** с помощью интерфейс командной строки Azure (CLI). При необходимости вы можете задать **Включение** в портал Azure **пользователя с правами администратора** .

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-admin-user.png" alt-text="Показывает диалоговое окно "обновление реестра контейнеров".":::

**Пароль для реестра контейнеров Azure**: укажите пароль для имени администратора, связанного с реестром контейнеров Azure и имеющего образ контейнера. Имя пользователя и пароль необходимы, чтобы предоставить компании доступ к реестру. Чтобы получить пароль из портал Azure, перейдите в раздел > **ключи доступа к** **реестру контейнеров**или с помощью Azure CLI с помощью [команды "отобразить".](https://docs.microsoft.com/cli/azure/acr/credential?view=azure-cli-latest#az-acr-credential-show)

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-username-password.png" alt-text="Демонстрирует экран ключа доступа в портал Azure.":::

**Имя репозитория в реестре контейнеров Azure**. Укажите имя репозитория реестра контейнеров Azure, в котором находится образ. Имя репозитория указывается при принудительной отправке образа в реестр. Имя репозитория можно найти, перейдя на > **страницу репозиториев** [реестра контейнеров](https://azure.microsoft.com/services/container-registry/). Дополнительные сведения см. [в разделе Просмотр репозиториев реестра контейнеров в портал Azure](https://docs.microsoft.com/azure/container-registry/container-registry-repositories). Обратите внимание, что после задания имени его нельзя изменить. Используйте уникальное имя для каждого предложения в вашей учетной записи.

### <a name="image-tags-for-new-versions-of-your-offer"></a>Теги изображений для новых версий предложения

Клиенты должны иметь возможность автоматически получать обновления из Azure Marketplace при публикации обновления. Если они не хотят обновляться, они должны иметь возможность остаться в определенной версии образа. Это можно сделать, добавив новые теги изображений каждый раз при обновлении образа.

**Тег Image**. В этом поле должен быть указан **последний** тег, указывающий на последнюю версию образа на всех поддерживаемых платформах. Он также должен включать тег версии (например, начиная с XX. XX. XX, где XX — это число). Клиенты должны использовать [теги манифеста](https://github.com/estesp/manifest-tool) для нескольких платформ. Все теги, указанные в теге манифеста, следует добавить в пакет для передачи. Все теги манифеста (за исключением последнего тега) должны начинаться с X. Y-or X. Y. Z, где X, Y и Z являются целыми числами. Например, если последний тег указывает на 1.0.1-Linux-x64, 1.0.1-Linux-ARM32 и 1.0.1-Windows-ARM32, эти шесть тегов необходимо добавить в это поле. Дополнительные сведения о тегах и управлении версиями см [. в статье подготовка IOT Edge модульных технических ресурсов.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets)

### <a name="default-deployment-settings-optional"></a>Параметры развертывания по умолчанию (необязательно)

Определите самые типичные параметры для развертывания модуля IoT Edge. Оптимизируйте развертывание клиентов, разрешая им запускать готовый модуль IoT Edge с этими параметрами по умолчанию.

**Маршруты по умолчанию**. Концентратор IoT Edge управляет взаимодействием между модулями, центром Интернета вещей и устройствами. Вы можете задать маршруты для ввода и вывода данных между модулями и центром Интернета вещей, что дает вам возможность гибко отправлять сообщения, где им нужно не требовать дополнительных служб для обработки сообщений или написания дополнительного кода. Маршруты создаются с помощью пар «имя-значение». Можно определить до пяти имен маршрутов по умолчанию, каждый из которых может иметь длину до 512 символов.

Не забудьте использовать правильный [синтаксис маршрута](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) в значении маршрута (обычно это ОПРЕДЕЛЯЕТСЯ как from/Message/* в $upstream). Это означает, что все сообщения, отправленные любыми модулями, отправляются в центр Интернета вещей. Чтобы сослаться на модуль, используйте имя модуля по умолчанию, которое будет использоваться в качестве **имени предложения**без пробелов и специальных символов. Для обращения к другим модулям, которые еще не известны, используйте соглашение> <FROM_MODULE_NAME, чтобы сообщить клиентам о необходимости обновления этих сведений. Дополнительные сведения о IoT Edge маршрутах см. в разделе [объявление маршрутов](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes).

Например, если модуль Контосомодуле прослушивает входные данные Контосоинпут и выходных данных на Контосуутпут, имеет смысл определить следующие два маршрута по умолчанию:

- Имя #1: Токонтосомодуле
- Значение #1: FROM/мессажес/модулес/<FROM_MODULE_NAME>/аутпутс/* в Брокередендпоинт ("/Модулес/контосомодуле/инпутс/контосоинпут")
- Имя #2: Фромконтосомодулетоклауд
- Значение #2: из/Мессажес/модулес/контонсомодуле/аутпутс/контосуутпут в $upstream

**Требуемые свойства двойника модуля по умолчанию**. Модуль двойника — это документ JSON в центре Интернета вещей, в котором хранятся сведения о состоянии для экземпляра модуля, включая требуемые свойства. Требуемые свойства используются вместе с сообщаемыми свойствами для синхронизации конфигурации или условий модуля. Серверная часть решения может задавать требуемые свойства, и модуль может их читать. Модуль также может получать уведомления об изменениях в нужных свойствах. Требуемые свойства создаются с использованием до пяти пар "имя-значение", и каждое значение по умолчанию должно содержать менее 512 символов. Можно определить до пяти требуемых свойств двойника имени и значения. Значения требуемых свойств двойника должны быть допустимыми JSON, не экранированными, без escape-массивов с максимальной вложенной иерархией из четырех уровней. В ситуации, когда параметр, необходимый для значения по умолчанию, не имеет смысла (например, IP-адрес сервера клиента), можно добавить параметр в качестве значения по умолчанию. Дополнительные сведения о требуемых свойствах двойника см. в разделе [Определение или обновление требуемых свойств](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

Например, если модуль поддерживает динамически настраиваемую частоту обновления с помощью требуемых свойств двойника, имеет смысл определить следующее требуемое свойство двойника по умолчанию:

- Имя #1: Рефрешрате
- Значение #1:60

**Переменные среды по умолчанию**. Переменные среды предоставляют дополнительные сведения для модуля, который помогает в процессе настройки. Переменные среды создаются с помощью пар "имя-значение". Имя и значение каждой переменной среды по умолчанию должны быть меньше 512 символов и можно определить до пяти. Если параметр, необходимый для значения по умолчанию, не имеет смысла (например, IP-адрес сервера клиента), можно добавить параметр в качестве значения по умолчанию.

Например, если модуль требует перед запуском принять условия использования, вы можете определить следующую переменную среды:

- Имя #1: ACCEPT_EULA
- Значение #1: Y

**Параметры создания контейнера по умолчанию**. Параметры создания контейнера направляют создание контейнера DOCKER модуля IoT Edge. IoT Edge поддерживает параметры создания контейнера API подсистемы DOCKER. Просмотрите все параметры в [контейнерах списка.](https://docs.docker.com/engine/api/v1.30/#operation/ContainerList) Поле параметров создания должно быть допустимым форматом JSON, без экранирования и менее 512 символов.

Например, если для модуля требуется привязка портов, определите следующие параметры создания:

"Хостконфиг": {"Портбиндингс": {"5012/TCP": [{"Хостпорт": "5012"}]}

## <a name="review-and-publish"></a>Проверка и публикация

Завершив все необходимые разделы предложения, вы можете отправить его для просмотра и публикации.

В правом верхнем углу портала выберите **проверить и опубликовать**.

На странице Просмотр можно увидеть состояние публикации:

- См. сведения о состоянии завершения для каждого раздела предложения. Нельзя опубликовать, пока все разделы предложения не будут помечены как завершенные.
    - **Не запущено** — раздел не был запущен и должен быть завершен.
    - **Неполный** — в разделе есть ошибки, которые необходимо исправить, или требуется предоставить дополнительные сведения. Инструкции см. в разделах, приведенных ранее в этом документе.
    - **Завершено** — раздел содержит все необходимые данные, и ошибки отсутствуют. Все разделы предложения должны быть завершены, прежде чем можно будет отправить предложение.
- Предоставьте инструкции по тестированию для группы сертификации, чтобы убедиться, что предложение проверено правильно. Кроме того, укажите дополнительные примечания, которые помогут вам понять ваше предложение.

Чтобы отправить предложение для публикации, выберите **опубликовать**.

Мы отправим вам сообщение электронной почты, которое позволит вам узнать, когда доступна предварительная версия предложения для просмотра и утверждения. Чтобы опубликовать предложение в общедоступном (или закрытом) предложении, перейдите в центр партнеров и выберите **Go-Live**.

## <a name="next-steps"></a>Дальнейшие шаги

- [Обновление существующего предложения в коммерческом магазине](https://docs.microsoft.com//azure/marketplace/partner-center-portal/update-existing-offer)