---
title: Распространенные сценарии управления приложениями для Azure Active Directory | Документация Майкрософт
description: Централизованное управление приложениями с помощью Azure AD
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/02/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1874a2f2cf96aaa905616bddcc6cb83c60c1d279
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83115614"
---
# <a name="centralize-application-management-with-azure-ad"></a>Централизованное управление приложениями с помощью Azure AD

Пароли, как ИТ-кошмар, так и трудности для сотрудников по всему миру. Это объясняется тем, что все больше и больше компаний переходят Azure Active Directory, решения Майкрософт по управлению удостоверениями и доступом для облака и всех других ресурсов. Переход от приложения к приложению без необходимости ввода пароля для каждого из них. Перейдем от Outlook к Workday, чтобы в ADP быстро открывать их, а также быстро и безопасно. Вы сможете сотрудничать с партнерами и даже другими за пределами вашей организации, не вызвав ее. Более того, Azure AD помогает управлять рисками, обеспечивая защиту приложений, используемых с точки зрения многофакторной проверки подлинности, чтобы убедиться, что вы используете непрерывное Адаптивное машинное обучение и логику безопасности, чтобы обнаруживать подозрительные входы в систему, обеспечивая безопасный доступ к нужным приложениям, где бы вы ни находились. Он не только подходит для пользователей, но и для него. Благодаря проверкам доступа по требованию и полнофункциональному набору управления Azure AD помогает поддерживать соответствие и применять политики. Благодаря этому можно даже автоматизировать подготовку учетных записей пользователей, упрощая управление доступом. Ознакомьтесь с некоторыми распространенными сценариями, в которых клиент использует возможности управления приложениями Azure Active Directory.

**Распространенные сценарии**


> [!div class="checklist"]
> * Единый вход для всех приложений
> * Автоматизация подготовки и отмены подготовки 
> * Защита приложений
> * Управление доступом к приложениям
> * Гибридный безопасный доступ

## <a name="scenario-1-set-up-sso-for-all-your-applications"></a>Сценарий 1. Настройка единого входа для всех приложений

Больше не следует управлять паролем. Безопасный доступ ко всем необходимым ресурсам с учетными данными Организации. 

