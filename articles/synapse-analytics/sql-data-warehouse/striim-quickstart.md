---
title: Быстрый запуск Стриим
description: Быстро приступите к работе со Striim и хранилищем данных SQL Azure.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e963d62df8d2c416726852ee3d46daf1cd22936e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80349943"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Руководство по установке Striim для хранилища данных SQL Azure

В этом кратком руководстве предполагается, что у вас имеется экземпляр хранилища данных SQL.

Найдите Striim в Azure Marketplace и выберите решение "Striim for Data Integration to SQL Data Warehouse (Staged)". 

![Установка Striim][install]

Настройте виртуальную машину Striim, указав заданные свойства, и запишите имя кластера Striim, пароль и пароль администратора.

![Настройка Striim][configure]

После развертывания щелкните имя \<виртуальной машины>-мастерноде в портал Azure, щелкните подключить и скопируйте имя входа с помощью локальной учетной записи виртуальной машины. 

![Подключение Striim к хранилищу данных SQL Azure][connect]

Скачайте файл sqljdbc42.jar по ссылке <https://www.microsoft.com/en-us/download/details.aspx?id=54671> на локальный компьютер. 

Откройте окно командной строки и перейдите в каталог, куда вы скачали JAR-файл JDBC. С помощью SCP передайте JAR-файл на виртуальную машину Striim, получив ее адрес и пароль на портале Azure.

![Копирование JAR-файла на виртуальную машину][copy-jar]

Откройте еще одно окно командной строки или используйте служебную программу ssh, чтобы подключиться по протоколу SSH к кластеру Striim.

![Установка SSH-подключения к кластеру][ssh]

Выполните следующие команды, чтобы переместить JAR-файл JDBC в каталог lib Striim, после чего запустите и остановите сервер.

   1. sudo su
   2. cd /tmp
   3. mv sqljdbc42.jar /opt/striim/lib
   4. systemctl stop striim-node
   5. systemctl stop striim-dbms
   6. systemctl start striim-dbms
   7. systemctl start striim-node

![Запуск кластера Striim][start-striim]

Теперь откройте свой любимый браузер и перейдите к \<DNS-имени>:9080

![Переход на экран входа][navigate]

Войдите с помощью имени пользователя и пароля, которые вы настроили на портале Azure. Выберите предпочитаемый мастер, чтобы приступить к работе, или перейдите на страницу приложений, чтобы начать использовать пользовательский интерфейс для перетаскивания.

![Вход с помощью учетных данных сервера][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
