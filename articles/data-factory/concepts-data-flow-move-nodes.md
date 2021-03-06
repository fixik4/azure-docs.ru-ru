---
title: Узлы перемещения Потока данных Фабрики данных Azure
description: Как перемещать узлы в диаграмме Потока данных Фабрики данных Azure
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: d82b32e876144a626333f3df1481c5fce9862067
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2019
ms.locfileid: "56732183"
---
# <a name="mapping-data-flow-move-nodes"></a>Сопоставление узлов перемещения Потока данных

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Параметры преобразования статистического выражения](media/data-flow/agghead.png "заголовок статистического выражения")

Поверхность конструктора Потока данных Фабрики данных Azure — это поверхность "конструкции", где вы можете создать потоки данных сверху вниз, слева направо. К каждому преобразованию прикреплена панель элементов со знаком плюс (+). Сосредоточьтесь на своей бизнес-логике, а не на соединении узлов через ребра в среде DAG свободной формы.

Таким образом, "перетаскивать" узлы преобразования при отсутствии общих принципов такого перемещения — это значит изменять входящий поток. Вместо этого вы будете перемещать преобразования, изменяя "входящий поток".

В Потоке данных Фабрики данных Azure потоки представляют собой поток данных. На панели параметров трансформации вы увидите поле "Incoming Stream" (Входящий поток). Это указывает, какой входящий поток данных подает это преобразование. Вы можете изменить физическое расположение вашего узла преобразования на графике, щелкнув "Incoming Stream name" (Имя входящего потока) и выбрав другой поток данных. Текущее преобразование вместе со всеми последующими преобразованиями в этом потоке будет перемещено в новое расположение.

Если вы перемещаете преобразование с одним или несколькими преобразованиями после него, то новое расположение в потоке данных будет присоединено с помощью новой ветви.

Если после выбранного узла у вас нет последующих преобразований, то только это преобразование будет перемещено в новое расположение.
