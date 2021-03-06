---
title: Сопоставлении слов с помощью API перевода текстов
titlesuffix: Azure Cognitive Services
description: Получение сведений о выравнивании слов из API перевода текстов.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.custom: seodec18
ms.openlocfilehash: cc795d6b164a97351ec8806c6b7c8bed0c0c1266
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916909"
---
# <a name="how-to-receive-word-alignment-information"></a>Как получить сведения о сопоставлении слов

## <a name="receiving-word-alignment-information"></a>Получение сведений о сопоставлении слов
Чтобы получить сведения о сопоставлении, следует использовать метод Translate, включив необязательный параметр includeAlignment.

## <a name="alignment-information-format"></a>Формат сведений о сопоставлении
Сведения о сопоставлении возвращаются в виде строкового значения в следующем формате для каждого слова источника. Сведения о каждом слове разделяются пробелом, в том числе в языках (системах письменности) без разделения пробелами, таких как китайский:

[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]] *

Пример строки со сведениями о сопоставлении: 0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21.

Иными словами, двоеточие разделяет начальный и конечный индексы, дефис — языки, а пробел — слова. Одно слово может соответствовать нулю, одному или нескольким словам другого языка. При этом сопоставленные слова могут не располагаться рядом. Если сведения о сопоставлении недоступны, элемент Alignment будет пустым. В этом случае метод не возвращает ошибку.

## <a name="restrictions"></a>Ограничения
Сведения о сопоставлении на этом этапе возвращаются только для группы языковых пар:
* с английского на любой другой язык;
* с любого другого языка на английский, за исключением китайского упрощенного, китайского традиционного и латышского на английский;
* с японского на корейский или с корейского на японский. Вы не получите сведений о сопоставлении, если предложение представляет собой стандартный перевод. Примеры таких переводов — "Это тест", "Я люблю тебя" и другие предложения, встречающиеся с высокой частотой.

## <a name="example"></a>Пример

Пример JSON

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
