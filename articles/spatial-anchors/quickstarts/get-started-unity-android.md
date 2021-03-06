---
title: Краткое руководство. Создание приложения Unity Android
description: В этом кратком руководстве вы узнаете, как создать приложение Android с Unity с помощью Пространственных привязок.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7acff7f0249cdedcebd367fc315be92cafb9ab78
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2020
ms.locfileid: "77615442"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Краткое руководство. Создание приложения Android в Unity с помощью Пространственных привязок Azure

В этом кратком руководстве показано, как создать приложения Android в Unity с помощью [Пространственных привязок Azure](../overview.md). "Пространственные привязки Azure" — это кроссплатформенная служба разработчика, которая позволяет создавать среды смешанной реальности с применением объектов, не меняющих своего расположения на устройствах с течением времени. После завершения вы получите приложение Android ARCore, разработанное с использованием Unity, которое может сохранять и отзывать пространственные привязки.

Вы узнаете, как:

> [!div class="checklist"]
> * создать учетную запись в службе "Пространственные привязки";
> * настроить параметры сборки Unity;
> * настроить идентификатор и ключ учетной записи в службе "Пространственные привязки";
> * Экспорт проекта Android Studio
> * развертывать и запускать на устройстве Android.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

В рамках этого краткого руководства вам потребуются:

- Компьютер macOS или Windows с <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019.1 или 2019.2</a>, включая Android Build Support, средства NDK и пакет SDK для Android.
  - Если вы используете ОС Windows, вам также потребуется <a href="https://git-scm.com/download/win" target="_blank">Git для Windows</a> и <a href="https://git-lfs.github.com/">Git LFS</a>.
  - Если вы используете macOS, установите Git с помощью Homebrew. Введите в одну строку терминала такую команду: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Затем выполните `brew install git` и `brew install git-lfs`.
- Устройство Android с включенным <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">режимом разработчика</a> и поддержкой <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore</a>.
  - Для взаимодействия компьютера с устройством Android могут потребоваться дополнительные драйверы устройств. Дополнительные сведения и инструкции см. [здесь](https://developer.android.com/studio/run/device.html).

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Скачивание и открытие примера проекта Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Настройка идентификатора и ключа учетной записи

В области **Project** (Проект) перейдите к `Assets/AzureSpatialAnchors.Examples/Scenes` и откройте файл сцены `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Сохраните сцену, выбрав **File** -> **Save** (Файл > Сохранить).

## <a name="export-the-android-studio-project"></a>Экспорт проекта Android Studio

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

В окне **Run Device** (Запуск устройства) выберите свое устройство и щелкните **Сборка и запуск**. Вам будет предложено сохранить файл `.apk`, для которого можно выбрать любое имя.

Следуйте инструкциям в программе для размещения и отзыва привязки.

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="rendering-issues"></a>Проблемы отрисовки

Если во время запуска приложения вы не видите камеру как фон (к примеру, вы видите вместо этого пустой, синий фон или фон с другой текстурой), то вам нужно повторно импортировать файлы в Unity. Остановите приложение. В верхнем меню в Unity выберите **Assets -> Reimport all** (Ресурсы -> Повторно импортировать все). Затем снова запустите приложение.

### <a name="unity-20193"></a>Unity 2019.3

Из-за критических изменений сейчас Unity 2019.3 не поддерживается. Используйте Unity 2019.1 или 2019.2.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Руководство. по совместному использованию службы "Пространственные привязки" на разных устройствах](../tutorials/tutorial-share-anchors-across-devices.md)
