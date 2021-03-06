---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 555a8e3e92dc1d12cb7c6d6e06d2511f15a2c862
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52973125"
---
|**SKU**   | **Туннели<br>S2S или "виртуальная сеть — виртуальная сеть"** | **Подключение "точка — сеть"<br> SSTP-подключения** | **Подключение "точка — сеть"<br> IKEv2-подключения** | **Эталонная агрегированная<br>пропускная способность** | **BGP** |
|---       | ---        | ---       | ---            | ---       | --- |
|**базовая;** | Макс. 10    | Макс. 128  | Не поддерживается  | 100 Мбит/с  | Не поддерживается|
|**VpnGw1**| Макс. 30 *   | Макс. 128  | Макс. 250       | 650 Мбит/с  | Поддерживаются |
|**VpnGw2**| Макс. 30 *   | Макс. 128  | Макс. 500       | 1 Гбит/с    | Поддерживаются |
|**VpnGw3**| Макс. 30 *   | Макс. 128  | Макс. 1000      | 1,25 Гбит/с | Поддерживаются |


(*)Используйте [Виртуальную глобальную сеть](../articles/virtual-wan/virtual-wan-about.md), если требуется больше 30 VPN-туннелей S2S.

* Эталонная агрегированная пропускная способность основана на измерениях нескольких туннелей, агрегированных для одного шлюза. Эталонная агрегированная пропускная способность для VPN-шлюза — сумма показателей S2S и P2S. **Если у вас много подключений "точка — сеть", это может отрицательно повлиять на подключение "сеть — сеть" из-за ограничений пропускной способности.** Эталонная агрегированная пропускная способность не гарантируется, так как этот параметр зависит от характеристик интернет-трафика и особенностей работы вашего приложения.

* Эти ограничения подключений являются раздельными. Например, у вас может быть 128 SSTP-подключений, а также 250 IKEv2 для номера SKU VpnGw1.

* Дополнительные сведения о ценах можно просмотреть на [этой странице](https://azure.microsoft.com/pricing/details/vpn-gateway) .

* См. сведения о [соглашении об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Номера SKU VpnGw1, VpnGw2 и VpnGw3 поддерживаются только для VPN-шлюзов, созданных на основе модели развертывания с помощью Resource Manager.

* Номер SKU "Базовый" считается устаревшим. Номер SKU Basic имеет ограничения некоторых функций. Вы не можете изменить размер шлюза, который использует SKU Basic, на один из новых номеров SKU шлюза. Вместо этого нужно использовать новый номер SKU с предварительным удалением и повторным созданием шлюза VPN. Проверьте, поддерживается ли необходимая функция, перед использованием номера SKU Basic.
