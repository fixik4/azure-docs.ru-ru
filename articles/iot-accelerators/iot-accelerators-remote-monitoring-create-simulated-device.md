---
title: Моделирование устройств с решением Интернета вещей для удаленного мониторинга в Azure | Документация Майкрософт
description: 'В этом руководстве показано, как использовать симулятор устройств с акселератором решений для удаленного мониторинга:'
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: 81efd9dc5d33ed23574b1cb66f26ccb444d5a3ff
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58180540"
---
# <a name="create-and-test-a-new-simulated-device"></a>Создание и тестирование нового имитированного устройства

Акселератор решений для удаленного мониторинга позволяет определить собственные имитированные устройства. В этой статье показано, как определить новое имитируемое устройство типа Lightbulb, а затем проверить его локально. Акселератор решений содержит имитированные устройства, такие как холодильники и грузовики. Тем не менее, вы можете определить собственные имитированные устройства для тестирования решений Интернета вещей до развертывания настоящих устройств.

> [!NOTE]
> В этой статье описывается использование имитированных устройств, размещенных в службе моделирования устройств. Если вам требуется создать реальное устройство, см. статью [Подключение устройства к акселератору решений для удаленного мониторинга (Node.js)](iot-accelerators-connecting-devices.md).

В этом руководстве показано, как настроить микрослужбу имитации устройств. Эта микрослужба является частью акселератора решений для удаленного мониторинга. Чтобы продемонстрировать возможности имитации устройств, в этом практическом руководстве рассматривается два сценария с приложением Интернета вещей компании Contoso.

[!INCLUDE [iot-solution-accelerators-create-device](../../includes/iot-solution-accelerators-create-device.md)]

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве показано, как создавать пользовательские типы имитированных устройств и проверять их, запустив микрослужбу имитации устройств в локальной среде.

Далее мы рекомендуем научиться развертывать пользовательские типы имитированных устройств в [акселератор решений для удаленного мониторинга](iot-accelerators-remote-monitoring-deploy-simulated-device.md).
