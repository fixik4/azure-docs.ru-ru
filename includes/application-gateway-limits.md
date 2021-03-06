---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 3/26/2019
ms.author: victorh
ms.openlocfilehash: 5ad1339c04444bcb4cc550be26e239e65227d2ce
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2019
ms.locfileid: "58495229"
---
| Ресурс | Ограничение по умолчанию | Примечание |
| --- | --- | --- |
| Шлюз приложений Azure |1000 на подписку | |
| Интерфейсные IP-конфигурации |2 |1 открытая и 1 закрытая |
| Интерфейсные порты |100<sup>1</sup> | |
| Серверные пулы адресов |100<sup>1</sup> | |
| Количество внутренних серверов на пул |1200 | |
| HTTP-прослушиватели |100<sup>1</sup> | |
| Правила балансировки нагрузки HTTP |100<sup>1</sup> | |
| Параметры HTTP для сервера |100<sup>1</sup> | |
| Экземпляров на шлюз |32 | |
| SSL-сертификаты |100<sup>1</sup> |1 на HTTP-прослушиватель |
| Сертификаты аутентификации клиента |100 | |
| Доверенные корневые сертификаты |100 | |
| Минимальное время ожидания запроса |1 с | |
| Максимальное время ожидания запроса |24 часа | |
| Количество сайтов |100<sup>1</sup> |1 на HTTP-прослушиватель |
| Количество сопоставлений URL-адреса на прослушиватель |1 | |
| Максимальное количество правил на основе путей на сопоставление URL-адреса|100||
| Конфигурации перенаправления |100<sup>1</sup>| |
| Одновременные подключения WebSocket |Шлюзы средних 20 КБ<br> Большой шлюзы 50 тысяч| |
| Максимальная длина URL-адреса|8000||
| Максимальный размер передаваемого файла, Standard |2 ГБ | |
| Максимальный размер передаваемого файла (WAF) |Шлюзы средних WAF, 100 МБ<br>Большие WAF шлюзы, 500 МБ| |
| Предельный размер текста, WAF, без файлов|128 КБ||

<sup>1</sup> для номеров SKU с поддержкой WAF, рекомендуется ограничить количество ресурсов до 40 для обеспечения оптимальной производительности.
