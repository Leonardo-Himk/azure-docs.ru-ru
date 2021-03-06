---
title: Включить имя файла
description: включить файл
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 04/16/2020
ms.author: akjosh
ms.custom: include file
ms.openlocfilehash: 5cb3e6d53f6840b8f4e535976739c188daed18b2
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82789048"
---
Коллекция общих образов — это служба, которая помогает создавать структуру и организацию для управляемых образов. В галереях общих образов предусмотрены следующие задачи:

- Управляемая Глобальная репликация образов.
- Управление версиями и группирование образов для упрощения управления.
- Высокодоступные образы с учетными записями избыточного хранилища зоны (ZRS) в регионах, которые поддерживают Зоны доступности. ZRS обеспечивает лучшую устойчивость к сбоям зональные.
- Поддержка хранилища класса Premium (Premium_LRS).
- Совместное использование подписок и даже между клиентами Active Directory (AD) с помощью RBAC.
- Масштабирование развертываний с помощью реплик образа в каждом регионе.

Используя коллекцию общих образов, вы можете делиться своими образами с другими пользователями, участниками службы или группами конструктора приложений в вашей организации. Общие образы можно реплицировать в несколько регионов для более быстрого масштабирования развертываний.

Образ — это копия полной виртуальной машины (включая все подключенные диски данных) или только диска ОС в зависимости от способа создания. При создании виртуальной машины из образа копии виртуальных жестких дисков в образе используются для создания дисков для новой виртуальной машины. Образ остается в хранилище и может использоваться снова и снова для создания новых виртуальных машин.

При наличии большого количества образов, которые необходимо поддерживать, и необходимости сделать их доступными во всей Организации, можно использовать общую коллекцию образов в качестве репозитория. 

Функция коллекции общих образов имеет несколько типов ресурсов.

