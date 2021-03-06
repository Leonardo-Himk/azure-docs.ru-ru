---
title: Определяемые пользователем функции в шаблонах
description: Описывает, как определять и использовать определяемые пользователем функции в шаблоне Azure Resource Manager.
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 2c09572a460aa028b23987033d2b77e2aad8a0cd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78943216"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Определяемые пользователем функции в шаблоне Azure Resource Manager

В шаблоне можно создать свои собственные функции. Эти функции доступны для использования в шаблоне. Определяемые пользователем функции отделены от [стандартных функций шаблонов](template-functions.md) , которые автоматически доступны в шаблоне. Создавайте собственные функции при наличии сложных выражений, которые многократно используются в шаблоне.

В этой статье описывается добавление определяемых пользователем функций в шаблон Azure Resource Manager.

## <a name="define-the-function"></a>Определение функции

Для функции требуется значение пространства имен, чтобы избежать конфликтов именования с функциями шаблона. В следующем примере показана функция, возвращающая уникальное имя:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

## <a name="use-the-function"></a>Использование функции

В следующем примере показан шаблон, включающий определяемую пользователем функцию. Она использует эту функцию для получения уникального имени для учетной записи хранения. Шаблон имеет параметр с именем **стораженамепрефикс** , который передается в функцию в качестве параметра.

```json
{
 "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
 "contentVersion": "1.0.0.0",
 "parameters": {
   "storageNamePrefix": {
     "type": "string",
     "maxLength": 11
   }
 },
 "functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
 "resources": [
   {
     "type": "Microsoft.Storage/storageAccounts",
     "apiVersion": "2019-04-01",
     "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
     "location": "South Central US",
     "sku": {
       "name": "Standard_LRS"
     },
     "kind": "StorageV2",
     "properties": {
       "supportsHttpsTrafficOnly": true
     }
   }
 ]
}
```

## <a name="limitations"></a>Ограничения

При определении пользовательской функции есть несколько ограничений:

* Функция не может обращаться к переменным.
* Функция может использовать только параметры, определенные в самой функции. При использовании функции [Parameters](template-functions-deployment.md#parameters) в определяемой пользователем функции вы можете использовать только параметры этой функции.
* Функция не может вызывать другие функции, определяемые пользователем.
* Функция не может использовать функцию [Reference](template-functions-resource.md#reference) или любую из функций [списка](template-functions-resource.md#list) .
* Для параметров этой функции нельзя задавать значения по умолчанию.


## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о доступных свойствах определяемых пользователем функций см. в разделе Общие сведения о [структуре и синтаксисе шаблонов Azure Resource Manager](template-syntax.md).
* Список доступных функций шаблонов см. в разделе [функции шаблонов Azure Resource Manager](template-functions.md).
