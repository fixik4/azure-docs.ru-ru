---
title: Автоматическая подготовка пользователей для приложения SaaS в Azure AD | Документы Майкрософт
description: Общие сведения об использовании Azure AD для автоматической подготовки, отзыва и постоянного обновления учетных записей пользователей в нескольких приложениях SaaS сторонних разработчиков.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2019
ms.author: celested
ms.reviewer: asmalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40e8aaa60359fcfb85c79c4210f7c5cc14633c7b
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894412"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory

## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Что такое автоматическая подготовка пользователей для приложений SaaS
Azure Active Directory (Azure AD) позволяет автоматизировать создание, обслуживание и удаления удостоверений пользователей в облаке ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) приложений, таких как Dropbox, Salesforce, ServiceNow и многое другое.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**Эта функция позволяет:**

- Автоматическое создание новых учетных записей в соответствующих системах для новых членов вашей команды или организации.
- Автоматическая деактивация учетных записей в соответствующих системах, когда люди покидают команду или организацию.
- Постоянное обновление удостоверений в ваших приложениях и системах при внесении изменений в каталог или в систему управления персоналом.
- Подготовка не созданных пользователями объектов, таких как группы, в приложениях, которые их поддерживают.

**Автоматическая подготовка пользователей включает эту функцию:**

- Сопоставление существующих удостоверений в исходных и целевых системах.
- Настраиваемые сопоставления атрибутов, которые определяют, какие данные пользователя должны поступать из исходной системы в целевую систему.
- Необязательная отправка сообщения с оповещением об ошибке при подготовке.
- Составление отчетов и журналов действий для упрощения отслеживания и устранения неполадок.

## <a name="why-use-automated-provisioning"></a>Для чего нужна автоматическая подготовка

Вот несколько основных причин, по которым стоит использовать эту функцию:

- Исключение затрат, неэффективности и ошибок, связанных с человеческим фактором, которые могут возникать в ходе выполнения подготовки вручную.
- Отсутствие затрат, связанных с размещением и поддержкой специально разработанных решений и скриптов для подготовки.
- Защита вашей организации, мгновенно удаляя из ключевых приложений SaaS удостоверения пользователей, когда они покидают организацию.
- Легко Импорт большое количество пользователей в конкретное приложение SaaS или систем.
- Применения единого набора политик, чтобы определить, кто подготавливается и кто может входить приложение.

## <a name="how-does-automatic-provisioning-work"></a>Как работает автоматическая подготовка
    
**Служба подготовки Azure AD** подготавливает пользователей для приложений SaaS и других систем путем подключения к конечным точкам API управления пользователя, предоставляемые каждым поставщиком приложения. Эти конечные точки API управления пользователями позволяют Azure AD создавать, обновлять и удалять пользователей путем программирования. Для выбранных приложений служба подготовки можно также создать, обновить и удалить дополнительные объекты, связанные с удостоверениями, таких как группы и роли. 

![Подготовка](./media/user-provisioning/provisioning0.PNG)
*рис. 1: Служба подготовки Azure AD*

![Исходящая Подготовка](./media/user-provisioning/provisioning1.PNG)
*рис. 2: "Исходящий" рабочий процесс подготовки пользователей из Azure AD в популярные приложения SaaS*

![Входящая Подготовка](./media/user-provisioning/provisioning2.PNG)
*рис. 3: "Входящий" рабочий процесс подготовки пользователей из популярных приложений управления кадрами (HCM) в Azure Active Directory и Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Какие приложения и системы можно использовать с автоматической подготовкой пользователей Azure AD

Azure AD предлагает предварительно интегрированную поддержку многих популярных приложений и отдела кадров системой SaaS и универсальную поддержку приложений, которые реализуют определенные части стандарта SCIM 2.0.

### <a name="pre-integrated-applications"></a>Предварительно интегрированные приложения

Перечень всех приложений, для которых Azure AD поддерживает предварительно интегрированный соединитель подготовки, приведен в [списке руководств по приложениям для подготовки пользователей](../saas-apps/tutorial-list.md).

