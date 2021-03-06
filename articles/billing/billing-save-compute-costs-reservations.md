---
title: Общие сведения об Azure Reserved VM Instances | Документация Майкрософт
description: Сведения о резервировании Azure, которые позволяют сэкономить на ваших виртуальных машинах, базах данных SQL, других расходах на ресурсы, Azure Cosmos DB и соответствующих ценах.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: banders
ms.openlocfilehash: 1349a05e1dd235c7b375335ae2c9fed16170a61f
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649397"
---
# <a name="what-are-azure-reservations"></a>Общие сведения об Azure Reserved VM Instances

Резервирование Azure помогает сэкономить деньги путем внесения предоплаты за использование виртуальной машины, вычислительной мощности Базы данных SQL, пропускной способности Azure Cosmos DB или других ресурсов Azure за один или три года. Предоплата позволяет получить скидку на использование ресурсов. Резервирование может сократить ваши расходы на виртуальную машину, вычислительную мощность Базы данных SQL, Azure Cosmos DB или другие ресурсы до 72 % с оплатой по мере использования. Резервирования предоставляют скидку при выставлении счетов и не влияют на состояние выполнения ваших ресурсов.

Резервирование можно приобрести на [портале Azure](https://aka.ms/reservations). Дополнительные сведения см. в следующих статьях:

Планы обслуживания:
- [Виртуальные машины Azure зарезервированных экземпляров виртуальных Машин](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Ресурсы Azure Cosmos DB с помощью Azure Cosmos DB зарезервированная емкость](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Вычислительные ресурсы базы данных SQL с базой данных SQL Azure зарезервированная емкость](../sql-database/sql-database-reserved-capacity.md)

Планы программного обеспечения:
- [Планы программного обеспечения Red Hat из Azure резервирования](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Планы программного обеспечения SUSE из Azure резервирования](../virtual-machines/linux/prepay-suse-software-charges.md)

## <a name="why-buy-a-reservation"></a>Зачем покупать резервирования?

Если у вас есть виртуальные машины, Azure Cosmos DB или баз данных SQL, которые работают длительное время, покупки резервирования дает наиболее экономичное решение. Например если вы постоянно работают четыре экземпляра службы без резервирования, плата взимается по ставкам повременной оплаты. Если вы покупаете резервирование для этих ресурсов, вы немедленно получаете скидка на резервирование. К ресурсам больше не будут применяться ставки оплаты по мере использования.

## <a name="charges-covered-by-reservation"></a>Расходы, связанная с резервированием

Планы обслуживания:

- Зарезервированный экземпляр виртуальной машины. Резервирование покрывает только затраты на вычислительные ресурсы виртуальной машины. Оно не включает оплату дополнительного программного обеспечения, сетей и хранилищ.
- Резервная мощность Azure Cosmos DB. Резервирование покрывает пропускную способность для ваших ресурсов. Но она не распространяется на расходы хранилища и сети.
- Зарезервированные виртуальные ядра Базы данных SQL. Покрываются расходы только на вычислительные ресурсы, включенные в резервирование. Лицензия оплачивается отдельно.
- Резервная мощность Azure Cosmos DB. Резервирование покрывает расходы на пропускную способность, подготовленную для ресурсов, но не распространяется на расходы хранилища и сети.

Если вы используете виртуальные машины Windows и Базу данных SQL, расходы на лицензирование вам поможет сократить [Преимущество гибридного использования Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Кто может приобрести резервирование?

Чтобы купить план, необходимо иметь роль владельца подписки в подписку с оплатой по мере использования (MS-AZR - 003P или MS-AZR - 0023P) или Enterprise (MS-AZR - 0017P или MS-AZR - 0148P). Поставщиков облачных решений можно использовать портал Azure или [центра партнеров](/partner-center/azure-reservations) для приобретения Azure резервирования.

Клиенты с соглашением Enterprise можно ограничить покупки администраторам EA, отключив **Добавление зарезервированных экземпляров** параметр на портале EA. EA "Администраторы" должен быть владельцем подписки для по крайней мере одна подписка EA для покупки резервирования. Параметр полезен для предприятий, желающих централизованной группе приобрести резервирование для влияния различных центров. После приобретения централизованное команд можно добавить владельцев center стоимость резервирования. Владельцы затем можно ограничить область резервирования в свои подписки. Центральная группа не имеют доступа владельца подписки, на котором приобретено резервирования.

Скидка на резервирование применяется только для ресурсов в подписках с Соглашением Enterprise, с оплатой по мере использования или CSP.

## <a name="how-is-a-reservation-billed"></a>Как оплачивается резервирование?

Для покупки резервирования используется метод оплаты, привязанный к подписке. Если вы используете подписку Enterprise Agreement, стоимость резервирования вычитается из баланса денежных обязательств. Если у вас недостаточно баланса денежных обязательств для покрытия полной стоимости резервирования, вам выставляется счет на соответствующую разницу. Если вы используете подписку с оплатой по мере использования, нужная сумма немедленно снимается с кредитной карты, которая подключена к учетной записи. Если вы используете оплату по накладным, эта сумма будет включена в ближайшую накладную.

## <a name="how-reservation-discount-is-applied"></a>Применение скидки резервирования

Скидка резервирования применяется к использованию ресурсов, которые соответствуют атрибутам, выбранным при покупке резервирования. Атрибуты включают в себя область, в которой выполняются виртуальные машины, базы данных SQL, Azure Cosmos DB или другие ресурсы. Например, если вы хотите использовать резервирование для получения скидки на четыре виртуальные машины D2 (цен. категория "Стандартный") в регионе "западная часть США", выберите ту подписку, в которой работают эти виртуальные машины. Если виртуальные машины работают в разных подписках в одной учетной записи, выберите вариант совместного использования. Совместное использование позволяет применить скидки резервирования сразу в нескольких подписках. Область резервирования можно изменить и после покупки. Дополнительные сведения см. в статье [Управление резервированиями в Azure](billing-manage-reserved-vm-instance.md).

Скидка на резервирование применяется только для ресурсов в подписках с Соглашением Enterprise, с оплатой по мере использования или CSP. Ресурсы, работающие в подписке с любым другим типом, не могут воспользоваться скидкой резервирования.

Чтобы лучше понять влияние резервирования выставлением счетов, см. в разделе со следующими статьями:

Планы обслуживания:

- [Сведения о скидках на зарезервированные экземпляры виртуальной машины Azure](billing-understand-vm-reservation-charges.md)
- [Сведения о скидках на резервирование Azure](billing-understand-vm-reservation-charges.md)
- [Общие сведения о применении скидки на резервирование в Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md)

Планы программного обеспечения:

- [Общие сведения о скидка на резервирование Azure и использовании для Red Hat](billing-understand-rhel-reservation-charges.md)
- [Сведения о применении скидки на программное обеспечение SUSE Linux Enterprise](billing-understand-suse-reservation-charges.md)

## <a name="when-the-reservation-term-expires"></a>Когда срок резервирования

При завершении срока резервирования скидка на оплату отменяется, и для виртуальной машины, базы данных SQL, Azure Cosmos DB или другого ресурса снова применяется оплата по мере использования. Резервирования Azure не возобновляются автоматически. Чтобы сохранить скидку на оплату, необходимо приобрести новое резервирование для соответствующих служб и программного обеспечения.

## <a name="discount-applies-to-different-sizes"></a>Скидка применяется к разных размеров

Когда вы покупаете резервирование, скидку можно применить к другим экземплярам с атрибутами, которые находятся в группе того же размера. Эта функция называется гибкость размер экземпляра. Гибкость покрытия скидки зависит от типа резервирования и атрибутов, которые вы выбираете при покупке резервирования.

Планы обслуживания:

- Зарезервированные экземпляры виртуальных машин. При покупке резервирования и выберите **оптимизировано для**: **экземпляра гибкость размер**, покрытие скидка зависит от размера виртуальной Машины, выбран. Резервирование можно применить к размерам в группе виртуальных машин серии того же размера. Дополнительные сведения см. в статье [Гибкость размеров виртуальных машин при использовании зарезервированных экземпляров виртуальных машин](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Резервная мощность Базы данных SQL. Покрытие скидки зависит от выбранного уровня производительности. Дополнительные сведения см. в статье [Сведения о применении скидки на резервирование к Базам данных SQL](billing-understand-reservation-charges.md).
- Резервная мощность Azure Cosmos DB. Покрытие скидки зависит от подготовленной пропускной способности. Дополнительные сведения см. в статье [Общие сведения о применении скидки на резервирование в Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или нужна помощь, [создать запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

- Снижайте расходы на виртуальные машины, приобретая [зарезервированный экземпляр виртуальной машины](../virtual-machines/windows/prepay-reserved-vm-instances.md), [резервную мощность Базы данных SQL](../sql-database/sql-database-reserved-capacity.md) или [резервную мощность Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md).
- Дополнительные сведения о резервировании в Azure см. по следующим ссылкам:
    - [Управление Azure Reserved VM Instances](billing-manage-reserved-vm-instance.md)
    - [Общие сведения об использовании резервирования Azure для подписки с оплатой по мере использования](billing-understand-reserved-instance-usage.md)
    - [Общие сведения об использовании зарезервированных экземпляров Azure с Соглашением о регистрации Enterprise](billing-understand-reserved-instance-usage-ea.md)
    - [Затраты на программное обеспечение Windows, которые не включены в стоимость зарезервированных экземпляров Azure](billing-reserved-instance-windows-software-costs.md)
    - [Приобретение зарезервированных экземпляров Azure](https://docs.microsoft.com/partner-center/azure-reservations)
