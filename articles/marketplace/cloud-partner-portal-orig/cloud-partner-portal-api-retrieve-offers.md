---
title: Получение предложений API | Документация Майкрософт
description: API получает сводный список предложений в пространстве имен издателя.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: de9261548ec79e206b0db87caabc1fa4c9ad6771
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56591556"
---
<a name="retrieve-offers"></a>Получение предложений
===============

Получить сводный список предложений в пространстве имен издателя.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`


<a name="uri-parameters"></a>Параметры универсального кода ресурса (URI)
--------------

| **Имя**         |  **Описание**                         |  **Тип данных** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherid     | Идентификатор издателя, например `contoso` |   Строка    |
|  api-version     | API последней версии                    |    Дата        |
|  |  |


<a name="header"></a>Заголовок
------

|  **Имя**        |         **Значение**       |
|  --------------- |       ----------------  |
|  Content-Type    | `application/json`      |
|  Авторизация   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>Пример текста запроса
------------

### <a name="response"></a>Ответ

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Свойства текста ответа

|  **Имя**       |       **Описание**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | Определяет тип предложения                                                                                           |
|  publisherid    | Идентификатор, который определяет издателя.                                                                      |
|  status         | Состояние предложения. Список возможных значений, см. ниже в разделе [Состояние предложения](#offer-status).                         |
|  id             | Идентификатор GUID, который определяет предложения в пространстве имен издателя.                                                    |
|  версия        | Текущая версия предложения. Клиент не может изменить свойство версии. Оно увеличивается после каждой публикации. |
|  Определение     | Содержит итоговое представление о фактическом определении рабочей нагрузки. Подробное определение можно получить с помощью API для [извлечения сведений о конкретном предложении](./cloud-partner-portal-api-retrieve-specific-offer.md). |
|  changedTime    | Время последнего изменения предложения.                                                                              |
|  |  |


### <a name="response-status-codes"></a>Коды состояния ответа

| **Код**  |  **Описание**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK` — запрос успешно обработан, и все предложения в разделе издателя были возвращены клиенту.  |
|  400      | `Bad/Malformed request` — текст ответа ошибки может содержать дополнительные сведения.                                    |
|  403      | `Forbidden` — клиент не имеет доступ к указанным пространствам имен.                                          |
|  404      | `Not found` — указанная сущность не существует.                                                                 |
|  |  |


### <a name="offer-status"></a>Состояние предложения

|  **Имя**                    | **Описание**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | Предложение не было опубликовано.                  |
|  NotStarted                  | Предложения — новое, но не запущенное.                 |
|  WaitingForPublisherReview   | Предложение ожидает утверждения издателя.         |
|  Выполнение                     | Отправка предложения обрабатывается.             |
|  Succeeded                   | Отправка предложения прекратила обработку.       |
|  Canceled                    | Отправка предложения была отменена.                   |
|  Сбой                      | Не удалось отправить предложение.                         |
|  |  |
