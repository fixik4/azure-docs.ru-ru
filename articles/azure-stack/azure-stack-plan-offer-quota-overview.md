---
title: Общие сведения о планах, предложениях, квотах и подписках Azure Stack | Документация Майкрософт
description: Сведения о планах, предложениях, квотах и подписках Azure Stack (для операторов облачной среды).
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 10/12/2018
ms.openlocfilehash: 0de021f4666da805eab8faba527f7c5322c39e9d
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57763458"
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Обзор планов, предложений, квот и подписок

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

[Azure Stack](azure-stack-poc.md) позволяет предоставлять широкий набор служб, таких как виртуальные машины, базы данных SQL Server, SharePoint, Exchange и даже [элементы Azure Marketplace](azure-stack-marketplace-azure-items.md). Оператор среды Azure Stack отвечает за настройку и предоставление таких служб в Azure Stack на основе планов, предложений и квот.

Предложения содержат один или несколько планов, а каждый план содержит одну или несколько служб. Создавая планы и объединяя их в различные предложения, вы можете управлять следующими аспектами:

- доступом пользователей к службам и ресурсам;
- объемом потребления ресурсов пользователями;
- доступом к ресурсам из определенных регионов.

Чтобы предоставить службу, выполните следующие общие действия:

1. Добавление службы, которую требуется предоставить пользователям.
2. Создайте план, содержащий одну или несколько служб. При создании плана вы выбираете или создаете квоты, которые определяют ограничения ресурсов каждой службы в плане.
3. Создайте предложение, содержащее один или несколько планов. Предложение может включать базовые планы и необязательные планы надстройки.

Когда предложение будет создано, пользователи смогут подписаться на него, чтобы получить доступ к службам и ресурсам, которые он предоставляет. Количество подписок на предложения не ограничено. На рисунке ниже приведен простой пример пользователя, подписавшегося на два предложения. Каждое предложение содержит два плана, и каждый план предоставляет доступ к службам.

![Подписки клиента с предложениями и планами](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Планы

Планы — это группы, содержащие одну или несколько служб. Оператор Azure Stack создает [планы](azure-stack-create-plan.md) и предлагает их пользователям. В свою очередь, пользователи подписываются на предложения, чтобы использовать содержащиеся в них планы и службы. При создании планов следует задать квоты, определить базовые планы и при необходимости добавить дополнительные планы.

### <a name="quotas"></a>Квоты

Чтобы управлять производительностью облака, следует выбрать стандартные настроенные *квоты* или создать новые квоты для каждой службы в плане. Квоты определяют верхние границы ресурсов, которые может подготовить или использовать пользователь в рамках подписки. Например, квота может разрешать пользователю создать до пяти виртуальных машин.

Квоты можно настраивать отдельно для разных регионов. Например, можно создать план, предоставляющий службы вычислений для региона А, с квотой в две виртуальные машины.

>[!NOTE]
>Каждый Пакет средств разработки Azure Stack имеет один регион (с именем *local*).

См. дополнительные сведения о [типах квот в Azure Stack](azure-stack-quota-types.md).

### <a name="base-plan"></a>Базовый план

При создании предложения администратор служб может добавить базовый план. Эти базовые планы включаются по умолчанию, когда пользователь подписывается на это предложение. Когда пользователь подписывается, он получает доступ ко всем поставщикам ресурсов, указанным в этих базовых планах (с учетом установленных квот).

### <a name="add-on-plans"></a>Дополнительные планы

Планы надстройки — это необязательные планы, добавляемые в предложение. Дополнительные планы не включаются по умолчанию в подписке. Это дополнительные планы (с квотами), доступные в предложении, которые подписчик может добавить в свою подписку. Например, вы можете создать предложение с базовым планом, ограничивающим доступ к ресурсам в рамках бесплатной пробной версии, и дополнительный план, предоставляющий доступ к большинству ресурсов пользователям, решившим использовать службу.

## <a name="offers"></a>Предложения

Предложения — это группы, содержащие один или несколько планов, на которые могут подписаться пользователи. Например, предложение "Альфа" может содержать план A с набором служб вычислений и план Б с набором служб хранения данных и сети.

При [создании предложения](azure-stack-create-offer.md) необходимо добавить по крайней мере один базовый план, но вы также можете создать дополнительные планы, которые пользователи могут добавить к подписке.

## <a name="subscriptions"></a>Подписки

Подписка предоставляет пользователям доступ к предложениям. Если вы являетесь оператором Azure Stack для поставщика услуг, пользователи (клиенты) могут приобрести ваши услуги, подписавшись на доступные предложения. Если вы оператор Azure Stack в организации, пользователи (сотрудники) могут бесплатно подписаться на предоставленные службы.

Каждое сочетание пользователя и предложения представляет собой уникальную подписку. Пользователь может подписаться на несколько предложений, но каждая подписка относится только к одному предложению. Планы, предложения и квоты относятся только к уникальной подписке, и их нельзя перемещать между подписками. Каждый созданный пользователем ресурс связан с одной подпиской.

### <a name="default-provider-subscription"></a>Подписка поставщика по умолчанию

Подписка поставщика по умолчанию автоматически создается при развертывании Пакета средств разработки Azure Stack. Эта подписка позволяет управлять Azure Stack, привлекать дополнительных поставщиков ресурсов, а также создавать планы и предложения для пользователей. В соответствии с требованиями к лицензированию и безопасности выполнять рабочие нагрузки и приложения клиента в рамках подписки нельзя.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о планах и предложения см. в статье [Создание плана в Azure Stack](azure-stack-create-plan.md).
