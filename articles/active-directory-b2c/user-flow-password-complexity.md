---
title: Настройка требований к сложности пароля
titleSuffix: Azure AD B2C
description: Узнайте, как настроить требования к сложности паролей, которые клиенты указывают в Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c5ef550af0c7e19531ea19093ea937880f7dcf14
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78185647"
---
# <a name="configure-complexity-requirements-for-passwords-in-azure-active-directory-b2c"></a>Настройка требований к сложности паролей в Azure Active Directory B2C.

Azure Active Directory B2C (Azure AD B2C) поддерживает изменение требований сложности к паролям, которые пользователи указывают при создании учетной записи. По умолчанию Azure AD B2C использует пароли уровня `Strong`. Azure AD B2C также поддерживает параметры конфигурации для управления сложностью паролей, используемых клиентами.

## <a name="password-rule-enforcement"></a>Применение правила к паролю

Во время регистрации или сброса паролей пользователю необходимо ввести пароль, отвечающий требованиям сложности. Правила сложности пароля применяются согласно потоку пользователя. Во время регистрации один поток пользователя должен иметь Четырехзначный ПИН-код, тогда как во время регистрации для другого потока пользователя требуется 8-значная строка. Например, вы можете использовать поток пользователя с разной сложностью пароля для взрослых и детей.

Требования к сложности пароля не применяются во время входа. Во время входа пользователям никогда не предлагается сменить пароль, так как это не соответствует текущим требованиям сложности.

Сложность пароля можно настроить в следующих типах потоков пользователей.

- поток пользователя регистрации или входа в систему;
- поток пользователя сброса пароля;

При использовании пользовательских политик вы можете настраивать сложность пароля в них (подробнее см. [здесь](custom-policy-password-complexity.md)).

## <a name="configure-password-complexity"></a>Настройка сложности пароля

1. Войдите на [портал Azure](https://portal.azure.com).
2. Щелкните значок **Каталог + подписка** на панели инструментов портала, а затем выберите каталог, содержащий клиент Azure AD B2C.
3. В портал Azure найдите и выберите **Azure AD B2C**.
4. Выберите **потоки пользователя (политики)**.
2. Выберите поток пользователя и щелкните **Свойства**.
3. В разделе **Сложность пароля** выберите значение **Простой**, **Надежный** или **Настраиваемый**.

### <a name="comparison-chart"></a>Сравнительная таблица

| Сложность | Описание |
| --- | --- |
| Простая | Пароль, который содержит от 8 до 64 знаков. |
| Уровень согласованности Strong (сильная) | Пароль, который содержит от 8 до 64 знаков. Он требует наличия 3 из 4 знаков нижнего и верхнего регистра, цифр или символов. |
| Особые настройки | Этот параметр обеспечивает полный контроль над правилами сложности пароля.  Он дает возможность настроить пользовательскую длину,  а также принимать только числовые пароли (ПИН-коды). |

## <a name="custom-options"></a>Настраиваемые параметры

### <a name="character-set"></a>Кодировка

Дает возможность принимать только цифры (ПИН-коды) или полную кодировку.

- При использовании типа **Только цифры** во время ввода пароля можно применять только цифры (0–9).
- При использовании типа **Все** можно применять любые буквы, цифры или символы.

### <a name="length"></a>Длина

Дает возможность управлять требованиями к длине пароля.

- **Минимальная длина** предусматривает не менее 4 знаков.
- **Максимальная длина** предусматривает большее количество знаков, чем используется в минимальной длине (или равное ей), но не более 64 знаков.

### <a name="character-classes"></a>Категории знаков

Позволяет управлять типами других знаков, используемых в пароле.

- Тип **2 из 4: знаки нижнего и верхнего регистра, цифры (0–9), символы** гарантирует, что в пароле используются по крайней мере два типа знаков, например цифра и знак нижнего регистра.
- **3 из 4: строчные буквы, прописные буквы, цифры (0-9), символы** гарантируют, что пароль содержит по крайней мере три типа символов. например цифра, знак нижнего и верхнего регистра.
- Тип **4 из 4: знаки нижнего и верхнего регистра, цифры (0–9), символы** гарантирует, что в пароле используются все типы знаков.

    > [!NOTE]
    > Использование типа **4 из 4** может вызвать трудности у пользователей. Некоторые исследования показали, что это требование не повысит энтропию пароля. См. раздел [Appendix A — Strength of Memorized Secrets](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)(Приложение А — надежность запоминаемых секретов).
