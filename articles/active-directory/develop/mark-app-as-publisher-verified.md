---
title: Создание для приложения метки "проверенный издатель" — Платформа удостоверений Майкрософт | Azure
description: Сведения о том, как пометить приложение как "проверенный издатель". Если приложение помечено как "проверенный издатель", это означает, что издатель подтвердил удостоверение с помощью учетной записи Microsoft Partner Network, для которой выполнен процесс проверки и привязка учетной записи MPN к регистрации приложения.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/08/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: 0c1f279e49b938fb391223bec47d23326e34b2ea
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595693"
---
# <a name="mark-your-app-as-publisher-verified-preview"></a>Добавление для приложения метки "Проверенный издатель" (предварительная версия)

Если приложение помечено как "проверенный издатель", это означает, что издатель подтвердил удостоверение с помощью учетной записи Microsoft Partner Network (MPN) и привязал эту учетную запись MPN к регистрации приложения. В этой статье описывается, как завершить [процесс проверки издателя (предварительная версия)](publisher-verification-overview.md).

## <a name="quickstart"></a>Краткое руководство
Если вы уже зарегистрировались в Microsoft Partner Network (MPN) и выполнили все [предварительные требования](publisher-verification-overview.md#requirements), можно сразу приступить к работе: 

1. Перейдите на страницу предварительной версии [портала регистрации приложений](https://aka.ms/PublisherVerificationPreview).

1. Выберите приложение и щелкните **Фирменная символика**. 

1. Щелкните **Add MPN ID to verify publisher** (Добавить идентификатор MPN для проверки издателя) и ознакомьтесь с указанными требованиями.

1. Введите идентификатор MPN и нажмите кнопку **Проверить и сохранить**.

Дополнительные сведения о конкретных преимуществах, требованиях и часто задаваемых вопросах см. в [обзоре](publisher-verification-overview.md).


## <a name="mark-your-app-as-publisher-verified"></a>Добавление для приложения метки "Проверенный издатель"
Убедитесь, что выполнены [предварительные требования](publisher-verification-overview.md#requirements), а затем выполните следующие действия, чтобы пометить приложения как "проверенный издатель".  

1. Войдите в систему с помощью учетной записи организации (Azure AD), которая имеет право вносить изменения в приложения, которые вы хотите пометить как "проверенный издатель", и в учетной записи MPN в Центре партнеров. 

    - В Azure AD этот пользователь должен быть владельцем приложения или иметь одну из следующих ролей: Администратор приложения, администратор облачного приложения, глобальный администратор. 

    - В Центре партнеров этот пользователь должен иметь следующие роли: Администратор MPN, администратор учетных записей или глобальный администратор (это общая роль, приобретенная в Azure AD). 

1. Перейдите на страницу предварительной версии портала регистрации приложений.  

1. Щелкните приложение, которое вы хотите пометить как "проверенный издатель" и откройте колонку "Фирменная символика". 

1. Убедитесь, что домен издателя приложения настроен соответствующим образом. Этот домен должен: 

    - быть добавленным в клиент Azure AD в качестве пользовательского домена, проверенного DNS;  

    - соответствовать домену адреса электронной почты, используемого в процессе проверки учетной записи MPN. 

1. Щелкните **Add MPN ID to verify publisher** (Добавить идентификатор MPN для проверки издателя) в нижней части страницы. 

1. Введите **Идентификатор MPN**. Этот идентификатор MPN должен соответствовать: 

    - Допустимой учетной записи Microsoft Partner Network, которая завершила процесс проверки.  

    - Глобальной учетной записи партнера (PGA) для вашей организации. 

1. Нажмите кнопку **Проверить и сохранить**. 

1. Подождите до завершения обработки запроса, это может занять несколько минут. 

1. Если проверка прошла успешно, окно проверки издателя закроется, и вы вернетесь к колонке "Фирменная символика". Рядом с **проверенным именем издателя** отобразится синий индикатор события проверки имени. 

1. Пользователи, которым будет предложено дать согласие на использование вашего приложения, увидят индикатор вскоре после успешного прохождения процесса, однако на его распространение по всей системе может потребоваться некоторое время. 

1. Протестируйте эту функцию, войдя в приложение и убедившись, что индикатор событий подтверждения отображается на экране согласия. Если вы вошли в систему как пользователь, которому уже предоставлено согласие на приложение, можно использовать параметр запроса *prompt=consent*, чтобы принудительно выполнить запрос согласия. 

1. Если необходимо, повторите этот процесс для дополнительных приложений, для которых нужно отобразить индикатор событий. Чтобы выполнить массовое подтверждение и ускорить процесс, можно использовать Microsoft Graph. Кроме того, вскоре для этого можно будет использовать командлеты PowerShell. Дополнительные сведения см. в пункте [Вызовы Microsoft Azure AD Graph](troubleshoot-publisher-verification.md#making-microsoft-graph-api-calls). 

Это все! Сообщите нам, если у вас есть какие-либо отзывы о процессе, результатах или функции в целом. 

## <a name="next-steps"></a>Дальнейшие действия
При возникновении проблем ознакомьтесь со [сведениями об устранении неполадок](troubleshoot-publisher-verification.md).