---
title: Обработчики событий службы "Сетка событий Azure"
description: Описание обработчиков событий, поддерживаемых в службе "Сетка событий Azure"
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: spelluru
ms.openlocfilehash: 33604a16f5895e20d4475d1dd8b27c34184feb72
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478473"
---
# <a name="event-handlers-in-azure-event-grid"></a>Обработчики событий в службе "Сетка событий Azure"

Обработчик событий — это место, куда отправляются события. Обработчик выполняет последующую обработку полученного события. Некоторые службы Azure автоматически настроены для обработки событий. Также для этого можно использовать любой веб-перехватчик. Для обработки событий, веб-перехватчик не обязательно размещать в Azure. Сетка событий поддерживает только конечные точки HTTPS веб-перехватчиков.

В этой статье представлены ссылки на информацию о каждом обработчике событий.

## <a name="azure-automation"></a>Служба автоматизации Azure

Используйте службу автоматизации Azure для обработки событий с помощью автоматизированных модулей runbook.

|Название  |ОПИСАНИЕ  |
|---------|---------|
|[Руководство по службе автоматизации Azure со службой "Сетка событий Azure" и Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Создайте виртуальную машину, которая отправляет событие. Это событие вызывает модуль runbook службы автоматизации, который присваивает виртуальной машине тег и отправляет сообщение в канал Microsoft Teams. |

## <a name="azure-functions"></a>Функции Azure

Используйте службу "Функции Azure" для бессерверного реагирования на события.

При использовании Функций Azure в качестве обработчика используйте триггер службы "Сетка событий" вместо универсальных триггеров HTTP. Служба "Сетка событий" автоматически проверяет триггеры функций сетки событий. При использовании универсальных триггеров HTTP вам нужно реализовать [ответ проверки](security-authentication.md#webhook-event-delivery).

|Название  |ОПИСАНИЕ  |
|---------|---------|
| [Триггер службы "Сетка событий" для Функций Azure](../azure-functions/functions-bindings-event-grid.md) | Общие сведения об использовании триггера службы "Сетка событий" в Функциях Azure. |
| [Руководство. Автоматическое изменение размера переданных изображений с помощью службы "Сетка событий"](resize-images-on-storage-blob-upload-event.md) | Пользователи отправляют изображения в учетную запись хранения через веб-приложение. При создании большого двоичного объекта в хранилище служба "Сетка событий" отправляет событие приложению-функции, которое изменяет размер отправленного изображения. |
| [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md) | Когда Центры событий создают файл сбора, служба "Сетка событий" отправляет событие приложению-функции. Это приложение извлекает файл сбора и переносит данные в хранилище данных. |
| [Руководство с примерами интеграции служебной шины Azure со службой "Сетка событий Azure"](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Служба "Сетка событий" отправляет сообщения из раздела служебной шины в приложение-функцию и приложение логики. |

## <a name="event-hubs"></a>Центры событий;

Центры событий можно использовать, если решение получает больше событий, чем может обработать. Приложение обрабатывает события из Центров событий по собственному расписанию. Вы можете расширить параметры обработки входящих событий.

Центры событий могут действовать либо как источник события, либо как обработчик события. В следующей статьи показано, как использовать Центры событий в качестве обработчика.

|Название  |ОПИСАНИЕ  |
|---------|---------|
| [Краткое руководство. Перенаправление пользовательских событий в концентраторы событий Azure с помощью Azure CLI и службы "Сетка событий"](custom-event-to-eventhub.md) | Отправляет пользовательское событие в концентратор событий для обработки приложением. |
| [Шаблон Resource Manager для создания пользовательского раздела и конечной точки Центров событий](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Шаблон Resource Manager, который создает подписку для пользовательского раздела. Он отправляет события в Центры событий Azure. |

Примеры Центров событий в качестве источника, см. в [источнике Центров событий](event-sources.md#event-hubs).

## <a name="hybrid-connections"></a>через гибридные подключения

Гибридные подключения Azure Relay используются для отправки событий в приложения, которые находятся в корпоративной сети и не имеют общедоступной конечной точки.

|Название  |ОПИСАНИЕ  |
|---------|---------|
| [Руководство. Отправка пользовательских событий для службы Сетка событий Azure по гибридному подключению](custom-event-to-hybrid-connection.md) | Отправка пользовательских событий в существующее гибридное подключение для обработки приложением прослушивателя. |

## <a name="logic-apps"></a>Logic Apps

Используйте Logic Apps для автоматизации бизнес-процессов реагирования на события.

|Название  |ОПИСАНИЕ  |
|---------|---------|
| [Отслеживание изменений виртуальной машины с помощью Azure Logic Apps и службы "Сетка событий Azure"](monitor-virtual-machine-changes-event-grid-logic-app.md) | Приложение логики отслеживает изменения в виртуальной машине и отправляет сообщения электронной почты об этих изменениях. |
| [Отправка уведомлений электронной почты о событиях в Центре Интернета вещей Azure с помощью Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Приложение логики отправляет уведомление по электронной почте каждый раз, когда добавляется устройство в Центр Интернета вещей. |
| [Руководство с примерами интеграции служебной шины Azure со службой "Сетка событий Azure"](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Служба "Сетка событий" отправляет сообщения из раздела служебной шины в приложение-функцию и приложение логики. |

## <a name="queue-storage"></a>Хранилище очередей

Используйте хранилище очередей для получения событий, которые должны извлекаться. Вы можете использовать хранилище очередей при наличии процесса выполнения, для реагирования которого необходимо много времени. Отправляя события в хранилище очередей, приложение может запрашивать и обрабатывать события по собственному расписанию.

|Название  |ОПИСАНИЕ  |
|---------|---------|
| [Краткое руководство. Перенаправление пользовательских событий в хранилище очередей Azure с помощью Azure CLI и службы "Сетка событий"](custom-event-to-queue-storage.md) | В этой статье объясняется, как отправлять пользовательские события в хранилище очередей. |

## <a name="webhooks"></a>Веб-перехватчики

Используйте веб-перехватчики для создания настраиваемых конечных точек, реагирующих на события.

|Название  |ОПИСАНИЕ  |
|---------|---------|
| Краткое руководство. Создание и перенаправление пользовательских событий с помощью [Azure CLI](custom-event-quickstart.md), [PowerShell](custom-event-quickstart-powershell.md) и [портала Azure](custom-event-quickstart-portal.md). | Содержит сведения об отправке пользовательских событий в веб-перехватчик. |
| Краткое руководство. Перенаправление событий хранилища BLOB-объектов в пользовательскую конечную веб-точку с помощью [Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json), [PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json), и [портала Azure](blob-event-quickstart-portal.md). | Содержит сведения об отправке событий хранилища BLOB-объектов в веб-перехватчик. |
| [Быстрое руководство по отправке событий реестра контейнеров](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Содержит сведения об отправке событий Реестра контейнеров с помощью Azure CLI. |
| [Общие сведения. Получение событий через конечную точку HTTP](receive-events.md) | В этой статье описывается, как проверить получение событий на конечную точку HTTP из подписки на события, а также как получать события и десериализировать их. |

## <a name="next-steps"></a>Дополнительная информация

* Общие сведения о службе "Сетка событий" см. в разделе [Общие сведения о службе "Сетка событий Azure"](overview.md).
* Сведения о том, как быстро приступить к использованию службы "Сетка событий", см. в разделе [Создание и перенаправление пользовательского события со службой "Сетка событий Azure"](custom-event-quickstart.md).
