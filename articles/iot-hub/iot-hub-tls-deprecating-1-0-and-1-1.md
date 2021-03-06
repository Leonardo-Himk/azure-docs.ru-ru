---
title: Устаревшие протоколы TLS 1,0 и 1,1 в центре Интернета вещей | Документация Майкрософт
description: Рекомендации по нерекомендуемым протоколам TLS 1,0 и 1,1 и поддерживаемым шифрам в центре Интернета вещей.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: a887dd4df44ba58b0e6646ffb1c10eb21edf3e69
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81381301"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>Устаревшие протоколы TLS 1,0 и 1,1 в центре Интернета вещей

Чтобы обеспечить лучшее в своем классе шифрование, центр Интернета вещей переходит на протокол TLS 1,2 в качестве механизма шифрования, выбранного для устройств и служб Интернета вещей. 

## <a name="timeline"></a>Временная шкала

Центр Интернета вещей продолжит поддерживать протокол TLS 1.0/1.1, пока не появится уведомление. Однако рекомендуется как можно скорее перенести всех клиентов на TLS 1,2.

## <a name="supported-ciphers"></a>Поддерживаемые шифры

Ниже приведена временная шкала доступности различных шифров, используемых в подтверждении TLS.

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (сейчас поддерживается)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (будет поддерживаться во второй половине 2020)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (будет поддерживаться во второй половине 2020)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (будет поддерживаться во второй половине 2020)

## <a name="customer-feedback"></a>Отзывы клиентов

Несмотря на то, что применение TLS 1,2 является лучшим в своем классе выбором шифрования и будет включено в соответствии с планом, мы по-прежнему хотели бы слышать пользователей о конкретных развертываниях и проблемах, связанных с принятием TLS 1,2. Для этой цели комментарии можно отправить в [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com).
