---
title: Персональные данные Azure Resource Manager | Документация Майкрософт
description: Узнайте, как управлять персональными данными, связанными с операциями Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: db4e1b8705b879fd5716763869462bafdf1f905c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495213"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Управление персональными данными, связанными с Azure Resource Manager

Во избежание раскрытия конфиденциальной информации удаляйте персональные данные, которые могли быть предоставлены в развертываниях, группах ресурсов или тегах. Azure Resource Manager предоставляет операции, что позволяют управлять персональными данными, которые могли быть предоставлены в развертываниях, группах ресурсов или тегах.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Удаление персональных данных в журнале развертывания

Для развертываний Resource Manager сохраняет значения параметров и сообщения о состоянии в журнал развертывания. Эти значения сохраняются до удаления развертывания из журнала. Чтобы проверить, предоставлялись ли персональные данные в этих значениях, выведите список развертываний. Если найдете персональные данные, то удалите развертывание из журнала.

Для вывода списка **развертываний** в журнале используйте:

* [вывод списка по группе ресурсов](/rest/api/resources/deployments/listbyresourcegroup);
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az group deployment list](/cli/azure/group/deployment#az-group-deployment-list).

Чтобы удалить **развертывания** из журнала, используйте:

* [Удалить](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az group deployment delete](/cli/azure/group/deployment#az-group-deployment-delete).

## <a name="delete-personal-data-in-resource-group-names"></a>Удаление персональных данных в именах групп ресурсов

Имя группы ресурсов сохраняется до удаления группы ресурсов. Чтобы проверить, предоставлялись ли персональные данные в именах, выведите список групп ресурсов. Если найдете персональные данные, то [переместите ресурсы](resource-group-move-resources.md) в новую группу ресурсов и удалите группу ресурсов с персональными данными в имени.

Для вывода списка **групп ресурсов** используйте:

* [Список](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [az group list](/cli/azure/group#az-group-list).

Для удаления **групп ресурсов** используйте:

* [Удалить](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Удаление персональных данных в тегах

Имена и значения тегов сохраняются до удаления или изменения тега. Чтобы проверить, предоставлялись ли персональные данные в тегах, выведите список тегов. Если вы найдете персональные данные, то удалите теги.

Для вывода списка **тегов** используйте:

* [Список](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [az tag list](/cli/azure/tag#az-tag-list).

Для удаления **тегов** используйте:

* [Удалить](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [az tag delete](/cli/azure/tag#az-tag-delete).

## <a name="next-steps"></a>Дополнительная информация
* Общие сведения об Azure Resource Manager см. в [этой статье](resource-group-overview.md).