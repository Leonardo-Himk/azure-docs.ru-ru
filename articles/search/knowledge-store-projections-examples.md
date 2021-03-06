---
title: Определение проекций в хранилище знаний
titleSuffix: Azure Cognitive Search
description: Примеры распространенных шаблонов для проецирования дополненных документов в хранилище знаний для использования с Power BI или МАШИНным обучением Azure.
manager: eladz
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: 23c370289669c2dde4f8969a2921018cd0abc08c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "78943684"
---
# <a name="knowledge-store-projections-how-to-shape-and-export-enrichments"></a>Проекции магазина базы знаний: как формировать и экспортировать обогащения

> [!IMPORTANT] 
> Сейчас предоставляется общедоступная предварительная версия хранилища знаний. Для предварительной версии функции соглашение об уровне обслуживания не предусмотрено. Мы не рекомендуем использовать ее в рабочей среде. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [Версия REST API 2019-05-06-Preview](search-api-preview.md) предоставляет предварительные версии функций. Сейчас доступна ограниченная поддержка портала. Поддержка пакета SDK для .NET пока не реализована.

Проекции — это физическое выражение для обогащенных документов в хранилище знаний. Для эффективного использования расширенных документов требуется структура. В этой статье вы ознакомитесь с структурой и связями, узнаете, как создавать свойства проекции, а также как связывать данные между создаваемыми типами проекций. 

Чтобы создать проекцию, необходимо сформировать данные с помощью [навыка](cognitive-search-skill-shaper.md) для создания пользовательского объекта или использовать синтаксис встроенного формирования в определении проекции. 

Фигура данных содержит все данные, которые планируется проецировать, сформированные как иерархия узлов. В этой статье показано несколько методов формирования данных, которые можно прогнозировать на физические структуры, пригодного отчеты, анализ или обработку. 

Примеры, приведенные в этой статье, можно найти в этом [REST API примере](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/projections/Projections%20Docs.postman_collection.json), который можно скачать и запустить в HTTP-клиенте.

## <a name="introduction-to-the-examples"></a>Общие сведения о примерах

Если вы знакомы с [проекциями](knowledge-store-projection-overview.md), вы помните о трех типах:

+ Таблицы
+ Объекты
+ Файлы

Проекции таблиц хранятся в хранилище таблиц Azure. Проекции объектов и файлов записываются в хранилище BLOB-объектов, где проекции объектов сохраняются как JSON-файлы и могут содержать содержимое из исходного документа, а также любые выводы или дополнения навыков. Конвейер обогащения также может извлекать двоичные файлы, например изображения, эти двоичные файлы проецируются в виде файловых проекций. Если двоичный объект проецируется как проекция объекта, только связанные с ним метаданные сохраняются в виде большого двоичного объекта JSON. 

Чтобы понять, как пересечение между формированием и проекцией данных, мы будем использовать следующий набор навыков для изучения различных конфигураций. Этот набор навыков обрабатывает необработанное изображение и текстовое содержимое. Проекции будут определены на основе содержимого документа и выходных данных навыков для сценариев, которые мы хотим поддерживать.

> [!IMPORTANT] 
> При экспериментах с проекциями полезно [задать свойство кэша индексатора](search-howto-incremental-index.md) , чтобы обеспечить управление затратами. Изменение проекций приведет к тому, что весь документ будет дополнен снова, если кэш индексатора не задан. Если кэш установлен и изменения внесены только в проекции, выполнение наборов навыков для ранее дополненных документов не приводит к появлению новых Cognitive Services расходов.

