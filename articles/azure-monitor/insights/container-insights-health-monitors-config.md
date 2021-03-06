---
title: Azure Monitor для конфигурации мониторов работоспособности контейнеров | Документация Майкрософт
description: В этой статье содержатся сведения о конфигурации мониторов работоспособности в Azure Monitor для контейнеров.
ms.topic: conceptual
ms.date: 12/01/2019
ms.openlocfilehash: 99ea6e96f5a8a486784cb3d633a6e031b60eaad7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80055704"
---
# <a name="azure-monitor-for-containers-health-monitor-configuration-guide"></a>Azure Monitor по настройке монитора работоспособности контейнеров

Мониторы являются основным элементом для измерения работоспособности и обнаружения ошибок в Azure Monitor для контейнеров. Эта статья поможет вам понять принципы измерения работоспособности и элементы, составляющие модель работоспособности, для отслеживания работоспособности кластера Kubernetes и составления отчетов о его состоянии с помощью функции [работоспособности (Предварительная версия)](container-insights-health.md) .

>[!NOTE]
>В настоящее время функция работоспособности доступна в общедоступной предварительной версии.
>

## <a name="monitors"></a>Мониторы

Монитор измеряет работоспособность определенного аспекта управляемого объекта. У каждого монитора два и три состояния работоспособности. В каждый момент времени монитор может находиться только в одном из потенциальных состояний. Когда монитор загружается контейнерным агентом, он инициализируется в работоспособном состоянии. Состояние изменяется только в том случае, если обнаружены указанные условия для другого состояния.

Общая работоспособность объекта определяется работоспособностью каждого из его мониторов. Эта иерархия проиллюстрирована на панели Иерархия работоспособности в Azure Monitor для контейнеров. Политика, определяющая степень свертки работоспособности, является частью конфигурации составных мониторов.

## <a name="types-of-monitors"></a>Типы мониторов

|Монитор | Описание | 
|--------|-------------|
| Монитор модуля |Базовый монитор измеряет определенный аспект ресурса или приложения. Это может быть проверка счетчика производительности для определения производительности ресурса или его доступности. |
|Составной монитор | Составные мониторы группируют несколько мониторов, чтобы обеспечить единое сводное состояние работоспособности. Базовые мониторы обычно настраиваются в определенном составном мониторе. Например, сводный монитор узла отображает сводный показатель загрузки ЦП узла, использования памяти и состояния узла.
 |

### <a name="aggregate-monitor-health-rollup-policy"></a>Сводная политика сводного показателя работоспособности монитора

Каждый составной монитор определяет политику сводного показателя работоспособности, которая служит логикой, используемой для определения работоспособности составного монитора в зависимости от работоспособности мониторов под ним. Возможны следующие политики сводного показателя работоспособности для составного монитора.

#### <a name="worst-state-policy"></a>Политика наихудшего состояния

Состояние составного монитора соответствует состоянию дочернего монитора с наихудшим состоянием работоспособности. Это наиболее распространенная политика, используемая в составных мониторах.

![Пример наихудшего состояния сводного показателя мониторинга](./media/container-insights-health-monitoring-cfg/aggregate-monitor-rollup-worstof.png)

### <a name="percentage-policy"></a>Процентная политика

Исходный объект соответствует наихудшему состоянию одного члена указанного процента целевых объектов в оптимальном состоянии. Эта политика используется, когда определенный процент целевых объектов должен быть работоспособным, чтобы целевой объект считался работоспособным. Политика в процентах сортирует мониторы в порядке убывания степени серьезности состояния, а состояние статистического монитора вычисляется как наихудшее состояние N% (N определяется параметром конфигурации *статесрешолдперцентаже*).

Например, предположим, что имеется пять экземпляров контейнеров для образа контейнера, а их индивидуальные состояния являются **критическими**, **предупреждением**, **работоспособным**, **работоспособным**, **работоспособным**.  Состояние монитора использования ЦП контейнера будет **критическим**, так как наихудшее состояние 90% контейнеров является **критически важным** при сортировке в порядке убывания степени серьезности.

