---
title: Шифрование неактивных данных в службе речи
titleSuffix: Azure Cognitive Services
description: Шифрование неактивных данных в службе распознавания речи.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: egeaney
ms.openlocfilehash: c2e52fbab8d984f7442d8a336e90e9f22c0bf061
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198674"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Шифрование неактивных данных в службе речи

Служба распознавания речи автоматически шифрует данные при их сохранении в облаке. Шифрование службы распознавания речи защищает ваши данные и помогает удовлетворить ваши обязательства по обеспечению безопасности и соответствия требованиям Организации.

## <a name="about-cognitive-services-encryption"></a>Сведения о шифровании Cognitive Services

Данные шифруются и расшифровываются с помощью [fips 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) , совместимого с [256-БИТНЫМ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) шифрованием AES. Шифрование и расшифровка прозрачны, то есть Управление шифрованием и доступом осуществляется за вас. Данные по умолчанию безопасны, и вам не нужно изменять код или приложения, чтобы воспользоваться преимуществами шифрования.

## <a name="about-encryption-key-management"></a>Сведения об управлении ключами шифрования

При использовании Пользовательское распознавание речи и настраиваемого голоса служба речи может хранить в облаке следующие данные:  

* Речевая трассировка данных — только при включении трассировки для пользовательской конечной точки
* Обучающие и проверочные данные отправлены

По умолчанию данные хранятся в хранилище Майкрософт, а ваша подписка использует ключи шифрования, управляемые корпорацией Майкрософт. Кроме того, у вас есть возможность подготовить собственную учетную запись хранения. Доступ к хранилищу управляется управляемым удостоверением, а служба речи не может напрямую обращаться к собственным данным, таким как данные трассировки речи, обучающие данные и пользовательские модели.

Дополнительные сведения об управляемом удостоверении см. в разделе [что такое управляемые удостоверения](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Приведите собственное хранилище (BYOS) для настройки и ведения журнала

Чтобы запросить доступ к своему хранилищу, заполните и отправьте [форму запроса службы "речь" (BYOS)](https://aka.ms/cogsvc-cmk). После утверждения вам потребуется создать собственную учетную запись хранения для хранения данных, необходимых для настройки и ведения журнала. При добавлении учетной записи хранения ресурс службы речи включит управляемое удостоверение, назначенное системой. После включения управляемого удостоверения, назначенного системой, этот ресурс будет зарегистрирован в Azure Active Directory (AAD). После регистрации управляемому удостоверению будет предоставлен доступ к учетной записи хранения. Дополнительные сведения об управляемых удостоверениях см. здесь. Дополнительные сведения об управляемом удостоверении см. в разделе [что такое управляемые удостоверения](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

> [!IMPORTANT]
> При отключении управляемых удостоверений, назначенных системой, доступ к учетной записи хранения будет удален. Это приведет к тому, что части службы распознавания речи, требующие доступа к учетной записи хранения, перестанут работать.  

Служба "речь" в настоящее время не поддерживает защищенное хранилище. Тем не менее данные клиентов можно хранить с помощью BYOS, что позволяет достичь аналогичных элементов управления данными для [защищенное хранилище](../../security/fundamentals/customer-lockbox-overview.md). Помните, что данные службы речи остаются и обрабатываются в регионе, где был создан речевой ресурс. Это относится к любым неоставшимся данным и передаче данных. При использовании функций настройки, например Пользовательское распознавание речи и пользовательского голоса, все данные клиента передаются, сохраняются и обрабатываются в том же регионе, где находятся ресурсы BYOS (если используется) и служба речи.

> [!IMPORTANT]
> Корпорация Майкрософт **не** использует данные клиентов для улучшения своих речевых моделей. Кроме того, если ведение журнала конечных точек отключено и никакие настройки не используются, данные клиента не сохраняются. 

## <a name="next-steps"></a>Дальнейшие действия

* [Служба распознавания речи — форма запроса собственного хранилища (BYOS)](https://aka.ms/cogsvc-cmk)
* [Что такое управляемые удостоверения](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
