---
title: Настройка языка в потоках пользователей Azure AD
description: Дополнительные сведения о настройке языка в потоках пользователя.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: mimart
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed56acc9276177951919a3bc63c8a1dc3876e1c4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594703"
---
# <a name="language-customization-in-azure-active-directory-preview"></a>Настройка языка в Azure Active Directory (предварительная версия)
|     |
| --- |
| Функция самостоятельной регистрации в Azure Active Directory предоставляется в режиме общедоступной предварительной версии. См. подробные сведения о [дополнительных условиях использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Настройка языка в Azure Active Directory (Azure AD) позволяет добавить в поток пользователя различные языки в соответствии с потребностями ваших пользователей. Корпорация Майкрософт предоставляет переводы для [36 языков](#supported-languages). Даже если интерфейс предоставляется только для одного языка, вы можете настроить имена атрибутов на странице "Коллекция атрибутов".

## <a name="how-language-customization-works"></a>Как работает функция настройки языка

По умолчанию настройка языка включена для пользователей, которые регистрируются, чтобы предоставить согласованное взаимодействие при регистрации. Вы можете использовать языки для изменения строк, отображаемых пользователям в процессе сбора атрибутов во время регистрации.

> [!NOTE]
> Если используются пользовательские атрибуты, необходимо предоставить собственный перевод. Дополнительные сведения см. в разделе [Настройка строк](#customize-your-strings).

## <a name="customize-your-strings"></a>Настройка строк

Настройка языка позволяет настроить любую строку в информации, передаваемой потоку пользователя.

1. Войдите на [портал Azure](https://portal.azure.com) с учетной записью администратора Azure AD.
2. В разделе **Службы Azure** щелкните **Azure Active Directory**.
3. В меню слева щелкните **Внешние удостоверения**.
4. Выберите **Потоки пользователей (предварительная версия)** .
3. Выберите поток пользователя, который необходимо включить для переводов.
4. Щелкните **Языки**.
5. На странице **Языки** для потока пользователя выберите язык, который требуется настроить.
6. Разверните **страницу сбора атрибутов**
7. Нажмите кнопку **Скачать значения по умолчанию** (или **Скачать переопределения**, если этот язык ранее менялся).

В результате будет создан JSON-файл, который можно использовать для изменения строк.

### <a name="change-any-string-on-the-page"></a>Замена любой строки на странице

1. Откройте скачанный JSON-файл в редакторе JSON.
1. Найдите элемент, требующий замены. Вы можете найти `StringId` искомой строки или найти атрибут `Value`, который вы хотите изменить.
1. Замените атрибут `Value` значением, которое требуется отобразить.
1. В каждой строке, которую необходимо изменить, замените `Override` на `true`.
1. Сохраните файл и отправьте изменения. (Элемент управления отправкой можно найти в том же месте, где вы загружали JSON-файл.)

> [!IMPORTANT]
> Если необходимо переопределить строку, измените значение `Override` на `true`. В противном случае запись не учитывается.

### <a name="change-extension-attributes"></a>Изменение атрибутов расширения

Если вы хотите изменить или добавить в JSON строку пользовательского атрибута, это нужно сделать в следующем формате:

```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Замените `<ExtensionAttribute>` именем пользовательского атрибута.

Замените `<ExtensionAttributeValue>` новой строкой для отображения.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Укажите список значений с помощью LocalizedCollections

Если вы хотите предоставить список значений для ответов, необходимо создать атрибут `LocalizedCollections`. `LocalizedCollections` является массивом пар `Name` и `Value`. Порядок размещения элементов определяет порядок их отображения. Для добавления `LocalizedCollections` используйте следующий формат.

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType",
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` представляет собой пользовательский атрибут, ответом на который является `LocalizedCollections`.
* `Name` является значением, которое видит пользователь.
* `Value` возвращается в утверждении при выборе этого параметра.

### <a name="upload-your-changes"></a>Загрузка изменений

1. Внеся изменения в JSON-файл, вернитесь к клиенту.
1. Щелкните **Потоки пользователей** и выберите поток пользователя, который необходимо включить для переводов.
1. Щелкните **Языки**.
1. Выберите язык, на который требуется выполнить перевод.
1. Выберите **страницу сбора атрибутов**
1. Щелкните значок папки и выберите JSON-файл для отправки.

Изменения автоматически сохраняются в потоке пользователя.

## <a name="additional-information"></a>Дополнительные сведения

### <a name="page-ui-customization-labels-as-overrides"></a>Использование меток настройки пользовательского интерфейса страницы для переопределения параметров страницы

При включении настройки языка все предыдущие изменения меток, использующих настройки пользовательского интерфейса страницы, сохраняются в JSON-файле для английского языка (en). Вы можете продолжить изменять метки и другие текстовые строки, отправив языковые ресурсы в настройках языка.

### <a name="up-to-date-translations"></a>Актуальные переводы

Корпорация Майкрософт стремится предоставить наиболее актуальные переводы Мы постоянно улучшаем переводы в соответствии с вашими требованиями. Мы выявляем ошибки и изменения в глобальной терминологии и обновляем ее, обеспечивая непрерывную поддержку в потоке пользователя.

### <a name="support-for-right-to-left-languages"></a>Поддержка языков с написанием справа налево

Корпорация Майкрософт сейчас не поддерживает языки с написанием справа налево. Их поддержку можно реализовать, используя пользовательские языковые стандарты и каскадные таблицы стилей для изменения способа отображения строк. При необходимости такой поддержки проголосуйте на [этом форуме для пользователей Azure](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).

### <a name="social-identity-provider-translations"></a>Переводы для поставщиков удостоверений в социальных сетях

Корпорация Майкрософт предоставляет параметр `ui_locales` OIDC для имен входа в социальные сети. Однако некоторые поставщики удостоверений в социальных сетях, включая Facebook и Google, не принимают его.

### <a name="browser-behavior"></a>Поведение браузера

И Chrome, и Firefox запрашивают настроенный язык. Если этот язык поддерживается, ему отдается предпочтение над языком, заданным по умолчанию. Сейчас Microsoft Edge не запрашивает язык и сразу отображает язык по умолчанию.

## <a name="supported-languages"></a>Поддерживаемые языки

Azure AD поддерживает следующие языки. Языки потока пользователей предоставляются Azure AD. Языки уведомлений многофакторной идентификации (MFA) предоставляются [Azure MFA](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).

| Язык              | Код языка | Маршруты пользователей         | Уведомления MFA  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Арабский                | ar            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Болгарский             | bg            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Бенгальский                | bn            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Каталонский               | ca            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Чешский                 | cs            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Датский                | da            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Немецкий                | de            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Греческий                 | el            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Английский               | en            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Испанский               | es            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Эстонский              | et            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Баскский                | eu            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Финский               | fi            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Французский                | fr            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Галисийский              | gl            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Гуджарати              | gu            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Иврит                | he            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Hindi                 | hi            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Хорватский              | hr            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Венгерский             | hu            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Индонезийский            | идентификатор            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Итальянский               | it            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Японский              | ja            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Казахский                | kk            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Каннада               | kn            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Корейский                | ko            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Литовский            | lt            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Латышский               | lv            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Малаялам             | ml            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Маратхи               | mr            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Малайский                 | ms            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Норвежский (букмол)      | nb            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Нидерландский                 | nl            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Норвежский             | нет            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Панджаби               | pa            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Польский                | pl            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Португальский (Бразилия)   | pt-br         | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Португальский (Португалия) | pt-pt         | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Румынский              | ro            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Русский               | ru            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Словацкий                | sk            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Словенский             | sl            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Сербский — кириллица    | sr-cryl-cs    | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Сербский — латиница       | sr-latn-cs    | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Шведский               | sv            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Тамильский                 | ta            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Телугу                | te            | ![да](./media/user-flow-customize-language/yes.png) | ![нет](./media/user-flow-customize-language/no.png) |
| Тайский                  | th            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Турецкий               | tr            | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Украинский             | uk            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Вьетнамский            | vi            | ![нет](./media/user-flow-customize-language/no.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Китайский (упрощенное письмо)  | zh-hans       | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |
| Китайский (традиционное письмо) | zh-hant       | ![да](./media/user-flow-customize-language/yes.png) | ![да](./media/user-flow-customize-language/yes.png) |