---
title: Автоматическая подготовка пользователей приложения SaaS в Azure AD
description: Общие сведения об использовании Azure AD для автоматической подготовки, отзыва и постоянного обновления учетных записей пользователей в нескольких приложениях SaaS сторонних разработчиков.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: conceptual
ms.workload: identity
ms.date: 11/25/2019
ms.author: mimart
ms.reviewer: arvinh, celested
ms.openlocfilehash: 1e72d885858b543999090a4a0521845d556802fd
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82593120"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-applications-with-azure-ad"></a>Автоматизация подготовки пользователей и ее отмены в приложениях с помощью Azure AD

В Azure Active Directory (Azure AD) термин " **Подготовка приложения** " означает автоматическое создание удостоверений пользователей и ролей в облачных приложениях ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)), к которым пользователям требуется доступ. Кроме создания удостоверений пользователей, автоматическая подготовка включает в себя обслуживание и удаление удостоверений пользователей по мере изменения их статуса или ролей. Типичные сценарии включают подготовку пользователя Azure AD к таким приложениям, как [Dropbox](../saas-apps/dropboxforbusiness-provisioning-tutorial.md), [Salesforce](../saas-apps/salesforce-provisioning-tutorial.md), [ServiceNow](../saas-apps/servicenow-provisioning-tutorial.md), и других.

![Обзорная схема подготовки](./media/user-provisioning/provisioning-overview.png)

Эта функция позволяет:

- **Автоматизация подготовки**: автоматическое создание новых учетных записей в правильных системах для новых людей, присоединяемых к вашей команде или организации.
- **Автоматизация отмены инициализации:** Автоматически деактивировать учетные записи в правильных системах, когда люди оставляют команду или организацию.
- **Синхронизация данных между системами:** Убедитесь, что удостоверения в приложениях и системах актуальны в соответствии с изменениями в каталоге или системе отдела кадров.
- **Группы инициализации:** Подготавливайте группы для приложений, которые их поддерживают.
- **Управление доступом:** Мониторинг и аудит, подготовленные для приложений.
- **Простое развертывание в условиях использования поля Браун:** Сопоставление существующих удостоверений между системами и предоставление простой интеграции, даже если пользователи уже существуют в целевой системе.
- **Используйте широкие возможности настройки:** Воспользуйтесь преимуществами настраиваемых сопоставлений атрибутов, определяющих, какие данные пользователя должны передаваться из исходной системы в целевую систему.
- **Получение оповещений о критических событиях:** Служба подготовки предоставляет оповещения для критически важных событий и позволяет интегрировать Log Analytics, где можно определить пользовательские оповещения для удовлетворения бизнес-потребностей.

## <a name="benefits-of-automatic-provisioning"></a>Преимущества автоматической подготовки

По мере роста количества приложений, используемых в современных организациях, ИТ – администраторы получают управление доступом в масштабе. Такие стандарты, как SAML или Open ID Connect (OIDC), позволяют администраторам быстро настроить единый вход (SSO), но для доступа также требуется, чтобы пользователи были подготовлены в приложении. Для многих администраторов подготовка означает ручное создание каждой учетной записи пользователя или отправку CSV-файлов каждую неделю, но эти процессы требуют длительных, дорогостоящих и подверженных ошибкам. Такие решения, как SAML JIT (JIT), были реализованы для автоматизации подготовки, но компаниям также требуется решение для отмены подготовки пользователей, когда они оставляют организацию или больше не требуют доступа к определенным приложениям в зависимости от изменения роли.

Ниже перечислены некоторые распространенные причины использования автоматической подготовки.

- Максимально эффективное быстродействие и точность процессов подготовки.
- Экономия ресурсов, связанных с размещением и обслуживанием пользовательских решений и сценариев подготовки.
- Защита Организации путем мгновенного удаления удостоверений пользователей из ключевых приложений SaaS при выходе из Организации.
- Простое импортирование большого числа пользователей в определенное приложение SaaS или систему.
- Наличие одного набора политик для определения пользователей, которые подготавливается, и пользователей, которые могут входить в приложение.

