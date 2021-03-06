---
title: Настройка параметров портал Azure | Документация Майкрософт
description: Параметры портал Azure по умолчанию можно изменить в соответствии с собственными предпочтениями. Параметры включают время ожидания неактивного сеанса, представление по умолчанию, режим меню, контрастность, тему, уведомления, язык и региональные форматы
services: azure-portal
keywords: параметры, время ожидания, язык, региональный
author: mgblythe
ms.author: mblythe
ms.date: 12/19/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 7bcfdeec832b14eb53c0dab6cb2f53970d85c804
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76310685"
---
# <a name="set-your-azure-portal-preferences"></a>Задание настроек для портала Azure

Можно изменить параметры портал Azure по умолчанию в соответствии с собственными предпочтениями. Каждый из перечисленных ниже параметров можно изменить.

* [Время ожидания неактивного сеанса](#change-the-idle-duration-for-inactive-sign-out)
* [Представление по умолчанию](#choose-your-default-view)
* [Режим меню портала](#choose-a-portal-menu-mode)
* [Тема "цвет и высокая контрастность"](#choose-a-theme)
* [Всплывающие уведомления](#enable-or-disable-pop-up-notifications)
* [Язык и региональный формат](#change-language-and-regional-settings)

## <a name="change-general-portal-settings"></a>Изменение общих параметров портала

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Параметры** в заголовке глобальной страницы.

    ![Снимок экрана: значки верхнего колонтитула глобальной страницы с выделенными параметрами](./media/set-preferences/header-settings.png)

### <a name="change-the-idle-duration-for-inactive-sign-out"></a>Изменение длительности бездействия для неактивного выхода

Настройка времени ожидания при неактивном доступе помогает защитить ресурсы от несанкционированного доступа, если вы забыли защитить рабочую станцию. Когда вы простаивает, вы автоматически выйдете из сеанса портал Azure.

Выберите раскрывающийся список в разделе **выйти, если неактивен**. Выберите продолжительность выхода из сеанса портал Azure в случае бездействия.

   ![Снимок экрана: параметры портала с выделенными неактивными параметрами времени ожидания](./media/set-preferences/inactive-signout-user.png)

Изменение сохраняется автоматически. Если вы простаиваете, ваш сеанс портал Azure выйдет по истечении установленной вами длительности.

Этот параметр также может быть создан администратором на уровне каталога для принудительного применения максимального времени простоя. Если администратор установил параметр времени ожидания на уровне каталога, можно задать срок неактивного выхода. Выберите значение времени, меньшее, чем указано на уровне каталога.

Если администратор включил политику неактивного времени ожидания бездействия, установите флажок **переопределить политику времени ожидания бездействия в каталоге** . Задайте интервал времени, который меньше параметра политики.

   ![Снимок экрана с параметрами портала с переопределением выделенного параметра политики времени ожидания бездействия в каталоге](./media/set-preferences/inactive-signout-override.png)


> [!NOTE]
> Если вы являетесь администратором и хотите применить параметр неактивного времени ожидания для всех пользователей портал Azure, см. статью [Задание времени ожидания бездействия на уровне каталога для пользователей портал Azure](admin-timeout.md)
>

### <a name="choose-your-default-view"></a>Выбор представления по умолчанию 

Вы можете изменить страницу, которая открывается по умолчанию при входе в портал Azure.

   ![Снимок экрана, показывающий параметры портал Azure с выделенным представлением по умолчанию](./media/set-preferences/default-view.png)

Параметр представления по умолчанию определяет, какое портал Azure представление отображается при входе в систему. Вы можете открыть домашнюю страницу Azure по умолчанию или представление панели мониторинга.

* **Домашняя страница** не может быть настроена.  Здесь отображаются ярлыки популярных служб Azure и перечислены наиболее часто используемые ресурсы. Мы также предоставляем полезные ссылки на ресурсы, такие как Microsoft Learn и стратегия Azure.
* Панели мониторинга можно настроить для создания рабочей области, специально предназначенной для вас. Например, можно создать панель мониторинга, которая является задачей проекта, задачи или роли. Если выбрать **панель мониторинга**, то представление по умолчанию будет отправлено на последнюю использовавшуюся панель мониторинга.

### <a name="choose-a-portal-menu-mode"></a>Выбор режима меню портала

Режим по умолчанию для меню портала управляет тем, сколько пространства занимает меню портала на странице.

* Когда меню портала находится в раскрывающемся **режиме,** оно скрыто до тех пор, пока оно не понадобится. Щелкните значок меню, чтобы открыть или закрыть меню.
* Если для меню портала выбран режим **закрепленный** , он всегда виден. Вы можете свернуть меню, чтобы увеличить рабочее пространство. 

### <a name="choose-a-theme"></a>Выберите тему

Выбранная тема влияет на цвет фона и шрифта, которые отображаются в портал Azure. Можно выбрать одну из четырех предустановленных цветовых тем. Выберите каждый эскиз, чтобы найти наиболее подходящий для вас тему.

   ![Снимок экрана, показывающий параметры портал Azure с выделенными темами](./media/set-preferences/theme.png)

Вместо этого можно выбрать одну из наиболее контрастных тем. Параметры высокой контрастности упрощают чтение портал Azure для пользователей с ослабленным зрением и переопределяют все остальные параметры темы. Дополнительные сведения см. [в разделе Включение высокой контрастности или изменение темы](azure-portal-change-theme-high-contrast.md).

### <a name="enable-or-disable-pop-up-notifications"></a>Включение или отключение всплывающих уведомлений

Уведомления — это системные сообщения, связанные с текущим сеансом. Они предоставляют сведения, такие как текущий баланс кредита, время доступности только что созданных ресурсов или подтверждение последнего действия. Если всплывающие уведомления включены, сообщения кратко отображаются в верхнем углу экрана. 

Чтобы включить или отключить всплывающие уведомления, установите или снимите флажок **включить всплывающие уведомления** .

   ![Снимок экрана, показывающий параметры портал Azure с выделенными всплывающими уведомлениями](./media/set-preferences/popup-notifications.png)

Чтобы прочитать все уведомления, полученные во время текущего сеанса, выберите **уведомления** из глобального заголовка.

   ![Снимок экрана, показывающий портал Azure глобальный заголовок с выделенными уведомлениями](./media/set-preferences/read-notifications.png)

Если вы хотите читать уведомления из предыдущих сеансов, найдите события в журнале действий. Дополнительные сведения см. в статье [Просмотр и получение событий журнала действий Azure](/azure/azure-monitor/platform/activity-log-view).

### <a name="settings-under-useful-links"></a>Параметры в разделе Полезные ссылки

Если вы внесли изменения в параметры портал Azure и хотите их удалить, выберите **восстановить параметры по умолчанию**. Все изменения, внесенные в параметры портала, будут потеряны. Этот параметр не влияет на настройки панели мониторинга.

Дополнительные сведения о **экспорте всех параметров** или **удалении всех параметров и личных панелей мониторинга**см. в разделе [Экспорт или удаление параметров пользователя](azure-portal-export-delete-settings.md).

## <a name="change-language-and-regional-settings"></a>Изменение языковых и региональных параметров

Существует два параметра, управляющих отображением текста в портал Azure. **Языковой параметр определяет** язык, который отображается для текста в портал Azure. В **региональном формате** определяются способы отображения дат, времени, чисел и валюты.

Чтобы изменить язык, используемый в портал Azure, используйте раскрывающийся список, чтобы выбрать из списка доступных языков.

Выбор регионального формата изменится, чтобы отображались региональные параметры только для выбранного языка. Чтобы изменить этот автоматический выбор, используйте раскрывающийся список, чтобы выбрать нужный региональный формат.

Например, если выбрать английский язык, а затем выбрать США в качестве регионального формата, то в долларах США будет показана денежная единица. Если в качестве языка выбрать английский, а затем в качестве регионального формата выбрать Европа, то в евро будет показана Валюта.

Нажмите кнопку **Применить** , чтобы обновить язык и региональные параметры формата.

   ![Снимок экрана, показывающий параметры языка и регионального формата](./media/set-preferences/language.png)

>[!NOTE]
>Эти языковые и региональные параметры влияют только на портал Azure. Ссылки на документацию, которые открываются в новой вкладке или окне, будут использовать языковые параметры браузера для определения языка, который будет отображаться.
>

## <a name="next-steps"></a>Дальнейшие шаги

* [Создание и совместное использование настраиваемых панелей мониторинга](azure-portal-dashboards.md)
* [Серия видеоинструкций по работе с порталом Azure](azure-portal-video-series.md)
