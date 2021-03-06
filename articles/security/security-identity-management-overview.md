---
title: Функции безопасности Azure, упрощающие управление удостоверениями | Документация Майкрософт
description: " В статье представлены общие сведения о базовых функциях безопасности Azure, которые помогут в управлении удостоверениями. Решения Майкрософт по управлению удостоверениями и доступом помогают ИТ-службам защищать доступ к приложениям и ресурсам в корпоративных центрах обработки данных и в облаке. Они предоставляют такие дополнительные уровни проверки, как Многофакторная идентификация и политики условного доступа. "
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: terrylan
Customer intent: As an IT Pro or decision maker I am trying to learn about identity management capabilities in Azure
ms.openlocfilehash: 29c04fc04b5d277e982a37402a128b2dbe787e2c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "57898505"
---
# <a name="azure-identity-management-security-overview"></a>Общие сведения о безопасности при управлении удостоверениями в Azure

 Управление удостоверениями — это процесс проверки подлинности и авторизации [субъектов безопасности](https://docs.microsoft.com/windows/security/identity-protection/access-control/security-principals). Он также включает в себя управление сведения об этих субъектах (удостоверениях). В число субъектов безопасности (удостоверений) могут входить службы, приложения, пользователи, группы и т. д. Решения по управлению идентификацией и доступом корпорации Майкрософт позволяют ИТ-отделам защитить доступ к приложениям и ресурсам в корпоративном центре обработки данных и в облаке. Такая защита предусматривает дополнительные уровни проверки, такие как Многофакторная идентификация Microsoft Azure и политики условного доступа. Наблюдение за подозрительной активностью с использованием расширенных отчетов, аудита и предупреждений помогает смягчить потенциальные риски безопасности. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) предоставляет функцию единого входа (SSO) для тысяч облачных приложений программного обеспечения как услуга (SaaS), а также доступ к локальным веб-приложениям.
 
Преимущества использования средств защиты Azure Active Directory (Azure AD):

* Создание единого удостоверения для каждого пользователя в пределах гибридной системы, управление этими удостоверениями, а также синхронизация пользователей, групп и устройств. 
* Предоставление единого входа в приложения, включая тысячи предварительно интегрированных приложений SaaS.
* Возможность безопасного доступа к приложениям с применением многофакторной проверки подлинности на основе правил для локальных и облачных приложений.
* Предоставление безопасного удаленного доступа к локальным веб-приложениям через прокси-сервер приложений Azure AD.

В статье содержатся общие сведения о базовых функциях безопасности Azure, которые помогут в управлении удостоверениями. Здесь также приводятся ссылки на статьи с дополнительными сведениями о каждой функции.  

В статье уделяется внимание следующим базовым возможностям управления идентификацией в Azure.

* Единый вход
* Обратный прокси-сервер.
* Многофакторная идентификация
* управление доступом на основе ролей (RBAC).
* Наблюдение за безопасностью, оповещения и отчеты на основе машинного обучения.
* Управление удостоверениями и доступом клиентов.
* Регистрация устройства
* Управление привилегированными пользователями (PIM)
* Защита идентификации
* Управление гибридными удостоверениями и Azure AD Connect
* Проверки доступа Azure AD

## <a name="single-sign-on"></a>Единый вход

Благодаря функции единого входа пользователи получают доступ ко всем приложениям и ресурсам, необходимым для работы. Для этого нужно только выполнить однократный вход с использованием одной учетной записи. После входа пользователю доступны все необходимые приложения без повторной проверки подлинности (например, ввода пароля).

Во многих организациях для эффективной работы пользователя используются такие приложения SaaS, как Office 365, Box и Salesforce. Обычно ИТ-специалистам приходилось отдельно создавать и обновлять учетные записи пользователей в каждом приложении SaaS, а пользователям нужно было запоминать новые пароли для каждого такого приложения.

Azure AD расширяет локальные среды Active Directory в облако. Теперь с помощью основной рабочей учетной записи пользователи могут входить не только на присоединенные к домену устройства и корпоративные ресурсы, но и во все необходимые для работы веб-приложения и приложения SaaS.

Теперь пользователям не нужно управлять несколькими наборами учетных данных, состоящих из имени и пароля. Вы можете предоставлять или отменять доступ к приложениям автоматически на основе членства в группах организации и статуса конкретного сотрудника. Microsoft Azure Active Directory предоставляет элементы управления безопасностью и доступом, с помощью которых можно централизованно контролировать доступ пользователей к приложениям SaaS.

Дополнительные сведения

* [Общие сведения о едином входе](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../active-directory/manage-apps/what-is-single-sign-on.md)
* [Интеграция единого входа Azure Active Directory с приложениями SaaS](../active-directory/manage-apps/configure-single-sign-on-portal.md)

## <a name="reverse-proxy"></a>Обратный прокси-сервер.

Прокси-сервер приложений Azure AD позволяет публиковать такие локальные приложения, как сайты [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US), [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), а также приложения на основе [IIS](https://www.iis.net/) в частной сети. Он также обеспечивает безопасный доступ к ним для пользователей вне локальной сети. Прокси-сервер приложений обеспечивает удаленный доступ и единый вход для многих типов локальных веб-приложений, а также для тысяч приложений SaaS, поддерживаемых Azure AD. Сотрудники могут войти в приложения дома на собственных устройствах и пройти проверку подлинности через этот облачный прокси-сервер.

Дополнительные сведения

* [Включение прокси приложения Azure AD](../active-directory/manage-apps/application-proxy-enable.md)
* [Публикация приложений с помощью прокси приложения Azure AD](../active-directory/active-directory-application-proxy-publish.md)
* [Единый вход с помощью прокси приложения](../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Работа с условным доступом](../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Многофакторная идентификация

Многофакторная идентификация Azure – это метод аутентификации, который требует применения более одного метода проверки и добавляет критически важный второй уровень безопасности для операций входа и транзакций пользователя. Многофакторная идентификация помогает защитить доступ к данным и приложениям, при этом не усложняя процесс входа пользователя в систему. Она обеспечивает строгую проверку подлинности разными способами: с помощью телефонных звонков, текстовых сообщений, уведомлений в мобильном приложении, кодов подтверждения или маркеров OAuth сторонних поставщиков.

Дополнительные сведения

* [Многофакторная идентификация](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Что такое Многофакторная идентификация Azure?](../active-directory/authentication/multi-factor-authentication.md)
* [Принципы работы службы Многофакторной идентификации Azure](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="rbac"></a>RBAC

RBAC — это система авторизации на основе Azure Resource Manager, которая обеспечивает точное управление доступом к ресурсам в Azure. RBAC позволяет детально управлять уровнем доступа пользователей. Например, одному пользователю можно разрешить управлять только виртуальными сетями, а другому — управлять всеми ресурсами в группе ресурсов. В Azure есть несколько встроенных ролей. Ниже перечислены четыре основные встроенные роли. Первые три роли охватывают все типы ресурсов.

Дополнительные сведения

* [Что такое управление доступом на основе ролей (RBAC)?](../role-based-access-control/overview.md)
* [Встроенные роли управления доступом на основе ролей в Azure](../role-based-access-control/built-in-roles.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Наблюдение за безопасностью, оповещения и отчеты на основе машинного обучения.

Мониторинг безопасности, оповещения, а также отчеты на базе машинного обучения, которые позволяют определить несогласованные схемы доступа, помогают защитить бизнес-процессы. Отчеты Azure Active Directory об использовании и получении доступа помогут оценить целостность и защищенность каталога данных в вашей организации. C помощью этой информации администратор каталога может более точно определить источники возможных угроз безопасности, что позволяет правильно спланировать их устранение.

На портале Microsoft Azure отчеты делятся на следующие категории:

* **Отчеты об аномалиях**. Содержат сведения о событиях входа, которые мы считаем необычными. Наша цель — уведомлять вас о таких событиях, чтобы вы могли определить, является ли то или иное событие подозрительным.
* **Отчеты о встроенных приложениях**. Предоставляют информацию о том, как ваша организация использует облачные приложения. Azure Active Directory обеспечивает интеграцию с множеством облачных приложений.
* **Отчеты об ошибках**. Указывают на ошибки, которые могли возникнуть при подготовке учетных записей для внешних приложений.
* **Пользовательские отчеты**. Предоставляют данные о действиях для входа в устройство для конкретного пользователя.
* **Журналы действий**. Содержат записи обо всех событиях аудита за последние 24 часа, 7 дней или 30 дней, а также записи об изменениях в составе групп, событиях сброса пароля и регистрации.

Дополнительные сведения

* [Просмотр отчетов о доступе и использовании](../active-directory/active-directory-view-access-usage-reports.md)
* [Приступить к работе со средством создания отчетов Azure Active Directory](../active-directory/active-directory-reporting-getting-started.md)
* [Руководство по отчетам Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Управление удостоверениями и доступом клиентов.

Active Directory B2C – глобальная высокодоступная служба управления удостоверениями для клиентских приложений, которая обеспечивает масштабируемость до сотен миллионов удостоверений. Служба интегрируется с мобильными и веб-платформами. Клиенты могут входить в ваши приложения через настраиваемый интерфейс, используя существующие учетные записи социальных сетей или создавая новые.

Раньше разработчикам приложений, которым было необходимо регистрировать потребителей и выполнять их вход в своих приложениях, приходилось записывать собственный код. Кроме того, для хранения имен пользователей и паролей приходилось использовать локальные базы данных или системы. Azure Active Directory B2C помогает организациям оптимизировать интеграцию управления удостоверениями со своими приложениями с помощью безопасной платформы, основанной на стандартах, и широкого набора расширяемых политик.

Azure Active Directory B2C позволяет клиентам регистрироваться в приложениях с использованием существующих учетных записей в социальных сетях (Facebook, Google, Amazon, LinkedIn). Кроме того, пользователи могут создавать новые учетные данные (электронный адрес и пароль или имя пользователя и пароль).

Дополнительные сведения

* [Что такое Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Что такое Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)
* [Типы приложений, используемые в Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Регистрация устройства

Регистрация устройств в Microsoft Azure Active Directory – это основа всех сценариев [условного доступа](../active-directory/active-directory-conditional-access-device-registration-overview.md) с использованием устройств. В ходе регистрации служба регистрации устройств Azure Active Directory назначает устройству идентификатор, который используется для аутентификации устройства при входе пользователя. Аутентифицированное устройство и его атрибуты затем можно использовать для принудительного соблюдения политик условного доступа в приложениях, размещенных в облаке и локально.

При применении вместе с таким решением управления мобильными устройствами, как Intune, в атрибуты устройства в Azure Active Directory добавляются дополнительные данные о нем. Вы можете создавать правила условного доступа, которые обеспечивают доступ с устройств в соответствии с вашими стандартами безопасности и соблюдением нормативных требований.

Дополнительные сведения

* [Знакомство с регистрацией устройств в Azure Active Directory](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Автоматическая регистрация в Azure Active Directory присоединенных к домену устройств Windows](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Настройка автоматической регистрации присоединенных к домену устройств Windows в Azure Active Directory](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Управление привилегированными пользователями (PIM)

С помощью Azure AD Privileged Identity Management вы можете контролировать и отслеживать пользователей с привилегированными удостоверениями и доступ к ресурсам в Azure AD и других веб-службах Майкрософт, таких как Office 365 и Microsoft Intune.

Пользователям иногда требуется выполнять привилегированные операции с ресурсами в Azure или Office 365 в других приложениях SaaS. Эта потребность, как правило, означает, что организации должны обеспечить пользователю постоянный привилегированный доступ в Azure AD. Такой доступ является возрастающей угрозой безопасности ресурсов, размещенных в облаке, поскольку организация не может эффективно отслеживать действия пользователей с привилегиями администраторов. Кроме того, скомпрометированная учетная запись с привилегированным доступом могла повлиять на общую безопасность облака в организации. Azure AD Privileged Identity Management помогает снизить этот риск.

С расширенными возможностями Azure AD Privileged Identity Management вы можете:

* Видеть, какие пользователи являются администраторами Azure AD.
* Своевременно включать JIT по требованию к службам Microsoft Services, таким как Office 365 и Intune.
* Получать отчеты по данным журнала доступа администраторов и изменениям в назначениях администраторов.
* Получать оповещения о доступе к привилегированным ролям.

Дополнительные сведения

* [Что такое Azure AD Privileged Identity Management?](../active-directory/privileged-identity-management/pim-configure.md)
* [Назначение ролей каталога Azure AD в PIM ](../active-directory/privileged-identity-management/pim-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Защита идентификации

Служба защиты идентификации Azure AD — это служба безопасности, которая предоставляет согласованное представление всех событий риска и возможных уязвимостей, которые могут влиять на корпоративные удостоверения. Защита идентификации использует существующие возможности обнаружения аномалий Azure Active Directory, которые доступны в отчетах об аномальных действиях Azure Active Directory. Защита идентификации также вводит новые типы событий риска, которые могут обнаружить аномалии в режиме реального времени.

Дополнительные сведения

* [Защита идентификации Azure AD](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD and Identity Show: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview) (Channel 9. Передача об идентификации в Azure AD. Общие сведения о защите идентификации)

## <a name="hybrid-identity-managementazure-ad-connect"></a>Управление гибридными удостоверениями и Azure AD Connect

Решения для идентификации Майкрософт обладают локальными и облачными возможностями, создавая единое удостоверение пользователя для проверки подлинности и авторизации для всех ресурсов, независимо от их расположения. Мы называем это гибридной идентификацией. Azure AD Connect — это инструмент Майкрософт для достижения ваших целей гибридной идентификации. Таким образом вы сможете предоставить пользователям возможность получать доступ с использованием одного удостоверения для приложений Office 365, Azure и программного обеспечения как услуги (SaaS), интегрированных с Azure AD. Она предоставляет следующие возможности.

* Синхронизация
* AD FS и интеграция федерации
* Сквозная проверка подлинности
* Мониторинг работоспособности

Дополнительные сведения

* [Hybrid identity white paper](https://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Блог группы Azure AD](https://blogs.technet.microsoft.com/ad/)

## <a name="azure-ad-access-reviews"></a>Проверки доступа Azure AD

Проверки доступа Azure Active Directory (Azure AD) позволяют организациям эффективно управлять членством в группе, доступом к корпоративным приложениям и назначением привилегированных ролей.

Дополнительные сведения

* [Проверки доступа Azure AD](../active-directory/governance/access-reviews-overview.md)
* [Управление пользовательским доступом с помощью проверок доступа Azure AD](../active-directory/governance/access-reviews-overview.md)
