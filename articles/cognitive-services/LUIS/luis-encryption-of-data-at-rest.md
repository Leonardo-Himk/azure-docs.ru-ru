---
title: Language Understanding шифрование неактивных данных в службе
titleSuffix: Azure Cognitive Services
description: Language Understanding шифрование неактивных данных службы.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 59e066974f690bda2384504cc27af5aa94b7b75b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "79372340"
---
# <a name="language-understanding-service-encryption-of-data-at-rest"></a>Language Understanding шифрование неактивных данных в службе

Служба Language Understanding автоматически шифрует данные при их сохранении в облаке. Шифрование службы Language Understanding защищает ваши данные и помогает удовлетворить ваши обязательства по обеспечению безопасности и соответствия требованиям Организации.

## <a name="about-cognitive-services-encryption"></a>Сведения о шифровании Cognitive Services

Данные шифруются и расшифровываются с помощью [fips 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) , совместимого с [256-БИТНЫМ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) шифрованием AES. Шифрование и расшифровка прозрачны, то есть Управление шифрованием и доступом осуществляется за вас. Данные по умолчанию безопасны, и вам не нужно изменять код или приложения, чтобы воспользоваться преимуществами шифрования.

## <a name="about-encryption-key-management"></a>Сведения об управлении ключами шифрования

По умолчанию в подписке используются ключи шифрования, управляемые корпорацией Майкрософт. Существует также возможность управлять подпиской с помощью собственных ключей. Ключи, управляемые клиентом (CMK), обеспечивают большую гибкость при создании, повороте, отключении и отмене контроля доступа. Кроме того, можно выполнять аудит ключей шифрования, используемых для защиты данных.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Управляемые клиентом ключи с использованием Azure Key Vault

Существует также возможность управлять подпиской с помощью собственных ключей. Ключи, управляемые клиентом (CMK), также известные как собственный ключ (BYOK), обеспечивают большую гибкость при создании, повороте, отключении и отмене контроля доступа. Кроме того, можно выполнять аудит ключей шифрования, используемых для защиты данных.

