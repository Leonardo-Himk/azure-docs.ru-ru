---
title: Спецификации оборудования для Azure Kinect DK
description: Узнайте о компонентах, спецификациях и возможностях Azure Kinect DK.
author: tesych
ms.author: tesych
ms.reviewer: jarrettr
ms.prod: kinect-dk
ms.date: 02/14/2020
ms.topic: article
keywords: azure, kinect, спецификации, оборудование, DK, возможности, глубина, цвет, RGB, IMU, микрофон, массив, глубина
ms.custom:
- CI 114092
- CSSTroubleshooting
audience: ITPro
manager: dcscontentpm
ms.localizationpriority: high
ms.openlocfilehash: e0d42a3ce1dd9deb5e73500371c367134ca852e1
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77619964"
---
# <a name="azure-kinect-dk-hardware-specifications"></a>Спецификации оборудования для Azure Kinect DK

В этой статье содержатся сведения о том, как оборудование Azure Kinect интегрирует новейшую технологию датчиков Майкрософт в единое подключенное к USB периферийное оборудование.

![Azure Kinect DK](./media/resources/hardware-specs-media/device-wire.png)

## <a name="terms"></a>Термины

Эти сокращенные термины используются во всей этой статье.

- NFOV — режим узкого поля обзора;
- WFOV — режим широкого поля обзора;
- FOV — поле обзора;
- FPS — кадры в секунду;
- IMU — инерциальный измерительный блок;
- FoI — область возможного использования.

## <a name="product-dimensions-and-weight"></a>Габариты и вес устройства

Устройство Azure Kinect имеет следующие габариты и вес.

- **Измерения.** 103 x 39 x 126 мм
- **Вес**: 440 г

![Габариты Azure Kinect DK](./media/resources/hardware-specs-media/dimensions.png)

## <a name="operating-environment"></a>Операционная среда

Продукт Azure Kinect DK предназначен для разработчиков и коммерческих компаний, работающих в следующих условиях окружающей среды.

- **Temperature**: 10–25 ⁰C.
- **Влажность**: 8–90 % относительной влажности (без конденсации).

> [!NOTE]
> Использование вне указанных условий может привести к сбою или неправильной работе устройства. Эти условия применимы к окружающей среде вокруг самого устройства при любых условиях эксплуатации. При использовании с внешним корпусом рекомендуется активный контроль температуры или другие решения для охлаждения, чтобы обеспечить работу устройства в этих условиях. Конструкция устройства включает в себя канал охлаждения между передним и задним кожухом. При внедрении устройства убедитесь, что этот канал охлаждения не заблокирован.