```json
{
    "name": "azureblob-skillset",
    "description": "Skillset created from the portal. skillsetName: azureblob-skillset; contentField: merged_content; enrichmentGranularity: document; knowledgeStoreStorageAccount: confdemo;",
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "name": "#1",
            "description": null,
            "context": "/document/merged_content",
            "categories": [
                "Person",
                "Quantity",
                "Organization",
                "URL",
                "Email",
                "Location",
                "DateTime"
            ],
            "defaultLanguageCode": "en",
            "minimumPrecision": null,
            "includeTypelessEntities": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "persons",
                    "targetName": "people"
                },
                {
                    "name": "organizations",
                    "targetName": "organizations"
                },
                {
                    "name": "locations",
                    "targetName": "locations"
                },
                {
                    "name": "entities",
                    "targetName": "entities"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "name": "#2",
            "description": null,
            "context": "/document/merged_content",
            "defaultLanguageCode": "en",
            "maxKeyPhraseCount": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "keyPhrases",
                    "targetName": "keyphrases"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "name": "#3",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                }
            ],
            "outputs": [
                {
                    "name": "languageCode",
                    "targetName": "language"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
            "name": "#4",
            "description": null,
            "context": "/document",
            "insertPreTag": " ",
            "insertPostTag": " ",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/content"
                },
                {
                    "name": "itemsToInsert",
                    "source": "/document/normalized_images/*/text"
                },
                {
                    "name": "offsets",
                    "source": "/document/normalized_images/*/contentOffset"
                }
            ],
            "outputs": [
                {
                    "name": "mergedText",
                    "targetName": "merged_content"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
            "name": "#5",
            "description": null,
            "context": "/document/normalized_images/*",
            "textExtractionAlgorithm": "printed",
            "lineEnding": "Space",
            "defaultLanguageCode": "en",
            "detectOrientation": true,
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                }
            ],
            "outputs": [
                {
                    "name": "text",
                    "targetName": "text"
                },
                {
                    "name": "layoutText",
                    "targetName": "layoutText"
                }
            ]
        }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "DemosCS",
        "key": "<COGNITIVE SERVICES KEY>"
    },
    "knowledgeStore": null
}
```

Используя этот набор навыков со значением NULL `knowledgeStore` в качестве базиса, наш первый пример заполняет `knowledgeStore` объект, настроенный с проекциями, которые создают табличные структуры данных, которые можно использовать в других сценариях. 

## <a name="projecting-to-tables"></a>Проецирование на таблицы

Проецирование на таблицы в службе хранилища Azure полезно для создания отчетов и анализа с помощью таких средств, как Power BI. Power BI может считывать из таблиц и обнаруживать связи на основе ключей, созданных во время проекции. Если вы пытаетесь создать панель мониторинга, то связанные данные упростит эту задачу. 

Предположим, что мы пытаемся создать панель мониторинга, на которой можно визуализировать ключевые фразы, извлеченные из документов, как слово Cloud. Чтобы создать правильную структуру данных, можно добавить навык создания фигуры в набор навыков, чтобы создать пользовательскую фигуру с подробными сведениями о документе и ключевыми фразами. Пользовательская фигура будет вызвана `pbiShape` на `document` корневом узле.

> [!NOTE] 
> Проекции таблиц — это таблицы службы хранилища Azure, управляемые ограничениями хранилища, накладываемыми службой хранилища Azure. Дополнительные сведения см. в разделе [ограничения хранилища таблиц](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). Полезно помнить, что размер сущности не может превышать 1 МБ, а одно свойство не может быть больше 64 КБ. Эти ограничения делают таблицы хорошим решением для хранения большого количества небольших сущностей.

### <a name="using-a-shaper-skill-to-create-a-custom-shape"></a>Использование навыка фигуры для создания пользовательской фигуры

Создайте пользовательскую фигуру, которую можно проецировать в хранилище таблиц. Без пользовательской фигуры проекция может ссылаться только на один узел (одна проекция на выход). Создание пользовательской фигуры позволяет объединять различные элементы в новое логическое целое число, которое может быть проецировано в виде одной таблицы или разбито по коллекциям таблиц. 

В этом примере пользовательская фигура сочетает метаданные и идентифицированные сущности и ключевые фразы. Объект вызывается `pbiShape` и является родительским по отношению `/document`к. 

