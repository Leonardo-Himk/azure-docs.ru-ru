---
title: Автоматизация развертывания виртуальной машины в Amazon Web Services
description: В этой статье показано, как автоматизировать создание виртуальной машины Amazon Web Service с помощью службы автоматизации Azure.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 52a887d2d934aa2f7e13f0c2fdb4332066aee0e7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81604827"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Сценарий службы автоматизации Azure: подготовка виртуальной машины AWS
В этой статье вы узнаете, как с помощью службы автоматизации Azure подготовить виртуальную машину в подписке Amazon Web Service (AWS) и присвоить виртуальной машине конкретное имя, которое в AWS называется "тегом" виртуальной машины.

## <a name="prerequisites"></a>Предварительные условия
Необходимо иметь учетную запись службы автоматизации Azure и подписку Amazon Web Services (AWS). Дополнительные сведения о настройке учетной записи службы автоматизации Azure и об указании в ней учетных данных подписки AWS см. в статье [Проверка подлинности модулей Runbook с помощью Amazon Web Services](automation-config-aws-account.md). Перед продолжением этой учетной записи необходимо создать или обновить учетные данные подписки AWS, так как вы ссылаетесь на эту учетную запись в следующих разделах.

## <a name="deploy-amazon-web-services-powershell-module"></a>Развертывание модуля PowerShell для Amazon Web Services
Runbook подготовки виртуальной машины использует модуль PowerShell AWS для выполнения своей работы. Чтобы добавить модуль в учетную запись службы автоматизации, настроенную с учетными данными подписки AWS, выполните следующие действия.  