См. дополнительные сведения о [безопасной эксплуатации](https://support.microsoft.com/help/4023454/safety-information) устройства.

## <a name="depth-camera-supported-operating-modes"></a>Поддерживаемые режимы работы камеры глубины

В Azure Kinect DK интегрирована разработанная Майкрософт 1-мегапиксельная камера глубины времени полета (ToF) с использованием датчика изображения, [представленного на конференции ISSCC 2018](https://docs.microsoft.com/windows/mixed-reality/ISSCC-2018). Камера глубины поддерживает перечисленные ниже режимы.

 | Режим            | Решение | FoI       | FPS                | Рабочий диапазон* | Время выдержки |
|-----------------|------------|-----------|--------------------|------------------|---------------|
| NFOV без привязки   | 640 x 576    | 75° x 65°   | 0, 5, 15, 30       | 0,5–3,86 м       | 12,8 мс        |
| NFOV 2 x 2 с привязкой (SW) | 320 x 288    | 75° x 65°   | 0, 5, 15, 30       | 0,5–5,46 м       | 12,8 мс        |
| WFOV 2 x 2 с привязкой | 512 x 512    | 120° x 120° | 0, 5, 15, 30       | 0,25–2,88 м      | 12,8 мс        |
| WFOV без привязки   | 1024 x 1024  | 120° x 120° | 0, 5, 15           | 0,25–2,21 м      | 20,3 мс        |
| Пассивный датчик инфракрасного излучения      | 1024 x 1024  | Недоступно       | 0, 5, 15, 30       | Недоступно              | 1,6 мс         |

\*Отражательная способность от 15 % до 95 % при 850 Нм, 2,2 мкВт/см<sup>2</sup>/нм, случайная ошибка (стандартное отклонение). ≤ 17 мм, типичная систематическая ошибка < 11 мм + 0,1 % расстояния без многолучевой интерференции. Глубина может быть указана за пределами указанного выше диапазона. Это зависит от отражательной способности объекта.

## <a name="color-camera-supported-operating-modes"></a>Поддерживаемые режимы работы камеры для цветной съёмки

Устройство Azure Kinect DK оснащено КМОП-матрицей OV12A10 12 МП со сдвигаемым затвором. Ниже перечислены собственные режимы работы.

|             Разрешение камеры RGB (Г х В)  |          Пропорции  |          Варианты форматов   |          Частота кадров (FPS)  |          Номинальное FOV (Г х В) (после обработки)  |
|------------------------------------------|------------------------|---------------------------|-----------------------------|---------------------------------------------|
|       3840 x 2160                          |          16:9          |          MJPEG            |          0, 5, 15, 30       |          90 °x 59°                              |
|       2560 x 1440                          |          16:9          |          MJPEG            |          0, 5, 15, 30       |          90 °x 59°                              |
|       1920 x 1080                          |          16:9          |          MJPEG            |          0, 5, 15, 30       |          90 °x 59°                              |
|       1280 x 720                           |          16:9          |          MJPEG/YUY2/NV12  |          0, 5, 15, 30       |          90 °x 59°                              |
|       4096 x 3072                          |          4:3           |          MJPEG             |          0, 5, 15           |          90° x 74,3°                            |
|       2048 x 1536                          |          4:3           |          MJPEG             |          0, 5, 15, 30       |          90° x 74,3°                            |

Камера RGB совместима с видеоустройствами класса USB, и ее можно использовать без пакета SDK для датчиков. Цветовое пространство камеры RGB: Полный диапазон BT. 601 [0–255]. 

> [!NOTE]
> Пакет SDK для датчиков позволяет снимать цветные изображения в формате пикселей BGRA. Это нестандартный режим, поддерживаемый устройством, который при использовании создает дополнительную нагрузку на ЦП. ЦП узла используется для преобразования из изображений MJPEG, полученных с устройства.

## <a name="rgb-camera-exposure-time-values"></a>Значения времени экспозиции камеры RGB

Ниже приведено сопоставление приемлемых значений экспозиции в ручном режиме камеры RGB:

| exp| 2^exp | 50 Гц   |60 Гц    |
|----|-------|--------|--------|
| -11|     488|    500|    500 |
| –10|     977|   1250|   1250 |
|  –9|    1953|   2500|   2500 |
|  –8|    3906|  10000|   8330 |
|  -7|    7813|  20 000|  16670 |
|  -6|   15625|  30 000|  33 330 |
|  -5|   31 250|  40 000|  41 670 |
|  –4|   62 500|  50 000|  50 000 |
|  –3|  125 000|  60 000|  66 670 |
|  -2|  250 000|  80 000|  83 330 |
|  -1|  500000| 100 000| 100 000 |
|   0| 1000000| 120000| 116 670 |
|   1| 2 000 000| 130 000| 133 330 |

## <a name="depth-sensor-raw-timing"></a>Время синхронизации необработанных данных датчика глубины

Режим глубины | IR <br>Импульсы | Импульс <br>Ширина  | Бездействие <br>Периоды| Время простоя | Экспозиция <br> Time
-|-|-|-|-|-
NFOV без привязки <br>  NFOV 2xx с привязкой <br> WFOV 2 x 2 с привязкой | 9 | 125 мкс | 8 | 1450 мкс | 12,8 мс 
WFOV без привязки                                            | 9 | 125 мкс | 8 | 2390 мкс | 20,3 мс


## <a name="camera-field-of-view"></a>Поле обзора камеры

На следующем рисунке показана глубина и поле обзора камеры RGB, а также углы обзора для датчиков. На этой диаграмме показана камера RGB с соотношением сторон 4:3.

![FOV камеры](./media/resources/hardware-specs-media/camera-fov.png)

На этом рисунке показано поле обзора камеры, видимое спереди с расстояния 2000 мм.

![FOV в передней плоскости камеры](./media/resources/hardware-specs-media/fov-front.png)

> [!NOTE]
> Если датчик глубины пребывает в режиме NFOV, то камера RGB обеспечивает более высокий уровень перекрытия пикселей при соотношении сторон 4:3, чем при 16:9.

## <a name="motion-sensor-imu"></a>Датчик движения (IMU)

Встроенный инерциальный измерительный блок (IMU) — LSM6DSMUS оснащен акселерометром и гироскопом. Данные акселерометра и гироскопа записываются одновременно при частоте 1,6 кГц. Выборки передаются на узел с частотой 208 Гц.

## <a name="microphone-array"></a>Массив микрофонов

В Azure Kinect DK встроен высококачественный круговой массив из семи микрофонов, который определяется как стандартное USB-аудиоустройство класса 2.0. Доступны все семь каналов. Характеристики производительности:

- Чувствительность: -22 дБ FS (УЗД 94 дБ, 1 кГц)
- Отношение сигнала к шуму: > 65 дБ
- Точка акустической перегрузки: 116 дБ

![Наконечник микрофона](./media/resources/hardware-specs-media/mic-bubble.png)

## <a name="usb"></a>USB

Azure Kinect DK — это составное устройство USB 3, которое предоставляет следующие аппаратные конечные точки для операционной системы.

Идентификатор поставщика — 0x045E (Майкрософт), таблица идентификаторов продуктов приведена ниже:

|    Интерфейс USB        |    IP-адрес PNP    |     Примечания            |
|-------------------------|--------------|----------------------|
|    Концентратор USB 3.1 Gen1    |    0x097A    |    Основной концентратор    |
|    Концентратор USB 2.0         |    0x097B    |    HS   USB          |
|    Камера глубины       |    0x097C    |    USB 3.0            |
|    Камера для цветной съёмки       |    0x097D    |    USB 3.0            |
|    Микрофоны        |    0x097E    |    HS   USB          |

## <a name="indicators"></a>Индикаторы

В устройстве есть индикатор потоковой передачи камеры на передней панели, который можно отключить программно с помощью пакета SDK для датчиков.

Светодиодный индикатор состояния, который находится на задней части устройства, указывает состояние устройства:

| Цвет индикатора     | Значение                                                   |
|-----------------------|------------------------------------------------------------|
| Постоянно белый           | Устройство включено и работает правильно.                         |
| Мигающий белый        | Устройство включено, но отсутствует подключение USB 3.0.   |
| Мигающий оранжевый        | Недостаточное питание устройства для работы.               |
| Мигающий оранжево-белый  | Выполняется обновление встроенного ПО или восстановление.                    |

## <a name="power-device"></a>Устройство питания

Питание устройства может осуществляться двумя способами:

1. С помощью встроенного блока питания. Данные передаются с помощью отдельного USB-кабеля-переходника для разъемов Type-C и Type-A.
2. С помощью кабеля для разъемов Type-C, который можно использовать как для питания, так и для передачи данных.

Кабель для разъемов Type-C не входит в комплектацию Azure Kinect DK.

> [!NOTE]
> - Поставляемый кабель питания представляет собой USB-кабель для разъема Type-A и цилиндрического соединителя. Используйте прилагаемый розеточный блок питания с этим кабелем. Устройство способно потреблять больше энергии, чем можно предоставить через два стандартных USB-порта Type-A.
> - USB-кабели имеют важное значение, поэтому мы рекомендуем использовать высококачественные кабели и проверять функциональность перед удаленным развертыванием устройства.

> [!TIP]
> Чтобы выбрать подходящий кабель для разъемов Type-C, следует учитывать следующее:
> - Сертифицированный [USB](https://www.usb.org/products)-кабель должен поддерживать как питание, так и передачу данных.
> - Длина пассивного кабеля должна быть меньше 1,5 м. Если она больше, используйте активный кабель. 
> - Кабель должен поддерживать силу тока не менее 1,5 А. В противном случае необходимо подключить внешний источник питания.

Проверьте кабель:

- Подключите устройство с помощью кабеля к главному компьютеру.
- Убедитесь, что все устройства правильно перечислены в диспетчере устройств Windows. Должна отобразиться RGB-камера глубины, как показано в примере ниже.

  ![Azure Kinect DK в Диспетчере устройств](./media/resources/hardware-specs-media/device-manager.png)

- Убедитесь, что кабель обеспечивает надежную передачу данных со всех датчиков в средстве просмотра Azure Kinect со следующими параметрами.

  - Камера глубины: NFOV без привязки
  - RGB-камера: 2160p
  - Микрофоны и IMU включены

## <a name="what-does-the-light-mean"></a>Что означает цвет индикатора?

Индикатор питания — это светодиодный индикатор на задней панели Azure Kinect DK. Цвет индикатора меняется в зависимости от состояния устройства.

![На рисунке показана задняя часть Azure Kinect DK. Изображены три пронумерованных выноски: одна — для светодиодного индикатора, а под ней две — для кабелей.](./media/quickstarts/azure-kinect-dk-power-indicator.png)

На этом рисунке отмечены следующие компоненты:

1. Индикатор питания
1. Кабель питания (подключенный к источнику питания).
1. Кабель данных USB-C (подключенный к ПК).

Убедитесь, что кабели подключены, как показано ниже. Затем просмотрите следующую таблицу, чтобы узнать о различных состояниях индикатора питания.

|Цвет индикатора |Значение |Предполагаемые действия |
| ---| --- | --- |
|Постоянно белый |Устройство включено и работает правильно. |Устройство готово к использованию. |
|Не подсвечивается |Устройство не подключено к компьютеру. |Убедитесь, что кабель питания подключен к устройству и USB-адаптеру питания.<br /><br />Убедитесь, что кабель USB-C подключен к устройству и компьютеру. |
|Мигающий белый |Устройство включено, но отсутствует подключение USB 3.0. |Убедитесь, что кабель питания подключен к устройству и USB-адаптеру питания.<br /><br />Убедитесь, что кабель USB-C подключен к устройству и порту USB 3.0 на компьютере.<br /><br />Подсоедините устройство к другому порту USB 3.0 на компьютере.<br /><br />На компьютере откройте Диспетчер устройств (**Пуск** > **Панель управления** > **Диспетчер устройств**) и убедитесь, что компьютер оснащен поддерживаемым контроллером узла USB 3.0. |
|Мигающий оранжевый |Недостаточное питание устройства для работы. |Убедитесь, что кабель питания подключен к устройству и USB-адаптеру питания.<br /><br />Убедитесь, что кабель USB-C подключен к устройству и компьютеру. |
|Мигающий оранжево-белый |Устройство включено и получает обновление встроенного ПО или восстанавливает заводские настройки. |Дождитесь, когда индикатор питания станет постоянно подсвечиваться белым. Дополнительные сведения см. в статье [Reset Azure Kinect DK](reset-azure-kinect-dk.md) (Сброс Azure Kinect DK). |

## <a name="power-consumption"></a>Энергопотребление

Azure Kinect DK потребляет до 5,9 Вт. Энергопотребление зависит от условий использования.

## <a name="calibration"></a>Калибровка

Azure Kinect DK откалиброван на фабрике. Параметры калибровки визуальных и инерционных датчиков можно запрашивать программно с помощью пакета SDK для датчиков.

## <a name="device-recovery"></a>Восстановление устройства

Встроенное ПО устройства можно сбросить до исходных настроек с помощью кнопки под блокировочной защелкой.

![Кнопка восстановления Azure Kinect DK](./media/resources/hardware-specs-media/recovery.png)

Чтобы восстановить устройство, ознакомьтесь с [инструкциями здесь](reset-azure-kinect-dk.md).

## <a name="next-steps"></a>Дальнейшие действия

- [About Azure Kinect Sensor SDK](about-sensor-sdk.md) (Пакет SDK для датчиков Azure Kinect)
- [Настройка оборудования](set-up-azure-kinect-dk.md)
