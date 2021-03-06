---
title: Настройка утверждений о группах для приложений с Azure Active Directory | Документация Майкрософт
description: Сведения о том, как настроить утверждения группы для использования с Azure AD.
services: active-directory
documentationcenter: ''
ms.reviewer: paulgarn
manager: daveba
ms.component: hybrid
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/27/2019
ms.author: billmath
author: billmath
ms.openlocfilehash: 622a3ce0f80bd09bd09fa7ff097f68155318142d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58080362"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Настройка утверждений о группах для приложений с Azure Active Directory (общедоступная Предварительная версия)

Azure Active Directory можно предоставить сведения о членстве пользователей группы в маркеров для использования в приложениях.  Поддерживаются два основных типа:

- Группы, их Azure Active Directory с идентификатором объекта (OID) (общедоступная версия)
- Группы, определяемый SAMAccountName или GroupSID синхронизации Active Directory (AD), групп и пользователей (Предварительная версия)

> [!Note]
> Поддержка использования имен и локальных идентификаторов безопасности (SID) обеспечивает возможность перемещение существующих приложений из AD FS.    Группы, управляемой в Azure AD не содержат атрибуты, необходимые для выдачи этих утверждений.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-idps"></a>Групповые заявки для приложений, переход с AD FS и других поставщиков удостоверений

Во многих приложениях, которые настроены для проверки подлинности в AD FS используется сведения о членстве в группах в форме Windows AD группы атрибутов.   Эти атрибуты являются группы SAMAccountName, который может быть по доменное имя или идентификатор безопасности группы Windows.  Если приложение входит в федерацию с AD FS, AD FS использует функцию TokenGroups извлекаемого членство в группах для пользователя.

В соответствии с маркера, приложение получит от AD FS, группы и роли утверждения может передаваться содержащий домен указанием SAMAccountName, а не идентификатор объекта группы Azure Active Directory.

Ниже приведены поддерживаемые форматы для утверждения о группе.

- **Azure Active Directory GroupObjectId** (доступен для всех групп)
- **SAMAccountName** (доступен для групп, синхронизированных из Active Directory)
- **NetbiosDomain\samAccountName** (доступен для групп, синхронизированных из Active Directory)
- **DNSDomainName\samAccountName** (доступен для групп, синхронизированных из Active Directory)

> [!NOTE]
> Атрибуты SAMAccountName и OnPremisesGroupSID доступны только для объектов группы, синхронизированные из Active Directory.   Они недоступны на группы, созданные в Azure Active Directory или Office 365.   Приложения, которые зависят от атрибутов в локальной группе заставить синхронизированных только для групп.

## <a name="options-for-applications-to-consume-group-information"></a>Параметры для приложений, использующих сведения о группе

Один из способов для приложения, чтобы получать сведения о группе является вызов конечная точка группы Graph для получения членства в группах для проверенного пользователя. Этот вызов гарантирует, что все группы, которые пользователь является членом доступны, даже в том случае, когда нет большому числу групп и приложению необходимо перечислить все группы, пользователь не является членом.  Перечисление группы, затем не зависит от ограничения на размер маркера.

Тем не менее если приложение уже ожидает получить сведения о группе с помощью утверждений в маркере, полученные, Azure Active Directory можно настроить ряд параметров разные утверждения в соответствии с потребностями приложения.  Можно воспользоваться следующими вариантами:

