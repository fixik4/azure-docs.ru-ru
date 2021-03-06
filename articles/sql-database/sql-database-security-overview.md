---
title: Обзор защиты базы данных SQL Azure | Документация Майкрософт
description: Дополнительные сведения о безопасности баз данных SQL Azure и SQL Server, в том числе описание различий между облачной и локальной базой данных SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto, carlrab, emlisa
manager: craigg
ms.date: 03/29/2019
ms.openlocfilehash: 7fc69c3d0b6820db2227b241d63e4304152d99bf
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664681"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Обзор возможностей безопасности базы данных SQL Azure

В этой статье описано, как защитить уровень данных приложения, использующего Базу данных SQL Azure. Эта стратегия безопасности использует многоуровневый подход (эшелонированная защита), который представлен на следующем рисунке (уровни рассматриваются, начиная с наружного):

![sql-security-layer.png](media/sql-database-security-overview/sql-security-layer.png)

## <a name="network-security"></a>Безопасность сети

База данных SQL Microsoft Azure предоставляет службу реляционной базы данных для облачных и корпоративных приложений. Чтобы защитить клиентские данные, брандмауэр запрещает сетевой доступ к серверу базы данных, кроме разрешенного явным образом по IP-адресу или виртуальной сети Azure источника трафика.

### <a name="ip-firewall-rules"></a>Правила брандмауэра для IP-адресов

Правила брандмауэра для IP-адресов предоставляют доступ к базам данным на основе исходного IP-адреса каждого запроса. См. дополнительные сведения о [правилах брандмауэра служб "База данных SQL Azure" и "Хранилище данных SQL"](sql-database-firewall-configure.md).

### <a name="virtual-network-firewall-rules"></a>Правила брандмауэра для виртуальной сети

[Конечные точки службы виртуальной сети](../virtual-network/virtual-network-service-endpoints-overview.md) подключают вашу виртуальную сеть к магистральной сети Azure, позволяя Базе данных SQL Azure идентифицировать подсеть виртуальной сети, из которой к ней направляется трафик. Чтобы трафик поступал в Базу данных SQL Azure, используйте [теги служб](../virtual-network/security-overview.md) SQL, которые разрешают исходящий трафик через механизм групп безопасности сети.

[Правила виртуальных сетей](sql-database-vnet-service-endpoint-rule-overview.md) позволяют Базе данных SQL Azure принимать только те подключения, которые отправлены из определенных подсетей в виртуальной сети.

> [!NOTE]
> Управление доступом с помощью правил брандмауэра *не* применяется к **управляемому экземпляру**. Дополнительные сведения о необходимой конфигурации сети см. в руководстве по [подключению приложения к управляемому экземпляру](sql-database-managed-instance-connect-app.md).

## <a name="access-management"></a>управление доступом

> [!IMPORTANT]
> Управление базами данных и серверами баз данных в Azure осуществляется путем назначения ролей учетной записи пользователя портала. Дополнительные сведения об этом см. в разделе [Управление доступом на основе ролей на портале Azure](../role-based-access-control/overview.md).

### <a name="authentication"></a>Authentication

Аутентификацией называют процесс подтверждения личности пользователя. База данных SQL Azure поддерживает два типа аутентификации:

- **Аутентификация SQL.**

    Аутентификация Базы данных SQL выполняется на основе имени пользователя и пароля при подключении пользователя к [Базе данных SQL Azure](sql-database-technical-overview.md). При создании сервера базы данных для базы данных следует создать учетную запись администратора сервера, указав имя пользователя и пароль. Используя эти учетные данные, администратор сервера сможет выполнить аутентификацию в любой базе данных на этом сервере и получить права ее владельца. После этого администратор сервера сможет создать дополнительные имена входа и пользователей SQL, чтобы позволить пользователям подключаться с выделенными для них именами пользователей и паролями.

