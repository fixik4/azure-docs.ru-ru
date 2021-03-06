---
title: Планирование среды "Аналитика временных рядов Azure" (предварительная версия) | Документация Майкрософт
description: Запланируйте среду службы"Аналитика временных рядов Azure (предварительная версия)".
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 251e95744f57d9b5e42df9bdc3743f4880ff5381
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58077002"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Запланируйте среду службы"Аналитика временных рядов Azure (предварительная версия)"

В этой статье описываются рекомендации по быстрому планированию и началу работы с помощью службы "Аналитика временных рядов Azure (предварительная версия)".

## <a name="best-practices-for-planning-and-preparation"></a>Рекомендации для планирования и подготовки

Чтобы приступить к работе со службой "Аналитика временных рядов Azure", необходимо понимать:

* Что вы получите, предоставив среду службы "Аналитика временных рядов Azure (предварительная версия)".
* Каковы ваши идентификаторы временных рядов и свойства метки времени.
* Что такое новая модель временных рядов, и как создать свою собственную.
* Как эффективно отправлять события в JSON. 
* Опции непрерывности бизнес-процессов и аварийного восстановления службы "Аналитика временных рядов Azure".

Служба "Аналитика временных рядов Azure" использует бизнес-модель оплаты по мере использования. Дополнительные сведения о расходах и производительности см. в статье [Цены на Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-time-series-insights-preview-environment"></a>Среда службы "Аналитика временных рядов (предварительная версия)"

При подготовке среды "Аналитика временных рядов (предварительная версия)" создаются два ресурса Azure:

* Среда службы "Аналитика временных рядов (предварительная версия)"
* Учетная запись службы хранилища Azure общего назначения версии 1

Для начала работы вам понадобятся три дополнительных элемента:
 
- [Модель временных рядов](./time-series-insights-update-tsm.md) 
- [Источник событий, подключенный к службе "Аналитика временных рядов Azure"](./time-series-insights-how-to-add-an-event-source-iothub.md) 
- [События, поступающие в источник событий ](./time-series-insights-send-events.md), которые сопоставлены с моделью и имеют допустимый формат JSON 

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>Настройка идентификаторов временных рядов и свойств метки времени

Чтобы создать новую среду службы "Аналитика временных рядов", выберите идентификатор временных рядов. Это действует как логический раздел для ваших данных. Как уже говорилось, убедитесь в том, что у вас есть готовые идентификаторы временных рядов.

> [!IMPORTANT]
> Идентификаторы временных рядов являются *неизменным*  и *не могут быть изменены позже*. Перед окончательным выбором и первым использованием проверьте каждый их них.

Чтобы дифференцировать ваши ресурсы уникальным образом, вы можете выбрать до трех (3) ключей. Дополнительные сведения см. в статьях [Best practices for choosing a Time Series ID](./time-series-insights-update-how-to-id.md) (Рекомендации при выборе идентификатора временного ряда) и [Storage and ingress](./time-series-insights-update-storage-ingress.md) (Хранение и доступ к данным в службе "Аналитика временных рядов Azure (предварительная версия)").

Свойство метки времени также является важным. Вы можете назначить это свойство при добавлении источников событий. Каждый источник событий имеет дополнительное свойство метки времени, которое используется для отслеживания источников событий с течением времени. Значения метки времени учитывают регистр и должны быть отформатированы в соответствии с индивидуальной спецификацией каждого источника события.

> [!TIP]
> Проверьте требования к форматированию и анализу для ваших источников событий.

Если оставить значение пустым, в качестве метки времени события будет использоваться время постановки события в очередь в пределах источника событий. Если вы отправляете данные истории или пакетные события, настройка свойства метки времени более полезна, чем время постановки события в очередь по умолчанию. Дополнительные сведения см. в статье [Как добавить в среду службы "Аналитика временных рядов Azure" источник событий Центра Интернета вещей](./time-series-insights-how-to-add-an-event-source-iothub.md). 

## <a name="understand-the-time-series-model"></a>Понимание модели временного ряда

Теперь вы можете настроить модель временных рядов среды службы "Аналитика временных рядов Azure". Новая модель позволяет легко находить и анализировать данные Центра Интернета вещей. Это позволяет осуществлять обработку, обслуживание и обогащение данных временных рядов и помогает подготовить наборы данных готовые для потребителей. Модель использует **идентификаторы временных рядов**, которые сопоставляются с экземпляром, который связывает уникальный ресурс с переменными, известными как типы, и иерархиями. Прочтите о новой [модели временных рядов](./time-series-insights-update-tsm.md).

Модель является динамичной, поэтому ее можно построить в любое время. Чтобы быстро приступить к работе, создайте и загрузите ее до загрузки данных в службу "Аналитика временных рядов Azure". Чтобы построить модель, см. статью [Use the Time Series Model](./time-series-insights-update-how-to-tsm.md) (Использование модели временных рядов).

Для многих клиентов модель временных рядов сопоставляется с уже существующей моделью активов или системой ERP. Если у вас нет существующей модели, [предоставляется](https://github.com/Microsoft/tsiclient) предварительно созданный пользовательский интерфейс для быстрого запуска и выполнения. Чтобы представить, как модель может вам помочь, посмотрите [демонстрационный пример среды](https://insights.timeseries.azure.com/preview/demo). 

## <a name="shape-your-events"></a>Формирование ваших событий

Вы можете проверить способ отправки событий в службу "Аналитика временных рядов Azure". В идеале, ваши мероприятия денормализуются хорошо и эффективно.

Хорошее правило:

* Храните метаданные в вашей модели временных рядов
* Модель временных рядов, поля экземпляров и события содержат только необходимую информацию, такую как:
  * Идентификатор временного ряда
  * Timestamp

Дополнительные сведения см. в статье [Отправка событий в среду Time Series Insights через концентратор событий](./time-series-insights-send-events.md#json).

## <a name="business-disaster-recovery"></a>Непрерывность бизнес-процессов и аварийное восстановление

"Аналитика временных рядов Azure" — это служба высокой доступности, которая использует избыточности на уровне региона Azure. Для использования этих присущих функций не требуется конфигурация. Платформа Microsoft Azure также включает в себя ряд возможностей для создания решений с функциями аварийного восстановления или межрегиональной доступностью. Чтобы обеспечить глобальную высокую доступность между регионами для устройств или пользователей, используйте эти функции аварийного восстановления Azure. 

Дополнительные сведения о встроенных функциях Azure для обеспечения непрерывности бизнеса и аварийного восстановления (BCDR) см. в статье [Проектирование устойчивых приложений для Azure](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance). Руководство по архитектуре стратегий для приложений Azure для обеспечения высокой доступности и аварийного восстановления см. в документе [Проектирование устойчивых приложений для Azure](https://docs.microsoft.com/azure/architecture/resiliency/index).

> [!NOTE]
> 
>  Служба "Аналитика временных рядов Azure" не имеет встроенного BCDR.
> По умолчанию в службах хранилища Microsoft Azure, концентратор Центра Интернета вещей Azure и концентраторы событий Azure встроено восстановление.

Чтобы узнать больше, ознакомьтесь со статьями:

* [Репликация службы хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Высокая доступность и аварийное восстановление Центра Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr)
* [Геоизбыточное аварийное восстановление в службе "Центры событий Azure"](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)

Если вам требуется BCDR, вы можете реализовать стратегию восстановления. Создайте вторую среду службы "Аналитика временных рядов Azure" в резервной области Azure. Отправляйте события в эту вторичную среду из основного источника событий. Используйте вторую выделенную группу потребителей и BCDR-рекомендации источника событий.

Выполните следующие действия, чтобы создать и использовать вторичную среду службы "Аналитика временных рядов Azure".

1. Создайте среду во втором регионе. Дополнительные сведения см. в статье [Создание среды Time Series Insights на портале Azure](./time-series-insights-get-started.md).
1. Создайте для вашего источника событий вторую выделенную группу потребителей. Подключите этот источник событий к новой среде. Обязательно назначьте вторую выделенную группу потребителей. Дополнительные сведения см. в статье [Как добавить в среду службы "Аналитика временных рядов Azure" источник событий Центра Интернета вещей](./time-series-insights-how-to-add-an-event-source-iothub.md) или [Предоставление доступа к данным для среды Time Series Insights с помощью портала Azure](./time-series-insights-data-access.md).
1. Если во время инцидента ваш основной регион дал сбой, переключите операции на резервную среду службы "Аналитика временных рядов".

> [!IMPORTANT]
> * Обратите внимание, что задержка может возникнуть в случае сбоя.
> * Аварийное переключение также может вызвать кратковременный всплеск обработки сообщений при перенаправлении операций.
> * Дополнительные сведения см. в статье [Отслеживание и уменьшение регулирования для сокращения задержек в службе "Аналитика временных рядов Azure"](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать больше, ознакомьтесь со статьями:

- [Data storage and ingress in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md) (Хранилище данных и входящий трафик в службе "Аналитика временных рядов Azure (предварительная версия)")
- [Data modeling](./time-series-insights-update-tsm.md) (Моделирование данных)