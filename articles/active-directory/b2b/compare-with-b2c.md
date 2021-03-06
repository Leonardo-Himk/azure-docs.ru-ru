---
title: Сравнение внешних удостоверений (Azure Active Directory) | Документация Майкрософт
description: С помощью внешних удостоверений Azure AD пользователи вне вашей организации могут получать доступ к приложениям и ресурсам, используя собственные удостоверения. Сравните решения для внешних удостоверений, например службу Azure Active Directory B2B для совместной работы и Azure AD B2C.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9006a70ae941abb700412a7c596627939c994028
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587516"
---
# <a name="compare-solutions-for-external-identities-in-azure-active-directory"></a>Сравнение решений для внешних удостоверений в Azure Active Directory

Внешние удостоверения Azure AD позволяют предоставлять пользователям вне вашей организации доступ к приложениям и ресурсам, разрешая им вход с любыми удостоверениями на их выбор. Ваши партнеры, дистрибьюторы, поставщики и другие гостевые пользователи смогут "предоставлять собственные удостоверения". Независимо от того, входят ли они в Azure AD (или другую систему, управляемую ИТ-отделом) или используют неуправляемые идентификаторы социальных сетей (таких, как Google или Facebook), они могут использовать для входа собственные учетные данные. Поставщик удостоверений управляет удостоверениями внешних пользователей, а вы через Azure AD контролируете доступ к приложениям в соответствии с политикой защиты ресурсов. 

## <a name="external-identities-scenarios"></a>Сценарии использования внешних удостоверений

Внешние удостоверения Azure AD не учитывают характер связи пользователя с организацией, а предназначены для поддержки методов входа, которые этот пользователь хочет применять для доступа к приложениям и ресурсам. В рамках этой платформы Azure AD поддерживает несколько сценариев для совместной работы между предприятиями (B2B) и взаимодействия организации с клиентами и потребителями (B2C).

- **Совместное использование приложений с внешними пользователями (совместная работа B2B)** . Пригласите внешних пользователей в свой клиент в качестве "гостевых" пользователей, которым вы сможете назначить разрешения (для авторизации), сохраняя возможность использовать существующие учетные данные (для проверки подлинности). Пользователи выполняют вход в систему для доступа к общим ресурсам, используя простой процесс приглашения и активации с помощью рабочей, учебной учетной записи или любой учетной записи электронной почты. Теперь, благодаря наличию потоков самостоятельной регистрации пользователей (предварительная версия) вы можете предоставить внешним пользователям интерфейс для входа через приложение, к которому вы предоставляете общий доступ. Вы можете настроить параметры пользовательского потока, чтобы управлять методами входа пользователей в приложение, сохраняя для них возможность входить с помощью рабочей, учебной учетной записи или любого удостоверения социальной сети (например, Google или Facebook).  Дополнительные сведения см. в [документации по Azure AD B2B](index.yml).

- **Разрабатывайте приложения, предназначенные для других клиентов Azure AD (с одним или несколькими клиентами)** . При разработке приложений для Azure AD можно нацеливаться на пользователей из одной организации (с одним клиентом) или пользователей из любой организации, у которой есть клиент Azure AD (приложение с несколькими клиентами). Такие приложения с несколькими клиентами регистрируются в Azure AD один раз, но могут использоваться любым пользователем Azure AD из любой организации, не требуя с вашей стороны дополнительных действий.

- **Разрабатывайте небрендированные приложения для потребителей и клиентов (Azure AD B2C)** . Если ваша организация или вы как разработчик создаете приложения для взаимодействия с клиентами, Azure AD B2C поможет вам выйти на широкий круг потребителей, клиентов и (или) граждан. Разработчики могут использовать Azure AD в качестве полнофункциональной системы идентификации для своего приложения, разрешая пользователям вход с помощью имеющихся удостоверений (например, Gmail или Facebook). С помощью Azure AD B2C вы можете настраивать и контролировать процесс регистрации, входа и управления профилями клиентов при использовании ваших приложений. Дополнительные сведения см. в [документации по Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/).

В следующей таблице приводится подробное сравнение разных сценариев, которые можно реализовать с помощью внешних удостоверений Azure AD.

