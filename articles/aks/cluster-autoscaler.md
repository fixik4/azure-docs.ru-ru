---
title: Использование средства автомасштабирования кластера в Службе контейнеров Azure
description: Узнайте, как использовать средство автомасштабирования кластера для автомасштабирования кластера в Службе контейнеров Azure в соответствии с требованиями приложения.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/29/2019
ms.author: iainfou
ms.openlocfilehash: d8e095303161002d10914ca7c3213ac0c6894e5d
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2019
ms.locfileid: "58444023"
---
# <a name="preview---automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Preview — автоматическое масштабирование кластера в соответствии с требованиями приложения в службе Azure Kubernetes (AKS)

Чтобы удовлетворить растущие требования приложения к Службе контейнеров Azure (AKS), вы можете изменить число узлов, на которых выполняются рабочие нагрузки. Компоненты кластера автомасштабирования могут отслеживать нехватку назначенных pod в кластере, связанную с ограничениями на ресурсы. При обнаружении проблем число узлов увеличивается в соответствии с требованиями приложения. Также регулярно проверяется наличие узлов с малым числом запущенных pod, при их обнаружении число узлов соответствующим образом снижается. Такая возможность автоматически масштабировать количество узлов в кластере AKS обеспечивает эффективное и экономное использование кластера.

В этой статье показано, как включить и администрировать средство автомасштабирования для кластера AKS.

> [!IMPORTANT]
> Компоненты предварительной версии AKS, самообслуживания и согласиться. Предварительные версии предоставляются для сбора отзывов и ошибки нашего сообщества. Тем не менее они не поддерживаются в службе технической поддержки Azure. Если создать кластер, или добавить эти компоненты в имеющиеся кластеры, этого кластера не поддерживается, пока эта функция больше не находится в предварительной версии и этапах общедоступная (GA).
>
> При возникновении проблем с помощью функции предварительной версии, [сообщите о них в репозитории AKS GitHub] [ aks-github] именем функции предварительной версии в заголовке ошибки.

## <a name="before-you-begin"></a>Перед началом работы

Для работы с этой статьей требуется Azure CLI 2.0.55 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0][azure-cli-install].

### <a name="install-aks-preview-cli-extension"></a>Установка расширения интерфейса командной строки aks-preview

Кластерам AKS для поддержки средства автомасштабирования кластера необходимо использовать масштабируемые наборы виртуальных машин и Kubernetes *1.12.4* или более поздней версии. Поддержка этих масштабируемых наборов пока предоставляется в предварительной версии. Чтобы зарегистрироваться для ее использования и создать кластеры с масштабируемыми наборами, сначала установите расширение Azure CLI *aks-preview* с помощью команды [az extension add][az-extension-add], как показано в следующем примере:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Если вы ранее установили *предварительной версии aks* расширения, устанавливать доступные обновления с помощью `az extension update --name aks-preview` команды.

### <a name="register-scale-set-feature-provider"></a>Регистрация поставщика компонента масштабируемых наборов

Чтобы создать службу AKS, которая использует масштабируемые наборы, необходимо также включить соответствующий флаг компонента в своей подписке. Чтобы зарегистрировать флаг компонента *VMSSPreview*, используйте команду [az feature register][az-feature-register], как показано в следующем примере.

