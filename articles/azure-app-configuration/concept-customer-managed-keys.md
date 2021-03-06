---
title: Использование управляемых клиентом ключей для шифрования данных конфигурации
description: Шифрование данных конфигурации с помощью управляемых клиентом ключей
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/18/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: ace34cf4a72b871ba6646b279007b8ce21c03e9b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81457439"
---
# <a name="use-customer-managed-keys-to-encrypt-your-app-configuration-data"></a>Использование управляемых клиентом ключей для шифрования данных конфигурации приложения
Конфигурация приложения Azure [шифрует неактивных конфиденциальных данных](../security/fundamentals/encryption-atrest.md). Использование управляемых клиентом ключей обеспечивает улучшенную защиту данных, позволяя управлять ключами шифрования.  При использовании шифрования управляемого ключа все конфиденциальные данные в конфигурации приложения шифруются с помощью предоставленного пользователем ключа Azure Key Vault.  Это дает возможность смены ключа шифрования по запросу.  Кроме того, она позволяет отозвать доступ конфигурации приложения Azure к конфиденциальным сведениям, отменив доступ экземпляра конфигурации приложения к ключу.

## <a name="overview"></a>Обзор 
Конфигурация приложения Azure шифрует конфиденциальные данные при хранении с помощью 256-разрядного ключа шифрования AES, предоставляемого корпорацией Майкрософт. Каждый экземпляр конфигурации приложения имеет собственный ключ шифрования, который управляется службой и используется для шифрования конфиденциальной информации. Конфиденциальная информация включает значения, содержащиеся в парах "ключ-значение".  Если функция ключа, управляемого клиентом, включена, в конфигурации приложения используется управляемое удостоверение, назначенное экземпляру конфигурации приложения для проверки подлинности с помощью Azure Active Directory. Затем управляемое удостоверение вызывает Azure Key Vault и заключает в оболочку ключ шифрования экземпляра конфигурации приложения. Затем будет сохранен инкапсулированный ключ шифрования, а неупакованный ключ шифрования будет кэшироваться в конфигурации приложения в течение одного часа. Конфигурация приложения обновляет неупакованную версию ключа шифрования экземпляра конфигурации приложения ежечасно. Это обеспечит доступность при нормальных условиях работы. 

>[!IMPORTANT]
> Если удостоверению, назначенному экземпляру конфигурации приложения, больше не разрешено разворачивать ключ шифрования экземпляра, или если управляемый ключ безвозвратно удален, то будет невозможно расшифровать конфиденциальные данные, хранящиеся в экземпляре конфигурации приложения. Использование функции [обратимого удаления](../key-vault/general/overview-soft-delete.md) Azure Key Vault снижает вероятность случайного удаления ключа шифрования.

Когда пользователи включают возможности управляемого ключа клиента в своем экземпляре конфигурации приложения Azure, они управляют возможностью службы обращаться к их конфиденциальной информации. Управляемый ключ служит в качестве корневого ключа шифрования. Пользователь может отозвать доступ экземпляра конфигурации приложения к своему управляемому ключу, изменив политику доступа к хранилищу ключей. Когда этот доступ будет отозван, Конфигурация приложения потеряет возможность расшифровывать данные пользователя в течение одного часа. На этом этапе экземпляр конфигурации приложения запрещает все попытки доступа. Эта ситуация может быть восстановлена путем предоставления службе доступа к управляемому ключу снова.  В течение одного часа Конфигурация приложения сможет расшифровать данные пользователя и работать в нормальных условиях.

>[!NOTE]
>Все данные конфигурации приложения Azure хранятся в изолированной резервной копии до 24 часов. К ним относится неупакованный ключ шифрования. Эти данные не сразу доступны группе службы или службы. В случае аварийного восстановления Конфигурация приложения Azure будет повторно отзывать себя из управляемых данных ключа.

## <a name="requirements"></a>Requirements (Требования)
Чтобы успешно включить управляемую клиентом функцию ключа для конфигурации приложений Azure, необходимы следующие компоненты:
- Экземпляр конфигурации приложения Azure уровня "Стандартный"
- Azure Key Vault с включенными функциями обратимого удаления и очистки
- Ключ RSA или RSA-HSM в Key Vault
    - Срок действия ключа не должен истечь, он должен быть включен и должен быть включен и поддержка функций переноса, и для распаковки.

После настройки этих ресурсов необходимо выполнить два действия, чтобы разрешить конфигурации приложения Azure использовать ключ Key Vault:
1. Назначение управляемого удостоверения экземпляру конфигурации приложения Azure
2. Предоставьте удостоверению `GET`, `WRAP`и `UNWRAP` разрешения в политике доступа целевого Key Vault.

## <a name="enable-customer-managed-key-encryption-for-your-azure-app-configuration-instance"></a>Включение шифрования ключа, управляемого клиентом, для экземпляра конфигурации приложения Azure
Для начала вам потребуется правильно настроить экземпляр конфигурации приложения Azure. Если у вас еще нет экземпляра конфигурации приложения, воспользуйтесь одним из следующих кратких руководств, чтобы настроить их.
- [Создание приложения ASP.NET Core с помощью конфигурации приложения Azure](quickstart-aspnet-core-app.md)
- [Создание приложения .NET Core с помощью конфигурации приложения Azure](quickstart-dotnet-core-app.md)
- [Создание приложения на .NET Framework с помощью службы конфигурации приложений Azure](quickstart-dotnet-app.md)
- [Создание приложения Java Spring с помощью службы "Конфигурация приложений Azure"](quickstart-java-spring-app.md)

