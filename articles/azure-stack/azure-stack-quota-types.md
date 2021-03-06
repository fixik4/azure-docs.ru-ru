---
title: Типы квот в Azure Stack | Документация Майкрософт
description: Просмотр и редактирование разных типов квот, доступных для служб и ресурсов в Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: sethm
ms.reviewer: xiaofmao
ms.lastreviewed: 12/07/2018
ms.openlocfilehash: 3d9376ba5945c97d18f6cf68c242d5217beee679
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2019
ms.locfileid: "58349711"
---
# <a name="quota-types-in-azure-stack"></a>Типы квот в Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

[Квоты](azure-stack-plan-offer-quota-overview.md#plans) определяют ограничения ресурсов, которые может подготовить или использовать пользователь в рамках подписки. Например, квота может позволить пользователю создавать до пяти виртуальных машин. Каждый ресурс может иметь собственные типы квот.

## <a name="compute-quota-types"></a>Типы квот вычислений

| **Тип** | **Значение по умолчанию** | **Описание** |
| --- | --- | --- |
| Максимальное число виртуальных машин | 50 | Максимальное число виртуальных машин, которые могут быть созданы с помощью подписки в этом расположении. |
| Максимальное число ядер виртуальной машины | 100 | Максимальное количество ядер, которые можно создать в рамках подписки в этом расположении (например, виртуальная машина A3 имеет четыре ядра). |
| Максимальное число групп доступности | 10 | Максимальное количество групп доступности, которые могут быть созданы в этом расположении. |
| Максимальное число масштабируемых наборов виртуальных машин | 100 | Максимальное количество масштабируемых наборов виртуальных машин, которые могут быть созданы в этом расположении. |
| Максимальная емкость (в ГБ) управляемого диска класса Standard | 2048 | Максимальное число управляемых дисков класса Standard, которые можно создать в этом расположении. |
| Максимальная емкость (в ГБ) управляемого диска класса Premium | 2048 | Максимальное число управляемых дисков класса Premium, которые можно создать в этом расположении. |

> [!NOTE]  
> Максимальная емкость неуправляемого диска (страничные BLOB-объекты) отличается от квоты управляемого диска. Ее следует задавать в рамках квоты хранилища.

## <a name="storage-quota-types"></a>Типы квот хранилища 

| **Элемент** | **Значение по умолчанию** | **Описание** |
| --- | --- | --- |
| Максимальная емкость (ГБ) |2048 |Общая емкость хранения (включая большие двоичные объекты и все связанные моментальные снимки, таблицы и очереди), используемая подпиской в этом расположении. |
| Общее количество учетных записей хранения |20 |Максимальное число учетных записей хранения, которые могут быть созданы с помощью подписки в этом расположении. |

> [!NOTE]  
> Принудительное применение квоты хранилища может занять до двух часов. Максимальная емкость управляемого диска отличается от общей квоты хранилища. Ее следует задавать в рамках квоты вычислений.

## <a name="network-quota-types"></a>Типы квот сети

| **Элемент** | **Значение по умолчанию** | **Описание** |
| --- | --- | --- |
| Максимальное число общедоступных IP-адресов |50 |Максимальное число IP-адресов, которые могут быть созданы с помощью подписки в этом расположении. |
| Максимальное число виртуальных сетей |50 |Максимальное число виртуальных сетей, которые могут быть созданы с помощью подписки в этом расположении. |
| Максимальное число шлюзов виртуальной сети |1 |Максимальное число шлюзов виртуальной сети (VPN-шлюзов), которые могут быть созданы с помощью подписки в этом расположении. |
| Максимальное число сетевых подключений |2 |Максимальное количество сетевых подключений ("точка — точка" или "сеть — сеть"), которые можно создать с помощью подписки для всех шлюзов виртуальной сети в этом расположении. |
| Максимальное число подсистем балансировки нагрузки |50 |Максимальное число балансировщиков нагрузки, которые могут быть созданы с помощью подписки в этом расположении. |
| Максимальное число сетевых адаптеров |100 |Максимальное число сетевых интерфейсов, которые могут быть созданы с помощью подписки в этом расположении. |
| Максимальное число групп безопасности |50 |Максимальное число групп безопасности сети, которые могут быть созданы с помощью подписки в этом расположении. |

## <a name="view-an-existing-quota"></a>Просмотр имеющихся квот

Существует два разных способа просмотра имеющихся квот.

### <a name="plans"></a>Планы

1. В левой области навигации на портале администрирования выберите **Планы**.
2. Щелкните имя плана, подробные сведения о котором нужно просмотреть.
3. В открывшейся колонке выберите **Services and quotas** (Службы и квоты).
4. Выберите квоту, которую нужно просмотреть, щелкнув ее в столбце **Имя**.

    [![Квоты](media/azure-stack-quota-types/quotas1sm.png "Просмотр квот")](media/azure-stack-quota-types/quotas1.png#lightbox)

### <a name="resource-providers"></a>Поставщики ресурсов

1. На панели мониторинга по умолчанию на портале администрирования найдите плитку **Поставщики ресурсов**.
2. Выберите службу с квотой, которую необходимо просмотреть, например **Вычисление**, **Сеть** или **Хранилище**.
3. Щелкните **Квоты** и выберите квоту, которую необходимо просмотреть.

## <a name="edit-a-quota"></a>Изменение квоты

Есть два различных способа изменения квот.

### <a name="edit-a-plan"></a>Изменение плана

1. В левой области навигации на портале администрирования выберите **Планы**.
2. Щелкните имя плана, квоту которого нужно изменить.
3. В открывшейся колонке выберите **Services and quotas** (Службы и квоты).
4. Выберите квоту, которую нужно изменить, щелкнув ее в столбце **Имя**.
    [![Квоты](media/azure-stack-quota-types/quotas1sm.png "Просмотр квот")](media/azure-stack-quota-types/quotas1.png#lightbox)

5. В открывшейся колонке выберите **Edit in Compute** (Изменить в вычислениях), **Edit in Network** (Изменить в сети) или **Edit in Storage** (Изменить в хранилище).
    ![Квоты](media/azure-stack-quota-types/quotas3.png "Просмотр квот")

Кроме того, необходимо выполнить эту процедуру, чтобы изменить квоту:

1. На панели мониторинга по умолчанию на портале администрирования найдите плитку **Поставщики ресурсов**.
2. Выберите службу с квотой, которую необходимо изменить, например **Вычисление**, **Сеть** или **Хранилище**.
3. Далее щелкните **Квоты** и выберите квоту, которую необходимо изменить.
4. Измените значения в области **Set Storage quotas** (Задать квоты хранилища), **Set Compute quotas** (Задать квоты вычислений) или **Set Network quotas** (Задать квоты сети) (в зависимости от типа квоты, которую вы выбрали), а затем выберите **Сохранить**.

### <a name="edit-original-configuration"></a>Изменение исходной конфигурации
  
Вы можете изменить исходную конфигурацию квоты вместо [использования дополнительного плана](create-add-on-plan.md). При изменении квоты новая конфигурация автоматически применяется глобально ко всем планам, которые используют эту квоту, и всем имеющимся подпискам, которые используют эти планы. Редактирование квоты отличается от использования дополнительного плана для предоставления измененной квоты, который пользователь выбирает для подписки.

Новые значения для квоты применяются глобально ко всем планам, использующим измененную квоту, и всем имеющимся подпискам, использующим эти планы.

## <a name="next-steps"></a>Дополнительная информация

- [Дополнительные сведения о планах, предложениях и квотах.](azure-stack-plan-offer-quota-overview.md)
- [Create a plan in Azure Stack](azure-stack-create-plan.md) (Создание плана в Azure Stack)
