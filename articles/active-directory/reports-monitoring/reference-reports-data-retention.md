---
title: Как долго Azure AD хранит данные отчетов? | Документы Майкрософт
description: Узнайте, как долго Azure хранит различные типы данных отчетов.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/24/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54636600c208f8f5df9fa2e25460c63dd9f46e85
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80239551"
---
# <a name="how-long-does-azure-ad-store-reporting-data"></a>Как долго Azure AD хранит данные отчетов?


Из этой статьи вы узнаете о политиках хранения данных, которые используются для разных отчетов о действиях в Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Когда Azure AD начинает сбор данных?

| Выпуск Azure AD | Начало сбора |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Когда регистрироваться для оформления подписки |
| Azure AD уровня "Бесплатный"| При первом открытии [колонки Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) или использовании [API-интерфейсов отчетности](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Когда данные о действиях становятся доступными на портале Azure?

- **Немедленно** — если вы уже работали с отчетами в портал Azure.
- **В течение 2 часов** — если вы не включили функцию создания отчетов на портале Azure.

---

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Как скоро после получения лицензии уровня "Премиум" я увижу данные о действиях?

Если у вас уже есть данные о действиях с бесплатной лицензией, то вы увидите их сразу же после обновления. Если у вас нет данных, то потребуется один или два дня, чтобы эти данные отобразились в отчетах после обновления лицензии до уровня "Премиум".

---

### <a name="can-i-see-last-months-data-after-getting-an-azure-ad-premium-license"></a>Можно ли просмотреть данные за прошлый месяц после получения лицензии Azure AD Premium?

Если вы недавно перешли на версию Premium (включая пробную версию), то первоначально сможете увидеть данные за 7 дней. По мере накопления данных этот период увеличится до 30 дней.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Когда Azure AD начинает сбор данных о сигналах системы безопасности?  

Процесс сбора сигналов системы безопасности начинается, когда вы соглашаетесь использовать **центр защиты идентификации**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Как долго в Azure AD хранятся данные?

**Отчеты об активности**    

| Report                 | Azure AD уровня "Бесплатный" | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Журналы аудита             | 7 дней        | 30 дней             | 30 дней             |
| Вход в систему               | 7 дней        | 30 дней             | 30 дней             |
| Использование Azure MFA        | 30 дней       | 30 дней             | 30 дней             |

Данные о действиях аудита и входа в систему можно хранить дольше указанных выше сроков хранения по умолчанию, направив их в учетную запись хранения с помощью Azure Monitor. Дополнительные сведения см. в статье [Архивация журналов Azure AD в учетной записи хранения Azure](quickstart-azure-monitor-route-logs-to-storage-account.md).

**Сигналы системы безопасности**

| Report         | Azure AD уровня "Бесплатный" | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Пользователи, подверженные риску  | 7 дней        | 30 дней             | 90 дней             |
| Вход, представляющий риск | 7 дней        | 30 дней             | 90 дней             |

---
