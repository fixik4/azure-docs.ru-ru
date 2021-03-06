---
title: Вопросы и ответы по Windows Server для Azure Stack | Документация Майкрософт
description: Список статей в Azure Stack Marketplace с вопросами и ответами по Windows Server
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 11/12/2018
ms.openlocfilehash: b498283ee117935438c55e5162e92acdb0f5ebfe
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56649906"
---
# <a name="windows-server-in-azure-stack-marketplace-faq"></a>Windows Server в Azure Stack Marketplace: вопросы и ответы

В этой статье содержатся ответы на распространенные вопросы об образах Windows Server в [Azure Stack Marketplace](azure-stack-marketplace.md).

## <a name="marketplace-items"></a>Элементы Marketplace

### <a name="how-do-i-update-to-a-newer-windows-image"></a>Как обновить систему до нового образа Windows?

Во-первых, определите, есть ли шаблоны Azure Resource Manager, которые требуют определенной версии. Если такие шаблоны есть, обновите их или сохраните предыдущие версии образов. Лучше всего использовать параметр **version: latest**.

Во-вторых, если любой из масштабируемых наборов виртуальных машин ссылается на конкретную версию, оцените вероятность масштабирования этого набора в будущем и на основе этой оценки решите, не лучше ли сохранить старые версии. Если ни одно из этих условий не применимо, удалите старые образы из Marketplace, прежде чем скачивать более новые. Если для их скачивания ранее применялось управление Marketplace, используйте его и для удаления. Теперь скачайте более новую версию.

### <a name="what-are-the-licensing-options-for-windows-server-marketplace-images-on-azure-stack"></a>Какие варианты лицензирования существуют для образов Marketplace Windows Server в Azure Stack?

Корпорация Майкрософт предоставляет две версии образов Windows Server через Azure Stack Marketplace.

- **Оплата по мере использования**. В таких образах используются единицы измерения полной цены для Windows.
   Предназначены для: клиентов с Соглашением Enterprise (EA), которые используют *модель выставления счетов за потребление*; поставщиков облачных решений, которые не хотят использовать лицензирование SPLA.
- **С использованием собственной лицензии (BYOL)**. Эти образы выполняют базовые единицы измерения.
   Предназначены для: клиентов EA с лицензией Windows Server; поставщиков облачных решений с лицензированием SPLA.

Преимущество гибридного использования Azure (AHUB) в Azure Stack не поддерживается. Клиенты, которые приобрели лицензию по модели "Производительность", должны использовать образ BYOL. При тестировании с помощью Пакета средств разработки Azure Stack (ASDK) можно использовать любой из этих вариантов.

### <a name="what-if-i-downloaded-the-wrong-version-to-offer-my-tenantsusers"></a>Что делать, если я скачаю неправильную версию для своих клиентов или пользователей?

Сначала удалите неправильную версию с помощью управления Marketplace. Дождитесь полного завершения (то есть уведомления о завершении), не довольствуясь состоянием в колонке управления Marketplace. После этого скачайте правильную версию.

### <a name="what-if-my-user-incorrectly-checked-the-i-have-a-license-box-in-previous-windows-builds-and-they-dont-have-a-license"></a>Что делать, если мой пользователь ошибочно установил флажок "У меня есть лицензия" в предыдущих сборках Windows, но у него нет лицензии?