- **Аутентификация Azure Active Directory.**

    Аутентификация Azure Active Directory — это механизм подключения к службам [База данных SQL Azure](sql-database-technical-overview.md) и [Хранилище данных SQL](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) с помощью удостоверений в Azure Active Directory (Azure AD). С помощью аутентификации Azure Active Directory администраторы могут централизованно управлять удостоверениями и разрешениями для пользователей базы данных совместно с другими службами корпорации Майкрософт. Это снижает количество хранимых паролей, а также позволяет применять централизованные политики их смены.

     Чтобы применить для Базы данных SQL аутентификацию Azure AD, необходимо создать администратора сервера, который в этом контексте называется **администратором Active Directory**. См. дополнительные сведения об [использовании аутентификации Azure Active Directory для аутентификации с помощью SQL](sql-database-aad-authentication.md). Аутентификация Azure AD поддерживает как управляемые, так и федеративные учетные записи. Федеративные учетные записи применимы для пользователей и групп Windows в пользовательских доменах, для которых настроена федерация с Azure AD.

    Кроме того, для аутентификации Azure Active Directory доступны [универсальная аутентификация Active Directory для SQL Server Management Studio](sql-database-ssms-mfa-authentication.md), которая поддерживает [многофакторную проверку подлинности](../active-directory/authentication/concept-mfa-howitworks.md), и [условный доступ](sql-database-conditional-access.md).

> [!IMPORTANT]
> Управление базами данных и серверами в Azure осуществляется путем назначения ролей учетной записи пользователя портала. Дополнительные сведения об этом см. в разделе [Управление доступом на основе ролей на портале Azure](../role-based-access-control/overview.md). Управление доступом с помощью правил брандмауэра *не* применяется к **управляемому экземпляру**. Дополнительные сведения о необходимой конфигурации сети см. в следующей статье, посвященной [подключению к управляемому экземпляру](sql-database-managed-instance-connect-app.md).

Авторизация — это по сути набор разрешений, которые назначены пользователю в Базе данных SQL Azure и которые определяют, какие действия доступны этому пользователю. Для управления разрешениями учетным записям пользователей назначаются [роли базы данных](/sql/relational-databases/security/authentication-access/database-level-roles), которые определяют разрешения на уровне базы данных или предоставляют требуемые [разрешения на уровне объектов](/sql/relational-databases/security/permissions-database-engine). См. дополнительные сведения об [операциях входа и пользователях](sql-database-manage-logins.md).

Мы рекомендуем всегда назначать пользователям роли с минимально необходимым для выполнения должностных функций набором разрешений. Учетная запись администратора сервера является членом роли db_owner, которая имеет широкие права доступа, а значит должна использоваться осторожно. Если вы используете приложения для работы с Базой данных SQL Azure, используйте [роли приложения](/sql/relational-databases/security/authentication-access/application-roles) с ограниченными разрешениями. Так вы будете уверены, что подключенные к базе данных приложения получат минимально необходимые права доступа.

### <a name="row-level-security"></a>Безопасность на уровне строк

Безопасность на уровне строк позволяет пользователям управлять доступом к строкам в таблице базы данных в зависимости от характеристик пользователя, выполняющего запрос (например, членство в группе или контекст выполнения). Дополнительные сведения см. в статье [Безопасность на уровне строк](/sql/relational-databases/security/row-level-security).

![azure-database-rls.png](media/sql-database-security-overview/azure-database-rls.png)

  Это метод аутентификации с использованием имени пользователя и пароля. 

