---
title: Создание клиента Azure Active Directory
description: Узнайте, как создать клиент Azure Active Directory для регистрации и создания приложений.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: quickstart
ms.date: 03/12/2020
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40, fasttrack-edit
ms.openlocfilehash: 0e2247e94b20846f19c2ed26c96a5dc53972e770
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883819"
---
# <a name="quickstart-set-up-a-tenant"></a>Краткое руководство. Настройка клиента

Платформа удостоверений Майкрософт позволяет разработчикам создавать приложения, предназначенные для широкого круга пользовательских сред и удостоверений Microsoft 365. Чтобы начать работу с платформой удостоверений Майкрософт, вам потребуется доступ к среде, также называемой клиентом Azure AD, которая может регистрировать приложения и управлять ими, получать доступ к данным Microsoft 365 и развертывать ограничения пользовательского условного доступа и клиента.

Клиент представляет организацию. Это выделенный экземпляр службы Azure AD, который организация или разработчик приложения получает после начала сотрудничества с корпорацией Майкрософт, то есть после регистрации в Azure, Microsoft Intune или Microsoft 365.

Каждый клиент Azure AD отличается и отделяется от других клиентов Azure AD и имеет собственное представление рабочих и учебных учетных записей, удостоверений потребителей (если это клиент Azure AD B2C) и регистрации приложений. Регистрация приложения в клиенте может позволить осуществить проверку подлинности из учетных записей только в своем клиенте или во всех клиентах.

## <a name="determining-environment-type"></a>Определение типа среды

Вы можете создать среду одного из двух типов. Чтобы выбрать тип, необходимо основываться исключительно на типах пользователей, для которых приложение будет выполнять проверку подлинности.

* Рабочие и учебные (учетные записи Azure AD) или учетные записи Майкрософт (например, outlook.com и live.com)
* Учетные записи социальных сетей и локальные учетные записи (Azure AD B2C)

Это краткое руководство разбивается на два сценария, в зависимости от типа приложения, которое вы хотите создать. Если вам нужна дополнительная помощь в выборе типа удостоверения, взгляните на [общие сведения о платформе удостоверений Майкрософт](about-microsoft-identity-platform.md).

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>Рабочие и учебные учетные записи или личные учетные записи Майкрософт

### <a name="use-an-existing-tenant"></a>Использование имеющегося клиента

У многих разработчиков уже есть клиенты благодаря службам или подпискам, которые связаны с клиентами Azure AD (например, подписки Microsoft 365 или Azure).

1. Чтобы проверить клиента, войдите на [портал Azure](https://portal.azure.com) под учетной записью, которую вы хотите использовать для управления приложением.
1. Посмотрите в правый верхний угол. Если у вас есть клиент, вы автоматически войдете в него и сможете увидеть имя клиента непосредственно под своей учетной записью.
   * Наведите указатель мыши на имя учетной записи в верхней правой части портала Azure, чтобы увидеть свое имя, адрес электронной почты, идентификатор каталога или клиента (GUID), а также домен.
   * Если ваша учетная запись связана с несколькими клиентами, выберите имя учетной записи, чтобы открыть меню, где можно переключаться между клиентами. У каждого клиента есть свой идентификатор.

> [!TIP]
> Чтобы найти идентификатор клиента, вы можете сделать следующее:
> * Наведите указатель мыши на имя вашей учетной записи, чтобы получить данные об идентификаторе каталога или клиента.
> * Выберите **Azure Active Directory > Свойства > Идентификатор каталога** на портале Azure.

Если у вас нет клиента, связанного с учетной записью, GUID отобразится под именем учетной записи. Но вы не сможете регистрировать приложения, пока не выполните действия из следующего раздела.

### <a name="create-a-new-azure-ad-tenant"></a>Создание нового клиента Azure AD

Если у вас нет клиента Azure AD или вы хотите создать клиент для разработки, см. [это краткое руководство](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) или используйте [процедуру создания каталога](../fundamentals/active-directory-access-create-new-tenant.md). Вы должны будете предоставить следующие сведения, чтобы создать клиент:

- **Название организации**
- **Исходный домен** — он будет частью *.onmicrosoft.com. Вы сможете настроить домен позже.
- **Страна или регион**

> [!NOTE]
> Имя вашего клиента должно содержать буквы и цифры. Использовать специальные знаки не допускается. Длина имени не должна превышать 256 символов.

## <a name="social-and-local-accounts"></a>Учетные записи социальных сетей и локальные учетные записи

Чтобы приступить к созданию приложений, в которые выполняется вход под учетными записями социальных сетей и локальных учетных записей, необходимо создать клиент Azure AD B2C. Чтобы начать, перейдите к [созданию клиента Azure AD B2C](../../active-directory-b2c/tutorial-create-tenant.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Зарегистрируйте приложение](quickstart-register-app.md) и выполните интеграцию с платформой идентификации Майкрософт. 
* Ознакомьтесь с [основами аутентификации](authentication-scenarios.md).
* См. сведения о том, как [подписки Azure связаны с арендатором Azure AD](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
