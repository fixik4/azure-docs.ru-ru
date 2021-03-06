---
title: Перезапись заголовков HTTP в существующем Шлюзе приложений Azure
description: Статья содержит сведения о том, как переписать заголовки HTTP в существующем Шлюзе приложений Azure с помощью Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: f3bda2cf680fa1ad17b5cbb3b9ba5911c78a1e4e
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2019
ms.locfileid: "57316512"
---
# <a name="rewrite-http-headers-in-an-existing-application-gateway"></a>Перезапись заголовков HTTP в существующем шлюзе приложений

Для настройки [правил перезаписи заголовков запросов и ответов HTTP](rewrite-http-headers.md) в существующем [SKU автоматического масштабирования и шлюза приложений, избыточного между зонами](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant), можно использовать Azure PowerShell.

> [!IMPORTANT]
> SKU автоматического масштабирования и шлюза приложений, избыточного между зонами, на данный момент доступен в общедоступной предварительной версии. Предварительная версия предоставляется без соглашения об уровне обслуживания. Не рекомендуем использовать ее в рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. См. [дополнительные условия использования для предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
>
> * получить сведения о конфигурации существующего шлюза приложений;
> * указать конфигурацию правила перезаписи заголовка HTTP;
> * обновить шлюз приложения с помощью вышеуказанной конфигурации для перезаписи заголовков HTTP.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Технические условия

Для работы с этим руководством требуется запустить Azure PowerShell в локальной среде. Необходим модуль Az 1.0.0 или более поздней версии. Чтобы узнать версию, выполните команду `Import-Module Az`, а затем команду `Get-Module Az`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps). После проверки версии PowerShell выполните командлет `Login-AzAccount`, чтобы создать подключение к Azure.

## <a name="sign-in-to-azure"></a>Вход в Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Укажите конфигурацию правила перезаписи заголовка HTTP**

Настройте новые объекты, необходимые для перезаписи заголовков HTTP.

- **RequestHeaderConfiguration**: этот объект используется для указания полей заголовка запроса, которые нужно перезаписать, и нового значения, которое должно быть записано в этих полях заголовка.
- **ResponseHeaderConfiguration**: этот объект используется для указания полей заголовка ответа, которые необходимо повторно создать, и нового значения, в котором нужно повторно создать исходные заголовки.
- **ActionSet**: этот объект содержит конфигурации запросов и ответов заголовков, указанных выше.
- **RewriteRule**: этот объект содержит все указанные выше *actionSets*.
- **RewriteRuleSet**: этот объект содержит все *rewriteRules* и должен быть подключен к базовому правилу или правилу маршрутизации запроса на основе путей.

```azurepowershell
$requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-isThroughProxy" -HeaderValue "True"
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -HeaderValue "max-age=31536000"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name rewriteRule1 -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name rewriteRuleSet1 -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Получение сведений о конфигурации существующего шлюза приложения

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Получение сведений о конфигурации существующего правила маршрутизации запроса

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name Rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Обновление шлюза приложения с помощью конфигурации для перезаписи заголовков HTTP

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name rewriteRuleSet1 -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Удаление правила переопределения

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "rewriteRuleSet1" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание шлюза приложений с правилами маршрутизации на основе URL-путей](./tutorial-url-route-powershell.md)
