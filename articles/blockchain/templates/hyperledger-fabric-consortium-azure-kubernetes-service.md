---
title: Microsoft Kubernetes Service Fabric Consortium в службе Azure (AKS)
description: Развертывание и настройка сети консорциума Kubernetes для структуры Microsoft Azure
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: v-umha
ms.openlocfilehash: da4ec99f1b9d73ab67a2312094feaa1a89aee394
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82980237"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Microsoft Kubernetes Service Fabric Consortium в службе Azure (AKS)

Для развертывания и настройки сети "консорциум по структуре" в Azure можно использовать шаблон Service Fabric (ХЛФ) в службе Kubernetes Azure (AKS).

После прочтения этой статьи вы узнаете:

- Получите опыт работы со структурой «Главная книга» и различные компоненты, которые формируют стандартные блоки блокчейн Network структуры.
- Узнайте, как развернуть и настроить консорциум по работе с Kubernetes в службе Azure для рабочих сценариев.

## <a name="hyperledger-fabric-consortium-architecture"></a>Архитектура консорциума по этой структуре в книге

Чтобы создать сеть структуры «Главная книга» в Azure, необходимо развернуть службу упорядочения и организацию с одноранговыми узлами. Ниже перечислены различные фундаментальные компоненты, создаваемые в рамках развертывания шаблона.

- **Узлы заказов**. узел, отвечающий за упорядочение транзакций в главной книге. Вместе с другими узлами упорядоченные узлы формируют службу упорядочения в сети структуры «Главная книга».

- **Одноранговые узлы**: узел, в котором главным образом размещаются книги учета и интеллектуальные контракты, эти фундаментальные элементы сети.

- **CA Fabric**. ЦС структуры — это центр сертификации (CA) для структуры "Главная книга". ЦС структуры позволяет инициализировать и запустить серверный процесс, на котором размещается центр сертификации. Она позволяет управлять удостоверениями и сертификатами. Каждый кластер AKS, развернутый как часть шаблона, по умолчанию будет иметь модуль ЦС Fabric.

- **CouchDB или левелдб**: база данных состояния мира для одноранговых узлов может храниться либо в левелдб, либо в CouchDB. Левелдб — это база данных состояния по умолчанию, внедренная в одноранговый узел и хранящая данные чаинкоде в виде простых пар "ключ-значение" и поддерживающих только запросы ключей, диапазонов ключей и составных ключевых запросов. CouchDB — это необязательная альтернативная база данных состояний, которая поддерживает расширенные запросы, когда чаинкоде значения данных моделируются как JSON.

Шаблон при развертывании поворачивает различные ресурсы Azure в подписке. Развернутые ресурсы Azure:

- **Кластер AKS**: кластер Kubernetes Azure, настроенный в соответствии с входными параметрами, предоставленными клиентом. В кластере AKS есть различные модули, настроенные для работы сетевых компонентов структуры «книга». Различные созданные модули Pod:

  - **Средства структуры**. средство структуры отвечает за настройку компонентов структуры в книге.
  - Модули **(одноранговые**модули): узлы сети хлф.
  - **Прокси**— модуль-посредник нгникс, через который клиентские приложения могут взаимодействовать с кластером AKS.
  - **CA Fabric**: модуль, на котором работает ЦС структуры.
- **PostgreSQL**: экземпляр PostgreSQL развертывается для поддержки удостоверений ЦС структуры.

- **Хранилище ключей Azure**. экземпляр хранилища ключей развертывается для сохранения учетных данных ЦС структуры и корневых сертификатов, предоставленных клиентом, который используется в случае повторной попытки развертывания шаблона для работы с механизмом шаблона.
- **Управляемый диск Azure**: управляемый диск Azure предназначен для постоянного хранения для главной базы данных и однорангового узла.
- **Общедоступный IP-адрес**: конечная точка общедоступного IP-адреса кластера AKS, развернутая для взаимодействия с кластером.

## <a name="hyperledger-fabric-blockchain-network-setup"></a>Настройка сети в Блокчейн Fabric

Чтобы приступить к работе, вам понадобится подписка Azure, которая может поддерживать развертывание нескольких виртуальных машин и стандартные учетные записи хранения. Если у вас еще нет ее, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/).

Настройте Блокчейн Network для фабрики, выполнив следующие действия.

- [Развертывание заказа или одноранговой Организации](#deploy-the-ordererpeer-organization)
- [Создание консорциума](#build-the-consortium)

## <a name="deploy-the-ordererpeer-organization"></a>Развертывание заказа или одноранговой Организации

Чтобы начать работу с развертыванием сетевых компонентов ХЛФ, перейдите к [портал Azure](https://portal.azure.com). Выберите **создать ресурс > блокчейн** > поиска **в службе Kubernetes Azure**.

1. Выберите **создать** , чтобы начать развертывание шаблона. Откроется **служба создания фабрики Kubernetes в Azure** .

    ![Шаблон "структура" в Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-fabric-aks.png)

2. Введите сведения о проекте на странице " **основные** ".

    ![Шаблон "структура" в Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-basics.png)

3. Введите следующие сведения:
    - **Подписка**: выберите имя подписки, в которой вы хотите развернуть сетевые компоненты хлф.
    - **Группа ресурсов**. Создайте новую группу ресурсов или выберите существующую пустую группу ресурсов. Группа ресурсов будет содержать все ресурсы, развернутые в составе шаблона.
    - **Регион**: Выберите регион Azure, в котором вы хотите развернуть кластер Azure Kubernetes для компонентов хлф. Шаблон доступен во всех регионах, где AKS доступен для выбора региона, в котором ваша подписка не достигает предельной квоты виртуальной машины.
    - **Префикс ресурса**: префикс для именования развертываемых ресурсов. Длина префикса ресурса должна быть меньше шести символов, а сочетание символов должно содержать как цифры, так и буквы.
4. Выберите вкладку **Параметры структуры** , чтобы определить компоненты сети хлф, которые будут развернуты.

    ![Шаблон "структура" в Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-settings.png)

5. Введите следующие сведения:
    - **Название организации**: имя организации структуры, которая необходима для различных операций с плоскостью данных. Имя Организации должно быть уникальным для каждого развертывания.
    - **Компонент Fabric Network**: выберите либо службу упорядочения, либо одноранговые узлы, основанные на сетевом компоненте блокчейн, который вы хотите настроить.
    - **Число узлов** . ниже приведены два типа узлов.
        - Служба упорядочения — выберите число узлов, для которых была предоставлена отказоустойчивость сети. Количество узлов заказа поддерживается только для трех, 5 и 7.
        - Одноранговые узлы. Вы можете выбрать 1-10 узлов в соответствии с вашими требованиями.
    - **База данных состояния мира однорангового узла**: выберите между Левелдб и каукбдб. Это поле отображается, когда пользователь выбирает одноранговый узел в раскрывающемся списке сетевой компонент структуры.
    - **Имя пользователя структуры**. Введите имя пользователя, используемое для проверки подлинности ЦС структуры.
    - **Пароль ЦС структуры**. Введите пароль для проверки подлинности ЦС структуры.
    - **Подтверждение пароля**: Подтвердите пароль ЦС структуры.
    - **Сертификаты**. Если вы хотите использовать собственные корневые сертификаты для инициализации центра сертификации, выберите параметр Отправить корневой сертификат для центра сертификации. в противном случае по умолчанию ЦС структуры создает самозаверяющие сертификаты.
    - **Корневой сертификат**: Отправьте корневой сертификат (открытый ключ), с помощью которого необходимо инициализировать ЦС Fabric. Поддерживаются сертификаты формата PEM, сертификаты должны быть действительны в часовом поясе UTC.
    - **Закрытый ключ корневого сертификата**. Отправьте закрытый ключ корневого сертификата. Если у вас есть PEM-сертификат, который объединяет открытый и закрытый ключи, отправьте его здесь.


6. Выберите вкладку **Параметры кластера AKS** , чтобы определить конфигурацию кластера Azure Kubernetes, которая является базовой инфраструктурой, в которой будут настраиваться сетевые компоненты структуры.

    ![Шаблон "структура" в Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-aks-cluster-settings-1.png)

7. Введите следующие сведения:
    - **Имя кластера Kubernetes**. имя создаваемого кластера AKS. Это поле заполняется в соответствии с указанным префиксом ресурса, при необходимости можно изменить.
    - **Версия Kubernetes**. версия Kubernetes, которая будет развернута в кластере. В зависимости от региона, выбранного на вкладке « **основы** », доступные версии могут измениться.
    - **Префикс DNS**: префикс имени системы доменных имен (DNS) для кластера AKS. Вы будете использовать DNS для подключения к API Kubernetes при управлении контейнерами после создания кластера.
    - **Размер узла**. размер узла Kubernetes, который можно выбрать в списке номеров SKU виртуальной машины, доступных в Azure. Для оптимальной производительности рекомендуется использовать стандартные DS3 версии 2.
    - **Число узлов**: количество узлов Kubernetes, которые должны быть развернуты в кластере. Рекомендуется, чтобы количество узлов в параметрах Fabric не превышало, чем указано или больше.
    - **Идентификатор клиента субъекта-службы**. Введите идентификатор клиента существующего субъекта-службы или создайте новый, необходимый для проверки подлинности AKS. См. раздел, пошаговые инструкции по [созданию субъекта-службы](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal).
    - **Секрет клиента субъекта-службы**. Введите секрет клиента субъекта-службы, указанный в идентификаторе клиента субъекта-службы.
    - **Подтверждение секрета клиента**: подтвердите секрет клиента, указанный в секрете клиента субъекта-службы.
    - **Включить мониторинг контейнеров**: выберите Включение мониторинга AKS, которое позволяет журналам AKS отправлять данные в указанную рабочую область log Analytics.
    - **Рабочая область log Analytics**: Рабочая область log Analytics будет заполнена рабочей областью по умолчанию, созданной при включенном мониторинге.

8. После предоставления всех указанных выше сведений выберите вкладку **Проверка и создание** . Проверка и создание активирует проверку предоставленных значений.
9. После прохождения проверки можно нажать кнопку **создать**.
Развертывание обычно занимает 10-12 минут, в зависимости от размера и количества указанных узлов AKS.
10. После успешного развертывания вы получите уведомление через уведомления Azure в правом верхнем углу.
11. Выберите **Переход к группе ресурсов** , чтобы проверить все ресурсы, созданные в ходе развертывания шаблона. Все имена ресурсов будут начинаться с префикса, указанного в параметре **основы** .

## <a name="build-the-consortium"></a>Создание консорциума

Чтобы построить блокчейн Consortium после развертывания службы упорядочения и одноранговых узлов, необходимо выполнить последовательность действий ниже. Сценарий Azure ХЛФ (азлф), который помогает настроить консорциум, создать канал и операции чаинкоде.

> [!NOTE]
> В скрипте есть обновление, это обновление предназначено для предоставления дополнительных функциональных возможностей с помощью сценария Azure ХЛФ. Если вы хотите обратиться к старому сценарию, [см. здесь](https://github.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/blob/master/consortiumScripts/README.md). Этот сценарий совместим со структурой "Microsoft Kubernetes Service Template" в шаблоне службы Azure 2.0.0 и более поздних версий. Чтобы проверить версию развертывания, выполните действия, описанные в разделе [Устранение неполадок](#troubleshoot).

> [!NOTE]
> Скрипт Azure ХЛФ (азлф) предназначен для использования только в сценариях Demo и DevTest. Канал и консорциум, созданные этим сценарием, имеют базовые политики ХЛФ для упрощения сценария демонстрации и DevTest. Для настройки рабочей среды мы рекомендуем обновить политики Channel/Consortium ХЛФ в соответствии с потребностями Организации, используя собственные API ХЛФ.


Все команды для выполнения скрипта Azure ХЛФ можно выполнить с помощью командной строки Azure bash. Интерфейс (CLI). Вы можете войти в веб-версию оболочки Azure с помощью  ![Шаблон "структура" в Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) в правом верхнем углу портал Azure. В командной строке введите Bash и введите, чтобы переключиться на интерфейс командной строки bash.

Дополнительные сведения см. в разделе [оболочка Azure](https://docs.microsoft.com/azure/cloud-shell/overview) .

![Шаблон "структура" в Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


На следующем рисунке показан пошаговый процесс создания консорциума между Организацией-последовательностью и одноранговой Организацией. Подробные команды для выполнения этих действий записываются в следующих разделах.

![Шаблон "структура" в Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/process-to-build-consortium-flow-chart.png)

Выполните следующие команды для начальной настройки клиентского приложения: 

1.  [Скачать файлы клиентских приложений](#download-client-application-files)
2.  [Настройка переменных среды](#setup-environment-variables)
3.  [Импорт профиля подключения Организации, пользователя с правами администратора и MSP](#import-organization-connection-profile-admin-user-identity-and-msp)

После завершения начальной настройки можно использовать клиентское приложение для выполнения следующих операций:  

- [Команды управления каналами](#channel-management-commands)
- [Команды управления консорциумом](#consortium-management-commands)
- [Команды управления чаинкоде](#chaincode-management-commands)

### <a name="download-client-application-files"></a>Скачать файлы клиентских приложений

Сначала необходимо загрузить файлы клиентского приложения. Выполните приведенную ниже команду, чтобы скачать все необходимые файлы и пакеты.

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/azhlfToolSetup.sh | bash
cd azhlfTool
npm install
npm run setup

```
Эти команды будут клонировать код клиентского приложения Azure ХЛФ из общедоступного репозитория GitHub с последующим загрузкой всех зависимых пакетов NPM. После успешного выполнения команды вы увидите папку node_modules в текущем каталоге. Все необходимые пакеты загружаются в папку node_modules.


### <a name="setup-environment-variables"></a>Настройка переменных среды

> [!NOTE]
> Все переменные среды соответствуют соглашению об именовании ресурсов Azure.


**Задать следующие переменные среды для клиента организации заказа**


```bash
ORDERER_ORG_SUBSCRIPTION=<ordererOrgSubscription>
ORDERER_ORG_RESOURCE_GROUP=<ordererOrgResourceGroup>
ORDERER_ORG_NAME=<ordererOrgName>
ORDERER_ADMIN_IDENTITY="admin.$ORDERER_ORG_NAME"
CHANNEL_NAME=<channelName>
```
**Задайте следующие переменные среды для клиента одноранговой Организации.**

```bash
PEER_ORG_SUBSCRIPTION=<peerOrgSubscritpion>
PEER_ORG_RESOURCE_GROUP=<peerOrgResourceGroup>
PEER_ORG_NAME=<peerOrgName>
PEER_ADMIN_IDENTITY="admin.$PEER_ORG_NAME"
CHANNEL_NAME=<channelName>
```

> [!NOTE]
> В зависимости от числа одноранговых Организации в вашем консорциуме, возможно, потребуется повторить одноранговые команды и задать соответствующим образом переменную среды.

**Задайте следующие переменные среды для настройки учетной записи хранения Azure.**

```bash
STORAGE_SUBSCRIPTION=<subscriptionId>
STORAGE_RESOURCE_GROUP=<azureFileShareResourceGroup>
STORAGE_ACCOUNT=<azureStorageAccountName>
STORAGE_LOCATION=<azureStorageAccountLocation>
STORAGE_FILE_SHARE=<azureFileShareName>
```

Выполните действия, описанные ниже, для создания учетной записи хранения Azure. Если учетная запись хранения Azure уже создана, пропустите эти действия.

```bash
az account set --subscription $STORAGE_SUBSCRIPTION
az group create -l $STORAGE_LOCATION -n $STORAGE_RESOURCE_GROUP
az storage account create -n $STORAGE_ACCOUNT -g  $STORAGE_RESOURCE_GROUP -l $STORAGE_LOCATION --sku Standard_LRS
```

Выполните следующие действия для создания файлового ресурса в учетной записи хранения Azure. Если у вас уже есть созданная общая папка, пропустите эти действия.

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
az storage share create  --account-name $STORAGE_ACCOUNT  --account-key $STORAGE_KEY  --name $STORAGE_FILE_SHARE
```

Выполните действия ниже, чтобы создать строку подключения к общей папке Azure.

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
SAS_TOKEN=$(az storage account generate-sas --account-key $STORAGE_KEY --account-name $STORAGE_ACCOUNT --expiry `date -u -d "1 day" '+%Y-%m-%dT%H:%MZ'` --https-only --permissions lruwd --resource-types sco --services f | tr -d '"')
AZURE_FILE_CONNECTION_STRING=https://$STORAGE_ACCOUNT.file.core.windows.net/$STORAGE_FILE_SHARE?$SAS_TOKEN

```

### <a name="import-organization-connection-profile-admin-user-identity-and-msp"></a>Импорт профиля подключения Организации, удостоверения пользователя администратора и MSP

Выполните указанные ниже команды, чтобы получить профиль подключения Организации, удостоверение пользователя администратора и MSP из кластера Azure Kubernetes и сохранить эти удостоверения в локальном хранилище клиентского приложения, т. е. в каталоге "Азлфтул/Stores".

Для организации заказа:

```bash
./azhlf adminProfile import fromAzure -o $ORDERER_ORG_NAME -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION
./azhlf connectionProfile import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME   
./azhlf msp import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME
```

Для одноранговой Организации:

```bash
./azhlf adminProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf connectionProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf msp import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
```

### <a name="channel-management-commands"></a>Команды управления каналами

> [!NOTE]
> Прежде чем начать с любой операции с каналом, убедитесь, что начальная настройка клиентского приложения выполнена.  

Ниже приведены две команды управления каналами.

1. [Команда Create Channel](#create-channel-command)
2. [Установка одноранговых узлов-команда](#setting-anchor-peers-command)


#### <a name="create-channel-command"></a>Команда Create Channel

В клиенте организации заказа выполните команду, чтобы создать новый канал. Эта команда создает канал, в котором только в нем находится Организация заказа.  

```bash
./azhlf channel create -c $CHANNEL_NAME -u $ORDERER_ADMIN_IDENTITY -o $ORDERER_ORG_NAME
```

#### <a name="setting-anchor-peers-command"></a>Установка одноранговых узлов-команда
В клиенте одноранговой организации выдайте приведенную ниже команду, чтобы установить одноранговые узлы для одноранговой Организации в указанном канале.

>[!NOTE]
> Перед выполнением этой команды убедитесь, что в канале добавлена одноранговая организация с помощью команд управления консорциумом.

```bash
./azhlf channel setAnchorPeers -c $CHANNEL_NAME -p <anchorPeersList> -o $PEER_ORG_NAME -u $PEER_ADMIN_IDENTITY
```

`<anchorPeersList>`Разделенный пробелом список одноранговых узлов, которые должны быть установлены в качестве однорангового узла. Например,

  - Задайте `<anchorPeersList>` как "Peer1", если вы хотите задать только узел Peer1 в качестве закрепленного однорангового узла.
  - Задайте `<anchorPeersList>` "Peer1" "peer3", если вы хотите задать узел Peer1 и peer3 в качестве закрепленного однорангового узла.

### <a name="consortium-management-commands"></a>Команды управления консорциумом

>[!NOTE]
> Прежде чем начать работу с любой операцией консорциума, убедитесь, что начальная настройка клиентского приложения выполнена.  

Выполните приведенные ниже команды в указанном порядке, чтобы добавить одноранговую организацию в канал и консорциум.
1.  Из клиента одноранговой Организации передача MSP в службе хранилища Azure

      ```bash
      ./azhlf msp export toAzureStorage -f  $AZURE_FILE_CONNECTION_STRING -o $PEER_ORG_NAME
      ```
2.  В окне "клиент Организации заказа" Скачайте файл MSP для одноранговой Организации из службы хранилища Azure, а затем выполните команду, чтобы добавить одноранговую организацию в канал/консорциум.

      ```bash
      ./azhlf msp import fromAzureStorage -o $PEER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel join -c  $CHANNEL_NAME -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ./azhlf consortium join -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ```

3.  В службе "клиент Организации заказа" профиль подключения к заказу на отправку в хранилище Azure позволяет одноранговой Организации подключаться к узлам заказа с помощью этого профиля подключения

      ```bash
      ./azhlf connectionProfile  export toAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ```

4.  От клиента одноранговой Организации профиль подключения к заказу для скачивания из службы хранилища Azure, а затем выдается команда для добавления одноранговых узлов в канале.

      ```bash
      ./azhlf connectionProfile  import fromAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel joinPeerNodes -o $PEER_ORG_NAME  -u $PEER_ADMIN_IDENTITY -c $CHANNEL_NAME --ordererOrg $ORDERER_ORG_NAME
      ```

Аналогичным образом, чтобы добавить другие одноранговые Организации в канал, обновите переменные одноранговой среды в соответствии с требуемой одноранговой Организацией и выполните шаги 1 – 4.


### <a name="chaincode-management-commands"></a>Команды управления чаинкоде

>[!NOTE]
> Перед началом выполнения любой операции чаинкоде убедитесь, что начальная настройка клиентского приложения выполнена.  

**Задайте указанные ниже переменные среды чаинкоде.**

```bash
# peer organization name where chaincode operation is to be performed
ORGNAME=<PeerOrgName>
USER_IDENTITY="admin.$ORGNAME"  
# If you are using chaincode_example02 then set CC_NAME=“chaincode_example02”
CC_NAME=<chaincodeName>  
# If you are using chaincode_example02 then set CC_VERSION=“1” for validation
CC_VERSION=<chaincodeVersion>
# Language in which chaincode is written. Supported languages are 'node', 'golang' and 'java'  
# Default value is 'golang'  
CC_LANG=<chaincodeLanguage>  
# CC_PATH contains the path where your chaincode is place.
# If you are using chaincode_example02 to validate then CC_PATH=“/home/<username>/azhlfTool/chaincode/src/chaincode_example02/go”
CC_PATH=<chaincodePath>  
# Channel on which chaincode is to be instantiated/invoked/queried  
CHANNEL_NAME=<channelName>  
```

Ниже перечислены чаинкоде операции, которые могут быть выполнены.  

- [Установка чаинкоде](#install-chaincode)  
- [Создание экземпляра чаинкоде](#instantiate-chaincode)  
- [Вызвать чаинкоде](#invoke-chaincode)
- [Чаинкоде запросов](#query-chaincode)


### <a name="install-chaincode"></a>Установка чаинкоде  

Выполните приведенную ниже команду, чтобы установить чаинкоде в одноранговой Организации.  

```bash
./azhlf chaincode install -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION  

```
Он установит чаинкоде на всех одноранговых узлах одноранговой Организации, установленной в переменной среды ОРГНАМЕ. Если в канале есть две или более одноранговых Организации и вы хотите установить чаинкоде на всех этих узлах, выполните эту команду отдельно для каждой одноранговой Организации.  

Выполните следующие действия.  

1.  Задайте `ORGNAME` и `USER_IDENTITY` в качестве для каждой peerOrg1 `./azhlf chaincode install` и команды Issue.  
2.  Задайте `ORGNAME` и `USER_IDENTITY` в качестве для каждой peerOrg2 `./azhlf chaincode install` и команды Issue.  

### <a name="instantiate-chaincode"></a>Создание экземпляра чаинкоде  

Из однорангового клиентского приложения выполните приведенную ниже команду, чтобы создать экземпляр чаинкоде в канале.  

```bash
./azhlf chaincode instantiate -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -v $CC_VERSION -l $CC_LANG -c $CHANNEL_NAME -f <instantiateFunc> --args <instantiateFuncArgs>  
```
Передайте имя функции создания экземпляра и список аргументов, разделенных `<instantiateFunc>` пробелами, в и `<instantiateFuncArgs>` соответственно. Например, в chaincode_example02. Go чаинкоде, чтобы создать экземпляр чаинкоде со значением `<instantiateFunc>` `init`и `<instantiateFuncArgs>` в "a" "2000" "b" "1000".

> [!NOTE]
> Выполните команду один раз из любой одноранговой Организации в канале. После успешной отправки транзакции в заказ, заказ распределяет эту транзакцию всем одноранговым организациям в канале. Таким образом, экземпляр чаинкоде создается на всех одноранговых узлах во всех одноранговых организациях в канале.  


### <a name="invoke-chaincode"></a>Вызвать чаинкоде  

В клиенте одноранговой организации выполните следующую команду, чтобы вызвать функцию чаинкоде:  

```bash
./azhlf chaincode invoke -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <invokeFunc> -a <invokeFuncArgs>  
```

Передайте имя функции вызова и список аргументов, разделенных `<invokeFunction>` пробелами, в и `<invokeFuncArgs>` соответственно. Продолжим работу chaincode_example02 с примером чаинкоде. Go, чтобы выполнить операцию `<invokeFunction>` Invoke `invoke` со `<invokeFuncArgs>` значением и на "a" "b" "10".  

>[!NOTE]
> Выполните команду один раз из любой одноранговой Организации в канале. После успешной отправки транзакции в заказ, заказ распределяет эту транзакцию всем одноранговым организациям в канале. Таким образом, состояние мира обновляется на всех одноранговых узлах всех одноранговых организаций в канале.  


### <a name="query-chaincode"></a>Чаинкоде запросов  

Выполните приведенную ниже команду, чтобы запросить чаинкоде:  

```bash
./azhlf chaincode query -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <queryFunction> -a <queryFuncArgs>  
```
Передайте имя функции запроса и список аргументов, разделенных `<queryFunction>` пробелами, в и `<queryFuncArgs>` соответственно. Опять же, chaincode_example02. Go в качестве ссылки, чтобы запросить значение "a" в мировом состоянии со значением `<queryFunction>`  `query` и `<queryArgs>` в "a".  

## <a name="troubleshoot"></a>Диагностика

**Проверка версии выполняющегося шаблона**

Выполните приведенные ниже команды, чтобы найти версию развертывания шаблона.

Задайте следующие переменные среды в соответствии с группой ресурсов, в которой развернут шаблон.

```bash

SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
AKS_CLUSTER_SUBSCRIPTION=<AKSClusterSubscriptionID>
AKS_CLUSTER_RESOURCE_GROUP=<AKSClusterResourceGroup>
AKS_CLUSTER_NAME=<AKSClusterName>
```
Выполните следующую команду, чтобы распечатать версию шаблона
```bash
SWITCH_TO_AKS_CLUSTER $AKS_CLUSTER_RESOURCE_GROUP $AKS_CLUSTER_NAME $AKS_CLUSTER_SUBSCRIPTION
kubectl describe pod fabric-tools -n tools | grep "Image:" | cut -d ":" -f 3

```