- При использовании членства в группе для цели авторизации приложения (ли членство в группе получается из маркера или из графа), рекомендуется использовать идентификатор объекта группы, которое постоянными и уникальными в Azure Active Directory и доступны для всех групп .
- Если вы используете группу SAMAccountName для авторизации, используйте имена квалификатором домена;  имеет уменьшается вероятность ситуаций, которые могут возникнуть было конфликта имен. SAMAccountName сам по себе может быть уникальным в пределах домена Active Directory, но если более одного домена Active Directory синхронизируется с клиентом Azure Active Directory есть вероятность для более чем одной группе, чтобы иметь то же имя.
- Рассмотрите возможность использования [ролей приложения](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) для предоставления уровня косвенного обращения между членства в группах и приложением.   Затем приложение принимает решения об внутренней авторизации, в зависимости от роли clams в маркере.
- Если приложение настроено для получения атрибутов группы, синхронизированные из Active Directory и группы не содержит эти атрибуты не будут включены в заявках.
- Группа утверждения в маркерах Включить вложенные группы.   Если пользователь является членом группы b, группы b является членом GroupA утверждения группы для пользователя будет содержать GroupA и группы b. Для организаций с интенсивного использования вложенных групп и пользователей с большим количеством членства в группах число групп, перечисленных в маркере может увеличиваться размер маркера.   Azure Active Directory ограничение на количество групп, которые он будет выдавать маркер для утверждения SAML 150 и 200 для JWT.

> Предварительные требования для использования группы атрибутов, синхронизированных из Active Directory:   Группы должны быть синхронизированы из Active Directory с помощью Azure AD Connect.

Существуют два этапа настройки Azure Active Directory для создания имен групп для группы Active Directory.

1. **Синхронизировать имена групп из Active Directory** перед Azure Active Directory могут создавать имена групп или в локальной группе утверждения SID группы или роли, необходимые атрибуты нужно синхронизировать из Active Directory.  Необходимо запустить Azure AD Connect версии 1.2.70 или более поздней версии.   До версии 1.2.70 Azure AD Connect синхронизирует th групповых объектов из Active Directory, но не включает атрибуты имени необходимая группа по умолчанию.  Следует обновить до текущей версии.

2. **Настройка регистрации приложения в Azure Active Directory для включения в токены утверждений о группах** могут быть групповые заявки, настроенные в разделе корпоративных приложений на портале для коллекции или единого входа SAML не из коллекции приложений, или в разделе регистрации приложения с помощью манифест приложения.  Настройка утверждений о группах в приложение манифеста см. раздел «Настройка регистрация Azure Active Directory приложения для группы атрибутов» ниже.

## <a name="configure-group-claims-for-saml-applications-using-sso-configuration"></a>Настройка утверждения группы для приложения SAML, с помощью конфигурации единого входа

Чтобы настроить групповые заявки для коллекции или SAML не из коллекции приложений, откройте корпоративных приложений, щелкните приложение в списке и выберите настройку единого входа.

Щелкните значок редактирования рядом с «Группы, возвращаемые в маркере»

