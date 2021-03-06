---
title: JIT-доступ к виртуальным машинам в Центре безопасности Azure | Документация Майкрософт
description: В этом документе описано, как JIT-доступ в Центре безопасности Azure помогает управлять доступом к виртуальным машинам Azure.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: cc4e267c6912b8938db1ba5497a27f9c0026bd79
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80887339"
---
# <a name="secure-your-management-ports-with-just-in-time-access"></a>Защита портов управления с помощью JIT-доступа

Если вы находитесь в ценовой категории "Стандартный" центра безопасности (см. раздел [цены](/azure/security-center/security-center-pricing)), вы можете заблокировать входящий трафик на виртуальные машины Azure с помощью JIT-доступа к виртуальной машине. Это снижает вероятность атак, обеспечивая при необходимости простой доступ к виртуальным машинам.

> [!NOTE]
> Механизм JIT-доступа Центра безопасности сейчас поддерживается только для виртуальных машин, развернутых с помощью Azure Resource Manager. Дополнительные сведения о классической модели развертывания и модели развертывания Resource Manager см. в разделе [Модель развертывания Azure Resource Manager и классическая модель развертывания](../azure-resource-manager/management/deployment-models.md).

[!INCLUDE [security-center-jit-description](../../includes/security-center-jit-description.md)]

## <a name="configure-jit-on-a-vm"></a>Настройка JIT на виртуальной машине

Существует три способа настройки политики JIT на виртуальной машине.

