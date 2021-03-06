---
title: Поставщики многофакторной идентификации Azure — Azure Active Directory
description: Когда следует использовать поставщик аутентификации с Azure MFA?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf50a8f58978a010fe3d8228ace8579fcf52eb38
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81309899"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Когда следует использовать поставщик службы Многофакторной идентификации Azure

> [!IMPORTANT]
> С 1 сентября 2018 года создавать новые поставщики аутентификации невозможно. Существующие поставщики проверки подлинности могут продолжать использоваться и обновляться, но миграция больше невозможна. Многофакторная аутентификация по-прежнему будет предоставляться как функция в лицензиях Azure AD ценовой категории "Премиум".

Двухфакторная проверка подлинности доступна по умолчанию для глобальных администраторов, у которых есть служба Azure Active Directory, и для пользователей Office 365. Однако если вы хотите использовать преимущества [дополнительных функций](howto-mfa-mfasettings.md), необходимо приобрести полную версию службы Многофакторной идентификации Microsoft Azure (MFA).

Поставщик Многофакторной идентификации Azure позволяет пользователям, у которых **нет лицензий**, воспользоваться преимуществами функций Многофакторной идентификации Azure.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Предупреждения о пакете SDK для Azure MFA

Обратите внимание на то, что этот пакет SDK является нерекомендуемым и будет работать только до 14 ноября 2018 года. После этого вызовы к пакету SDK будут завершаться сбоем.

## <a name="what-is-an-mfa-provider"></a>Что такое поставщик службы "Многофакторная идентификация"?

Поставщики аутентификации бывают двух типов. Их отличие заключается в способе оплаты за подписку Azure. При оплате за каждую проверку подлинности вычисляется количество событий проверки подлинности, выполненных для клиента в течение месяца. Этот вариант подойдет, если аутентификация редко выполняется для определенного числа пользователей. При оплате за каждого пользователя вычисляется количество пользователей в клиенте, выполнявших двухфакторную проверку подлинности в течение месяца. Этот вариант подойдет, если количества имеющихся лицензий для пользователей недостаточно для MFA.

## <a name="manage-your-mfa-provider"></a>Управление поставщиком службы "Многофакторная идентификация"

После создания поставщика MFA изменить модель использования невозможно (на включенного пользователя или на отдельную проверку подлинности).

Если вы приобрели достаточно лицензий, чтобы охватить всех пользователей, для которых включена MFA, можно полностью удалить поставщик MFA.

Если поставщик MFA не связан с клиентом Azure AD или новый поставщик MFA связан с другим клиентом Azure AD, пользовательские настройки и параметры конфигурации не будут применяться. Кроме того, вам потребуется повторно активировать существующие серверы Azure MFA с помощью учетных данных активации, созданных с помощью поставщика многофакторной идентификации.

### <a name="removing-an-authentication-provider"></a>Удаление поставщика проверки подлинности

> [!CAUTION]
> При удалении поставщика проверки подлинности подтверждение не выполняется. Выбор **удаления** является постоянным процессом.

Поставщики проверки подлинности можно найти в **портал Azure** > **Azure Active Directory** > **Security** > **MFA** > **поставщиков**mfa. Щелкните список поставщиков, чтобы просмотреть сведения и конфигурации, связанные с этим поставщиком.

Перед удалением поставщика проверки подлинности запишите все настроенные параметры, настроенные в поставщике. Определите, какие параметры необходимо перенести в общие параметры MFA от поставщика, и завершите перенос этих параметров. 

Серверы Azure MFA, связанные с поставщиками, потребуется активировать повторно с помощью учетных данных, созданных в разделе **портал Azure** > **Azure Active Directory** > **Параметры безопасности** > **MFA** > **сервера**mfa. Перед повторной активацией необходимо удалить следующие файлы из `\Program Files\Multi-Factor Authentication Server\Data\` каталога на серверах Azure MFA в вашей среде:

- caCert
- cert
- граупкацерт
- groupKey
- groupName
- Ключ лицензии
- PKEY

![Удаление поставщика проверки подлинности из портал Azure](./media/concept-mfa-authprovider/authentication-provider-removal.png)

Убедившись в том, что все параметры были перенесены, можно перейти к**Security** >  **портал Azure** > **Azure Active Directory** > **поставщики** **MFA** > и выбрать многоточие **...** и нажать кнопку **Удалить**.

> [!WARNING]
> При удалении поставщика проверки подлинности будут удалены все сведения о отчетах, связанные с этим поставщиком. Перед удалением поставщика может потребоваться сохранить отчеты о действиях.

> [!NOTE]
> Пользователям с более старыми версиями приложения Microsoft Authenticator и Azure MFA может потребоваться повторная регистрация приложения.

## <a name="next-steps"></a>Дальнейшие шаги

[Настройка параметров Многофакторной идентификации](howto-mfa-mfasettings.md)
