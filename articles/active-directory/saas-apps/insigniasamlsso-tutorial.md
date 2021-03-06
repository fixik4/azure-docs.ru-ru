---
title: Руководство. Интеграция Azure Active Directory с Insignia SAML SSO | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Insignia SAML SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 828c981c-c3dd-4eb2-8699-0f732baa43f6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc492142200c7fd028360d01c2602904531550dc
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884488"
---
# <a name="tutorial-azure-active-directory-integration-with-insignia-saml-sso"></a>Руководство. Интеграция Azure Active Directory с Insignia SAML SSO

В этом руководстве описано, как интегрировать Insignia SAML SSO с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Insignia SAML SSO обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Insignia SAML SSO.
- Вы можете включить автоматический вход пользователей в Insignia SAML SSO (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с Insignia SAML SSO, вам потребуется:

- подписка Azure AD;
- подписка Insignia SAML SSO с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Insignia SAML SSO из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-insignia-saml-sso-from-the-gallery"></a>Добавление Insignia SAML SSO из коллекции
Чтобы настроить интеграцию Insignia SAML SSO с Azure AD, необходимо добавить Insignia SAML SSO из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Insignia SAML SSO из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

1. В поле поиска введите **Insignia SAML SSO**, выберите **Insignia SAML SSO** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Insignia SAML SSO в списке результатов](./media/insigniasamlsso-tutorial/tutorial_insigniasamlsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Insignia SAML SSO с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Insignia SAML SSO соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Insignia SAML SSO.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Insignia SAML SSO.

Чтобы настроить и проверить единый вход Azure AD в Insignia SAML SSO, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Insignia SAML SSO](#create-an-insignia-saml-sso-test-user)** требуется для того, чтобы в Insignia SAML SSO существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В данном разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Insignia SAML SSO.

**Чтобы настроить Azure AD единого входа в Insignia SAML SSO, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Insignia SAML SSO** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/insigniasamlsso-tutorial/tutorial_insigniasamlsso_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Insignia SAML SSO** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Insignia SAML SSO](./media/insigniasamlsso-tutorial/tutorial_insigniasamlsso_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате:
    
    | |
    |--|
    | `https://<customername>.insigniails.com/ils` |
    | `https://<customername>.insigniails.com/` |
    | `https://<customername>.insigniailsusa.com/` |

    2. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<customername>.insigniailsusa.com/<uniqueid>`

    > [!NOTE] 
    > Эти значения приведены для примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов Insignia SAML SSO](https://www.insigniasoftware.com/insignia/Techsupport.aspx). 
 

1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Ссылка для скачивания сертификата](./media/insigniasamlsso-tutorial/tutorial_insigniasamlsso_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/insigniasamlsso-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация Insignia SAML SSO** щелкните **Настроить Insignia SAML SSO**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода и URL-адрес службы единого входа SAML** из раздела **Quick Reference** (Краткий справочник).

    ![Конфигурация Insignia SAML SSO](./media/insigniasamlsso-tutorial/tutorial_insigniasamlsso_configure.png) 

1. Чтобы настроить единый вход на стороне **Insignia SAML SSO**, нужно отправить скачанный **сертификат (Base64)**, **URL-адрес выхода и URL-адрес службы единого входа SAML** [группе поддержки Insignia SAML SSO](https://www.insigniasoftware.com/insignia/Techsupport.aspx). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия.**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/insigniasamlsso-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/insigniasamlsso-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/insigniasamlsso-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/insigniasamlsso-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    2. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-an-insignia-saml-sso-test-user"></a>Создание тестового пользователя Insignia SAML SSO

В этом разделе описано, как создать пользователя Britta Simon в Insignia Library System. Обратитесь к [группе поддержки Insignia Library System](https://www.insigniasoftware.com/insignia/Techsupport.aspx), чтобы добавить пользователей на платформу Insignia Library System.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Insignia SAML SSO.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Insignia SAML SSO, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **Insignia SAML SSO**.

    ![Ссылка на Insignia SAML SSO в списке приложений](./media/insigniasamlsso-tutorial/tutorial_insigniasamlsso_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Insignia SAML SSO" на панели доступа, вы автоматически войдете в приложение Insignia SAML SSO.
Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/insigniasamlsso-tutorial/tutorial_general_01.png
[2]: ./media/insigniasamlsso-tutorial/tutorial_general_02.png
[3]: ./media/insigniasamlsso-tutorial/tutorial_general_03.png
[4]: ./media/insigniasamlsso-tutorial/tutorial_general_04.png

[100]: ./media/insigniasamlsso-tutorial/tutorial_general_100.png

[200]: ./media/insigniasamlsso-tutorial/tutorial_general_200.png
[201]: ./media/insigniasamlsso-tutorial/tutorial_general_201.png
[202]: ./media/insigniasamlsso-tutorial/tutorial_general_202.png
[203]: ./media/insigniasamlsso-tutorial/tutorial_general_203.png

