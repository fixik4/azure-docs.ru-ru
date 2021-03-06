---
title: Введение в Пакет средств разработки Azure Stack (ASDK) | Документация Майкрософт
description: Из этой статьи вы узнаете, какие типичные варианты используются для оценки Microsoft Azure Stack и что такое ASDK.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 54eb2ff43a5f36999294b8d0c580bc425ab65b28
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629073"
---
# <a name="what-is-the-azure-stack-development-kit"></a>Что такое Пакет средств разработки Azure Stack?
[Интегрированные системы Microsoft Azure Stack](../azure-stack-poc.md) размером от 4 до 16 узлов, которые совместно поддерживают партнер по оборудованию и корпорация Майкрософт. Использование интегрированных систем Azure Stack позволяет реализовать новые сценарии для производственных рабочих нагрузок. Если вы оператор Azure Stack, который управляет инфраструктурой интегрированных систем и предлагает службы, ознакомьтесь с нашей [документацией для операторов](https://docs.microsoft.com/azure/azure-stack).

Пакет средств разработки Azure Stack (ASDK) — это развертывание Azure Stack с использованием одного узла, которое можно скачать и применять **бесплатно**. Все компоненты ASDK устанавливаются на виртуальных машинах, работающих на едином главном компьютере сервера. Эти компоненты должны соответствовать [минимальным требованиям к оборудованию](asdk-deploy-considerations.md#hardware) или превышать их. Пакет ASDK предоставляет среду, в которой можно оценить возможности Azure Stack и разрабатывать современные приложения с помощью API-интерфейсов и средств, совместимых с Azure, в *непроизводственной* среде. 

> [!IMPORTANT]
> ASDK не предназначен для использования или поддержки в рабочей среде.

Так как все компоненты ASDK развертываются на едином главном компьютере пакета разработки, для них доступны ограниченные физические ресурсы. При развертывании ASDK виртуальные машины инфраструктуры Azure Stack и виртуальные машины клиентов сосуществуют на одном и том же компьютере сервера. Эта конфигурация не предназначена для оценки масштабируемости или производительности.

ASDK предназначен для обеспечения согласованной работы в гибридном облаке в Azure для таких специалистов:
- **Администраторов** (операторов Azure Stack). ASDK — это отличный ресурс для оценки доступных служб Azure Stack и получения дополнительных сведений о них.
- **Разработчиков**. ASDK можно использовать для локальной разработки гибридных или современных приложений (среды разработки и тестирования). Это обеспечивает возможность воспроизведения разработки до выпуска рабочих развертываний Azure Stack или возможность параллельного выполнения этих процедур. 

Дополнительные сведения об ASDK можно узнать из видео:

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>Разница между ASDK и многоузловой инфраструктурой Azure Stack
Ниже представлены важные различия одноузловых развертываний ASDK и многоузловых развертываний Azure Stack, на которые следует обратить внимание.

|ОПИСАНИЕ|ASDK|Многоузловая Azure Stack|
|-----|-----|-----|
|**Масштабирование**|Все компоненты установлены на одноузловой серверный компьютер.|Размер варьируется от 4 до 16 узлов.|
|**Устойчивость**|Конфигурация с одним узлом не обеспечивает высокую доступность.|Поддерживаются возможности [высокой доступности](../azure-stack-overview.md#providing-high-availability).|
|**Сеть**|Главный компьютер с ASDK маршрутизирует весь сетевой трафик ASDK. Отсутствуют дополнительные требования для коммутатора.|Развертываниям с несколькими узлами необходима более сложная [сетевая инфраструктура](../azure-stack-network.md#network-infrastructure), включая коммутатор TOR, контроллер управления основной платой (BMC) и пограничные коммутаторы (сети центров обработки данных).|
|**Patch and update process** (Процесс исправления и обновления)|Чтобы переместиться в новую версию ASDK, необходимо повторно развернуть ASDK на главном компьютере пакета средств разработки.|Процесс [исправления и обновления](../azure-stack-updates.md) используется для обновления установленной версии Azure Stack.|
|**Поддержка**|Форум MSDN Azure Stack. Поддержка службы поддержки пользователей Майкрософт (CSS) *недоступна* для нерабочих сред.|[Форум MSDN Azure Stack](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) и полная поддержка CSS.|
| | |

## <a name="learn-about-available-services"></a>Дополнительные сведения о доступных службах
Оператору Azure Stack необходимо знать, какие службы можно сделать доступными для пользователей. Azure Stack поддерживает подмножество служб Azure. Со временем их список будет расти.

### <a name="foundational-services"></a>Базовые службы
По умолчанию Azure Stack включает следующие "базовые службы" при развертывании ASDK.
- Службы вычислений
- Хранилище
- Сеть
- Key Vault

С этими базовыми службами вы можете предложить своим пользователям инфраструктуру как услугу (IaaS) с минимальной конфигурацией.

### <a name="additional-services"></a>Дополнительные службы
Сейчас поддерживаются следующие дополнительные службы платформы как услуги (PaaS):
- Служба приложений
- Функции Azure
- Базы данных SQL и MySQL

> [!NOTE]
> Эти службы нужно дополнительно настроить, прежде чем вы сможете сделать их доступными для пользователей. После установки ASDK они недоступны по умолчанию.

## <a name="service-roadmap"></a>Стратегия развития службы
Azure Stack будет расширять поддержку дополнительных служб Azure. Дополнительные сведения о нововведениях в Azure Stack см. на странице [стратегии развития Azure](https://azure.microsoft.com/roadmap/?tag=azure-stack). 


## <a name="next-steps"></a>Дополнительная информация
Чтобы приступить к оценке Azure Stack, сначала необходимо [скачать последнюю версию ASDK](asdk-download.md) и подготовить главный компьютер с ASDK. После подготовки узла для пакета средств разработки, можно установить ASDK и войти на порталы администратора и пользователя для начала работы с Azure Stack.