>[!TIP]
> Azure Cloud Shell — это бесплатная интерактивная оболочка, с помощью которой можно выполнять инструкции командной строки, описанные в этой статье.  Она содержит предварительно установленные общие инструменты Azure, включая пакет SDK для .NET Core. Если вы вошли в подписку Azure, запустите [Azure Cloud Shell](https://shell.azure.com) на сайте shell.azure.com.  Дополнительные сведения об Azure Cloud Shell см. в [нашей документации](../cloud-shell/overview.md)

### <a name="create-and-configure-an-azure-key-vault"></a>Создание и настройка Azure Key Vault
1. Создайте Azure Key Vault с помощью Azure CLI.  Обратите внимание `vault-name` , `resource-group-name` что и, и, являются предоставленными пользователем и должны быть уникальными.  Мы используем `contoso-vault` и `contoso-resource-group` в этих примерах.

    ```azurecli
    az keyvault create --name contoso-vault --resource-group contoso-resource-group
    ```
    
1. Включите обратимое удаление и очистку для Key Vault. Замените имена Key Vault (`contoso-vault`) и группы ресурсов (`contoso-resource-group`), созданной на шаге 1.

    ```azurecli
    az keyvault update --name contoso-vault --resource-group contoso-resource-group --enable-purge-protection --enable-soft-delete
    ```
    
1. Создайте ключ Key Vault. Укажите уникальный `key-name` для этого ключа и замените имена Key Vault (`contoso-vault`), созданных на шаге 1. Укажите, следует ли `RSA` использовать `RSA-HSM` или шифровать.

    ```azurecli
    az keyvault key create --name key-name --kty {RSA or RSA-HSM} --vault-name contoso-vault
    ```
    
    Выходные данные этой команды показывают идентификатор ключа ("Детская") для созданного ключа.  Запишите идентификатор ключа, который будет использоваться далее в этом упражнении.  Идентификатор ключа имеет вид: `https://{my key vault}.vault.azure.net/keys/{key-name}/{Key version}`.  Идентификатор ключа имеет три важных компонента:
    1. Key Vault URI: "https://{мое хранилище ключей}. Vault. Azure. NET
    1. Имя ключа Key Vault: {имя ключа}
    1. Key Vault версия ключа: {версия ключа}

1. Создайте управляемое удостоверение, назначенное системой, с помощью Azure CLI, подставив имя экземпляра конфигурации приложения и группы ресурсов, которые использовались на предыдущих шагах. Управляемое удостоверение будет использоваться для доступа к управляемому ключу. Мы используем `contoso-app-config` для иллюстрации имени экземпляра конфигурации приложения:
    
    ```azurecli
    az appconfig identity assign --na1. me contoso-app-config --group contoso-resource-group --identities [system]
    ```
    
    Выходные данные этой команды включают идентификатор субъекта ("principalId") и идентификатор клиента ("Тенандид") назначенного системой удостоверения.  Он будет использоваться для предоставления удостоверению доступа к управляемому ключу.

    ```json
    {
    "principalId": {Principal Id},
    "tenantId": {Tenant Id},
    "type": "SystemAssigned",
    "userAssignedIdentities": null
    }
    ```

1. Управляемому удостоверению экземпляра конфигурации приложения Azure требуется доступ к ключу для выполнения проверки ключа, шифрования и расшифровки. Конкретный набор действий, к которым требуется доступ `GET`, включает, `WRAP`и `UNWRAP` для ключей.  Для предоставления доступа требуется идентификатор участника управляемого удостоверения экземпляра конфигурации приложения. Это значение было получено на предыдущем шаге. Он показан ниже в виде `contoso-principalId`. Предоставьте разрешение на управляемый ключ с помощью командной строки:

    ```azurecli
    az keyvault set-policy -n contoso-vault --object-id contoso-principalId --key-permissions get wrapKey unwrapKey
    ```

1. После того как экземпляр конфигурации приложения Azure сможет получить доступ к управляемому ключу, можно включить управляемую клиентом функцию ключа в службе с помощью Azure CLI. Вспомним следующие свойства, записанные при создании ключа `key name` `key vault URI`:.

    ```azurecli
    az appconfig update -g contoso-resource-group -n contoso-app-config --encryption-key-name key-name --encryption-key-version key-version --encryption-key-vault key-vault-Uri
    ```

Теперь экземпляр конфигурации приложения Azure настроен для использования управляемого клиентом ключа, хранящегося в Azure Key Vault.

## <a name="next-steps"></a>Дальнейшие шаги
В этой статье вы настроили в экземпляре конфигурации приложения Azure использование ключа, управляемого клиентом, для шифрования.  Узнайте, как [интегрировать службу с управляемыми удостоверениями Azure](howto-integrate-azure-managed-service-identity.md).