---
title: Руководство. Интеграция Azure Active Directory с Predictix Ordering | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Predictix Ordering.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2fe2f976-e97f-4368-9695-3e1624409e8b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95f8ca4332fb3bae1ced01111cd33c5890848b2b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162430"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a>Руководство. Интеграция Azure Active Directory с Predictix Ordering

В этом руководстве описано, как интегрировать Predictix Ordering с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Predictix Ordering обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Predictix Ordering.
- Вы можете включить автоматический вход пользователей в Predictix Ordering (единый вход) с использованием учетных записей Azure AD.
- Вы можете управлять учетными записями централизованно на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Predictix Ordering, вам потребуется:

- подписка Azure AD;
- подписка Predictix Ordering с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление приложения Predictix Ordering из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-predictix-ordering-from-the-gallery"></a>Добавление приложения Predictix Ordering из коллекции
Чтобы настроить интеграцию Predictix Ordering с Azure AD, необходимо добавить приложение Predictix Ordering из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Predictix Ordering из коллекции, выполните указанные ниже действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

1. В поле поиска введите **Predictix Ordering**, выберите **Predictix Ordering** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Predictix Ordering в списке результатов](./media/predictixordering-tutorial/tutorial_predictixordering_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Predictix Ordering с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Predictix Ordering соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Predictix Ordering.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Predictix Ordering.

Чтобы настроить и проверить единый вход Azure AD в Predictix Ordering, выполните следующие стандартные действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Predictix Ordering](#create-a-predictix-ordering-test-user)** требуется для создания в Predictix Ordering пользователя Britta Simon, связанного с соответствующим представлением в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Predictix Ordering.

**Чтобы настроить единый вход Azure AD в Predictix Ordering, выполните указанные ниже действия.**

1. На портале Azure на странице интеграции с приложением **Predictix Ordering** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/predictixordering-tutorial/tutorial_predictixordering_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Predictix Ordering** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Predictix Ordering](./media/predictixordering-tutorial/tutorial_predictixordering_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname-pricing>.ordering.predictix.com/sso/request`

    б) В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: 
    
    | |
    |--|
    | `https://<companyname-pricing>.dev.ordering.predictix.com` |
    | `https://<companyname-pricing>.ordering.predictix.com` |

    > [!NOTE] 
    > Эти значения приведены для примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Predictix Ordering](https://www.predix.io/support/). 
 
1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Ссылка для скачивания сертификата](./media/predictixordering-tutorial/tutorial_predictixordering_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/predictixordering-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация Predictix Ordering** щелкните **Настроить Predictix Ordering**, чтобы открыть окно **Настройка входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка Predictix Ordering](./media/predictixordering-tutorial/tutorial_predictixordering_configure.png) 

1. Чтобы настроить единый вход на стороне **Predictix Ordering**, нужно передать скачанный **сертификат (Base64)**, **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** в [службу поддержки Predictix Ordering](https://www.predix.io/support/). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/predictixordering-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/predictixordering-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/predictixordering-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/predictixordering-tutorial/create_aaduser_04.png)

   a. В поле **Имя** введите **BrittaSimon**.

   Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

   c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

   4.3. Нажмите кнопку **Создать**.
 
### <a name="create-a-predictix-ordering-test-user"></a>Создание тестового пользователя в Predictix Ordering

В этом разделе описано, как создать пользователя Britta Simon в приложении Predictix Ordering. Обратитесь в [службу поддержки Predictix Ordering](https://www.predix.io/support/), чтобы добавить пользователей на платформу Predictix Ordering.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к Predictix Ordering.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению Predictix Ordering, выполните указанные ниже действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Predictix Ordering**.

    ![Ссылка на Predictix Ordering в списке "Приложения"](./media/predictixordering-tutorial/tutorial_predictixordering_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Predictix Ordering на панели доступа, вы автоматически войдете в приложение Predictix Ordering.


## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/predictixordering-tutorial/tutorial_general_01.png
[2]: ./media/predictixordering-tutorial/tutorial_general_02.png
[3]: ./media/predictixordering-tutorial/tutorial_general_03.png
[4]: ./media/predictixordering-tutorial/tutorial_general_04.png

[100]: ./media/predictixordering-tutorial/tutorial_general_100.png

[200]: ./media/predictixordering-tutorial/tutorial_general_200.png
[201]: ./media/predictixordering-tutorial/tutorial_general_201.png
[202]: ./media/predictixordering-tutorial/tutorial_general_202.png
[203]: ./media/predictixordering-tutorial/tutorial_general_203.png

