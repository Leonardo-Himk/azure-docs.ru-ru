---
title: Решение VMware для Azure, Клаудсимпле — Настройка vCenter в частном облаке для службы автоматизации Вреализе
description: В этой статье описывается, как настроить сервер VMware vCenter в частном облаке Клаудсимпле в качестве конечной точки для службы автоматизации VMware Вреализе.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: df73acfc469a8b7b5329b61095aefdbd73baafd4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77024846"
---
# <a name="set-up-vcenter-on-your-private-cloud-for-vmware-vrealize-automation"></a>Настройка vCenter в частном облаке для службы автоматизации Вреализе VMware

Вы можете настроить сервер VMware vCenter в частном облаке Клаудсимпле в качестве конечной точки для службы автоматизации VMware Вреализе.

## <a name="before-you-begin"></a>Подготовка к работе

Перед настройкой сервера vCenter выполните следующие задачи:

* Настройте VPN-подключение типа " [сеть — сеть](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway) " между локальной средой и частным облаком.
* [Настройте DNS-перенаправление локальных запросов DNS](on-premises-dns-setup.md) на DNS-серверы частного облака.
* Отправьте [запрос в службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) , чтобы создать административного пользователя IaaS вреализе Automation с набором разрешений, перечисленных в следующей таблице.

| Значение атрибута | Разрешение |
------------ | ------------- |  
| Хранилище данных |  Выделить место <br> Просмотр хранилища данных |
| Кластер хранилища данных | Настройка кластера хранилища данных |
| Папка | Создание папки <br>Удаление папки |
| Global |  Управление настраиваемыми атрибутами<br>Задание пользовательского атрибута |
| Сеть | Назначить сеть |
| Разрешения | Изменение разрешений |
| Ресурс | Назначение виртуальной машины пулу ресурсов<br>Миграция виртуальной машины с питанием<br>Миграция на платформе виртуальной машины |
| Инвентаризация виртуальных машин |  Создать из существующего<br>Создать<br>Переместить<br>Удалить | 
| Взаимодействие виртуальных машин |  Настройка носителя для компакт-дисков<br>Взаимодействие с консолью<br>Подключение устройств<br>Выключение питания<br>Включение<br>Reset<br>Приостановить<br>Средства установки | 
| Конфигурация виртуальной машины |  Добавить существующий диск<br>Добавить новый диск<br>Добавить или удалить<br>Удалить диск<br>Дополнительно<br>Изменение числа ЦП<br>Изменить ресурс<br>Расширение виртуального диска<br>Отслеживание изменений диска<br>Память<br>Изменение параметров устройства<br>Переименовать<br>Задать заметку (версия 5,0 и более поздние)<br>"Настройки"<br>Размещение файл подкачки |
| Подготовка |  Настройка<br>Клонировать шаблон<br>Клонировать виртуальную машину<br>Развертывание шаблона<br>Чтение спецификаций настройки |
| Состояние виртуальной машины | Создать моментальный снимок<br>Удалить моментальный снимок<br>Вернуться к моментальному снимку |

## <a name="install-vrealize-automation-in-your-on-premises-environment"></a>Установка службы автоматизации Вреализе в локальной среде

1. Войдите на серверное устройство Вреализе Automation IaaS в качестве администратора IaaS, который Клаудсимпле поддерживает.
2. Разверните агент vSphere для конечной точки автоматизации Вреализе.
    1. Перейдите по адресу https://*вра-URL*: 5480/Installer, где *вра-URL* — это URL-адрес, используемый для доступа к пользовательскому интерфейсу администрирования автоматизации вреализе.
    2. Щелкните **установщик IaaS** , чтобы скачать установщик.<br>
    Соглашение об именовании для файла установщика — setup_*вра-URL*@5480.exe.
    3. Запустите установщик. На экране приветствия щелкните **Далее**.
    4. Примите условия лицензионного соглашения и нажмите кнопку **Далее**.
    5. Укажите данные для входа, нажмите кнопку **принять сертификат**, а затем нажмите кнопку **Далее**.
    ![учетные данные вра](media/configure-vra-endpoint-login.png)
    6. Выберите **пользовательские установки** и **агенты прокси-сервера** и нажмите кнопку **Далее**.
    ![Тип установки вра](media/configure-vra-endpoint-install-type.png)
    7. Введите данные для входа на сервер IaaS и нажмите кнопку **Далее**. Если вы используете Active Directory, введите имя пользователя в формате **домен \ пользователь** . В противном **user@domain** случае используйте Format.
    ![сведения о входе в вра](media/configure-vra-endpoint-account.png)
    8. В качестве параметров прокси-сервера введите **vSphere** для **типа агента**. Введите имя агента.
    9. Введите полное доменное имя сервера IaaS в полях узел **службы диспетчера** и **узел веб-службы диспетчера моделей** . Нажмите кнопку **тест** , чтобы проверить подключение для каждого из значений полного доменного имени. Если проверка завершается неудачно, измените параметры DNS, чтобы разрешить имя узла сервера IaaS.
    10. Введите имя конечной точки сервера vCenter для частного облака. Запишите имя для использования в дальнейшем в процессе настройки.

        ![прокси-сервер установки вра](media/configure-vra-endpoint-proxy.png)

    11. Щелкните **Далее**.
    12. Нажмите кнопку **Установить**.

## <a name="configure-the-vsphere-agent"></a>Настройка агента vSphere

1. Перейдите по*адресу https://вра-URL*/вкак и войдите в систему как **конфигуратионадмин**.
2. Выберите конечные**точки** >  **инфраструктуры** > **конечных точек.**
3. Выберите **Новый** > **виртуальный** > **vSphere**.
4. Введите имя конечной точки vSphere, указанное в предыдущей процедуре.
5. В поле **адрес**введите URL-адрес частного облака vCenter Server в формате HTTPS://*vCenter-FQDN*/SDK, где *vCenter-FQDN* — это имя сервера vCenter.
6. Введите учетные данные для пользователя с правами администратора IaaS Вреализе Automation, который Клаудсимпле поддерживает.
7. Нажмите кнопку **проверить подключение** , чтобы проверить учетные данные пользователя. Если тест не пройден, проверьте URL-адрес, сведения об учетной записи и [имя конечной точки](#verify-the-endpoint-name) и повторите проверку.
8. После успешного теста нажмите кнопку **ОК** , чтобы создать конечную точку vSphere.
    ![доступ к конфигурации конечной точки вра](media/configure-vra-endpoint-vra-edit.png)

### <a name="verify-the-endpoint-name"></a>Проверка имени конечной точки

Чтобы найти правильное имя конечной точки сервера vCenter, выполните следующие действия.

1. Откройте командную строку на устройстве IaaS.
2. Измените каталог на C:\Program Files (x86) \Вмваре\вкак\ажентс\ажент-наме, где *Agent-Name* — это имя, назначенное конечной точке сервера vCenter.
3. Выполните следующую команду.

```
..\..\Server\DynamicOps.Vrm.VRMencrypt.exe VRMAgent.exe.config get
```

Выходные данные похожи на приведенные ниже. Значением `managementEndpointName` поля является имя конечной точки.

```
managementEndpointName: cslab1pc3-vc
doDeletes: true
```
