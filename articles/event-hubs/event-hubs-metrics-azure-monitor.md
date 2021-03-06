---
title: Метрики в Azure Monitor — Центры событий Azure | Документация Майкрософт
description: В этой статье описывается, как использовать приложение Azure Monitor для мониторинга Центров событий Azure.
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: b5df69e9670c01b576afe242b39532acb1e1c526
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2019
ms.locfileid: "58849372"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Метрики Центров событий Azure в Azure Monitor

Метрики Центров событий предоставляют состояние ресурсов Центров событий в подписке Azure. Используя обширный набор данных метрик, можно оценить общую работоспособность концентраторов событий не только на уровне пространства имен, но также и на уровне сущности. Эти статистические данные могут быть важны, так как они позволяют отслеживать состояние концентраторов событий. Кроме того, метрики могут помочь в устранении первопричин проблем без необходимости обращения в службу поддержки Azure.

Azure Monitor предоставляет унифицированный пользовательский интерфейс для мониторинга различных служб Azure. Дополнительные сведения см. в разделе [Обзор мониторинга в Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md). Ознакомьтесь также с примером [получения данных метрик Azure Monitor с помощью .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) на сайте GitHub.

## <a name="access-metrics"></a>Доступ к метрикам

Azure Monitor предоставляет несколько способов доступа к метрикам. Вы можете использовать [портал Azure](https://portal.azure.com), интерфейсы API Azure Monitor (REST и .NET), а также решения для анализа, например Operation Management Suite и Центры событий. Дополнительные сведения см. в разделе [Мониторинг данных, собранных Azure Monitor](../azure-monitor/platform/data-platform.md).

Метрики включены по умолчанию, и вы можете получить доступ к данным за последние 30 дней. Если необходимо хранить данные метрик в течение длительного периода времени, вы можете архивировать их в учетную запись хранения Azure. Для настройки используйте [параметры диагностики](../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings) в Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Доступ к метрикам на портале

На [портале Azure](https://portal.azure.com) можно отслеживать метрики в течение продолжительного периода времени. В приведенном ниже примере показано, как просмотреть успешные запросы и входящие запросы на уровне учетной записи.

![Просмотр метрик успешного выполнения][1]

Можно также открыть метрики непосредственно с помощью пространства имен. Чтобы сделать это, выберите нужное пространство имен и нажмите кнопку **метрики**. Чтобы отобразить метрики, отфильтрованные по концентратору событий, выберите концентратор событий и щелкните **Метрики**.

Для метрик с поддержкой измерений необходимо выполнить фильтрацию по нужному значению измерения, как показано в примере ниже.

![Фильтрация по значению измерения][2]

## <a name="billing"></a>Выставление счетов

Использование метрик в Azure Monitor пока бесплатно. Тем не менее, если вы используете дополнительные решения для приема данных метрик, за использование этих решений может взиматься плата. Например, при архивации данных метрик в учетную запись хранения Azure плата будет взиматься службой хранилища Azure, Плата будет взиматься с Azure при потоковой передаче данных метрик в Azure Monitor журналы для расширенного анализа.

Приведенные ниже метрики позволяют получить общие сведения о работоспособности службы. 

> [!NOTE]
> Несколько метрик стали неподдерживаемыми, так как теперь они доступны под другим именем. Это может потребовать обновления ссылок. Метрики, помеченные ключевым словом "нерекомендуемая", больше не будут поддерживаться.

Все значения метрик ежеминутно отправляются в Azure Monitor. Степень детализации определяет интервал времени, за который представлены значения метрик. Поддерживаемый интервал времени для всех метрик Центров событий составляет 1 минуту.

## <a name="request-metrics"></a>Метрики запросов

Подсчитывают объем данных и количество запросов операций управления.

| Имя метрики | ОПИСАНИЕ |
| ------------------- | ----------------- |
| Incoming Requests (preview) | Число запросов к службе Центров событий Azure за указанный период. <br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName |
| Successful Requests (preview)   | Число успешных запросов к службе Центров событий Azure за указанный период. <br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName |
| Server Errors (preview) | Число запросов, которые не были обработаны из-за ошибки в службе Центров событий Azure, за указанный период. <br/><br/>Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName |
|User Errors (preview)|Количество запросов, не обработанных из-за ошибки пользователя, за указанный период.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Quota Exceeded Errors (preview)|Количество запросов, превысивших доступную квоту. Дополнительные сведения о квотах Центров событий см. в [этой статье](event-hubs-quotas.md).<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|

## <a name="throughput-metrics"></a>Метрики пропускной способности

| Имя метрики | ОПИСАНИЕ |
| ------------------- | ----------------- |
|Throttled Requests (preview)|Число запросов, которые были отрегулированы из-за превышения квоты единиц пропускной способности.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|

## <a name="message-metrics"></a>Метрики использования

| Имя метрики | ОПИСАНИЕ |
| ------------------- | ----------------- |
|Incoming Messages (preview)|Количество событий или сообщений, отправленных в Центры событий, за указанный период.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Outgoing Messages (preview)|Количество событий или сообщений, полученных из Центров событий, за указанный период.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Incoming Bytes (preview)|Число байтов, переданных в службу Центров событий Azure, за указанный период.<br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Outgoing Bytes (preview)|Число байтов, полученных от службы Центров событий Azure, за указанный период.<br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|

## <a name="connection-metrics"></a>Метрики подключения

| Имя метрики | ОПИСАНИЕ |
| ------------------- | ----------------- |
|ActiveConnections (preview)|Число активных подключений для пространства имен, а также для сущности.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Connections Opened (preview)|Число открытых подключений.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Connections Closed (preview)|Число закрытых подключений.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|

## <a name="event-hubs-capture-metrics"></a>Метрики функции "Сбор" в Центрах событий

Можно отслеживать метрики функции "Сбор" в Центрах событий, если для ваших концентраторов событий включена эта функция. Приведенные ниже метрики описывают возможности отслеживания с помощью функции записи.

| Имя метрики | ОПИСАНИЕ |
| ------------------- | ----------------- |
|Capture Backlog (Preview)|Число байтов, которые будут записаны в выбранное назначение.<br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Captured Messages (Preview)|Число сообщений или событий, записанных в выбранное назначение, за указанный период.<br/><br/> Единица измерения: Количество <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|
|Captured Bytes (Preview)|Число байтов, записанных в выбранное назначение, за указанный период.<br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Итого <br/> Измерение: EntityName|

## <a name="metrics-dimensions"></a>Измерения метрик

Центры событий Azure поддерживают следующие измерения метрик в Azure Monitor. Добавление измерений для метрик является необязательным. Если не добавить измерения, то метрики указываются на уровне пространства имен. 

| Имя метрики | ОПИСАНИЕ |
| ------------------- | ----------------- |
|EntityName| Центры событий поддерживают сущности концентраторов событий в пространстве имен.|

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь с разделом [Обзор мониторинга в Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md).
* Изучите пример [получения данных метрик Azure Monitor с помощью .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) на сайте GitHub. 

Дополнительные сведения о Центрах событий см. по следующим ссылкам:

* Начало работы с помощью [учебника по Центрам событий](event-hubs-dotnet-standard-getstarted-send.md)
* [Часто задаваемые вопросы о Центрах событий](event-hubs-faq.md)
* [Примеры приложений, использующих Центры событий](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



