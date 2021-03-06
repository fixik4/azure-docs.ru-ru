---
title: Определение терминов SAP HANA в Azure (крупные экземпляры) | Документация Майкрософт
description: Определение терминов SAP HANA в Azure (крупные экземпляры).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a8131bc953c2aba3c8d33f866cbbe9b1e232e168
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2019
ms.locfileid: "56819094"
---
# <a name="know-the-terms"></a>Определение терминов

В этом руководстве широко используются некоторые общие определения. Ознакомьтесь со следующими терминами и их описанием.

- **IaaS**: Инфраструктура как услуга.
- **PaaS**: Платформа как услуга.
- **SaaS**: Программное обеспечение как услуга.
- **Компонент SAP**: Отдельное приложение SAP, например ERP Central Component (ECC), Business Warehouse (BW), Solution Manager или Enterprise Portal (EP). Компоненты SAP могут основываться на традиционной технологии ABAP или Java или быть приложениями не на основе NetWeaver, например бизнес-объектами.
- **Среда SAP**: Один или несколько компонентов SAP, логически сгруппированных для выполнения бизнес-функций, например разработки, контроля качества, обучение, аварийное восстановление или рабочей среде.
- **Ландшафт SAP**: Ссылается на все ресурсы SAP, доступные в ИТ-ландшафте. Ландшафт SAP включает все рабочие и нерабочие среды.
- **Системы SAP**: Сочетание уровня СУБД и уровня приложений, например, системы разработки SAP ERP, SAP BW тестирования системы и рабочей системы SAP CRM. В развертываниях Azure не поддерживается разделение этих двух уровней между локальной средой и Azure. Система SAP должна быть развернута полностью в локальной среде или полностью в Azure. При этом вы можете развернуть различные системы ландшафта SAP как в Azure, так и локально. Например, системы разработки и тестирования SAP CRM можно развернуть в Azure, а рабочую систему SAP CRM — в локальной среде. Решение SAP HANA в Azure (крупные экземпляры) предназначено для размещения уровня приложений систем SAP на виртуальных машинах и соответствующего экземпляра SAP HANA в единице в стеке крупных экземпляров SAP HANA в Azure.
- **Стек крупных экземпляров**: Аппаратный стек, сертифицированные SAP HANA TDI, предназначенный для запуска экземпляров SAP HANA в Azure.
- **SAP HANA в Azure (крупные экземпляры):** Официальное название предложения в Azure для запуска экземпляров HANA в на оборудовании, сертифицированном для программы SAP HANA TDI, который развертывается в стеках крупных экземпляров в разных регионах Azure. Связанный термин *крупные экземпляры HANA* — сокращенное название *SAP HANA в Azure (крупные экземпляры)*, которое широко используется в этом техническом руководстве по развертыванию.
- **Распределенное развертывание**: Описывает сценарии, в которых развернуты виртуальные машины с подпиской Azure, которая имеет сайт сайт, многосайтового или подключение Azure ExpressRoute между локальными центрами обработки данных и Azure. В документации Azure развертывания такого рода также называются распределенными развертываниями. Такое подключение используется для расширения в Azure локальных доменов, локальной службы Active Directory, OpenLDAP и локальной службы DNS. Локальная среда распространяется на ресурсы Azure, входящие в подписки Azure. При таком расширении виртуальные машины могут быть частью локального домена. 

   Пользователи локального домена имеют доступ к серверам и могут запускать службы на этих виртуальных машинах (например, службы СУБД). Возможно взаимодействие и разрешение имен между виртуальными машинами, развернутыми в локальной сети и в Azure. Это типичный сценарий, при котором развертывается большинство ресурсов SAP. Дополнительные сведения см. в разделе [VPN-шлюза Azure](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) и [Создание виртуальной сети с подключением типа сеть — сеть с помощью портала Azure](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Tenant:** Пользователь, развернутый в стеке крупных экземпляров HANA, изолируется в *клиента.* Клиент изолируется от других клиентов на уровне сети, хранилища и вычислений. Таким образом, единицы хранения и вычисления, назначенные разным клиентам, не могут видеть друг друга или взаимодействовать друг с другом на уровне стека крупных экземпляров HANA. Пользователь может выполнить развертывание в разные клиенты. Но и в таком случае клиенты не будут взаимодействовать на уровне стека крупных экземпляров HANA.
- **Категория номеров SKU**: Для крупных экземпляров HANA предлагаются следующие две категории номеров SKU:
    - **Тип класса**: S72, S72m, S96, S144, S144m, S192, S192m и S192xm
    - **Класс типа II**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm и S960m


Существует множество дополнительных ресурсов, посвященных развертыванию рабочих нагрузок SAP в облаке. Если вы планируете развернуть SAP HANA в Azure, рекомендуется ознакомиться с особенностями сред Azure IaaS и подготовиться к развертыванию рабочих нагрузок SAP в такой среде. Прежде чем продолжить работу с руководством, ознакомьтесь со статьей [Размещение и выполнение сценариев рабочей нагрузки SAP с помощью Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

**Дальнейшие действия**
- См. раздел [HLI Certification](hana-certification.md) (Сертификация HLI)