---
title: Очистить кэш маркеров (MSAL.NET) | Службы
titleSuffix: Microsoft identity platform
description: Узнайте, как очистить кэш маркеров с помощью библиотеки проверки подлинности Майкрософт для .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: a10efb5ff0a2c6a3ced3631dfe82c86e3e8a72fc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77084766"
---
# <a name="clear-the-token-cache-using-msalnet"></a>Очистка кэша маркеров с помощью MSAL.NET

При [получении маркера доступа](msal-acquire-cache-tokens.md) с помощью библиотеки проверки подлинности Майкрософт для .net (MSAL.NET) маркер кэшируется. Когда приложению требуется токен, сначала необходимо вызвать `AcquireTokenSilent` метод, чтобы проверить, находится ли допустимый маркер в кэше. 

Очистка кэша достигается путем удаления учетных записей из кэша. Файл cookie сеанса, находящийся в браузере, не будет удален.  Следующий пример создает экземпляр общедоступного клиентского приложения, получает учетные записи для приложения и удаляет учетные записи.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Дополнительные сведения о получении и кэшировании токенов см. в статье [Получение маркера доступа](msal-acquire-cache-tokens.md).
