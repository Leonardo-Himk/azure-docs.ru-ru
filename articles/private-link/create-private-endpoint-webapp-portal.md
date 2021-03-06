---
title: Подключайтесь к веб-приложению с помощью частной конечной точки Azure
description: Подключайтесь к веб-приложению с помощью частной конечной точки Azure
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: article
ms.date: 03/12/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 2f10c7378ae7681b14df6e96b6a6f1adac832d1b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80287821"
---
# <a name="connect-privately-to-a-web-app-using-azure-private-endpoint-preview"></a>Подключайтесь к веб-приложению с использованием частной конечной точки Azure (Предварительная версия)

Частная конечная точка Azure — это фундаментальный Стандартный блок для частной ссылки в Azure. Он позволяет подключаться к веб-приложению в частном порядке.
В этом кратком руководстве вы узнаете, как развернуть веб-приложение с помощью частной конечной точки и подключиться к этому веб-приложению из виртуальной машины.

Дополнительные сведения см. [в статье использование частных конечных точек для веб-приложения Azure][privatenedpointwebapp].

> [!Note]
>Предварительная версия доступна в регионах "Восточная часть США" и "Западная часть США 2" для всех категории премиум v2 веб-приложений Windows и Linux. 

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу https://portal.azure.com.

## <a name="virtual-network-and-virtual-machine"></a>Виртуальная сеть и виртуальная машина

В этом разделе вы создадите виртуальную сеть и подсеть для размещения виртуальной машины, которая используется для доступа к веб-приложению через закрытую конечную точку.

### <a name="create-the-virtual-network"></a>Создание виртуальной сети

В этом разделе вы создадите виртуальную сеть и подсеть.

1. В верхней левой части экрана выберите **создать ресурс** > **Сетевые подключения** > **Виртуальная сеть** или найдите **виртуальную сеть** в поле поиска.

1. В окне **Создание виртуальной сети**введите или выберите эти сведения на вкладке "основы":

   > [!div class="mx-imgBorder"]
   > ![Создать виртуальную сеть][1]

1. Щелкните **"Далее: IP-адреса >"** и введите или выберите следующие сведения:

   > [!div class="mx-imgBorder"]
   >![Настройка IP-адресов][2]

1. В разделе подсеть щелкните **"+ добавить подсеть"** и введите следующие сведения и нажмите кнопку **"Добавить"** .

   > [!div class="mx-imgBorder"]
   >![Добавить подсеть][3]

1. Щелкните **"Проверка и создание"** .

1. После прохождения проверки нажмите кнопку **"создать"** .

### <a name="create-virtual-machine"></a>Создание виртуальной машины

1. В верхней левой части экрана портал Azure выберите **создать ресурс** > **вычислений** > **Виртуальная машина** .

1. В окне Создание виртуальной машины — Основы введите или выберите следующую информацию:

   > [!div class="mx-imgBorder"]
   >![Виртуальная машина Basic][4]

1. Выберите **"Далее: диски".**

   Используйте параметры по умолчанию.

1. Выберите **"Далее: сеть"**, чтобы выбрать следующие сведения:

   > [!div class="mx-imgBorder"]
   >![Сеть][5]

1. Щелкните **"Проверка и создание"** .

1. После передачи сообщения проверки нажмите кнопку **"создать"** .

## <a name="create-your-web-app-and-private-endpoint"></a>Создание веб-приложения и частной конечной точки

В этом разделе вы создадите частное веб-приложение, используя для него закрытую конечную точку.

> [!Note]
>Функция закрытой конечной точки доступна только для SKU Premium версии 2.

### <a name="web-app"></a>Веб-приложение

1. В верхней левой части экрана портал Azure выберите **создать ресурс** > **веб** > **-приложение** .

1. В статье Создание веб-приложения — основы введите или выберите следующие сведения:

   > [!div class="mx-imgBorder"]
   >![Базовое веб-приложение][6]

1. Выберите **"Проверка + создать"** .

1. После передачи сообщения проверки нажмите кнопку **"создать"** .

### <a name="create-the-private-endpoint"></a>Создание частной конечной точки

1. В свойствах веб-приложения выберите **Параметры** > **сеть** и щелкните **"настроить подключения к частным конечным точкам"** .

   > [!div class="mx-imgBorder"]
   >![Сетевые веб-приложения][7]

1. В мастере нажмите кнопку **"+ Добавить"** .

   > [!div class="mx-imgBorder"]
   >![Частная конечная точка веб-приложения][8]

1. Укажите сведения о подписке, виртуальной сети и подсетях и нажмите кнопку **"ОК"** .

   > [!div class="mx-imgBorder"]
   >![Сетевые веб-приложения][9]

1. Просмотр создания частной конечной точки

   > [!div class="mx-imgBorder"]
   >![Просмотр][10]
   >![окончательного представления частной конечной точки][11]

## <a name="connect-to-a-vm-from-the-internet"></a>Подключение к виртуальной машине из Интернета

1. На панели поиска портала введите **myVm** .
1. Нажмите **кнопку Подключить**. После нажатия кнопки подключиться откроется виртуальная машина, выберите **RDP** .

   > [!div class="mx-imgBorder"]
   >![Кнопка RDP][12]

