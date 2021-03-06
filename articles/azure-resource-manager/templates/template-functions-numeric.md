---
title: Функции шаблонов — числовые
description: Описывает функции, используемые в шаблоне Azure Resource Manager для работы с числами.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: dc15ade453fc5ea4dc031ced0377892f4f8cf27d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192354"
---
# <a name="numeric-functions-for-arm-templates"></a>Числовые функции для шаблонов ARM

Диспетчер ресурсов предоставляет следующие функции для работы с целыми числами в шаблоне Azure Resource Manager (ARM):

* [add](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [float](#float)
* [int](#int)
* [max](#max)
* [минимум](#min)
* [взят](#mod)
* [mul](#mul)
* [Директор](#sub)

## <a name="add"></a>add

`add(operand1, operand2)`

Возвращает сумму двух указанных целочисленных значений.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
|операнд1 |Да |INT |Первое слагаемое. |
|операнд2 |Да |INT |Второе слагаемое. |

### <a name="return-value"></a>Возвращаемое значение

Целое число, содержащее сумму параметров.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) суммируются два параметра.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| addResult | Int | 8 |

## <a name="copyindex"></a>copyIndex

`copyIndex(loopName, offset)`

Возвращает индекс цикла итерации.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| loopName | Нет | строка | Имя цикла для получения итерации. |
| offset |Нет |INT |Число, добавляемое к отсчитываемому от нуля значению итерации. |

### <a name="remarks"></a>Remarks

Эта функция всегда используется с объектом **copy**. Если значение **offset** не указано, возвращается текущее значение итерации. Значение итерации начинается с нуля.

Свойство **loopName** позволяет указать, на какую итерацию ссылается copyIndex: ресурса или свойства. Если значение **loopName** не указано, используется текущая итерация типа ресурса. Укажите значение **loopName** при выполнении итерации по свойству.

Дополнительные сведения об использовании копирования см. в следующих статьях:

* [Итерация ресурсов в шаблонах ARM](copy-resources.md)
* [Итерация свойства в шаблонах ARM](copy-properties.md)
* [Итерация переменных в шаблонах ARM](copy-variables.md)
* [Выходная итерация в шаблонах ARM](copy-outputs.md)

### <a name="example"></a>Пример

В следующем примере представлен цикл копирования, в котором значение индекса включается в имя.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {}
}
```

### <a name="return-value"></a>Возвращаемое значение

Целое число, представляющее текущей индекс итерации.

## <a name="div"></a>div

`div(operand1, operand2)`

Возвращает целую часть частного от деления двух указанных целочисленных значений.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| операнд1 |Да |INT |Делимое. |
| операнд2 |Да |INT |Делитель. Не может быть равно 0. |

### <a name="return-value"></a>Возвращаемое значение

Целое число, представляющее деление.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) один параметр делится на другой.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| divResult | Int | 2 |

## <a name="float"></a>FLOAT

`float(arg1)`

Преобразует значение в число с плавающей запятой. Эта функция используется только при передаче пользовательских параметров в приложение, такое как приложение логики.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| arg1 |Да |строка или целое число |Значение, которое необходимо преобразовать в число с плавающей запятой. |

### <a name="return-value"></a>Возвращаемое значение

Число с плавающей запятой.

### <a name="example"></a>Пример

В следующем примере показано, как с помощью функции float передать параметры в приложение логики:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
            "custom1": {
                "value": "[float('3.0')]"
            },
            "custom2": {
                "value": "[float(3)]"
            },
```

## <a name="int"></a>INT

`int(valueToConvert)`

Преобразует указанное значение в целое число.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| valueToConvert |Да |строка или целое число |Значение, которое необходимо преобразовать в целое число. |

### <a name="return-value"></a>Возвращаемое значение

Целое число преобразованного значения.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) указанное пользователем значение параметра преобразуется в целое число.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": {
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| intResult | Int | 4 |

## <a name="max"></a>max

`max (arg1)`

Возвращает максимальное значение из массива целых чисел или разделенный запятыми список целых чисел.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| arg1 |Да |массив целых чисел или разделенный запятыми список целых чисел |Коллекция, для которой необходимо получить максимальное значение. |

### <a name="return-value"></a>Возвращаемое значение

Целое число, представляющее максимальное значение из коллекции.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) показано, как использовать функцию max с массивом и списком целых чисел.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

## <a name="min"></a>Min

`min (arg1)`

Возвращает минимальное значение из массива целых чисел или разделенный запятыми список целых чисел.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| arg1 |Да |массив целых чисел или разделенный запятыми список целых чисел |Коллекция, для которой необходимо получить минимальное значение. |

### <a name="return-value"></a>Возвращаемое значение

Целое число, представляющее минимальное значение из коллекции.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) показано, как использовать функцию min с массивом и списком целых чисел.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

## <a name="mod"></a>mod

`mod(operand1, operand2)`

Возвращает остаток от деления двух указанных целочисленных значений.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| операнд1 |Да |INT |Делимое. |
| операнд2 |Да |INT |Число, используемое для деления, не может быть равно 0. |

### <a name="return-value"></a>Возвращаемое значение

Целое число, представляющее остаток.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) возвращается остаток от деления одного параметра на другой.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| modResult | Int | 1 |

## <a name="mul"></a>mul

`mul(operand1, operand2)`

Возвращает произведение двух указанных целочисленных значений.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| операнд1 |Да |INT |Первый множитель. |
| операнд2 |Да |INT |Второй множитель. |

### <a name="return-value"></a>Возвращаемое значение

Целое число, представляющее произведение.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) один параметр умножается на другой.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

## <a name="sub"></a>sub

`sub(operand1, operand2)`

Возвращает разность двух указанных целочисленных значений.

### <a name="parameters"></a>Параметры

| Параметр | Обязательно | Type | Описание |
|:--- |:--- |:--- |:--- |
| операнд1 |Да |INT |Уменьшаемое. |
| операнд2 |Да |INT |Вычитаемое. |

### <a name="return-value"></a>Возвращаемое значение

Целое число, представляющее разность.

### <a name="example"></a>Пример

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) один параметр вычитается из другого.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| Имя | Тип | Значение |
| ---- | ---- | ----- |
| subResult | Int | 4 |

## <a name="next-steps"></a>Следующие шаги

* Описание разделов в шаблоне Azure Resource Manager см. [в разделе Общие сведения о структуре и синтаксисе шаблонов ARM](template-syntax.md).
* Чтобы выполнить итерацию указанного числа раз при создании типа ресурса, см. раздел [Создание нескольких экземпляров ресурсов в Azure Resource Manager](copy-resources.md).
