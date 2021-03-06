---
title: Шифрование данных в таблице службы хранилища Azure | Документация Майкрософт
description: Сведения о шифровании данных таблиц в службе хранилища Azure.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/11/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: f56946702011968a0fcb31f6fbecbaacdc89ea42
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "60326009"
---
# <a name="encrypt-table-data"></a>Шифрование данных таблиц
Клиентская библиотека хранилища Azure для .NET поддерживает шифрование строковых свойств для операций вставки и замены. Зашифрованные строки хранятся в службе в виде двоичных свойств. Они преобразуются обратно в строки после расшифровки.    

Что касается таблиц, то в дополнение к политике шифрования пользователи должны указать свойства, которые необходимо зашифровать. Это можно сделать путем указания атрибута [PropertyAttribute] \(для сущностей POCO, которые являются производными от TableEntity) или с помощью сопоставителя шифрования в параметрах запроса. Сопоставитель шифрования — это делегат, который получает ключ секции, ключ строки и имя свойства, а затем возвращает логическое значение, которое указывает, следует ли это свойство шифровать. Во время шифрования клиентская библиотека использует эти сведения, чтобы решить, следует ли шифровать свойство перед отправкой. Делегат также обеспечивает возможность логики в отношении того, как шифруются свойства. (Например, если X, то зашифруйте свойство A; в противном случае зашифруйте свойства A и B.) Нет необходимости предоставлять эти сведения при чтении или запросе сущностей.

## <a name="merge-support"></a>Поддержка слияния

Слияние в настоящее время не поддерживается. Так как подмножество свойств могло уже быть зашифровано с помощью другого ключа, простое слияние новых свойств и обновление метаданных приведет к потере данных. Для слияния требуется либо сначала прочитать данные существующей сущности в службе, либо использовать новый ключ для каждого свойства, однако оба способа не подходят из-за низкой эффективности.     

Дополнительные сведения о шифровании данных таблицы см. в статье [Шифрование на стороне клиента для службы хранилища Microsoft Azure](../common/storage-client-side-encryption.md).  

## <a name="next-steps"></a>Следующие шаги

- [Шаблоны проектирования таблиц](table-storage-design-patterns.md)
- [Моделирование отношений](table-storage-design-modeling.md)
- [Моделирование отношений](table-storage-design-modeling.md)
- [Разработка для изменения данных](table-storage-design-for-modification.md)