| Ресурс | Описание|
|----------|------------|
| **Источник изображения** | Это ресурс, который можно использовать для создания **версии образа** в коллекции образов. Источником образа может быть существующая виртуальная машина Azure, которая либо является [обобщенной, либо специализированной](#generalized-and-specialized-images), управляемым образом, моментальным снимком или версией образа в другой коллекции образов. |
| **Коллекция изображений** | Как и Azure Marketplace, **коллекция образов** — это репозиторий для управления и совместного использования образов, но в отличие от Azure Marketplace доступ к коллекции образов контролируете вы. |
| **Определение образа** | Определения образов создаются в коллекции и содержат сведения о образе и требованиях для их внутреннего использования. Эти сведения включают в себя: определение, относится ли этот образ к Windows или к Linux, заметки о выпуске, а также минимальные и максимальные требования к памяти. Это определение типа образа. |
| **Версия образа** | **Версия образа** используется для создания виртуальной машины с помощью коллекции. В зависимости от требований для вашей среды, у вас может быть несколько версий образа. Так же как и управляемый образ при использовании **версии образа** для создания виртуальной машины, версия образа используется для создания новых дисков для виртуальной машины. Версии образов можно использовать несколько раз. |

<br>

![Рисунок, показывающий, как использовать несколько версий образа в коллекции](./media/shared-image-galleries/shared-image-gallery.png)

## <a name="image-definitions"></a>Определения образов

Определения изображений — это логическая группировка для версий образа. Определение образа содержит сведения о том, почему было создано изображение, какую операционную систему он использует, и другую информацию об использовании образа. Определение образа похоже на план для всех деталей, связанных с созданием определенного образа. Вы не развертываете виртуальную машину из определения образа, а из версий образа, созданных из определения.

Существует три параметра для каждого определения образа, которые используются в комбинации **издателя**, **предложения** и **SKU**. Они используются для поиска определенного определения образа. У вас могут быть версии образов, которые совместно используют одно или два, но не все три значения.  В качестве примера ниже приведены три определения образа и их значения.

|Определение образа|Издатель|ПРЕДЛОЖЕНИЕ|Sku|
|---|---|---|---|
|myImage1|Contoso|Finance.|Серверная часть|
|myImage2|Contoso|Finance.|Внешний интерфейс|
|myImage3|Тестирование|Finance.|Внешний интерфейс|

Все три определения образа имеют уникальные наборы значений. Этот формат аналогичен тому, как в настоящее время можно указать издателя, предложение и SKU для [образов Azure Marketplace](../articles/virtual-machines/windows/cli-ps-findimage.md) в Azure PowerShell, чтобы получить последнюю версию образа Marketplace. Каждое определение изображения должно иметь уникальный набор этих значений.

Ниже приведены другие параметры, которые можно задать для определения образа, чтобы упростить отслеживание ресурсов.

* Состояние операционной системы — можно задать для состояния ОС значение [обобщенная или специализированная](#generalized-and-specialized-images).
* Операционная система — может быть либо Windows, либо Linux.
* Описание — Используйте описание, чтобы получить более подробные сведения о том, почему определение образа существует. Например, у вас может быть определение образа для сервера переднего плана, на котором приложение предварительно установлено.
* EULA — может использоваться для указания лицензионного соглашения, относящегося к определению образа.
* Заявление о конфиденциальности и заметки о выпуске — храните заметки о выпуске и заявления о конфиденциальности в службе хранилища Azure и предоставляют универсальный код ресурса (URI) для доступа к ним в определении образа.
* Дата окончания срока действия. Присоедините дату окончания жизненного цикла к определению образа, чтобы иметь возможность использовать автоматизацию для удаления старых определений образов.
* Тег. Теги можно добавлять при создании определения изображения. Дополнительные сведения о тегах см. [в статье Использование тегов для Организации ресурсов](../articles/azure-resource-manager/management/tag-resources.md) .
* Минимальные и максимальные виртуальных ЦП и рекомендации по использованию памяти. Если образ имеет виртуальных ЦП и рекомендации по использованию памяти, можно присоединить эти сведения к определению образа.
* Запрещенные типы дисков. Вы можете предоставить сведения о требованиях к хранению для виртуальной машины. Например, если образ не подходит для дисков стандартного жесткого диска, добавьте его в список запретов.
* Создание Hyper-V. Вы можете указать, был ли образ создан на основе виртуального жесткого диска Hyper-V поколения 1 или Gen 2.

## <a name="generalized-and-specialized-images"></a>Обобщенные и специализированные образы

Коллекция общих образов поддерживает два состояния операционной системы. Как правило, для образов требуется, чтобы виртуальная машина, используемая для создания образа, была обобщенной перед созданием образа. Обобщение — это процесс, удаляющий сведения о компьютерах и пользователях из виртуальной машины. Для Windows также используется Sysprep. Для Linux можно использовать [waagent](https://github.com/Azure/WALinuxAgent) `-deprovision` или `-deprovision+user` параметры.

Специализированные виртуальные машины не проходят через процесс удаления информации и учетных записей, специфичных для компьютера. Кроме того, с виртуальными машинами, созданными `osProfile` из специализированных образов, не связан. Это означает, что специальные образы будут иметь некоторые ограничения в дополнение к некоторым преимуществам.

- Виртуальные машины и масштабируемые наборы, созданные из специализированных образов, могут работать быстрее. Поскольку они создаются из источника, который уже использовался при первой загрузке, виртуальные машины, созданные на основе этих образов, быстрее загружаются.
- Учетные записи, которые можно использовать для входа в виртуальную машину, также можно использовать на любой виртуальной машине, созданной с помощью специализированного образа, созданного из этой виртуальной машины.
- Виртуальные машины будут иметь **имя компьютера** виртуальной машины, из которой был взят образ. Необходимо изменить имя компьютера, чтобы избежать конфликтов.
- `osProfile` Именно так конфиденциальная информация передается на виртуальную машину с `secrets`помощью. Это может вызвать проблемы с использованием KeyVault, WinRM и других функций, `secrets` используемых в `osProfile`. В некоторых случаях для обхода этих ограничений можно использовать управляемые удостоверения службы (MSI).

## <a name="regional-support"></a>Поддержка в регионах

Исходные регионы перечислены в следующей таблице. Все общедоступные регионы могут быть целевыми регионами, но для репликации в Центральная Австралия и Центральная Австралия 2 вам потребуется список разрешений подписки. Чтобы запросить список разрешений, перейдите к: https://azure.microsoft.com/global-infrastructure/australia/contact/


| Исходные регионы        |                   |                    |                    |
| --------------------- | ----------------- | ------------------ | ------------------ |
| Центральная Австралия     | Восточный Китай        | Южная Индия        | Западная Европа        |
| Центральная Австралия 2   | Восточный Китай 2      | Юго-Восточная Азия     | южная часть Соединенного Королевства           |
| Восточная Австралия        | Северный Китай       | Восточная Япония         | западная часть Соединенного Королевства            |
| Юго-Восточная часть Австралии   | Северный Китай 2     | Западная Япония         | центральный регион US DoD     |
| Южная Бразилия          | Восточная Азия         | Республика Корея, центральный регион      | восточный регион US DoD        |
| Центральная Канада        | Восточная часть США           | Республика Корея, южный регион        | US Gov (Аризона)     |
| Восточная Канада           | восточная часть США 2         | Центрально-северная часть США   | US Gov (Техас)       |
| Центральная Индия         | Восточная часть США 2 (EUAP)    | Северная Европа       | US Gov (Вирджиния)    |
| Центральная часть США            | Центральная Франция    | Центрально-южная часть США   | Западная Индия         |
| Центральная часть США (EUAP)       | Южная Франция      | центрально-западная часть США    | западная часть США            |
|                       |                   |                    | западная часть США 2          |



## <a name="limits"></a>Ограничения 

Существуют ограничения на подписку для развертывания ресурсов с помощью общих коллекций образов.
- 100 коллекций общих образов на подписку для каждого региона
- 1 000 определений образов для каждой подписки на регион
- 10 000. версии образов на подписку на регион
- 10 реплик версии образа на одну подписку на регион
- Любой диск, подключенный к образу, должен быть меньше или равен 1 ТБ.

Дополнительные сведения см. в разделе [Проверка использования ресурсов в соответствии с ограничениями](https://docs.microsoft.com/azure/networking/check-usage-against-limits) в качестве примера проверки текущего использования.
 
## <a name="scaling"></a>Масштабирование
Коллекция общих образов позволяет указать число реплик, которые вы хотите, чтобы Azure сохранил для образов. Это помогает в сценариях развертывания нескольких виртуальных машин, поскольку их развертывания могут быть распространены на разные реплики, уменьшая вероятность того, что процесс создания экземпляра будет регулироваться из-за перегрузки отдельной реплики.

С помощью общей коллекции образов теперь можно развернуть до 1 000 экземпляров ВИРТУАЛЬНЫХ машин в масштабируемом наборе виртуальных машин (от 600 с управляемыми образами). Реплики образа обеспечивают более высокую производительность, надежность и согласованность при развертывании. Можно задать различные счетчики реплик в каждом целевом регионе в зависимости от потребностей масштабирования для региона. Так как Каждая реплика является глубоким копированием образа, это позволяет линейно масштабировать развертывания с каждой дополнительной репликой. Хотя мы понимаем, что два изображения или региона не совпадают, Вот общие рекомендации по использованию реплик в регионе.

- Для развертываний масштабируемых наборов виртуальных машин (VMSS) — для каждых 20 виртуальных машин, которые вы создаете одновременно, рекомендуется использовать одну реплику. Например, если вы создаете виртуальные машины 120 параллельно с помощью одного и того же образа в регионе, мы рекомендуем разместить по крайней мере 6 реплик образа. 
- Для развертываний масштабируемого набора виртуальных машин (VMSS) — для каждого развертывания масштабируемого набора, в котором до 600 экземпляров рекомендуется разместить по крайней мере одну реплику. Например, если вы создаете 5 наборов масштабирования одновременно, каждый с экземплярами виртуальных машин 600, использующих один образ в одном регионе, мы рекомендуем разместить по крайней мере 5 реплик образа. 

Мы всегда рекомендуем вам переподготавливать количество реплик из-за таких факторов, как размер образа, содержимое и тип ОС.

![Рисунок, показывающий, как можно масштабировать образы](./media/shared-image-galleries/scaling.png)

## <a name="make-your-images-highly-available"></a>Обеспечение высокой доступности образов

[Хранилище, избыточное в пределах зоны Azure (ZRS)](https://azure.microsoft.com/blog/azure-zone-redundant-storage-in-public-preview/) , обеспечивает устойчивость к сбою зоны доступности в регионе. Общедоступная Коллекция образов позволяет сохранять образы в учетных записях ZRS в регионах с Зоны доступности. 

Также можно выбрать тип учетной записи для каждого из целевых регионов. Тип учетной записи хранения по умолчанию — Standard_LRS, но можно выбрать Standard_ZRS для регионов с Зоны доступности. Проверьте региональную доступность ZRS [здесь](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs).

![Иллюстрация, показывающая ZRS](./media/shared-image-galleries/zrs.png)

## <a name="replication"></a>Репликация
Коллекция общих образов также позволяет автоматически реплицировать свои образы в других регионах Azure. Каждую версию общего образа можно реплицировать в разных регионах в зависимости от того, что лучше всего подходит для вашей организации. Примером может служить репликация последних образов в нескольких регионах, хотя все старые версии доступны только в одном регионе. Это позволяет экономить на стоимости хранения для версий общих образов. 

Области, в которых реплицирована версия общего образа, можно обновить после создания. Время, необходимое для репликации в разных регионах, зависит от объема копируемых данных и количества регионов, в которые реплицируется версия. В некоторых случаях это может занять несколько часов. В процессе репликации можно просмотреть ее состояние для каждого региона. После завершения репликации образа в регионе можно развернуть виртуальную машину или масштабируемый набор с помощью этой версии образа в регионе.

![Рисунок, показывающий, как можно реплицировать образы](./media/shared-image-galleries/replication.png)

## <a name="access"></a>Доступ

Поскольку коллекция общих образов, определение образа и версия образа являются ресурсами, их можно совместно использовать с помощью встроенных собственных элементов управления RBAC в Azure. С помощью RBAC можно предоставить общий доступ к этим ресурсам другим пользователям, субъектам-службам и группам. Вы даже можете предоставить доступ пользователям за пределами клиента, которые они создали в. Когда пользователь получает доступ к общей версии образа, он может развернуть виртуальную машину или масштабируемый набор виртуальных машин.  Ниже приведена матрица общего доступа, которая помогает понять, к чему пользователь получает доступ.

| Доступ предоставлен пользователю     | Общая коллекция образов | Определение образа | Версия образа |
|----------------------|----------------------|--------------|----------------------|
| Общая коллекция образов | Да                  | Да          | Да                  |
| Определение образа     | нет                   | Да          | Да                  |

Мы рекомендуем использовать общий доступ на уровне коллекции для получения лучших возможностей. Не рекомендуется предоставлять общий доступ к отдельным версиям образа. Дополнительные сведения о RBAC см. в статье [Управление доступом к ресурсам Azure с помощью RBAC](../articles/role-based-access-control/role-assignments-portal.md).

Образы можно совместно использовать, масштабировать, даже в разных клиентах с помощью регистрации приложения с несколькими клиентами. Дополнительные сведения о совместном использовании образов в клиентах см. в статье [совместное использование образов виртуальных машин коллекции в клиентах Azure](../articles/virtual-machines/linux/share-images-across-tenants.md).

## <a name="billing"></a>Выставление счетов
За использование службы "Коллекция общих образов" дополнительная оплата не взимается. Плата взимается за следующие ресурсы.
- Расходы хранилища для хранения версий общих образов. Стоимость зависит от количества реплик версии образа и от количества регионов, на которые реплицируется версия. Например, если у вас есть 2 изображения и обе реплицируются в 3 региона, вам будет назначено шесть управляемых дисков в зависимости от их размера. Дополнительные сведения см. в статье [Цены на управляемые диски](https://azure.microsoft.com/pricing/details/managed-disks/).
- Плата исходящего трафика для репликации первой версии образа из исходного региона в реплицированные. Последующие реплики обрабатываются в пределах региона, поэтому дополнительная плата не взимается. 

## <a name="updating-resources"></a>Обновление ресурсов

После создания можно внести некоторые изменения в ресурсы коллекции образов. Они ограничены:
 
Коллекция общих образов
- Описание

Определение образа
- Рекомендуемое число виртуальных ЦП
- Рекомендуемая память
- Описание
- Дата окончания жизненного цикла

Версия образа
- Количество региональных реплик
- Целевые регионы
- Исключить из последней
- Дата окончания жизненного цикла

## <a name="sdk-support"></a>Поддержка пакетов SDK

Следующие пакеты SDK поддерживают создание коллекций общих образов.

- [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/virtualmachines/management?view=azure-dotnet)
- [Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable)
- [Node.js](https://docs.microsoft.com/javascript/api/@azure/arm-compute)
- [Python](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)
- [GO](https://docs.microsoft.com/azure/go/)

## <a name="templates"></a>Шаблоны

Вы можете создать ресурс коллекции общих образов с помощью шаблонов. Существует несколько шаблонов быстрого запуска Azure: 

- [Создание Общей коллекции образов.](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Создание определения образа в Общей коллекции образов](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Создание версии образа в Общей коллекции образов](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Создание виртуальной машины из версии образа](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

## <a name="frequently-asked-questions"></a>Вопросы и ответы 

* [Как создать список всех ресурсов коллекции общих образов в разных подписках?](#how-can-i-list-all-the-shared-image-gallery-resources-across-subscriptions) 
* [Можно ли перенести существующий образ в коллекцию общих образов?](#can-i-move-my-existing-image-to-the-shared-image-gallery)
* [Можно ли создать версию образа из специализированного диска?](#can-i-create-an-image-version-from-a-specialized-disk)
* [Можно ли переместить ресурс коллекции общих образов в другую подписку после ее создания?](#can-i-move-the-shared-image-gallery-resource-to-a-different-subscription-after-it-has-been-created)
* [Можно ли реплицировать мои версии образов в облаках, например в Azure для Китая или в Германии или в облаке Azure для государственных организаций?](#can-i-replicate-my-image-versions-across-clouds-such-as-azure-china-21vianet-or-azure-germany-or-azure-government-cloud)
* [Можно ли реплицировать версии образов между подписками?](#can-i-replicate-my-image-versions-across-subscriptions)
* [Возможно ли предоставить общий доступ к версиям образов между клиентами Azure Active Directory?](#can-i-share-image-versions-across-azure-ad-tenants)
* [Сколько времени занимает репликация версии образа между целевыми регионами?](#how-long-does-it-take-to-replicate-image-versions-across-the-target-regions)
* [В чем разница между исходным и целевым регионом?](#what-is-the-difference-between-source-region-and-target-region)
* [Как указать исходный регион при создании версии образа?](#how-do-i-specify-the-source-region-while-creating-the-image-version)
* [Как задать число реплик версии образа, создаваемых в каждом регионе?](#how-do-i-specify-the-number-of-image-version-replicas-to-be-created-in-each-region)
* [Можно ли создать коллекцию общих образов в другом расположении, чем для определения образа и версии образа?](#can-i-create-the-shared-image-gallery-in-a-different-location-than-the-one-for-the-image-definition-and-image-version)
* [Какова стоимость использования коллекции общих образов?](#what-are-the-charges-for-using-the-shared-image-gallery)
* [Какую версию API следует использовать для создания общей коллекции образов и определения образа и версии образа?](#what-api-version-should-i-use-to-create-shared-image-gallery-and-image-definition-and-image-version)
* [Какую версию API следует использовать для создания общей виртуальной машины или масштабируемого набора виртуальных машин из версии образа?](#what-api-version-should-i-use-to-create-shared-vm-or-virtual-machine-scale-set-out-of-the-image-version)
* [Можно ли обновить масштабируемый набор виртуальных машин, созданный с помощью управляемого образа, для использования образов общей коллекции образов?]

### <a name="how-can-i-list-all-the-shared-image-gallery-resources-across-subscriptions"></a>Как создать список всех ресурсов коллекции общих образов в разных подписках?

Чтобы получить список всех ресурсов коллекции общих образов для всех подписок, к которым у вас есть доступ на портал Azure, выполните следующие действия:

1. Откройте [портал Azure](https://portal.azure.com).
1. Прокрутите страницу вниз и выберите **все ресурсы**.
1. Выберите все подписки, из которых вы хотите включить в список все ресурсы.
1. Найдите ресурсы типа " **Коллекция общих образов**",.
  
Чтобы получить список всех ресурсов коллекции общих образов в разных подписках, к которым у вас есть доступ, выполните следующую команду в Azure CLI.

```azurecli
   az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
```

Дополнительные сведения см. в статье **Управление ресурсами коллекции** с помощью [Azure CLI](../articles/virtual-machines/update-image-resources-cli.md) или [PowerShell](../articles/virtual-machines/update-image-resources-powershell.md).

### <a name="can-i-move-my-existing-image-to-the-shared-image-gallery"></a>Можно ли перенести существующий образ в коллекцию общих образов?
 
Да. Существует 3 сценария, в зависимости от типов образов, которые у вас могут быть.

 Сценарий 1. Если у вас есть управляемый образ, из него можно создать определение образа и версию образа. Дополнительные сведения см. в статье **Миграция из управляемого образа в версию образа** с помощью [Azure CLI](../articles/virtual-machines/image-version-managed-image-cli.md) или [PowerShell](../articles/virtual-machines/image-version-managed-image-powershell.md).

 Сценарий 2. Если у вас есть неуправляемый образ, можно создать управляемый образ из него, а затем создать определение образа и его версию на основе образа. 

 Сценарий 3. Если в локальной файловой системе имеется виртуальный жесткий диск, необходимо передать его в управляемый образ, после чего вы сможете создать определение образа и его версию на основе образа.

- Если виртуальный жесткий диск является виртуальной машиной Windows, см. статью [Отправка виртуального жесткого диска](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
- Если это виртуальный жесткий диск виртуальной машины Linux, см. раздел [Создание виртуальной машины Linux на основе пользовательского диска с помощью Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd).

### <a name="can-i-create-an-image-version-from-a-specialized-disk"></a>Можно ли создать версию образа из специализированного диска?

Да, поддержка специализированных дисков в качестве образов доступна в предварительной версии. Виртуальную машину можно создать только из специализированного образа с помощью портала, PowerShell или API. 


Используйте [PowerShell для создания образа специализированной виртуальной машины](../articles/virtual-machines/image-version-vm-powershell.md).

Используйте портал для создания [Windows](../articles/virtual-machines/linux/shared-images-portal.md) или [Linux] (.. /артиклес/Виртуал-мачинес/Линукс/Шаред-имажес-портал.МД) изображение. 


### <a name="can-i-move-the-shared-image-gallery-resource-to-a-different-subscription-after-it-has-been-created"></a>Можно ли переместить ресурс коллекции общих образов в другую подписку после ее создания?

Нет, ресурс коллекции общих образов нельзя переместить в другую подписку. Вы можете реплицировать версии образа из коллекции в другие регионы или скопировать образ из другой коллекции с помощью [Azure CLI](../articles/virtual-machines/image-version-another-gallery-cli.md) или [PowerShell](../articles/virtual-machines/image-version-another-gallery-powershell.md).

### <a name="can-i-replicate-my-image-versions-across-clouds-such-as-azure-china-21vianet-or-azure-germany-or-azure-government-cloud"></a>Можно ли реплицировать мои версии образов в облаках, например в Azure для Китая или в Германии или в облаке Azure для государственных организаций?

Нет, реплицировать версии образов между облаками невозможно.

### <a name="can-i-replicate-my-image-versions-across-subscriptions"></a>Можно ли реплицировать версии образов между подписками?

Нет, можно реплицировать версии образов между регионами в рамках подписки и использовать их в других подписках с помощью RBAC.

### <a name="can-i-share-image-versions-across-azure-ad-tenants"></a>Возможно ли предоставить общий доступ к версиям образов между клиентами Azure Active Directory? 

Да, можно использовать RBAC для предоставления общего доступа отдельным пользователям между клиентами. Но для совместного использования масштабирования см. раздел "совместное использование образов коллекций в клиентах Azure" с помощью [PowerShell](../articles/virtual-machines/windows/share-images-across-tenants.md) или [CLI](../articles/virtual-machines/linux/share-images-across-tenants.md).

### <a name="how-long-does-it-take-to-replicate-image-versions-across-the-target-regions"></a>Сколько времени занимает репликация версии образа между целевыми регионами?

Время репликации версии образа полностью зависит от размера образа и числа регионов, в которые он реплицируется. Однако для достижения наилучших результатов рекомендуется, чтобы размер образа был маленьким, а исходный и целевые регионы находились близко друг к другу. Можно проверить состояние репликации, используя флажок ReplicationStatus.

### <a name="what-is-the-difference-between-source-region-and-target-region"></a>В чем разница между исходным и целевым регионом?

Исходный регион — это регион, в котором будет создана версия образа. А целевые регионы — это регионы, в которых будет храниться копия вашей версии образа. Для каждой версии образа может быть только один исходный регион. Кроме того, убедитесь, что при создании версии образа вы передали расположение исходного региона как одно из целевых регионов.

### <a name="how-do-i-specify-the-source-region-while-creating-the-image-version"></a>Как указать исходный регион при создании версии образа?

При создании версии образа, чтобы указать исходный регион, можно использовать теги **--location** в интерфейсе командной строки и **-Location** в PowerShell. Убедитесь, что управляемый образ, который используется в качестве базового для создания версии образа, находится в том же расположении, в котором вы хотите создать версию образа. Кроме того, убедитесь, что при создании версии образа вы передали расположение исходного региона как одно из целевых регионов.  

### <a name="how-do-i-specify-the-number-of-image-version-replicas-to-be-created-in-each-region"></a>Как задать число реплик версии образа, создаваемых в каждом регионе?

Задать число реплик версии образа, создаваемых в каждом регионе, можно двумя способами:
 
1. Количество региональных реплик, которое указывает на число реплик, создаваемых на регион. 
2. Общее количество реплик, которое является значением по умолчанию для регионов в случае, если не указано количество региональных реплик. 

Чтобы указать число региональных реплик, передайте расположение и число реплик, которые вы хотите создать в этом регионе: "Юго-Центральная часть США = 2". 

Если количество региональных реплик указано не для всех расположений, то количеством по умолчанию будет общее количество реплик, которое вы указали. 

Чтобы указать общее количество реплик в интерфейсе командной строки, используйте аргумент **--replica-count** в команде `az sig image-version create`.

### <a name="can-i-create-the-shared-image-gallery-in-a-different-location-than-the-one-for-the-image-definition-and-image-version"></a>Можно ли создать коллекцию общих образов в другом расположении, чем для определения образа и версии образа?

Да, это возможно. Однако рекомендуется размещать в одном расположении группу ресурсов, коллекцию общих образов, определение образа и версию образа.

### <a name="what-are-the-charges-for-using-the-shared-image-gallery"></a>Какова стоимость использования коллекции общих образов?

За использование службы "Коллекция общих образов" плата не взимается, за исключением расходов на хранение версий образов и на исходящий трафик для репликации версий образов из исходного региона в целевые регионы.

### <a name="what-api-version-should-i-use-to-create-shared-image-gallery-and-image-definition-and-image-version"></a>Какую версию API следует использовать для создания общей коллекции образов и определения образа и версии образа?

Для работы с коллекцией общих образов, определениями образов и версиями образов рекомендуется использовать API версии 2018-06-01. Для хранилища, избыточного в виде зоны (ZRS), требуется версия 2019-03-01 или более поздняя.

### <a name="what-api-version-should-i-use-to-create-shared-vm-or-virtual-machine-scale-set-out-of-the-image-version"></a>Какую версию API следует использовать для создания общей виртуальной машины или масштабируемого набора виртуальных машин из версии образа?

Для развертываний виртуальной машины и масштабируемого набора виртуальных машин с помощью версии образа рекомендуется использовать API версии 2018-04-01 или более поздней.

### <a name="can-i-update-my-virtual-machine-scale-set-created-using-managed-image-to-use-shared-image-gallery-images"></a>Можно ли обновить масштабируемый набор виртуальных машин, созданный с помощью управляемого образа, для использования образов общей коллекции образов?

Да, вы можете обновить ссылку на образ масштабируемого набора из управляемого образа на общий образ коллекции образов, если тип ОС, создание Hyper-V и макет диска данных совпадают между этими изображениями. 
