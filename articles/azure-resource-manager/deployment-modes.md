---
title: Режимы развертывания в Azure Resource Manager | Документация Майкрософт
description: В этой статье описывается, как с помощью Azure Resource Manager задать полный или пошаговый режим развертывания.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2019
ms.author: tomfitz
ms.openlocfilehash: 5213affe953636c46486614ee2a020d7727e1478
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57407542"
---
# <a name="azure-resource-manager-deployment-modes"></a>Режимы развертывания в Azure Resource Manager

При развертывании имеющихся ресурсов можно указать, что развертывание является добавочным или полным обновлением.  Основное различие между этими двумя режимами заключается в том, как Resource Manager обрабатывает ресурсы, имеющиеся в группе ресурсов, которых нет в шаблоне. По умолчанию используется пошаговый режим.

Для обоих режимов Resource Manager пытается создать все ресурсы, указанные в шаблоне. Если ресурс уже существует в группе ресурсов и его параметры не изменились, для этого ресурса не выполняются операции. Если значения свойств ресурса были изменены, они обновятся в ресурсе. При попытке обновить расположение или тип имеющегося ресурса развертывание завершится ошибкой. Вместо этого вы можете развернуть новый ресурс нужного вам типа с необходимым расположением.

## <a name="complete-mode"></a>Полный режим

При использовании полного режима Resource Manager **удаляет** ресурсы, которые существуют в группе ресурсов, но не указаны в шаблоне. Ресурсы, указанные в шаблоне, но не развернутые, так как [условие](resource-group-authoring-templates.md#condition) принимает значение false, не будут удалены.

Существует разница между тем, как разные типы ресурсов удаляются в полном режиме. Родительские ресурсы автоматически удаляются, если находятся не на шаблоне, который развертывается в полном режиме. Некоторые дочерние ресурсы не удаляются автоматически, если находятся вне шаблона. Однако, этот дочерний ресурс удаляется вместе с родительским. 

Например, если ваша группа ресурсов содержит зону DNS (тип ресурса Microsoft.Network/dnsZones) и запись CNAME (тип ресурса Microsoft.Network/dnsZones/CNAME), то зона DNS является родительским ресурсом для записи CNAME. Если вы используете полный режим во время развертывания и не включаете в вашем шаблоне зону DNS, то и зона DNS, и запись CNAME удаляться. Если вы включили зону DNS в вашем шаблоне, но исключили из него запись CNAME, то она будет удалена. 

Дополнительные сведения об удалении разных типов ресурсов см. в статье [Deletion of Azure resources for complete mode deployments](complete-mode-deletion.md) (Удаление ресурсов Azure для развертываний в полном режиме).

> [!NOTE]
> Только шаблоны корневого уровня поддерживают режим полного развертывания. Для [связанных или вложенных шаблонов](resource-group-linked-templates.md) необходимо использовать добавочный режим. 
>
> [Развертывания уровня подписки](deploy-to-subscription.md) полный режим не поддерживается.
>
> В настоящее время портал не поддерживает полный режим.
>

## <a name="incremental-mode"></a>Пошаговый режим

В пошаговом режиме Resource Manager **оставляет без изменения** ресурсы, которые существуют в группе ресурсов, но не указаны в шаблоне. При повторном развертывании ресурса в инкрементном режиме укажите все значения свойств для ресурса, а не только те, которые вы обновляете. Если вы не укажете определенные свойства, Resource Manager интерпретирует это обновление как перезапись таких значений.

## <a name="example-result"></a>Пример результата

Чтобы понять различие между инкрементным и полным режимом, рассмотрим следующий сценарий.

**Группа ресурсов** содержит:

* ресурс А;
* ресурс Б;
* ресурс В;

**Шаблон** содержит:

* ресурс А;
* ресурс Б;
* ресурс Г.

При развертывании в **пошаговом** режиме группа ресурсов содержит:

* ресурс А;
* ресурс Б;
* ресурс В;
* ресурс Г.

При развертывании в **полном** режиме ресурс В будет удален. Группа ресурсов содержит:

* ресурс А;
* ресурс Б;
* ресурс Г.

## <a name="set-deployment-mode"></a>Установка режима развертывания

Чтобы установить режим развертывания при развертывании с помощью PowerShell, используйте параметр `Mode`.

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Чтобы установить режим развертывания при развертывании с помощью Azure CLI, используйте параметр `mode`.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

В примере ниже показан связанный шаблон с добавочным режимом развертывания:

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о создании шаблонов Resource Manager см. в статье [Создание шаблонов диспетчера ресурсов Azure](resource-group-authoring-templates.md).
* Сведения о развертывании ресурсов см. в статье [Развертывание ресурсов с использованием шаблонов Resource Manager и Azure PowerShell](resource-group-template-deploy.md).
* Чтобы просмотреть операции для поставщика ресурсов, ознакомьтесь с [Azure REST API](/rest/api/).
