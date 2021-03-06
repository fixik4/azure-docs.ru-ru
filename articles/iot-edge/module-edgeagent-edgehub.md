---
title: Справочник по требуемым свойствам EdgeAgent и EdgeHub (Azure IoT Edge) | Документация Майкрософт
description: Обзор определенных свойств и их значений для двойников модулей EdgeAgent и EdgeHub
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: b6eb0c5b0d52bba3d34c9853a73b1f3e07b112a7
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2019
ms.locfileid: "54230294"
---
# <a name="properties-of-the-iot-edge-agent-and-iot-edge-hub-module-twins"></a>Свойства двойников модулей агента IoT Edge и центра IoT Edge

Агент IoT Edge и центр IoT Edge — это два модуля, которые составляют среду выполнения IoT Edge. Дополнительные сведения о функциях каждого модуля см. в статье [Общие сведения о среде выполнения Azure IoT Edge и ее архитектуре (предварительная версия)](iot-edge-runtime.md). 

В этой статье представлены требуемые и отображаемые в отчете свойства двойников модулей для среды выполнения. Дополнительные сведения о развертывании модулей на устройствах IoT Edge см. в разделе [Развертывание и мониторинг](module-deployment-monitoring.md).

## <a name="edgeagent-desired-properties"></a>Требуемые свойства EdgeAgent

Двойник модуля для агента IoT Edge называется `$edgeAgent`. Он координирует взаимодействие между агентом IoT Edge, выполняющимся на устройстве, и Центром Интернета вещей. Требуемые свойства задаются при применении манифеста развертывания на конкретном устройстве в ходе развертывания на одно устройство или несколько. 

