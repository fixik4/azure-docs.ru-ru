---
title: Определение лиц. Компьютерное зрение
titleSuffix: Azure Cognitive Services
description: Понятия, связанные с функцией определения лиц API компьютерного зрения.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 1056b8be113d56342aea8f83d5325737f7ecb93b
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308457"
---
# <a name="face-detection-with-computer-vision"></a>Определение лиц с помощью компьютерного зрения

Компьютерное зрение обнаруживает на изображении лица людей и создает прямоугольник для каждого из них, а также указывает пол и возраст. 

> [!NOTE]
> Такую функцию также предлагает служба [Распознавание лиц](/azure/cognitive-services/face/). Дополнительные сведения об альтернативе для анализа лица, включая идентификацию лица и определение позы. 

## <a name="face-detection-examples"></a>Примеры определения лиц

В следующем примере демонстрируется ответ в формате JSON, возвращаемый компьютерным зрением для изображения, на котором имеется одно лицо.

![Анализ изображения. Распознавание лица женщины на крыше](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

В следующем примере демонстрируется ответ в формате JSON, возвращаемый для изображения, на котором имеется несколько лиц.

![Анализ изображения. Распознавание лиц на семейном фото](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об использовании функции обнаружения лиц см. в справочной документации по[анализу образа](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).