## <a name="understand-the-monitoring-configuration"></a>Общие сведения о конфигурации мониторинга

Azure Monitor для контейнеров содержит ряд ключевых сценариев мониторинга, которые настраиваются следующим образом.

### <a name="unit-monitors"></a>Базовые мониторы

|**Имя монитора** | Тип монитора | **Описание** | **Параметр** | **Значение** |
|-----------------|--------------|-----------------|---------------|-----------|
|Использование памяти узла |Монитор модуля |Этот монитор оценивает использование памяти узла каждую минуту, используя данные отчета cadvisor. |консекутивесамплесфорстатетранситион<br> фаилифгреатерсанперцентаже<br> варнифгреатерсанперцентаже | 3<br> 90<br> 80  ||
|Загрузка ЦП узла |Базовый монитор |Этот монитор проверяет загрузку ЦП узла каждую минуту, используя отчетные данные cadvisor. | консекутивесамплесфорстатетранситион<br> фаилифгреатерсанперцентаже<br> варнифгреатерсанперцентаже | 3<br> 90<br> 80  ||
|Состояние узла |Монитор модуля |Этот монитор проверяет состояние узла, сообщаемого Kubernetes.<br> В настоящее время проверяются следующие условия узла: недостаток места на диске, нехватка памяти, нехватка PID, недостаток диска, сеть недоступна, состояние готовности для узла.<br> В случае превышения указанных выше условий, если *недоступен* *диск* или сеть имеет **значение true**, монитор переходит в **критическое** состояние.<br> Если любое другое условие равно **true**, а не состояние **готовности** , монитор переходит в состояние **предупреждения** . | нодекондитионтипефорфаиледстате | аутофдиск, нетворкунаваилабле ||
|Использование памяти контейнера |Монитор модуля |Этот монитор сообщает об общем состоянии работоспособности использования памяти (RSS) экземпляров контейнера.<br> Он выполняет простое сравнение, которое сравнивает каждый пример с одним пороговым значением и определяется параметром конфигурации **консекутивесамплесфорстатетранситион**.<br> Его состояние вычисляется как наихудшее состояние 90% экземпляров контейнера (Статесрешолдперцентаже), отсортированных в порядке убывания степени серьезности состояния работоспособности контейнера (то есть критическое, предупреждение, работоспособное).<br> Если ни одна запись не получена из экземпляра контейнера, состояние работоспособности экземпляра контейнера выводится как **неизвестное**и имеет более высокий приоритет в порядке сортировки по **критическому** состоянию.<br> Состояние каждого отдельного экземпляра контейнера вычисляется с использованием пороговых значений, указанных в конфигурации. Если использование превышает критическое пороговое значение (90%), то экземпляр находится в **критическом** состоянии, если он меньше критического порога (90%) но больше порога предупреждения (80%), экземпляр находится в состоянии **предупреждения** . В противном случае он находится в **работоспособном** состоянии. |консекутивесамплесфорстатетранситион<br> фаилифлесссанперцентаже<br> статесрешолдперцентаже<br> варнифгреатерсанперцентаже| 3<br> 90<br> 90<br> 80 ||
|Использование ЦП контейнера |Монитор модуля |Этот монитор сообщает об общем состоянии работоспособности использования ЦП экземплярами контейнера.<br> Он выполняет простое сравнение, которое сравнивает каждый пример с одним пороговым значением и определяется параметром конфигурации **консекутивесамплесфорстатетранситион**.<br> Его состояние вычисляется как наихудшее состояние 90% экземпляров контейнера (Статесрешолдперцентаже), отсортированных в порядке убывания степени серьезности состояния работоспособности контейнера (то есть критическое, предупреждение, работоспособное).<br> Если ни одна запись не получена из экземпляра контейнера, состояние работоспособности экземпляра контейнера выводится как **неизвестное**и имеет более высокий приоритет в порядке сортировки по **критическому** состоянию.<br> Состояние каждого отдельного экземпляра контейнера вычисляется с использованием пороговых значений, указанных в конфигурации. Если использование превышает критическое пороговое значение (90%), то экземпляр находится в **критическом** состоянии, если он меньше критического порога (90%) но больше порога предупреждения (80%), экземпляр находится в состоянии **предупреждения** . В противном случае он находится в **работоспособном** состоянии. |консекутивесамплесфорстатетранситион<br> фаилифлесссанперцентаже<br> статесрешолдперцентаже<br> варнифгреатерсанперцентаже| 3<br> 90<br> 90<br> 80 ||
|Готовые модули для рабочих нагрузок системы |Монитор модуля |Этот монитор сообщает состояние на основе процента модулей Pod в состоянии готовности в заданной рабочей нагрузке. Состояние имеет значение **критическое** , если менее 100% модулей Pod находятся в **работоспособном** состоянии. |консекутивесамплесфорстатетранситион<br> фаилифлесссанперцентаже |2<br> 100 ||
|Состояние API KUBE |Монитор модуля |Этот монитор сообщает о состоянии службы API KUBE. Монитор находится в критическом состоянии, так как конечная точка API KUBE недоступна. Для этого конкретного монитора состояние определяется путем выполнения запроса к конечной точке Nodes для сервера KUBE-API. Что угодно, кроме кода ответа «ОК», монитор переходит в **критическое** состояние. | Нет свойств конфигурации |||

