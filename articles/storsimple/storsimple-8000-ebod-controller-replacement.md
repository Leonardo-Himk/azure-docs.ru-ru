---
title: Замена контроллера EBOD на устройстве StorSimple 8600 | Документация Майкрософт
description: Объясняется процесс снятия или замены одного или обоих контроллеров EBOD на устройстве StorSimple 8600.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: b05d1f36d1e74b3d915e216676859654fbcbacf3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79254889"
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Замена контроллера EBOD на устройстве StorSimple

## <a name="overview"></a>Обзор
В этом учебнике объясняется, как заменить неисправный модуль контроллера EBOD на устройстве Microsoft Azure StorSimple. Чтобы заменить модуль контроллера EBOD, необходимо:

* снять неисправный контроллер EBOD
* установить новый контроллер EBOD

Прежде чем начать, ознакомьтесь с приведенной ниже информацией.

* Во все неиспользуемые отсеки должны быть установлены пустые модули EBOD. Если хотя бы один отсек останется открытым, корпус не сможет правильно охлаждаться.
* Контроллер EBOD поддерживает горячую замену, и его можно снять или заменить. Не снимайте неисправный модуль, если у вас нет модуля на замену. После начала процесса замены его необходимо завершить в течение 10 минут.

> [!IMPORTANT]
> Перед снятием или заменой любого компонента StorSimple обязательно ознакомьтесь с [условными обозначениями сведений о безопасности](storsimple-safety.md#safety-icon-conventions) и другими [мерами предосторожности](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Снятие контроллера EBOD
Перед заменой неисправного модуля контроллера EBOD в устройстве StorSimple убедитесь, что модуль другого контроллера EBOD включен и работает. Снятие модуля контроллера EBOD описано в следующей процедуре и таблице.

#### <a name="to-remove-an-ebod-module"></a>Снятие модуля EBOD
1. Откройте портал Azure.
2. Перейдите к устройству и перейдите к разделу **Параметры** > **работоспособность оборудования**и убедитесь, что состояние индикатора активного EBOD контроллера горит зеленым, а индикатор для неисправного модуля контроллера EBOD — красный.
3. Найдите неисправный модуль контроллера EBOD на задней стороне устройства.
4. Отключите кабели, подключающие модуль контроллера EBOD к контроллеру, затем извлеките модуль EBOD из системы.
5. Запишите точный номер порта SAS модуля контроллера EBOD, который был подключен к контроллеру. После замены модуля EBOD система должна вернуться к прежней конфигурации.
   
   > [!NOTE]
   > Как правило, это порт A, обозначенный как **Входной порт узла** на приведенной ниже схеме.
   
    ![Задняя панель контроллера EBOD](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Рис. 1.** Задняя поверхность модуля EBOD
   
   | Метка | Описание |
   |:--- |:--- |
   | 1 |Индикатор сбоя |
   | 2 |Индикатор питания |
   | 3 |Соединители SAS |
   | 4 |Индикаторы SAS |
   | 5 |Последовательные порты только для служебного использования |
   | 6 |Порт A (Входной порт узла) |
   | 7 |Порт B (Выходной порт узла) |
   | 8 |Порт C (только для служебного использования) |

## <a name="install-a-new-ebod-controller"></a>установить новый контроллер EBOD
Установка модуля контроллера EBOD для устройства StorSimple описана в следующей процедуре и таблице.

#### <a name="to-install-an-ebod-controller"></a>Для установки контроллера EBOD
1. Проверьте контроллер EBOD, особенно соединительный разъем на отсутствие повреждений. Если контакты разъема изогнуты, не устанавливайте новый контроллер EBOD.
2. С защелками в открытом положении установите модуль в корпус и продвигайте его до срабатывания защелок.
   
    ![Установка контроллера EBOD](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Рис. 2**  Установка модуля контроллера EBOD
3. Закройте защелку. При срабатывании защелки вы должны услышать щелчок.
   
    ![Освобождение защелки EBOD](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Рис. 3**  Закрытие кратковременной блокировки модуля EBOD
4. Подключите кабели. Конфигурация должна остаться той же, которая была до замены модуля. Подробные сведения о подключении кабелей приведены на следующей схеме и в таблице.
   
    ![Подключите питание к устройству 4U](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Рис. 4**. Повторное подключение кабелей
   
   | Метка | Описание |
   |:--- |:--- |
   | 1 |Основной корпус |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Контроллер 0 |
   | 5 |Контроллер 1 |
   | 6 |Контроллер EBOD 0 |
   | 7 |Контроллер EBOD 1 |
   | 8 |Корпус EBOD |
   | 9 |Блоки распределения питания |

## <a name="next-steps"></a>Дальнейшие шаги
Узнайте подробнее о [замене компонентов оборудования StorSimple](storsimple-8000-hardware-component-replacement.md).