1. Откройте браузер и перейдите к [коллекции PowerShell](https://www.powershellgallery.com/packages/AWSPowerShell/), а затем нажмите кнопку **Deploy to Azure Automation** (Развернуть в службе автоматизации Azure).<br><br> ![Импорт модуля AWS PS](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Вы перейдете на страницу входа в Azure. После проверки подлинности откроется следующая страница портала Azure.<br><br> ![Страница импорта модуля](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Выберите учетную запись службы автоматизации для использования и нажмите кнопку **ОК** , чтобы запустить развертывание.

   > [!NOTE]
   > Когда служба автоматизации Azure импортирует модуль PowerShell, она извлекает командлеты. Действия не отображаются, пока Автоматизация не будет полностью завершена импортом модуля и извлечением командлетов. Это может занять несколько минут.  
   > <br>

1. На портале Azure выберите свою учетную запись службы автоматизации.
2. Щелкните плитку **активы** и в области ресурсы выберите **модули**.
3. На странице Модули вы увидите в списке модуль **AWSPowerShell**.

## <a name="create-aws-deploy-vm-runbook"></a>Создание модуля Runbook для развертывания виртуальной машины в AWS
После развертывания модуля AWS PowerShell можно создать модуль Runbook для автоматизации подготовки виртуальной машины в AWS с помощью сценария PowerShell. Ниже показано, как использовать собственный сценарий PowerShell в службе автоматизации Azure.  

> [!NOTE]
> Дополнительные параметры и сведения, касающиеся этого сценария, можно найти в [коллекции PowerShell](https://www.powershellgallery.com/packages/New-AwsVM/).
> 

1. Скачайте сценарий PowerShell New-AwsVM из коллекция PowerShell, открыв сеанс PowerShell и введя следующую команду:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. В портал Azure откройте учетную запись службы автоматизации и выберите **модули Runbook** в разделе **Автоматизация процессов**.  
3. На странице Модули Runbook выберите **Добавить Runbook**.
4. На панели добавить Runbook выберите **Быстрое создание** , чтобы создать новый Runbook.
5. В области свойства Runbook введите имя модуля Runbook.
6. В раскрывающемся списке **тип Runbook** выберите **PowerShell**и нажмите кнопку **создать**.<br><br> ![Панель создания модуля Runbook](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. При открытии страницы "Изменение сценария PowerShell для модуля Runbook" скопируйте и вставьте сценарий PowerShell в область создания модуля Runbook.<br><br> ![Сценарий PowerShell для модуля Runbook](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > При работе с примером сценария PowerShell имейте в виду следующее:
    > 
    > * Модуль Runbook содержит несколько параметров по умолчанию. Проверьте все значения по умолчанию и при необходимости обновите их.
    > * Если вы сохранили учетные данные AWS в качестве ресурса учетных данных с `AWScred`именем, отличающимся от, необходимо обновить скрипт в строке 57 в соответствии с соответствующим образом.  
    > * При работе с командами AWS в PowerShell, особенно в этом примере модуля Runbook, необходимо указывать регион AWS. В противном случае командлеты завершатся с ошибкой. Дополнительные сведения на сайте AWS см. в разделе [Specify AWS Region](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) (Указание региона AWS) документации "AWS Tools for Windows PowerShell" (Инструменты AWS для Windows PowerShell).  
    >

7. Чтобы получить список имен образов из подписки AWS, запустите PowerShell ISE и импортируйте модуль AWS PowerShell. Аутентификация в AWS путем `Get-AutomationPSCredential` замены в среде ISE на `AWScred = Get-Credential`. Эта инструкция запрашивает учетные данные и позволяет указать идентификатор ключа доступа для имени пользователя и секретного ключа доступа для пароля. 

        ```powershell
        #Sample to get the AWS VM available images
        #Please provide the path where you have downloaded the AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up the environment to access AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile
        ```
        
    Возвращается следующий результат:<br><br>
   ![Получение образов AWS](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Скопируйте и вставьте одно из имен изображений в переменную автоматизации, как это делается в модуле Runbook, `$InstanceType`как. Так как в этом примере используется бесплатная многоуровневая подписка, в качестве примера модуля Runbook используется **T2. Micro** .  
9. Сохраните модуль Runbook, нажмите кнопку **Опубликовать**, чтобы опубликовать модуль Runbook, а затем кнопку **Да** в появившемся запросе.

### <a name="testing-the-aws-vm-runbook"></a>Тестирование модуля Runbook для AWS
Прежде чем продолжить тестирование модуля Runbook, необходимо проверить несколько моментов.

* Был создан ресурс `AWScred` , вызываемый для проверки подлинности в AWS, или сценарий был обновлен для ссылки на имя ресурса учетных данных.    
* Модуль PowerShell AWS был импортирован в службу автоматизации Azure.  
* Создан новый модуль Runbook, и значения параметров были проверены и обновлены там, где это необходимо.  
* Запись **подробных записей в журнал** и при необходимости **записи хода выполнения в журнале** операций Runbook **и трассировка** были установлены в значение **On**.<br><br> ![Ведение журнала и трассировка](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)Runbook.  

1. Нажмите кнопку **Пуск** , чтобы запустить модуль Runbook, а затем нажмите кнопку **ОК** , когда откроется панель запуск Runbook.
2. В области запуск Runbook введите имя виртуальной машины. Примите значения по умолчанию для других параметров, предварительно настроенных в скрипте. Нажмите кнопку **ОК** , чтобы запустить задание Runbook.<br><br> ![Запуск модуля Runbook: New-AwsVM](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Откроется область заданий с созданным заданием runbook. Закройте эту область.
4. Вы можете просмотреть ход выполнения задания и просмотреть выходные потоки, выбрав **все журналы** в области задания Runbook.<br><br> ![Раздел "Поток": выходные данные](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. Чтобы убедиться, что виртуальная машина подготовлена, войдите в консоль управления AWS, если вы не вошли в систему.<br><br> ![Консоль AWS: развернутая виртуальная машина](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Дальнейшие шаги
* Чтобы приступить к работе с графическими модулями Runbook, см. раздел [Мой первый графический модуль Runbook](automation-first-runbook-graphical.md).
* Чтобы приступить к работе с модулями Runbook рабочих процессов PowerShell, см. [Мой первый Runbook рабочего процесса PowerShell](automation-first-runbook-textual.md).
* См. сведения о типах runbook, их преимуществах и ограничениях в описании [типов последовательностей runbook в службе автоматизации Azure](automation-runbook-types.md)
* Дополнительные сведения о функции поддержки скриптов PowerShell см. [в статье поддержка собственных сценариев PowerShell в службе автоматизации Azure](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).