> [!IMPORTANT] 
> Одной из целей формирования является обеспечение того, что все узлы обогащения выражаются в формате JSON с правильным форматом, который необходим для проецирования в хранилище знаний. Это особенно важно, если дерево обогащения содержит узлы, которые не являются правильным форматом JSON (например, когда обогащение является родительским для примитива, такого как строка).
>
> Обратите внимание на два последних `KeyPhrases` узла `Entities`, и. Они упаковываются в допустимый объект JSON с помощью `sourceContext`. Это требуется, как `keyphrases` и `entities` , являются дополнениями к примитивам и должны быть преобразованы в допустимые JSON, прежде чем их можно будет запланировать.
>


```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "name": "ShaperForTables",
    "description": null,
    "context": "/document",
    "inputs": [
        {
            "name": "metadata_storage_content_type",
            "source": "/document/metadata_storage_content_type",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "metadata_storage_name",
            "source": "/document/metadata_storage_name",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "metadata_storage_path",
            "source": "/document/metadata_storage_path",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "metadata_content_type",
            "source": "/document/metadata_content_type",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "keyPhrases",
            "source": null,
            "sourceContext": "/document/merged_content/keyphrases/*",
            "inputs": [
                {
                    "name": "KeyPhrases",
                    "source": "/document/merged_content/keyphrases/*"
                }

            ]
        },
        {
            "name": "Entities",
            "source": null,
            "sourceContext": "/document/merged_content/entities/*",
            "inputs": [
                {
                    "name": "Entities",
                    "source": "/document/merged_content/entities/*/name"
                }

            ]
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "pbiShape"
        }
    ]
}
```

Добавьте вышеуказанный навык в набор навыков. 

```json
    "name": "azureblob-skillset",
    "description": "A friendly description of the skillset goes here.",
    "skills": [
        {
            Shaper skill goes here
            }
        ],
    "cognitiveServices":  "A key goes here",
    "knowledgeStore": []
}  
```

Теперь, когда у нас есть все данные, необходимые для проецирования в таблицы, обновите объект Кновледжесторе с помощью определений таблиц. В этом примере у нас есть три таблицы, определяемые установкой `tableName`свойств `source` и. `generatedKeyName`

```json
"knowledgeStore" : {
    "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
    "projections": [
        {
            "tables": [
                {
                    "tableName": "pbiDocument",
                    "generatedKeyName": "Documentid",
                    "source": "/document/pbiShape"
                },
                {
                    "tableName": "pbiKeyPhrases",
                    "generatedKeyName": "KeyPhraseid",
                    "source": "/document/pbiShape/keyPhrases/*"
                },
                {
                    "tableName": "pbiEntities",
                    "generatedKeyName": "Entityid",
                    "source": "/document/pbiShape/Entities/*"
                }
            ],
            "objects": [],
            "files": []
        }
    ]
}
```

Вы можете обработать работу, выполнив следующие действия.

1. Задайте для ```storageConnectionString``` свойства допустимую строку подключения учетной записи хранения общего назначения v2.  

1. Обновите набор навыков, выполнив запрос на размещение.

1. После обновления набора навыков запустите индексатор. 

Теперь у вас есть рабочая проекция с тремя таблицами. Импорт этих таблиц в Power BI должен привести к Power BI автоматическому обнаружению связей.

Прежде чем перейти к следующему примеру, можно повторно перейти к аспектам проекции таблицы, чтобы понять механику срезов и связанных данных.

### <a name="slicing"></a>Среза 

Срез — это методика, которая разделяет всю объединенную фигуру на составляющие части. Результат состоит из отдельных, но связанных таблиц, с которыми можно работать по отдельности.

В примере `pbiShape` — это консолидированная фигура (или узел обогащения). В определении `pbiShape` проекции разделены на дополнительные таблицы, что позволяет извлекать части фигуры ```keyPhrases``` и. ```Entities``` В Power BI это полезно, так как несколько сущностей и фраз связаны с каждым документом, и вы получите более подробные сведения, если видите сущности и фраз в качестве данных по категориям.

