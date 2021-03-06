---
title: Условия использования — Azure Active Directory | Документация Майкрософт
description: Описывает, как приступить к работе с Azure Active Directory условия использования для предоставления сведений для сотрудников или гостей перед получением доступа.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/03/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1f03cd518a15d08971968e04fa69954951c77e0
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2019
ms.locfileid: "59052362"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Функция "Условия использования Azure Active Directory"
Функция "Условия использования Azure AD" — это простой способ, которым организации могут предоставлять сведения своим пользователям. Благодаря этой презентации пользователи видят соответствующие заявления об отказе согласно юридическим требованиям и для соответствия стандартам. В этой статье описывается, как приступить к работе с условиями использования.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Видеообзоры

В следующем видео представлен краткий обзор условий использования.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Дополнительные видеоролики см. по следующим ссылкам:
- [Как развернуть условия использования в Azure Active Directory](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Как развернуть условия использования в Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>Что можно делать с помощью условий использования?
Условия использования Azure AD предоставляют следующие возможности:
- можно требовать от сотрудников или гостей принять ваши условия использования, чтобы получить доступ;
- можно требовать от сотрудников или гостей принять ваши условия использования на каждом устройстве, чтобы получить доступ;
- можно требовать от сотрудников или гостей принимать ваши условия использования по расписанию с периодическим повторением, чтобы получить доступ.
- предоставлять общие условия использования для всех пользователей в вашей организации;
- предоставлять определенные условия использования на основе атрибутов пользователя (например, для медсестер и врачей или местных и международных сотрудников, что выполняется с помощью [динамических групп](../users-groups-roles/groups-dynamic-membership.md));
- отображать определенные условия использования при доступе к особо важным для бизнеса приложениям, таким как Salesforce;
- отображать условия использования на разных языках;
- составлять список тех, кто принял или не принял условия использования;
- помогать в соблюдении законов о конфиденциальности;
- отображать журнал действий по условиям использования для соответствия требованиям законодательства и аудита.
- создавать условия использования интерфейсов [API Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) (в настоящее время доступны в предварительной версии) и управлять ими.

## <a name="prerequisites"></a>Технические условия
Чтобы настроить условия использования Azure AD и пользоваться ими, необходимо иметь следующее:

- Подписка Azure AD Premium P1, P2, EMS E3 или EMS E5.
    - Если у вас нет одной из этих подписок, вы можете [получить подписку Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) или [активировать пробную версию Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Одна из следующих учетных записей администратора для каталога, который нужно настроить.
    - глобального администратора;
    - Администратор безопасности
    - Администратор условного доступа

## <a name="terms-of-use-document"></a>Документ "Условия использования"

Для условий использования Azure AD применяется формат PDF. Это может быть любое содержимое, например существующая контрактная документация, позволяющее собрать пользовательские соглашения во время входа пользователя. Для поддержки пользователей на мобильных устройствах в PDF-файлах рекомендуется использовать 24-й размер шрифта.

## <a name="add-terms-of-use"></a>Добавление условий использования
Когда ваш документ "Условия использования" будет готов, используйте следующую процедуру, чтобы добавить его.

1. Войдите на портал Azure от имени глобального администратора, администратора безопасности или администратора условного доступа.

1. Перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

    ![Колонка "Условия использования"](./media/terms-of-use/tou-blade.png)

1. Щелкните **Добавить условия**.

    ![Добавление условий использования](./media/terms-of-use/new-tou.png)

1. В поле **Имя** введите имя для условий использования, которое будет применено на портале Azure.

1. В поле **Отображаемое имя** введите название, которое будет отображаться для пользователей при входе.

1. **Документ "Условия использования"**: перейдите к PDF-файлу с окончательными условиями использования и выберите его.

1. Выберите язык для документа с условиями использования. Настроив параметр языка, вы сможете передать несколько экземпляров условий использования на разных языках. Версия условий использования, отображаемая для пользователя, будет зависеть от настроек браузера.

1. Чтобы обязать пользователей просмотреть условия использования, прежде чем принять их, для параметра **Требовать, чтобы пользователи развернули условия использования** выберите значение **Вкл**.

1. Чтобы пользователи принимали условия использования на каждом устройстве, с помощью которого они осуществляют доступ, для параметра **Требовать согласие пользователей для каждого устройства** выберите значение **Вкл**. Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory"](#per-device-terms-of-use).

1. Если требуется, чтобы срок действия согласия с условиями использования истекал по расписанию, для параметра **Истечение срока действия согласия** выберите значение **Вкл**. Когда этот параметр включен, отображаются два дополнительных параметра расписания.

    ![Истечение срока действия согласия](./media/terms-of-use/expire-consents.png)

1. Используйте параметры **Начало истечения срока действия** и **Частота**, чтобы указать расписание для истечения срока действия условий использования. В следующей таблице показан результат нескольких примеров настроек.

    | Начало истечения срока действия | Frequency | Результат |
    | --- | --- | --- |
    | Текущая дата  | Ежемесячная | Начиная с сегодняшнего дня пользователям необходимо принимать условия использования, а затем повторно принимать их каждый месяц. |
    | Дата в будущем  | Ежемесячная | Начиная с сегодняшнего дня пользователи должны принимать условия использования. Когда наступит указанная дата, срок действия согласия истечет и пользователи должны будут повторно принимать условия каждый месяц.  |

    Например, если задать срок действия с **1 января** и частоту **Ежемесячно**, то вот как может истекать срок действия принятия для двух пользователей.

    | Пользователь | Дата первого принятия | Дата первого истечения срока действия | Дата второго истечения срока действия | Дата третьего истечения срока действия |
    | --- | --- | --- | --- | --- |
    | Алиса | 1 января | 1 февраля | 1 марта | 1 апреля |
    | Владимир | 15 января | 1 февраля | 1 марта | 1 апреля |

1. Используйте параметр **Duration before re-acceptance requires (days)** (Период до обязательного повторного принятия (дн)), чтобы указать число дней, предшествующих обязательному повторному принятию пользователем условий использования. Это позволяет пользователям следовать собственному расписанию. Например, если установить период в **30** дней, то вот как может истекать срок действия принятия для двух пользователей.

    | Пользователь | Дата первого принятия | Дата первого истечения срока действия | Дата второго истечения срока действия | Дата третьего истечения срока действия |
    | --- | --- | --- | --- | --- |
    | Алиса | 1 января | 31 января | 2 марта | 1 апреля |
    | Владимир | 15 января | 14 февраля | 16 марта | 15 апреля |

    Можно использовать параметры **Истечение срока действия согласия** и **Duration before re-acceptance requires (days)** (Период до обязательного повторного принятия (дн)) вместе, но обычно используется только один из них.

1. В разделе **Условный доступ** из списка **Enforce with conditional access policy template** (Использование шаблона политики условного доступа) выберите шаблон для применения условий использования.

    ![Шаблоны условного доступа](./media/terms-of-use/conditional-access-templates.png)

    | Шаблон | ОПИСАНИЕ |
    | --- | --- |
    | **Доступ к облачным приложениям для всех гостей** | Политика условного доступа будет создана для всех гостей и всех облачных приложений. Эта политика влияет на портал Azure. После ее создания может потребоваться выйти из системы, а затем снова войти. |
    | **Доступ к облачным приложениям для всех пользователей** | Будет создана политика условного доступа для всех пользователей и всех облачных приложений. Эта политика влияет на портал Azure. После ее создания потребуется выйти из системы, а затем снова войти. |
    | **Настраиваемая политика** | Выберите пользователей, группы и приложения, к которым будут относиться эти условия использования. |
    | **Создать политику условного доступа позже** | Условия использования будут показаны в списке по предоставлению управления при создании политики условного доступа. |

    >[!IMPORTANT]
    >Элементы управления политиками условного доступа (включая условия использования) не поддерживают принудительное применение в учетных записях служб. Рекомендуется исключить все учетные записи служб из политики условного доступа.

     Пользовательские политики условного доступа реализуют детализированные условия использования, вплоть до конкретного облачного приложения или группы пользователей. Дополнительные сведения см. в разделе [ Требование принятия условий использования перед доступом к облачным приложениям](require-tou.md)

1. Нажмите кнопку **Создать**.

    Если выбран настраиваемый шаблон условного доступа, откроется новый экран, где можно будет создать настраиваемую политику условного доступа.

    ![Настраиваемая политика](./media/terms-of-use/custom-policy.png)

    Теперь вы увидите ваши новые условия использования.

    ![Добавление условий использования](./media/terms-of-use/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Просмотр отчета о тех, кто принял или отклонил условия
В колонке "Условия использования" показано количество пользователей, которые приняли или отклонили условия использования. Имена и количество тех, кто принял или отклонил условия, хранятся в течение жизненного цикла Условий использования.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

    ![Колонка "Условия использования"](./media/terms-of-use/view-tou.png)

1. Чтобы просмотреть текущее состояние пользователей, в колонке "Условия использования" щелкните числа возле счетчика пользователей, которые **приняли** или **отклонили** условия.

    ![Согласие с условиями использования](./media/terms-of-use/accepted-tou.png)

1. Чтобы просмотреть журнал для отдельного пользователя, щелкните многоточие (**…**) и выберите **Просмотреть журнал**.

    ![Меню "Просмотр журнала"](./media/terms-of-use/view-history-menu.png)

    В области "Просмотр журнала" отобразится журнал всех принятий, отклонений и сроков действия.

    ![Область "Просмотр журнала"](./media/terms-of-use/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Просмотр журналов аудита Azure AD
Если вы хотите просмотреть дополнительные действия, в Условиях использования Azure AD предусмотрены журналы аудита. Принятие условий каждым пользователем инициирует событие, которое регистрируется в журналах аудита и хранится в течение **30 дней**. Эти журналы можно просмотреть на портале или загрузить в виде CSV-файла.

Чтобы начать работу с журналами аудита Azure AD, выполните следующие действия.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

1. Выберите условия использования.

1. Нажмите кнопку **Просмотреть журналы аудита**

    ![Колонка "Условия использования"](./media/terms-of-use/audit-tou.png)

1. На экране "Журналы аудита" Azure AD можно фильтровать информацию с помощью списков, чтобы получить информацию журнала аудита для конкретного целевого объекта.

    Вы также можете скачать данные в CSV-файл для использования на локальном компьютере, щелкнув **Загрузить**.

    ![Журналы аудита](./media/terms-of-use/audit-logs-tou.png)

    Если щелкнуть журнал, отобразится область с дополнительными сведениями о действиях.

    ![Сведения о действиях](./media/terms-of-use/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>Как условия использования выглядят для пользователей
После создания и применения условий использования охватываемые во время входа пользователи увидят следующие экраны.

![Вход пользователей в веб-службы](./media/terms-of-use/user-tou.png)

Пользователи могут просматривать условия использования и, при необходимости используйте кнопки для увеличения и уменьшения масштаба.

![Просмотреть условия использования, с помощью кнопок масштабирования](./media/terms-of-use/zoom-buttons.png)

На рис. ниже показано, как выглядят условия использования на мобильных устройствах.

![Вход пользователей в мобильные службы](./media/terms-of-use/mobile-tou.png)

Только пользователи должны принять условия использования один раз, и они больше не будет отображаться условия использования на последующие входы в систему.

### <a name="how-users-can-review-their-terms-of-use"></a>Каким образом пользователи могут изменить свое решение об условиях использования
Чтобы изменить решение об условиях использования, принятое пользователем, ему необходимо сделать следующее.

1. Войдите на портал [https://myapps.microsoft.com](https://myapps.microsoft.com).

1. В правом верхнем углу щелкните свое имя, а затем выберите **Профиль**.

    ![Профиль](./media/terms-of-use/tou14.png)

1. На странице профиля щелкните **Ознакомьтесь с условиями использования**.

    ![Профиль — просмотр условий использования](./media/terms-of-use/tou13a.png)

1. Здесь можно просмотреть принятые вами условия использования.

## <a name="edit-terms-of-use-details"></a>Изменение условий использования
Вы можете изменить некоторые сведения в условиях использования, но не существующий документ. Ниже описано, как изменить эти сведения.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

1. Выберите условия использования, которые нужно изменить.

1. Щелкните **Изменить условия**.

1. В области "Изменение условий использования" измените значения параметров "Имя", "Отображаемое имя" или "Требовать, чтобы пользователи развернули условия использования".

    Если вы хотите изменить другие параметры, например, выбрать PDF-файл, требовать согласие пользователей для каждого устройства, указать срок действия согласий, настроить длительность перед повторным принятием или политику условного доступа, необходимо создать новые условия использования.

    ![Изменение условий использования](./media/terms-of-use/edit-tou.png)

1. Нажмите кнопку **Сохранить**, чтобы сохранить изменения.

    Когда вы сохраните изменения, пользователям не потребуется принимать их.

## <a name="add-a-terms-of-use-language"></a>Добавление языка условий использования
Ниже описано, как добавить язык условий использования.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

1. Выберите условия использования, которые нужно изменить.

1. В области сведений щелкните вкладку **Языки**.

    ![Добавление условий использования](./media/terms-of-use/languages-tou.png)

1. Щелкните **Добавить язык**.

1. В области "Добавление языка условий использования" передайте локализованный PDF-файл и выберите язык.

    ![Добавление условий использования](./media/terms-of-use/language-add-tou.png)

1. Чтобы добавить язык, щелкните **Добавить**.

## <a name="per-device-terms-of-use"></a>Отдельные условия использования для каждого устройства

Параметр **Требовать согласие пользователей для каждого устройства** позволяет требовать от пользователей, чтобы они принимали условия использования на каждом устройстве, с помощью которого они осуществляют доступ. Пользователю потребуется присоединить свое устройство к Azure AD. Если устройство присоединено, его идентификатор используется для принудительного применения условий использования на каждом устройстве.

Ниже приведен список поддерживаемых платформ и программного обеспечения.

> [!div class="mx-tableFixed"]
> |  | iOS | Android | Windows 10 | Другие |
> | --- | --- | --- | --- | --- |
> | **Собственное приложение** | Yes | Да | Yes |  |
> | **Microsoft Edge** | Yes | Да | Yes |  |
> | **Internet Explorer** | Yes | Да | Yes |  |
> | **Chrome (с расширением)** | Yes | Да | Yes |  |

Отдельные условия использования для каждого устройства имеют следующие ограничения:

- устройство может быть присоединено только к одному клиенту;
- у пользователя должны быть разрешения на присоединение устройства;
- приложение для регистрации Intune не поддерживается.
- Пользователи Azure AD B2B не поддерживаются.

Если устройство пользователя не присоединено, он получит сообщение о том, что требуется присоединить устройство. Возможности взаимодействия будут зависеть от платформы и программного обеспечения.

### <a name="join-a-windows-10-device"></a>Присоединение устройства Windows 10

Если пользователь использует Windows 10 и Microsoft Edge, он получит сообщение, аналогичное приведенному ниже, предлагающее [присоединить устройство](../user-help/user-help-join-device-on-network.md#to-join-an-already-configured-windows-10-device).

![Windows 10 и Microsoft Edge: запрос на присоединение устройства](./media/terms-of-use/per-device-win10-edge.png)

Если используется Chrome, то пользователю будет предложено установить [расширение для учетных записей Windows 10](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="browsers"></a>Браузеры

Если пользователь использует браузер, который не поддерживается, ему будет предложено использовать другой браузер.

![Неподдерживаемый браузер](./media/terms-of-use/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Удаление условий использования
Вы можете удалить старые условия использования, используя следующую процедуру.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

1. Выберите условия использования, которые вы хотите удалить.

1. Нажмите кнопку **Удалить условия**.

1. В сообщении, которое появится на экране с вопросом о том, нужно ли продолжить, нажмите кнопку **Да**.

    ![Удаление условий использования](./media/terms-of-use/delete-tou.png)

    Ваши новые условия использования больше не должны отображаться.

## <a name="deleted-users-and-active-terms-of-use"></a>Удаленные пользователи и активные условия использования
По умолчанию удаленный пользователь будет оставаться удаленным в Azure AD в течение 30 дней, на протяжении которых его при необходимости может восстановить администратор. По истечении 30 дней пользователь будет окончательно удален. Кроме того, с помощью портала Azure Active Directory глобальный администратор может явно [навсегда удалить недавно удаленного пользователя](../fundamentals/active-directory-users-restore.md) до истечения этого периода времени. Если пользователь удален навсегда, впоследствии данные об этом пользователе будут удалены из условий использования Active Directory. Сведения аудита об удаленных пользователях остаются в журнале аудита.

## <a name="policy-changes"></a>Изменения политики
Политики условного доступа вступают в силу немедленно. В этом случае администратор начнет видеть "грустные облака" или сообщение типа "Проблемы с токеном Azure AD". Администратору необходимо выйти и войти снова для удовлетворения требований новой политики.

>[!IMPORTANT]
> Охватываемым пользователям необходимо выйти и снова войти для удовлетворения требований новой политики, если:
> - политика условного доступа включена для условий использования;
> - созданы другие условия использования.

## <a name="b2b-guests-preview"></a>Гости B2B (предварительная версия)

Большинство организаций использует собственный процесс принятия сотрудниками условий использования и заявлений о конфиденциальности организации. Но как требовать аналогичное принятие условий от гостей B2B (бизнес-бизнес) в Azure AD, когда они добавляются с помощью SharePoint или Teams? С помощью условного доступа и условий использования вы можете применить политику непосредственно к гостевым пользователям B2B. Во время потока активации приглашения пользователю предоставляются условия использования. Сейчас эта возможность доступна в предварительной версии.

Условия использования отображаются только в том случае, если у пользователя есть гостевая учетная запись в Azure AD. SharePoint Online в настоящее время используется [нерегламентированных внешних процедура совместного использования получателей](/sharepoint/what-s-new-in-sharing-in-targeted-release) совместно использовать документ или папку, которая не требует, чтобы у гостевая учетная запись пользователя. В этом случае условия использования не отображаются.

![Все гостевые пользователи](./media/terms-of-use/b2b-guests.png)

## <a name="support-for-cloud-apps-preview"></a>Поддержка облачных приложений (предварительная версия)

Условия использования могут использоваться для различных облачных приложений, например Azure Information Protection и Microsoft Intune. Сейчас эта возможность доступна в предварительной версии.

### <a name="azure-information-protection"></a>Azure Information Protection

Можно настроить политику условного доступа для приложения Azure Information Protection и требовать принятия условий использования, когда пользователь открывает защищенный документ. Это позволит активировать условия использования перед тем, как пользователь впервые получит доступ к защищенному документу.

![Облачное приложение Azure Information Protection](./media/terms-of-use/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Регистрация в Microsoft Intune

Можно настроить политику условного доступа для приложения для регистрации Microsoft Intune и требовать принятия условий использования перед регистрацией устройства в Intune. Дополнительные сведения см. в записи блога о [выборе правильного решения условий использования для организации](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

![Облачное приложение Microsoft Intune](./media/terms-of-use/cloud-app-intune.png)

> [!NOTE]
> Приложение для регистрации Intune не поддерживается [отдельными условиями использования для каждого устройства](#per-device-terms-of-use).

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

**Вопрос. Как просмотреть при /, если пользователь принял условия использования?**<br />
Ответ. В колонке "Условия использования" щелкните число под надписью **Принято**. Вы также можете просматривать или выполнять поиск в журналах аудита Azure AD. Дополнительные сведения см. в разделах "Просмотр тех, кто принял или отклонил Условия использования" и [Просмотр журналов аудита Azure AD](#view-azure-ad-audit-logs).

**Вопрос. Как долго будет сведения?**<br />
Ответ. Пользователь учитывается в отчете "Условия использования" и имена тех, кто принял или отклонил, хранятся в течение всего жизненного цикла условий использования. Журналы аудита Azure AD хранятся в течение 30 дней.

**Вопрос. Почему Разное количество согласие с условиями использования отчетов и Azure AD журналы аудита?**<br />
Ответ. Отчет "Условия использования" хранится в течение всего времени его существования, а журналы аудита Azure AD хранятся в течение 30 дней. Кроме того, Условия использования отображают только состояние согласия пользователей. Например, если пользователь отклоняет, а затем принимает Условия использования, тогда Условия использования будут отображать только то, что пользователь принимает их. Если нужно просмотреть историю, используйте журнал аудита Azure AD.

**Вопрос. Если я изменить сведения для условий использования, необходимо ли пользователям принять их еще раз?**<br />
Ответ. Нет, если администратор изменяет сведения условий использования (например, изменяет имя, отображаемое имя, требование прочтения пользователями или добавляет язык), то пользователям не потребуется повторно принять новые условия.

**Вопрос. Можно ли обновить существующие условия использование документов?**<br />
Ответ. В настоящее время невозможно обновить существующий документ условий использования. Чтобы изменить такой документ, вам потребуется создать новый экземпляр условий использования.

**Вопрос. Если гиперссылки в терминах использование документов PDF, конечных пользователей будет возможность щелкнуть элемент их?**<br />
Ответ. PDF-файл отображается как JPEG-файл, поэтому щелкнуть какую-либо гиперссылку будет невозможно. Имеют ли возможность пользователи выбирать раздел **Проблемы с просмотром? Щелкните здесь**, что позволит преобразовать файл PDF в такой, где поддерживаются гиперссылки.

**Вопрос. Условия использования поддерживают несколько языков?**<br />
Ответ. Да. В настоящее время существует 108 различных языков, которые администратор может настроить для одного из Условий использования. Администратор может передать несколько PDF-файлов и пометить эти документы соответствующими тегами языков (до 108). Когда пользователь выполняет вход, мы определяем языковые параметры его браузера и отображаем соответствующий документ. Если совпадений нет, отображается документ по умолчанию, которым является первый переданный документ.

**Вопрос. Когда рекомендуется использовать активируются условия использования?**<br />
Ответ. Условия использования активируются, когда пользователь входит в систему.

**Вопрос. Для каких приложений можно применять условия использования?**<br />
Ответ. Вы можете создать политику условного доступа для корпоративных приложений, использующих современную аутентификацию. Дополнительные сведения см. в статье [Просмотр всех корпоративных приложений, которыми вы можете управлять в Azure Active Directory](./../manage-apps/view-applications-portal.md).

**Вопрос. Можно добавить несколько экземпляров условий использования для определенного пользователя или приложения?**<br />
Ответ. Да, для этого можно создать несколько политик условного доступа, предназначенных для этих групп или приложений. Если пользователь подпадает в область действия нескольких вариантов условий использования, он может принять только один вариант условий использования одновременно.

**Вопрос. Что произойдет, если пользователь отклонит условия использования?**<br />
Ответ. Пользователь не сможет получить доступ к приложению. Чтобы получить доступ, ему необходимо будет повторно войти в систему и принять условия.

**Вопрос. Существует возможность unaccept условия использования, которые ранее были приняты?**<br />
Ответ. Вы можете [просмотреть ранее принятые условия использования](#how-users-can-review-their-terms-of-use), но отказаться от согласия с ними в настоящее время невозможно.

**Вопрос. Что произойдет, если я использую, также условия Intune?**<br />
Ответ. Если вы настроили условия использования Azure AD и [условия Intune](/intune/terms-and-conditions-create), пользователь должен будет принять все эти условия. Дополнительные сведения см. в записи блога о [выборе правильного решения условий использования для организации](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

## <a name="next-steps"></a>Дальнейшие действия

- [Краткое руководство. Требование принятия условий использования перед доступом к облачным приложениям](require-tou.md)
- [Рекомендации по работе с условным доступом в Azure Active Directory](best-practices.md)
