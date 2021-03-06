---
title: Замена узла единицы масштабирования в интегрированной системе Azure Stack | Документация Майкрософт
description: Узнайте, как заменить физический узел единицы масштабирования в интегрированной системе Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 12/06/2018
ms.openlocfilehash: 1024661fabfd97ce5f058d071de514fa7d02e527
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57782315"
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Замена узла единицы масштабирования в интегрированной системе Azure Stack

*Область применения: интегрированные системы Azure Stack*

В этой статье описывается общий процесс замены физического компьютера (также называемого узлом единицы масштабирования) в интегрированной системе Azure Stack. Фактические шаги по замене узла единицы масштабирования будут варьироваться в зависимости от поставщика изготовителя оборудования (OEM). Подробные инструкции, относящиеся к вашей системе, см. в документации поставщика по элементам, заменяемым в условиях эксплуатации (FRU).

На блок-схеме ниже показан общий процесс FRU для замены всего узла единицы масштабирования.

![Блок-схема процедуры замены узла](media/azure-stack-replace-node/replacenodeflow.png)

* Это действие может не потребоваться в зависимости от физического состояния оборудования.

> [!Note]  
> Если операция завершения работы завершается сбоем, рекомендуется использовать операцию очистки, а затем операцию остановки. Дополнительные сведения см. в описании доступных операций с узлами.  

## <a name="review-alert-information"></a>Просмотр сведений об оповещении

Если узел единицы масштабирования не работает, отобразятся следующие критические оповещения:

- "Node not connected to network controller" (Узел не подключен к сетевому контроллеру);
- "Node inaccessible for virtual machine placement" (Узел недоступен для замены виртуальной машины);
- "Scale unit node is offline" (Узел единицы масштабирования отключен).

![Список оповещений для неработающей единицы масштабирования](media/azure-stack-replace-node/nodedownalerts.png)

При открытии оповещения **Scale unit node is offline** (Узел единицы масштабирования отключен) в его описании указывается, что узел единицы масштабирования недоступен. Также могут отобразиться дополнительные оповещения в решении для мониторинга от изготовителя оборудования, которое работает на узле жизненного цикла оборудования.

![Описание оповещения об отключенном узле](media/azure-stack-replace-node/nodeoffline.png)

## <a name="scale-unit-node-replacement-process"></a>Процесс замены узла единицы масштабирования

Следующие инструкции дают общее представление о процессе замены узла единицы масштабирования. Подробные инструкции, относящиеся к вашей системе, приведены в документации поставщика изготовителя оборудования (OEM) по элементам, заменяемым в условиях эксплуатации (FRU). Не выполняйте приведенные ниже действия, не ознакомившись с документацией изготовителя оборудования.

1. Для правильного завершения работы узла единицы масштабирования используйте действие **Завершение работы**. Это действие может не потребоваться в зависимости от физического состояния оборудования. 

2. В маловероятном случае сбоя действия завершения работы воспользуйтесь действием [Очистка](azure-stack-node-actions.md#drain), чтобы перевести узел единицы масштабирования в режим обслуживания. Это действие может не потребоваться в зависимости от физического состояния оборудования.

   > [!NOTE]  
   > В любом случае, не нарушая работу S2D (локальных дисковых пространств), за один раз можно останавливать и выключать только один узел.

3. После перевода узла единицы масштабирования в режим обслуживания используйте действие [Остановка](azure-stack-node-actions.md#stop). Это действие может не потребоваться в зависимости от физического состояния оборудования.

   > [!NOTE]  
   > В редких случаях, когда не работает действие выключения питания, следует использовать веб-интерфейс контроллера управления основной платой (BMC).

4. Замените физический компьютер. Как правило, это выполняет поставщик изготовителя оборудования (OEM).
5. Используйте действие [Восстановление](azure-stack-node-actions.md#repair), чтобы добавить новый физический компьютер в единицу масштабирования.
6. Используйте привилегированную конечную точку, чтобы [проверить состояние восстановления виртуального диска](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). После установки новых дисков данных задание полного восстановления хранилища может длиться несколько часов, в зависимости от загрузки системы и использованного пространства.
7. После завершения действия восстановления проверьте, все ли активные оповещения закрылись автоматически.

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения о замене физического диска без включения системы см. в разделе [Замена диска](azure-stack-replace-disk.md). 
- Сведения о замене компонента оборудования, для которой требуется выключение системы, см. в разделе [Замена компонента оборудования на узле единицы масштабирования Azure Stack](azure-stack-replace-component.md).
