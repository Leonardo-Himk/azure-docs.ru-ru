---
title: Проверка состояния публикации предложения коммерческого магазина
description: Проверьте состояние проверки, сертификации и шагов предварительной версии, необходимых для публикации предложения через коммерческий магазин в центре партнеров Майкрософт.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 012a574887d9980e0c71c3af84ff70ca8d31312c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80275685"
---
# <a name="check-the-publishing-status-of-your-commercial-marketplace-offer"></a>Проверка состояния публикации предложения коммерческого магазина

Текущее **состояние публикации** можно просмотреть на вкладке " **Обзор** " [портала коммерческого рынка](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) в центре партнеров.

Для каждого предложения должен отображаться один из следующих индикаторов состояния.

| **Состояние**    | **Описание**  |
| :---------- | :-------------------|
| **Draft** | Предложение создано, но не опубликовано. |
| **Publish in progress** (Публикация выполняется) | Предложение или план работают на этапах процесса публикации. |
| **Требуется внимание** | Критическая ошибка была обнаружена во время сертификации корпорацией Майкрософт или любой из этапов публикации. |
| **Предварительный просмотр** | Предложение было сертифицировано корпорацией Майкрософт и теперь ожидает окончательной проверки издателя. Щелкните "начать прямо", чтобы сделать предложение активным. |
| **Опубликовано** | Предложение находится в Marketplace и может быть просмотрено и приобретено клиентами. |
| **Ожидание окончания продаж** | Выбран издатель "закончить продажу" в предложении или плане, но действие еще не завершено. |
| **Недоступно в Marketplace** | Ранее опубликованное предложение или план в Marketplace были удалены. |

## <a name="automated-validation"></a>Автоматическая проверка

Первый шаг в процессе публикации — это набор автоматизированных проверок. Каждый этап проверки соответствует функции, выбранной для включения при создании предложения. Если эта функция не была включена, проверка переходит к следующему шагу публикации. Каждая проверка должна быть завершена до утверждения состояния публикации.

- **Настройка потока покупки предложения (<10 мин)**

На этом шаге мы гарантируем, что предложение может быть выполнено при покупке клиентами через портал Azure. Этот шаг применим только для предложений, продаваемых через корпорацию Майкрософт.

- **Проверка данных тестового диска (не более 5 минут)**

На этом шаге мы проверяем данные, указанные в разделе Техническая конфигурация тестового диска предложения. Тестирование и утверждение функциональности тестового диска. Этот шаг применим только для предложений с включенным тестовым диском.

- **Подготовка тестового выпуска (не более 30 мин)**

На этом шаге после проверки данных и функциональности тестового диска на предыдущем шаге мы развернем и реплицирует экземпляры тестового диска, чтобы они были готовы к использованию клиентами.  Этот шаг применим только для предложений с включенным тестовым диском.

- **Проверка и регистрация управления интересами (<15 минут)**

На этом шаге мы подтверждаем, что ваша система управления интересами может получить интересы клиентов на основе сведений, указанных в настройках предложения. Этот шаг применим только для предложений с включенным управлением интересами.

## <a name="certification"></a>Сертификация

