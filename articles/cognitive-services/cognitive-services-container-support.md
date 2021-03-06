---
title: Поддержка контейнеров
titleSuffix: Azure Cognitive Services
description: Узнайте, как c помощью контейнеров Docker выполнять обработку в Cognitive Services как можно ближе к своим данным.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: a60013bce63ed234e15dfddb13c07fbdc33a4073
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57339629"
---
# <a name="container-support-in-azure-cognitive-services"></a>Поддержка контейнеров в Azure Cognitive Services

Благодаря поддержке контейнеров в Azure Cognitive Services разработчики могут использовать те же интеллектуальные интерфейсы API, которые доступны в Azure, но с традиционной для [контейнеров Docker](https://www.docker.com/what-container) гибкостью в выборе расположения для развертывания и размещения служб. Сейчас поддержка контейнеров доступна в предварительной версии для подмножества служб Azure Cognitive Services, включая [Компьютерное зрение](Computer-vision/Home.md), [Распознавание лиц](Face/Overview.md), [Анализ текста](text-analytics/overview.md) и [Распознавание речи](LUIS/luis-container-howto.md) (LUIS).

Контейнеризация — это подход к распространению программного обеспечения, при котором приложение или служба упаковывается в образ контейнера вместе со всеми зависимостями и конфигурациями. Образ контейнера можно развертывать на узле контейнера с незначительной модификацией или без нее. Контейнеры изолированы друг от друга и от базовой операционной системы. При этом они расходуют меньше ресурсов, чем виртуальная машина. Контейнеры для краткосрочных задач можно создавать на основе образов контейнеров и удалять их, когда они станут ненужными.

В следующем видео показано использование контейнера Cognitive Services.

[Container demonstration for Cognitive Services![](./media/index/containers-video-image.png)](https://azure.microsoft.com/resources/videos/containers-support-of-cognitive-services) (Демонстрация контейнера для Cognitive Services)

Службы [Компьютерное зрение](Computer-vision/Home.md), [Распознавание лиц](Face/Overview.md), [Анализ текста](text-analytics/overview.md) и [Распознавание речи (LUIS)](LUIS/what-is-luis.md) доступны в [Microsoft Azure](https://azure.microsoft.com). Войдите на [портал Azure](https://portal.azure.com/), чтобы создать и изучить ресурсы Azure для этих служб.

## <a name="features-and-benefits"></a>Возможности и преимущества:

- **Контроль над данными**. Позвольте клиентам выбирать, где Cognitive Services будут обрабатывать их данные. Это особенно важно для тех клиентов, у которых есть потребность в технологии Cognitive Services, но нет возможности отправлять данные в облако. Поддержка согласованности в гибридной среде, в том числе данных, управления, идентификатора и безопасности.
- **Контроль над обновлениями модели**. Предоставьте клиентам гибкость в управлении версиями и обновлением моделей, развернутых в своих решениях.
- **Переносимая архитектура**. Позвольте создавать архитектуру переносимого приложения, которое можно развернуть в Azure, в локальной среде или на пограничном устройстве. Контейнеры можно развертывать непосредственно в [Службе Azure Kubernetes](../aks/index.yml), [Экземплярах контейнеров Azure](../container-instances/index.yml) или в кластере [Kubernetes](https://kubernetes.io/), развернутом в [Azure Stack](../azure-stack/index.yml). Дополнительные сведения см. в статье [Развертывание Kubernetes в Azure Stack](../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).
- **Высокая пропускная способность и низкая задержка.** Обеспечьте клиентам возможность масштабировать требования для повышения пропускной способности и снижения задержек, позволив выполнение Cognitive Services поблизости от физического расположения логики и данных приложения. Контейнеры не ограничивают число транзакций в секунду (TPS), и их можно увеличивать и уменьшать для удовлетворения спроса, предоставив необходимые аппаратные ресурсы. 


## <a name="containers-in-azure-cognitive-services"></a>Контейнеры в Azure Cognitive Services

Контейнеры Azure Cognitive Services предоставляют описанный ниже набор контейнеров Docker, каждый из которых содержит подмножество функций нескольких служб в Azure Cognitive Services.

| Service | Контейнер| ОПИСАНИЕ |
|---------|----------|-------------|
|[Компьютерное зрение](Computer-vision/computer-vision-how-to-install-containers.md) |**Распознавание текста** |Извлекает печатный текст из изображений различных объектов с разными поверхностями и фонами, например чеков, плакатов и визитных карточек.<br/><br/>**Важно!** Контейнер распознавания текста в настоящее время работает только на английском языке.<br>[Запрос доступа](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Распознавание лиц](Face/face-how-to-install-containers.md) |**Распознавание лиц** |Обнаруживает человеческие лица на изображениях и определяет методом машинного прогнозирования их характеристики, например черты лица (нос, глаза и т. д.), пол, возраст и многое другое. Кроме обнаружения лиц, служба может сравнивать два лица на одном или на разных изображениях, используя оценку достоверности, а также искать по базе данных похожие или идентичные лица. Схожие лица можно упорядочивать в группы по близким характеристикам.<br>[Запрос доступа](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[LUIS](LUIS/luis-container-howto.md) |**LUIS** ([образ](https://go.microsoft.com/fwlink/?linkid=2043204))|Загружает обученную или опубликованную модель Распознавания речи, также известную как приложение LUIS, в контейнер Docker и предоставляет доступ к прогнозам запроса из конечных точек API контейнера. Можно собирать журналы запросов из контейнера и отправлять их обратно на [портал LUIS](https://www.luis.ai) для повышения точности прогнозов приложения.|
|[Анализ текста](text-analytics/how-tos/text-analytics-how-to-install-containers.md) |**Извлечение ключевых фраз** ([изображение](https://go.microsoft.com/fwlink/?linkid=2018757)) |Извлекает ключевые фразы для определения основных идей текста. Например, для входного текста "Еда была вкусной и персонал был замечательным" API вернет основные тезисы в записи: "еда" и "замечательный персонал". |
|[Анализ текста](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Определение языка** ([изображение](https://go.microsoft.com/fwlink/?linkid=2018759)) |Решение может определить язык введенного текста (поддерживается более 120 языков) и сообщить один код языка для каждого документа, отправленного в запросе. Код языка сопряжен с показателем, указывающим степень оценки. |
|[Анализ текста](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Анализ тональности** ([изображение](https://go.microsoft.com/fwlink/?linkid=2018654)) |Анализирует необработанный текст на предмет положительной или отрицательной тональности. Этот API возвращает оценку тональности (0 или 1) для каждого документа, где 1 означает положительную тональность. Модели анализа предварительно обучены с использованием обширного набора текстов и технологий естественного языка корпорации Майкрософт. Для [выбранных языков](./text-analytics/language-support.md) API может анализировать и оценивать любой необработанный текст, напрямую возвращая результаты вызывающему приложению. |

## <a name="container-availability-in-azure-cognitive-services"></a>Доступность контейнеров в Azure Cognitive Services

Контейнеры Azure Cognitive Services общедоступны через подписку Azure, а образы для контейнеров Docker можно извлечь из Реестра контейнеров Microsoft или Docker Hub. Используйте команду [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), чтобы скачать образ контейнера из соответствующего репозитория.

> [!IMPORTANT]
> Сейчас для доступа к контейнерам [Распознавание лиц](Face/face-how-to-install-containers.md) и [Распознавание текста](Computer-vision/computer-vision-how-to-install-containers.md) требуется пройти регистрацию. Для этого нужно заполнить и отправить анкету с вопросами о вас, вашей компании и планируемых к реализации сценариях использования контейнеров. Получив доступ и учетные данные, вы сможете извлечь образы контейнеров "Распознавание лиц" и (или) "Распознавание текста" из закрытого реестра контейнеров в Реестре контейнеров Azure.

## <a name="prerequisites"></a>Технические условия

Прежде чем использовать контейнеры Azure Cognitive Services, необходимо выполнить указанные ниже предварительные требования.

**Модуль Docker**. Модуль Docker должен быть установлен в локальной среде. Docker предоставляет пакеты для настройки среды с Docker для [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) и [Windows](https://docs.docker.com/docker-for-windows/). В Windows Docker нужно настроить для поддержки контейнеров Linux. Контейнеры Docker можно также развертывать непосредственно в [Службе Azure Kubernetes](../aks/index.yml) или [Экземплярах контейнеров Azure](../container-instances/index.yml).

Docker нужно настроить таким образом, чтобы контейнеры могли подключать и отправлять данные о выставлении счетов в Azure.

**Опыт работы с Реестром контейнеров Майкрософт и Docker**. Требуется базовое представление о Реестре контейнеров Майкрософт и понятиях Docker, о реестрах, репозиториях, контейнерах и образах контейнеров, а также знание основных команд `docker`.

Ознакомьтесь с [общими сведениями о Docker и контейнерах](https://docs.docker.com/engine/docker-overview/).

Для некоторых контейнеров могут действовать дополнительные требования, в том числе к характеристикам сервера и (или) объему памяти.

## <a name="developer-samples"></a>Примеры для разработчиков

Примеры для разработчиков доступны в нашем [репозитории GitHub](https://github.com/Azure-Samples/cognitive-services-containers-samples).

## <a name="next-steps"></a>Дальнейшие действия

Установите и изучите функциональные возможности, предоставляемые контейнерами в Azure Cognitive Services:

* [Установка и использование контейнеров Компьютерного зрения](Computer-vision/computer-vision-how-to-install-containers.md)
* [Установка и использование контейнеров Распознавания лиц](Face/face-how-to-install-containers.md)
* [Установка и использование контейнеров Анализа текста](text-analytics/how-tos/text-analytics-how-to-install-containers.md)
* [Install and run LUIS docker containers](LUIS/luis-container-howto.md) (Установка и запуск контейнеров Docker для LUIS)