```azurecli-interactive
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Через несколько минут отобразится состояние *Registered* (Зарегистрировано). Состояние регистрации можно проверить с помощью команды [az feature list][az-feature-list].

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Когда все будет готово, обновите регистрацию поставщика ресурсов *Microsoft.ContainerService* с помощью команды [az provider register][az-provider-register].

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="about-the-cluster-autoscaler"></a>Сведения о средстве автомасштабирования кластера

Чтобы учитывать постоянно меняющиеся требования приложения, например регулярное изменение нагрузки в течение дня или недели, часто требуется механизм автомасштабирования кластера. Для кластеров AKS существуют два способа масштабирования.

* **Средство автомасштабирования кластера** отслеживает невозможность назначить на узел новые pod, связанную с ограниченными ресурсами. При возникновении такой ситуации число узлов в кластере автоматически повышается.
* **Средство горизонтального автомасштабирования pod** использует сервер метрик в кластере Kubernetes для отслеживания потребностей pod в ресурсах. Если службе нужно больше ресурсов, количество pod автоматически увеличивается в соответствии с требованиями.

![Средство автомасштабирования кластера и средство горизонтального автомасштабирования pod часто используются вместе, чтобы учитывать любые изменения требований приложения.](media/autoscaler/cluster-autoscaler.png)

Оба этих средства могут уменьшать количество pod и узлов, когда в них отпадает надобность. Средство автомасштабирования кластера уменьшает количество узлов, если в течение определенного периода времени существует неиспользуемая емкость. Pod на узле, который будет удаляться, безопасно перемещаются средством автомасштабирования в другое расположение в том же кластере. Средство автомасштабирования кластера не сможет уменьшить масштаб, если pod невозможно переместить, например в следующих ситуациях:

* если pod создан напрямую на узле, а не через объект контроллера, например в наборе развертывания или реплик;
* если бюджет неработоспособности pod имеет слишком строгие условия и не допускает снижение числа pod ниже определенного порогового значения;
* если pod использует селекторы узла или свойства удаления сходства, которые невозможно выполнить, так как они запланированы на другом узле.

Дополнительные сведения о причинах, которые препятствуют средству автомасштабирования уменьшать масштаб, описаны в разделе [What types of pods can prevent CA from removing a node?][autoscaler-scaledown] (Какие типы pod могут помешать средству автомасштабирования кластера удалить узел?).

Средство автомасштабирования кластера использует параметры запуска для выбора интервалов времени между событиями масштабирования, пороговых значений для ресурсов и т. п. Эти параметры определяются платформой Azure и пока не доступны для настройки вручную. Дополнительные сведения о параметрах, используемых средством автомасштабирования кластера, см. в разделе [What are the parameters to CA?][autoscaler-parameters] (Какие параметры использует средство автомасштабирования кластера?).

Упомянутые два средства автомасштабирования могут работать вместе, и часто оба средства развертываются в кластере. При совместной работе средство горизонтального автомасштабирования pod берет на себя управление числом pod, необходимых для удовлетворения требований приложения. Средство автомасштабирования кластера контролирует, в свою очередь, количество узлов для работы запланированного числа pod.

> [!NOTE]
> При использовании средства автомасштабирования кластера отключается возможность масштабирования вручную. Позвольте этому средству самостоятельно определять необходимое количество узлов. Если вы предпочитаете выполнять масштабирование кластера вручную, [отключите средство автомасштабирования кластера](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Создание кластера AKS и включение средства автомасштабирования кластера

Если вам нужен новый кластер AKS, выполните команду [az aks create][az-aks-create]. В параметре *--kubernetes-version* укажите версию не ниже минимально необходимой, как описано выше в разделе [Перед началом работы](#before-you-begin). Чтобы включить и настроить средство автомасштабирования кластера, примените параметр *--enable-cluster-autoscaler* и укажите минимальное (*--min-count*) и максимальное (*--max-count*) число узлов.

> [!IMPORTANT]
> Компонент Kubernetes является средством автомасштабирования кластера. Хотя в кластере AKS используется масштабируемый набор виртуальных машин для узлов, не включайте и не изменяйте вручную параметры автомасштабирования масштабируемого набора на портале Azure или с помощью Azure CLI. Разрешите средству автомасштабирования кластера Kubernetes устанавливать необходимые параметры масштабирования. Дополнительные сведения см. в разделе часто задаваемых вопросов [Можно ли изменять теги и другие свойства ресурсов AKS в группе ресурсов MC_*?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc_-resource-group).

В следующем примере создается кластер AKS с масштабируемым набором виртуальных машин, включенным средством автомасштабирования кластера и числом узлов в диапазоне от *1* до *3*:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location canadaeast

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --kubernetes-version 1.12.6 \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Создание кластера и настройка параметров автомасштабирования кластера занимает несколько минут.

### <a name="enable-the-cluster-autoscaler-on-an-existing-aks-cluster"></a>Включение средства автомасштабирования кластера для существующего кластера AKS

Вы можете включить средство автомасштабирования кластера для существующего кластера AKS, который соответствует требованиям из предыдущего раздела [Перед началом работы](#before-you-begin). Выполните команду [az aks update][az-aks-update] с параметром *--enable-cluster-autoscaler*, а также укажите значения *--min-count* и *--max-count*. В следующем примере включается средство автомасштабирования кластера для существующего кластера и настраивается диапазон от *1* до *3* для числа узлов:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Если минимальное число узлов имеет значение больше, чем существующее количество узлов в кластере, потребуется несколько минут для создания дополнительных узлов.

## <a name="change-the-cluster-autoscaler-settings"></a>Изменение параметров средства автомасштабирования кластера

На предыдущем шаге при создании и обновлении кластера AKS мы указывали для средства автомасштабирования кластера минимальное число узлов *1* и максимальное число узлов *3*. По мере изменения требований приложения вы можете скорректировать настроенное количество узлов для средства автомасштабирования кластера.

Чтобы изменить число узлов, выполните команду [az aks update][az-aks-update] и укажите минимальное и максимальное значения. В следующем примере для параметра *--min-count* указывается значение *1*, а для параметра *--max-count* — значение *5*:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

> [!NOTE]
> На этапе предварительной версии вы не можете задать более высокое минимальное число узлов, чем уже настроено для кластера. Например, если у вас есть кластер с минимальным числом узлов *1*, вы не сможете указать значение min-count *3*.

Отслеживайте производительность приложений и служб, а затем корректируйте диапазон числа узлов для средства автомасштабирования кластера в соответствии с требуемой производительностью.

## <a name="disable-the-cluster-autoscaler"></a>Отключение средства автомасштабирования кластера

Когда отпадет потребность в средстве автомасштабирования кластера, его можно отключить с помощью команды [az aks update][az-aks-update]. При отключении средства автомасштабирования кластера узлы не удаляются.

Чтобы удалить средство автомасштабирования кластера, укажите параметр *--disable-cluster-autoscaler*, как показано в следующем примере:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

Вы можете вручную масштабировать кластер с помощью команды [az aks scale][az-aks-scale]. Если вы используете средство горизонтального автомасштабирования pod, оно будет и далее работать после отключения средства автомасштабирования кластера, но при исчерпании ресурсов существующих узлов оно не сможет назначить новые pod.

## <a name="next-steps"></a>Дальнейшие действия

В этой статье показано, как автоматически масштабировать количество узлов AKS. Также вы можете использовать средство горизонтального автомасштабирования pod для автоматической настройки числа pod, на которых выполняется приложение. Инструкции по использованию средства горизонтального автомасштабирования pod см. в статье [Руководство. Масштабирование приложений в Службе Azure Kubernetes (AKS)][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-github]: https://github.com/azure/aks/issues

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