| Свойство | ОПИСАНИЕ | Обязательно |
| -------- | ----------- | -------- |
| schemaVersion | Должно быть "1.0". | Yes |
| runtime.type | Должно быть "docker". | Yes |
| runtime.settings.minDockerVersion | Задайте минимальную версию Docker, которая требуется для этого манифеста развертывания. | Yes |
| runtime.settings.loggingOptions | Переведенные в строку JSON, содержащую параметры ведения журнала для контейнера агента IoT Edge. [Параметры ведения журнала Docker](https://docs.docker.com/engine/admin/logging/overview/) | Нет  |
| runtime.settings.registryCredentials<br>.{registryId}.username | Имя пользователя для реестра контейнеров. Для Реестра контейнеров Azure именем пользователя обычно является имя реестра.<br><br> Учетные данные реестра необходимы для любых образов модулей, не являющихся общедоступными. | Нет  |
| runtime.settings.registryCredentials<br>.{registryId}.password | Пароль для реестра контейнеров. | Нет  |
| runtime.settings.registryCredentials<br>.{registryId}.address | Адрес для реестра контейнеров. Для Реестра контейнеров Azure адрес обычно имеет вид *{имя реестра}.azurecr.io*. | Нет  |  
| systemModules.edgeAgent.type | Должно быть "docker". | Yes |
| systemModules.edgeAgent.settings.image | Универсальный код ресурса (URI) образа агента IoT Edge. В настоящее время агент IoT Edge не может обновить себя сам. | Yes |
| systemModules.edgeAgent.settings<br>.createOptions | Переведенные в строку JSON, содержащую параметры для создания контейнера агента IoT Edge. [Параметры создания Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Нет  |
| systemModules.edgeAgent.configuration.id | Идентификатор развертывания, которое развернуло этот модуль. | Центр Интернета вещей задает свойство при применении этого манифеста с помощью развертывания. Не является частью манифеста развертывания. |
| systemModules.edgeHub.type | Должно быть "docker". | Yes |
| systemModules.edgeHub.status | Должно быть "running". | Yes |
| systemModules.edgeHub.restartPolicy | Должно быть "always". | Yes |
| systemModules.edgeHub.settings.image | Универсальный код ресурса (URI) образа центра IoT Edge. | Yes |
| systemModules.edgeHub.settings<br>.createOptions | Переведенные в строку JSON, содержащую параметры для создания контейнера центра IoT Edge. [Параметры создания Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Нет  |
| systemModules.edgeHub.configuration.id | Идентификатор развертывания, которое развернуло этот модуль. | Центр Интернета вещей задает свойство при применении этого манифеста с помощью развертывания. Не является частью манифеста развертывания. |
| modules.{ИД_модуля}.version | Определяемая пользователем строка, представляющая версию этого модуля. | Yes |
| modules.{ИД_модуля}.type | Должно быть "docker". | Yes |
| modules.{Ид_модуля}.status | {"running" \| "stopped"} | Yes |
| modules.{ИД_модуля}.restartPolicy | {"never" \| "on-failed" \| "on-unhealthy" \| "always"} | Yes |
| modules.{ИД_модуля}.settings.image | Универсальный код ресурса (URI) для образа модуля. | Yes |
| modules.{ИД_модуля}.settings.createOptions | Переведенные в строку JSON, содержащую параметры для создания контейнера модуля. [Параметры создания Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Нет  |
| modules.{ИД_модуля}.configuration.id | Идентификатор развертывания, которое развернуло этот модуль. | Центр Интернета вещей задает свойство при применении этого манифеста с помощью развертывания. Не является частью манифеста развертывания. |

## <a name="edgeagent-reported-properties"></a>Отображаемые в отчете свойства EdgeAgent

Сообщаемые свойства агента IoT Edge включают в себя три основных элемента информации:

1. состояние применения последних требуемых свойств;
2. состояние модулей, запущенных на устройстве, зарегистрированное агентом IoT Edge;
3. копия требуемых свойств, выполняемых в данное время на устройстве.

Последний элемент данных полезен в случае, если требуемые свойства не были успешно применены в среде выполнения и устройство по прежнему работает под управлением предыдущего манифеста развертывания.

> [!NOTE]
> Сообщаемые свойства агента IoT Edge полезны, так как к ним можно выполнять запросы с помощью [языка запросов Центра Интернета вещей](../iot-hub/iot-hub-devguide-query-language.md), чтобы узнать состояние развертывания в масштабе. Дополнительные сведения об использовании свойств агента IoT Edge, чтобы узнать состояние развертывания, см. в статье [Общие сведения об автоматических развертываниях IoT Edge для отдельных устройств или в требуемом масштабе](module-deployment-monitoring.md).

Следующая таблица не включает сведения, которые копируются из требуемых свойств.

| Свойство | ОПИСАНИЕ |
| -------- | ----------- |
| lastDesiredVersion | Это целое число относится к последней версии требуемых свойств, обрабатываемых агентом IoT Edge. |
| lastDesiredStatus.code | Это код состояния, ссылающийся на последние требуемые свойства, используемые агентом IoT Edge. Допустимые значения: `200` — успех, `400` — недопустимая конфигурация,`412` — недопустимая версия схемы, `417` — нужные свойства пусты, `500` — сбой. |
| lastDesiredStatus.description | Текстовое описание состояния. |
| deviceHealth | Имеет значение `healthy`, если состояние среды выполнения всех модулей имеет значение `running` или `stopped`, в противном случае — `unhealthy`. |
| configurationHealth.{ИД_развертывания}.health | Имеет значение `healthy`, если состояние среды выполнения всех модулей, установленное развертыванием {ИД_развертывания}, имеет значение `running` или `stopped`, в противном случае — `unhealthy`. |
| runtime.platform.OS | Сообщает ОС, выполняющуюся на устройстве. |
| runtime.platform.architecture | Сообщает архитектуру ЦП на устройстве. |
| systemModules.edgeAgent.runtimeStatus | Состояние агента IoT Edge: {"running" \| "unhealthy"} |
| systemModules.edgeAgent.statusDescription | Текстовое описание сообщаемого состояния агента IoT Edge. |
| systemModules.edgeHub.runtimeStatus | Состояние центра IoT Edge: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| systemModules.edgeHub.statusDescription | Текстовое описание состояния центра IoT Edge, если он неработоспособен. |
| systemModules.edgeHub.exitCode | Если выполнен выход, код завершения, сообщаемый контейнером центра IoT Edge. |
| systemModules.edgeHub.startTimeUtc | Время последнего запуска центра IoT Edge. |
| systemModules.edgeHub.lastExitTimeUtc | Время последнего выхода центра IoT Edge. |
| systemModules.edgeHub.lastRestartTimeUtc | Время последнего перезапуска центра IoT Edge. |
| systemModules.edgeHub.restartCount | Количество раз, когда этот модуль был перезагружен в рамках политики перезапуска. |
| modules.{ИД_модуля}.runtimeStatus | Состояние модуля: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| modules.{ИД_модуля}.statusDescription | Текстовое описание состояния модуля, если он неработоспособен. |
| modules.{ИД_модуля}.exitCode | Если выполнен выход, код завершения, сообщаемый контейнером модуля. |
| modules.{ИД_модуля}.startTimeUtc | Время последнего запуска модуля. |
| modules.{ИД_модуля}.lastExitTimeUtc | Время последнего завершения работы модуля. |
| modules.{ИД_модуля}.lastRestartTimeUtc | Время последнего перезапуска модуля. |
| modules.{ИД_модуля}.restartCount | Количество раз, когда этот модуль был перезагружен в рамках политики перезапуска. |

## <a name="edgehub-desired-properties"></a>Требуемые свойства EdgeHub

Двойник модуля центра IoT Edge называется `$edgeHub`. Он координирует взаимодействие между центром IoT Edge, выполняющимся на устройстве, и Центром Интернета вещей. Требуемые свойства задаются при применении манифеста развертывания на конкретном устройстве в ходе развертывания на одно устройство или несколько. 

| Свойство | ОПИСАНИЕ | Требуется в манифесте развертывания |
| -------- | ----------- | -------- |
| schemaVersion | Должно быть "1.0". | Yes |
| routes.{имя_маршрута} | Строка, представляющая маршрут центра IoT Edge. | Элемент `routes` может присутствовать, но быть пустым. |
| storeAndForwardConfiguration.timeToLiveSecs | Время в секундах, в течение которого центр IoT Edge сохраняет сообщения в случае отключения конечных точек маршрутизации, например отключения от Центра Интернета вещей или локального модуля. | Yes |

## <a name="edgehub-reported-properties"></a>Отображаемые в отчете свойства EdgeHub

| Свойство | ОПИСАНИЕ |
| -------- | ----------- |
| lastDesiredVersion | Это целое число относится к последней версии требуемых свойств, обрабатываемых центром IoT Edge. |
| lastDesiredStatus.code | Это код состояния, ссылающийся на последние требуемые свойства, используемые центром IoT Edge. Допустимые значения: `200` — успех, `400` — недопустимая конфигурация, `500` — сбой. |
| lastDesiredStatus.description | Текстовое описание состояния. |
| clients.{идентификатор устройства или модуля}.status | Состояние подключения этого устройства или модуля. Возможные значения {"connected" \| "disconnected"}. В отключенном состоянии могут находиться только удостоверения модуля. Подчиненные устройства, подключающиеся к центру IoT Edge, отображаются только при подключении. |
| clients.{идентификатор устройства или модуля}.lastConnectTime | Время последнего подключения модуля или устройства. |
| clients.{идентификатор устройства или модуля}.lastDisconnectTime | Время последнего отключения модуля или устройства. |

## <a name="next-steps"></a>Дополнительная информация

Сведения о том, как использовать эти свойства для создания манифестов развертывания, см. в статье [Сведения об использовании, настройке и повторном использовании модулей Azure IoT Edge (предварительная версия)](module-composition.md).
