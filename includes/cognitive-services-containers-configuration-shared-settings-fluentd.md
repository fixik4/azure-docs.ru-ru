---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: ffd17f7a641e1481aa4c88f8b2eb12ec11fa7d8b
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741252"
---
Fluentd — это сборщик данных с открытым кодом для унифицированного ведения журнала. Параметры `Fluentd` управляют подключением контейнера к серверу [Fluentd](https://www.fluentd.org). В состав контейнера входит поставщик ведения журнала Fluentd, который позволяет контейнеру записывать данные журнала и (необязательно) данные метрик на сервер Fluentd.

В следующей таблице описаны параметры конфигурации, поддерживаемые в разделе `Fluentd`.

| ИМЯ | Тип данных | ОПИСАНИЕ |
|------|-----------|-------------|
| `Host` | Строка | IP-адрес или имя узла DNS сервера Fluentd. |
| `Port` | Целое число  | Порт сервера Fluentd.<br/> Значение по умолчанию — 24224. |
| `HeartbeatMs` | Целое число  | Интервал пульса в миллисекундах. Если до окончания этого интервала не отправлялся никакой трафик событий, пульс отправляется на сервер Fluentd. Значение по умолчанию — 60 000 миллисекунд (1 минута). |
| `SendBufferSize` | Целое число  | Место в сетевом буфере (в байтах), выделенное для операций отправки. Значение по умолчанию — 32768 байт (32 килобайта). |
| `TlsConnectionEstablishmentTimeoutMs` | Целое число  | Время ожидания (в миллисекундах) до установки соединения по протоколу SSL/TLS с сервером Fluentd. Значение по умолчанию — 10 000 миллисекунд (10 секунд).<br/> Если для параметра `UseTLS` задано значение false, то это значение игнорируется. |
| `UseTLS` | Логическое | Указывает, должен ли контейнер использовать протокол SSL/TLS для связи с сервером Fluentd. По умолчанию для этого параметра используется значение false. |