---
title: Рекомендации для оператора. Обеспечение высокого уровня доступности и аварийного восстановления в Службе Azure Kubernetes (AKS)
description: Рекомендации для оператора кластера по обеспечению максимального времени работы приложений и высокого уровня доступности, а также подготовки к аварийному восстановлению в Службе Azure Kubernetes (AKS)
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 926f470b8a4dbdb6d6cbfe09ee61349a819600e7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58098633"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Рекомендации по обеспечению непрерывности бизнес-процессов и аварийного восстановления в Службе Azure Kubernetes (AKS)

При управлении кластерами в Службе Azure Kubernetes (AKS) время бесперебойной работы приложений является важным аспектом. AKS обеспечивает высокий уровень доступности, объединяя несколько узлов в одну группу доступности. Но эти несколько узлов не защитят вас от регионального сбоя. Чтобы максимально увеличить время работы, реализуйте соответствующие возможности, поддерживающие непрерывность бизнес-процессов и аварийное восстановление.

Эта статья содержит рекомендации по планированию непрерывности бизнес-процессов и аварийного восстановления в AKS. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * планирование развертывания кластеров AKS в нескольких регионах;
> * маршрутизация трафика между несколькими кластерами с помощью диспетчера трафика Azure;
> * применение георепликации для реестров образов контейнеров;
> * планирование сохранения состояния приложений между несколькими кластерами;
> * репликация хранилища между несколькими регионами.

## <a name="plan-for-multi-region-deployment"></a>Планирование развертывания в нескольких регионах

**Наилучший вариант**. При развертывании нескольких кластеров AKS выбирайте те регионы, в которых доступна служба AKS, и используйте пары регионов.

Кластер AKS развертывается в одном регионе. Для защиты от регионального сбоя разверните приложение в нескольких кластерах AKS, размещенных в разных регионах. При выборе регионов для развертывания кластера AKS учитывайте следующее.

