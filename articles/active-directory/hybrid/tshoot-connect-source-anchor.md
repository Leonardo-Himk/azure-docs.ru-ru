---
title: 'Azure AD Connect: устранение проблем привязки к источнику во время установки | Документация Майкрософт'
description: В этом разделе содержатся инструкции по устранению проблем с привязкой к источнику во время установки.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: fac33a01afc2efc1ab06c4783c11f7a089bb6208
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2020
ms.locfileid: "62114161"
---
# <a name="troubleshooting-source-anchor-issues-during-installation"></a>Устранение проблем привязки к источнику во время установки
В этой статье описываются различные проблемы, связанные с привязкой к источнику, которые могут возникнуть во время установки, и способы их устранения.

## <a name="invalid-source-anchor-in-azure-active-directory"></a>Недопустимая привязка к источнику в Azure Active Directory

### <a name="custom-installation"></a>Выборочная установка Azure AD Connect

Во время пользовательской установки Azure AD Connect считывает политику привязки источника из Azure Active Directory. Если политика существует в Azure Active Directory, Azure AD Connect применяет ту же политику, если она не переопределена клиентом. Мастер информирует, какой атрибут был прочитан. Кроме того, мастер выдает предупреждение при попытке переопределить политику привязки к источнику.

Во время этой операции чтения возможно, что политика привязки к источнику в Azure Active Directory непредвиденна. В этом случае Azure AD Connect не знает, какую привязку источника использовать и требуется переопределять вручную.</br>
![unexpected](media/tshoot-connect-source-anchor/source1.png)

Чтобы устранить эту проблему, можно вручную переопределить привязку к источнику, выбрав определенный атрибут. Продолжайте работу с этим параметром, только если вы уверены, какой именно атрибут следует выбрать. Если вы не уверены, обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/contactus/) за инструкциями. Изменение политики привязки к источнику может привести к нарушению связи между локальными пользователями и связанными ресурсами Azure.</br>
![unexpected](media/tshoot-connect-source-anchor/source2.png)

### <a name="express-installation"></a>Экспресс-установка
Во время быстрой установки Azure AD Connect считывает политику привязки источника из Azure Active Directory. Если политика существует в Azure Active Directory, Azure AD Connect применяет ту же политику. Возможность переопределения вручную не предусмотрена.

Во время этой операции чтения возможно, что политика привязки к источнику в Azure Active Directory непредвиденна. В этом случае Azure AD Connect не знает, что должна быть привязкой к источнику.</br>
![unexpected](media/tshoot-connect-source-anchor/source3.png)

Чтобы устранить эту проблему, необходимо переустановить с помощью пользовательского режима и вручную переопределить привязку источника, выбрав определенный атрибут. Продолжайте работу с этим параметром, только если вы уверены, какой именно атрибут следует выбрать. Если вы не уверены, обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/contactus/) за инструкциями. Изменение политики привязки к источнику может привести к нарушению связи между локальными пользователями и связанными ресурсами Azure.

### <a name="invalid-source-anchor-in-sync-engine"></a>Недопустимая привязка к источнику в модуле синхронизации
Во время установки возможно Azure AD Connect пытается настроить модуль синхронизации с использованием недопустимой привязки к источнику. Эта операция, скорее всего, вызывает проблемы с продуктом, и установка Azure AD Connect завершится ошибкой. Если вы запустили эту ошибку, обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/contactus/) .</br>
![unexpected](media/tshoot-connect-source-anchor/source4.png)


## <a name="next-steps"></a>Следующие шаги
Дополнительные сведения об [интеграции локальных удостоверений с Azure Active Directory](whatis-hybrid-identity.md).