---
title: Подключение корневого или вершине домена к существующей передней дверце — портал Azure
description: Узнайте, как подключить корневой или вершине-домен к существующей передней дверце с помощью портал Azure.
services: front-door
author: sharad4u
ms.service: frontdoor
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: 4b74338f22a82d76ef13126ee0862b841bd89a99
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80878890"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Подключение корневого или вершине домена на передней дверце
Передняя дверца Azure использует записи CNAME для проверки владения доменом для адаптации личных доменов. Кроме того, передняя дверца не предоставляет внешний IP-адрес, связанный с профилем передней дверцы, и поэтому вы не можете сопоставить домен вершине с IP-адресом, если цель — подключить ее к передней дверце Azure.

Протокол DNS предотвращает назначение записей CNAME на вершине зоны. Например, если домен имеет значение `contoso.com`; записи CNAME можно создавать для `somelabel.contoso.com`; но вы не можете создать CNAME `contoso.com` для себя. Это ограничение представляет собой проблему для владельцев приложений, имеющих приложения с балансировкой нагрузки на основе передней дверцы Azure. Поскольку для использования профиля передней дверцы требуется создать запись CNAME, невозможно указать профиль передней дверцы из зоны вершине.

Эта проблема решена с помощью записей псевдонима в Azure DNS. В отличие от записей CNAME, записи псевдонимов создаются в зоне вершине, и владельцы приложения могут использовать его для указания записи вершине зоны в профиль передней дверцы с общедоступными конечными точками. Владельцы приложений указывают на тот же профиль передней дверцы, который используется для любого другого домена в своей зоне DNS. Например, `contoso.com` и `www.contoso.com` может указывать на один и тот же профиль передней дверцы. 

Для сопоставления вершине или корневого домена с профилем передней дверцы, по сути, требуется сведение CNAME или отслеживание DNS, то есть механизм, в котором поставщик DNS рекурсивно разрешает запись CNAME, пока не достигнет IP-адреса. Эта функция поддерживается Azure DNS для конечных точек передней дверцы. 

> [!NOTE]
> Кроме того, существуют другие поставщики DNS, поддерживающие сведение CNAME или отслеживание DNS, однако в передней дверце Azure рекомендуется использовать Azure DNS для своих клиентов для размещения своих доменов.

Вы можете использовать портал Azure для подключения домена вершине на передней дверце и включения протокола HTTPS, связав его с сертификатом для завершения TLS. Домены вершине также называются корневыми или доменными доменами с атрибутом naked.

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание записи псевдонима, указывающей на профиль передней дверцы
> * Добавление корневого домена к передней дверце
> * Настройка HTTPS в корневом домене

> [!NOTE]
> Для работы с этим руководством требуется, чтобы профиль передней дверцы был создан. Ознакомьтесь с другими учебниками, такими как [Краткое руководство. Создание передней дверцы](./quickstart-create-front-door.md) или [Создание передней дверцы с перенаправлением HTTP в HTTPS](./front-door-how-to-redirect-https.md) для начала работы.

## <a name="create-an-alias-record-for-zone-apex"></a>Создание записи псевдонима для зоны вершине

1. Откройте конфигурацию **Azure DNS** для домена, который необходимо подключить.
2. Создайте или измените запись для зоны вершине.
3. Выберите **тип** _записи в качестве записи_ , а затем выберите _Да_ для **набора записей псевдонима**. Для **типа псевдонима** необходимо задать значение _Azure Resource_.
4. Выберите подписку Azure, в которой размещается профиль передней дверцы, а затем выберите ресурс "Передняя дверь" из раскрывающегося списка **ресурсов Azure** .
5. Нажмите кнопку **ОК** , чтобы отправить изменения.

    ![Запись псевдонима для вершине зоны](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. На приведенном выше шаге будет создана запись зоны вершине, указывающая на ресурс передней дверцы, а также сопоставление записи CNAME "афдверифи" `afdverify.contosonews.com`(example `afdverify.<name>.azurefd.net` -), которое будет использоваться для подключения домена в профиле передней дверцы.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Подключение пользовательского домена на передней дверце

1. На вкладке "конструктор передней дверцы" щелкните значок "+" в разделе "интерфейсные узлы", чтобы добавить новый личный домен.
2. Введите имя корневого или вершине домена в поле имя пользовательского узла, например `contosonews.com`.
3. После проверки сопоставления CNAME из домена с доменом к передней дверце нажмите кнопку **Добавить** , чтобы добавить личный домен.
4. Нажмите кнопку **сохранить** , чтобы отправить изменения.

![Меню личных доменов](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>Включение протокола HTTPS в пользовательском домене

1. Щелкните личный домен, который был добавлен, и в разделе " **пользовательский домен HTTPS**" измените состояние на " **включено**".
2. Выберите **тип управления сертификатами** , чтобы _использовать мой собственный сертификат_.

> [!WARNING]
> Тип управления управляемыми сертификатами передней дверцы в настоящее время не поддерживается для вершине или корневых доменов. Единственный параметр, доступный для включения HTTPS в вершине или корневом домене для передней дверцы, использует собственный пользовательский сертификат TLS/SSL, размещенный на Azure Key Vault.

3. Убедитесь, что вы настроили необходимые разрешения для доступа к хранилищу ключей с помощью интерфейса "Передняя дверца", как указано в пользовательском интерфейсе, прежде чем переходить к следующему шагу.
4. Выберите **учетную запись KEY Vault** из текущей подписки, а затем выберите соответствующую **секретную** и **секретную версию** , чтобы связать его с нужным сертификатом.
5. Щелкните **Обновить** , чтобы сохранить выбор, а затем нажмите кнопку **сохранить**.
6. Нажмите кнопку **Обновить** через пару минут, а затем снова щелкните личный домен, чтобы увидеть ход подготовки сертификата. 

> [!WARNING]
> Убедитесь, что вы создали соответствующие правила маршрутизации для домена вершине или добавили домен в существующие правила маршрутизации.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [создании Front Door](quickstart-create-front-door.md).
- Дополнительные сведения о том, [как работает Front Door](front-door-routing-architecture.md).