---
title: Перевод речи с помощью пакета SDK службы "Речь" для C++
titleSuffix: Azure Cognitive Services
description: Эта статья содержит пример кода для перевода речи с помощью пакета SDK для службы "Речь" в среде C++.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: dd73314f5151be848567db3131ef016404f94829
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864321"
---
# <a name="translate-speech-with-the-cognitive-services-speech-sdk-for-c"></a>Перевод речи с помощью пакета SDK службы "Речь" (Cognitive Services) для C++

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-translate-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-translate-speech-intro.md)]

[!INCLUDE [Introduction to top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/translation_samples.cpp#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-translate-speech-microphone.md)]

[!code-cpp[Translation using a microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/translation_samples.cpp#TranslationWithMicrophone)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Найдите код, используемый в этой статье, в папке samples/cpp/windows/console.

## <a name="next-steps"></a>Дополнительная информация

- [Как распознавать речь](how-to-recognize-speech-cpp.md).
- [Как распознавать намерения на основе речи](how-to-recognize-intents-from-speech-cpp.md).
