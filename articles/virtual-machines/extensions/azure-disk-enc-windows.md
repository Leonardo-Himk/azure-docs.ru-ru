---
title: Шифрование дисков Azure для Windows
description: Развертывание шифрования дисков Azure на виртуальной машине Windows с помощью расширения виртуальной машины.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/19/2020
ms.author: ejarvi
ms.openlocfilehash: e975e1757b77b4aab52a59d1f0709ef9cadae94e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80066865"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Шифрование дисков Azure для Windows (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>Обзор

При шифровании дисков Azure используется Bitlocker для полного шифрования диска на виртуальных машинах Azure под управлением Windows.  Шифрование дисков Azure интегрировано в Azure Key Vault, что позволяет управлять секретами и ключами шифрования дисков в подписке Key Vault. 

## <a name="prerequisites"></a>Предварительные условия

Полный список необходимых компонентов см. в разделе [Шифрование дисков Azure для виртуальных машин Windows](../windows/disk-encryption-overview.md), в частности следующие разделы:

- [Поддерживаемые виртуальные машины и операционные системы](../windows/disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Требования к сети](../windows/disk-encryption-overview.md#networking-requirements)
- [Требования к групповая политика](../windows/disk-encryption-overview.md#group-policy-requirements)

## <a name="extension-schema"></a>Схема расширения

Существует две версии схемы расширения для шифрования дисков Azure (ADE):
- v 2.2 — Новая Рекомендуемая схема, которая не использует свойства Azure Active Directory (AAD).
- v 1.1 — старая схема, для которой требуются свойства Azure Active Directory (AAD). 

Чтобы выбрать целевую схему, `typeHandlerVersion` свойство должно иметь значение, равное версии схемы, которую вы хотите использовать.

### <a name="schema-v22-no-aad-recommended"></a>Схема v 2.2: нет AAD (рекомендуется)

Схема v 2.2 рекомендуется для всех новых виртуальных машин и не требует Azure Active Directory свойств.

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "AzureDiskEncryption",
        "typeHandlerVersion": "2.2",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "EncryptionOperation": "[encryptionOperation]",
          "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
          "KeyVaultURL": "[keyVaultURL]",
          "KekVaultResourceId": "[keyVaultResourceID]",
          "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KeyVaultResourceId": "[keyVaultResourceID]",
          "SequenceVersion": "sequenceVersion]",
          "VolumeType": "[volumeType]"
        }
  }
}
```


### <a name="schema-v11-with-aad"></a>Схема v 1.1: с AAD 

Для новых виртуальных машин `aadClientID` не рекомендуется `aadClientSecret` использовать `AADClientCertificate` схему 1,1.

Использование среды `aadClientSecret`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "1.1",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    }
  }
}
```

Использование среды `AADClientCertificate`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientCertificate": "[aadClientCertificate]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "1.1",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    }
  }
}
```


### <a name="property-values"></a>Значения свойств

| Имя | Значение и пример | Тип данных |
| ---- | ---- | ---- |
| версия_API | 2019-07-01 | Дата |
| publisher | Microsoft.Azure.Security | строка |
| type | AzureDiskEncryption | строка |
| typeHandlerVersion | 2,2, 1,1 | строка |
| (схема 1,1) аадклиентид | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | guid | 
| (схема 1,1) AADClientSecret | пароль | строка |
| (схема 1,1) аадклиентцертификате | thumbprint | строка |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | строка | 
| (необязательно-по умолчанию RSA-OAEP) кэйенкриптионалгорисм | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | строка |
| KeyVaultURL | url | строка |
| кэйваултресаурцеид | url | строка |
| используемых KeyEncryptionKeyURL | url | строка |
| используемых кекваултресаурцеид | url | строка |
| используемых секуенцеверсион | UNIQUEIDENTIFIER | строка |
| VolumeType | ОС, данные, все | строка |

## <a name="template-deployment"></a>Развертывание шаблона

Пример развертывания шаблона на основе схемы v 2.2 см. в статье шаблон быстрого запуска Azure [201-Encrypting-Windows-VM-без-AAD](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad).

Пример развертывания шаблона на основе схемы v 1.1 см. в статье шаблон быстрого запуска Azure [201-Encrypting-Windows-VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

>[!NOTE]
> Кроме того `VolumeType` , если параметр имеет значение ALL, диски данных будут шифроваться только в том случае, если они имеют правильный формат. 

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Дополнительные сведения см. в [руководстве по устранению неполадок с шифрованием дисков Azure](../windows/disk-encryption-troubleshooting.md).

### <a name="support"></a>Поддержка

Если вам нужна дополнительная помощь в любой момент в этой статье, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack overflow](https://azure.microsoft.com/support/community/). 

Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите в [службу поддержки Azure](https://azure.microsoft.com/support/options/) и выберите получить поддержку. Сведения об использовании службы поддержки Azure см. в статье [часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Дальнейшие шаги

* См. дополнительные сведения о [расширениях и компонентах виртуальных машин Windows](features-windows.md).
* Дополнительные сведения о шифровании дисков Azure для Windows см. в статье [виртуальные машины Windows](../../security/fundamentals/azure-disk-encryption-vms-vmss.md#windows-virtual-machines).
