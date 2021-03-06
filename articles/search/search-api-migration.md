---
title: Обновление REST API службы "Поиск Azure" до последней версии — служба "Поиск Azure"
description: Просмотрите различия в версиях API и узнайте, какие действия необходимы для переноса имеющегося кода в последнюю версию REST API службы "Поиск Azure".
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 23003859b9a75fb986fe65f5528004f3dd150f9b
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633050"
---
# <a name="upgrade-to-the-latest-azure-search-service-rest-api-version"></a>Обновление REST API службы "Поиск Azure" до последней версии
Если вы используете предыдущую версию [REST API службы "Поиск Azure"](https://docs.microsoft.com/rest/api/searchservice/), эта статья поможет вам обновить приложения для использования следующей общедоступной версии API 2017-11-11.

Версия 2017-11-11 REST API содержит некоторые изменения из более ранних версий. Эти отличия в основном обратно совместимы, поэтому изменение кода потребует минимальных усилий в зависимости от версии, которая использовался ранее. В разделе [Действия по обновлению](#UpgradeSteps) вы найдете инструкции о том, как изменить код для использования новой версии API.

> [!NOTE]
> Экземпляр службы поиска Azure поддерживает несколько версий REST API, включая последнюю. Можно продолжать использовать версию, которая больше не является последней, но рекомендуется выполнить перенос кода, чтобы использовать последнюю версию.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2017-11-11"></a>Новые возможности в версии 2017-11-11
Версия 2017-11-11 — это последний общедоступный выпуск REST API службы "Поиск Azure". Новые возможности в этой версии API:

* [Синонимы](search-synonyms.md). Новая функция синонимов позволяет определить эквивалентные термины и расширить область действия запроса.
* [Поддержка эффективной индексации текстовых больших двоичных объектов](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#IndexingPlainText). Новый режим синтаксического анализа `text` для индексаторов больших двоичных объектов Azure значительно повышает производительность индексирования.
* [API статистики службы](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics). Просмотрите текущее использование и ограничения ресурсов в службе "Поиск Azure" с помощью этого нового API.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Действия по обновлению
При обновлении с общедоступной версии 2015-02-28 или 2016-09-01 вам, вероятно, понадобится изменить в коде только номер версии. Изменение кода может потребоваться только в следующих случаях:

* Код завершается ошибкой, если в ответе API возвращаются нераспознанные свойства. По умолчанию приложение должно игнорировать свойства, которые не распознаются.
* Код сохраняет запросы API и пытается повторно отправить их в новую версию API. Например, это может произойти, если приложение сохраняет маркеры продолжения, возвращенные API поиска (см. сведения об `@search.nextPageParameters` в [справочнике по API поиска](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Если одна из этих ситуаций относится к вам, может потребоваться изменить код соответствующим образом. В противном случае вам понадобится вносить изменения, только если вы хотите использовать [новые возможности](#WhatsNew) версии 2017-11-11.

Тоже самое относится и к обновлению с предварительной версии API. Однако следует учитывать, что некоторые возможности предварительной версии недоступны в версии 2017-11-11:

* Поддержка индексатора хранилища BLOB-объектов Azure для CSV-файлов и больших двоичных объектов, содержащих массивы JSON.
* Запросы "Скорее всего".

Если код использует эти возможности, вы не сможете выполнить обновление до версии 2017-11-11, если не отключите их.

> [!IMPORTANT]
> Помните, что предварительные версии API предназначены для тестирования и ознакомления. Они не должны использоваться в рабочей среде.
> 
> 

## <a name="conclusion"></a>Заключение
Дополнительные сведения об использовании REST API службы поиска Azure см. в [справочнике по API](https://docs.microsoft.com/rest/api/searchservice/) на сайте MSDN.

Будем рады вашим отзывам о службе поиска Azure. Если вы столкнулись с проблемами, то всегда можете обратиться за помощью на [форуме по службе поиску Azure на сайте MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) или [StackOverflow](https://stackoverflow.com/). Если вы задаете вопрос о службе поиска Azure на сайте StackOverflow, обязательно добавьте к нему `azure-search`.

Благодарим вас за использование поиска Azure!