```generatedKeyName``` При использовании среза неявным образом создается связь между родительской и дочерней таблицами с помощью объекта в родительской таблице для создания столбца с тем же именем в дочерней таблице. 

### <a name="naming-relationships"></a>Именование связей

Свойства ```generatedKeyName``` и ```referenceKeyName``` используются для связи данных между таблицами или даже между типами проекции. Каждая строка дочерней таблицы или проекции имеет свойство, указывающее на родительский элемент. Имя столбца или свойства дочернего элемента — ```referenceKeyName``` из родительского объекта. Если параметр ```referenceKeyName``` не указан, служба по умолчанию имеет значение ```generatedKeyName``` из родительского объекта. 

Power BI полагается на эти созданные ключи для обнаружения связей в таблицах. Если столбец в дочерней таблице необходим по-другому, установите ```referenceKeyName``` свойство для родительской таблицы. Одним из примеров является установка идентификатора ```generatedKeyName``` AS в таблице Пбидокумент и ```referenceKeyName``` DocumentID. Это приведет к тому, что столбец в таблицах Пбиентитиес и Пбикэйфрасес, содержащий идентификатор документа, называется DocumentID.

## <a name="projecting-to-objects"></a>Проецирование на объекты

Проекции объектов не имеют тех же ограничений, что и проекции таблиц, и лучше подходят для проецирования больших документов. В этом примере мы выполним проецирование всего документа в проекцию объекта. Проекции объектов ограничены одной проекцией в контейнере и не могут быть срезами.

Чтобы определить проекцию объекта, мы будем использовать ```objects``` массив в проекциях. Вы можете создать новую фигуру с помощью навыка для фигуры или использовать встроенную форму проекции объекта. Хотя в примере таблиц показан подход к созданию фигуры и среза, в этом примере демонстрируется использование встроенной формы. 

Встроенная форма — это возможность создать новую фигуру в определении входных данных для проекции. Встроенная форма создает анонимный объект, идентичный тому, что будет выдавать навык, `pbiShape`в нашем случае. Встроенная форма полезна при определении фигуры, которую не планируется использовать повторно.

Свойство проекций является массивом. В этом примере мы добавляем новый экземпляр проекции в массив, где определение Кновледжесторе содержит встроенные проекции. При использовании встроенных проекций можно опустить навык фигуры.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [ ],
                "objects": [
                    {
                        "storageContainer": "sampleobject",
                        "source": null,
                        "generatedKeyName": "myobject",
                        "sourceContext": "/document",
                        "inputs": [
                            {
                                "name": "metadata_storage_name",
                                "source": "/document/metadata_storage_name"
                            },
                            {
                                "name": "metadata_storage_path",
                                "source": "/document/metadata_storage_path"
                            },
                            {
                                "name": "content",
                                "source": "/document/content"
                            },
                            {
                                "name": "keyPhrases",
                                "source": "/document/merged_content/keyphrases/*"
                            },
                            {
                                "name": "entities",
                                "source": "/document/merged_content/entities/*/name"
                            },
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            },
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]

                    }
                ],
                "files": []
            }
        ]
    }
```

## <a name="projecting-to-file"></a>Проецирование в файл

Проекции файлов — это изображения, которые либо извлекаются из исходного документа, либо из результатов обогащения, которые можно запланировать за пределами процесса обогащения. Проекции файлов, схожие с проекциями объектов, реализуются как большие двоичные объекты в службе хранилища Azure и содержат образ. 

Чтобы создать файловую проекцию, мы используем `files` массив в объекте проекции. Этот пример проецирует все изображения, извлеченные из документа, в контейнер `samplefile`с именем.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [ ],
                "objects": [ ],
                "files": [
                    {
                        "storageContainer": "samplefile",
                        "source": "/document/normalized_images/*"
                    }
                ]
            }
        ]
    }
```

## <a name="projecting-to-multiple-types"></a>Проецирование на несколько типов

