---
title: Рабочие учетные записи компании и центр партнеров
description: Как проверить, настроена ли в вашей компании Рабочая учетная запись Майкрософт, создать новую рабочую учетную запись или настроить несколько рабочих учетных записей для использования с центром партнеров.
author: dsindona
ms.author: parthp
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 3c0ee2f8d5291f7904435dea32d913adeaaf25c5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81262372"
---
# <a name="company-work-accounts-and-partner-center"></a>Рабочие учетные записи компании и центр партнеров

Центр партнеров использует рабочие учетные записи организации, также известные как клиенты Azure Active Directory (AD), для управления доступом к учетной записи для нескольких пользователей, разрешений управления, групп узлов и приложений, а также для обслуживания данных профиля. Связав домен учетной записи электронной почты вашей компании с учетной записью центра партнеров, сотрудники вашей компании могут войти в центр партнеров для управления предложениями Marketplace, используя собственные имена пользователей и пароли рабочих учетных записей.

## <a name="check-whether-your-company-already-has-a-work-account"></a>Проверьте, есть ли у вашей компании Рабочая учетная запись

Если ваша компания подписалась на облачную службу Майкрософт, например Azure, Microsoft Intune или Office 365, у вас уже есть домен рабочей учетной записи электронной почты (также называемый клиентом Azure Active Directory), который можно использовать с центром партнеров.

Выполните следующие действия, чтобы проверить следующее:
1. Войдите на портал администрирования Azure по адресу https://portal.azure.com.
2. Выберите **Azure Active Directory** в меню навигации слева и выберите **пользовательские доменные имена**.
3. Если у вас уже есть рабочая учетная запись, отобразится имя вашего домена.

Если у вашей компании еще нет рабочей учетной записи, она будет создана в процессе регистрации в центре партнеров.

## <a name="set-up-multiple-work-accounts"></a>Настройка нескольких рабочих учетных записей

Прежде чем выбрать существующую рабочую учетную запись, определите, сколько пользователей в рабочей учетной записи потребуется для доступа к центру партнеров. Если у вас есть пользователи в рабочей учетной записи, которым не требуется доступ к центру партнеров, вы можете создать несколько рабочих учетных записей, чтобы только те пользователи, которым требуется доступ к центру партнеров, были представлены в определенной учетной записи.

## <a name="create-a-new-work-account"></a>Создать новую рабочую учетную запись

Чтобы создать новую рабочую учетную запись для компании, выполните следующие действия. Может потребоваться помощь у любого пользователя, обладающего правами администратора в учетной записи Microsoft Azure вашей компании.

1. Войдите в [портал Microsoft Azure](https://portal.azure.com).
2. В меню навигации слева выберите **Azure Active Directory** -> **пользователей**.
3. Выберите **Новый пользователь** и создайте новую рабочую учетную запись Azure, введя имя и адрес электронной почты. Убедитесь, что для **роли каталога** задано значение **пользователь** , и установите флажок **Показать пароль** внизу, чтобы просмотреть и заметку автоматически созданного пароля.
4. Выберите **создать** , чтобы сохранить нового пользователя.

Адрес электронной почты для учетной записи пользователя должен быть проверенным доменным именем в каталоге. Вы можете вывести список всех проверенных доменов в каталоге, выбрав **Azure Active Directory** -> **имена пользовательских доменов** в меню навигации слева.

Дополнительные сведения о добавлении пользовательских доменов в Azure Active Directory см. в статье [Добавление домена в Azure AD или связывание с ним](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain).

## <a name="troubleshoot-work-email-sign-in"></a>Устранение неполадок при входе в рабочую почту

Если у вас возникают проблемы при входе в рабочую учетную запись (также известную как клиент Azure AD), найдите сценарий на приведенной ниже схеме, который наилучшим образом соответствует вашей ситуации, и выполните рекомендуемые действия.

![Схема для устранения неполадок при входе в рабочую учетную запись](./media/onboarding-aad-flow.png)

## <a name="next-steps"></a>Дальнейшие шаги

- [Управление учетной записью коммерческого магазина в центре партнеров](./manage-account.md) 
