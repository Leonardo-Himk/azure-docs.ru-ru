---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: fd1a7f133c5719873133554fc2292e94e6fe26a4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "75980330"
---
1. Войдите на [портал Azure](https://portal.azure.com) > **Службы восстановления**.
2. Последовательно выберите **Создать ресурс** > **Monitoring + Management** (Мониторинг и управление) > **Backup and Site Recovery**.
3. В поле **Имя**укажите понятное имя для идентификации хранилища. Если у вас есть несколько подписок, выберите нужную.
4. [Создайте новую группу ресурсов](../articles/azure-resource-manager/templates/deploy-portal.md) или выберите существующую. Укажите регион Azure. 
5. Чтобы быстро получить доступ к хранилищу на панели мониторинга, щелкните **Закрепить на панели мониторинга** > **Создать**.

   ![Новое хранилище](./media/site-recovery-create-vault/new-vault-settings.png)

   Новое хранилище появится в колонке **Панель мониторинга** > **Все ресурсы** и на основной странице **Хранилища служб восстановления**.
