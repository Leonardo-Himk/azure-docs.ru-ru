---
title: Управление данными в хранилище BLOB-объектов Azure и их поиск с помощью индекса больших двоичных объектов (Предварительная версия)
description: Узнайте, как использовать теги индекса больших двоичных объектов для категоризации, управления и выполнения запросов для обнаружения объектов BLOB.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2020
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: f1a4d9af8a1b1095527078dd790e80ef45a5ee9a
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722899"
---
# <a name="manage-and-find-data-on-azure-blob-storage-with-blob-index-preview"></a>Управление данными в хранилище BLOB-объектов Azure и их поиск с помощью индекса больших двоичных объектов (Предварительная версия)

Так как наборы данных увеличиваются и увеличиваются, поиск определенного объекта в Sea может быть трудной задачей и затруднительным. Индекс большого двоичного объекта предоставляет возможности управления данными и обнаружения с помощью атрибутов тегов индекса "ключ-значение", которые позволяют классифицировать и найти объекты в одном контейнере или во всех контейнерах в учетной записи хранения. Позже, по мере изменения данных, можно динамически изменить классификацию объектов, обновив Теги индекса, сохраняя их в текущей организации контейнера. Использование индекса больших двоичных объектов позволяет упростить разработку, объединяя данные большого двоичного объекта и связанные атрибуты индекса в одной службе. позволяет создавать эффективные и масштабируемые приложения с помощью собственных функций. 

Индекс больших двоичных объектов позволяет:

- Динамически классифицировать большие двоичные объекты с помощью тегов индекса "ключ — значение" для управления данными
- Быстрый поиск конкретных больших двоичных объектов с тегами в одном контейнере или во всей учетной записи хранения
- Указание условного поведения для API-интерфейсов BLOB-объектов на основе оценки тегов индекса
- Использование тегов индекса для расширенного управления функциями платформы BLOB-объектов, такими как [Управление жизненным циклом](storage-lifecycle-management-concepts.md)

Рассмотрим ситуацию, когда у вас есть миллионы больших двоичных объектов в вашей учетной записи хранения, которые имеют доступ ко многим различным приложениям. Необходимо найти все связанные данные из одного проекта, но вы не уверены, что находится в области видимости, так как данные можно распределять по нескольким контейнерам с различными соглашениями об именовании BLOB-объектов. Однако вы уверены, что ваши приложения передают все данные с тегами на основе соответствующего проекта и идентифицируют описание. Вместо того чтобы выполнять поиск по миллионам больших двоичных объектов и сравнивать имена и свойства `Project = Contoso` , можно просто использовать в качестве условия обнаружения. Индекс BLOB-объекта фильтрует все контейнеры во всей учетной записи хранения, чтобы быстро найти и вернуть только набор из `Project = Contoso`50 больших двоичных объектов. 

Чтобы приступить к работе с примерами использования индекса больших двоичных объектов, см. раздел Использование [индекса больших двоичных объектов для управления и поиска данных](storage-blob-index-how-to.md).

## <a name="blob-index-tags-and-data-management"></a>Теги и Управление данными индексов больших двоичных объектов

Префиксы имени контейнера и BLOB-объекта — это одномерный способ классификации хранимых данных. Индекс BLOB-объекта теперь позволяет выполнять категоризацию с несколькими измерениями для всех [типов данных больших двоичных объектов (блоков, добавлений или страниц)](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) с применением тегов атрибутов. Эта многомерная классификация имеет собственный индекс и становится доступной для быстрого запроса и поиска данных.

Рассмотрим следующие пять больших двоичных объектов в вашей учетной записи хранения:
>
> container1/Transaction. CSV  
> контейнера container2/кампания. docx  
> Photos/баннерфото. png  
> архивирует/Completed/2019review. PDF  
> журналы/2020/01/01/logfile. txt  
>

