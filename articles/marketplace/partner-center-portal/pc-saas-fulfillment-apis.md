---
title: API-интерфейсы выполнения SaaS в коммерческом магазине Майкрософт
description: Введение в интерфейсы API выполнения, которые позволяют интегрировать предложения SaaS в Microsoft AppSource и Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dsindona
ms.openlocfilehash: ba1b158bc529b148a8e3138d122c13ead19e073e
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858094"
---
# <a name="saas-fulfillment-apis-in-microsoft-commercial-marketplace"></a>API-интерфейсы выполнения SaaS в коммерческом магазине Майкрософт

API-интерфейсы выполнения SaaS позволяют независимым поставщикам программного обеспечения интегрировать свои приложения SaaS в Microsoft AppSource и Azure Marketplace. Эти API позволяют приложениям ISV участвовать во всех каналах с поддержкой торговли: Direct, «партнер-участник» (Торговый посредник) и светодиодный индикатор поля. Они необходимы для перечисления предложений SaaS на языке Microsoft AppSource и Azure Marketplace.

> [!WARNING]
> Текущей версией этого API является версия 2, которая должна использоваться для всех новых предложений SaaS.  Версия 1 API устарела и поддерживается для поддержки существующих предложений.

## <a name="business-model-support"></a>Поддержка бизнес-моделей

Этот API поддерживает следующие возможности бизнес-модели. Вы можете:

* Укажите несколько планов для предложения. Эти планы имеют различные функциональные возможности и могут различаться по-разному.
* Предоставление предложения для каждого сайта или модели выставления счетов для каждого пользователя.
* Предоставьте ежемесячные и ежегодные (платные) варианты выставления счетов.
* Предоставьте клиентам частные цены на основе согласованного бизнес-соглашения.


## <a name="next-steps"></a>Дальнейшие шаги

Если вы еще не сделали этого, зарегистрируйте приложение SaaS в [портал Azure](https://ms.portal.azure.com) , как описано в разделах [Регистрация приложения Azure AD](./pc-saas-registration.md).  Затем используйте самую последнюю версию этого интерфейса для разработки: [API выполнения SaaS версии 2](./pc-saas-fulfillment-api-v2.md).
