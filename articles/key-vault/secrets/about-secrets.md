---
title: Сведения о секретах Azure Key Vault — Azure Key Vault
description: В этой статье приведены общие сведения об интерфейсе Azure Key Vault REST и подробные сведения о секретах.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: overview
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: eabfa03aa70f54a967fe256f694ef59ad0fe7ebe
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685440"
---
# <a name="about-azure-key-vault-secrets"></a>Сведения о секретах Azure Key Vault

Key Vault обеспечивает безопасное хранение секретов, таких как пароли и строки подключения к базам данных.

С точки зрения разработчика API-интерфейсы Key Vault принимают и возвращают значения секретов в виде строк. На внутреннем уровне Key Vault хранит секреты и управляет ими в виде последовательности октетов (8 бит) максимальным объемом 25 килобайтов каждый. Служба Key Vault не предоставляет никакой семантики для секретов. Она просто принимает данные, шифрует и сохраняет их, возвращая идентификатор секрета — "id". Идентификатор может использоваться, чтобы получить секрет позже.  

Клиентам следует рассмотреть дополнительные уровни защиты конфиденциальных данных. Шифрование данных с помощью отдельного ключа защиты перед помещением в хранилище в Key Vault является одним из примеров.  

Key Vault также поддерживает поле contentType для секретов. Клиенты могут указать тип содержимого секрета, чтобы помочь в интерпретации данных секрета при извлечении. Максимальная длина этого поля — 255 символов. Предварительно определенные значения отсутствуют. Предлагаемое использование — это подсказка для интерпретации данных секрета. Например, реализация может сохранить пароли и сертификаты как секреты, а затем использовать это поле для их распознания. Предварительно определенные значения отсутствуют.  

## <a name="secret-attributes"></a>Атрибуты секретов

В дополнение к секретным данным можно указать следующие атрибуты:  