Подготовка пользователей Azure AD поможет решить эти проблемы. Чтобы узнать больше о том, как клиенты используют подготовку пользователей Azure AD, можно ознакомиться с примером [использования ASOS](https://aka.ms/asoscasestudy). В видео ниже приведены общие сведения о подготовке пользователей в Azure AD.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Какие приложения и системы можно использовать с автоматической подготовкой пользователей Azure AD

В Azure AD реализована предварительно интегрированная поддержка многих популярных приложений SaaS и систем отдела кадров, а также общая поддержка приложений, которые реализуют определенные части [стандарта SCIM 2,0](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010).

* **Предварительно интегрированные приложения (приложения SaaS в коллекции)**. Все приложения, для которых Azure AD поддерживает предварительно интегрированный соединитель подготовки, можно найти в [списке руководств по приложениям для подготовки пользователей](../saas-apps/tutorial-list.md). Предварительно интегрированные приложения, перечисленные в галерее, обычно используют API-интерфейсы управления пользователями на основе SCIM 2,0 для подготовки. 

   ![Логотип Salesforce](./media/user-provisioning/gallery-app-logos.png)

   Если вы хотите запросить новое приложение для подготовки, вы можете [запросить интеграцию приложения с нашей коллекцией приложений](../develop/howto-app-gallery-listing.md). Для запроса на подготовку пользователя требуется, чтобы приложение имело конечную точку, совместимую с SCIM. Запросите поставщика приложения следовать стандарту SCIM, чтобы мы могли быстро подключить приложение к нашей платформе.

* **Приложения, поддерживающие SCIM 2,0**. Сведения о универсальных способах подключения приложений, реализующих API-интерфейсы управления пользователями на основе SCIM 2,0, см. в статьях [Создание конечной точки scim и Настройка подготовки пользователей](use-scim-to-provision-users-and-groups.md).

## <a name="what-is-system-for-cross-domain-identity-management-scim"></a>Что такое система для управления междоменными удостоверениями (SCIM)?

Чтобы помочь автоматизировать подготовку и отмену подготовки, приложения предоставляют собственные интерфейсы API пользователей и групп. Однако все, кто пытается управлять пользователями в нескольких приложениях, покажет, что каждое приложение пытается выполнить одни и те же простые действия, такие как создание или обновление пользователей, Добавление пользователей в группы или отменяется предоставление пользователям. Но все эти простые действия реализуются немного по-разному, используя разные пути к конечным точкам, различные методы для указания сведений о пользователе и другая схема для представления каждого элемента информации.

Для решения этих проблем спецификация SCIM предоставляет общую схему пользователя, помогающую пользователям переходить к приложениям, выходить из них и вокруг них. SCIM становится стандартом де-факто для подготовки и, при использовании в сочетании с стандартами Федерации, такими как SAML или OpenID Connect Connect, предоставляет администраторам комплексное решение для управления доступом на основе стандартов.

Подробное руководство по разработке конечной точки SCIM для автоматизации подготовки и деподготовки пользователей и групп для приложения см. в разделе [Создание конечной точки scim и Настройка подготовки пользователей](use-scim-to-provision-users-and-groups.md). Для предварительно интегрированных приложений в коллекции (временной резерв, Azure Databricks, снежинки и т. д.) вы можете пропустить документацию для разработчиков и использовать приведенные [здесь](../saas-apps/tutorial-list.md)учебники.

## <a name="manual-vs-automatic-provisioning"></a>Сравнение подготовки вручную и автоматической подготовки

Приложения в коллекции Azure AD поддерживают один из двух режимов подготовки:

* Подготовка **вручную** означает, что для этого приложения еще нет соединителя автоматической подготовки Azure AD. Учетные записи пользователей должны создаваться вручную, например путем добавления пользователей непосредственно в административный портал приложения или отправки электронной таблицы со сведениями об учетной записи пользователя. Обратитесь к документации, предоставляемой приложением, или обратитесь к разработчику приложения, чтобы определить, какие механизмы доступны.

* **Автоматическая подготовка** означает, что для этого приложения был разработан соединитель для подготовки Azure AD. Следуйте указаниям в руководстве по установке, чтобы настроить подготовку для приложения. Руководства по настройке для приложений можно найти в [Списке руководств по интеграции приложений SaaS в Azure Active Directory](../saas-apps/tutorial-list.md).

В коллекции Azure AD приложения, поддерживающие автоматическую подготовку, обозначаются значком **подготовки** . Переключитесь на новую предварительную версию коллекции, чтобы просмотреть эти значки (в заголовке в верхней части **страницы Добавление приложения**выберите ссылку, **щелкнув здесь, чтобы испытать новую и улучшенную коллекцию приложений**).

![Значок подготовки в коллекции приложений](./media/user-provisioning/browse-gallery.png)

Режим подготовки, поддерживаемый приложением, также отображается на вкладке **Подготовка** после добавления приложения в **корпоративные приложения**.

## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Как настроить автоматический вход в приложение

Для предварительно интегрированных приложений, перечисленных в коллекции, доступны пошаговые инструкции по настройке автоматической подготовки. См. [список учебников по интегрированным приложениям галереи](../saas-apps/tutorial-list.md). В следующем видео показано, как настроить автоматическую подготовку пользователей для SalesForce.

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Для других приложений, поддерживающих SCIM 2,0, выполните действия, описанные в статье [Создание конечной точки scim и Настройка подготовки пользователей](use-scim-to-provision-users-and-groups.md).


## <a name="related-articles"></a>Похожие статьи

- [Список руководств по интеграции приложений SaaS](../saas-apps/tutorial-list.md)
- [Настройка сопоставлений атрибутов для подготовки пользователей](customize-application-attributes.md)
- [Написание выражений для сопоставлений атрибутов](../app-provisioning/functions-for-customizing-application-data.md)
- [Фильтры области для подготовки пользователей](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
- [Создание конечной точки SCIM и Настройка подготовки пользователей](use-scim-to-provision-users-and-groups.md)
- [Общие сведения об API синхронизации Azure AD](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
