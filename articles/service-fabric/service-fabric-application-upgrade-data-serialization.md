---
title: 'Обновление приложения: сериализация данных'
description: Лучшие методики для сериализации данных и ее влияние на последовательные обновления приложений.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.openlocfilehash: 7dc60c28b56982f82c1ac90db55ac752977ea2d6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75457493"
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Влияние сериализации данных на обновление приложений
При [последовательном обновлении приложения](service-fabric-application-upgrade.md)обновление применяется к подмножеству узлов, переходя от одного домена обновления к другому. Во время этого процесса некоторые домены обновления используют более раннюю версию приложения, чем другие. В ходе развертывания новая версия вашего приложения должна иметь способность считывать старые версии данных, а старая версия приложения — новую версию данных. Если формат данных не обладает прямой и обратной совместимостью, обновление может завершиться неудачей либо данные могут быть утрачены. В этой статье обсуждаются составляющие формата данных, а также передовой опыт в обеспечении прямой и обратной совместимости данных.

## <a name="what-makes-up-your-data-format"></a>Что составляет ваш формат данных?
В Azure Service Fabric сохраняемые и реплицируемые данные происходят от классов C#. Для приложений, использующих [надежные коллекции](service-fabric-reliable-services-reliable-collections.md), данные представляют собой объекты в надежных коллекциях и очередях. Для приложений, использующих модель [Reliable Actors](service-fabric-reliable-actors-introduction.md), это состояние резервирования для субъекта. Чтобы сохраняться и реплицироваться, эти классы C# должны поддерживать сериализацию. Поэтому формат данных определяется сериализованными полями и свойствами, а также способом их сериализации. Например, в `IReliableDictionary<int, MyClass>` данные представляют собой сериализованный `int` и сериализованный `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Изменения кода, вызывающие изменение формата данных
Поскольку формат данных определяется классами C#, изменения в этих классах могут привести к изменению формата данных. Необходимо обратить внимание на то, чтобы при последовательном обновлении такая смена формата учитывалась. Примеры, которые могут вызвать изменения формата данных:

* Добавление или удаление полей или свойств
* Переименование полей или свойств
* Изменение типов полей или свойств
* Изменение имени класса или пространства имен

### <a name="data-contract-as-the-default-serializer"></a>Контракт данных — это сериализатор по умолчанию
Сериализатор обычно отвечает за чтение данных и его десериализацию в текущей версии, даже если данные принадлежат более старой или *более новой* версии. Сериализатором по умолчанию является [сериализатор контрактов данных](https://msdn.microsoft.com/library/ms733127.aspx), который имеет четко заданные правила управления версиями. Надежные коллекции позволяют переопределить настройки сериализатора, однако Reliable Actors в настоящее время этого не допускает. Сериализатор данных играет важную роль в обеспечении последовательных обновлений. Сериализатор контрактов данных является рекомендуемым сериализатором для приложений Service Fabric.

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Влияние формата данных на последовательное обновление
Во время последовательного обновления существует два основных сценария, где сериализатор может столкнуться с более старой или *более новой* версией данных.

1. После обновления и перезапуска узла новый сериализатор загрузит данные, которые сохранялись на диске старой версией.
2. Во время последовательного обновления кластер будет содержать смесь новых и старых версий кода. Поскольку реплики могут быть помещены в разные домены обновления и могут обмениваться данными друг с другом, сериализатор новой и/или старой версии может столкнуться с новыми и/или старыми версиями данных.

> [!NOTE]
> "Новая версия" и "старая версия" здесь обозначают версию запущенного кода. "Новый сериализатор" означает код сериализатора, выполняющийся в новой версии приложения. "Новые данные" означает сериализованный класс C# из новой версии приложения.
> 
> 

Две версии кода и формата данных должны обладать прямой и обратной совместимостью. Если они несовместимы, последовательное обновление может дать сбой, при этом данные могут быть утрачены. Последовательное обновление может завершиться сбоем из-за того, что код или сериализатор может создавать исключения или ошибки при взаимодействии с другой версией. Данные могут быть утрачены, если, например, добавляется новое свойство, но старый сериализатор отклоняет его в момент сериализации.

Контракт данных является рекомендуемым решением для обеспечения совместимости ваших данных. Он обладает строго заданными правилами управления версиями при добавлении, удалении или изменении полей. Кроме того, он обладает поддержкой обработки неизвестных полей, участвующей в процессе сериализации и десериализации, а также наследования классов. Дополнительные сведения см. в разделе [Использование контракта данных](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Следующие шаги
[Руководство по обновлению приложений Service Fabric с помощью Visual Studio](service-fabric-application-upgrade-tutorial.md) поможет вам выполнить поэтапное обновление приложения с помощью Visual Studio.

[Обновление приложения с помощью PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) поможет вам выполнить обновление приложения с помощью PowerShell.

Управление обновлениями приложения осуществляется с помощью [параметров обновления](service-fabric-application-upgrade-parameters.md).

Узнайте, как использовать расширенные функциональные возможности при обновлении приложения, обратившись к [дополнительным разделам](service-fabric-application-upgrade-advanced.md).

Решения распространенных проблем при обновлении приложений см. в статье [Устранение неполадок при обновлении приложений](service-fabric-application-upgrade-troubleshooting.md).