Более сложный сценарий может потребовать наличия содержимого проекта в разных типах проекций. Например, если необходимо проецировать некоторые данные, такие как ключевые фразы и сущности, в таблицы, сохраните результаты распознавания текста и макета в виде объектов, а затем попытайтесь проецировать изображения в виде файлов. 

В этом примере обновления для набора навыков включают следующие изменения:

1. Создайте таблицу со строкой для каждого документа.
1. Создайте таблицу, связанную с таблицей документов, с каждой ключевой фразой, идентифицированной в виде строки в этой таблице.
1. Создайте таблицу, связанную с таблицей документов, с каждой сущностью, идентифицированной в виде строки в этой таблице.
1. Создайте проекцию объекта с текстом макета для каждого изображения.
1. Создание проекции файла, проецирование каждого извлеченного изображения.
1. Создайте таблицу перекрестных ссылок, содержащую ссылки на таблицу документов, проекцию объекта с текстом макета и проекцию файла.

Эти изменения отражаются в определении Кновледжесторе. 

### <a name="shape-data-for-cross-projection"></a>Данные фигуры для перекрестной проекции

Чтобы получить фигуры, необходимые для этих проекций, начните с добавления нового навыка для создания фигуры, который создает объект с именем `crossProjection`. 

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "name": "ShaperForCross",
    "description": null,
    "context": "/document",
    "inputs": [
        {
            "name": "metadata_storage_name",
            "source": "/document/metadata_storage_name",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "keyPhrases",
            "source": null,
            "sourceContext": "/document/merged_content/keyphrases/*",
            "inputs": [
                {
                    "name": "KeyPhrases",
                    "source": "/document/merged_content/keyphrases/*"
                }

            ]
        },
        {
            "name": "entities",
            "source": null,
            "sourceContext": "/document/merged_content/entities/*",
            "inputs": [
                {
                    "name": "Entities",
                    "source": "/document/merged_content/entities/*/name"
                }

            ]
        },
        {
            "name": "images",
            "source": null,
            "sourceContext": "/document/normalized_images/*",
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                },
                {
                    "name": "layoutText",
                    "source": "/document/normalized_images/*/layoutText"
                },
                {
                    "name": "ocrText",
                    "source": "/document/normalized_images/*/text"
                }
                ]
        }
 
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "crossProjection"
        }
    ]
}
```

### <a name="define-table-object-and-file-projections"></a>Определение проекций таблиц, объектов и файлов

Из консолидированного объекта Кросспрожектион можно разделить объект на несколько таблиц, записать вывод OCR в виде больших двоичных объектов, а затем сохранить изображение как файлы (также в хранилище BLOB-объектов).

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [
                    {
                        "tableName": "crossDocument",
                        "generatedKeyName": "Id",
                        "source": "/document/crossProjection"
                    },
                    {
                        "tableName": "crossEntities",
                        "generatedKeyName": "EntityId",
                        "source": "/document/crossProjection/entities/*"
                    },
                    {
                        "tableName": "crossKeyPhrases",
                        "generatedKeyName": "KeyPhraseId",
                        "source": "/document/crossProjection/keyPhrases/*"
                    },
                    {
                        "tableName": "crossReference",
                        "generatedKeyName": "CrossId",
                        "source": "/document/crossProjection/images/*"
                    }
                     
                ],
                "objects": [
                    {
                        "storageContainer": "crossobject",
                        "generatedKeyName": "crosslayout",
                        "source": null,
                        "sourceContext": "/document/crossProjection/images/*/layoutText",
                        "inputs": [
                            {
                                "name": "OcrLayoutText",
                                "source": "/document/crossProjection/images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": [
                     {
                        "storageContainer": "crossimages",
                        "generatedKeyName": "crossimages",
                        "source": "/document/crossProjection/images/*/image"
                    }
                    ]
                
            }
        ]
    }
```

Для проецирования объектов требуется имя контейнера для каждой проекции, проекции объектов или проекции файлов не могут совместно использовать контейнер. 

