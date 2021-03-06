---
title: Выбор варианта миграции VMware с миграцией Azure миграция сервера | Документация Майкрософт
description: Содержит общие сведения о вариантах переноса виртуальных машин VMware в Azure с помощью миграции сервера миграция Azure.
ms.topic: conceptual
ms.date: 07/09/2019
ms.openlocfilehash: 52e7103ea3ebcd83369a866cc3f75b0bf0e889a2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76028714"
---
# <a name="select-a-vmware-migration-option"></a>Выберите вариант миграции VMware

Вы можете перенести виртуальные машины VMware в Azure с помощью средства "Миграция сервера" службы "Миграция Azure". Это средство предлагает несколько вариантов миграции виртуальных машин VMware:

- Миграция с использованием безагентной репликации. Миграция виртуальных машин без необходимости установки каких-либо компонентов.
- Миграция с агентом для репликации. Установите агент на виртуальную машины для репликации.




## <a name="compare-migration-methods"></a>Сравнение методов миграции

Используйте эти выбранные сравнения, чтобы определить, какой метод использовать. Кроме того, вы можете ознакомиться с полными требованиями к миграции [на основе](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) [агентов и](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) без них.

**Параметр** | **Безагентное** | **На основе агентов**
--- | --- | ---
**Разрешения Azure** | Вам понадобятся разрешения для создания проекта службы "миграция Azure" и регистрации приложений Azure AD, созданных при развертывании устройства "миграция Azure". | Вам нужны разрешения участника на подписку Azure. 
**Одновременная репликация** | Можно одновременно реплицировать не более 100 виртуальных машин из vCenter Server.<br/> Если у вас более 50 виртуальных машин для миграции, создайте несколько пакетов виртуальных машин.<br/> Репликация в одно время может повлиять на производительность. | Н/Д
**Развертывание устройства** | [Устройство для миграции Azure](migrate-appliance.md) развертывается локально. | Устройство репликации для службы " [Миграция Azure](migrate-replication-appliance.md) " развертывается локально.
**Совместимость Site Recovery** | Совместимы. | Вы не можете выполнить репликацию с миграцией Azure Migration Server, если вы настроили репликацию для компьютера с помощью Site Recovery.
**Целевой диск** | Управляемые диски | Управляемые диски
**Ограничения дискового пространства** | Диск ОС: 2 ТБ<br/><br/> Диск данных: 4 ТБ<br/><br/> Максимальное число дисков: 60 | Диск ОС: 2 ТБ<br/><br/> Диск данных: 8 ТБ<br/><br/> Максимальное число дисков: 63
**Транзитные диски** | Не поддерживается | Поддерживается
**Загрузка UEFI** | Не поддерживается | Перенесенная виртуальная машина в Azure будет автоматически преобразована в загрузочную виртуальную машину BIOS.<br/><br/> На диске ОС должно быть до четырех разделов, а тома должны быть отформатированы с помощью NTFS.


## <a name="deployment-steps-comparison"></a>Сравнение шагов развертывания

После рассмотрения ограничений основные этапы развертывания каждого решения могут помочь выбрать вариант.

**Задача** | **Сведения** |**Безагентное** | **На основе агентов**
--- | --- | --- | ---
**Оценка** | Оценка серверов перед миграцией.  Оценка является необязательной. Мы рекомендуем оценить компьютеры перед их миграцией, но вам не нужно. <br/><br/> Для оценки служба "миграция Azure" настраивает упрощенное устройство для обнаружения и оценки виртуальных машин. | Если вы запускаете миграцию без агента после оценки, то для миграции без агента используется то же устройство миграции Azure, которое настроено для оценки.  |  При выполнении миграции на основе агента после оценки устройство, настроенное для оценки, не используется во время миграции без агента. Можно оставить устройство на месте или удалить его, если вы не хотите выполнять дальнейшее обнаружение и оценку.
**Подготовка серверов и виртуальных машин VMware к миграции** | Настройте ряд параметров на серверах и виртуальных машинах VMware. | Обязательный | Обязательный
**Добавление средства "Миграция сервера"** | Добавьте средство миграции сервера Azure Migration Server в проект службы "миграция Azure". | Обязательный | Обязательный
**Развертывание устройства "миграция Azure"** | Настройка упрощенного устройства на виртуальной машине VMware для обнаружения и оценки виртуальных машин. | Обязательный | Не требуется.
**Установка службы Mobility Service на виртуальных машинах** | Установите службу Mobility Service на каждой виртуальной машине, которую необходимо реплицировать. | Не требуется | Обязательный
**Развертывание устройства репликации миграция миграции сервера Azure** | Настройка устройства на виртуальной машине VMware для обнаружения виртуальных машин и моста между службой Mobility Service, работающей на виртуальных машинах, и миграцией миграции сервера в Azure | Не требуется | Обязательный
**Репликация виртуальных машин**. Включение репликации виртуальных машин. | Настройка параметров репликации и выбор виртуальных машин для репликации | Обязательный | Обязательный
**Выполнение тестовой миграции** | Выполнение тестовой миграции, позволяющей убедиться в правильной работе всех компонентов. | Обязательный | Обязательный
**Выполнение полной миграции** | Перенесите виртуальные машины. | Обязательный | Обязательный




## <a name="next-steps"></a>Следующие шаги

[Перенос виртуальных машин VMware](tutorial-migrate-vmware.md) с миграцией без агента.



