---
title: Руководство по Интеграция Azure Active Directory с ThousandEyes | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в ThousandEyes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d85c4dbd3cc1c3d827a3f8324e63f75eb942425
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "57892988"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Руководство по Интеграция Azure Active Directory с ThousandEyes

В этом руководстве описано, как интегрировать приложение ThousandEyes с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением ThousandEyes обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к ThousandEyes.
- Вы можете включить автоматический вход пользователей в ThousandEyes (единый вход) с применением учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с ThousandEyes, вам потребуется:

- подписка Azure AD;
- подписка ThousandEyes с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по этой ссылке: [пробная версия](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление ThousandEyes из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-thousandeyes-from-the-gallery"></a>Добавление ThousandEyes из коллекции
Чтобы настроить интеграцию ThousandEyes с Azure AD, необходимо добавить ThousandEyes из коллекции в список управляемых приложений SaaS.

**Чтобы добавить ThousandEyes из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **ThousandEyes**.

    ![Создание тестового пользователя Azure AD](./media/thousandeyes-tutorial/tutorial_thousandeyes_search.png)

1. На панели результатов выберите **ThousandEyes** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в ThousandEyes с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в ThousandEyes соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в ThousandEyes.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в ThousandEyes.

Чтобы настроить и проверить единый вход Azure AD в ThousandEyes, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя ThousandEyes](#creating-a-thousandeyes-test-user)** требуется для того, чтобы в ThousandEyes существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении ThousandEyes.

**Чтобы настроить единый вход Azure AD в ThousandEyes, сделайте следующее.**

1. На портале Azure на странице интеграции с приложением **ThousandEyes** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Настройка единого входа](./media/thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

1. В разделе **Домены и URL-адреса приложения ThousandEyes** сделайте следующее.

    ![Настройка единого входа](./media/thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в формате `https://app.thousandeyes.com/login/sso`.

1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/thousandeyes-tutorial/tutorial_thousandeyes_certificate.png)

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/thousandeyes-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация ThousandEyes** щелкните **Настроить ThousandEyes**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

1. В другом окне веб-браузера войдите на веб-сайт **ThousandEyes** своей компании в качестве администратора.

1. В верхнем меню нажмите пункт **Параметры**.

    ![Параметры](./media/thousandeyes-tutorial/ic790066.png "Параметры")

1. В нижней части страницы нажмите кнопку **Учетная запись**

    ![Учетная запись](./media/thousandeyes-tutorial/ic790067.png "Учетная запись")

1. Откройте вкладку **Security & Authentication** (Безопасность и проверка подлинности).

    ![Безопасность и проверка подлинности](./media/thousandeyes-tutorial/ic790068.png "Безопасность и проверка подлинности")

1. В разделе **Настройка единого входа** сделайте следующее:

    ![Настройка единого входа](./media/thousandeyes-tutorial/ic790069.png "Настройка единого входа")

    a. Выберите пункт **Включить единый вход**.

    2. В текстовое поле **Login Page URL** (URL-адрес страницы входа) вставьте значение **SAML Single Sign-On Service URL** (URL-адрес службы единого входа SAML), скопированное на портале Azure.

    c. В текстовое поле **Logout Page URL** (URL-адрес выхода) вставьте значение **Sign-Out URL** (URL-адрес выхода), скопированное на портале Azure.

    d. В текстовое поле **Identity Provider Issuer** (Издатель поставщика удостоверений) вставьте значение **SAML Entity ID** (Идентификатор сущности SAML), скопированное на портале Azure.

    д. В разделе **Verification Certificate** (Сертификат проверки) нажмите кнопку **Choose file** (Выбрать файл) и передайте сертификат, скачанный с портала Azure.

    Е. Выберите команду **Сохранить**.

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/thousandeyes-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/thousandeyes-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.

    ![Создание тестового пользователя Azure AD](./media/thousandeyes-tutorial/create_aaduser_03.png)

1. На странице диалогового окна **Пользователь** выполните следующие действия.

    ![Создание тестового пользователя Azure AD](./media/thousandeyes-tutorial/create_aaduser_04.png)

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.

### <a name="creating-a-thousandeyes-test-user"></a>Создание тестового пользователя ThousandEyes

В рамках этого раздела создается пользователь с именем Britta Simon в ThousandEyes. ThousandEyes поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](thousandeyes-provisioning-tutorial.md).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

1. Войдите на свой корпоративный веб-сайт ThousandEyes в качестве администратора.

1. Щелкните **Параметры**.

    ![Параметры](./media/thousandeyes-tutorial/IC790066.png "Параметры")

1. Выберите раздел **Учетная запись**.

    ![Учетная запись](./media/thousandeyes-tutorial/IC790067.png "Учетная запись")

1. Щелкните вкладку **Accounts & Users** (Учетные записи и пользователи).

    ![Учетные записи и пользователи](./media/thousandeyes-tutorial/IC790073.png "Учетные записи и пользователи")

1. В разделе **Add Users & Accounts** (Добавление пользователей и учетных записей) сделайте следующее.

    ![Добавление учетных записей пользователей](./media/thousandeyes-tutorial/IC790074.png "Добавление учетных записей пользователей")

    a. В текстовое поле **Name** (Имя) введите имя пользователя, например **Britta Simon**.

    2. В **электронной почты** текстовом поле введите адрес электронной почты, например **brittasimon\@contoso.com**.

    2. Щелкните **Добавить нового пользователя к учетной записи**.

    > [!NOTE]
    > Владелец учетной записи Azure Active Directory получит электронное сообщение со ссылкой для подтверждения и активации учетной записи.

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя ThousandEyes или API, предоставляемые ThousandEyes для подготовки учетных записей пользователя Azure Active Directory.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к ThousandEyes.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в ThousandEyes, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **ThousandEyes**.

    ![Настройка единого входа](./media/thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "ThousandEyes" на панели доступа, вы автоматически войдете в приложение ThousandEyes.

См. дополнительные сведения о [панели доступа](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Руководство по настройке Google Apps для автоматической подготовки пользователей](thousandeyes-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/thousandeyes-tutorial/tutorial_general_203.png
