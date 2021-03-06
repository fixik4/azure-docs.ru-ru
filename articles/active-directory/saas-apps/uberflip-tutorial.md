---
title: Руководство. Интеграция Azure Active Directory с Uberflip | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Uberflip.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 754b1f5b-6694-4fd6-9e1e-9fad769c64db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28add36dedcd942150a96d32508246ee03da60f1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182584"
---
# <a name="tutorial-azure-active-directory-integration-with-uberflip"></a>Руководство по Интеграция Azure Active Directory с Uberflip

В этом руководстве описано, как интегрировать Uberflip с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Uberflip обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к приложению Uberflip.
- Вы можете включить автоматический вход пользователей в Uberflip (единый вход) с использованием их учетных записей Azure AD.
- Вы можете управлять учетными записями централизованно на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Uberflip, вам потребуется:

- подписка Azure AD;
- подписка Uberflip с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. добавление Uberflip из коллекции;
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-uberflip-from-the-gallery"></a>добавление Uberflip из коллекции;

Чтобы настроить интеграцию Uberflip с Azure AD, необходимо добавить Uberflip из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Uberflip из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

4. В поле поиска введите **Uberflip**, выберите **Uberflip** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Uberflip в списке результатов](./media/uberflip-tutorial/tutorial_uberflip_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Uberflip с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в Uberflip соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Uberflip.

Чтобы настроить и проверить единый вход Azure AD в Uberflip, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создайте тестового пользователя в Uberflip](#create-an-uberflip-test-user)**. Это нужно для того, чтобы в Uberflip существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Uberflip.

**Чтобы настроить единый вход Azure AD в Uberflip, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Uberflip** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Диалоговое окно "Единый вход"](./media/uberflip-tutorial/tutorial_uberflip_samlbase.png)

3. Если вы хотите настроить приложение в режиме, инициированном **поставщиком удостоверений**, в разделе **Домены и URL-адреса приложения Uberflip** выполните следующие действия:

    ![Сведения о домене и URL-адресах для единого входа в приложение Uberflip](./media/uberflip-tutorial/tutorial_uberflip_url1.png)

    В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://app.uberflip.com/sso/saml2/<IDPID>/<ACCOUNTID>`.

    > [!NOTE]
    > Это значение приведено для справки. Вместо него нужно указать фактический URL-адрес ответа. Для получения этого значения обратитесь в [группу поддержки клиентов Uberflip](mailto:support@uberflip.com).

4. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах для единого входа в приложение Uberflip](./media/uberflip-tutorial/tutorial_uberflip_url2.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес: `https://app.uberflip.com/users/login`

5. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/uberflip-tutorial/tutorial_uberflip_certificate.png) 

6. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/uberflip-tutorial/tutorial_general_400.png)

7. Чтобы настроить единый вход на стороне **Uberflip**, отправьте в [группу поддержки Uberflip](mailto:support@uberflip.com) скачанный **XML-файл метаданных**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/uberflip-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/uberflip-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/uberflip-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/uberflip-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    4.3. Нажмите кнопку **Создать**.

### <a name="create-an-uberflip-test-user"></a>Создание тестового пользователя в Uberflip

Цель этого раздела — создать пользователя с именем Britta Simon в Uberflip. Приложение Uberflip поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Новый пользователь будет создан при попытке получить доступ к Uberflip (если он еще не создан).

> [!Note]
> Чтобы создать пользователя вручную, обратитесь к [группе поддержки Uberflip](mailto:support@uberflip.com).

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Uberflip, чтобы он мог использовать единый вход Azure.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Uberflip, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **Uberflip**.

    ![Ссылка на Uberflip в списке "Приложения"](./media/uberflip-tutorial/tutorial_uberflip_app.png)  

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Uberflip на панели доступа, вы автоматически войдете в приложение Uberflip.
См. дополнительные сведения о [панели доступа](../user-help/active-directory-saas-access-panel-introduction.md) 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/uberflip-tutorial/tutorial_general_01.png
[2]: ./media/uberflip-tutorial/tutorial_general_02.png
[3]: ./media/uberflip-tutorial/tutorial_general_03.png
[4]: ./media/uberflip-tutorial/tutorial_general_04.png

[100]: ./media/uberflip-tutorial/tutorial_general_100.png

[200]: ./media/uberflip-tutorial/tutorial_general_200.png
[201]: ./media/uberflip-tutorial/tutorial_general_201.png
[202]: ./media/uberflip-tutorial/tutorial_general_202.png
[203]: ./media/uberflip-tutorial/tutorial_general_203.png
