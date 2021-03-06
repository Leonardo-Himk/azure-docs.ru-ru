---
title: Рекомендации по защите Azure Key Vault
description: Рекомендации по безопасности для Azure Key Vault. Реализация этого руководства поможет вам выполнить обязательства по обеспечению безопасности, как описано в нашей общей модели ответственности.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: article
ms.date: 09/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 0da1a3019124f62aba6a959ce9104c85bd85d3fc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81616495"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Рекомендации по защите Azure Key Vault

Эта статья содержит рекомендации по безопасности для Azure Key Vault. Реализация этих рекомендаций поможет вам удовлетворить обязательства по обеспечению безопасности, как описано в нашей общей модели ответственности. Дополнительные сведения о том, что делает корпорация Майкрософт для выполнения обязанностей поставщиков услуг, см. в статье [Общие обязанности для облачных вычислений](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91).

Некоторые из рекомендаций, описанных в этой статье, можно автоматически отслеживать с помощью центра безопасности Azure. Центр безопасности Azure — это первая строка защиты ресурсов в Azure. Он периодически анализирует состояние безопасности ресурсов Azure и выявляет потенциальные уязвимости системы безопасности. Затем он предоставляет рекомендации по их устранению.

- Дополнительные сведения о рекомендациях центра безопасности Azure см. [в статье рекомендации по безопасности в центре безопасности Azure](../../security-center/security-center-recommendations.md).
- Сведения о центре безопасности Azure см. в статье [что такое центр безопасности Azure?](../../security-center/security-center-intro.md)

## <a name="data-protection"></a>Защита данных

| Рекомендация | Комментарии | Центр безопасности |
|-|----|--|
|Включить обратимое удаление | [Обратимое удаление](overview-soft-delete.md)) позволяет восстанавливать удаленные хранилища и объекты хранилища. |  - |
| Ограничение доступа к данным хранилища  | Следуйте принципу минимальных прав доступа и ограничьте, какие члены вашей организации имеют доступ к данным хранилища. |  - |

## <a name="identity-and-access-management"></a>Управление удостоверениями и доступом

| Рекомендация | Комментарии | Центр безопасности |
|-|----|--|
| Ограничение числа пользователей с доступом участника | Если у пользователя есть разрешения участника на плоскость управления хранилища ключей, он может предоставить себе доступ к плоскости данных, задав политику доступа Key Vault. Вы должны жестко контролировать, кто имеет роль участника на доступ к вашим хранилищам ключей. Убедитесь, что только те, которым требуется доступ авторизованных лиц, могут получать доступ к хранилищам и управлять ими. Вы можете прочитать [безопасный доступ к хранилищу ключей](secure-your-key-vault.md).) | - |

## <a name="monitoring"></a>Наблюдение

| Рекомендация | Комментарии | Центр безопасности |
|-|----|--|
 Журналы диагностики в Key Vault должны быть включены. | Включите журналы и сохраняйте их на год. Это позволит воссоздать следы действий для анализа инцидентов безопасности или при компрометации сети. | [Да](../../security-center/security-center-identity-access.md) |
| Ограничение доступа пользователей к журналам хранилища ключей Azure | [Журналы Key Vault](logging.md)) Сохранение сведений о действиях, выполняемых в хранилище, таких как создание или удаление хранилищ, ключей, секретов, а также использование во время расследования |  - |

## <a name="networking"></a>Сеть

| Рекомендация | Комментарии | Центр безопасности |
|-|----|--|
|Ограничение доступности сети | Доступ к сети должен быть ограничен виртуальными сетями, используемыми решениями, которым требуется доступ к хранилищу. Ознакомьтесь со сведениями о [конечных точках службы виртуальной сети для Azure Key Vault](overview-vnet-service-endpoints.md)) | - |

## <a name="next-steps"></a>Дальнейшие шаги

Обратитесь к поставщику приложения за дополнительными требованиями к безопасности. Дополнительные сведения о разработке безопасных приложений см. в статье [Безопасная документация по разработке](../../security/fundamentals/abstract-develop-secure-apps.md).
