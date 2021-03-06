---
title: Настройка пиринга с Майкрософт
titleSuffix: Azure
description: Общие сведения о пиринге
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: overview
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 576bc3e37711851acd7d6c7ac811a10e40080710
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75908915"
---
# <a name="internet-peering-overview"></a>Общие сведения о пиринге в Интернете

Пиринг — это соединение между глобальной сетью Майкрософт (AS8075) и вашей сетью с целью обмена интернет-трафиком от онлайн-служб Microsoft и Microsoft Azure Services и к ним. Операторы или поставщики услуг могут запросить соединение с Майкрософт в любом из наших расположений Edge. Каждый запрос рассматривается Майкрософт, чтобы убедится, что он соответствует нашей политике пиринга. В сети Майкрософт можно настроить пиринг двумя способами:

* **Прямой пиринг.**

    Пиринг устанавливается по прямым физическим соединениям между сетью Майкрософт в Microsoft Edge и вашей сетью. Сеансы BGP настраиваются через эти соединения в соответствии с нашей политикой маршрутизации и с использованием предварительно согласованного соглашения. Это также называется PNI.

* **Пиринг через точку обмена:**

    Это относится к стандартным общедоступным пиринговым соединениям при межсетевом обмене трафиком (IX). Физические соединения между сетью Майкрософт и вашей сетью осуществляются через коммутационную структуру, управляемую IX. Сеансы BGP настраиваются с использованием IP-пространства, предоставляемого IX.

## <a name="benefits-of-peering-with-microsoft"></a>Преимущества пиринга с Майкрософт
* Сократите транзитные расходы доставляя трафик Майкрософт, используя пиринг с Майкрософт.
* Повысьте производительность своих пользователей, сократив количество прыжков и задержек в сети Microsoft Edge.
* Защитите трафик пользователей от сбоев в вашей сети или в сети транзитного поставщика, подключившись к Майкрософт в избыточных расположениях.
* Узнайте показатели производительности для ваших пиринговых соединений и используйте полезные сведения для устранения неполадок в сети.

## <a name="benefits-of-using-azure-to-set-up-peering"></a>Преимущества использования Azure для настройки пиринга

Пиринг в Майкрософт можно запросить, используя Azure PowerShell или портал. Таким образом, настройка пиринга, управляется как ресурс Azure и обеспечивает приведенные ниже преимущества.
* Упрощенные и автоматизированные шаги по настройке пиринга и управлению им, используя Майкрософт.
* Быстрый и простой способ просмотра всех пирингов и управления ими в одном месте.
* Отслеживание данных о состоянии и пропускной способности всех ваших соединений.
* Используйте ту же подписку для доступа к Облачным службам Azure.

Если вы уже установили пиринги с Майкрософт, они называются **устаревшими пирингами**. Вы можете управлять такими пирингами, как ресурс Azure, чтобы воспользоваться вышеуказанными преимуществами. Чтобы отправить новый запрос на пиринг или преобразовать устаревший пиринг в ресурс Azure, перейдите по ссылкам в разделе **Дальнейшие действия** ниже.

## <a name="peering-policy"></a>Политика пиринга
Майкрософт имеет выборочную, но в целом открытую политику пиринга. Одноранговые узлы выбираются на основе их производительности, возможностей и взаимной выгоды, и подчиняются определенным техническим, коммерческим и юридическим требованиям. Дополнительные сведения см. [политика пиринга](policy.md).

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ
Часто задаваемые вопросы о пиринге см. в разделе [Internet peering - FAQs](faqs.md) (Интернет-пиринг — вопросы и ответы).

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы узнать, как настроить прямой пиринг с Майкрософт следуйте инструкциям в [Direct peering walkthrough](walkthrough-direct-all.md) (Пошаговое руководство по прямому пирингу)
* Чтобы узнать, как настроить пиринг через точку обмена с Майкрософт следуйте инструкциям в [Exchange peering walkthrough](walkthrough-exchange-all.md) (Пошаговое руководство по пирингу через точку обмена)
* Ознакомьтесь с некоторыми другими ключевыми [сетевыми возможностями](https://docs.microsoft.com/azure/networking/networking-overview) Azure.