* [Доступность AKS по регионам](https://docs.microsoft.com/azure/aks/container-service-quotas#region-availability):
  * выбирайте регионы, расположенные недалеко от пользователей. Список регионов, поддерживающих AKS, постоянно расширяется.
* [Пары регионов Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
  * Выберите в своей географической области два региона, которые составляют пару. Для таких пар регионов при необходимости координируется обновление платформы и устанавливается приоритетность действий по восстановлению.
* Уровень доступности службы ("Горячий" и "Горячий", "Горячий" и "Теплый", "Горячий" и "Холодный")
  * Какой вариант вам больше подходит: оба региона работают одновременно, но только один из них *готов* к обслуживанию трафика, или для региона допускается время на подготовку к обслуживанию трафика.

Доступность по регионам AKS и пары регионов следует учитывать в контексте согласованного использования. Развертывайте кластеры AKS в парах регионах, для которых настроено совместное управление аварийным восстановлением при региональных сбоях. Например, AKS доступны в регионах *Восточная часть США* и *Западная часть США*. Эти регионы считаются парой. Именно эти два региона мы рекомендуем использовать при создании стратегии BC/DR для AKS.

При развертывании приложения следует добавить еще один шаг в конвейер CI/CD, на котором будут развертываться несколько кластеров AKS. Если вы не обновите конвейеры развертывания, приложения будут развертываться только в один регион и один кластер AKS. Для клиентского трафика, который будет направлен в дополнительный регион, не будут учитываться последние обновления кода.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Использование диспетчера трафика Azure для маршрутизации трафика

**Наилучший вариант**. Диспетчер трафика Azure может направлять клиентов в ближайший кластер AKS и экземпляр приложения. Чтобы обеспечить оптимальную производительность и избыточность, направляйте весь трафик приложений через диспетчер трафика перед передачей в кластер AKS.

Если вы используете несколько кластеров AKS в разных регионах, вам придется управлять передачей трафика к приложениям, которые выполняются в каждом кластере. [Диспетчер трафика Azure](https://docs.microsoft.com/azure/traffic-manager/) — это подсистема балансировки нагрузки на основе DNS, которая может распределять сетевой трафик между регионами. Вы можете направлять пользователей с учетом времени отклика кластера или в зависимости от географического расположения.

![Использование AKS с диспетчером трафика Azure](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Если используется один кластер AKS, клиенты обычно подключаются по *IP-адресу службы* или DNS-имени конкретного приложения. В развертывании с несколькими кластерами клиентам следует подключаться по DNS-имени диспетчера трафика, которое приведет их к службам в каждом кластере AKS. Эти службы определяются через конечные точки диспетчера трафика. Каждая конечная точка имеет *IP-адрес службы подсистемы балансировки нагрузки*. Такая конфигурация позволяет направить трафик из конечной точки диспетчера трафика в одном регионе в конечную точку в другом регионе.

![Географическая маршрутизация с помощью диспетчера трафика Azure](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Диспетчер трафика используется, чтобы выполнять запросы DNS и возвращать наиболее подходящую для пользователя конечную точку. Поддерживаются вложенные профили с приоритетом основного расположения. Например, пользователь должен в первую очередь подключаться к самому близкому географическому региону. Если в этом регионе возникнет проблема, диспетчер трафика направит пользователя в дополнительный регион. Такой подход гарантирует, что клиенты всегда смогут подключиться к экземпляру приложения, даже если ближайший географический регион будет недоступен.

См. дополнительные сведения о [настройке метода маршрутизации трафика по географическому расположению с помощью диспетчера трафика](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door"></a>Маршрутизация приложений 7-го уровня с помощью Azure Front Door

Диспетчер трафика Azure использует для распределения трафика службу DNS (3-й уровень). [Azure двери (в настоящее время в предварительной версии)](https://docs.microsoft.com/azure/frontdoor/front-door-overview) предоставляет возможность маршрутизации HTTP/HTTPS (уровень 7). Дополнительные возможности Front Door включают SSL-терминацию, личный домен, брандмауэр веб-приложения, переопределение URL-адресов и сходство сеансов.

Оцените требования к трафику для приложения, чтобы выбрать оптимальное решение.

## <a name="enable-geo-replication-for-container-images"></a>Включение георепликации для образов контейнеров

**Наилучший вариант**. Сохраните образы контейнера в Реестре контейнеров Azure и настройте георепликацию реестра в каждый регион AKS.

Чтобы развернуть и выполнить приложения в AKS, вам нужен определенный способ хранить и извлекать образы контейнеров. Реестр контейнеров Azure (ACR) можно интегрировать в AKS, чтобы безопасно хранить образы контейнеров или диаграммы Helm. ACR поддерживает географическую репликацию из нескольких источников, позволяя автоматически реплицировать образы в регионы Azure по всему миру. Чтобы повысить производительность и доступность, создайте с помощью георепликации ACR реестр в каждом регионе, где у вас настроен кластер AKS. Каждый кластер AKS извлекает образы контейнеров из локального реестра ACR в том же регионе:

![Георепликация образов контейнеров в Реестре контейнеров Azure](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Георепликация ACR предоставляет следующие преимущества.

* **Извлечение образов из того же региона выполняется быстрее.** В пределах региона Azure вы будете получать образы через сетевое подключение с высокой скоростью и низкой задержкой.
* **Извлечение образов из того же региона выполняется надежнее.** В случае недоступности региона кластер AKS извлекает образ из другого реестра ACR, который остается доступным.
* **Извлечение образов из того же региона обходится дешевле.** Плата за исходящий сетевой трафик между центрами обработки данных не взимается.

Георепликация входит в набор возможностей реестров ACR номера SKU *Премиум*. Инструкции по настройке репликации см. в статье [Георепликация в реестре контейнеров Azure](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).

## <a name="remove-service-state-from-inside-containers"></a>Удаление состояния службы из контейнеров

**Наилучший вариант**. По возможности не храните состояние службы в контейнере. Вместо этого используйте службы Azure PaaS, которые поддерживают репликацию между несколькими регионами.

Состоянием службы называются данные в памяти или на диске, необходимые для функционирования службы. Сюда входят структуры данных и переменные-члены, которые считываются и записываются службой. В зависимости от архитектуры службы к состоянию могут относиться файлы и другие ресурсы, которые хранятся на диске. Например, это могут быть файлы, которые база данных будет использовать для хранения данных и журналов транзакций.

Состояние можно разместить за пределами или в пределах кода, который оперирует этим состоянием. Экспорт состояния, как правило, выполняется с помощью базы данных или другого хранилища данных, работающего на других компьютерах в сети или в других процессах на том же компьютере.

Контейнеры и микрослужбы работаю более устойчиво, когда выполняющиеся в них процессы не сохраняют состояние. Так как приложению почти всегда требуются некоторые данные о состоянии, мы рекомендуем использовать решение "платформа как услуга", например Базу данных Azure для MySQL или Postgres либо Azure SQL.

Дополнительные сведения о том, как создавать переносимые приложения, см. в следующих статьях:

* [The Twelve-Factor App Methodology](https://12factor.net/) (Модель двенадцатифакторного приложения)
* [Запуск веб-приложения в нескольких регионах Azure для обеспечения высокой доступности](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Создание плана миграции хранилища

**Наилучший вариант**. Если вы используете службу хранилища Azure, подготовьте и протестируйте перенос хранилища из основного региона в дополнительный.

Ваши приложения могут использовать для своих данных службу хранилища Azure. Так как приложения распределены между несколькими кластерами AKS в разных регионах, потребуется поддерживать синхронизацию хранилища. Для репликации хранилища чаще всего используются следующие два подхода:

* асинхронная репликация на основе приложений;
* асинхронная репликация на основе инфраструктуры.

### <a name="infrastructure-based-asynchronous-replication"></a>Асинхронная репликация на основе инфраструктуры

Приложению может потребоваться постоянное хранилище даже после удаления группы pod. В Kubernetes для сохранения хранилища данных можно использовать постоянные тома. Эти постоянные тома подключаются к узлу виртуальной машины и группам pod предоставляется доступ к ним. Постоянные тома переносятся вместе с группами pod, даже если эти группы перемещаются в другой узел в том же кластере.

В зависимости от используемого решения хранилища могут применяться разные стратегии репликации. В качестве хранилища традиционно используются [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [CEPH](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [Rook](https://rook.io/docs/rook/master/disaster-recovery.html) и [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps). По использованию каждого из них есть отдельные рекомендации.

Основным подходом считается наличие единой точки доступа к хранилищу, через которую приложения записывают свои данные. Затем эти данные реплицируются в другие регионы и предоставляются для локального доступа.

![Асинхронная репликация на основе инфраструктуры](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Если вы используете управляемые диски Azure, для репликации и аварийного восстановления доступны следующие решения и подходы:

* [Ark в Azure](https://github.com/heptio/ark/blob/master/docs/azure-config.md);
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Асинхронная репликация на основе приложений

Сейчас в Kubernetes отсутствует собственная реализация для асинхронной репликации на основе приложений. Слабосвязанная природа контейнеров и Kubernetes позволяет использовать для такой реализации любое традиционное приложение или язык. Основным подходом считается самостоятельная репликация приложениями запросов к хранилищу, которые записываются в базовое хранилище данных в каждом кластере.

![Асинхронная репликация на основе приложений](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Дальнейшие действия

В этой статье описаны способы обеспечения непрерывности бизнес-процессов и аварийного восстановления в кластерах AKS. Дополнительную информацию об операциях кластера в AKS см. в рекомендациях на такие темы:

* [Мультитенантность и изоляция кластера][aks-best-practices-cluster-isolation]
* [Основные функции планировщика Kubernetes][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
