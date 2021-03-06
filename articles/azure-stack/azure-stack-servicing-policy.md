---
title: Политика обслуживания Azure Stack | Документация Майкрософт
description: Информация о политике обслуживания Azure Stack и рекомендации по сохранению поддерживаемого состояния для интегрированной системы.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: harik
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: a61820dc05752d43774b13399d071c5a2be98483
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400529"
---
# <a name="azure-stack-servicing-policy"></a>Политика обслуживания Azure Stack

В этой статье описаны политика обслуживания для интегрированных систем Azure Stack и действия, необходимые для сохранения поддерживаемого состояния системы.

## <a name="download-update-packages-for-integrated-systems"></a>Загрузка пакетов обновления для интегрированных систем

Корпорация Майкрософт выпускает оба полных ежемесячных пакета обновления, а также пакеты исправления для устранения конкретных неполадок.

Ежемесячные пакеты обновления размещаются в защищенной конечной точке Azure. Вы можете загрузить их вручную с помощью [средства загрузки обновлений Azure Stack](https://aka.ms/azurestackupdatedownload). Если единица масштабирования подключена, обновление автоматически отображается на портале администратора как **Доступно обновление**. Полные ежемесячные пакеты обновления хорошо задокументированы в каждом выпуске. Чтобы получить дополнительные сведения о каждом выпуске, нажмите на любой выпуск в разделе [Последовательность выпуска пакетов обновления](#update-package-release-cadence) в этой статье.

Пакеты исправления размещаются в той же защищенной конечной точке Azure. Вы можете загрузить их вручную с помощью встроенных ссылок в каждой соответствующей статье базы знаний об исправлении, например [Исправление Azure Stack 1.1809.12.114](https://support.microsoft.com/help/4481548/azure-stack-hotfix-1-1809-12-114). Как и с ежемесячными пакетами обновления, операторы Azure Stack могут скачать файлы .xml, .bin и .exe и импортировать их, как описано в разделе [Применение обновлений в Azure Stack](azure-stack-apply-updates.md). Для операторов Azure Stack с подключенными единицами масштабирования исправления автоматически отображаются на портале администратора с сообщением **Доступно обновление**.

Если ваша единица масштабирования не подключена, а вы хотите получать уведомления о каждом выпуске исправления, подпишитесь на каналы [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss) или [ATOM](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom), указанные в каждом выпуске.  

## <a name="update-package-types"></a>Типы пакетов обновления

Существуют два типа пакетов обновления для интегрированных систем.

- **Обновления программных продуктов корпорации Майкрософт.** Корпорация Майкрософт отвечает за весь жизненный цикл пакетов обновлений для программного обеспечения корпорации Майкрософт. Такие пакеты могут включать свежие обновления безопасности для Windows Server, не связанные с безопасностью обновления или обновления компонентов Azure Stack. Эти пакеты обновления вы можете получить непосредственно от корпорации Майкрософт.

- **Обновления, предоставленные производителями оборудования OEM.** Партнеры по оборудованию Azure Stack отвечают за весь жизненный цикл пакетов обновлений (и рекомендаций по их применению) для встроенного ПО оборудования и драйверов устройств. Кроме того, партнеры по оборудованию Azure Stack самостоятельно контролируют и обслуживают программное и аппаратное обеспечение на узле поддержки жизненного цикла оборудования. Поставщики оборудования OEM размещают свои пакеты обновления на собственных сайтах для загрузки обновлений.

## <a name="update-package-release-cadence"></a>Последовательность выпуска пакетов обновления

Корпорация Майкрософт планирует выпускать обновления программного обеспечения ежемесячно. Но в некоторые месяцы будет выпускаться несколько обновлений (или ни одного). Поставщики оборудования OEM выпускают обновления по мере необходимости.

Документацию по планированию обновлений и управлению ими, а также по определению текущей версии можно найти в разделе [Общие сведения об управлении обновлениями в Azure Stack](azure-stack-updates.md).

Дополнительные сведения об определенном обновлении и инструкции по его скачиванию можно получить из заметок о выпуске этого обновления:

- [Обновление 1902 для Azure Stack](azure-stack-update-1902.md)
- [Обновление 1901 для Azure Stack](azure-stack-update-1901.md)
- [Обновление 1811 для Azure Stack](azure-stack-update-1811.md)
- [Обновление 1809 для Azure Stack](azure-stack-update-1809.md)

## <a name="hotfixes"></a>Исправления

Время от времени корпорация Майкрософт выпускает исправления для Azure Stack, которые устраняют определенную проблему и являются профилактическими или срочными.  Для каждого выпущенного исправления публикуется соответствующая статья в базе знаний Майкрософт, в которой описывается проблема, причина и решение.

Исправления можно скачать и установить точно так же, как обычные полные пакеты обновления для Azure Stack. Однако, в отличие от полного обновления, исправления можно установить всего за несколько минут. Мы рекомендуем операторам Azure Stack устанавливать периоды обслуживания для установки исправлений. Исправления обновляют версию облака Azure Stack, поэтому можно легко определить, применено ли исправление. Для каждой версии Azure Stack, для которой предоставляется поддержка, выпускается отдельное исправление. Каждое исправление для конкретной итерации является накопительным и включает в себя предыдущие обновления для той же версии. Вы можете узнать больше о назначении определенного исправления из соответствующей статьи базы знаний об исправлении.  

## <a name="keep-your-system-under-support"></a>Сохранение поддержки для вашей системы

Чтобы продолжать получать поддержку, необходимо сохранить текущее развертывание Azure Stack. Политика откладывания обновлений: чтобы поддержка вашего развертывания Azure Stack сохранялась, оно должно использовать последнюю выпущенную версию обновления или одну из двух предыдущих основных версий обновления. Исправления не считаются основными версиями обновления. Если облако Azure Stack отстает *больше чем на два обновления*, считается, что оно не соответствует требованиям и должно быть обновлено до минимальной поддерживаемой версии для получения поддержки.

Например, если последняя доступная версия обновления 1805, а предыдущие два пакета обновления имели версии 1804 и 1803, они также будут поддерживаться. Однако версия 1802 поддерживаться не будет. Политика справедлива, если на протяжении двух месяцев нет обновлений. Например, если текущий выпуск 1805, а выпуска 1804 не было, предыдущие два пакета обновления 1803 и 1802 будут поддерживаться.

Пакеты обновления для программного обеспечения Майкрософт не являются накопительными. Обязательное предварительное условие для каждого из них — установка всех предыдущих пакетов обновления или исправлений. Если вы откладываете применение одного или нескольких обновлений, учитывайте общее время обновления до последней версии.

## <a name="get-support"></a>Получение поддержки

Azure Stack использует тот же процесс поддержки, что и Azure. Клиенты уровня "Корпоративный" могут следовать процессу, описанному в разделе [Создание запроса на поддержку Azure](/azure/azure-supportability/how-to-create-azure-support-request). Если вы являетесь клиентом поставщика облачных служб (CSP), обратитесь к своему CSP для получения поддержки.  Дополнительные сведения см. на странице [Часто задаваемые вопросы о поддержке Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Дополнительная информация

- [Управления обновлениями в Azure Stack](azure-stack-updates.md)