В настоящее время эти BLOB-объекты разделены с помощью префикса контейнера, виртуальной папки или большого двоичного объекта. С помощью индекса больших двоичных объектов можно задать атрибут `Project = Contoso` тега индекса для этих пяти больших двоичных объектов, чтобы классифицировать их вместе, сохраняя их текущую организацию префиксов. Это избавляет от необходимости перемещать данные, предоставляя возможность фильтрации и поиска данных с помощью многомерного индекса платформы хранилища.

## <a name="setting-blob-index-tags"></a>Задание тегов индекса больших двоичных объектов

Теги индекса больших двоичных объектов — это атрибуты ключевого значения, которые могут применяться к новым или существующим объектам в вашей учетной записи хранения. Теги индекса можно указать во время процесса отправки с помощью операций PutBlob, PutBlockList или CopyBlob, а также необязательного заголовка x-MS-Tags. Если у вас уже есть большие двоичные объекты в вашей учетной записи хранения, вы можете вызвать Сетблобтагс с форматированным XML-документом, указав атрибуты тега индекса большого двоичного объекта в тексте запроса. 

Рассмотрим следующие примеры тегов, которые можно задать

Вы можете применить один тег к большому двоичному объекту, чтобы описать момент завершения обработки данных.
>
> "Процесседдате" = "2020-01-01"
>
К большому двоичному объекту можно применить несколько тегов, чтобы сделать их более описательными.
>
> "Проект" = "contoso"  
> "Классифицированные" = "true"  
> "Состояние" = "не обработано"  
> "Priority" = "01" 
>

Чтобы изменить существующие атрибуты тега индекса, необходимо сначала получить существующие атрибуты тега, изменить атрибуты тега и заменить на операцию Сетблобтагс. Чтобы удалить все теги индекса из большого двоичного объекта, вызовите операцию Сетблобтагс без указания атрибутов тега. Так как теги индекса больших двоичных объектов являются подресурсом для содержимого данных большого двоичного объекта, Сетблобтагс не изменяет никакого базового содержимого и не изменяет время последнего изменения большого двоичного объекта.

К тегам индекса больших двоичных объектов применяются следующие ограничения.
- Каждый большой двоичный объект может содержать до 10 тегов индекса больших двоичных объектов
- Длина ключей тегов должна составлять от 1 до 128 символов
- Значения тегов должны быть в диапазоне от 0 до 256 символов
- В ключах и значениях тегов учитывается регистр
- Ключи и значения тегов поддерживают только строковые типы данных; все числа или специальные символы будут сохранены в виде строк
- Ключи и значения тегов должны соответствовать следующим правилам именования:
  - Буквенные цифры: a – z, A – Z, 0-9
  - Специальные символы: пробел, плюс, минус, точка, двоеточие, знак равенства, подчеркивание, косая черта

## <a name="getting-and-listing-blob-index-tags"></a>Получение и перечисление тегов индекса больших двоичных объектов

Теги индекса больших двоичных объектов хранятся в виде вложенных ресурсов вместе с данными большого двоичного объекта и могут извлекаться независимо от базового содержимого данных большого двоичного объекта. После установки Теги индекса больших двоичных объектов для одного большого двоичного объекта можно получить и просмотреть немедленно с помощью операции Жетблобтагс. Операция ListBlobs с `include:tags` параметром также возвращает все большие двоичные объекты в контейнере вместе с их примененными тегами индекса больших двоичных объектов. 

Для больших двоичных объектов, имеющих по крайней мере один тег индекса больших двоичных объектов, в операциях ListBlobs, BLOB и GetBlobProperties возвращается число x-MS-Tag, указывающее количество тегов индекса больших двоичных объектов, существующих в большом двоичном объекте.

## <a name="finding-data-using-blob-index-tags"></a>Поиск данных с помощью тегов индекса больших двоичных объектов

