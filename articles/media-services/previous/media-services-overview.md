---
title: Общие сведения о службах мультимедиа Azure | Документация Майкрософт
description: Службы мультимедиа Azure — это расширяемая облачная платформа, которая позволяет разработчикам создавать масштабируемые приложения для управления и доставки файлов мультимедиа. В этой статье приводятся общие сведения о службах мультимедиа Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/19/2019
ms.author: juliako
ms.openlocfilehash: 1c2d6287a89c7816c30cf26978859c07dba0251d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78197510"
---
# <a name="azure-media-services-overview"></a>Общие сведения о службах мультимедиа Azure 

> [!div class="op_single_selector" title1="Выберите версию служб мультимедиа, которую вы используете:"]
> * [Версия 3](../latest/media-services-overview.md)
> * [Версия 2](media-services-overview.md)

> [!NOTE]
> В Службы мультимедиа версии 2 больше не добавляются новые функции. <br/>Ознакомьтесь с последней версией [служб мультимедиа v3](https://docs.microsoft.com/azure/media-services/latest/). См. также [руководство по миграции из v2 в версии 3](../latest/migrate-from-v2-to-v3.md) .

Службы мультимедиа Azure (AMS) — это расширяемая облачная платформа, которая позволяет разработчикам создавать масштабируемые приложения для управления и доставки файлов мультимедиа. С помощью служб мультимедиа Azure можно безопасно передавать, сохранять, кодировать и упаковывать видео- или аудиосодержимое для потоковой трансляции разным клиентам (например, на ТВ, ПК и мобильные устройства) или для трансляции по требованию.

Можно создавать сквозные рабочие процессы, полностью использующие службы мультимедиа. Также можно использовать сторонние компоненты в качестве некоторых частей рабочего процесса. Например, можно выполнять кодирование с помощью стороннего кодировщика. А затем отправить, защитить, упаковать и доставить содержимое с использованием служб мультимедиа. Вы можете выполнить потоковую передачу содержимого динамически или доставить содержимое по запросу. 


## <a name="compliance-privacy-and-security"></a>Соответствие требованиям, конфиденциальность и безопасность

В качестве важного напоминания вы должны соблюдать все применимые законы в использовании служб мультимедиа Azure, и вы не можете использовать службы мультимедиа или любые службы Azure, которые нарушают права других пользователей или могут быть опасны для других.

Перед отправкой видео-или изображений в службы мультимедиа необходимо иметь все необходимые права на использование видео или изображения, в том числе, если это необходимо для закона, всех необходимых пользователей (если таковые имеются) в видео или изображении, для использования, обработки и хранения данных в службах мультимедиа и Azure. Некоторые юрисдикции могут накладывать особые юридические требования для коллекции, оперативную обработку и хранение определенных категорий данных, таких как биометрические данные. Прежде чем использовать службы мультимедиа и Azure для обработки и хранения любых данных в соответствии с особыми юридическими требованиями, необходимо обеспечить соответствие всем таким юридическим требованиям, которые могут быть применимы к вам.

Сведения о соответствии, конфиденциальности и безопасности в службах мультимедиа см. в [центре управления безопасностью](https://www.microsoft.com/trust-center/?rtc=1)Майкрософт. Для обязательств по конфиденциальности корпорации Майкрософт, обработки данных и рекомендаций по хранению, включая сведения об удалении данных, ознакомьтесь с заявлением корпорации Майкрософт [о конфиденциальности](https://privacy.microsoft.com/PrivacyStatement), [условиями веб-служб](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) ("OST") и [дополнением к обработке данных](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA"). Используя службы мультимедиа, вы соглашаетесь с использованием OST, DPA и заявления о конфиденциальности.
 
## <a name="prerequisites"></a>Предварительные условия

Приступить к использованию служб мультимедиа Azure можно только при наличии следующих компонентов.

* Учетная запись Azure. Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в статье [Бесплатная пробная версия Azure](https://azure.microsoft.com).
* Учетная запись служб мультимедиа Azure. Дополнительные сведения см. в статье [Создание учетной записи служб мультимедиа Azure с помощью портала Azure](media-services-portal-create-account.md).
* Настройка среды разработки (необязательно) Выберите .NET или REST API для среды разработки. Дополнительные сведения см. в [разделе о настройке среды](media-services-dotnet-how-to-use.md).

    Кроме того, ознакомьтесь с разделом о [подключении с помощью программных средств к API AMS](media-services-use-aad-auth-to-access-ams-api.md).
* Конечная точка потоковой передачи уровня "Стандартный" или "Премиум" в состоянии "Запущено".  Дополнительные сведения см. в разделе [Управление конечными точками потоковой передачи](media-services-portal-manage-streaming-endpoints.md) .

## <a name="sdks-and-tools"></a>Пакеты SDK и средства

Для сборки решений на базе служб мультимедиа можно использовать

* [REST API для службы мультимедиа.](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Один из доступных клиентских пакетов SDK:
    * Пакет SDK служб мультимедиа Azure для .NET
    
        * [Пакет NuGet](https://www.nuget.org/packages/windowsazure.mediaservices/)
        * [Исходный код GitHub](https://github.com/Azure/azure-sdk-for-media-services)
    * [Пакет Azure SDK для Java](https://github.com/Azure/azure-sdk-for-java),
    * [Пакет Azure SDK для PHP](https://github.com/Azure/azure-sdk-for-php).
    * [Службы мультимедиа Azure для Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (это версия пакета SDK для Node.js сторонних производителей. Она поддерживается сообществом и в настоящее время не обладает полным покрытием всех интерфейсов API AMS).
* Существующие средства:
    * [Портал Azure](https://portal.azure.com/)
    * [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (обозреватель служб мультимедиа Azure (AMSE) является приложением Winforms/С# для Windows)

> [!NOTE]
> Чтобы получить последнюю версию пакета SDK для Java и приступить к разработке с помощью Java, перейдите к статье [Начало работы с клиентским пакетом SDK для Java для служб мультимедиа Azure](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Чтобы загрузить последний пакет SDK для PHP для служб мультимедиа, найдите версию 0.5.7 пакета Microsoft/WindowAzure в [репозитории Packagist](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="code-samples"></a>Примеры кода

Найдите несколько примеров кода в коллекции **образцов кода Azure**: [Примеры кода Azure](https://azure.microsoft.com/resources/samples/?service=media-services&sort=0).

## <a name="concepts"></a>Основные понятия

Основные понятия служб мультимедиа Azure см. в статье [Основные понятия служб мультимедиа Azure](media-services-concepts.md).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Поддерживаемые сценарии и доступность служб мультимедиа в центрах обработки данных

Дополнительные сведения см. в статье [Scenarios and availability of features and services across data centers](scenarios-and-availability.md) (Сценарии и доступность функций служб мультимедиа в центрах обработки данных).

## <a name="service-level-agreement-sla"></a>Соглашение об уровне обслуживания

Дополнительные сведения можно найти в разделе [Соглашение об уровне обслуживания Microsoft Azure](https://azure.microsoft.com/support/legal/sla/).

Дополнительные сведения о доступности в центрах обработки данных см. в разделе [Доступность](scenarios-and-availability.md#availability).

## <a name="support"></a>Поддержка

[Служба поддержки Azure](https://azure.microsoft.com/support/options/) обеспечивает различные варианты поддержки для Azure, включая службы мультимедиа.

## <a name="provide-feedback"></a>Предоставление отзыва

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
