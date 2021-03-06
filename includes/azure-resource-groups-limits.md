---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: c883383d3c870689bb95f808f6f60c5185c165c3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80334656"
---
| Ресурс | Ограничение |
| --- | --- |
| Ресурсов на [группу ресурсов](../articles/azure-resource-manager/management/overview.md#resource-groups) | Ресурсы не ограничиваются группой ресурсов. Вместо этого они ограничиваются типом ресурса в группе ресурсов. См. следующую строку. |
| Ресурсов на группу ресурсов на каждый тип ресурсов |800-некоторые типы ресурсов могут превышать ограничение 800. См. [ресурсы, не ограниченные 800 экземплярами на группу ресурсов](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md). |
| Развертываний на группу ресурсов в журнале развертывания |800<sup>1</sup> |
| Ресурсов в развертывании |800 |
| Блокировок управления на уникальную область |20 |
| Число тегов на ресурс или группу ресурсов |50 |
| Длина ключа тега |512 |
| Длина значения тега |256 |

<sup>1</sup> Если достигнут предел в 800 развертываний на группу ресурсов, удалите из журнала развертывания, которые больше не нужны. Удаление записи из журнала развертывания не влияет на развернутые ресурсы. Дополнительные сведения см. в разделе [Устранение ошибки, когда число развертываний превышает 800](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md).

#### <a name="template-limits"></a>Ограничения шаблонов

| Применение | Ограничение |
| --- | --- |
| Параметры |256 |
| Переменные |256 |
| Ресурсы (включая число копий) |800 |
| Выходные данные |64 |
| Выражение шаблона |24 576 символов |
| Ресурсы в экспортированных шаблонах |200 |
| Размер шаблона |4 МБ |
| Размер файла параметров |64 КБ |

Некоторые ограничения можно превысить, используя вложенные шаблоны. Дополнительные сведения см. в статье [Использование связанных шаблонов при развертывании ресурсов Azure](../articles/azure-resource-manager/templates/linked-templates.md). Чтобы уменьшить число параметров, переменных или выходных данных, можно объединить несколько значений в объект. См. дополнительные сведения об [использовании объектов как параметров](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
