---
title: Решение VMware для Azure по Клаудсимпле — безопасность для Клаудсимпле Services
description: Описание моделей общих обязанностей для обеспечения безопасности служб Клаудсимпле Services
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6d86c90828c081a542fa5574493a46e8a2e44640
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187483"
---
# <a name="cloudsimple-security-overview"></a>Общие сведения о безопасности Клаудсимпле

В этой статье приводятся общие сведения о реализации безопасности в решении Azure VMware с помощью службы Клаудсимпле, инфраструктуры и центра обработки данных. Вы узнаете о защите и безопасности данных, сетевой безопасности, а также об управлении уязвимостями и исправлениями.

## <a name="shared-responsibility"></a>Общая ответственность

Решение Azure VMware с помощью Клаудсимпле использует общую модель ответственности для обеспечения безопасности. Надежная безопасность в облаке достигается благодаря общим обязанностям клиентов и корпорации Майкрософт в качестве поставщика услуг. Эта матрица ответственности обеспечивает более высокий уровень безопасности и устраняет единую точку отказа.

## <a name="azure-infrastructure"></a>Инфраструктура Azure

Вопросы безопасности инфраструктуры Azure включают центры обработки данных и расположение оборудования.

### <a name="datacenter-security"></a>Безопасность центра обработки данных

Корпорация Майкрософт имеет целый отдел, посвященный проектированию, созданию и эксплуатации физических средств, поддерживающих Azure. Эта команда инвестируется в поддержание состояния физической безопасности. Дополнительные сведения о физической безопасности см. в статье [средства, локальные и физическая безопасность Azure](../security/azure-physical-security.md).

### <a name="equipment-location"></a>Расположение оборудования

Оборудование без операционной системы, в котором работают частные облака, размещается в расположениях центра обработки данных Azure.  В отсеках, где находится это оборудование, для получения доступа требуется биометрическая проверка подлинности на основе данных.

## <a name="dedicated-hardware"></a>Выделенное оборудование

В рамках службы Клаудсимпле все клиенты Клаудсимпле получают выделенные узлы без операционной системы с локальными подключенными дисками, физически изолированными от другого оборудования клиента. Гипервизор ESXi с vSAN выполняется на каждом узле. Управление узлами осуществляется с помощью выделенных клиентом VMware vCenter и НСКС. Отсутствие общего доступа к оборудованию между клиентами обеспечивает дополнительный уровень изоляции и защиты безопасности.

## <a name="data-security"></a>Безопасность данных

Клиенты сохраняют свои данные и контролируют их принадлежность. Ответственность за администраторе данных клиентов несет пользователь.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Защита данных при неактивных данных и перемещении в пределах внутренних сетей

Для неактивных данных в среде частного облака можно использовать шифрование vSAN. Шифрование vSAN работает с сертифицированными внешними серверами управления ключами VMware (KMS) в собственной виртуальной сети или локальной среде.  Вы сами управляете ключами шифрования данных. Для данных в рамках частного облака vSphere поддерживает шифрование данных по каналу для всего трафика вмкернел (включая трафик vMotion).

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Защита данных для данных, необходимых для перемещения по общедоступным сетям

Для защиты данных, перемещаемых через общедоступные сети, можно создать VPN-туннели IPsec и TLS для частных облаков. Поддерживаются общие методы шифрования, включая 128-Byte и 256-byte AES. Данные во время передачи (включая проверку подлинности, административный доступ и данные пользователей) шифруются стандартными механизмами шифрования (SSH, TLS 1,2 и Secure RDP). При обмене данными с конфиденциальной информацией используются стандартные механизмы шифрования.

### <a name="secure-disposal"></a>Безопасная реализация

Если срок действия службы Клаудсимпле истечет или она завершится, вы несете ответственность за удаление или удаление данных. Клаудсимпле взаимодействует с вами, чтобы удалить или вернуть все данные клиента в соответствии с соглашением клиента, за исключением Клаудсимплености, необходимой применимым законодательством для хранения некоторых или всех персональных данных. Если необходимо сохранить персональные данные, Клаудсимпле будет архивировать данные и реализовать разумные меры, чтобы предотвратить дальнейшую обработку данных клиента.

### <a name="data-location"></a>Расположение данных

При настройке частных облаков вы выбираете регион Azure, в котором они будут развернуты. Данные виртуальной машины VMware не перемещаются из этого физического центра обработки данных, если только не выполняется миграция или автономная архивация данных. Вы также можете размещать рабочие нагрузки и хранить данные в нескольких регионах Azure, если это необходимо.

Данные клиента, находящиеся в частых облаках с технологией Hyper-in, не проходят через расположения без явного действия администратора клиента. Вы обязаны реализовать высокую доступность рабочих нагрузок.

### <a name="data-backups"></a>Резервные копии данных

Клаудсимпле не выполняет резервное копирование или архивацию данных клиента. Клаудсимпле выполняет периодическое резервное копирование данных vCenter и НСКС, чтобы обеспечить высокий уровень доступности серверов управления. Перед резервным копированием все данные шифруются в источнике vCenter с помощью интерфейсов API VMware. Зашифрованные данные перемещаются и хранятся в большом двоичном объекте Azure. Ключи шифрования для резервных копий хранятся в надежном управляемом хранилище Клаудсимпле, работающем в виртуальной сети Клаудсимпле в Azure.

## <a name="network-security"></a>Безопасность сети

Решение Клаудсимпле опирается на уровни сетевой безопасности.

### <a name="azure-edge-security"></a>Безопасность Azure ребра

Службы Клаудсимпле созданы на основе базовой сетевой безопасности, предоставляемой Azure. Azure применяет методы глубокой защиты для обнаружения и своевременного реагирования на сетевые атаки, связанные с аномальными входными или исходящими шаблонами трафика и атаками типа "отказ в обслуживании" (от атак DDoS). Этот контроль безопасности применяется к окружениям частного облака и программного обеспечения для управления плоскостью, разработанного Клаудсимпле.

### <a name="segmentation"></a>Сегментация

Служба Клаудсимпле логически разделяет сети уровня 2, которые ограничивают доступ к частным сетям в среде частного облака. Вы можете дополнительно защитить сети частного облака с помощью брандмауэра. Портал Клаудсимпле позволяет определять правила управления сетевым трафиком для личного и NS-трафика для всего сетевого трафика, включая трафик частного облака, трафик между частным облаком, общий трафик к Интернету и сетевой трафик к локальной сети через VPN-подключение по протоколу IPsec или ExpressRoute.

## <a name="vulnerability-and-patch-management"></a>Управление уязвимостью и исправлениями

Клаудсимпле отвечает за периодическое обновление системы безопасности управляемого программного обеспечения VMware (ESXi, vCenter и НСКС).

## <a name="identity-and-access-management"></a>Управление удостоверениями и доступом

Клиенты могут проходить проверку подлинности в учетной записи Azure (в Azure AD), используя многофакторную проверку подлинности или единый вход. На портал Azure можно запустить портал Клаудсимпле без повторного ввода учетных данных.

Клаудсимпле поддерживает необязательную настройку источника удостоверений для частного облака vCenter. Вы можете использовать [локальный источник удостоверений](set-vcenter-identity.md), новый источник удостоверений для частного облака или [Azure AD](azure-ad.md).

По умолчанию клиентам предоставляются права, необходимые для повседневных операций vCenter в частном облаке. Этот уровень разрешений не включает административный доступ к vCenter. Если административный доступ является временным, вы можете [расширить свои привилегии](escalate-private-cloud-privileges.md) в течение ограниченного периода времени при выполнении административных задач.
