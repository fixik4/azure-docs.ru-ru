---
title: Сведения об Azure DevTest Labs | Документы Майкрософт
description: Узнайте, как DevTest Labs может упростить создание и отслеживание виртуальных машин Azure, а также управление ими.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 599b4325e0eb9aa7f6ccca2626c6d4b14f38149d
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339335"
---
# <a name="about-azure-devtest-labs"></a>Сведения об Azure DevTest Labs
Azure DevTest Labs — это служба, которая позволяет разработчикам команды эффективно самообслуживания виртуальных машин и ресурсов PaaS, которые им необходимы для разработки, тестирования, обучения и демонстрации и т.д., не дожидаясь константы утверждения о средствах, которые им необходимы. 

Лаборатория уже состоит из предварительно настроенных базовых классов или шаблонов Resource Manager с помощью все необходимые инструменты и программное обеспечение, которое разработчики могут использовать для создания сред. Разработчики могут создавать свои среды через несколько минут, в отличие от нескольких часов или дней. 

С помощью DevTest Labs, можно проверить последнюю версию приложения, выполнив следующие задачи:

- Быстро Подготовив среды Windows и Linux, с помощью многократно используемых шаблонов и артефактов
- Легко интегрируйте конвейер развертывания с DevTest Labs для подготовки сред по запросу
- Увеличьте нагрузочное тестирование путем предоставления нескольких агентов тестирования и создайте предварительно подготовленные среды для обучения и демонстраций.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

## <a name="capabilities"></a>Возможности
DevTest Labs предоставляют следующие возможности для разработчиков, работе с виртуальными машинами.

- Быстро создайте виртуальную машину, выполнив меньше, чем пять простых шагов.
- Выберите проверенный список баз виртуальных Машин, которые настраиваются, утверждения и авторизуется руководитель команды или центрального ИТ.
- Создание машин на основе предварительно созданных пользовательских образов, которые имеют все программное обеспечение и средства, установленные в образе. 
- Создайте машины на основе формулы, которые по сути пользовательских образов и последнюю сборку программного обеспечения, установленного на момент создания виртуальной машины.
- Установка артефактов, которые являются расширениями, которые развернуты на виртуальной Машине после ее подготовки.
- Настроить автоматическое завершение работы и auto начала расписания на компьютерах для завершения работы в конце дня и запущен и работает утра следующего дня.
- Просто запросить предварительно созданной виртуальной машины без необходимости проходить через процесс создания машины. 

DevTest Labs предоставляет следующие возможности для разработчиков, работающих с сред PaaS:

- Создание сред PaaS Azure Resource Manager, которые быстро выполнив менее чем три простых шага.
- Выберите проверенный список шаблонов Resource Manager, которые настраиваются, утверждения и авторизуется руководитель команды или центрального ИТ.
- Запускайте пустую группу ресурсов (песочница) с помощью шаблона Resource Manager для просмотра всей платформы Azure, продолжая в контексте лабораторной среде.
- DevTest Labs предоставляет следующие преимущества при создании и настройке сред разработки и тестирования в облаке, а также управлении этими средами.

Помимо модель самообслуживания для разработчиков в команде, эта служба дает возможность центрального ИТ-специалистам управлять пустая трата, оптимизировать затраты на ресурсы и оставаться в пределах бюджета, выполнив следующие задачи: 

- Параметр Автоматическое завершение работы и автоматического запуска расписания на виртуальных машинах.
- Настройка политик на количество пользователей, виртуальные машины можно создать.
- Настройка политик о размерах виртуальных машин и коллекции образы пользователей можно выбрать из.
- Отслеживания затрат и установив параметр целевых объектов в лаборатории.
- Получение уведомлений о высокой прогнозируемых затрат лаборатории, таким образом, можно выполнить необходимые действия. 

DevTest Labs предоставляет следующие преимущества в создание, Настройка и управление средами в облаке:

## <a name="cost-control-and-governance"></a>Управление затратами и контроль над
DevTest Labs облегчает контроль расходов, позволяя выполнять следующие задачи:

- Задавать политики для лаборатории — например, количество виртуальных машин (ВМ), каждому пользователю и число виртуальных машин на лабораторию. 
- Создание политик для автоматического запуска виртуальных машин и завершить работу.
- Позволяет отслеживать затраты на виртуальные машины и ресурсов PaaS, развернутые в лаборатории таким образом, может оставаться в пределах вашего бюджета предопределенные. 
- Разработчики могут оставаться в пределах контекста лаборатории, таким образом, чтобы они не попадут задействовав любых ресурсов за пределами его в базовой группе ресурсов или подписки.

## <a name="quickly-get-to-ready-to-test"></a>Быстрый переход в состояние "Готово к тестированию"
DevTest Labs позволяет создавать готовые среды, включающие все, что нужно участникам вашей группы, чтобы начать разработку и тестирование приложений. Просто запросить сред, где установлена последняя Проверенная версия вашего приложения и начать работу прямо сейчас. Либо воспользуйтесь контейнерами и создайте среды еще быстрее и экономнее.

## <a name="create-once-use-everywhere"></a>Создайте один раз и используйте везде
Записать и сделайте PaaS шаблоны и артефакты среды внутри вашей команды или организации, — все в системе управления версиями - Создание разработчика среды и тестирования.

## <a name="worry-free-self-service"></a>Самообслуживание без проблем
DevTest Labs позволяет разработчикам и тест-инженерам быстро и легко создает виртуальные машины (IaaS) и ресурсов PaaS с помощью нескольких щелчков, используя набор ресурсов, предварительно настроенный для них.

## <a name="use-iaas-and-paas-resources"></a>Использовать ресурсы IaaS и PaaS 
DevTest Labs также позволяет разработчикам развертывать ресурсы PaaS, например веб-приложения, кластеров Service Fabric, ферм SharePoint и т. д. в лаборатории с помощью шаблона Resource Manager. Использование шаблонов Resource Manager из нашего репозитория на общедоступной среде или подключение лаборатории к в собственном репозитории Git, чтобы приступить к работе на PaaS в лабораториях. Вы также можете отслеживать затраты на эти ресурсы, чтобы уложиться в стандартных бюджетов. 

## <a name="integrate-with-your-existing-toolchain"></a>Интеграция с вашей цепочкой инструментов
Используйте готовые подключаемые модули или наш API для подготовки сред разработки и тестирования непосредственно из вашего вами средства непрерывной интеграции (CI), интегрированной среды разработки (IDE) или автоматизированного конвейера выпусков. Также можно использовать наше полнофункциональное средство командной строки.

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими статьями: 

- Дополнительные сведения о DevTest Labs, см. в разделе [основные понятия DevTest Labs](devtest-lab-concepts.md)
- Пошаговое руководство с пошаговыми инструкциями см. в разделе [руководства: Настройка лаборатории с помощью Azure DevTest Labs](tutorial-create-custom-lab.md)


