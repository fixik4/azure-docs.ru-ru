---
title: Сводка кратких руководств по использованию службы "Компьютерное зрение"
titleSuffix: Azure Cognitive Services
description: Из этих кратких руководств вы узнаете, как анализировать изображения, создавать эскизы и извлекать печатный и рукописный текст с помощью API компьютерного зрения.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 8fd27fc0038e91531fbf05942336ce7d6fc34e37
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310021"
---
# <a name="quickstart-summary"></a>Краткое руководство. Сводка

В этих кратких руководствах содержатся сведения и примеры кода, которые помогут вам быстро приступить к работе с API компьютерного зрения.

В примерах выполняются прямые HTTP-вызовы к API компьютерного зрения. Примеры использования клиентских библиотек для API компьютерного зрения с удобными методами создания оболочки для HTTP-вызовов см. в разделе *кратких руководств по пакетам SDK*.

Для быстрых экспериментов с API-интерфейсами компьютерного зрения можно использовать [открытую консоль тестирования API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

API компьютерного зрения позволяет выполнять следующие задачи:

* Анализ удаленного изображения
* Анализ локального изображения
* определение знаменитостей и достопримечательностей (модели предметной области);
* Интеллектуальное создание эскиза
* обнаружение и извлечение печатного текста (OCR) из изображения;
* обнаружение и извлечение рукописного текста из изображения.

Во всех примерах используется похожий код, но они посвящены разным возможностям API компьютерного зрения и методикам извлечения данных с использованием этой службы, в частности:

* _Создание эскизов_ возвращает изображение в виде массива байтов в тексте ответа.
* Для _анализа локального изображения_ изображение должно быть включено в запрос в виде массива байтов.
* Для _извлечения рукописного текста_ нужны два вызова.

## <a name="summary"></a>Сводка

| Быстрый запуск               | Параметры запроса                          | Ответ          |
| ------------------------ | ------------------------------------------- | ----------------  |
| Анализ удаленного изображения   | visualFeatures=Categories,Description,Color | Строка в формате JSON       |
| Анализ локального изображения    | data=image_data (массив байтов)                | Строка в формате JSON       |
| Определение знаменитостей       | model=celebrities                           | Строка в формате JSON       |
| Создание эскизов     | width=200&height=150&smartCropping=true     | массив байтов;        |
| Извлечение печатного текста     | language=unk&detectOrientation=true         | Строка в формате JSON       |
| Извлечение рукописного текста | handwriting=true                            | URL-адрес, строка в формате JSON  |

## <a name="next-steps"></a>Дополнительная информация

Ознакомьтесь с API-интерфейсами компьютерного зрения, которые позволяют анализировать изображения, обнаруживать знаменитостей и достопримечательности, создавать эскизы, извлекать печатный и рукописный текст.

> [!div class="nextstepaction"]
> [Сведения об API-интерфейсах компьютерного зрения](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
