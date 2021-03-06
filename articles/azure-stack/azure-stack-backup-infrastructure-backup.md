---
title: Резервное копирование и восстановление данных для Azure Stack с помощью службы резервного копирования инфраструктуры | Документация Майкрософт
description: Вы можете выполнить резервное копирование данных конфигурации и службы и восстановить их с помощью службы резервного копирования инфраструктуры.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: mabrigg
ms.reviewer: hectorl
ms.lastreviewed: 03/19/2019
ms.openlocfilehash: 080129ca1520dc2b1b085c69f6389508f11c7ba2
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58285927"
---
# <a name="backup-and-data-recovery-for-azure-stack-with-the-infrastructure-backup-service"></a>Резервное копирование и восстановление данных для Azure Stack с помощью службы резервного копирования инфраструктуры

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Вы можете выполнить резервное копирование данных конфигурации и службы и восстановить их с помощью службы резервного копирования инфраструктуры. Каждая установка Azure Stack содержит экземпляр службы. Резервные копии, созданные в службе для повторного развертывания в облаке Azure Stack, можно использовать для восстановления данных удостоверений, системы безопасности и Azure Resource Manager. 

Вы можете включить резервное копирование, как только облако будет готово к использованию в рабочей среде. Не включайте резервное копирование, если планируете выполнять длительное тестирование и проверку.

Прежде чем включить службу резервного копирования, убедитесь, что соблюдены все [требования](#verify-requirements-for-the-infrastructure-backup-service).

> [!Note]  
> Служба резервного копирования инфраструктуры не обрабатывает данные и приложения пользователя. Дополнительные сведения о защите приложений, развернутых на виртуальных машинах IaaS, см. в статье [Защита виртуальных машин, развернутых в Azure Stack](user/azure-stack-manage-vm-protect.md). Подробное описание защиты приложений в Azure Stack см. в этом техническом документе с [рекомендациями по обеспечению непрерывности бизнес-процессов и аварийного восстановления для Azure Stack](http://aka.ms/azurestackbcdrconsiderationswp).

## <a name="the-infrastructure-backup-service"></a>Служба резервного копирования инфраструктуры

Служба содержит следующие компоненты:

| Функция                                            | ОПИСАНИЕ                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Службы резервного копирования инфраструктуры                     | Управление резервным копированием для ряда служб инфраструктуры Azure Stack. В аварийной ситуации можно восстановить данные в процессе повторного развертывания. |
| Сжатие и шифрование экспортированных данных резервного копирования | Резервные копии данных сжимаются и шифруются в системе перед экспортом во внешнее место хранения, предоставленное администратором.                |
| Мониторинг заданий резервного копирования                              | В случае сбоя заданий резервного копирования система отправляет уведомления и рекомендации по устранению неполадок.                                                                                                |
| Интерфейс управления резервным копированием                       | Поставщик ресурсов резервного копирования поддерживает включение резервного копирования.                                                                                                                         |
| Восстановление в облаке                                     | В случае катастрофической потери данных можно использовать резервные копии для восстановления данных Azure Stack в процессе развертывания.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Проверка выполнения требований для службы резервного копирования инфраструктуры

- **Место хранения**  
  Требуется общий файловый ресурс, который доступен из Azure Stack и может содержать семь резервных копий. Размер каждой резервной копии составляет около 10 ГБ. В вашем общем ресурсе должно быть пространство для хранения резервных копий объемом 140 ГБ. Дополнительные сведения о выборе места хранения для службы резервного копирования инфраструктуры Azure Stack см. в разделе с [требованиями для контроллера резервного копирования](azure-stack-backup-reference.md#backup-controller-requirements).
- **Учетные данные**  
  Требуются учетная запись и учетные данные пользователя домена. Например, вы можете использовать учетные данные администратора Azure Stack.
- **Сертификат шифрования**  
  Файлы резервных копий шифруются с помощью открытого ключа в сертификате. Обязательно храните этот сертификат в безопасном расположении. 


## <a name="next-steps"></a>Дополнительная информация

Узнайте, как [включить резервное копирование для Azure Stack на портале администрирования](azure-stack-backup-enable-backup-console.md).

Узнайте, как [включить резервное копирование для Azure Stack с помощью PowerShell](azure-stack-backup-enable-backup-powershell.md).

Узнайте, как [выполнить резервное копирование для Azure Stack](azure-stack-backup-back-up-azure-stack.md ).

Узнайте, как [выполнить восстановление после катастрофической потери данных](azure-stack-backup-recover-data.md).
