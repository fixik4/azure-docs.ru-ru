---
title: Просмотр данных об использовании общедоступных IP-адресов в Azure Stack | Документация Майкрософт
description: Администраторы могут просматривать сведения об использовании общедоступных IP-адресов в регионе
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mabrigg
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 2cb1dde60f8d8aa76e0b678347df9be120c39e7c
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57782111"
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Просмотр данных об использовании общедоступных IP-адресов в Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Как администратор облака вы можете просмотреть:
 - количество общедоступных IP-адресов, выделенных для клиентов;
 - количество общедоступных IP-адресов, которые еще доступны для выделения;
 - процент общедоступных IP-адресов, выделенных в этом расположении.

Плитка **Public IP pools usage** (Использование пулов общедоступных IP-адресов) отражает общее количество общедоступных IP-адресов, используемых во всех пулах этих адресов. Для каждого IP-адреса эта плитка также отражает сведения об его использовании для экземпляров виртуальных машин IaaS, служб инфраструктуры структуры и ресурсов общедоступного IP-адреса, которые были явно созданы клиентами.

Эта плитка предназначена для предоставления операторам Azure Stack представления о количестве общедоступных IP-адресов, которые используются в этом расположении. Эти сведения помогают администраторам определить, заканчиваются ли доступные IP-адреса.

В пункте меню **Общедоступные IP-адреса** в разделе **Tenant Resources** (Ресурсы клиента) перечислены только те общедоступные IP-адреса, которые были *явно созданы клиентами*. Этот пункт меню можно найти, выбрав в колонке **Поставщики ресурсов** область **Сеть**. Число **использованных** общедоступных IP-адресов на плитке **Public IP pools usage** (Использование пулов общедоступных IP-адресов) всегда отличается от (больше) числа на плитке **Общедоступные IP-адреса** в разделе **Tenant Resources** (Ресурсы клиента).

## <a name="view-the-public-ip-address-usage-information"></a>Просмотр данных об использовании общедоступных IP-адресов

Чтобы просмотреть количество всех общедоступных IP-адресов, которые были использованы в регионе:

1. На портале администратора Azure Stack выберите **Все службы**. Затем в категории **Администрирование** выберите **Сеть**.
1. В области **Сеть** в разделе **Обзор** показана плитка **Public IP pools usage** (Использование пулов общедоступных IP-адресов).

![Область "Сеть", колонка "Поставщики ресурсов"](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

Число в разделе **Использовано** — это количество присвоенных общедоступных IP-адресов из пулов общедоступных IP-адресов. В разделе **Free** (Свободные) представлено число общедоступных IP-адресов из пулов общедоступных IP-адресов, которые не были присвоены и все еще доступны. Число **% Used** (% использованных) представляет число использованных или присвоенных адресов в процентах от общего количества общедоступных IP-адресов из пулов общедоступных IP-адресов в выбранном расположении.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Просмотр общедоступных IP-адресов, созданных подписками клиента

Выберите **Общедоступные IP-адреса** в разделе **Tenant Resources** (Ресурсы клиента). Просмотрите список общедоступных IP-адресов, которые были явным образом созданы в подписках клиентов в определенном регионе.

![Общедоступные IP-адреса клиента](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Обратите внимание, что некоторые общедоступные IP-адреса, которые были динамически выделены, отображаются в списке, но еще не имеют связанного с ними адреса. Ресурс адресов был создан в поставщике сетевых ресурсов, а не в сетевом контроллере.

Сетевой контроллер не присваивает адрес ресурсу, пока он не связан с интерфейсом, сетевым адаптером, балансировщиком нагрузки или шлюзом виртуальной сети. Когда общедоступный IP-адрес связан с интерфейсом, сетевой контроллер выделяет для него IP-адрес. Этот адрес появляется в поле **Адрес**.

## <a name="view-the-public-ip-address-information-summary-table"></a>Просмотр сводной таблицы данных об общедоступных IP-адресах

Есть несколько случаев, когда от типа общедоступного IP-адреса и объекта, которому он присвоен, зависит, отображается ли адрес в том или ином списке.

| **Вариант присвоения общедоступных IP-адресов** | **Отображается в сводке потребления** | **Отображается в списке общедоступных IP-адресов клиентов** |
| --- | --- | --- |
| Динамический общедоступный IP-адрес еще не присвоен сетевому адаптеру или балансировщику нагрузки (временно). |Нет  |Yes |
| Динамический общедоступный IP-адрес присвоен сетевому адаптеру или балансировщику нагрузки. |Yes |Yes |
| Статичный общедоступный IP-адрес присвоен сетевому адаптеру или балансировщику нагрузки клиента. |Yes |Yes |
| Статический общедоступный IP-адрес присвоен конечной точке службы инфраструктуры. |Yes |Нет  |
| Общедоступный IP-адрес неявно создан для экземпляров виртуальных машин IaaS и используется для исходящего подключения NAT в виртуальной сети. Сопутствующие ресурсы создаются в фоновом режиме, когда клиент создает экземпляр виртуальной машины, чтобы виртуальные машины могли отправлять информацию в Интернет. |Yes |Нет  |

## <a name="next-steps"></a>Дополнительная информация

[Управление учетными записями хранения в Azure Stack](azure-stack-manage-storage-accounts.md)