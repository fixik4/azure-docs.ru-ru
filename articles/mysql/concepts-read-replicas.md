---
title: Реплики чтения в базе данных Azure для MySQL.
description: В этой статье описывается настройка реплик чтения для базы данных Azure для MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 6e33c7571dc735ce9984a0ce1b37275a6c4c7eca
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2019
ms.locfileid: "56888478"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>Реплики чтения в базе данных Azure для MySQL

Компонент "Реплика чтения" позволяет реплицировать данные из сервера Базы данных Azure для MySQL (главный сервер) на несколько (до пяти) серверов только для чтения (реплики) в одном регионе Azure. Реплики только для чтения асинхронно обновляются с помощью технологии репликации на основе позиции файла собственного двоичного журнала (binlog) ядра MySQL. Дополнительные сведения о репликации binlog MySQL см. в [этой статье](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

Реплики, созданные в базе данных Azure для MySQL, — это новые серверы, управление которыми аналогично управлению обычными или изолированными серверами MySQL. Для каждой реплики чтения вы оплачиваете подготовленные вычислительные ресурсы, выраженные в виртуальных ядрах, и подготовленный объем хранилища, выраженный в ГБ/месяц.

Дополнительные сведения о функциях и проблемах репликации MySQL см. в [документации по репликации MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-read-replicas"></a>Когда следует использовать реплики чтения

Приложения и рабочие нагрузки с большим объемом операций чтения могут обслуживаться репликами только для чтения. Реплики чтения помогают повысить производительность операций чтения по сравнению с использованием изолированного сервера для операций чтения и записи. Рабочие нагрузки чтения можно изолировать в реплики, направив рабочие нагрузки записи в главную базу данных.

К типичным сценариям относится ситуация, когда рабочие нагрузки бизнес-аналитики и анализа используют реплику чтения в качестве источника данных для отчетов.

## <a name="considerations-and-limitations"></a>Ограничения и рекомендации

### <a name="pricing-tiers"></a>Ценовые категории

Реплики чтения сейчас доступны только в ценовых категориях "Общее назначение" и "Оптимизировано для памяти".

### <a name="master-server-restart"></a>Перезапуск главного сервера

При создании реплики для базы данных master, отсутствуют существующие реплики, образце сначала будет перезапущен, чтобы подготовить сам репликации. Следует это учитывать и выполнять такие операции в период низкой нагрузки.

### <a name="stopping-replication"></a>Остановка репликации

Вы можете остановить репликацию между главным сервером и сервером-репликой. Остановка репликации подразумевает удаление отношения репликации между главным сервером и сервером-репликой.

После остановки репликации сервер-реплика становится изолированным сервером. Данные на изолированном сервере — это данные, которые были доступны в реплике на момент запуска команды "Остановить репликацию". Данные на изолированном сервере не синхронизируются с главным сервером. Это сервер нельзя снова преобразовать в реплику.

### <a name="replicas-are-new-servers"></a>Реплики — это новые серверы

Реплики создаются как новые серверы базы данных Azure для MySQL. Существующие серверы нельзя преобразовать в реплики.

### <a name="replica-server-configuration"></a>Конфигурация сервера-реплики

Серверы-реплики создаются с помощью тех же конфигураций сервера, что и главный сервер, включая следующие аспекты:

- Ценовой уровень
- Поколение вычислительных ресурсов
- Виртуальные ядра
- Хранилище
- Срок хранения моментального снимка
- Вариант избыточности для резервного копирования
- Версия ядра MySQL
- Правила файрволла

После создания реплики можно изменить ценовую категорию (за исключением базовой), поколение вычислительных ресурсов, виртуальные ядра, хранилище и срок хранения резервных копий независимо от главного сервера.

### <a name="master-server-configuration"></a>Конфигурация главного сервера

Если конфигурация главного сервера (например, виртуальных ядер или хранилища) изменяется, необходимо также изменить конфигурацию реплики, задав равные или большие значения. В противном случае сервер-реплика не будет соответствовать изменениям, внесенным в конфигурацию главного сервера, что может повлечь за собой сбой сервера.

Новые правила брандмауэра, добавленные на главном сервере после создания сервера-реплики, не копируются в реплику. Новое правило брандмауэра нужно также настроить и в реплике.

### <a name="deleting-the-master-server"></a>Удаление главного сервера

При удалении главного сервера репликация останавливается для всех реплик чтения. Эти реплики становятся изолированными серверами. Удаляется сам главный сервер.

### <a name="user-accounts"></a>Учетные записи пользователей

Пользователи на главном сервере реплицируются в реплики чтения. К реплике чтения можно подключиться только с помощью учетных записей пользователей, доступных на главном сервере.

### <a name="other"></a>Другие

- Идентификаторы глобальных транзакций (GTID) не поддерживаются.
- Создание реплики реплики не поддерживается.
- Таблицы в памяти могут вызывать рассинхронизацию реплик. Это ограничение технологии репликации MySQL. Дополнительные сведения см. в [справочной документации по MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html).
- Настройка параметра [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) на главном сервере после создания сервера-реплики может привести к рассинхронизации реплики. Сервер-реплика не учитывает разные табличные пространства.
- Убедитесь, что у таблиц главного сервера есть первичные ключи. Отсутствие первичных ключей может привести к задержке репликации между главным сервером и репликами.
- Полный список ограничений репликации MySQL см. в [документации по MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [создавать реплики чтения и управлять ими с помощью портала Azure](howto-read-replicas-portal.md).
- Узнайте, как [создавать реплики чтения и управлять ими с помощью Azure CLI](howto-read-replicas-cli.md).
