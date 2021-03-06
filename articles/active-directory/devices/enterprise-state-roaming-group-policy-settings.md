---
title: Параметры групповая политика и MDM для ESR — Azure Active Directory
description: Параметры управления для Enterprise State Roaming
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdffbc3a140bd13dcd6d352db8c192803d39b03e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78672369"
---
# <a name="group-policy-and-mdm-settings"></a>Параметры групповой политики и управления мобильными устройствами

Используйте эти параметры групповой политики и управления мобильными устройствами (MDM) только на корпоративных устройствах, так как эти политики применяются ко всему устройству пользователя. Применение политики MDM для отключения синхронизации параметров на личном устройстве пользователя может негативно отразиться на использовании этого устройства. Кроме того, политика также влияет на другие учетные записи пользователей на устройстве.

Сотрудники предприятий, которым нужно включить или отключить перемещение данных на личных (неуправляемых) устройствах, могут воспользоваться порталом Azure, а не групповыми политиками или MDM.
В следующих таблицах описаны доступные параметры политики.

> [!NOTE]
> Эта статья относится к устаревшему браузеру на основе HTML Microsoft, запущенному с Windows 10 за июль 2015. Статья не относится к новому браузеру на основе Microsoft Chromium, выпущенному 15 января 2020 г. Дополнительные сведения о поведении синхронизации для нового Microsoft ребра см. в статье [Microsoft ребра Sync](/deployedge/microsoft-edge-enterprise-sync).

## <a name="mdm-settings"></a>Параметры MDM

Параметры политики MDM применяются к Windows 10 и Windows 10 Mobile.  Поддержка Windows 10 Mobile существует только для перемещения на основе учетной записи Майкрософт через учетную запись пользователя OneDrive. Дополнительные сведения о том, какие устройства поддерживаются для синхронизации на основе Azure AD, см. в разделе [устройства и конечные точки](enterprise-state-roaming-windows-settings-reference.md) .

| Имя | Описание |
| --- | --- |
| Разрешить подключение к учетной записи Майкрософт |Позволяет проходить проверку подлинности с использованием учетной записи Майкрософт на устройстве |
| Разрешить синхронизацию моих параметров |Позволяет перемещать параметры Windows и данные приложений. Отключение этой политики приведет к отключению синхронизации и архивации на мобильных устройствах. |

## <a name="group-policy-settings"></a>Параметры групповой политики

Параметры групповой политики применяются к устройствам Windows 10, присоединенным к домену Active Directory. Таблица содержит устаревшие параметры для управления настройками синхронизации. Они не действуют в службе Enterprise State Roaming для Windows 10, что указано в описании с помощью примечания "Не использовать".

Эти параметры расположены в разделе `Computer Configuration > Administrative Templates > Windows Components > Sync your settings`. 

| Имя | Описание |
| --- | --- |
| Учетные записи: заблокировать учетные записи Майкрософт |Этот параметр политики запрещает пользователям добавлять новые учетные записи Майкрософт на этом компьютере |
| Не синхронизировать |Не позволяет перемещать параметры Windows и данные приложений |
| Не синхронизировать персонализацию |Отключает синхронизацию группы "Темы" |
| Не синхронизировать параметры браузера |Отключает синхронизацию группы Internet Explorer |
| Не синхронизировать пароли |Отключает синхронизацию группы "Пароли" |
| Не синхронизировать другие параметры Windows |Отключает синхронизацию группы "Другие параметры Windows" |
| Не синхронизировать персонализацию рабочего стола |Не используйте. Не действует |
| Не синхронизировать лимитные подключения |Отключает перемещение лимитных подключений, таких как 3G на мобильных телефонах |
| Не синхронизировать приложения |Не используйте. Не действует |
| Не синхронизировать параметры приложений |Отключает перемещение данных приложений |
| Не синхронизировать параметры запуска |Не используйте. Не действует |

## <a name="next-steps"></a>Дальнейшие действия

Общие сведения см. в статье [Общие сведения о роуминге состояния предприятия](enterprise-state-roaming-overview.md).
