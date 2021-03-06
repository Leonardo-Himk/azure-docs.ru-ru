---
title: Режимы отрисовки
description: Описывает различные режимы отрисовки на стороне сервера
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 7f2b1031659864ae338bb0aa320c048ea23c21f3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681705"
---
# <a name="rendering-modes"></a>Режимы отрисовки

Удаленная визуализация предлагает два основных режима работы: режим **тилебаседкомпоситион** и режим **депсбаседкомпоситион** . Эти режимы определяют распределение рабочей нагрузки между несколькими GPU на сервере. Режим должен быть указан во время соединения и не может быть изменен во время выполнения.

Оба режима имеют преимущества, но также имеют ограничения, связанные с функциями, поэтому выбор наиболее подходящего режима зависит от варианта использования.

## <a name="modes"></a>Режимы

Два режима теперь обсуждаются более подробно.

### <a name="tilebasedcomposition-mode"></a>Режим "Тилебаседкомпоситион"

В режиме **тилебаседкомпоситион** каждый задействованный GPU визуализирует на экране определенные подпрямоугольники (плитки). Основной GPU формирует окончательный образ из плиток перед отправкой клиенту в качестве видеокадра. Соответственно, все GPU должны иметь одинаковый набор ресурсов для подготовки к просмотру, поэтому загруженные ресурсы должны поместиться в память одного GPU.

Качество рендеринга в этом режиме немного выше, чем в режиме **депсбаседкомпоситион** , так как MSAA может работать с полным набором геометрических объектов для каждого GPU. На следующем снимке экрана показано, что сглаживание правильно работает для обоих кромок:

![MSAA в Тилебаседкомпоситион](./media/service-render-mode-quality.png)

Более того, в этом режиме каждая часть может быть переключена на прозрачный материал или переключена на режим **просмотра** через [хиерарчикалстатеоверридекомпонент](../overview/features/override-hierarchical-state.md) .

### <a name="depthbasedcomposition-mode"></a>Режим "Депсбаседкомпоситион"

В режиме **депсбаседкомпоситион** каждый задействованный GPU визуализируется при полноэкранном разрешении, но только в подмножестве сеток. Окончательная композиция образа на основном GPU следит за тем, чтобы части были правильно объединены в соответствии со сведениями о глубине. Естественно, полезные данные распределяются по графическим процессорам, что позволяет использовать модели отрисовки, которые не помещаются в память одного GPU.

Каждый один GPU использует MSAA для сглаживания локального содержимого. Однако может существовать встроенное сглаживание между краями от разных GPU. Этот результат уменьшается путем обработки окончательного образа, но качество MSAA по-прежнему хуже, чем в режиме **тилебаседкомпоситион** .

Артефакты MSAA показаны на следующем рисунке: ![MSAA в депсбаседкомпоситион](./media/service-render-mode-balanced.png)

Сглаживание правильно работает между скулптуре и прикрытием, так как обе части отображаются на одном GPU. С другой стороны, ребро между прикрытием и Wall показывает некоторые псевдонимы, так как эти две части состоят из разных графических процессоров.

Крупнейшим ограничением этого режима является, что части геометрии нельзя динамически переключаться на прозрачные материалы, а режим **просмотра** [хиерарчикалстатеоверридекомпонент](../overview/features/override-hierarchical-state.md)не работает. Однако другие функции переопределения состояния (контур, цветовой оттенок,...) работают. Кроме того, в этом режиме все материалы, помеченные как прозрачные во время преобразования, работают правильно.

### <a name="performance"></a>Тестирование

Характеристики производительности в обоих режимах различаются в зависимости от варианта использования, а также по нетрудной причине или предоставлению общих рекомендаций. Если вы не ограничены упомянутыми выше ограничениями (память или прозрачность/см.), рекомендуется испытать оба режима и отслеживать производительность с помощью различных позиций камеры.

## <a name="setting-the-render-mode"></a>Установка режима рендеринга

Режим рендеринга, используемый на виртуальной машине для удаленной подготовки к `AzureSession.ConnectToRuntime` просмотру `ConnectToRuntimeParams`, задается при помощи.

```cs
async void ExampleConnect(AzureSession session)
{
    ConnectToRuntimeParams parameters = new ConnectToRuntimeParams();

    // Connect with one rendering mode
    parameters.mode = ServiceRenderMode.TileBasedComposition;
    await session.ConnectToRuntime(parameters).AsTask();

    session.DisconnectFromRuntime();

    // Wait until session.IsConnected == false

    // Reconnect with a different rendering mode
    parameters.mode = ServiceRenderMode.DepthBasedComposition;
    await session.ConnectToRuntime(parameters).AsTask();
}
```

## <a name="next-steps"></a>Дальнейшие шаги

* [Сеансы](../concepts/sessions.md)
* [Компонент переопределения иерархического состояния](../overview/features/override-hierarchical-state.md)