| Мультитенантные приложения  | Совместная работа с внешними пользователями (B2B) | Приложения для потребителей или клиентов (B2C)  |
| ---- | --- | --- |
| Основной сценарий: корпоративное программное обеспечение как услуга (SaaS) | Основной сценарий: совместная работа в приложениях Майкрософт (Office 365, Teams и т. п.) или в собственном программном обеспечении для совместной работы.  | Основной сценарий: транзакционные приложения на основе пользовательских приложений.   |
| Предполагаемое использование Организации, которые намерены предоставлять программное обеспечение многим корпоративным клиентам.    | Предполагаемое использование Организации, которым требуется аутентифицировать пользователей из партнерской организации независимо от поставщика удостоверений.    | Предполагаемое использование Приглашение пользователей мобильных приложений и веб-приложений (отдельных клиентов, учреждений или организаций) в каталог в Azure AD, отделенный от собственного каталога организации. |
| Поддерживаемые удостоверения Сотрудники с учетными записями Azure AD | Поддерживаемые удостоверения Сотрудники с рабочими или учебными учетными записями, партнеры с рабочими или учебными учетными записями либо любой адрес электронной почты. Вскоре будет реализована поддержка прямой федерации.      | Поддерживаемые удостоверения Пользователи с учетными записями в локальных приложениях (любой адрес электронной почты или имя пользователя) или любой поддерживаемый внешний идентификатор с прямой федерацией.       |
| Управление внешними пользователями осуществляется в собственном каталоге, изолированном от каталога, в котором зарегистрировано приложение.    | Внешние пользователи, для управления которыми используется тот же каталог, что и для сотрудников, но со специальными заметками. Этими пользователями можно управлять так же, как и сотрудниками, их можно добавить в те же группы, и так далее.    | Внешние пользователи, для управления которыми используется каталог приложения. Управление этими пользователями осуществляется отдельно от каталога сотрудников и партнеров организации (при наличии).  |
| Единый вход. Поддерживается единый вход для всех подключенных к Azure AD приложений.          | Единый вход. Поддерживается единый вход для всех подключенных к Azure AD приложений. Например, можно предоставить доступ к приложениям Office 365 или локальным приложениям, а также к другим приложениям SaaS, таким как Salesforce или Workday.    | Единый вход. В клиентах Azure AD B2C поддерживается единый вход в приложения пользователя. Единый вход в Office 365 или другие приложения SaaS корпорации Майкрософт не поддерживается.    |
| Жизненный пользователя За управление отвечает основная организация пользователя.      | Жизненный цикл партнера Управляется основной или приглашающей организацией.    | Жизненный пользователя Управляется самостоятельно или приложением.      |
| Политика безопасности и соответствие требованиям За управление отвечает основная или приглашающая организация (например, с использованием [политик условного доступа](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).           | Политика безопасности и соответствие требованиям За управление отвечает основная или приглашающая организация (например, с использованием [политик условного доступа](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)). | Политика безопасности и соответствие требованиям Управляется приложением.        |
| Фирменная символика Используется торговая марка основной или приглашающей организации.   | Фирменная символика Используется торговая марка основной или приглашающей организации.    | Фирменная символика Управляется приложением. Как правило, используется торговая марка продукта с изображением фирменной символики организации на фоне.   |
| Дополнительные сведения: [Управление удостоверением в мультитенантных приложениях](https://docs.microsoft.com/azure/architecture/multitenant-identity/), [практическое руководство](https://docs.microsoft.com/azure/active-directory/develop/howto-convert-app-to-be-multi-tenant) | Дополнительные сведения: [запись блога](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [документация](what-is-b2b.md)                   | Дополнительные сведения: [страница продукта](https://azure.microsoft.com/services/active-directory-b2c/), [документация](https://docs.microsoft.com/azure/active-directory-b2c/).       |

Защита клиентов и партнеров и управление ими за пределами организации с использованием внешних удостоверений Azure AD.

### <a name="next-steps"></a>Дальнейшие действия

- [Что такое служба совместной работы Azure AD B2B?](what-is-b2b.md)
- [Общие сведения об Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/overview)
