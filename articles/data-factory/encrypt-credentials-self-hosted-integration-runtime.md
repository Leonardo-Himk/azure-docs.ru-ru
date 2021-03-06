---
title: Шифрование учетных данных в фабрике данных Azure
description: Узнайте, как зашифровать и сохранить учетные данные для локальных хранилищ данных на компьютере с локальной средой выполнения интеграции.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: cd775c5a3bf367600a4537a9409a9bb8f902f588
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628986"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Шифрование учетных данных для локальных хранилищ данных в фабрике данных Azure

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Вы можете зашифровать и сохранить учетные данные для хранилищ данных в локальной среде (связанные службы с конфиденциальной информацией) на компьютере с локальной средой выполнения интеграции. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Передайте файл определения JSON с учетными данными в <br/>Командлет [**New-AzDataFactoryV2LinkedServiceEncryptedCredential**](/powershell/module/az.datafactory/New-AzDataFactoryV2LinkedServiceEncryptedCredential) для создания выходного файла определения JSON с зашифрованными учетными данными. Затем с помощью обновленного определения JSON создайте связанные службы.

## <a name="author-sql-server-linked-service"></a>Создание связанной службы SQL Server
В любой папке создайте JSON-файл с именем **SqlServerLinkedService.json** со следующим содержимым:  

Прежде чем сохранить файл, замените `<servername>`, `<databasename>`, `<username>` и `<password>` значениями своего сервера SQL Server. Кроме того, замените `<integration runtime name>` именем своей среды выполнения интеграции. 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Шифрование учетных данных
Чтобы зашифровать конфиденциальные данные из полезных данных JSON в локальной среде выполнения интеграции, выполните команду **New-AzDataFactoryV2LinkedServiceEncryptedCredential**и передайте полезные данные JSON. Этот командлет гарантирует, что учетные данные шифруются с помощью API защиты данных и сохраняются локально на узле локальной среда выполнения интеграции. Выходные данные выходных данных, содержащие зашифрованную ссылку на учетные данные, можно перенаправить в другой JSON-файл (в данном случае "encryptedLinkedService. JSON").

```powershell
New-AzDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Использование JSON с зашифрованными учетными данными
Теперь используйте выходной файл JSON, содержащий зашифрованные учетные данные, из предыдущей команды для настройки **SqlServerLinkedService**.

```powershell
Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Дальнейшие действия
Сведения о рекомендациях по безопасному перемещению данных см. в статье [Azure Data Factory — Security considerations for data movement](data-movement-security-considerations.md) (Вопросы безопасности при перемещении данных в фабрике данных Azure).