Если для больших двоичных объектов заданы Теги индекса больших двоичных объектов, то механизм индексирования предоставляет эти атрибуты «ключ-значение» в многомерный индекс. Хотя Теги индекса существуют в большом двоичном объекте и могут быть получены немедленно, может потребоваться некоторое время перед обновлением индекса больших двоичных объектов с помощью атрибутов тега обновленного индекса. После обновления индекса больших двоичных объектов теперь можно использовать собственные возможности запросов и обнаружения, предоставляемые хранилищем BLOB-объектов.

Операция Финдблобсбитагс позволяет получить отфильтрованный набор возвращаемых BLOB-объектов, Теги индекса которых соответствуют заданному выражению запроса индекса BLOB-объекта. Индекс больших двоичных объектов поддерживает фильтрацию по всем контейнерам в вашей учетной записи хранения. можно также ограничить область фильтрации только одним контейнером. Поскольку все ключи и значения тегов индекса BLOB-объектов являются строками, поддерживаемые реляционные операторы используют лексикографическим порядком сортировку по значениям тега индекса.

Следующие критерии применяются к фильтрации индекса больших двоичных объектов.
-   Ключи тегов должны быть заключены в двойные кавычки (")
-   Значения тегов и имена контейнеров должны быть заключены в одинарные кавычки (')
-   Символ @ разрешен только для фильтрации по определенному имени контейнера (т. е. @container = ' ContainerName ')
- Фильтры применяются с сортировкой лексикографическим порядком по строкам
-   Недопустимые операции с одинаковым диапазоном для одного и того же ключа (например, "Rank" > "10" и "Rank" >= "15")
- При использовании функции RESTFUL для создания критерия фильтра символы должны быть в кодировке URI

В таблице ниже показаны все допустимые операторы для Финдблобсбитагс:

|  Оператор  |  Описание  | Пример |
|------------|---------------|---------|
|     =      |     Равно     | "Состояние" = "выполняется" | 
|     >      |  Больше |  "Date" > "2018-06-18" |
|     >=     |  Больше или равно | "Priority" >= "5" | 
|     <      |  Меньше чем    | "Age" < "32" |
|     <=     |  Меньше или равно  | "Company" <= "contoso" |
|    AND     |  Логическое и  | "Rank" >= "010" и "Rank" < "100" |
| @container |  Область действия для определенного контейнера   | @container= "видеофилес" и "Status" = ' Done ' |

## <a name="conditional-blob-operations-with-blob-index-tags"></a>Условные операции с BLOB-объектами с тегами индекса больших двоичных объектов
В 2019-10-10 и более поздних версиях большинство [API службы BLOB-объектов](https://docs.microsoft.com/rest/api/storageservices/operations-on-blobs) теперь поддерживают условный заголовок, x-MS-if-теги, чтобы операция была выполнена только при выполнении условия индекса большого двоичного объекта. Если условие не выполнено, вы получите `error 412: The condition specified using HTTP conditional header(s) is not met`.

Заголовок x-MS-if-Tags может быть объединен с другими существующими условными заголовками HTTP (если-Match, если-None-Match и т. д.).  Если в запросе указано несколько условных заголовков, для выполнения операции все они должны оцениваться как true.  Все условные заголовки фактически объединяются с логическими и. 

В таблице ниже показаны все допустимые операторы для условных операций:

|  Оператор  |  Описание  | Пример |
|------------|---------------|---------|
|     =      |     Равно     | "Состояние" = "выполняется" |
|     <>     |   Не равно   | "Состояние"  <>  "Готово"  | 
|     >      |  Больше |  "Date" > "2018-06-18" |
|     >=     |  Больше или равно | "Priority" >= "5" | 
|     <      |  Меньше чем    | "Age" < "32" |
|     <=     |  Меньше или равно  | "Company" <= "contoso" |
|    AND     |  Логическое и  | "Rank" >= "010" и "Rank" < "100" |
|     OR     |  Логическое или   | "Status" = "Done" или "Priority" >= "05" |

> [!NOTE]
> Существует два дополнительных оператора, которые не равны и являются логическими или, которые допускаются в условном заголовке x-MS-if-Tags для операции с BLOB-объектом, но не существуют в операции Финдблобсбитагс.

## <a name="platform-integrations-with-blob-index-tags"></a>Интеграция платформ с тегами индекса больших двоичных объектов

Теги индекса больших двоичных объектов помогают классифицировать, управлять и выполнять поиск данных больших двоичных объектов, но также обеспечивают интеграцию с другими функциями службы BLOB-объектов, такими как [Управление жизненным циклом](storage-lifecycle-management-concepts.md). 

### <a name="lifecycle-management"></a>Управление жизненным циклом

Используя новый Блобиндексматч в качестве фильтра правил в управлении жизненным циклом, можно перемещать данные на дополнительные уровни или удалять данные на основе тегов индекса, примененных к BLOB-объектам. Это позволяет более детально применять правила и перемещать или удалять данные только в том случае, если они соответствуют заданным критериям тегов.

Можно задать сопоставление индекса больших двоичных объектов в качестве отдельного фильтра, установленного в правиле жизненного цикла для применения действий к данным с тегами. Также можно объединить сопоставление префиксов и индекса больших двоичных объектов в соответствии с более конкретными наборами данных. Применение нескольких фильтров к правилу жизненного цикла рассматривает логическую операцию и, так что действие будет применено только в том случае, если все условия фильтра совпадают. 

Следующий пример правила управления жизненным циклом применяется для блочных BLOB-объектов в контейнере "видеофилес" и многоуровневых больших двоичных объектов в архивном хранилище только в том случае ```"Status" = 'Processed' AND "Source" == 'RAW'```, если данные соответствуют критериям тега индекса BLOB-объекта.

# <a name="portal"></a>[Портал](#tab/azure-portal)
![Пример правила соответствия индекса больших двоичных объектов для управления жизненным циклом в портал Azure](media/storage-blob-index-concepts/blob-index-lifecycle-management-example.png)
   
# <a name="json"></a>[JSON](#tab/json)
```json
{
    "rules": [
        {
            "enabled": true,
            "name": "ArchiveProcessedSourceVideos",
            "type": "Lifecycle",
            "definition": {
                "actions": {
                    "baseBlob": {
                        "tierToArchive": {
                            "daysAfterModificationGreaterThan": 0
                        }
                    }
                },
                "filters": {
                    "blobIndexMatch": [
                        {
                            "name": "Status",
                            "op": "==",
                            "value": "Processed"
                        },
                        {
                            "name": "Source",
                            "op": "==",
                            "value": "RAW"
                        }
                    ],
                    "blobTypes": [
                        "blockBlob"
                    ],
                    "prefixMatch": [
                        "videofiles/"
                    ]
                }
            }
        }
    ]
}
```
---

## <a name="permissions-and-authorization"></a>Разрешения и авторизация

Вы можете авторизовать доступ к индексу большого двоичного объекта, используя один из следующих подходов:

- С помощью управления доступом на основе ролей (RBAC) предоставьте разрешения для субъекта безопасности Azure Active Directory (Azure AD). Корпорация Майкрософт рекомендует использовать Azure AD для обеспечения более высокого уровня безопасности и простоты в использовании. Дополнительные сведения об использовании Azure AD с операциями BLOB-объектов см. в разделе [авторизация доступа к BLOB-объектам и очередям с помощью Azure Active Directory](../common/storage-auth-aad.md).
- Используя подписанный URL-адрес (SAS) для делегирования доступа к индексу большого двоичного объекта. Дополнительные сведения о подписанных URL-адресах см. [в статье предоставление ограниченного доступа к ресурсам службы хранилища Azure с помощью подписанных URL (SAS)](../common/storage-sas-overview.md).
- С помощью ключей доступа к учетной записи для авторизации операций с общим ключом. Дополнительные сведения см. в статье [Авторизация с помощью общего ключа](/rest/api/storageservices/authorize-with-shared-key).

Теги индекса больших двоичных объектов являются вложенными ресурсами для данных больших двоичных объектов. Пользователь с разрешениями или маркером SAS для чтения или записи больших двоичных объектов может не иметь доступа к тегам индекса больших двоичных объектов. 

### <a name="role-based-access-control"></a>Управление доступом на основе ролей 
Вызывающим объектам, использующим [удостоверение AAD](../common/storage-auth-aad.md) , могут быть предоставлены следующие разрешения на работу с тегами индекса BLOB-объектов. 

|   Операции с BLOB-объектами   |  Действие RBAC   |
|---------------------|----------------|
| Поиск больших двоичных объектов по тегам  | Microsoft. Storage/storageAccounts/Блобсервицес/Containers/blobs/Filter |
| Задать теги больших двоичных объектов         | Microsoft. Storage/storageAccounts/Блобсервицес/Containers/blobs/Tags/запись | 
| Получение тегов больших двоичных объектов         | Microsoft. Storage/storageAccounts/Блобсервицес/контейнеры/BLOB-объекты/Теги/чтение |

Для обработки тегов требуются дополнительные разрешения, отделенные от базовых данных большого двоичного объекта. Роли участника данных BLOB-объекта хранилища будут предоставлены все три из этих разрешений. Модулю чтения BLOB-объектов хранилища будут предоставлены разрешения "Поиск больших двоичных объектов по тегам" и "получить Теги BLOB-объектов".

### <a name="sas-permissions"></a>Разрешения SAS 
Вызывающим объектам, использующим [подписанный URL-адрес (SAS)](../common/storage-sas-overview.md) , могут быть предоставлены разрешения для выполнения тегов больших двоичных объектов.
#### <a name="blob-sas"></a>SAS BLOB-объекта
Следующие разрешения могут быть предоставлены в сопоставлении безопасности службы BLOB-объектов, чтобы разрешить доступ к тегам индекса BLOB-объектов. Единственные разрешения на чтение и запись больших двоичных объектов недостаточно, чтобы разрешить чтение или запись тегов индекса.

|  Разрешение  |  Символ URI  | Разрешенные операции |
|--------------|--------------|--------------------|
|  Теги индекса  |      t         | Получение и задание тегов индекса больших двоичных объектов для большого двоичного объекта |

#### <a name="container-sas"></a>SAS контейнера
Чтобы разрешить фильтрацию по тегам больших двоичных объектов, в SAS службы контейнеров можно предоставить следующие разрешения.  Разрешение на список BLOB-объектов недостаточно для того, чтобы разрешить фильтрацию больших двоичных объектов по их тегам индекса.

|  Разрешение  |  Символ URI  | Разрешенные операции |
|--------------|--------------|--------------------|
| Теги индекса     |      f       | Поиск больших двоичных объектов с помощью тегов индекса больших двоичных объектов | 

## <a name="choosing-between-metadata-and-blob-index-tags"></a>Выбор между метаданными и тегами индекса больших двоичных объектов 
Теги индекса больших двоичных объектов и метаданные предоставляют возможность хранить произвольные определяемые пользователем свойства ключа и значения вместе с ресурсом большого двоичного объекта. Они могут быть получены и заданы напрямую без возврата или изменения содержимого большого двоичного объекта. Можно использовать метаданные и теги индекса.

Однако только теги индекса больших двоичных объектов автоматически индексируются и делаются доступными для запросов собственной службой BLOB-объектов. Метаданные не могут индексироваться в собственном режиме и запрашиваться только при использовании отдельной службы, такой как [Поиск Azure](../../search/search-blob-ai-integration.md). Теги индекса больших двоичных объектов также имеют дополнительные разрешения на чтение, фильтрацию и запись, которые отделены от базовых данных больших двоичных объектов. Метаданные используют те же разрешения, что и BLOB-объект, и возвращаются в виде заголовков HTTP в операциях GetBlobProperties или больших двоичных объектов. Теги индекса больших двоичных объектов шифруются при хранении с помощью [ключа, управляемого корпорацией Майкрософт](../common/storage-service-encryption.md) , в то время как метаданные шифруются с использованием того же ключа шифрования, что и данные большого двоичного объекта. 

В следующей таблице перечислены различия между метаданными и тегами индекса больших двоичных объектов.

|              |   Метаданные   |   Теги индекса больших двоичных объектов  |
|--------------|--------------|--------------------|
| **Ограничения**         | Нет числового ограничения; 8 КБ всего; без учета регистра | 10 тегов на максимальное количество больших двоичных объектов; 768 байт на тег; с учетом регистра |
| **Обновления**      | Не разрешено на уровне архива; SetBlobMetadata заменяет все существующие метаданные; SetBlobMetadata изменяет время последнего изменения большого двоичного объекта | Разрешено для всех уровней доступа; Сетблобтагс заменяет все существующие теги; Сетблобтагс не изменяет время последнего изменения большого двоичного объекта |
| **Память**        | Сохранение с данными большого двоичного объекта |  Подресурсы в данные большого двоичного объекта | 
| **Индексирование & запросов** | Н/д изначально; необходимо использовать отдельную службу, например поиск Azure. | Да, собственные возможности индексирования и создания запросов, встроенные в хранилище BLOB-объектов |
| **Шифрование** | Зашифровано при хранении с тем же ключом шифрования, который используется для данных больших двоичных объектов |  Шифрование неактивных ключей с помощью ключа шифрования, управляемого корпорацией Майкрософт |
| **Цены**   | Размер метаданных включается в затраты на хранилище для большого двоичного объекта. |    Фиксированные затраты на тег индекса | 
| **Ответ заголовка** | Метаданные, возвращаемые в виде заголовков в GetBlobProperties и больших двоичных объектов | Тагкаунт, возвращенный в большом двоичном объекте или GetBlobProperties; Теги, возвращаемые только в Жетблобтагс и ListBlobs |
| **Разрешения**  |    Разрешения на чтение или запись данных большого двоичного объекта расширяются на метаданные |    Для чтения, фильтрации или записи тегов требуются дополнительные разрешения. |

## <a name="pricing"></a>Цены
Цены на индекс BLOB-объектов в настоящее время доступны в общедоступной предварительной версии и могут быть изменены для общедоступности. Клиентам выставляются счета за общее количество тегов индекса больших двоичных объектов в пределах учетной записи хранения, усредненное за месяц. Нет никаких затрат на подсистему индексирования. Запросы к Сетблобтагс, Жетблобтагс и Финдблобсбитагс начисляются в соответствии с соответствующими типами операций. [Дополнительные сведения см. в разделе цены на блочные BLOB](https://azure.microsoft.com/pricing/details/storage/blobs/)-объекты.

## <a name="regional-availability-and-storage-account-support"></a>Поддержка региональной доступности и учетных записей хранения

В настоящее время индекс BLOB-объекта доступен только для учетных записей общего назначения v2 (GPv2). В портал Azure можно обновить существующую учетную запись общего назначения (GPv1) до учетной записи GPv2. Дополнительные сведения об учетных записях хранения см. в статье [Общие сведения об учетной записи хранения](../common/storage-account-overview.md).

В общедоступной предварительной версии индекс BLOB-объекта в настоящее время доступен только в следующих регионах выбора:
- Центральная Франция
- Южная Франция

Чтобы приступить к работе, см. раздел [использование индекса больших двоичных объектов для управления и поиска данных](storage-blob-index-how-to.md).

> [!IMPORTANT]
> См. раздел "условия" этой статьи. Чтобы зарегистрироваться в предварительной версии, ознакомьтесь с разделом регистрация подписки в этой статье. Чтобы использовать индекс больших двоичных объектов в учетных записях хранения, необходимо зарегистрировать подписку.

### <a name="register-your-subscription-preview"></a>Регистрация подписки (Предварительная версия)
Так как индекс большого двоичного объекта доступен только в общедоступной предварительной версии, необходимо зарегистрировать подписку, прежде чем использовать эту функцию. Чтобы отправить запрос, выполните следующие команды в PowerShell или интерфейсе командной строки.

#### <a name="register-by-using-powershell"></a>Регистрация с помощью PowerShell
```powershell
Register-AzProviderFeature -FeatureName BlobIndex -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

#### <a name="register-by-using-azure-cli"></a>Регистрация с помощью Azure CLI
```azurecli
az feature register --namespace Microsoft.Storage --name BlobIndex
az provider register --namespace 'Microsoft.Storage'
```

## <a name="conditions-and-known-issues-preview"></a>Условия и известные проблемы (Предварительная версия)
В этом разделе описаны известные проблемы и условия в текущем общедоступном предварительном просмотре индекса больших двоичных объектов. Как и в случае с большинством предварительных версий, эту функцию не следует использовать для рабочих нагрузок, пока она не достигнет общедоступной версии, так как поведение может измениться.

-   Для предварительной версии необходимо сначала зарегистрировать подписку, прежде чем использовать индекс BLOB-объектов для вашей учетной записи хранения в регионах предварительного просмотра.
-   Сейчас в предварительной версии поддерживаются только учетные записи GPv2. Учетные записи BLOB-объектов, Блоккблобстораже и HNS, включенные в настоящее время, не поддерживаются с индексом больших двоичных объектов.
-   При отправке страничных BLOB-объектов с тегами индекса в настоящее время теги не сохраняются. Теги необходимо задать после отправки страничного BLOB-объекта.
-   Если фильтрация ограничена одним контейнером, @container может передаваться только в том случае, если все теги индекса в выражении фильтра являются проверками равенства (ключ = значение). 
-   При использовании оператора Range с условием AND можно указать только то же имя ключа тега индекса (Age > "013" и Age < "100").
-   Управление версиями и индексом больших двоичных объектов в настоящее время не поддерживается. Теги индекса больших двоичных объектов сохраняются для версий, но в настоящее время не передаются в обработчик индекса больших двоичных объектов.
-   Отработка отказа учетной записи в настоящее время не поддерживается. После отработки отказа индекс BLOB-объекта может не обновиться должным образом.
-   Сейчас Управление жизненным циклом поддерживает только проверки равенства с совпадением индекса больших двоичных объектов.
-   CopyBlob не копирует Теги индекса больших двоичных объектов из исходного BLOB-объекта в новый целевой BLOB-объект. Вы можете указать теги, которые необходимо применить к целевому большому двоичному объекту во время операции копирования. 
-   Теги сохраняются при создании моментального снимка; Однако повышение уровня моментального снимка в настоящее время не поддерживается и может привести к созданию пустого набора тегов.

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ

### <a name="can-blob-index-help-me-filter-and-query-content-inside-my-blobs"></a>Может ли индекс BLOB-объектов фильтровать и запрашивать содержимое в больших двоичных объектах? 
Нет, Теги индекса BLOB-объектов помогут найти искомые большие двоичные объекты. Если вам нужно выполнить поиск в больших двоичных объектах, используйте ускорение запросов или поиск Azure.

### <a name="are-blob-index-tags-and-azure-resource-manager-tags-related"></a>Являются ли Теги индексов BLOB-объектов и Azure Resource Manager тегами?
Нет, Azure Resource Manager Теги помогают упорядочить ресурсы плоскости управления, такие как подписки, группы ресурсов и учетные записи хранения. Теги индекса больших двоичных объектов обеспечивают управление объектами и их обнаружение в ресурсах плоскости данных, таких как большие двоичные объекты в учетной записи хранения.

## <a name="next-steps"></a>Дальнейшие действия

См. Пример использования индекса больших двоичных объектов. См. раздел [использование индекса больших двоичных объектов для управления и поиска данных](storage-blob-index-how-to.md) .

