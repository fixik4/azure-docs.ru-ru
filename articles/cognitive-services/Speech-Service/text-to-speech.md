---
title: Преобразование текста в речь с помощью служб Azure речи
titleSuffix: Azure Cognitive Services
description: Преобразование текста в речь из служб речи Azure — это служба, на основе REST, которая позволяет приложениям, инструменты или устройств для преобразования текста в естественной человекоподобным синтезированную речь. Выберите "стандартный" и нейронной голоса или создать собственные пользовательские голосовые, уникальные для продуктов или торговой марке. 75 стандартный голоса, доступны в более чем 45 языков и языковых стандартов и 5 нейронной голоса, доступны в 4 языков и региональных параметров.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 1edc2587ef8680f60082bf6271b73d30184f331b
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521250"
---
# <a name="what-is-text-to-speech"></a>Что такое преобразования текста в речь?

Преобразование текста в речь из служб речи Azure — это служба, на основе REST, которая позволяет приложениям, инструменты или устройств для преобразования текста в естественной человекоподобным синтезированную речь. Выберите "стандартный" и нейронной голоса или создать собственные пользовательские голосовые, уникальные для продуктов или торговой марке. 75 стандартный голоса, доступны в более чем 45 языков и языковых стандартов и 5 нейронной голоса, доступны в 4 языков и региональных параметров. Полный список см. в разделе [поддерживаемые языки](language-support.md#text-to-speech).

Технологии преобразования текста в речь позволяет создателям содержимого для взаимодействия с пользователями по-разному. Преобразование текста в речь можно повысить доступность, предоставив пользователям возможность взаимодействовать с содержимым, что необходимо воспроизвести. Ли пользователь слабого зрения, слуха обучения или требует сведения о навигации, снижая, преобразования текста в речь повышает существующий стиль работы. Преобразование текста в речь также является ценным надстроек для программы-роботы голосовой и виртуальные помощники.

### <a name="neural-voices"></a>Синтезирование голоса с помощью нейронных сетей

Нейронные голосовые модели могут сделать взаимодействие с чат-ботами и виртуальными помощниками более естественным и приятным. Они позволяют преобразовывать цифровой текст из электронных книг в аудиокниги, а также помогают совершенствовать автомобильные системы навигации. С человекоподобным естественным prosody и очистить articulation слов нейронных голоса значительно снизить усталость при взаимодействии с системами искусственного Интеллекта. Дополнительные сведения о синтезировании голоса с помощью нейронных сетей см. [здесь](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Настраиваемое синтезирование голоса

Настройка голоса позволяет создавать голос запоминающееся, один тип для вашего бренда. Чтобы создать ваш настраиваемый голос, запись studio и отправить связанные сценарии, как набор обучающих данных. Затем служба создает модель уникального голоса, настроенную для записи. Можно использовать этот настраиваемый голос для синтезирования речи. Дополнительные сведения см. в разделе [настраиваемых голосах](how-to-customize-voice-font.md).

## <a name="core-features"></a>Основные функции

В этой таблице перечислены основные функции для преобразования текста в речь.

| Вариант использования  | SDK | REST |
|----------|-----|------|
| Преобразования текста в речь. | Нет  | Yes |
| Передача наборов данных для адаптации голоса. | Нет  | Да\* |
| Создание и управление ими голосовых моделей шрифта. | Нет  | Да\* |
| Создание и управление ими голоса шрифта развертываний. | Нет  | Да\* |
| Создание и управление ими голоса шрифта тестов. | Нет  | Да\* |
| Управление подписками. | Нет  | Да\* |

\* *Эти службы доступны cris.ai конечной точки. См. в разделе [Swagger ссылку](https://westus.cris.ai/swagger/ui/index).*

> [!NOTE]
> Конечную точку преобразования текста в речь реализует регулирование ограничивает запросы на 25 в 5 секунд. Если регулирование, вы будете получать уведомления через заголовки сообщений.

## <a name="get-started-with-text-to-speech"></a>Начало работы с преобразования текста в речь

Мы предлагаем краткие руководства предназначены для запуска кода в менее чем за 10 минут. Эта таблица содержит список озвучивания текста кратких руководств, упорядоченных по языка.

| Быстрый запуск | платформа | Справочник по API |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Обзор](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Окно, macOS, Linux | [Обзор](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Окно, macOS, Linux | [Обзор](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Пример кода

Пример кода для преобразования текста в речь доступен на сайте GitHub. Эти примеры охватывают преобразование текста в речь в наиболее популярных языков программирования.

* [Примеры преобразования текста в речь (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Справочная документация

* [пакет SDK для службы "Речь"](speech-sdk-reference.md);
* [Пакет SDK для речевых устройств](speech-devices-sdk.md)
* [REST API: Преобразование речи в текст](rest-speech-to-text.md)
* [REST API: Преобразование текста в речь](rest-text-to-speech.md)
* [REST API: Пакетное транскрибирование и настройка](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Дальнейшие действия

* [Получение бесплатной подписки на службу "Речь"](get-started.md).
* [Создание настраиваемого голоса](how-to-customize-voice-font.md)
