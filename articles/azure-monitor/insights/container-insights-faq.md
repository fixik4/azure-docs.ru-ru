---
title: 'Azure Monitor для контейнеров: вопросы и ответы | Документация Майкрософт'
description: Решение Azure Monitor для контейнеров отслеживает состояние работоспособности кластера AKS и Экземпляров контейнеров в Azure. В этой статье приведены ответы на распространенные вопросы.
services: azure-monitor
author: mgoedtel
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: magoedte
ms.openlocfilehash: 250bf6ede3a54b26b6e9c68850f8e7260020002e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960519"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Azure Monitor для контейнеров: вопросы и ответы
В этом списке корпорация Майкрософт собрала часто задаваемые вопросы с ответами об Azure Monitor для контейнеров. Если у вас есть другие вопросы об этом решении, вы можете задать их на [форуме для обсуждений](https://feedback.azure.com/forums/34192--general-feedback). Если вопрос задается часто, мы добавим его в эту статью, чтобы его можно было найти быстро и легко.

## <a name="i-am-unable-to-see-any-data-in-the-log-analytics-workspace-at-a-certain-time-everyday-how-do-i-resolve-this"></a>Не удается просматривать никакие данные в рабочей области Log Analytics в определенное время ежедневно. Как решить эту проблему? 

Возможно, достигнуто ограничение по умолчанию 500 МБ или ежедневное ограничение, указанное для управления объемом ежедневно собираемых данных. Если ограничение удовлетворяется за день, сбор данных прерывается и возобновляется только на следующий день. Чтобы просмотреть использования данных и обновить до другой ценовой категории на основе ожидаемого использования шаблонов, см. в разделе [журнал данных об использовании и стоимости](../platform/manage-cost-storage.md). 

## <a name="what-are-the-states-of-containers-specified-in-the-containerinventory-table"></a>Что означают состояния контейнеров, которые указаны в таблице ContainerInventory?
Таблица ContainerInventory содержит сведения об остановленных и запущенных контейнерах. Эта таблица заполняется рабочим процессом агента, который получает из docker полный список контейнеров (работающих и остановленных) и пересылает эти данные в рабочую область Log Analytics.
 
## <a name="how-do-i-resolve-errors-related-to-missing-subscription-registration-for-microsoftoperationsmanagement"></a>Как устранить ошибки, связанные с **отсутствием регистрации Microsoft.OperationsManagement для подписки**?
Чтобы устранить такую ошибку, зарегистрируйте поставщик ресурсов **Microsoft.OperationsManagement** в той подписке, где определена рабочая область. Документацию по этому процессу можно найти [здесь](../../azure-resource-manager/resource-manager-register-provider-errors.md).

## <a name="does-azure-monitor-for-containers-include-support-for-rbac-enabled-aks-clusters"></a>Обеспечена ли в Azure Monitor для контейнеров поддержка кластеров AKS с управлением доступом на основе ролей?
Решение для мониторинга контейнеров не поддерживает RBAC, но она поддерживается с помощью Azure Monitor для контейнеров. На странице сведений о решении может неправильно отображаться информация в колонках с данными о таких кластерах.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Как включить сбор журналов для контейнеров в пространстве имен kube-system через Helm?
Сбор журналов из контейнеров в пространстве имен kube-system по умолчанию отключен. Вы можете включить сбор журналов, задав переменную среды для агента OMS. Дополнительные сведения см. в разделе [Azure Monitor для контейнеров](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers) странице GitHub. 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>Как обновить агент OMS до последней выпущенной версии?
Сведения об обновлении агента, см. в статье [Как управлять агентом Azure Monitor для контейнеров](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>Как включить многострочное ведение журнала?
В настоящее время Azure Monitor для контейнеров не поддерживает многострочное ведение журнала, но вы можете применить обходные пути. Например, настройте вывод данных из всех служб в формате JSON, и тогда Docker/Moby будут сохранять эти данные в одну строку.

Следующий пример демонстрирует сохранение журнала в формате объекта JSON для тестового приложения node.js:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Эти данные будет выглядеть как в следующем примере в Azure Monitor для журналов, при запросе для его:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Подробное описание этого вопроса можно найти по [этой ссылке на сайте GitHub](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-active-directory-errors-when-i-enable-live-logs"></a>Как устранить ошибки Azure Active Directory, возникающие при включении оперативных журналов? 
Вы можете получать следующую ошибку: **Ответ URL-адрес, указанный в запросе не совпадает с URL-адреса ответа, настроенным для приложения: "< идентификатор приложения\>"**. Решение, чтобы устранить эту проблему можно найти в статье [Просмотр контейнера журналы реальном времени с помощью Azure Monitor для контейнеров](container-insights-live-logs.md#configure-aks-with-azure-active-directory). 

## <a name="next-steps"></a>Дальнейшие действия
Чтобы получить представление о требованиях и доступных методах для мониторинга и начать мониторинг кластера AKS, просмотрите [How to onboard the Azure Monitor for containers](container-insights-onboard.md) (Как подключить Azure Monitor для контейнеров). 