### <a name="aggregate-monitors"></a>Составные мониторы

|**Имя монитора** | **Описание** | **Алгоритм** |
|-----------------|-----------------|---------------|
|Узел |Этот монитор представляет собой совокупность всех мониторов узла. Он соответствует состоянию дочернего монитора с наихудшим состоянием работоспособности:<br> Загрузка ЦП узла<br> Использование памяти узла<br> Состояние узла | Наихудший из|
|Пул узлов |Этот монитор сообщает об Объединенном состоянии работоспособности всех узлов в пуле узлов *ажентпул*. Это монитор с тремя состояниями, состояние которого основано на наихудшем состоянии 80% узлов в пуле узлов, отсортированном в порядке убывания уровней серьезности состояний узлов (критическое, предупреждение, работоспособное).|Процент |
|Узлы (родительский элемент пула узлов) |Это составной монитор всех пулов узлов. Его состояние зависит от наихудшего состояния дочерних мониторов (то есть пулов узлов, имеющихся в кластере). |Наихудший из |
|Кластер (родительский узел узлов/<br> Инфраструктура Kubernetes) |Это родительский монитор, соответствующий состоянию дочернего монитора с наихудшим состоянием работоспособности, который является kubernetes инфраструктурой и узлами. |Наихудший из |
|Инфраструктура Kubernetes |Этот монитор сообщает об Объединенном состоянии работоспособности компонентов управляемой инфраструктуры кластера. его состояние вычисляется как "наихудшее" состояние дочернего монитора, т. е. рабочие нагрузки KUBE-System и состояние сервера API. |Наихудший из|
|Рабочая нагрузка системы |Этот монитор сообщает о состоянии работоспособности рабочей нагрузки KUBE-System. Этот монитор соответствует состоянию дочернего монитора с наихудшим состоянием работоспособности, который является модулями Pod **в состоянии готовности** (монитор и контейнеры в рабочей нагрузке). |Наихудший из |
|Контейнер |Этот монитор сообщает об общем состоянии работоспособности контейнера в заданной рабочей нагрузке. Этот монитор соответствует состоянию дочернего монитора с наихудшим состоянием работоспособности. Это мониторы **использования ЦП** и **использования памяти** . |Наихудший из |

## <a name="next-steps"></a>Дальнейшие шаги

Просмотрите [Монитор работоспособности кластера](container-insights-health.md) , чтобы узнать о просмотре состояния работоспособности кластера Kubernetes.
