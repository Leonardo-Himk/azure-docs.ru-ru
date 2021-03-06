---
title: Включить имя файла
description: включить файл
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d16412e4e35714c840516670f520f77daed1676d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80059964"
---
Созданный сертификат клиента автоматически устанавливается на компьютере, который использовался для его создания. Если вы хотите установить созданный сертификат клиента на другой клиентский компьютер, то его необходимо экспортировать.

1. Чтобы экспортировать сертификат клиента, откройте раздел **Управление сертификатами пользователей**. По умолчанию создаваемые сертификаты клиента хранятся в папке Certificates - Current User\Personal\Certificates. Щелкните правой кнопкой мыши сертификат клиента, который требуется экспортировать, выберите **все задачи**, а затем щелкните **Экспорт** , чтобы открыть **Мастер экспорта сертификатов**.

   ![Экспортировать](./media/vpn-gateway-certificates-export-client-cert-include/export.png)
2. В мастере экспорта сертификатов нажмите кнопку **Далее**, чтобы продолжить.

   ![Дальше](./media/vpn-gateway-certificates-export-client-cert-include/next.png)
3. Выберите **Да, экспортировать закрытый ключ**, а затем нажмите кнопку **Далее**.

   ![Экспорт закрытого ключа](./media/vpn-gateway-certificates-export-client-cert-include/privatekeyexport.png)
4. На странице **Формат экспортируемого файла** оставьте настройки по умолчанию. Убедитесь, что выбран параметр **включить все сертификаты в путь сертификации, если это возможно** . При этом также будут экспортированы данные корневого сертификата, необходимые для успешной аутентификации клиента. Без этих данных аутентификация клиента завершится ошибкой, так как у клиента не будет доверенного корневого сертификата. Затем щелкните **Далее**.

   ![Формат экспортируемого файла](./media/vpn-gateway-certificates-export-client-cert-include/includeallcerts.png)
5. На странице **Безопасность** следует защитить закрытый ключ. Если вы решите использовать пароль, обязательно запишите или запомните пароль, заданный для этого сертификата. Затем щелкните **Далее**.

   ![security](./media/vpn-gateway-certificates-export-client-cert-include/security.png)
6. На странице **Имя экспортируемого файла** нажмите кнопку **Обзор**, чтобы перейти в расположение для экспорта сертификата. В поле **Имя файла**введите имя для файла сертификата. Затем щелкните **Далее**.

   ![Имя экспортируемого файла](./media/vpn-gateway-certificates-export-client-cert-include/filetoexport.png)
7. Нажмите кнопку **Готово**, чтобы выполнить экспорт сертификата.

   ![Готово](./media/vpn-gateway-certificates-export-client-cert-include/finish.png)