---
title: Разработка модулей для Azure IoT Edge | Документация Майкрософт
description: Разработка пользовательских модулей для Azure IoT Edge, способных взаимодействовать со средой выполнения и Центром Интернета вещей
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: ea2d1dbcf991ce858f07905bc1bb41809f9bcdd8
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2019
ms.locfileid: "56882745"
---
# <a name="develop-your-own-iot-edge-modules"></a>Разработка собственных модулей IoT Edge

Модули Azure IoT Edge можно подключить к другим службам Azure, чтобы расширить свой облачный конвейер данных. В этой статье описывается, как можно разрабатывать модули для взаимодействия со средой выполнения IoT Edge и Центром Интернета вещей, то есть с остальной частью облака Azure. 

## <a name="iot-edge-runtime-environment"></a>Среда выполнения IoT Edge
Среда выполнения IoT Edge предоставляет инфраструктуру для интеграции функциональных возможностей нескольких модулей IoT Edge и их развертывания на пограничных устройствах Интернета вещей. На высоком уровне любую программу можно упаковать в виде модуля IoT Edge. Однако, чтобы воспользоваться всеми преимуществами взаимодействия и управления IoT Edge, программу, работающую в модуле, нужно подключить к локальному концентратору IoT Edge, интегрированному в среде выполнения IoT Edge.

## <a name="using-the-iot-edge-hub"></a>Использование концентратора IoT Edge
Концентратор IoT Edge предоставляет две основные функции: прокси-сервер для Центра Интернета вещей и локальные подключения.

### <a name="iot-hub-primitives"></a>Основные сведения о Центре Интернета вещей
Центр Интернета вещей рассматривает экземпляр модуля как устройство, в том смысле, что он:

* имеет двойник модуля, который является уникальным и изолирован от [двойника устройства](../iot-hub/iot-hub-devguide-device-twins.md) и других двойников модуля этого устройства;
* может отправлять [сообщения с устройства в облако](../iot-hub/iot-hub-devguide-messaging.md);
* может получать [прямые методы](../iot-hub/iot-hub-devguide-direct-methods.md), специально предназначенные для него.

Сейчас модуль не может получать сообщения из облака на устройство или использовать функцию передачи файлов.

При записи модуля можно воспользоваться [пакетом SDK для устройств Azure IoT](../iot-hub/iot-hub-devguide-sdks.md), чтобы подключиться к концентратору IoT Edge и получить доступ к описанным выше возможностям, как и при использовании Центра Интернета вещей с приложением устройства. Единственная разница заключается в том, что из серверной части вашего приложения необходимо ссылаться на идентификатор модуля, а не устройства.

### <a name="device-to-cloud-messages"></a>Отправка сообщений с устройства в облако
Чтобы разрешить сложную обработку сообщений с устройства в облако, центр IoT Edge предоставляет декларативную маршрутизацию сообщений между самими модулями и модулями и Центром Интернета вещей. Декларативная маршрутизация позволяет модулям перехватывать и обрабатывать сообщения, отправленные другими модулями, и распространять их в сложные конвейеры. Дополнительные сведения см. в разделе [Развертывание модулей и объявление маршрутов в IoT Edge](module-composition.md).

Модуль IoT Edge, в отличие от обычного приложения устройства Центра Интернета вещей, может получать сообщения с устройства в облако, которые передаются его локальному центру IoT Edge для обработки.

Центр IoT Edge передает сообщения в ваш модуль на основе декларативных маршрутов, описанных в [манифесте развертывания](module-composition.md). При разработке модуля IoT Edge вы можете получать эти сообщения, настроив обработчики сообщений.

Для упрощения создания маршрутов IoT Edge добавляет понятие *входных* и *выходных* конечных точек модуля. Модуль может получать и отправлять все сообщения с устройства в облако, предназначенные для него, не указывая входные и выходные данные (соответственно). Использование явных входов и выходов упрощает правила маршрутизации. 

Сообщения с устройства в облако, обрабатываемые концентратором Edge, имеют следующие свойства системы:

| Свойство | ОПИСАНИЕ |
| -------- | ----------- |
| $connectionDeviceId | Идентификатор устройства клиента, отправившего сообщение. |
| $connectionModuleId | Идентификатор модуля, отправившего сообщение. |
| $inputName | Входная конечная точка, получившая это сообщение. Это свойство может быть пустым. |
| $outputName | Выходная конечная точка, используемая для отправки сообщения. Это свойство может быть пустым. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Подключение к концентратору IoT Edge из модуля
Подключение к локальному центру IoT Edge из модуля состоит из двух этапов: 
1. Создайте экземпляр ModuleClient в приложении.
2. Убедитесь, что приложение принимает сертификат, предоставленный центром IoT Edge на этом устройстве.

Создайте экземпляр ModuleClient для подключения модуля к центру IoT Edge, запущенному на устройстве, аналогично тому, как экземпляры DeviceClient подключают устройства Интернета вещей к Центру Интернета вещей. Дополнительные сведения о классе ModuleClient и его методах обмена данными см. в справочнике по API для предпочитаемого языка пакета SDK: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C и Python](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable) или [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).


## <a name="next-steps"></a>Дальнейшие действия

[Подготовьте разработки и тестовой среды для IoT Edge](development-environment.md)

[Используйте Visual Studio 2017 для разработки C# модули Edge Интернета вещей](how-to-visual-studio-develop-csharp-module.md)

[Использование Visual Studio Code для разработки модулей IoT Edge](how-to-vs-code-develop-module.md)

