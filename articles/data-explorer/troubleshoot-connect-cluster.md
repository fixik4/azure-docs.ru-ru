---
title: Устранение ошибки подключения к кластеру обозреватель данных Azure
description: В этой статье описываются действия по устранению неполадок для подключения к кластеру в обозреватель данных Azure.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: c71af799f614e9cd28221d79634666cbc3b2c987
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044631"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>Устранение неполадок, Сбой при подключении к кластеру в обозревателе данных Azure

Если вы не можете подключиться к кластеру в обозревателе данных Azure, выполните следующие действия.

1. Строка подключения должна быть указана правильно. Он должен быть в формате: `https://<ClusterName>.<Region>.kusto.windows.net`, как в следующем примере: `https://docscluster.westus.kusto.windows.net`.

1. Убедитесь, что у вас есть соответствующие разрешения. Если этого не сделать, вы получите ответ *несанкционированного*.

    Дополнительные сведения о разрешениях см. в разделе [Управлять разрешениями базы данных](manage-database-permissions.md). При необходимости обратитесь к администратору кластера, чтобы он добавил вас к соответствующей роли.

1. Убедитесь, что кластер не был удален: ознакомьтесь с журналом в вашей подписке.

1. Проверьте [панель мониторинга работоспособности службы Azure](https://azure.microsoft.com/status/). Просмотрите состояние обозревателя данных Azure в регионе, где вы пытаетесь подключиться к кластеру.

    Если состояние не **Хорошо** (зеленая галочка), попробуйте подключиться к кластеру после улучшения состояния.

1. Если вам нужна помощь в решении проблемы, откройте запрос на поддержку на [портале Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).