---
title: Поддерживаемые операционные системы на виртуальной машине для Azure Stack | Документация Майкрософт
description: В Azure Stack можно использовать эти операционные системы на виртуальной машине.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/17/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 1ab59dfc42213fc5c5ce22841e572e2724e239aa
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58119009"
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Поддерживаемые операционные системы на виртуальной машине для Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

## <a name="windows"></a> Windows

Azure Stack поддерживает гостевые операционные системы Windows, перечисленные в таблице ниже.

| Операционная система | ОПИСАНИЕ | Наличие в marketplace |
| --- | --- | --- |
| Windows Server, версия1709 | 64-разрядная | Core с контейнерами |
| Windows Server 2016 | 64-разрядная |  Центр обработки данных, ядро центра обработки данных, центр обработки данных с контейнерами |
| Windows Server 2012 R2 | 64-разрядная |  Центр обработки данных |
| Windows Server 2012 | 64-разрядная |  Центр обработки данных |
| Windows Server 2008 R2 с пакетом обновления 1 | 64-разрядная |  Центр обработки данных |
| Windows Server 2008 с пакетом обновления 2 (SP2) | 64-разрядная |  Использование собственного образа |
| Windows 10 *(см. примечание 1)* | 64-разрядная версия, Pro и Корпоративная | Использование собственного образа |

> [!NOTE]
> Чтобы развернуть клиентские операционные системы Windows 10 в Azure Stack требуются [лицензии Windows на каждого пользователя](https://www.microsoft.com/en-us/Licensing/product-licensing/windows10.aspx). Лицензии также можно приобрести у уполномоченного поставщика услуг многоклиентного размещения ([QMTH](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)).

Образы marketplace доступны для лицензирования по схеме "оплата по мере использования" или BYOL (EA и SPLA). Использование обеих схем в одном экземпляре Azure Stack не поддерживается. Во время развертывания Azure Stack внедряет подходящую версию гостевого агента в образ.

Выпуски Datacenter доступны для скачивания в marketplace. Клиенты могут использовать собственные образы серверов, включая другие выпуски. В Marketplace недоступны клиентские образы версий Windows.

## <a name="linux"></a>Linux

Перечисленные доступные в marketplace дистрибутивы Linux включают в себя необходимый агент Linux для Microsoft Azure (WALA). Если вы используете в Azure Stack собственный образ, следуйте указаниям в разделе [Добавление образов Linux в Azure Stack](azure-stack-linux.md).

> [!NOTE]
> Пользовательские образы должны содержать последнюю общедоступную версию WALA или версию 2.2.20. Версии ниже 2.2.20 и от 2.2.20 до 2.2.36 могут неправильно работать в Azure Stack. 
>
> В настоящее время [cloud-init](https://cloud-init.io/) не поддерживается в Azure Stack.

| Дистрибутив | ОПИСАНИЕ | ИЗДАТЕЛЬ | Marketplace |
| --- | --- | --- | --- |
| Версия 6.9 на основе CentOS | 64-разрядная | Rogue Wave | Yes |
| Версия 7.4 на основе CentOS | 64-разрядная | Rogue Wave | Yes |
| ClearLinux | 64-разрядная | ClearLinux.org | Yes |
| Контейнер Linux |  64-разрядная | CoreOS | Stable |
| Debian 8 "Jessie" | 64-разрядная | credativ |  Yes |
| Debian 9 "Stretch" | 64-разрядная | credativ | Yes |
| Red Hat Enterprise Linux 7.x | 64-разрядная | Red Hat |Использование собственного образа |
| SLES 11SP4 | 64-разрядная | SUSE | Yes |
| SLES 12SP3 | 64-разрядная | SUSE | Yes |
| Ubuntu 14.04-LTS | 64-разрядная | Canonical | Yes |
| Ubuntu 16.04-LTS | 64-разрядная | Canonical | Yes |
| Ubuntu 18.04-LTS | 64-разрядная | Canonical | Yes |

Сведения о поддержке Red Hat Enterprise Linux приведены в статье [Red Hat и Azure Stack: часто задаваемые вопросы](https://access.redhat.com/articles/3413531).

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure Stack Marketplace см. в следующих статьях:

- [Скачивание элементов marketplace](azure-stack-download-azure-marketplace-item.md)  
- [Создание и публикация элемента Marketplace](azure-stack-create-and-publish-marketplace-item.md)
