---
title: Краткое руководство. Перевод речи с помощью приложения C# для универсальной платформы Windows в службе "Речь"
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве вы создадите простое приложение универсальной платформы Windows (UWP) для записи речи пользователя, перевода ее на другой язык и вывода текста в командную строку. Это руководство предназначено для пользователей Windows.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: 6195d9c978ccb7d9ff16454cbff9f46dbe08dbb8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "57871770"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Краткое руководство. Перевод речи с помощью пакета SDK службы "Речь" для C# (UWP)

В этом кратком руководстве вы создадите простое приложение универсальной платформы Windows (UWP), которое записывает речь пользователя с микрофона компьютера, переводит речь и расшифровывает переведенный текст в командную строку в режиме реального времени. Приложение предназначено для работы в 64-разрядной версии Windows с использованием [пакета NuGet SDK службы "Речь"](https://aka.ms/csspeech/nuget) и Microsoft Visual Studio 2017.

Полный список языков для перевода речи см. в статье, посвященной [поддержке языков](language-support.md).

> [!NOTE]
> UWP позволяет разрабатывать приложения, работающие на любом устройстве, которое поддерживает Windows 10, включая компьютеры, Xbox, Surface Hub и другие устройства.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Ключ подписки Azure для службы "Речь". [Его можно получить бесплатно](get-started.md).

## <a name="create-a-visual-studio-project"></a>Создание проекта Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Добавление примеров кода

1. Пользовательский интерфейс приложения определяется с помощью XAML. Откройте `MainPage.xaml` в обозревателе решений. В представлении XAML конструктора вставьте следующий фрагмент кода XAML между `<Grid>` и `</Grid>`.

    [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Откройте исходный файл с кодом `MainPage.xaml.cs` (он находится в группе `MainPage.xaml`). Замените все содержимое приведенным ниже кодом.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. В обработчике `SpeechTranslationFromMicrophone_ButtonClicked` этого файла замените строку `YourSubscriptionKey` своим ключом подписки.

1. В обработчике `SpeechTranslationFromMicrophone_ButtonClicked` замените строку `YourServiceRegion` значением [региона](regions.md), связанного с подпиской (например, `westus` для бесплатной пробной подписки).

1. Сохраните все внесенные в проект изменения.

## <a name="build-and-run-the-app"></a>Создание и запуск приложения

1. Создайте приложение. В строке меню последовательно выберите **Сборка** > **Собрать решение**. Теперь код должен компилироваться без ошибок.

    ![Снимок экрана приложения Visual Studio с выделенным параметром "Собрать решение"](media/sdk/qs-csharp-uwp-08-build.png "Успешная сборка")

1. Запустите приложение. В строке меню последовательно выберите **Отладка** > **Начать отладку** или нажмите клавишу **F5**.

    ![Снимок экрана приложения Visual Studio с выделенным параметром "Начать отладку"](media/sdk/qs-csharp-uwp-09-start-debugging.png "Start the app into debugging")

1. Откроется окно. Выберите**Включить микрофон** и подтвердите разрешение в появившемся окне запроса.

    ![Снимок экрана запроса разрешения](media/sdk/qs-csharp-uwp-10-access-prompt.png "Запуск отладки приложения")

1. Щелкните **Распознавание речи с помощью микрофона** и произнесите фразу или предложение на английском языке в микрофон вашего устройства. Ваша речь передается в службу "Речь" и преобразовывается в текст, который появляется в том же окне.

    ![Снимок экрана пользовательского интерфейса распознавания речи](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Примеры для C# на сайте GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>См. также

- [Настройка акустических моделей](how-to-customize-acoustic-models.md)
- [Настройка языковых моделей](how-to-customize-language-model.md)
