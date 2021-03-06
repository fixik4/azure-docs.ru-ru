---
title: Настройка оповещений безопасности для ролей ресурсов Azure в PIM — Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить оповещения системы безопасности для ролей ресурсов Azure AD в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 09e04e6b61d3387cb8c50c2af4eef2cfb4bec196
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2019
ms.locfileid: "58575775"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Настройка оповещений системы безопасности для ролей ресурсов Azure в PIM
Azure Active Directory (Azure AD) Privileged Identity Management (PIM) создает предупреждения при обнаружении подозрительных или небезопасных действий в вашей среде. Активированные оповещения отображаются на странице "Оповещения". 

![Страница оповещений](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Просмотр оповещений
Выберите оповещение, чтобы просмотреть список пользователей или ролей, вызвавших предупреждение, а также рекомендации по исправлению.

![Отчет об оповещении](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Оповещения
| Предупреждение | Уровень серьезности | Триггер | Рекомендации |
| --- | --- | --- | --- |
| **Ресурсу назначено слишком много владельцев** |Средний |Слишком много пользователей имеют роль владельца. |Изучите список пользователей и назначьте некоторым из них менее привилегированные роли. |
| **Ресурсу назначено слишком много постоянных владельцев** |Средний |Слишком много пользователей имеют постоянно назначенную роль. |Изучите список пользователей и для некоторых из них назначьте обязательную активацию для использования этой роли. |
| **Создана дублирующая роль** |Средний |Несколько ролей имеют одинаковые условия. |Используйте только одну из этих ролей. |


### <a name="severity"></a>Уровень серьезности
* **Высокий**. Требуется немедленное действие из-за нарушения политики. 
* **Средний**. Немедленное действие не требуется, но обнаружено потенциальное нарушение политики.
* **Низкий**. Немедленное действие не требуется, но предлагается рекомендуемое изменение политики.

## <a name="configure-security-alert-settings"></a>Настройка параметров оповещений системы безопасности
Со страницы оповещений перейдите к **параметрам**.
![Параметры](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Настройте для оповещений параметры, которые подходят для вашей среды и целей безопасности.
![Настройка параметров](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Настройка оповещений системы безопасности для ролей ресурсов Azure в PIM](pim-resource-roles-configure-alerts.md)
