---
title: Отключить проверку электронной почты во время регистрации клиента с помощью настраиваемой политики
titleSuffix: Azure AD B2C
description: Узнайте, как отключить проверку электронной почты во время регистрации клиента в Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 13a5fa6a030d876d92651ca587e37fdc6a3ec600
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79136148"
---
# <a name="disable-email-verification-during-customer-sign-up-using-a-custom-policy-in-azure-active-directory-b2c"></a>Отключить проверку электронной почты во время регистрации клиента с помощью настраиваемой политики в Azure Active Directory B2C

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

## <a name="prerequisites"></a>Предварительные требования

Выполните шаги, описанные в статье [Начало работы с настраиваемыми политиками в Azure Active Directory B2C](custom-policy-get-started.md). У вас должна быть Рабочая настраиваемая политика для регистрации и входа с помощью социальных сетей и локальных учетных записей.

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Добавление метаданных в самостоятельно утвержденный технический профиль

Технический профиль **локалаккаунтсигнупвислогонемаил** — это [самоподтвержденный](self-asserted-technical-profile.md), который вызывается во время процесса регистрации. Чтобы отключить проверку по электронной почте, установите `EnforceEmailVerification` для метаданных значение false. Переопределите технические профили Локалаккаунтсигнупвислогонемаил в файле расширения. 

1. Откройте файл расширений политики. Например, <em> `SocialAndLocalAccounts/` </em>.
1. Найдите элемент `ClaimsProviders`. Если такой элемент не существует, добавьте его.
1. Добавьте в `ClaimsProviders` элемент следующий поставщик утверждений:

```XML
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <Metadata>
        <Item Key="EnforceEmailVerification">false</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="test-the-custom-policy"></a>Проверка пользовательской политики

1. Войдите на [портал Azure](https://portal.azure.com).
2. Убедитесь, что вы используете каталог, содержащий клиент Azure AD, выбрав фильтр " **каталог и подписка** " в верхнем меню и выбрав Каталог, содержащий ваш клиент Azure AD.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Регистрация приложений**.
4. Выберите **Инфраструктура процедур идентификации**.
5. Выберите **Отправка пользовательской политики** и отправьте два файла политики, которые вы изменили ранее.
2. Выберите отправленную вами политику регистрации или входа и нажмите кнопку **Выполнить**.
3. Вы можете зарегистрироваться, используя адрес электронной почты без проверки.


## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения о [самостоятельно утвержденном техническом профиле](self-asserted-technical-profile.md) см. в справочнике по инфраструктура процедур идентификации.