Для хранения ключей, управляемых клиентом, необходимо использовать Azure Key Vault. Можно либо создать собственные ключи и сохранить их в хранилище ключей, либо использовать Azure Key Vault API для создания ключей. Ресурс Cognitive Services и хранилище ключей должны находиться в одном и том же регионе и в одном клиенте Azure Active Directory (Azure AD), но могут находиться в разных подписках. Дополнительные сведения о Azure Key Vault см. в разделе [что такое Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

### <a name="customer-managed-keys-for-language-understanding"></a>Ключи, управляемые клиентом, для Language Understanding

Чтобы запросить возможность использования управляемых клиентом ключей, заполните и отправьте [форму запроса ключа, управляемого клиентом Luis Service](https://aka.ms/cogsvc-cmk). Для получения сведений о состоянии вашего запроса потребуется около 3-5 рабочих дней. В зависимости от спроса вы можете поместить в очередь и утвердить, как только пространство станет доступным. После утверждения для использования CMK с LUIS необходимо создать новый ресурс Language Understanding из портал Azure и выбрать в качестве ценовой категории E0. Новый номер SKU будет работать так же, как SKU F0, который уже доступен, за исключением CMK. Пользователи не смогут выполнить обновление с F0 на новый SKU E0.

Ресурсы E0 доступны только для службы разработки, и уровень E0 изначально будет поддерживаться только в регионе "Западная часть США".

![Изображение подписки LUIS](../media/cognitive-services-encryption/luis-subscription.png)

### <a name="regional-availability"></a>Доступность по регионам

Управляемые клиентом ключи в настоящее время доступны в регионе " **Западная часть США** ".

### <a name="limitations"></a>Ограничения

Существуют некоторые ограничения при использовании уровня E0 с существующими или ранее созданными приложениями.

* Миграция на ресурс E0 будет заблокирована. Пользователи смогут перенести свои приложения только в ресурсы F0. После переноса существующего ресурса в F0 можно создать новый ресурс на уровне E0. Дополнительные сведения о [миграции](https://docs.microsoft.com/azure/cognitive-services/luis/luis-migration-authoring)см. здесь.  
* Перемещение приложений в ресурс E0 или из него будет заблокировано. Чтобы обойти это ограничение, экспортируйте существующее приложение и импортируйте его как ресурс E0.
* Функция проверки орфографии Bing не поддерживается.
* Ведение журнала для конечного пользователя отключено, если приложение находится в состоянии E0.
* Функция Speech подготовка из службы Azure Bot не поддерживается для приложений на уровне E0. Эта функция доступна через службу Azure Bot, которая не поддерживает CMK.
* Для работы функции распознавания речи на портале требуется хранилище BLOB-объектов Azure. Дополнительные сведения см. в статье о том, как [подключить собственное хранилище](../Speech-Service/speech-encryption-of-data-at-rest.md#bring-your-own-storage-byos-for-customization-and-logging).

### <a name="enable-customer-managed-keys"></a>Включить управляемые клиентом ключи

Новый ресурс Cognitive Services всегда шифруется с помощью ключей, управляемых корпорацией Майкрософт. Невозможно включить управляемые клиентом ключи на момент создания ресурса. Ключи, управляемые клиентом, хранятся в Azure Key Vault, а хранилище ключей должно быть подготовлено с политиками доступа, которые предоставляют ключевые разрешения для управляемого удостоверения, связанного с ресурсом Cognitive Services. Управляемое удостоверение доступно только после создания ресурса с использованием ценовой категории для CMK.

Сведения об использовании управляемых клиентом ключей с Azure Key Vault для шифрования Cognitive Services см. в следующих статьях:

- [Настройте ключи, управляемые клиентом, с помощью Key Vault для Cognitive Servicesного шифрования из портал Azure](../Encryption/cognitive-services-encryption-keys-portal.md)

Включение управляемых ключей клиентов также позволит назначить управляемое системой удостоверение, компонент Azure AD. После включения управляемого удостоверения, назначенного системой, этот ресурс будет зарегистрирован в Azure Active Directory. После регистрации управляемому удостоверению будет предоставлен доступ к Key Vault, выбранному во время настройки управляемого ключа клиента. Дополнительные сведения об [управляемых удостоверениях](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)см. здесь.

> [!IMPORTANT]
> При отключении управляемых удостоверений, назначенных системой, доступ к хранилищу ключей будет удален, а все данные, зашифрованные с помощью ключей клиента, станут недоступны. Все функции, зависящие от этих данных, перестанут работать.

> [!IMPORTANT]
> Сейчас управляемые удостоверения не поддерживаются в сценариях работы с разными каталогами. При настройке ключей, управляемых клиентом, в портал Azure управляемое удостоверение автоматически назначается в рамках. Если впоследствии вы перемещаете подписку, группу ресурсов или ресурс из одного каталога Azure AD в другой, управляемое удостоверение, связанное с ресурсом, не передается в новый клиент, поэтому управляемые клиентом ключи могут перестать работать. Дополнительные сведения см. в статье **Передача подписки между каталогами Azure AD** в [часто задаваемых вопросах и известных проблемах с управляемыми удостоверениями для ресурсов Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories).  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Хранение ключей, управляемых клиентом, в Azure Key Vault

Чтобы включить управляемые клиентом ключи, необходимо использовать Azure Key Vault для хранения ключей. Необходимо включить как **обратимое удаление** , так и **не очищать** свойства в хранилище ключей.

При шифровании Cognitive Services поддерживаются только ключи RSA размером 2048. Дополнительные сведения о ключах см. в разделе **Key Vault Keys** раздела [о Azure Key Vault ключах, секретах и сертификатах](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys).

### <a name="rotate-customer-managed-keys"></a>Смена ключей, управляемых клиентом

Вы можете поворачивать ключ, управляемый клиентом, в Azure Key Vault в соответствии с политиками соответствия требованиям. При вращении ключа необходимо обновить ресурс Cognitive Services для использования нового URI ключа. Сведения о том, как обновить ресурс для использования новой версии ключа в портал Azure, см. в разделе **обновление ключа версии** раздела [Настройка управляемых клиентом ключей для Cognitive Services с помощью портал Azure](../Encryption/cognitive-services-encryption-keys-portal.md).

Вращение ключа не вызывает повторного шифрования данных в ресурсе. От пользователя не требуется предпринимать никаких действий.

### <a name="revoke-access-to-customer-managed-keys"></a>Отозвать доступ к ключам, управляемым клиентом

Чтобы отозвать доступ к ключам, управляемым клиентом, используйте PowerShell или Azure CLI. Дополнительные сведения см. в разделе [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault//) или [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault). Отзыв доступа позволяет блокировать доступ ко всем данным в Cognitive Services ресурсе, так как ключ шифрования недоступен для Cognitive Services.

## <a name="next-steps"></a>Следующие шаги

* [Форма запроса ключа, управляемого клиентом LUIS Service](https://aka.ms/cogsvc-cmk)
* [Дополнительные сведения о Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
