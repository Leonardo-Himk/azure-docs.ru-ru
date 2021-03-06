---
title: Оценка непрерывного доступа в Azure AD
description: Более быстрое реагирование на изменения пользовательской среды благодаря оценке непрерывного доступа в Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jlu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3713901dd3dd5d17c4e1ddcef529c663b68f5b43
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82112581"
---
# <a name="continuous-access-evaluation"></a>Непрерывная оценка доступа

Службы Майкрософт, такие как Azure Active Directory (Azure AD) и Office 365, используют открытые стандарты и протоколы для повышения совместимости. Одним из наиболее важных является Open ID Connect (OIDC). Когда клиентское приложение, например Outlook, подключается к службе, например Exchange Online, запросы API разрабатываются с помощью маркеров доступа OAuth 2,0. По умолчанию эти маркеры доступа действительны в течение одного часа. По истечении срока клиент перенаправляется обратно в Azure AD, чтобы обновить их. Это также дает возможность переоценивать политики для доступа пользователей — мы можем не обновлять маркер из-за политики условного доступа или из-за того, что пользователь отключен в каталоге. 

Срок действия маркера и обновление являются стандартным механизмом в отрасли. С другой стороны, клиенты сталкиваются с вопросами о задержке между изменением условий риска для пользователя (например, переход от главного офиса к локальному кафе или учетные данные пользователя, обнаруженные на черном рынке), а также когда политики могут быть применены к этому изменению. Мы экспериментируем с подходом "тупыми Object" к уменьшению времени существования маркеров, но обнаружили, что они могут снизить удобство работы и надежность пользователей, не устраняя риски.

Для своевременного реагирования на нарушения политик или проблемы безопасности действительно требуется "разговор" между издателем маркера, например Azure AD, и проверяющей стороной, например Exchange Online. Эта двусторонняя беседа дает нам две важные возможности. Проверяющая сторона может заметить, что изменились, например, клиент, поступающий из нового расположения, и сообщить поставщику маркера. Он также дает поставщику маркера возможность сообщить проверяющей стороне о необходимости прекратить соблюдение маркеров для данного пользователя из-за компрометации учетной записи, невозможности или других проблем. Механизм для этого диалога — это оценка непрерывного доступа (автоматизированного конструирования).

Корпорация Майкрософт является ранним участником инициативы по работе с протоколом КАЕП, как часть рабочей группы " [Общие сигналы" и "события](https://openid.net/wg/sse/) " в OpenID Connect Foundation. Поставщики удостоверений и проверяющие стороны смогут использовать события безопасности и сигналы, определенные Рабочей группой для повторной авторизации или завершения доступа. Это интересная работа и улучшит безопасность на многих платформах и приложениях.

Поскольку преимущества безопасности настолько хороши, мы разведем исходную реализацию Майкрософт в параллельном режиме, чтобы продолжить работу в теле стандартов. По мере того как мы работаем над развертыванием этих возможностей по непрерывной оценке доступа (автоматизированного конструирования) в службах Майкрософт, мы узнали многое и поделиться этой информацией с сообществом стандартов. Мы надеемся, что наш опыт в области развертывания поможет нам сообщить еще лучшему промышленному стандарту и прилагается к реализации этого стандарта после ратифицирован, что позволит всем участвующим службам воспользоваться преимуществами.

## <a name="how-does-cae-work-in-microsoft-services"></a>Как автоматизированного конструирования работает в службах Майкрософт?

Мы сосредоточены на нашей первоначальной реализации оценки непрерывного доступа для Exchange и команд. Мы надеемся расширить поддержку других служб Майкрософт в будущем. Мы начнем включить оценку непрерывного доступа только для клиентов без политик условного доступа. Мы будем использовать наши знания на этом этапе автоматизированного конструирования, чтобы сообщить о текущем выпуске автоматизированного конструирования.

## <a name="service-side-requirements"></a>Требования на стороне службы

Оценка непрерывного доступа реализуется путем включения служб (поставщиков ресурсов) для подписки на критические события в Azure AD, чтобы эти события можно было оценивать и применять практически в реальном времени. Следующие события будут применены в этом начальном автоматизированного КОНСТРУИРОВАНИЯе развертывания:

- Учетная запись пользователя удалена или отключена
- Пароль для пользователя изменен или сброшен
- Администратор явно отменяет все маркеры обновления для пользователя
- Риск для пользователей с повышенными правами, обнаруженный защита идентификации Azure AD