### <a name="relationships-among-table-object-and-file-projections"></a>Связи между проектами таблиц, объектов и файлов

В этом примере также выделяется другая функция проекций. Определив несколько типов проекций в одном объекте проекции, можно определить связь между различными типами (таблицами, объектами, файлами), что позволяет начать с строки таблицы для документа и найти весь текст для изображений в этом документе в проекции объекта. 

Если вы не хотите, чтобы данные были связаны, определите проекции в различных объектах проекции. Например, следующий фрагмент приведет к тому, что таблицы будут связаны, но без связи между таблицами и проекциями объекта (OCR Text). 

Группы проекций полезны в тех случаях, когда требуется проецировать одни и те же данные в разные фигуры для различных нужд. Например, Группа проекции для панели мониторинга Power BI и другая группа проекции для записи данных, используемых для обучения модели машинного обучения, упакованной в пользовательскую квалификацию.

При построении проекций разных типов сначала создаются проекции файлов и объектов, а в таблицы добавляются пути.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [
                    {
                        "tableName": "unrelatedDocument",
                        "generatedKeyName": "Documentid",
                        "source": "/document/pbiShape"
                    },
                    {
                        "tableName": "unrelatedKeyPhrases",
                        "generatedKeyName": "KeyPhraseid",
                        "source": "/document/pbiShape/keyPhrases"
                    }
                ],
                "objects": [
                    
                ],
                "files": []
            }, 
            {
                "tables": [],
                "objects": [
                    {
                        "storageContainer": "unrelatedocrtext",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/text",
                        "inputs": [
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            }
                        ]
                    },
                    {
                        "storageContainer": "unrelatedocrlayout",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/layoutText",
                        "inputs": [
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": []
            }
        ]
    }
```

## <a name="common-issues"></a>Распространенные проблемы

При определении проекции существует несколько распространенных проблем, которые могут привести к непредвиденным результатам. Проверьте эти проблемы, если выходные данные в хранилище знаний не являются предполагаемыми.

+ Не дополняют строки в допустимый формат JSON. Если строки расширены, `merged_content` например дополненными ключевыми фразами, обогащенное свойство представляется как дочерний элемент `merged_content` в дереве обогащения. Представление по умолчанию не является форматом JSON правильного формата. Поэтому во время проекции не забудьте преобразовать обогащение в допустимый объект JSON, указав имя и значение.

+ Пропуск в ```/*``` конце исходного пути. Если источником проекции является `/document/pbiShape/keyPhrases`, то массив ключевых фраз проецируется как одиночный объект или строка. Вместо этого задайте для исходного пути значение `/document/pbiShape/keyPhrases/*` , чтобы получить одну строку или объект для каждой из ключевых фраз.

+ Ошибки синтаксиса пути. Селекторы пути учитывают регистр и могут привести к пропущенным входным предупреждениям, если не используется точный регистр для выбора.

## <a name="next-steps"></a>Дальнейшие шаги

В примерах, приведенных в этой статье, демонстрируются распространенные шаблоны создания проекций. Теперь, когда у вас есть хорошее представление об основных понятиях, вы лучше разрабатывать проекции для конкретного сценария.

При изучении новых функций рассмотрите возможность поэтапного углубления в качестве следующего шага. Добавочное дополнение основано на кэшировании, что позволяет повторно использовать любые обогащения, которые не затрагиваются изменением набора навыков. Это особенно удобно для конвейеров, включающих распознавание текста и анализ изображений.

> [!div class="nextstepaction"]
> [Общие сведения о добавочном обогащении и кэшировании](cognitive-search-incremental-indexing-conceptual.md)

Общие сведения о проекциях см. в разделе Дополнительные сведения о возможностях, таких как группы и срезы, а также о том, как [определить их в наборе навыков](knowledge-store-projection-overview.md) .

> [!div class="nextstepaction"]
> [Проекции в хранилище знаний](knowledge-store-projection-overview.md)