![утверждения пользовательского интерфейса](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

С помощью переключателей для выбора групп, которые должны быть включены в маркер

![утверждения пользовательского интерфейса](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Выбор | ОПИСАНИЕ |
|----------|-------------|
| **Все группы** | Группы безопасности и распространения перечислены.   Она вызывает ролей каталога, пользователю назначается для добавления в утверждение «wids» и все роли приложений, которые пользователю назначается для добавления в утверждение ролей. |
| **Группы безопасности** | Создает группы безопасности, пользователь не является членом в утверждение groups |
| **Список рассылки** | Выдает списков рассылки, пользователь не является членом |
| **Роль каталога** | Если пользователю назначена роли каталога они создаются как wids утверждения (группы, утверждение не будет создаваться) |

Например чтобы выпустить все пользователь является членом группы безопасности, выберите группы безопасности

![утверждения пользовательского интерфейса](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

### <a name="advanced-options"></a>Расширенные параметры

Метод выдаются утверждения о группе можно изменить с помощью параметров в разделе дополнительных параметров

Изменить имя утверждения о группе:  Если флажок установлен, с другим типом утверждения можно указать для утверждения о группе.   Введите тип утверждения в поле имя и необязательное пространство имен для утверждения в поле пространство имен.

![утверждения пользовательского интерфейса](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

Для создания групп с помощью Active Directory атрибуты вместо идентификаторы objectid Azure AD установите флажок «Return группы как имена вместо идентификаторов» и из раскрывающегося списка выбрать формат.  Этот параметр заменяет идентификатор объекта в утверждениях со строковыми значениями, содержащий имена групп.   Будут включены только те группы, синхронизированные из Active Directory в заявках.

![утверждения пользовательского интерфейса](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Некоторые приложения требуют, чтобы отображаться в утверждении «роли». Дополнительно можно создать группы пользователя как роли, установив флажок «Создавать группы, роли утверждения».

![утверждения пользовательского интерфейса](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

> [!NOTE]
> Если используется параметр для передачи данных группы как роли, только те группы, будут отображаться в утверждении роли.  Все роли приложений, которые назначены пользователю будет отсутствовать в утверждении роли.

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>Настройка регистрации приложения Azure AD для группы атрибутов

Также можно настроить групповые заявки в [необязательные утверждения](../../active-directory/develop/active-directory-optional-claims.md) раздел [манифест приложения](../../active-directory/develop/reference-app-manifest.md).

1. На портале "->" Azure Active Directory "->" приложение регистрации -> выберите приложение манифест, "->"

2. Включить утверждения членства в группах, изменив groupMembershipClaim

   Допустимыми значениями являются:

   - «Все»
   - «SecurityGroup»
   - «DistributionList»
   - «DirectoryRole»

   Например: 

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   По умолчанию группы идентификаторы objectid, будет использовано в группе значение утверждения.  Чтобы изменить значение утверждения для размещения в локальной среде атрибуты группы или изменить тип утверждения роли, используйте конфигурацию OptionalClaims следующим образом:

3. Задайте имя группы: Конфигурация необязательные утверждения.

   Если вы хотите группы в маркере, который должен содержать в локальном каталоге AD атрибутов группы в разделе необязательные утверждения указать, какой тип маркера необязательного утверждения, которые должны быть применены к, имя необязательно требование и другие свойства и требуемого.  Может быть перечислено несколько типов маркеров:

   - idToken для маркера OIDC Идентификации
   - accessToken маркера доступа OAuth или OIDC
   - Saml2Token для маркеров SAML.

   > [!NOTE]
   > Saml2Token типа относится к SAML1.1 и SAML 2.0 формат маркеров

   Для каждого соответствующего типа маркера измените утверждение групп для использования в разделе OptionalClaims в манифесте. OptionalClaims схема выглядит следующим образом:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | Необязательные утверждения схемы | Значение |
   |----------|-------------|
   | **Имя:** | Должен быть «группы» |
   | **Источник:** | Не используется. Не указан или указано значение null |
   | **важные:** | Не используется. Не указан или задано значение false |
   | **additionalProperties:** | Список дополнительных свойств.  Допустимые значения: «sam_account_name», «dns_domain_and_sam_account_name», «netbios_domain_and_sam_account_name», «emit_as_roles» |

   AdditionalProperties только один из «sam_account_name», «dns_domain_and_sam_account_name», «netbios_domain_and_sam_account_name» являются обязательными.  При наличии более одного используется первый, и любые другие игнорируются.

   Некоторые приложения требуют группы сведений о пользователе в утверждении роли.  Чтобы изменить тип утверждения, из группы заявляют, что утверждение роли, добавьте дополнительные свойства «emit_as_roles».  Значения группы, будет использовано в утверждении роли.

   > [!NOTE]
   > Если используется «emit_as_roles» любой роли приложения настроены, что пользователю назначена будет отображается в утверждении роли

### <a name="examples"></a>Примеры

Создайте группы, что имена групп в маркеры доступа OAuth в формате dnsDomainName\SAMAccountName

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

Для порождения имена групп должны быть возвращены в формате netbiosDomain\samAccountName как роли Утверждение SAML и маркеров Идентификации OIDC:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }
 ```

## <a name="next-steps"></a>Дальнейшие действия

[Что собой представляет гибридная идентификация](whatis-hybrid-identity.md)