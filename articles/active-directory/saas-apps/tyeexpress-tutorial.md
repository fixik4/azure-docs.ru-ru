---
title: Руководство по Интеграция Azure Active Directory с T&E Express | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в T&E Express.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 5dc1451d75b14c89fd9fa1742c3e0b078b035d7f
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361852"
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>Руководство по Интеграция Azure Active Directory с T&E Express

В этом руководстве описано, как интегрировать приложение T&E Express с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением T&E Express обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к T&E Express.
* Вы можете включить автоматический вход пользователей в T&E Express (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с T&E Express, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка T&E Express с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* T&E Express поддерживает единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-te-express-from-the-gallery"></a>Добавление T&E Express из коллекции

Чтобы настроить интеграцию T&E Express с Azure AD, необходимо добавить T&E Express из коллекции в список управляемых приложений SaaS.

**Чтобы добавить T&E Express из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **T&E Express**, выберите **T&E Express** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![T&E Express в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в T&E Express с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в T&E Express.

Чтобы настроить и проверить единый вход Azure AD в T&E Express, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в T&E Express](#configure-te-express-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя T&E Express](#create-te-express-test-user)** требуется для создания пользователя Britta Simon в T&E Express, связанного с соответствующим представлением в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в T&E Express, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **T&E Express** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. На странице **Настройка единого входа с помощью SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения T&E Express](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите значение в виде URL-адреса в следующем формате: `https://<domain>.tyeexpress.com`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`.

    > [!NOTE]
    > Эти значения приведены для примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Мы рекомендуем использовать уникальное значение строки идентификатора. Чтобы получить эти значения, обратитесь в [группу поддержки клиентов T&E Express](http://www.tyeexpress.com/contacto.aspx). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Скопируйте требуемый URL-адрес из раздела **Настройка T&E Express**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-te-express-single-sign-on"></a>Настройка единого входа в T&E Express

1. Для настройки единого входа в **T&E Express** войдите в приложение T&E Express без единого входа SAML с использованием учетных данных администратора.

1. На вкладке **Администрирование** щелкните **SAML domain** (Домен SAML), чтобы открыть страницу параметров SAML.

    ![Настройка единого входа](./media/tyeexpress-tutorial/tye-SAML.png)

1. Измените для параметра **Activar(Activate)** (Активировать) значение с **Нет** на **SI(Yes)** (Да). В текстовом поле **Identity Provider Metadata** (Метаданные поставщика удостоверений) вставьте код XML метаданных, скачанный с портала Azure.

    ![Настройка единого входа](./media/tyeexpress-tutorial/tyeAdmin.png)

1. Щелкните кнопку **Guardar(Save)** (Сохранить), чтобы сохранить параметры.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon@yourcompanydomain.extension**.  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к T&E Express, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **T&E Express**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **T&E Express**.

    ![Ссылка на T&E Express в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-te-express-test-user"></a>Создание тестового пользователя T&E Express

Чтобы пользователи Azure AD могли выполнять вход в систему T&E Express, они должны быть подготовлены для нее. В случае T&E Express подготовка выполняется вручную.

**Чтобы подготовить учетные записи пользователей, выполните следующие действия.**

1. Войдите на веб-сайт T&E Express компании в качестве администратора.

1. В разделе "Администрирование" щелкните "Пользователи", чтобы открыть главную страницу пользователей.

    ![Добавление сотрудника](./media/tyeexpress-tutorial/tye-adminusers.png)

1. На домашней странице щелкните **+** для добавления пользователей.

    ![Добавление сотрудника](./media/tyeexpress-tutorial/tye-usershome.png)

1. Введите все обязательные сведения в форме и нажмите кнопку "Сохранить" для сохранения данных.

    ![Добавление сотрудника](./media/tyeexpress-tutorial/tye-usersadd.png)

    ![Добавление сотрудника](./media/tyeexpress-tutorial/tye-userssave.png)

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку T&E Express на панели доступа, вы автоматически войдете в приложение T&E Express, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