- *exp*. Необязательный атрибут со значением в формате IntDate, значение по умолчанию — **forever**. Атрибут *exp* (время окончания срока действия) определяет срок действия, в течение или после которого данные секрета НЕ ДОЛЖНЫ быть получены, за исключением [определенных ситуаций](#date-time-controlled-operations). Это поле предназначено для **информационных** целей только потому, что оно информирует пользователей службы хранилища ключей, что конкретный секрет не может использоваться. Нужно указать число, содержащее значение IntDate.   
- *nbf*. Необязательный атрибут со значением в формате IntDate, значение по умолчанию — **now**. Атрибут *nbf* (не ранее) определяет время, до которого данные секрета НЕ ДОЛЖНЫ быть получены, за исключением [определенных ситуаций](#date-time-controlled-operations). Это поле предназначено только для **информационных** целей. Нужно указать число, содержащее значение IntDate. 
- *enabled*. Необязательный атрибут с логическим значением, по умолчанию — **true**. Этот атрибут указывает, можно ли извлечь данные секрета. Атрибут enabled используется совместно с *nbf* и *exp* при выполнении операции между *nbf* и *exp*. Она будет разрешена, только если атрибут enabled имеет значение **true**. Операции вне окон *nbf* и *exp* автоматически запрещаются, за исключением [определенных ситуаций](#date-time-controlled-operations).  

Есть дополнительные атрибуты только для чтения, которые включены в любой ответ, который содержит атрибуты секрета:  

- *Создано*. Необязательное значение в формате IntDate. Атрибут created указывает, когда была создана эта версия секрета. Это значение равно NULL для секретов, созданных перед добавлением данного атрибута. Нужно указать число, содержащее значение IntDate.  
- *Обновлено*. Необязательное значение в формате IntDate. Атрибут updated указывает, когда была обновлена эта версия секрета. Это значение равно NULL для секретов, которые в последний раз обновлялись перед добавлением данного атрибута. Нужно указать число, содержащее значение IntDate.

### <a name="date-time-controlled-operations"></a>Операции, зависящие от даты и времени

Операцию **получения** секрета можно применять для еще не проверенных секретов и просроченных секретов за пределами окна *nbf* / *exp*. Вызов операции секрета **get** для еще не проверенных секретов может использоваться для тестирования. Извлечение (**получение**) просроченного секрета может использоваться в операциях восстановления.

## <a name="secret-access-control"></a>Контроль доступа к секретам

Контроль доступа к секретам, управляемым в Key Vault, предоставляется на уровне Key Vault, который содержит эти секреты. Существует политика управления доступом к секретам, отличная от политики управления доступом к секретам в том же Key Vault. Пользователи могут создать одно или несколько хранилищ для хранения секретов и должны поддерживать соответствующую сценарию сегментацию и управление секретами.   

Следующие разрешения могут использоваться на уровне субъекта в записи управления доступом к секретам в хранилище. Они практически точно отражают операции, разрешенные в объекте секрета.  

- Разрешения для операций управления секретами
  - *get*. Чтение секрета.  
  - *list*: Вывод списка секретов или их версий, хранимых в Key Vault.  
  - *set*. Создание секрета  
  - *delete*: Удаление секрета.  
  - *recover*. Восстановление удаленного секрета.
  - *backup*. Архивация секрета в хранилище ключей.
  - *restore*. Восстановление заархивированного секрета в хранилище ключей.

- Разрешения для привилегированных операций
  - *purge*. Очистка удаленного секрета (удаление без возможности восстановления).

Дополнительные сведения о работе с секретами см. в [справочнике по работе с Azure Key Vault с помощью REST API](/rest/api/keyvault). Сведения об установке разрешений см. в статьях [Vaults — Create Or Update](/rest/api/keyvault/vaults/createorupdate) (Хранилища. Создание или обновление) и [Vaults — Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) (Хранилища. Обновление политики доступа). 

## <a name="secret-tags"></a>Теги секретов  
В форме тегов можно указать дополнительные метаданные для конкретного приложения. Key Vault поддерживает до 15 тегов, каждый из которых может иметь имя и значение длиной 256 знаков.  

>[!Note]
>Теги могут быть прочитаны вызывающим объектом, если у него есть права *list* или *get*.

## <a name="azure-storage-account-key-management"></a>Управление ключами учетных записей хранения Azure

Key Vault может управлять ключами учетных записей хранения Azure:

- На внутреннем уровне Key Vault может выводить список ключей (синхронизировать их) с помощью учетной записи хранения Azure. 
- Key Vault периодически повторно создает (сменяет) ключи.
- Значения ключей никогда не возвращаются в ответе вызывающему объекту.
- Azure Key Vault управляет ключами как учетных записей хранения, так и классических учетных записей хранения.

Дополнительные сведения см. в статье [Manage storage account keys with Key Vault and the Azure CLI](../secrets/overview-storage-keys.md) (Управление ключами учетной записи хранения с помощью Key Vault и Azure CLI).

## <a name="storage-account-access-control"></a>Управление доступом к учетной записи хранения

Следующие разрешения можно использовать при авторизации пользователя или субъекта приложения для выполнения операций с управляемой учетной записью хранения:  

- Разрешения для управляемой учетной записи хранения и операции определения SaS
  - *get*. Получение сведений об учетной записи хранения. 
  - *list*: Список учетных записей хранения, управляемых Key Vault.
  - *update*: Обновление учетной записи хранения.
  - *delete*: Удаление учетной записи хранения  
  - *recover*. Восстановление удаленной учетной записи хранения.
  - *backup*. Архивация учетной записи хранения.
  - *restore*. Восстановление заархивированной учетной записи хранения в Key Vault.
  - *set*. Обновление или создание учетной записи хранения.
  - *regeneratekey*. Повторное создание значения указанного ключа для учетной записи хранения.
  - *getsas*. Получение сведений об определении SAS для учетной записи хранения.
  - *listsas*. Вывод списка определений SAS хранилища для учетной записи хранения.
  - *deletesas*. Удаление определения SAS из учетной записи хранения.
  - *setsas*. Создание или обновление нового определения или атрибутов SAS для учетной записи хранения.

- Разрешения для привилегированных операций
  - *purge*. Очистка (удаление без возможности восстановления) управляемой учетной записи хранения.

Дополнительные сведения о работе с сертификатами см. в статье [Azure Key Vault REST API reference](/rest/api/keyvault) (Справочник по REST API для Azure Key Vault). Сведения об установке разрешений см. в статьях [Vaults — Create Or Update](/rest/api/keyvault/vaults/createorupdate) (Хранилища. Создание или обновление) и [Vaults — Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) (Хранилища. Обновление политики доступа).

## <a name="next-steps"></a>Дальнейшие действия

- [Сведения о Key Vault](../general/overview.md)
- [Сведения о ключах, секретах и сертификатах](../general/about-keys-secrets-certificates.md)
- [Сведения о ключах](../keys/about-keys.md)
- [Сведения о сертификатах](../certificates/about-certificates.md)
- [Аутентификация, запросы и ответы](../general/authentication-requests-and-responses.md)
- [Руководство разработчика Azure Key Vault](../general/developers-guide.md)