В будущем мы надеемся добавить дополнительные события, включая такие события, как расположение и изменение состояния устройства. **Хотя наша цель — обеспечить возможность мгновенного применения, в некоторых случаях может наблюдаться задержка до 15 минут из-за времени распространения событий**. 

## <a name="client-side-claim-challenge"></a>Запрос на утверждение на стороне клиента

Перед оценкой непрерывного доступа клиенты всегда пытаются воспроизвести маркер доступа из своего кэша, если срок его действия не истек. В автоматизированного конструирования мы представляем новый случай, когда поставщик ресурсов может отклонить маркер, даже если срок его действия не истек. Чтобы уведомить клиентов о необходимости обхода кэша, несмотря на то, что срок действия кэшированных токенов не истек, мы представляем механизм, именуемый **запросом на утверждение**. АВТОМАТИЗИРОВАННОГО конструирования требует обновления клиента, чтобы разобраться в запросах на утверждение. Последняя версия следующих приложений ниже поддерживает запрос на утверждение.

- Outlook для Windows 
- Outlook iOS 
- Outlook для Android 
- Mac-адрес Outlook 
- Команды для Windows
- Команды iOS 
- Команды Android 
- Группы Mac 

## <a name="token-lifetime"></a>Token Lifetime

Так как риск и политика оцениваются в режиме реального времени, клиенты, которые согласовывают сеансы с непрерывным доступом для оценки, будут полагаться на автоматизированного конструирования, а не на существующие статические политики времени жизни маркера доступа. Это означает, что настраиваемая политика времени существования маркеров больше не будет учитываться для клиентов с поддержкой автоматизированного конструирования, которые согласовывают сеансы с

Время жизни маркера доступа увеличится до 24 часов в сеансах автоматизированного конструирования. Отзыв управляется критически важными событиями и оценкой политики, а не произвольным периодом времени. Это изменение повышает стабильность ваших приложений, не влияя на безопасность. 

## <a name="example-flows"></a>Примеры потоков

### <a name="user-revocation-event-flow"></a>Поток событий отзыва пользователя:

![Поток событий отзыва пользователя](./media/concept-fundamentals-continuous-access-evaluation/user-revocation-event-flow.png)

1. Клиент с поддержкой автоматизированного конструирования предоставляет учетные данные или маркер обновления для AAD, запрашивающий маркер доступа для некоторого ресурса.
1. Маркер доступа возвращается вместе с другими артефактами клиенту.
1. Администратор явным образом [отменяет все маркеры обновления для пользователя](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0). Событие отзыва будет отправлено поставщику ресурсов из Azure AD.
1. Маркер доступа предоставляется поставщику ресурсов. Поставщик ресурсов оценивает допустимость маркера и проверяет наличие какого-либо события отзыва для пользователя. Поставщик ресурсов использует эти сведения для принятия решения о предоставлении доступа к ресурсу.
1. В этом случае поставщик ресурсов запрещает доступ и отправляет клиенту запрос на утверждение 401 +.
1. Клиент, поддерживающий автоматизированного конструирования, понимает запрос 401 +. Он обходит кэши и снова переходит к шагу 1, отправляя его маркер обновления вместе с запросом заявки обратно в Azure AD. Затем Azure AD переопределит все условия и предложит пользователю выполнить повторную проверку подлинности в этом случае.
 
## <a name="faqs"></a>Частые вопросы

### <a name="what-is-the-lifetime-of-my-access-token"></a>Каково время существования маркера доступа?

Если вы не используете клиенты с поддержкой автоматизированного конструирования, срок жизни маркера доступа по умолчанию будет равен 1 часу, если только срок действия маркера доступа не настроился с помощью функции предварительной версии [срока действия маркера (CTL)](../develop/active-directory-configurable-token-lifetimes.md) .

Если вы используете клиенты с поддержкой автоматизированного конструирования, которые согласовывают сеансы, поддерживающие автоматизированного конструирования, параметры CTL для времени жизни маркера доступа будут перезаписаны, а время существования маркера доступа будет составлять 24 часа.

### <a name="how-quick-is-enforcement"></a>Как быстро обеспечить применение?

Хотя наша цель — обеспечить возможность мгновенного применения, в некоторых случаях может наблюдаться задержка до 15 минут из-за времени распространения событий.

### <a name="how-will-cae-work-with-sign-in-frequency"></a>Как будет автоматизированного конструирования работать с частотой входа?

Частота входа будет соблюдаться с автоматизированного конструирования или без них.

## <a name="next-steps"></a>Дальнейшие шаги

[Объявление о непрерывном доступе](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933)