|Компонент  | Описание | Рекомендация |
|---------|---------|---------|
|Единый вход|Федеративный единый вход на основе стандартов, использующий надежные отраслевые стандарты.|Всегда используйте [SAML/OIDC](https://docs.microsoft.com/azure/active-directory/manage-apps/isv-choose-multi-tenant-federation) , чтобы включить единый вход, когда приложение поддерживает его.|
|Панель доступа|Предоставляет пользователям простой центр для обнаружения приложений и доступа к ним. Предоставьте им более высокую производительность благодаря возможностям самообслуживания, таким как запрос доступа к приложениям и группам, или управление доступом к ресурсам от имени других пользователей.| Разверните [панель доступа](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-deployment-plan) в Организации после интеграции приложений с Azure AD для единого входа.|

## <a name="scenario-2-automate-provisioning-and-deprovisioning"></a>Сценарий 2. Автоматизация подготовки и отмены подготовки 


Большинству приложений требуется подготовка пользователя в приложении перед доступом к необходимым ресурсам. Использование CSV-файлов или сложных скриптов может быть трудоемким и сложным в управлении. Кроме того, клиентам необходимо гарантировать, что учетные записи будут удалены, когда у человека больше не будет доступа. Используйте приведенные ниже инструменты для автоматизации подготовки и отмены подготовки. 


|Компонент  |Описание|Рекомендация |
|---------|---------|---------|
|Подготовка SCIM|[Scim](https://aka.ms/SCIMOverview) — это лучшая в отрасли методика автоматизации подготовки пользователей. Любое приложение, совместимое с SCIM, можно интегрировать с Azure AD. Автоматическое создание, обновление и удаление учетных записей пользователей без необходимости сохранять CSV-файлы, пользовательские сценарии или локальные решения.|Ознакомьтесь со всем растущем списком [предварительно интегрированных](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) приложений в коллекции приложений Azure AD.|
|Microsoft Graph|Используйте вздохните и глубину данных, которые служба Azure AD должна расширить для приложения с помощью необходимых данных.|Используйте [Microsoft Graph](https://developer.microsoft.com/graph/) для получения данных из экосистемы корпорации Майкрософт. |


## <a name="scenario-3-secure-your-applications"></a>Сценарий 3. Защита приложений
Identity — это линчпин для безопасности. Если удостоверение нарушается, очень трудно приступить к его достижению. В среднем за 100 дней перед организациями обнаруживаются нарушения безопасности. Используйте средства, предоставляемые Azure AD, чтобы повысить уровень безопасности приложений. 

|Компонент  |Описание| Рекомендация |
|---------|---------| ---------|
|Многофакторная идентификация Azure|Многофакторная идентификация (MFA) Azure — решение Майкрософт для двухэтапной проверки. Используя методы проверки подлинности, утвержденные администратором, Azure MFA помогает защитить доступ к данным и приложениям, а также потребует простого процесса входа.| [Включите MFA](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/all-your-creds-are-belong-to-us/ba-p/855124) для пользователей.  |
|Условный доступ|С помощью условного доступа можно реализовать автоматизированные решения по контролю доступа, которые могут получать доступ к облачным приложениям на основе условий.| Проверьте [Параметры безопасности по умолчанию](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) и [Общие политики](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common) , которые клиенты используют. | 
|Защита идентификации|Защита идентификации использует сведения, полученные корпорацией Майкрософт в ходе выполнения обязанностей, в организациях с Azure AD, пространстве пользователей с учетными записями Майкрософт, а также в играх с Xbox, чтобы защитить пользователей. Чтобы обнаружить угрозы и защитить клиентов от таких угроз, корпорация Майкрософт анализирует 6,5 триллионов сигналов в день.|Включите [политики защиты идентификации по умолчанию](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-policies) , предоставляемые нашей службой. | 

## <a name="scenario-4-govern-access-to-your-applications"></a>Сценарий 4. Управление доступом к приложениям
Управление удостоверениями помогает организациям достичь баланса между производительностью — как быстро можно получить доступ к нужным приложениям, например, когда они присоединяются к моей организации? и безопасности (динамика изменения уровня доступа со временем, например при изменении роли пользователя в организации). 

|Компонент  |Описание|Рекомендация |
|---------|---------| ---------|
|елм|Управление назначением Azure AD позволяет пользователям как внутри, так и за пределами Организации более эффективно управлять доступом к своим приложениям.| Разрешить пользователям без прав администратора управлять доступом к приложениям с помощью [пакетов Access](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-first).|
|Проверки доступа|Доступ пользователей к приложениям можно проверить регулярно, чтобы убедиться, что доступ к ним имеют только нужные пользователи.| [Проверьте доступ](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) к наиболее важным приложениям. |
|Log Analytics|Создание отчетов о том, кто обращается к каким приложениям и сохраняет их в средстве SIEM, чтобы сопоставлять данные между источниками данных и со временем.| Включите [log Analytics](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) и настройте оповещения для критических событий, связанных с вашими приложениями. |


## <a name="scenario-5-hybrid-secure-access"></a>Сценарий 5. гибридный безопасный доступ
Удостоверение может быть только плоскостью управления, если она может подключаться ко всем облачным и локальным приложениям. Используйте средства, предоставляемые Azure AD и ее партнерами для безопасного доступа к приложениям на основе предыдущих версий.

|Компонент  |Описание|Рекомендация |
|---------|---------|---------|
|Прокси приложения|Сегодня сотрудникам нужно продуктивно работать без привязки к расположению, времени и оборудованию. Им требуется доступ к приложениям SaaS в облаке и корпоративных приложениях в локальной среде. Прокси приложения Azure AD обеспечивает этот надежный доступ без дорогостоящих и сложных виртуальных частных сетей (VPN) или демилитаризованных зон (сети периметра).|Настройка [удаленного доступа](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) к локальным приложениям. |
|F5, Akamai, Zscaler|Используя имеющийся контроллер доставки и сети, вы можете легко обеспечить безопасность важных устаревших приложений, которые вы раньше не могли защитить с помощью Azure AD. Теперь вы имеете все необходимое для защиты этих приложений.| С помощью Akamai, Citrix, F5 или Zscaler? Ознакомьтесь [с предварительно созданными решениями](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access). | 

## <a name="related-articles"></a>Похожие статьи

- [Управление приложениями](https://docs.microsoft.com/azure/active-directory/manage-apps/index)
- [Подготовка приложений](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning)
- [Гибридный безопасный доступ](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)
- [Управление удостоверениями](https://docs.microsoft.com/azure/active-directory/governance/identity-governance-overview)
- [Платформа удостоверений Майкрософт](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)
- [Безопасность идентификации](https://docs.microsoft.com/azure/active-directory/conditional-access/index)
