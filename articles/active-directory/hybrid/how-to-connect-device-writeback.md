---
title: 'Azure AD Connect: включение обратной записи устройств | Документация Майкрософт'
description: В этом документе объясняется, как включить функцию обратной записи устройств с помощью службы Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 632f6f80184c6ba3409bd30ae070cbaefc77f036
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "67109497"
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: включение обратной записи устройств
> [!NOTE]
> Для обратной записи устройств требуется подписка Azure AD Premium.
> 
> 

Следующая документация содержит сведения о том, как включить функцию обратной записи устройств в службе Azure AD Connect. Обратная запись устройств используется в ситуациях,

* Включение [Windows Hello для бизнеса с помощью развертывания гибридного доверия сертификатов](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust-prereqs#device-registration)
* Включение условного доступа на основе устройств в службы ADFS (2012 R2 или более поздней версии) защищенных приложений (отношения доверия с проверяющей стороной).

Это обеспечивает дополнительную защиту и гарантию того, что доступ к приложениям предоставляется только доверенным устройствам. Дополнительные сведения об условном доступе см. в статьях [Управление рисками с помощью условного доступа](../active-directory-conditional-access-azure-portal.md) и [Настройка локального условного доступа с использованием регистрация устройств Azure Active Directory](../../active-directory/active-directory-device-registration-on-premises-setup.md).

> [!IMPORTANT]
> <li>Устройства должны находиться в том же лесу, что и пользователи. Поскольку обратная запись устройств должна производиться в один лес, эта функция в настоящий момент не поддерживает развертывание с несколькими лесами пользователей.</li>
> <li>В локальный лес Active Directory можно добавить только один объект конфигурации регистрации. Эта функция несовместима с топологией, в которой локальный каталог Active Directory синхронизируется в несколько каталогов Azure AD.</li>

## <a name="part-1-install-azure-ad-connect"></a>Часть 1. Установка Azure AD Connect
Установите Azure AD Connect с использованием стандартных или пользовательских параметров. Корпорация Майкрософт рекомендует перед включением обратной записи устройства выполнить синхронизацию всех пользователей и групп.

## <a name="part-2-enable-device-writeback-in-azure-ad-connect"></a>Часть 2. Включение обратной записи устройств в Azure AD Connect
1. Снова запустите мастер установки. Выберите **Настройка параметров устройств** на странице "Дополнительные задачи" и нажмите кнопку **Далее**. 

    ![Настройка параметров устройств](./media/how-to-connect-device-writeback/deviceoptions.png)

    >[!NOTE]
    > Новая функция настройки параметров устройств доступна только в версии 1.1.819.0 и более поздних.

2. На странице параметров устройства выберите **Настроить обратную запись устройств**. Параметр **Отключить обратную запись устройств** станет доступным после включения обратной записи устройства. Щелкните **Далее**, чтобы перейти на следующую страницу мастера.
    ![Выбор операции устройства](./media/how-to-connect-device-writeback/configuredevicewriteback1.png)

3. На странице обратной записи вы увидите указанный домен в качестве леса обратной записи устройств по умолчанию.
   ![Выборочная установка: целевой лес обратной записи устройства](./media/how-to-connect-device-writeback/writebackforest.png)

4. Страница **Контейнер устройств** содержит параметр подготовки Active Directory с использованием одного из двух вариантов:

    а. **Предоставление учетных данных администратора предприятия**. Если учетные данные администратора предприятия предоставлены для леса, в который необходимо выполнить обратную запись устройств, Azure AD Connect автоматически подготовит лес во время настройки обратной записи устройства.

    b. **Загрузка сценария PowerShell **. Azure AD Connect автоматически создает сценарий PowerShell, который может подготовить Active Directory для обратной записи устройства. Если учетные данные администратора предприятия не могут быть предоставлены в Azure AD Connect, предлагаем загрузить сценарий PowerShell. Предоставьте загруженный сценарий PowerShell **CreateDeviceContainer.psq** администратору предприятия леса, в который будет выполнена обратная запись устройств.
    ![Подготовка леса Active Directory](./media/how-to-connect-device-writeback/devicecontainercreds.png)
    
    Для подготовки леса Active Directory выполняются следующие операции:
    * Если они больше не существуют, функция создает и настраивает их в разделах CN=Device Registration Configuration,CN=Services,CN=Configuration,[forest-dn].
    * Если они больше не существуют, функция создает и настраивает их в разделе CN=RegisteredDevices,[domain-dn]. Объекты устройства будут созданы в этом контейнере.
    * Задает необходимые разрешения для учетной записи Azure AD Connector для управления устройствами в Active Directory.
    * Предполагает выполнение только в одном лесу, даже если служба Azure AD Connect устанавливается в нескольких лесах.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Проверка синхронизации устройств с Active Directory
Функция обратной записи устройств должна теперь работать правильно. Имейте в виду, что обратная запись объектов устройств в AD может выполняться до 3 часов.  Проверить, правильно ли синхронизированы устройства после выполнения правила синхронизации, можно так.

1. Запустите центр администрирования Active Directory.
2. Разверните раздел RegisteredDevices в домене, который включается в федерацию.

   ![Центр администрирования Active Directory: зарегистрированные устройства](./media/how-to-connect-device-writeback/devicewriteback5.png)

3. Там будут перечислены зарегистрированные в настоящий момент устройства.

   ![Центр администрирования Active Directory: список зарегистрированных устройств](./media/how-to-connect-device-writeback/devicewriteback6.png)

## <a name="enable-conditional-access"></a>Включить условный доступ
Подробные инструкции о включении этого сценария см. в статье [Настройка локального условного доступа с помощью регистрации устройств в Azure Active Directory](../../active-directory/active-directory-device-registration-on-premises-setup.md).

## <a name="troubleshooting"></a>Устранение неполадок
### <a name="the-writeback-checkbox-is-still-disabled"></a>Флажок обратной записи по-прежнему отключен
Если флажок для обратной записи устройства не включен, несмотря на то, что были выполнены шаги, описанные выше, с помощью описанных ниже действий вы сможете проверить то, что проверяет мастер установки перед включением флажка.

Начнем сначала:

* Лес, где присутствуют устройства, должен иметь схему леса, обновленную до уровня Windows 2012 R2, чтобы присутствовали объект устройства и связанные с ним атрибуты.
* Если мастер установки уже запущен, то изменения не будут обнаружены. В этом случае завершите работу мастера установки и запустите его снова.
* Убедитесь, что учетная запись, указанная в сценарии инициализации, соответствует пользователю, используемому соединителем Active Directory. Чтобы это проверить, выполните следующие действия.
  * В меню "Пуск" щелкните **Служба синхронизации**.
  * Откройте вкладку **Соединители** .
  * Найдите соединитель с типом "Доменные службы Active Directory" и выберите его.
  * В разделе **действия**выберите **свойства**.
  * Выберите **Подключиться к лесу Active Directory**. Убедитесь, что домен и имя пользователя, указанные в этом окне, совпадают с учетной записью, указанной в сценарии.
    ![Учетная запись соединителя в диспетчере службы синхронизации](./media/how-to-connect-device-writeback/connectoraccount.png)

Проверьте конфигурацию Active Directory.

* Убедитесь, что служба регистрации устройств расположена в контексте именования конфигурации ниже (CN=DeviceRegistrationService,CN=Device Registration Services,CN=Device Registration Configuration,CN=Services,CN=Configuration).

![Устранение неполадок, служба регистрации устройств в пространстве имен конфигурации](./media/how-to-connect-device-writeback/troubleshoot1.png)

* Убедитесь, что имеется только один объект конфигурации, выполнив поиск в пространстве имен конфигурации. Если объектов несколько, удалите дубликаты.

![Устранение неполадок, поиск дубликатов объектов](./media/how-to-connect-device-writeback/troubleshoot2.png)

* В объекте службы регистрации устройств убедитесь, что атрибут msDS-DeviceLocation присутствует и для него установлено значение. Найдите указанное расположение и убедитесь, что оно существует с типом объекта msDS-DeviceContainer.

![Устранение неполадок, расположение устройств msDS](./media/how-to-connect-device-writeback/troubleshoot3.png)

![Устранение неполадок, класс объекта зарегистрированных устройств](./media/how-to-connect-device-writeback/troubleshoot4.png)

* Убедитесь, что учетная запись, используемая соединителем Active Directory, имеет необходимые разрешения для контейнера "Зарегистрированные устройства", обнаруженного на предыдущем шаге. Разрешения для контейнера должны быть следующими:

![Устранение неполадок, проверка разрешений в контейнере](./media/how-to-connect-device-writeback/troubleshoot5.png)

* Проверьте, что у учетной записи Active Directory есть разрешения для объекта CN=Device Registration Configuration,CN=Services,CN=Configuration.

![Устранение неполадок, проверка разрешений в конфигурации регистрации устройства](./media/how-to-connect-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Дополнительные сведения
* [Управление рисками с помощью условного доступа](../active-directory-conditional-access-azure-portal.md)
* [Настройка локального условного доступа с помощью Регистрация устройств Azure Active Directory](../../active-directory/active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения об [интеграции локальных удостоверений с Azure Active Directory](whatis-hybrid-identity.md).