1. Azure создает файл протокол удаленного рабочего стола (. RDP) и загружает его на компьютер после нажатия кнопки **скачать RDP-файл** .

   > [!div class="mx-imgBorder"]
   >![Скачать RDP-файл][13]

1. Откройте файл downloaded.rdp.

- При появлении запроса выберите Подключиться.
- Введите имя пользователя и пароль, указанные при создании виртуальной машины.

> [!Note]
> Вам может потребоваться выбрать дополнительные варианты > использовать другую учетную запись, чтобы указать учетные данные, введенные при создании виртуальной машины.

- Нажмите кнопку "ОК".

1. При входе в систему может появиться предупреждение о сертификате. В таком случае выберите Да или Продолжить.

1. Когда появится рабочий стол виртуальной машины, сверните его, чтобы вернуться на локальный рабочий стол.

## <a name="access-web-app-privately-from-the-vm"></a>Доступ к веб-приложению из виртуальной машины в частном порядке

В этом разделе вы будете подключаться в частном порядке к веб-приложению с помощью частной конечной точки.

1. Получите частный IP-адрес частной конечной точки, на панели поиска введите **Частная ссылка**и выберите частная ссылка.

   > [!div class="mx-imgBorder"]
   >![Приватный канал][14]

1. В частном центре ссылок выберите **частные конечные точки** , чтобы получить список всех частных конечных точек.

   > [!div class="mx-imgBorder"]
   >![Частный центр ссылок][15]

1. Выберите ссылку "Частная конечная точка" для веб-приложения и подсети.

   > [!div class="mx-imgBorder"]
   >![Свойства частной конечной точки][16]

1. Скопируйте частный IP-адрес частной конечной точки и полное доменное имя веб-приложения, в нашем случае webappdemope.azurewebsites.net 10.10.2.4

1. В myVM убедитесь, что веб-приложение недоступно через общедоступный IP-адрес. Откройте браузер и вставьте имя веб-приложения, требуется страница ошибки 403 запрещено.

   > [!div class="mx-imgBorder"]
   >![Запрещено][17]

> [!Important]
> Так как эта функция находится на этапе предварительной версии, необходимо вручную управлять записью DNS.

1. Создайте запись узла, откройте проводник и выберите файл hosts.

   > [!div class="mx-imgBorder"]
   >![Файл hosts][18]

1. Добавьте запись с частным IP-адресом и общедоступным именем веб-приложения, изменив файл hosts с помощью блокнота.

   > [!div class="mx-imgBorder"]
   >![Содержимое узла][19]

1. Сохраните файл.

1. Откройте браузер и введите URL веб-приложения.

   > [!div class="mx-imgBorder"]
   >![Веб-сайт с PE][20]

1. Доступ к веб-приложению осуществляется через частную конечную точку

## <a name="clean-up-resources"></a>Очистка ресурсов

Когда вы закончите использовать закрытую конечную точку, веб-приложение и виртуальную машину, удалите группу ресурсов и все содержащиеся в ней ресурсы.

1. Введите Ready-RG в поле поиска в верхней части портала и выберите Ready-RG в результатах поиска.
1. Выберите Удалить группу ресурсов.
1. Введите Ready-RG, чтобы ввести имя группы ресурсов, и выберите Удалить.

## <a name="next-steps"></a>Дальнейшие шаги

В этом кратком руководстве вы создали виртуальную машину в виртуальной сети, веб-приложении и частной конечной точке. Вы подключились к виртуальной машине из Интернета и безопасно взаимодействовали с веб-приложением, используя закрытую ссылку. Дополнительные сведения о частных конечных точках см. в статье [что такое частная конечная точка Azure][privateendpoint].

<!--Image references-->
[1]: ./media/create-private-endpoint-webapp-portal/createnetwork.png
[2]: ./media/create-private-endpoint-webapp-portal/ipaddresses.png
[3]: ./media/create-private-endpoint-webapp-portal/subnet.png
[4]: ./media/create-private-endpoint-webapp-portal/virtualmachine.png
[5]: ./media/create-private-endpoint-webapp-portal/vmnetwork.png
[6]: ./media/create-private-endpoint-webapp-portal/webapp.png
[7]: ./media/create-private-endpoint-webapp-portal/webappnetworking.png
[8]: ./media/create-private-endpoint-webapp-portal/webapppe.png
[9]: ./media/create-private-endpoint-webapp-portal/webapppenetwork.png
[10]: ./media/create-private-endpoint-webapp-portal/inprogress.png
[11]: ./media/create-private-endpoint-webapp-portal/webapppefinal.png
[12]: ./media/create-private-endpoint-webapp-portal/rdp.png
[13]: ./media/create-private-endpoint-webapp-portal/rdpdownload.png
[14]: ./media/create-private-endpoint-webapp-portal/pl.png
[15]: ./media/create-private-endpoint-webapp-portal/plcenter.png
[16]: ./media/create-private-endpoint-webapp-portal/privateendpointproperties.png
[17]: ./media/create-private-endpoint-webapp-portal/forbidden.png
[18]: ./media/create-private-endpoint-webapp-portal/explorer.png
[19]: ./media/create-private-endpoint-webapp-portal/hosts.png
[20]: ./media/create-private-endpoint-webapp-portal/webappwithpe.png

<!--Links-->
[privatenedpointwebapp]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
[privateendpoint]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview
