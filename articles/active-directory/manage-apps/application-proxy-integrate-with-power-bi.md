---
title: Включение удаленного доступа для Power BI с помощью AD Application Proxy Azure
description: В этой статье рассматриваются основы интеграции локального Power BI с Azure AD Application Proxy.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro, has-adal-ref
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a6fab618280f1383e3840c67d85136edc095b9a
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610094"
---
# <a name="enable-remote-access-to-power-bi-mobile-with-azure-ad-application-proxy"></a>Обеспечение удаленного доступа к Power BI Mobile с помощью Azure Active Directory Application Proxy

В этой статье описывается, как использовать AD Application Proxy Azure, чтобы разрешить Power BI мобильного приложения подключаться к Сервер отчетов Power BI (PBIRS) и SQL Server Reporting Services (SSRS) 2016 и более поздних версий. Благодаря этой интеграции пользователи, которые находятся вне корпоративной сети, могут получать доступ к отчетам Power BI из мобильного приложения Power BI и защищать их с помощью проверки подлинности Azure AD. Эта защита включает в себя такие [преимущества безопасности](application-proxy-security.md#security-benefits) , как условный доступ и многофакторная идентификация.

## <a name="prerequisites"></a>Предварительные условия

В этой статье предполагается, что вы уже развернули службы Reporting Services и [включили прокси приложения](application-proxy-add-on-premises-application.md).

- Для включения прокси приложения необходимо установить соединитель на сервере Windows Server и выполнить [необходимые условия](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) , чтобы соединитель мог обмениваться данными со СЛУЖБАМИ Azure AD.
- При публикации Power BI рекомендуется использовать те же внутренние и внешние домены. Дополнительные сведения о пользовательских доменах см. [в статье работа с пользовательскими доменами в прокси приложения](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-custom-domain).
- Эта интеграция доступна для приложения **Power BI Mobile iOS и Android** .

## <a name="step-1-configure-kerberos-constrained-delegation-kcd"></a>Шаг 1. Настройка ограниченного делегирования Kerberos (KCD)

Для локальных приложений, использующих проверку подлинности Windows, единый вход можно обеспечить с помощью протокола проверки подлинности Kerberos и функции, называемой ограниченным делегированием Kerberos (KCD). При настройке KCD позволяет соединителю прокси приложения получить маркер Windows для пользователя, даже если пользователь не вошел в Windows напрямую. Дополнительные сведения о KCD см. в статье [Общие сведения о ограниченном делегировании Kerberos](https://technet.microsoft.com/library/jj553400.aspx) и [ограниченном делегировании Kerberos для единого входа в приложения с помощью прокси приложения](application-proxy-configure-single-sign-on-with-kcd.md).

На стороне служб Reporting Services не требуется сложная настройка. Чтобы обеспечить правильную проверку подлинности Kerberos, убедитесь, что у вас есть допустимое имя субъекта-службы (SPN). Также убедитесь, что Reporting Services сервер включен для проверки подлинности согласования.

Чтобы настроить KCD для служб Reporting Services, выполните следующие действия.

### <a name="configure-the-service-principal-name-spn"></a>Настройка имени субъекта-службы (SPN)

Имя субъекта-службы — это уникальный идентификатор для службы, которая использует проверку подлинности Kerberos. Необходимо убедиться, что у вас есть правильное имя участника-службы HTTP для сервера отчетов. Руководство по настройке правильного имени субъекта-службы (SPN) для сервера отчетов см. в статье о [регистрации имени субъекта-службы (SPN) для сервера отчетов](https://msdn.microsoft.com/library/cc281382.aspx).
Чтобы проверить добавление имени субъекта-службы, выполните команду Setspn с параметром -L. Дополнительные сведения о команде setspn см. в [этой статье](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spn-setspn-syntax.aspx).

### <a name="enable-negotiate-authentication"></a>Включить проверку подлинности согласованием

Чтобы сервер отчетов использовал проверку подлинности Kerberos, настройте тип проверки подлинности сервера отчетов на RSWindowsNegotiate. Настройте этот параметр с помощью файла RSReportServer. config.

```xml
<AuthenticationTypes>
    <RSWindowsNegotiate />
    <RSWindowsKerberos />
    <RSWindowsNTLM />
</AuthenticationTypes>
```

Дополнительные сведения см. в статьях об [изменении файла конфигурации Reporting Services ](https://msdn.microsoft.com/library/bb630448.aspx) и [настройке проверки подлинности Windows на сервере отчетов](https://msdn.microsoft.com/library/cc281253.aspx).

### <a name="ensure-the-connector-is-trusted-for-delegation-to-the-spn-added-to-the-reporting-services-application-pool-account"></a>Убедитесь, что соединитель является доверенным для делегирования имени участника-службы, добавленного в учетную запись пула приложений Reporting Services.
Настройте KCD таким образом, чтобы служба AD Application Proxy Azure могла делегировать удостоверения пользователей в учетную запись пула приложений Reporting Services. Для этого настройте для соединителя прокси приложения возможность получать билеты Kerberos для пользователей, которые прошли проверку подлинности в Azure AD. Затем этот сервер передает контекст в целевое приложение или Reporting Services в этом случае.

Чтобы настроить KCD, повторите следующие шаги для каждого компьютера соединителя:

1. Войдите на контроллер домена в качестве администратора домена, а затем откройте **Active Directory пользователи и компьютеры**.
2. Найдите компьютер, на котором запущен соединитель.
3. Дважды щелкните компьютер, а затем выберите вкладку **Делегирование** .
4. Установите параметры делегирования **доверять компьютеру делегирование указанных служб**. а затем — **Use any authentication protocol** (Использовать любой протокол проверки подлинности).
5. Нажмите кнопку **Добавить**, а затем выберите **Пользователи или компьютеры**.
6. Введите учетную запись службы, которую вы используете для Reporting Services. Это учетная запись, в которую вы добавили имя субъекта-службы во время настройки Reporting Services.
7. Нажмите кнопку **ОК**. Чтобы сохранить изменения, нажмите кнопку **ОК** еще раз.

Дополнительные сведения см. [в статье ограниченное делегирование Kerberos для единого входа в приложения с помощью прокси приложения](application-proxy-configure-single-sign-on-with-kcd.md).

## <a name="step-2-publish-report-services-through-azure-ad-application-proxy"></a>Шаг 2. Публикация служб отчетов с помощью Azure AD Application Proxy

Теперь вы готовы к настройке AD Application Proxy Azure.

1. Опубликуйте службы отчетов через прокси приложения со следующими параметрами. Пошаговые инструкции по публикации приложения через прокси приложения см. в статье [Публикация приложений с помощью Azure AD application proxy](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).
   - **Внутренний URL-адрес**: введите URL-адрес сервера отчетов, к которому соединитель может подключиться в корпоративной сети. Убедитесь, что этот URL-адрес доступен с сервера, на котором установлен соединитель. Рекомендуется использовать домен верхнего уровня, например, `https://servername/` чтобы избежать проблем с вложенными путями, опубликованными через прокси приложения. Например, используйте `https://servername/` , а не `https://servername/reports/` или `https://servername/reportserver/`.
     > [!NOTE]
     > Мы рекомендуем использовать безопасное HTTPS-соединение с сервером отчетов. Дополнительные сведения см. в разделе [Настройка SSL-соединений на сервере отчетов, собственном режиме](https://docs.microsoft.com/sql/reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server?view=sql-server-2017) .
   - **Внешний URL-адрес**: введите общедоступный URL-адрес, к которому будет подключаться мобильное приложение Power BI. Например, он может выглядеть, как `https://reports.contoso.com` если бы использовался личный домен. Чтобы использовать личный домен, отправьте сертификат для домена и укажите запись DNS в домен msappproxy.net по умолчанию для вашего приложения. Подробные инструкции см. [в статье работа с пользовательскими доменами в AD application proxy Azure](application-proxy-configure-custom-domain.md).

   - **Метод предварительной проверки подлинности**: Azure Active Directory

2. После публикации приложения необходимо настроить параметры единого входа с помощью следующих действий:

   а. На странице приложения на портале выберите **Единый вход**.

   b. **В режиме единого входа**выберите **Встроенная проверка подлинности Windows**.

   c. Задайте для параметра **Внутреннее имя субъекта-службы приложения** значение, заданное ранее.

   d. Выберите **делегированную идентификацию для входа**, которую соединитель сможет использовать от имени пользователей. Дополнительные сведения см. в разделе [Работа с разными локальными и облачными удостоверениями](application-proxy-configure-single-sign-on-with-kcd.md#working-with-different-on-premises-and-cloud-identities).

   д) Нажмите кнопку **Сохранить**, чтобы сохранить изменения.

Чтобы завершить настройку приложения, перейдите в раздел **"пользователи и группы** " и назначьте пользователям доступ к этому приложению.

## <a name="step-3-modify-the-reply-uris-for-the-application"></a>Шаг 3. изменение универсального кода ресурса (URI) ответа для приложения

Прежде чем мобильное приложение Power BI сможет подключиться к службам отчетов и получить к ним доступ, необходимо настроить регистрацию приложения, автоматически созданную на шаге 2.

1. На странице **обзор** Azure Active Directory выберите **Регистрация приложений**.
2. На вкладке **все приложения** найдите приложение, созданное на шаге 2.
3. Выберите приложение, а затем выберите **Проверка подлинности**.
4. Добавьте следующие URI перенаправления в зависимости от используемой платформы.

   При настройке приложения для Power BI Mobile **iOS**добавьте следующие URI перенаправления типа Public Client (мобильный & Desktop):
   - `msauth://code/mspbi-adal%3a%2f%2fcom.microsoft.powerbimobile`
   - `msauth://code/mspbi-adalms%3a%2f%2fcom.microsoft.powerbimobilems`
   - `mspbi-adal://com.microsoft.powerbimobile`
   - `mspbi-adalms://com.microsoft.powerbimobilems`

   При настройке приложения для Power BI Mobile **Android**добавьте следующие URI перенаправления типа Public Client (мобильный & Desktop):
   - `urn:ietf:wg:oauth:2.0:oob`
   - `mspbi-adal://com.microsoft.powerbimobile`
   - `msauth://com.microsoft.powerbim/g79ekQEgXBL5foHfTlO2TPawrbI%3D`
   - `msauth://com.microsoft.powerbim/izba1HXNWrSmQ7ZvMXgqeZPtNEU%3D`

   > [!IMPORTANT]
   > Для правильной работы приложения необходимо добавить URI перенаправления. Если вы настраиваете приложение как для Power BI Mobile iOS, так и для Android, добавьте следующий универсальный код ресурса (URI) перенаправления типа Public Client (мобильный & Desktop) в список URI `urn:ietf:wg:oauth:2.0:oob`перенаправления, настроенных для iOS:.

## <a name="step-4-connect-from-the-power-bi-mobile-app"></a>Шаг 4. подключение из приложения Power BI Mobile

1. В мобильном приложении Power BI подключитесь к экземпляру Reporting Services. Для этого введите **внешний URL-адрес** приложения, опубликованного через прокси приложения.

   ![Power BI мобильное приложение с внешним URL-адресом](media/application-proxy-integrate-with-power-bi/app-proxy-power-bi-mobile-app.png)

2. Выберите **Подключиться**. Вы будете перенаправлены на страницу входа Azure Active Directory.

3. Введите допустимые учетные данные для пользователя и нажмите кнопку **войти**. Вы увидите элементы с сервера Reporting Services.

## <a name="step-5-configure-intune-policy-for-managed-devices-optional"></a>Шаг 5. Настройка политики Intune для управляемых устройств (необязательно)

Microsoft Intune можно использовать для управления клиентскими приложениями, используемыми сотрудниками вашей компании. Intune позволяет использовать такие возможности, как шифрование данных и дополнительные требования к доступу. Дополнительные сведения об управлении приложениями с помощью Intune см. в статье Управление приложениями Intune. Чтобы разрешить Power BI мобильному приложению работать с политикой Intune, выполните следующие действия.

1. Перейдите в **Azure Active Directory** и затем **Регистрация приложений**.
2. При регистрации собственного клиентского приложения выберите приложение, настроенное на шаге 3.
3. На странице приложения выберите **разрешения API**.
4. Нажмите кнопку **Добавить разрешение**.
5. В разделе API, которые **использует Моя организация**, выполните поиск по фразе "Управление мобильными приложениями Майкрософт" и выберите его.
6. Добавление разрешения **девицеманажементманажедаппс. ReadWrite** в приложение
7. Щелкните **предоставить согласие администратора** , чтобы предоставить разрешение на доступ к приложению.
8. Настройте нужную политику Intune, обратившись к [созданию и назначению политик защиты приложений](https://docs.microsoft.com/intune/app-protection-policies).

## <a name="troubleshooting"></a>Устранение неполадок

Если приложение возвращает страницу ошибки после попытки загрузить отчет более чем через несколько минут, может потребоваться изменить параметр времени ожидания. По умолчанию прокси приложения поддерживает приложения, которые отреагируют на запрос до 85 секунд. Чтобы увеличить значение этого параметра на 180 секунд, выберите время ожидания серверной части **на странице** параметры прокси приложения для приложения. Советы по созданию быстрых и надежных отчетов см. в разделе [Power BI отчеты рекомендации](https://docs.microsoft.com/power-bi/power-bi-reports-performance).

## <a name="next-steps"></a>Дальнейшие действия

- [Включение собственного клиентского приложения для взаимодействия с прокси-приложениями](application-proxy-configure-native-client-application.md)
- [Просмотр локальных отчетов на сервере отчетов и ключевых показателей эффективности в мобильных приложениях Power BI](https://docs.microsoft.com/power-bi/consumer/mobile/mobile-app-ssrs-kpis-mobile-on-premises-reports)