См. дополнительные сведения о [разрешениях в Базе данных SQL Azure](sql-database-manage-logins.md#permissions).

## <a name="threat-protection"></a>Защита от угроз

База данных SQL обеспечивает защиту клиентских данных, предоставляя возможности аудита и обнаружения угроз.

### <a name="sql-auditing-in-azure-monitor-logs-and-event-hubs"></a>Аудит SQL в Azure Monitor журналы и концентраторов событий

Аудит Базы данных SQL отслеживает действия в базе данных и помогает поддерживать соответствие стандартам безопасности, сохраняя события базы данных в журнал аудита, сохраненный в учетной записи хранения Azure. Благодаря аудиту пользователи могут в реальном времени отслеживать действия в базе данных, а также анализировать и изучать действия за прошедшие периоды, чтобы выявить потенциальные угрозы, возможные нарушения и риски безопасности. См. дополнительные сведения об [аудите Базы данных SQL](sql-database-auditing.md).  

### <a name="threat-detection"></a>Обнаружение угроз

Система обнаружения угроз дополняет возможности аудита, позволяя анализировать журналы аудита для выявления необычного поведения и потенциально вредоносных попыток доступа или использования баз данных. Создаются оповещения для многих подозрительных действий или аномальных событий доступа, таких как атаки путем внедрения кода SQL, потенциальный несанкционированный доступ к данным или атаки методом подбора пароля. Оповещения об обнаружении угроз отображаются в [Центре безопасности Azure](https://azure.microsoft.com/services/security-center/) вместе со сведениями о подозрительных действиях и рекомендациями по расследованию ситуации и устранению угрозы. За использование системы обнаружения угроз взимается плата в размере 15 долларов США за сервер в месяц. За первые 60 дней плата не взимается. Дополнительные сведения см. в статье [Обнаружение угроз Базы данных SQL Azure для отдельной базы данных или базы данных в составе пула](sql-database-threat-detection.md).

![azure-database-td.jpg](media/sql-database-security-overview/azure-database-td.jpg)

## <a name="information-protection-and-encryption"></a>Защита и шифрование информации

### <a name="transport-layer-security-tls-encryption-in-transit"></a>Протокол TLS (шифрование при передаче)

База данных SQL защищает данные клиентов, шифруя передаваемые данные по протоколу [TLS](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server).

SQL Server обеспечивает шифрование (SSL/TLS) во все значения времени для всех подключений. Это гарантирует, что все данные шифруются «в пути» между клиентом и сервером, независимо от параметра **Encrypt** или **TrustServerCertificate** в строке подключения.

Рекомендуется, рекомендуется в подключение вашего приложения строка вы укажите зашифрованное соединение и _**не**_ доверять сертификату сервера. Это заставляет приложение для проверки сертификата сервера и таким образом запрещает приложения не подвержена man в среднем типа атак.

Например, при использовании драйвера ADO.NET это достигается с помощью **Encrypt = True** и **TrustServerCertificate = False**. При получении строки подключения с портала Azure, он будет иметь правильные параметры.

> [!IMPORTANT]
> Обратите внимание, что некоторые драйверы сторонних разработчиков может использовать TLS по умолчанию или не полагаться на более старую версию TLS (< 2.0) для работы. В этом случае SQL Server по-прежнему позволяет подключиться к базе данных. Тем не менее мы рекомендуем оценить риски безопасности разрешения такие драйверы и приложения для подключения к базе данных SQL, особенно в том случае, если вы храните конфиденциальные данные. 
>
> Дополнительные сведения о TLS и подключения, см. в разделе [вопросы TLS](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

### <a name="transparent-data-encryption-encryption-at-rest"></a>Прозрачное шифрование данных (шифрование при хранении)

[Прозрачное шифрование данных (TDE) для Базы данных SQL Azure](transparent-data-encryption-azure-sql.md) добавляет дополнительный уровень безопасности, помогая защищать неактивные данные от несанкционированного или автономного доступа к необработанным файлам или резервным копиям данных. Это актуально для таких сценариев, как кража оборудования из центра обработки данных или небезопасная утилизация оборудования или носителей, например дисковых накопителей и лент с резервными копиями. TDE шифрует всю базу данных с помощью алгоритма шифрования AES, а значит разработчикам приложений не придется вносить изменения в существующие приложения.

Все создаваемые в Azure базы данных SQL по умолчанию шифруются, а ключ шифрования базы данных защищается встроенным сертификатом сервера.  Обслуживание и смена сертификатов автоматически выполняются службой, не требуя действий пользователя. Если клиент предпочитает самостоятельно контролировать ключи шифрования, он может управлять ими через [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md).

### <a name="key-management-with-azure-key-vault"></a>Управление ключами с помощью Azure Key Vault

Поддержка технологии [BYOK](transparent-data-encryption-byok-azure-sql.md) (создание собственных ключей) для  [прозрачного шифрования данных](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE) позволяет пользователям самостоятельно управлять созданием и сменой ключей с помощью облачной системы Azure для управления внешними ключами  [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md). В случае отзыва доступа базы данных к хранилищу ключей расшифровка и считывание такой базы данных станет невозможным. Azure Key Vault предоставляет централизованную платформу для управления ключами, которая использует тщательно отслеживаемые аппаратные модули безопасности (HSM) и позволяет разделять обязанности по управлению ключами и данными, обеспечивая соответствие требованиям безопасности.

### <a name="always-encrypted-encryption-in-use"></a>Always Encrypted (шифрование при использовании)

![azure-database-ae.png](media/sql-database-security-overview/azure-database-ae.png)

Функция [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) разработана для защиты от доступа к конфиденциальным данным, которые хранятся в определенных столбцах базы данных (например, номера кредитных карт или персональные коды, а также данные, доступ к которым предоставляется _строго ограниченному кругу лиц_). Например, администраторы баз данных или другие привилегированные пользователи имеют доступ к базе данных для выполнения задач управления, но с точки зрения бизнеса им не нужен доступ к конкретным данным в зашифрованных столбцах. Данные всегда зашифрованы и расшифровываются только для обработки в клиентских приложениях, которые имеют доступ к ключу шифрования.  Ключ шифрования никогда не предоставляется в формате SQL. Его можно хранить в [Windows Certificate Store](sql-database-always-encrypted.md) или в [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md).

### <a name="masking"></a>Маскирование

![azure-database-ddm.png](media/sql-database-security-overview/azure-database-ddm.png)

#### <a name="dynamic-data-masking"></a>Динамическое маскирование данных

Динамическое маскирование данных базы данных SQL Azure ограничивает возможность раскрытия конфиденциальных данных, маскируя их для обычных пользователей. Служба динамического маскирования данных автоматически обнаруживает потенциально конфиденциальные данные в базе данных SQL Azure и предоставляет практические рекомендации по маскированию этих полей с минимальным влиянием на прикладной уровень. Эта функция работает по принципу обсфукации, скрывая конфиденциальные данные в результирующем наборе запроса по заданным полям базы данных, не меняя сами данные в базе данных. Дополнительную информацию см. в статье [Начало работы с динамическим маскированием данных в базе данных SQL (портал Azure)](sql-database-dynamic-data-masking-get-started.md).

#### <a name="static-data-masking"></a>Статическое маскирование данных

[Статическое маскирование данных](/sql/relational-databases/security/static-data-masking) предоставляется как клиентское средство в [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) 18.0 предварительной версии 5 и выше.  Статические маскирование данных позволяет пользователям создать копию базы данных, в которой к данным в выбранных столбцах применена маска без возможности восстановления исходных значений. Здесь доступны маска NULL, маска по одному значению, смешивание и групповое смешивание, а также составная маска строк. Используя маскированные копии базы данных, организации могут разделять рабочие и тестовые среды. Это достаточно надежный способ защитить конфиденциальные данные, сохраняя все остальные характеристики базы данных. Маскирование базы данных мы рекомендуем использовать в тех случаях, когда необходимо предоставить третьим сторонам доступ к базам данных.

## <a name="security-management"></a>Управление безопасностью

### <a name="vulnerability-assessment"></a>Оценка уязвимостей

[Оценка уязвимостей](sql-vulnerability-assessment.md) — это легко настраиваемая служба, которая помогает обнаруживать, отслеживать и устранять потенциальные уязвимости, что позволяет предварительно повышать общую защиту базы данных. Оценка уязвимостей входит в состав предложения расширенной защиты данных (ADS), которое представляет собой унифицированный пакет расширенных средств обеспечения безопасности SQL. Доступ к службе оценки уязвимостей и управление ею можно выполнять через центральный портал SQL ADS.

### <a name="data-discovery--classification"></a>Обнаружение и классификация данных

Служба обнаружения и классификации данных (в настоящее время находится на этапе предварительной версии) предоставляет расширенные возможности, встроенные в службу "База данных SQL Azure", для обнаружения, классификации, добавления меток и защиты конфиденциальных данных в базах данных. Обнаружение и классификация конфиденциальных данных (деловых или финансовых, медицинских, персональных данных и т. д.) может играть ключевую роль в концепции защиты информации вашей организации. Она может использоваться в качестве инфраструктуры для следующего:

- Различные сценарии безопасности, такие как мониторинг (аудит) и оповещение об аномальном доступе к конфиденциальным данным.
- Управление доступом к базам данных, содержащим конфиденциальные данные, и усиление их защиты.
- Соблюдение стандартов конфиденциальности данных и нормативных требований.

Дополнительные сведения см. в статье [Обнаружение и классификация данных в службе "База данных SQL Azure"](sql-database-data-discovery-and-classification.md).

### <a name="compliance"></a>Соответствие нормативным требованиям

В дополнение к приведенным выше функциям и возможностям, которые помогают выполнять требования к безопасности приложений, в отношении Базы данных SQL Azure регулярно проводится аудит, а сама база данных сертифицирована в соответствии с рядом стандартов. Дополнительные сведения см. в [центре управления безопасностью Microsoft Azure](https://azure.microsoft.com/support/trust-center/), где представлен актуальный список [сертификатов соответствия базы данных SQL](https://www.microsoft.com/trustcenter/compliance/complianceofferings).

## <a name="next-steps"></a>Дальнейшие действия

- Как пользоваться функциями контроля доступа в базе данных SQL см. в [этой статье](sql-database-control-access.md).
- Обсуждение аудита баз данных см. в разделе [Аудит базы данных SQL](sql-database-auditing.md).
- Обсуждение обнаружение угроз см. в разделе [Обнаружение угроз для базы данных SQL](sql-database-threat-detection.md).
