---
title: Общие сведения об iDNS в Azure Stack | Документация Майкрософт
description: Общие сведения о компонентах и возможностях iDNS в Azure Stack
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
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: ab867af76821f90c6a87c08d42affdef8192e201
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258145"
---
# <a name="introducing-idns-for-azure-stack"></a>Общие сведения об iDNS для Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

iDNS — это функция сетевого взаимодействия Azure Stack, которая позволяет сопоставлять внешние имена DNS (например, https:\//www.bing.com.) и регистрировать имена внутренней виртуальной сети. Таким образом, можно разрешить виртуальные машины в одной и той же виртуальной сети по имени, а не по IP-адресу. Это избавляет от необходимости предоставлять пользовательские записи DNS-сервера. Общие сведения о DNS см. в [этой статье](https://docs.microsoft.com/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>Возможности iDNS

С iDNS в Azure Stack вы получаете следующие возможности без необходимости указывать пользовательские записи DNS-сервера:

- Общие службы разрешения DNS-имен для клиентских рабочих нагрузок.
- Полномочная служба DNS для разрешения имен и регистрации DNS в клиентской виртуальной сети.
- Рекурсивная служба DNS для разрешения имен Интернета из клиентских виртуальных машин. Клиенту больше не требуется указывать пользовательские DNS записи для разрешения имен Интернета (например, www.bing.com).

Вы по-прежнему можете подключить собственный DNS-сервер и использовать пользовательские DNS-серверы. Однако с помощью iDNS можно разрешать имена DNS Интернета и подключаться к другим виртуальным машинам в той же виртуальной сети. Не нужно создавать пользовательские записи DNS.

## <a name="what-doesnt-idns-do"></a>Что не делает iDNS?

iDNS не позволяет создать запись DNS для имени, которое можно разрешить за пределами виртуальной сети.

В Azure можно указать метку имени DNS, которая связана с общедоступным IP-адресом. Вы можете выбрать метку (префикс), но Azure выбирает суффикс, зависящий от региона, в котором создается общедоступный IP-адрес.

![Пример метки имени DNS](media/azure-stack-understanding-dns-in-tp2/image3.png)

На приведенном выше рисунке показано создание платформой Azure записи "A" в DNS для метки имени DNS, указанной в зоне **westus.cloudapp.azure.com**. Вместе префикс и суффикс образуют [полное доменное имя](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN), которое можно разрешить в любом месте в общедоступном Интернете.

В Azure Stack iDNS поддерживается только для внутренней регистрации имени, поэтому iDNS не позволяет выполнить следующие действия:

- создать запись DNS в существующей размещенной зоне DNS (например, local.azurestack.external);
- создать зону DNS (например, Contoso.com);
- создать запись в пользовательской зоне DNS;
- купить доменное имя.

## <a name="next-steps"></a>Дополнительная информация

[DNS в Azure Stack](azure-stack-dns.md)
