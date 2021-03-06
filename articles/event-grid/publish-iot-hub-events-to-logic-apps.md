---
title: Использование событий Центра Интернета вещей для активации Azure Logic Apps | Документация Майкрософт
description: Создайте автоматизированные процессы для выполнения действий Azure Logic Apps на основе событий в Центре Интернета вещей, используя службу маршрутизации событий "Сетка событий Azure".
services: iot-hub
documentationcenter: ''
author: kgremban
manager: philmea
editor: ''
ms.service: iot-hub
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/07/2018
ms.author: kgremban
ms.openlocfilehash: 9c84e1a62ad8b67e398c62074c390711f4b0be28
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58080003"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-logic-apps"></a>Руководство. Отправка уведомлений электронной почты о событиях в Центре Интернета вещей Azure с помощью Logic Apps

Сетка событий Azure позволяет реагировать на события в Центре Интернета вещей путем запуска действий в подчиненных бизнес-приложениях.

В этой статье рассматривается пример конфигурации, в котором используется Центр Интернета вещей и Сетка событий. По завершении работы с этим руководством у вас будет приложение логики Azure, настроенное на отправку уведомлений электронной почты каждый раз, когда в Центр Интернета вещей добавляется устройство. 

## <a name="prerequisites"></a>Предварительные требования

* Учетная запись электронной почты любого поставщика услуг электронной почты, поддерживаемого Azure Logic Apps, например Office 365 Outlook, Outlook.com или Gmail. Такая учетная запись используется для отправки уведомлений о событиях. Полный список поддерживаемых соединителей для приложения логики см. в статье [Соединители](https://docs.microsoft.com/connectors/).
* Активная учетная запись Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/).
* Центр Интернета вещей в Azure. Если вы его еще не создали, ознакомьтесь с соответствующими инструкциями в статье [Подключение устройства к Центру Интернета вещей с помощью .NET](../iot-hub/iot-hub-csharp-csharp-getstarted.md). 

## <a name="create-a-logic-app"></a>Создайте приложение логики

Сначала создайте приложение логики и добавьте триггер службы "Сетка событий", отслеживающий группу ресурсов для виртуальной машины. 

### <a name="create-a-logic-app-resource"></a>Создание ресурса приложения логики