Перед публикацией предложения, отправленные на коммерческий рынок в центре партнеров, должны быть сертифицированы. Отправленные предложения проходят жесткое тестирование, некоторые автоматизированные и другие руководства, включая проверку на соответствие [политикам сертификации Azure Marketplace](https://docs.microsoft.com/legal/marketplace/general-policies). Перед переходом к следующему шагу в потоке публикации для отправки предложения необходимо пометить его как пригодное для сертификации.

### <a name="types-of-validation-that-take-place-during-certification"></a>Типы проверки, которые происходят во время сертификации

Существует три уровня проверки, которые включены в процесс сертификации для каждого отправленного предложения.

- Допустимость для бизнеса издателя
- Проверка содержимого
- Техническая проверка

#### <a name="publisher-business-eligibility"></a>Допустимость для бизнеса издателя

Каждый тип предложения проверяет набор базовых критериев соответствия, которым должен соответствовать издатель. Критерии допустимости могут включать состояние MPN издателя, соблюдающие компетенции, уровни компетенций и т. д.

#### <a name="content-validation"></a>Проверка содержимого

Во время проверки содержимого сведения, введенные при создании предложения, проверяются на качество и релевантность. Эти проверки просматривают ваши записи о списке Marketplace, ценах, доступности, связанных планах и т. д. Чтобы обеспечить соответствие требованиям Azure Marketplace и (или) AppSource, мы протестируем, что ваше предложение включает:

- название, которое точно описывает предложение;
- хорошо написанные описания, которые предоставляют исчерпывающий обзор и ценность;
- изображения снимка качества и сопроводительные видео; перетаскивани
- объяснение того, как предложение использует платформы и инструменты Майкрософт.

Узнайте больше о критериях проверки содержимого, прочитав [Общие политики перечня](https://docs.microsoft.com/legal/marketplace/certification-policies#100-general).

#### <a name="technical-validation"></a>Техническая проверка

Во время технической проверки предложение (пакет или двоичный файл) перейдет к следующим проверкам.
- Проверено на наличие вредоносных программ
- Отслеживаемые сетевые вызовы
- Пакет проанализирован
- Тщательное сканирование фактической функциональности предложения

Предложение проверяется на различных платформах и версиях, чтобы обеспечить надежность.

Ознакомьтесь с подробными сведениями о конфигурации, необходимыми для вашего предложения, в разделе Technical Configuration этого документа.

### <a name="certification-failure-report"></a>Отчет о сбоях сертификации

После завершения проверки, если предложение прошло сертификацию, оно перейдет к следующему шагу процесса публикации. Если ваше предложение не прошло какие-либо проверки, технические или политики или если вы не имеете права отправлять предложение этого типа, создается отчет о сбоях сертификации и отправляется сообщение электронной почты.

В этом отчете содержатся описания всех политик, которые не удалось выполнить, а также примечания по проверке. Изучите этот отчет по электронной почте, устраните все проблемы, внесите необходимые изменения в свое предложение и отправьте предложение повторно с помощью [портала коммерческого рынка](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) в центре партнеров. (Вы можете повторно отправить предложение столько раз, сколько необходимо, до передачи сертификации).

## <a name="preview-creation"></a>Создание предварительной версии

На этапе **создания предварительной** версии мы создадим версию вашего предложения, доступную только для аудитории, указанной в разделе вашего предложения Предварительная версия.

>[!Note]
> Не используйте этот шаг, чтобы предоставить пользователям за пределами Организации представление о предложении. Вместо этого используйте параметр **private предложение** . На этом этапе ваше предложение не было полностью протестировано и проверено и не готово к внешнему распределению.

## <a name="publisher-approval"></a>Утверждение издателя

На этом шаге вам будет отправлен запрос на проверку и одобрение предварительной версии предложения перед заключительным этапом публикации.

Если вы решили продать предложение в корпорации Майкрософт, вы сможете протестировать приобретение и развертывание вашего предложения, чтобы убедиться, что оно соответствует вашим требованиям на этапе утверждения предварительной версии. Ваше предложение пока не будет доступно в общедоступном рынке. После тестирования и утверждения этой предварительной версии необходимо выбрать **Go-Live** на панели мониторинга [**Обзор предложения**](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) .

Если вы хотите внести изменения в предложение на этапе предварительной версии, вы можете изменить и повторно отправить его, чтобы опубликовать новую предварительную версию. Дополнительные сведения об изменениях см. в статье [обновление существующих решений Marketplace](#update-existing-marketplace-offers) .

Если ваше предложение уже активно и доступно для общего доступа в Marketplace, все вносимые вами обновления не будут работать до тех пор, пока вы не наберете **Go-Live** на панели мониторинга " [**Обзор предложения**](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) ".

### <a name="publish-offer-to-the-public"></a>Опубликовать предложение в общедоступном

Войдите в центр партнеров и получите доступ к предложению. Вы будете перенаправлены на страницу **Обзор предложения** . В верхней части этой страницы вы увидите вариант для **Go Live**. Выберите **Go Live (перейти в реальном времени),** после подтверждения предложение начнет публиковаться в общедоступной области. Когда предложение будет активно, вы получите уведомление по электронной почте.

## <a name="publish"></a>Публикация

Теперь, когда вы выбрали предложение **в Интернете,** сделав его доступным в Marketplace, вы получите серию окончательных проверок, которые будут проходить через, чтобы убедиться, что предложение Live Offer настроено так же, как и предварительная версия предложения.

- **Настройка потока покупки предложения (>10 мин)**

На этом шаге мы гарантируем, что предложение может быть выполнено при покупке клиентами через портал Azure. Этот шаг применим только для предложений, продаваемых через корпорацию Майкрософт.

- **Проверка данных тестового диска (не более 5 минут)**

На этом шаге мы проверяем данные, указанные в разделе Техническая конфигурация тестового диска предложения. Тестирование и утверждение функциональности тестового диска. Этот шаг применим только для предложений с включенным тестовым диском.

- **Подготовка тестового выпуска (не более 30 мин)**

На этом шаге мы выполним развертывание и репликацию экземпляров тестового диска, чтобы они были готовы к использованию клиентами.  Этот шаг применим только для предложений с включенным тестовым диском.

- **Проверка и регистрация управления интересами (>15 минут)**

На этом шаге мы подтверждаем, что ваша система управления интересами может получить интересы клиентов на основе сведений, указанных в настройках предложения. Этот шаг применим только для предложений с включенным управлением интересами.

- **Окончательная публикация (>30 минут)**

На этом шаге мы гарантируем, что ваше предложение станет общедоступным в Marketplace.

## <a name="update-existing-marketplace-offers"></a>Обновление существующих предложений Marketplace

Если вы хотите внести изменения в предложение, которое вы уже опубликовали, необходимо сначала обновить существующее предложение, а затем снова опубликовать его.

## <a name="next-steps"></a>Дальнейшие шаги

- [Update an existing offer in the Commercial Marketplace](./update-existing-offer.md) (Обновление имеющегося предложения на коммерческой платформе Marketplace)