См. рекомендации в статье [Перевод виртуальных машин Windows Server с поддержкой преимущества обратно на модель оплаты по мере использования](../virtual-machines/windows/hybrid-use-benefit-licensing.md#powershell-1).

### <a name="what-if-i-have-an-older-image-and-my-user-forgot-to-check-the-i-have-a-license-box-or-we-use-our-own-images-and-we-do-have-enterprise-agreement-entitlement"></a>Что делать, если я использую старый образ и мой пользователь забыл установить флажок "У меня есть лицензия", либо мы используем собственные образы и имеем право в рамках Соглашения Enterprise?

См. рекомендации в статье [Преобразование существующей виртуальной машины с поддержкой программы "Преимущество гибридного использования Azure" для Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md#convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server). Обратите внимание, что Преимущество гибридного использования Azure не применяется в Azure Stack, но значение этого параметра учитывается.

### <a name="what-about-other-vms-that-use-windows-server-such-as-sql-or-machine-learning-server"></a>Что будет с другими виртуальными машинами, которые используют Windows Server, например SQL Server или Machine Learning Server?

В этих образах применяется параметр **licenseType**, поэтому они оплачиваются по мере использования. Вы можете настраивать этот параметр (см. ответ на предыдущий вопрос). Это относится только к программному обеспечению Windows Server, но не к многоуровневым продуктам. Например, для SQL придется использовать собственную лицензию. Лицензирование с оплатой по мере использования не применяется к многоуровневым программным продуктам.

### <a name="i-have-an-enterprise-agreement-ea-and-will-be-using-my-ea-windows-server-license-how-do-i-make-sure-images-are-billed-correctly"></a>У меня есть Соглашение Enterprise (EA), и я буду использовать лицензию EA для Windows Server. Как можно убедиться, что образы оплачиваются правильно?

Вы можете добавить **licenseType: Windows_Server** в шаблоне Azure Resource Manager. Этот параметр добавляется в каждый блок ресурса виртуальной машины.

## <a name="activation"></a>Активация

Чтобы активировать виртуальную машину Windows Server в Azure Stack, должны выполняться следующие условия:

- Изготовитель оборудования указал правильное значение маркера BIOS на каждой хост-системе в Azure Stack.
- В Windows Server 2012 R2 и Windows Server 2016 используется [автоматическая активация виртуальной машины](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)). Служба управления ключами (KMS) и другие службы активации не поддерживаются в Azure Stack.

### <a name="how-can-i-verify-that-my-virtual-machine-is-activated"></a>Как убедиться, что моя виртуальная машина активирована?

Выполните следующую команду в командной строке с повышенными привилегиями:

```shell
slmgr /dlv
```

Если активация выполнена правильно, вы увидите явное сообщение об этом и имя узла в выходных данных `slmgr`. Не доверяйте водяным знакам на экране, так как они могут быть устаревшими или относиться к другой виртуальной машине, размещенной сзади нужной.

### <a name="my-vm-is-not-set-up-to-use-avma-how-can-i-fix-it"></a>В виртуальной машине не настроено использование AVMA. Как это исправить?

Выполните следующую команду в командной строке с повышенными привилегиями:

```shell
slmgr /ipk <AVMA key>
```

В статье [об автоматической активации виртуальной машины](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) вы найдете сведения о ключах для использования с образом.

### <a name="i-create-my-own-windows-server-images-how-can-i-make-sure-they-use-avma"></a>Я создаю собственные образы Windows Server. Как мне гарантировать для них использование AVMA?

Рекомендуем выполнить команду `slmgr /ipk` с подходящим ключом перед выполнением команды `sysprep`. Вы также можете поместить ключ AVMA в любой файл установки Unattend.exe.

### <a name="i-am-trying-to-use-my-windows-server-2016-image-created-on-azure-and-it-is-not-activating-or-using-kms-activation"></a>Я пытаюсь использовать созданный в Azure образ Windows Server 2016, но он не активируется и не использует KMS-активацию.

Выполните команду `slmgr /ipk`. В образах Azure не всегда правильно применяется резервный механизм AVMA, но они успешно пройдут активацию, если установят связь с системой Azure KMS. Мы рекомендуем проверить, настроено ли для этих виртуальных машин использование AVMA.

### <a name="i-have-performed-all-of-these-steps-but-my-virtual-machines-are-still-not-activating"></a>После выполнения всех инструкций виртуальные машины по-прежнему не могут активироваться.

Обратитесь к поставщику оборудования и убедитесь, что установлены правильные маркеры BIOS.

### <a name="what-about-earlier-versions-of-windows-server"></a>Как насчет более ранних версий Windows Server?

[Автоматическая активация виртуальной машины](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) не поддерживается для более ранних версий Windows Server. Такие виртуальные машины следует активировать вручную.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в следующих статьях:

- [Общие сведения об Azure Stack Marketplace](azure-stack-marketplace.md)
- [Скачивание элементов Marketplace из Azure в Azure Stack](azure-stack-download-azure-marketplace-item.md)