- [Настройка JIT-доступа в центре безопасности Azure](#jit-asc)
- [Настройка JIT-доступа на странице виртуальной машины Azure](#jit-vm)
- [Программная настройка политики JIT на виртуальной машине](#jit-program)

## <a name="configure-jit-in-azure-security-center"></a>Настройка JIT в центре безопасности Azure

В центре безопасности можно настроить политику JIT и запросить доступ к виртуальной машине с помощью политики JIT.

### <a name="configure-jit-access-on-a-vm-in-security-center"></a>Настройка JIT-доступа на виртуальной машине в центре безопасности<a name="jit-asc"></a>

1. Откройте панель мониторинга **центра безопасности**.

1. На панели слева выберите **JIT-доступ к виртуальной машине**.

    ![Плитка "JIT-доступ к виртуальной машине"](./media/security-center-just-in-time/just-in-time.png)

    Откроется окно " **JIT-доступ к виртуальной машине** ", в котором отображаются сведения о состоянии виртуальных машин.

    - **Настроенные** — виртуальные машины, для которых была настроена поддержка JIT-доступа. Указываются сведения за последнюю неделю, которые включают число одобренных запросов, дату и время последнего доступа и последнего пользователя для каждой виртуальной машины.
    - **Рекомендуется** — виртуальные машины, которые могут поддерживать JIT-доступ к виртуальным машинам, но не настроены для. Мы рекомендуем включить JIT-доступ для всех этих виртуальных машин.
    - **Нерекомендуемые** — причины, по которым виртуальная машина может попасть в эту группу, включают следующие:
      - Отсутствует группа безопасности сети — для решения JIT необходимо наличие группы безопасности сети.
      - Классическая виртуальная машина — JIT-доступ с использованием Центра безопасности сейчас поддерживается только для виртуальных машин, развернутых с помощью Azure Resource Manager. JIT-доступ для классической модели развертывания не поддерживается. 
      - Другие — виртуальная машина находится в этой категории, если JIT-решение отключено в политике безопасности подписки или группы ресурсов или если на виртуальной машине отсутствует общедоступный IP-адрес и отсутствует NSG.

1. Перейдите на вкладку **Рекомендуется**.

1. В разделе **Виртуальная машина**щелкните Виртуальные машины, которые необходимо включить. При этом устанавливается флажок рядом с виртуальной машиной.

      ![Включение JIT-доступа](./media/security-center-just-in-time/enable-just-in-time.png)

1. Щелкните **включить JIT на виртуальных машинах**. Откроется панель, в которой отображаются порты по умолчанию, рекомендованные центром безопасности Azure.
    - 22 — SSH;
    - 3389 — RDP;
    - 5985 — WinRM; 
    - 5986 — WinRM.
1. При необходимости можно добавить в список пользовательские порты:

      1. Нажмите кнопку **Добавить**. Откроется окно **Добавление конфигурации порта** .
      1. Для каждого порта, выбранного для настройки (по умолчанию и настраиваемый), можно настроить следующие параметры.
            - **Тип протокола** — указывает, какой протокол разрешается для этого порта после утверждения запроса;
            - **Allowed source IP addresses** (Разрешенные IP-адреса источника) — диапазоны IP-адресов, с которых разрешается доступ к этому порту после утверждения запроса;
            - **Максимальное время запроса** — максимальный интервал времени, в течение которого может быть открыт этот порт.

     1. Нажмите кнопку **ОК**.

1. Нажмите кнопку **Сохранить**.

> [!NOTE]
>Если для виртуальной машины включен JIT-доступ к виртуальной машине, центр безопасности Azure создает правила "запретить весь входящий трафик" для выбранных портов в связанных группах безопасности сети и брандмауэре Azure. Если для выбранных портов были созданы другие правила, то существующие правила имеют приоритет над новым правилом "запретить весь входящий трафик". Если на выбранных портах нет существующих правил, то новые правила "запретить весь входящий трафик" будут иметь наивысший приоритет в группах безопасности сети и брандмауэре Azure.


## <a name="request-jit-access-via-security-center"></a>Запрос JIT-доступа через центр безопасности

Чтобы запросить доступ к виртуальной машине через центр безопасности, выполните следующие действия.

1. В колонке **JIT-доступ к виртуальной машине** перейдите на вкладку **Настроенные**.

1. В разделе **Виртуальная машина**щелкните Виртуальные машины, для которых требуется запросить доступ. В этом случае рядом с виртуальной машиной помещается галочка.

    - Значок в столбце **сведения о соединении** указывает, включен ли JIT в NSG или FW. Если он включен для обоих типов, отображается только значок брандмауэра.

    - Столбец **сведения о подключении** содержит сведения, необходимые для подключения виртуальной машины и ее открытых портов.

      ![Запрос JIT-доступа](./media/security-center-just-in-time/request-just-in-time-access.png)

1. Щелкните **запросить доступ**. Откроется окно **запрос доступа** .

      ![Сведения о JIT](./media/security-center-just-in-time/just-in-time-details.png)

1. В колонке **Запрос на доступ** укажите для каждой виртуальной машины порты, которые нужно открыть, IP-адреса источников для этих портов и временной интервал, в течение которого будет открыт порт. Можно будет запросить доступ только к портам, настроенным в политике JIT. Максимально допустимое время для каждого порта определяется в политике JIT.

1. Щелкните **открыть порты**.

> [!NOTE]
> Если пользователь запрашивает доступ через прокси-сервер, то параметр **Мой IP-адрес** может не работать. Возможно, потребуется определить полный диапазон IP-адресов организации.



## <a name="edit-a-jit-access-policy-via-security-center"></a>Изменение политики JIT-доступа с помощью центра безопасности

Вы можете изменить существующую политику JIT-доступа для виртуальной машины, добавив и настроив новый защищенный порт для этой виртуальной машины или изменив любой другой параметр уже настроенного защищенного порта.

Чтобы изменить существующую политику JIT для виртуальной машины, сделайте следующее.

1. На вкладке **Настроенные** в разделе **Виртуальные машины** выберите ту виртуальную машину, для которой нужно добавить порт, щелкнув многоточие в строке этой виртуальной машины. 

1. Выберите команду **Изменить**.

1. В колонке **Настройка JIT-доступа к виртуальной машине** можно изменить существующие параметры уже защищенного порта или добавить новый порт. 
  ![JIT-доступ к виртуальной машине](./media/security-center-just-in-time/edit-policy.png)



## <a name="audit-jit-access-activity-in-security-center"></a>Аудит действий JIT-доступа в центре безопасности

Для просмотра действий виртуальной машины можно воспользоваться поиском по журналам. Для просмотра журналов выполните следующие действия:

1. В колонке **JIT-доступ к виртуальной машине** перейдите на вкладку **Настроенные**.
2. В разделе **виртуальные машины**выберите виртуальную машину для просмотра сведений, щелкнув три точки в строке для этой виртуальной машины и выбрав в меню пункт **Журнал действий** . Откроется **Журнал действий** .

   ![Выбор журнала действий](./media/security-center-just-in-time/select-activity-log.png)

   В колонке **Журнал действий** отображается отфильтрованный список предыдущих операций для этой виртуальной машины, а также дата, время и подписка.

Чтобы скачать сведения журналов, воспользуйтесь ссылкой **Щелкните здесь, чтобы загрузить все элементы в формате CSV**.

Измените фильтры и нажмите кнопку **Применить** , чтобы создать поиск и журнал.



## <a name="configure-jit-access-from-an-azure-vms-page"></a>Настройка JIT-доступа на странице виртуальной машины Azure<a name="jit-vm"></a>

Для удобства вы можете подключиться к виртуальной машине с помощью JIT-компилятора непосредственно из страницы виртуальной машины в центре безопасности.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-page"></a>Настройка JIT-доступа на виртуальной машине с помощью страницы виртуальной машины Azure

Чтобы упростить развертывание доступа JIT на виртуальных машинах, вы можете установить виртуальную машину и разрешить JIT-доступ только непосредственно из виртуальной машины.

1. В [портал Azure](https://ms.portal.azure.com)найдите и выберите **виртуальные машины**. 
2. Выберите виртуальную машину, для которой требуется ограничить доступ.
3. В меню выберите пункт **Конфигурация**.
4. В разделе **JIT-доступ**выберите **включить JIT**. 

Это обеспечивает JIT-доступ для виртуальной машины с помощью следующих параметров:

- Серверы Windows:
    - RDP-порт 3389;
    - Три часа максимального разрешенного доступа
    - Для разрешенных IP-адресов источника задано значение Any
- Серверы Linux:
    - SSH-порт 22;
    - Три часа максимального разрешенного доступа
    - Для разрешенных IP-адресов источника задано значение Any
     
Если в виртуальной машине уже включена функция JIT, вы увидите это при переходе на страницу конфигурации. Кроме того, вы можете перейти по этой ссылке, чтобы открыть политику в центре безопасности Azure для просмотра и изменения параметров.

![Настройка JIT в виртуальной машине](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-an-azure-vms-page"></a>Запрос JIT-доступа к виртуальной машине через страницу виртуальной машины Azure

На портале Azure при попытке подключиться к виртуальной машине Azure проверяет, настроена ли политика JIT-доступа для выбранной виртуальной машины.

- Если на виртуальной машине настроена политика JIT, можно щелкнуть **запросить доступ** , чтобы предоставить доступ в соответствии с политикой JIT, заданной для виртуальной машины. 

  >![JIT-запрос](./media/security-center-just-in-time/jit-request.png)

  Доступ запрашивается со следующими параметрами по умолчанию:

  - **Исходный IP-адрес**: "Any" (*) (невозможно изменить)
  - **диапазон времени**: три часа (невозможно изменить) <!--Isn't this set in the policy-->
  - **номер порта** Порт RDP 3389 для Windows или порт 22 для Linux (можно изменить)

    > [!NOTE]
    > После утверждения запроса для виртуальной машины, защищенной с помощью брандмауэра Azure, центр безопасности предоставляет пользователю правильные сведения о подключении (сопоставление портов из таблицы ДНАТ), используемые для подключения к виртуальной машине.

- Если на виртуальной машине не настроен JIT-компилятор, вам будет предложено настроить на ней политику JIT.

  ![Сообщение о JIT-доступе](./media/security-center-just-in-time/jit-prompt.png)

## <a name="configure-a-jit-policy-on-a-vm-programmatically"></a>Программная настройка политики JIT на виртуальной машине<a name="jit-program"></a>

Вы можете настраивать и использовать JIT-доступ через интерфейсы REST API или с помощью PowerShell.

### <a name="jit-vm-access-via-rest-apis"></a>JIT-доступ к виртуальной машине через API-интерфейсы

Функцию JIT-доступа к виртуальной машине можно использовать через API Центра безопасности Azure. Этот API позволяет получать сведения о настроенных виртуальных машинах, добавлять новые виртуальные машины, запрашивать доступ к ним и выполнять другие действия. Дополнительные сведения о REST API для JIT-доступа см. в статье [Jit Network Access Policies](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies) (Политики JIT-доступа к сети).

### <a name="jit-vm-access-via-powershell"></a>JIT-доступ к виртуальной машине через PowerShell

Чтобы применять решение JIT-доступа к виртуальной машине через PowerShell, можно использовать официальные командлеты PowerShell Центра безопасности Azure, в частности `Set-AzJitNetworkAccessPolicy`.

В следующем примере задается политика JIT-доступа для конкретной виртуальной машины со следующими параметрами.

1.    Закрытие портов 22 и 3389.

2.    Задание максимального временного окна в 3 часа для каждого порта, чтобы они были открыты для каждого утвержденного запроса.
3.    Предоставление пользователю, который запрашивает доступ, разрешения на управление исходными IP-адресами, и предоставление пользователю возможности установить сеанс после утвержденного запроса на JIT-доступ.

В PowerShell выполните следующее.

1.    Присвойте виртуальной машине переменную, которая содержит политику JIT-доступа к этой виртуальной машине:

        $JitPolicy = (@ {ID = "/Субскриптионс/субскриптионид/ресаурцеграупс/ресаурцеграуп/провидерс/Микрософт.компуте/виртуалмачинес/вмнаме" Ports = (@ {Number = 22;        Protocol = "\*";        Алловедсаурцеаддресспрефикс = @ ("\*");        Максрекуестакцессдуратион = "PT3H"}, @ {Number = 3389;        Protocol = "\*";        Алловедсаурцеаддресспрефикс = @ ("\*");        Максрекуестакцессдуратион = "PT3H"})})

2.    Поместите политику JIT-доступа к виртуальной машине в массив:
    
        $JitPolicyArr = @ ($JitPolicy)

3.    Настройте на выбранной виртуальной машине политику JIT-доступа:
    
        Set-Азжитнетворкакцессполици-Kind "базовый"-Location "расположение"-Name "default"-ResourceGroupName "RESOURCEGROUP"-VirtualMachine $JitPolicyArr 

### <a name="request-access-to-a-vm-via-powershell"></a>Запрос доступа к виртуальной машине с помощью PowerShell

В следующем примере приведен запрос на JIT-доступ к выбранной виртуальной машине, где запрашивается открытие порта 22 для определенного IP-адреса и на определенное время.

В PowerShell выполните следующее.
1.    Настройка свойств запроса доступа к виртуальной машине.

        $JitPolicyVm 1 = (@ {ID = "/Субскриптионид/ресаурцеграупс/ресаурцеграуп/провидерс/Микрософт.компуте/виртуалмачинес/вмнаме" Ports = (@ {Number = 22;      Ендтимеутк = "2018-09-17T17:00:00.3658798 Z";      Алловедсаурцеаддресспрефикс = @ ("IPV4ADDRESS")})})
2.    Вставка параметров запроса доступа к виртуальной машине в массив.

        $JitPolicyArr = @ ($JitPolicyVm 1)
3.    Отправка запроса на доступ (с использованием идентификатора ресурса, полученного на шаге 1).

        Start-Азжитнетворкакцессполици-ResourceId "/Субскриптионс/субскриптионид/ресаурцеграупс/ресаурцеграуп/провидерс/Микрософт.секурити/локатионс/локатион/житнетворкакцессполиЦиес/дефаулт" — VirtualMachine $JitPolicyArr

Дополнительные сведения см. в [документации по командлетам PowerShell](https://docs.microsoft.com/powershell/scripting/developer/cmdlet/cmdlet-overview).


## <a name="automatic-cleanup-of-redundant-jit-rules"></a>Автоматическая очистка избыточных правил JIT 

При каждом обновлении политики JIT автоматически запускается средство очистки для проверки допустимости всего набора правил. Средство ищет несоответствия между правилами в политике и правилами в NSG. Если средство очистки обнаруживает несовпадение, оно определяет причину и, когда это можно сделать, удаляет встроенные правила, которые больше не нужны. Средство удаления никогда не удаляет созданные вами правила.

Примеры сценариев, когда средство удаления может удалить встроенное правило:

- Если два правила с одинаковыми определениями имеют более высокий приоритет, чем другое (то есть правило меньшего приоритета никогда не будет использоваться);
- Если описание правила включает имя виртуальной машины, которая не соответствует IP-адресу назначения в правиле 


## <a name="next-steps"></a>Дальнейшие шаги

Из этой статьи вы узнали, как управлять доступом к виртуальным машинам Azure с помощью JIT-доступа в Центре безопасности.

Дополнительные сведения о Центре безопасности см. в следующих статьях:

- Модуль Microsoft Learn [защищает серверы и виртуальные машины от атак методом подбора и вредоносных программ с помощью центра безопасности Azure](https://docs.microsoft.com/learn/modules/secure-vms-with-azure-security-center/) .
- [Настройка политик безопасности](tutorial-security-policy.md) . Узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
- [Управление рекомендациями по безопасности](security-center-recommendations.md) — Узнайте, как рекомендации помогут защитить ресурсы Azure.
- [Наблюдение за работоспособностью системы безопасности](security-center-monitoring.md) — сведения об отслеживании работоспособности ресурсов Azure.