1. На [портале Azure](https://portal.azure.com) выберите **Создать ресурс** > **Интеграция** > **Приложение логики**.

   ![Создание приложения логики](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

2. Присвойте уникальное в вашей подписке имя приложению логики и выберите ту же подписку, группу ресурсов и расположение, которые используются для Центра Интернета вещей. 
3. Нажмите кнопку **Создать**.

4. Создав ресурс, перейдите к приложению логики. 

5. Конструктор Logic Apps отобразит несколько распространенных шаблонов, чтоб вы могли быстрее приступить к работе. В конструкторе приложений логики в разделе **Шаблоны** выберите **Пустое приложение логики**, чтобы создать приложение с нуля.

### <a name="select-a-trigger"></a>Выбор триггера

Триггер представляет собой определенное событие, которое запускает приложение логики. В этом руководстве триггер, который инициирует рабочий процесс, получает запрос через HTTP.  

1. На панели поиска триггеров и соединителей введите **HTTP**.
2. Выберите в качестве триггера вариант **Запрос — при получении HTTP-запроса**. 

   ![Выбор триггера запроса HTTP](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

3. Выберите **Использовать пример полезной нагрузки, чтобы создать схему**. 

   ![Выбор триггера запроса HTTP](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

4. Вставьте пример кода JSON ниже в текстовое поле, а затем нажмите кнопку **Готово**:

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "deviceEtag": "null",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice"
     },
     "dataVersion": "1",
     "metadataVersion": "1"
   }]
   ```

5. Вы можете получить всплывающее уведомление: **Не забудьте включить заголовок Content-Type со значением application/json в запросе.** Можно спокойно проигнорировать это уведомление и перейти к следующему разделу. 

### <a name="create-an-action"></a>Создание действия

Действия представляют собой любые шаги, которые выполняются после запуска триггером рабочего процесса приложения логики. В этом руководстве действием является отправка уведомления электронной почты с помощью поставщика электронной почты. 

1. Выберите **Новый шаг**. Откроется окно **Выбор действия**.

2. Введите в строку поиска **электронная почта**.

3. Найдите и выберите соединитель, соответствующий поставщику услуг электронной почты. В этом руководстве используется **Office 365 Outlook**. Шаги для других поставщиков услуг электронной почты схожи. 

   ![Выбор соединителя поставщика услуг электронной почты](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

4. Выберите действие **Отправить сообщение электронной почты**. 

5. Если отобразится запрос на вход в учетную запись электронной почты, выполните его. 

6. Создайте шаблон электронной почты. 
   * **Кому**. Введите адрес электронной почты для получения электронных уведомлений. Для работы с этим руководством выберите учетную запись электронной почты, которую можно использовать для тестирования. 
   * **Тема** и **Текст**. Введите текст сообщения электронной почты. В инструменте выбора щелкните свойства JSON для включения динамического содержимого на основе данных событий.  

   Ваш шаблон электронной почты может выглядеть следующим образом:

   ![Заполнение сведений для сообщения электронной почты](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

7. Сохраните приложение логики. 

### <a name="copy-the-http-url"></a>Копирование URL-адреса HTTP

Скопируйте для триггера URL-адрес, который прослушивают приложения логики, а затем закройте конструктор Logic Apps. Этот URL-адрес вы используете для настройки сетки событий. 

1. Щелкните поле конфигурации триггера **При получении HTTP-запроса**, чтобы развернуть его. 
2. Скопируйте значение **URL-адрес HTTP POST**, нажав кнопку копирования рядом с этим параметром. 

   ![Копирование URL-адреса HTTP POST](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

3. Сохраните этот URL-адрес, чтобы использовать его в следующем разделе. 

## <a name="configure-subscription-for-iot-hub-events"></a>Настройка подписки на события Центра Интернета вещей

В этом разделе вы настроите Центр Интернета вещей для публикации событий по мере их появления. 

1. Найдите нужный Центр Интернета вещей на портале Azure. 
2. Выберите **События**.

   ![Открытие сведений сетки событий](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. Выберите **Подписка на события**. 

   ![Создание подписки на события](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Создайте подписку на события со следующими значениями: 
   * **Тип события**. Снимите флажок "Подписаться на все типы событий" и выберите **Устройство создано** в меню.
   * **Сведения о конечной точке**. Выберите для типа конечной точки **Веб-перехватчик**, щелкните выбранную конечную точку, вставьте URL-адрес, скопированный из приложения логики, и подтвердите выбор.

     ![Выбор URL-адреса конечной точки](./media/publish-iot-hub-events-to-logic-apps/endpoint-url.png)

   * **Сведения о подписке на события**. Укажите описательное имя и выберите **Схема Сетки событий**.

   По завершении форма должна выглядеть следующим образом: 

    ![Пример формы подписки на события](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. На этом этапе можно сохранить подписку на события, после чего вы будете получать уведомления о каждом устройстве, созданном в Центре Интернета вещей. При работе с этим руководством мы используем необязательные поля для фильтрации определенных устройств. Выберите **Дополнительные функции** в верхней части формы. 

6. Создайте следующие фильтры:

   * **Тема начинается с**. Введите `devices/Building1_` для фильтрации событий устройств в здании 1.
   * **Тема заканчивается на**. Введите `_Temperature` для фильтрации событий устройств, связанных с температурой.

5. Выберите **Создать**, чтобы сохранить подписку на события.

## <a name="create-a-new-device"></a>Создание устройства

Протестируйте приложение логики, создав устройство для запуска уведомления электронной почты о событии. 

1. В Центре Интернета вещей выберите **IoT Devices** (Устройства Интернета вещей). 
2. Выберите **Добавить**.
3. Для **идентификатора устройства** введите `Building1_Floor1_Room1_Temperature`.
4. Щелкните **Сохранить**. 
5. Вы можете добавить несколько устройств с разными идентификаторами устройств для проверки работы фильтров подписки на события. Используйте примеры ниже: 
   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

Добавив несколько устройств в Центр Интернета вещей, проверьте почту, чтобы просмотреть, какие из них запустили приложение логики. 

## <a name="use-the-azure-cli"></a>Использование Azure CLI

Вместо того чтобы использовать портал Azure, шаги для работы с Центром Интернета вещей можно выполнить с помощью Azure CLI. С дополнительными сведениями можно ознакомиться в статьях, посвященных созданию [подписки на события](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) и [устройств Интернета вещей](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity) с использованием Azure CLI.

## <a name="clean-up-resources"></a>Очистка ресурсов

В этом руководстве используются ресурсы, за которые в подписке Azure предусмотрена плата. По завершении работы с этим руководством и тестирования результатов отключите или удалите ресурсы, которые больше не нужны. 

Чтобы не потерять работу, проделанную в приложении логики, мы рекомендуем отключить ресурсы, а не удалять их. 

1. Перейдите в приложение логики.
2. В колонке **Обзор** выберите **Удалить** или **Отключить**. 

В подписке может быть только один бесплатный Центр Интернета вещей. Если для работы с этим руководством вы создали бесплатный Центр Интернета вещей, его можно не удалять, так как никакая плата взиматься не будет.

1. Перейдите в Центр Интернета вещей. 
2. В колонке **Обзор** выберите **Удалить**. 

Даже если вы не удалите свой Центр Интернета вещей, вы можете удалить созданную подписку на события. 

1. В Центре Интернета вещей выберите **Сетка событий**.
2. Выберите подписку на события, которую нужно удалить. 
3. Нажмите кнопку **Удалить**. 

## <a name="next-steps"></a>Дополнительная информация

* Узнайте больше о [реагировании на события в Центре Интернета вещей, используя службу "Cетка событий" для запуска действий](../iot-hub/iot-hub-event-grid.md).
* См. дополнительные сведения об [упорядочении событий подключения и отключения устройств](../iot-hub/iot-hub-how-to-order-connection-state-events.md).
* Узнайте о дополнительных возможностях службы [Сетка событий](overview.md).


