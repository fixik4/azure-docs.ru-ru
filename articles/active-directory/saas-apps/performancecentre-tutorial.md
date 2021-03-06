---
title: Руководство по Интеграция Azure Active Directory с PerformanceCentre | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в PerformanceCentre.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: c5e9ccfa3c179aea190028ba5f97086186b70a02
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407578"
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Руководство по Интеграция Azure Active Directory с PerformanceCentre

В этом руководстве описано, как интегрировать PerformanceCentre с Azure Active Directory (Azure AD).
Интеграция PerformanceCentre с Azure AD обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к PerformanceCentre.
* Вы можете включить автоматический вход пользователей (единый вход) в PerformanceCentre с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с PerformanceCentre, вам потребуется следующее:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка PerformanceCentre с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* PerformanceCentre поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-performancecentre-from-the-gallery"></a>Добавление PerformanceCentre из коллекции

Чтобы настроить интеграцию PerformanceCentre с Azure AD, вам потребуется добавить PerformanceCentre из коллекции в список управляемых приложений SaaS.

**Чтобы добавить PerformanceCentre из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **PerformanceCentre**, выберите **PerformanceCentre** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![PerformanceCentre в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение PerformanceCentre с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в PerformanceCentre.

Чтобы настроить и проверить единый вход в Azure AD в PerformanceCentre, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в PerformanceCentre](#configure-performancecentre-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя PerformanceCentre](#create-performancecentre-test-user)** требуется для того, чтобы в PerformanceCentre существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в PerformanceCentre, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции приложений **PerformanceCentre** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения PerformanceCentre](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `http://<companyname>.performancecentre.com/saml/SSO`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `http://<companyname>.performancecentre.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить их, обратитесь в [службу поддержки клиентов PerformanceCentre](https://www.performancecentre.com/contact-us/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемый URL-адрес можно скопировать из раздела **Настройка PerformanceCentre**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-performancecentre-single-sign-on"></a>Настройка единого входа в PerformanceCentre

1. Войдите на корпоративный сайт **PerformanceCentre** с правами администратора.

2. На вкладке слева щелкните **Configure**(Настройка).
   
    ![единого входа Azure AD][10]

3. На вкладке слева щелкните **Miscellaneous** (Разное), а затем — **Single Sign On** (Единый вход).
   
    ![единого входа Azure AD][11]

4. В поле **Protocol** (Протокол) выберите **SAML**.
   
    ![единого входа Azure AD][12]

5. Откройте скачанный файл метаданных, скопируйте и вставьте его содержимое в текстовое поле **Identity Provider Metadata** (Метаданные поставщика удостоверений), а затем нажмите кнопку **Save** (Сохранить).
   
    ![единого входа Azure AD][13]

6. Проверьте правильность значений в полях **Entity Base URL** (Базовый URL-адрес сущности) и **Entity ID URL** (URL-адрес идентификатора сущности).
    
     ![единого входа Azure AD][14]

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

В этом разделе описано, как предоставить пользователю Britta Simon доступ к PerformanceCentre, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **PerformanceCentre**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **PerformanceCentre**.

    ![Ссылка PerformanceCentre в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-performancecentre-test-user"></a>Создание тестового пользователя в PerformanceCentre

Цель этого раздела — создать пользователя с именем Britta Simon в PerformanceCentre.

**Чтобы создать пользователя с именем Britta Simon в PerformanceCentre, выполните следующие действия.**

1. Войдите на корпоративный сайт PerformanceCentre с правами администратора.

2. В меню слева выберите **Interrelate** (Установить взаимосвязь), а затем нажмите кнопку **Create Participant** (Создать участника).
   
    ![Создать пользователя][400]

3. В диалоговом окне **Interrelate — Create Participant** (Взаимосвязь — создание участника) выполните следующие действия.
   
    ![Создать пользователя][401]
    
    a. Введите необходимые атрибуты для пользователя Britta Simon в соответствующие текстовые поля.
    
    >[!IMPORTANT]
    >Атрибут User Name этого пользователя в PerformanceCentre должен соответствовать имени пользователя в Azure AD.
    
    b. Выберите значение **Client Administrator** (Администратор клиента) в поле **Choose Role** (Выберите роль).
    
    c. Выберите команду **Сохранить**. 

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку PerformanceCentre на Панели доступа, вы автоматически войдете в приложение PerformanceCentre, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[10]: ./media/performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/performancecentre-tutorial/tutorial_performancecentre_10.png
[400]: ./media/performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/performancecentre-tutorial/tutorial_performancecentre_12.png