Для того чтобы связаться с командой инженеров Azure AD для обеспечения подготовки в других приложениях, отправьте сообщение через [форум отзывов Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> Чтобы приложение поддерживало автоматическую подготовку пользователей, сначала оно должно предоставить необходимые API управления пользователями, которые позволят внешним программам автоматизировать создание, обслуживание и удаление пользователей. Таким образом не все приложения SaaS совместимы с этой функцией. Для приложений, которые поддерживают API управления пользователями команда инженеров Azure AD можно затем собрать соединитель подготовки для этих приложений, и эта работа является приоритетным с требованиями текущих и потенциальных заказчиков. 

### <a name="connecting-applications-that-support-scim-20"></a>Подключение приложений с поддержкой SCIM 2.0

Общие инструкции по подключению приложений, которые реализуют API управления пользователями на основе SCIM 2.0, см. в статье [Использование SCIM для автоматической подготовки пользователей и групп Azure Active Directory для приложений](use-scim-to-provision-users-and-groups.md).

    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Как настроить автоматический вход в приложение

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Настройка подготовки службы для выбранного приложения Azure AD с помощью портала Azure Active Directory.

1. Откройте  **[портал Azure Active Directory](https://aad.portal.azure.com)**.

1. Выберите **корпоративные приложения** в левой области. Список всех настроенных приложений панель отображается.

1. Выберите **+ новое приложение** Чтобы добавить приложение. Добавьте один из следующих в зависимости от сценария:

   - **Добавление собственного приложения** параметр поддерживает специально разработанных интеграций SCIM.

   - Все приложения в **добавить из коллекции** > **популярные приложения** разделе поддерживают автоматическую подготовку. Дополнительные приложения см. в [списке руководств по приложениям для подготовки пользователей](../saas-apps/tutorial-list.md).

1. Сведения и выберите **добавить**. Новое приложение добавляется в список корпоративных приложений и открывает его экран управления приложения.

1. Выберите **Подготовка** для управления учетной записью пользователя, подготовки параметров для приложения.

   ![Параметры](./media/user-provisioning/provisioning_settings0.PNG)

1. Выберите параметр "автоматически" для **режим подготовки к работе** для задания параметров для учетных данных администратора, сопоставления, запуска и остановки и синхронизации.

   - Разверните **учетные данные администратора** вводить учетные данные, необходимые для Azure AD для подключения к Интерфейсу управления пользователями приложения. Этот раздел также позволяет включить уведомления по электронной почте, если ошибки учетных данных или задание подготовки переходит в [карантина](#quarantine).

   - Разверните **сопоставления** просматривать и изменять пользовательские атрибуты, которые проходят между Azure AD и целевом приложении при подготовке или обновлении учетных записей пользователей. Если целевое приложение поддерживает его, этот раздел позволяет дополнительно настроить подготовку групп и учетных записей пользователей. Выберите сопоставление в таблице, чтобы открыть редактор сопоставлений справа, где можно просмотреть и настроить атрибуты пользователя.
   
     **Фильтры области** сообщают службе подготовки, какие пользователи и группы в исходной системе должны быть предоставляются или удаляются в целевую систему. В **сопоставление атрибутов** области выберите **область исходного объекта** для фильтрации по значению конкретного атрибута. Например, вы можете указать, что в области подготовки должны быть только пользователи с атрибутом отдела "Продажи". Дополнительные сведения см. в разделе [Подготовка приложений на основе атрибутов с использованием фильтров области](define-conditional-rules-for-provisioning-user-accounts.md).
    
     Дополнительные сведения см. в статье [Настройка сопоставлений атрибутов для подготовки пользователей для приложений SaaS в Azure Active Directory](customize-application-attributes.md).

   - **Параметры** управляют работой службы подготовки для приложения, включая ли он выполняется в данный момент. **Область** меню позволяет указать, должен ли только назначенных пользователей и групп должны находиться в области для подготовки или всем пользователям в каталоге Azure AD должен быть подготовлен. Дополнительные сведения см. в статье [Назначение пользователя или группы корпоративному приложению в Azure Active Directory](assign-user-or-group-access-portal.md).

На экране управления приложения, выберите **журналы аудита** для просмотра записи каждой операции запуска с помощью Azure AD службой подготовки. Дополнительные сведения см. в статье [Руководство по отчетам об автоматической подготовке учетных записей пользователей](check-status-user-account-provisioning.md).

![Параметры](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> Для настройки службы подготовки пользователей Azure AD и управления ею можно также использовать [API Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>Что происходит при подготовке

Когда исходной системой является Azure AD, служба подготовки использует [функцию разностных запросов API Graph Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) для отслеживания пользователей и групп. Служба подготовки выполняет начальную синхронизацию исходной и целевой систем, после чего осуществляет периодическую добавочную синхронизацию. 

### <a name="initial-sync"></a>начальная синхронизация

При запуске служба подготовки будет когда-либо выполнить первую синхронизацию:

1. Запрашивает всех пользователей и все группы из исходной системы, получая все атрибуты, определенные в [сопоставлениях атрибутов](customize-application-attributes.md).
2. Фильтрует полученных пользователей и группы с помощью настроенных [назначений](assign-user-or-group-access-portal.md) или [фильтров области на основе атрибутов](define-conditional-rules-for-provisioning-user-accounts.md).
3. При назначении пользователю или в области для подготовки служба запрашивает у целевой системы соответствующего пользователя, используя указанный [соответствующие атрибуты](customize-application-attributes.md#understanding-attribute-mapping-properties). Пример: Если имя userPrincipal в исходной системе является совпадающим атрибутом и соответствует атрибуту userName в целевой системе, то служба подготовки запрашивает у целевой системы атрибуты userName, которые соответствуют значениям имени userPrincipal в исходной системе.
4. Если соответствующий пользователь не найден в целевой системе, он создается с помощью атрибутов, возвращенных из исходной системы. После создания учетной записи пользователя, подготовки служба обнаруживает и кэширует идентификатор целевой системе для нового пользователя, который используется для выполнения всех последующих операций на этого пользователя.
5. Если соответствующий пользователь найден, он обновляется с помощью атрибутов, предоставляемых исходной системы. После сопоставляется учетной записи пользователя, подготовки служба обнаруживает и кэширует идентификатор целевой системе для нового пользователя, который используется для выполнения всех последующих операций на этого пользователя.
6. Если сопоставления атрибутов содержат «ссылочные» атрибуты, служба выполняет дополнительные обновления в целевой системе для создания и связывания упоминаемые объекты. Например, пользователь может использовать атрибут Manager в целевой системе, который связан с другим пользователем, созданным в целевой системе.
7. Сохраняет водяной знак в конце начальной синхронизации, который обеспечивает начальную точку для более поздней версии добавочных синхронизаций.

Некоторые приложения, такие как ServiceNow, Google Apps и Box, подготавливают не только пользователей, но и группы и их участников. В таких случаях, если Подготовка групп включена в [сопоставления](customize-application-attributes.md), служба подготовки синхронизирует пользователей и групп и затем синхронизирует членство в группах. 

### <a name="incremental-syncs"></a>Добавочная синхронизация

После первоначальной синхронизации все другие операции синхронизации выполняются следующие действия:

1. Запрашивает у исходной системы всех пользователей и все группы, которые были обновлены с момента сохранения последнего водяного знака.
2. Фильтрует полученных пользователей и группы с помощью настроенных [назначений](assign-user-or-group-access-portal.md) или [фильтров области на основе атрибутов](define-conditional-rules-for-provisioning-user-accounts.md).
3. При назначении пользователю или в области для подготовки служба запрашивает у целевой системы соответствующего пользователя, используя указанный [соответствующие атрибуты](customize-application-attributes.md#understanding-attribute-mapping-properties).
4. Если соответствующий пользователь не найден в целевой системе, он создается с помощью атрибутов, возвращенных из исходной системы. После создания учетной записи пользователя, подготовки служба обнаруживает и кэширует идентификатор целевой системе для нового пользователя, который используется для выполнения всех последующих операций на этого пользователя.
5. Если соответствующий пользователь найден, он обновляется с помощью атрибутов, предоставляемых исходной системы. Если это только что назначенного учетной записи, соответствующей подготовки служба обнаруживает и кэширует идентификатор целевой системе для нового пользователя, который используется для выполнения всех последующих операций на этого пользователя.
6. Если сопоставления атрибутов содержат «ссылочные» атрибуты, служба выполняет дополнительные обновления в целевой системе для создания и связывания упоминаемые объекты. Например, пользователь может использовать атрибут Manager в целевой системе, который связан с другим пользователем, созданным в целевой системе.
7. Если пользователь, который ранее был в области для подготовки, удаляется из нее (включая отмену назначения), то служба отключает этого пользователя в целевой системе при обновлении.
8. Если пользователь, который ранее был в области для подготовки, отключается или обратимо удаляется в исходной системе, то служба отключает этого пользователя в целевой системе при обновлении.
9. Если пользователь, который ранее был в области для подготовки, необратимо удаляется в исходной системе, то служба удаляет этого пользователя в целевой системе. В Azure AD пользователи не необратимо удаляются через 30 дней после они обратимо удаленные.
10. Сохраняет новый водяной знак в конце добавочной синхронизации, который обеспечивает начальную точку для более поздней версии добавочных синхронизаций.

>[!NOTE]
> При необходимости можно отключить **создать**, **обновление**, или **удалить** операций с помощью **действия с целевыми объектами** флажки в [Сопоставления](customize-application-attributes.md) раздел. Логикой отключения пользователей во время обновления также управляет сопоставление атрибутов из поля, например accountEnabled.

Служба подготовки длится двумя межсетевыми экранами добавочную синхронизацию неопределенно долгое время, через интервалы, определенные в [учебник, в конкретных приложениях](../saas-apps/tutorial-list.md), пока не произойдет одно из следующих событий:

- Служба останавливается вручную с помощью портала Azure или соответствующей команды API Graph. 
- Активируется новая начальная синхронизации с помощью параметра **Clear state and restart** (Очистить состояние и перезапустить) на портале Azure или с помощью соответствующей команды API Graph. Это действие удаляет все сохраненные водяные знаки и вызывает все исходные объекты должны быть оценены повторно.
- Из-за изменений сопоставлений атрибутов или фильтров области активируется Новая начальная синхронизация. Это действие также удаляет все сохраненные водяные знаки и вызывает все исходные объекты должны быть оценены повторно.
- Процесс подготовки переходит в карантин (см. ниже) из-за высокой частоты ошибок и остается в карантине более четырех недель. В этом случае служба будет автоматически отключена.

### <a name="errors-and-retries"></a>Ошибки и повторные попытки

Если отдельного пользователя не добавлен, обновлен или удален в целевой системе, из-за ошибки в целевой системе, операция повторяется во время следующего цикла синхронизации. Если операция с пользователем продолжает завершаться сбоем, то повторные попытки будут выполняться с меньшей частотой вплоть до одной попытки в день. Чтобы устранить ошибку, администраторы должны проверить [журналы аудита](check-status-user-account-provisioning.md) для «дополнительной обработки» события, чтобы определить корневой первопричину и предпринять соответствующие действия. Ниже перечислены распространенные ошибки.

- Не заполнен атрибут пользователей в исходной системе, требуемый в целевой системе.
- Пользователи, имеющие значение атрибута в исходной системе, для которого имеется ограничение уникальности в целевой системе и то же значение присутствует в другой записи пользователя.

Эти ошибки можно устранить, настроив значения атрибутов для затронутого пользователя в исходной системе или настроив сопоставления атрибутов таким образом, чтобы не возникали конфликты.   

### <a name="quarantine"></a>Карантин

Если большинство или все вызовы к целевой системе последовательно ошибкой из-за ошибки (например, для неправильных учетных данных администратора), задание подготовки переходит в состояние «карантина». Это состояние обозначается в [сводный отчет о подготовке](check-status-user-account-provisioning.md) и по электронной почте, если были настроены уведомления по электронной почте на портале Azure. 

После перехода в карантин частота добавочной синхронизации постепенно уменьшается до одного раза в день. 

Задание подготовки будет удален из карантина после всех виновного ошибок являются постоянными, и начнется следующий цикл синхронизации. Если задания подготовки остается в карантине более четырех недель, оно отключается.


## <a name="how-long-will-it-take-to-provision-users"></a>Сколько времени занимает подготовка пользователей?

Производительность зависит от того, выполняется ли задание подготовки начальную синхронизацию или добавочную синхронизацию.

Для **начальной синхронизации**, время задания зависит от многих факторов, включая количество пользователей и групп в области для подготовки и общее число пользователей и группы в исходной системе. Полный список факторов, влияющих на производительность начальной синхронизации, кратко описан далее в этом разделе.

Для **добавочной синхронизации** длительность задания зависит от количества изменений, обнаруженных в данном цикле синхронизации. Если обнаружено менее 5000 изменений пользователей или членства в группах, задание может завершиться в течение одного цикла добавочной синхронизации. 

В следующей таблице перечислены данные о времени синхронизации, необходимом для общих сценариев подготовки. В этих сценариях исходной системой является Azure AD, а целевой — приложение SaaS. Указанное время основано на статистическом анализе заданий синхронизации для приложений SaaS ServiceNow, Workplace, Salesforce и Google Apps.


| Конфигурация области | Пользователи, группы и участники в области | Длительность начальной синхронизации | Длительность добавочной синхронизации |
| -------- | -------- | -------- | -------- |
| Синхронизация только назначенных пользователей и групп |  < 1000 |  < 30 минут | < 30 минут |
| Синхронизация только назначенных пользователей и групп |  1000–10 000 | 142–708 минут | < 30 минут |
| Синхронизация только назначенных пользователей и групп |   10 000–100 000 | 1170–2,340 минут | < 30 минут |
| Синхронизация всех пользователей и групп в Azure AD |  < 1000 | < 30 минут  | < 30 минут |
| Синхронизация всех пользователей и групп в Azure AD |  1000–10 000 | 30–120 минут | < 30 минут |
| Синхронизация всех пользователей и групп в Azure AD |  10 000–100 000  | 713–1425 минут | < 30 минут |
| Синхронизация всех пользователей в Azure AD|  < 1000  | < 30 минут | < 30 минут |
| Синхронизация всех пользователей в Azure AD | 1000–10 000  | 43–86 минут | < 30 минут |


Для конфигурации **синхронизации только назначенных пользователей и групп** можно использовать следующие формулы, чтобы определить приблизительную минимальную и максимальную длительность **начальной синхронизации**:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Сведения о факторах, влияющих на время, необходимое для завершения **начальной синхронизации**:

- Общее количество пользователей и групп, которые будут подготовлены.

- Общее количество пользователей, групп и участников группы, находящихся в исходной системе (Azure AD).

- Ли пользователи в области для подготовки сопоставляются с существующим пользователям в целевом приложении или должны быть созданы в первый раз. Заданий синхронизации, для которых все пользователи создаются в первый раз занимает приблизительно *два раза* как синхронизировать задания, для которых все пользователи сопоставляются с существующим пользователям.

- Количество ошибок в [журналах аудита](check-status-user-account-provisioning.md). Производительность также снижается, если возникает большое количество ошибок и служба подготовки перешла в состояние карантина.    

- Ограничения частоты и регулирования запросов, реализованные целевой системой. Некоторые целевые системы накладывают ограничения на частоту запросов и регулирования, которые могут повлиять на производительность во время больших операций синхронизации. В этих условиях приложение, получающее слишком много запросов за короткий промежуток времени, может снизить частоту реагирования или закрыть подключение. Чтобы повысить производительность, соединитель необходимо настроить таким образом, чтобы количество отправляемых запросов соответствовало количеству запросов, которые может обработать приложение. Соединители подготовки, созданные корпорацией Майкрософт, позволяют внести эту корректировку. 

- Количество и размеры назначенных групп. Синхронизация назначенных групп длится дольше, чем синхронизация пользователей. Число и размер назначенных групп оказывают влияние на производительность. Если в приложении [включена функция сопоставления для синхронизации объектов группы](customize-application-attributes.md#editing-group-attribute-mappings), свойства группы (например, названия групп и членства) синхронизируются в дополнение к пользователям. Эти дополнительные синхронизации занимают больше времени, чем синхронизация только объектов пользователей.


## <a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>Как узнать, правильно ли подготовлены пользователи?

Всех операций, выполняемых службой подготовки в Azure AD записываются журналы аудита. Это включает в себя все операции, выполняемые в исходной и целевой систем и данных пользователей, которые считывания или записи во время каждой операции чтения и записи.

Сведения о том, как читать журналы аудита на портале Azure, см. в разделе [подготовки руководство по отчетам](check-status-user-account-provisioning.md).


## <a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>Как устранить неполадки, связанные с подготовкой пользователей?

Руководство по устранению неполадок при автоматической подготовке пользователей со сценариями доступно в разделе [Проблемы при настройке и подготовке пользователей для приложения](application-provisioning-config-problem.md).


## <a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>Каковы рекомендации по развертыванию автоматической подготовки пользователей?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Пример поэтапного плана развертывания "исходящей" подготовки пользователей в приложении см. в документе [Outbound User Provisioning Deployment Plan](https://aka.ms/userprovisioningdeploymentplan) (План развертывания "исходящей" подготовки пользователей).

## <a name="more-frequently-asked-questions"></a>Другие часто задаваемые вопросы

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-b2b-users-in-azure-ad"></a>Поддерживается ли автоматическая подготовка пользователей для приложений SaaS для пользователей B2B в Azure AD?

Да, это можно использовать Azure AD пользователя, подготовки пользователей службы B2B подготовки (или гостя) в Azure AD для приложений SaaS.

Тем не менее для пользователей B2B для входа в приложение SaaS, с помощью Azure AD, в приложении SaaS необходим его на основе SAML единого входа настроена возможность определенным образом. Дополнительные сведения о настройке приложений SaaS для поддержки операции входа от пользователей B2B, см. в разделе [приложений SaaS, настройте для службы совместной работы B2B]( https://docs.microsoft.com/azure/active-directory/b2b/configure-saas-apps).

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-dynamic-groups-in-azure-ad"></a>Поддерживается ли автоматическая подготовка пользователей для приложений SaaS для динамических групп в Azure AD?

Да. Когда параметр «Синхронизация только назначенных пользователей и группы», служба подготовки пользователей Azure AD можно подготовить или отмену подготовки пользователей в приложении SaaS в зависимости от того, находятся ли они членами [динамическую группу](../users-groups-roles/groups-create-rule.md). Динамические группы также работают с параметром "Синхронизация всех пользователей и групп".

Тем не менее использование динамических групп может повлиять на общую производительность комплексной подготовки пользователей из Azure AD для приложений SaaS. При использовании динамических групп, примите во внимание эти предупреждения и рекомендации:

- Скорость подготовки и отзыва пользователя из динамической группы в приложении SaaS зависит от того, как быстро динамическая группа может оценить изменения членства. Сведения о том, как проверить состояние обработки динамической группы, см. в разделе [Проверка состояния обработки правила членства](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

- При использовании динамических групп, правила необходимо тщательно рассмотреть с пользователем при подготовке и отзыве помните, как потеря членства приводит к появлению отзыва события.

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-nested-groups-in-azure-ad"></a>Поддерживается ли автоматическая подготовка пользователей для приложений SaaS для вложенных групп в Azure AD?

№ Когда параметр «Синхронизация только назначенных пользователей и группы», служба подготовки пользователей Azure AD не может прочитать или подготовить пользователей, входящих в вложенных групп. Это только возможность считывать и подготовить пользователей, являющихся членами немедленно, явно назначенной группы.

Это ограничение назначений на основе группы для приложений, которое также влияет на единый вход. Оно описано в разделе [Использование группы для управления доступом к приложениям SaaS](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-saasapps ).

Обойти это ограничение, следует явно назначить (или другим способом [область действия в](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)) группы, которые содержат пользователей, которым нужно подготовить.

## <a name="related-articles"></a>Связанные статьи

- [Список руководств по интеграции приложений SaaS](../saas-apps/tutorial-list.md)
- [Настройка сопоставлений атрибутов для подготовки пользователей](customize-application-attributes.md)
- [Запись выражений для сопоставления атрибутов](functions-for-customizing-application-data.md)
- [Фильтры области для подготовки пользователей](define-conditional-rules-for-provisioning-user-accounts.md)
- [Автоматическая подготовка пользователей и групп из Azure Active Directory в приложениях с использованием SCIM](use-scim-to-provision-users-and-groups.md)
- [Общие сведения об API синхронизации Azure AD](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
