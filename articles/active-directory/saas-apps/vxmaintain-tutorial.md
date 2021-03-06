---
title: Руководство по Интеграция Azure Active Directory с vxMaintain | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и vxMaintain.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d0e8f8526d866c308be8684546397f282dcce51
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194110"
---
# <a name="tutorial-azure-active-directory-integration-with-vxmaintain"></a>Руководство. Интеграция Azure Active Directory с vxMaintain

В этом учебнике описано, как интегрировать vxMaintain с Azure Active Directory (Azure AD).

Такая интеграция обеспечивает несколько важных преимуществ. Вы можете:

- С помощью Azure AD вы можете контролировать доступ к vxMaintain.
- Вы можете включить автоматический вход пользователей в vxMaintain (единый вход) с учетной записью Azure AD.
- Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с vxMaintain, вам потребуется:

- подписка Azure AD;
- подписка vxMaintain с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для тестирования действий, выполняемых в этом руководстве.

При проверке действий в этом руководстве соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. 

Сценарий, описанный в этом руководстве, состоит из двух основных стандартных блоков:

* Добавление vxMaintain из коллекции
* настройка и проверка единого входа в Azure AD.

## <a name="add-vxmaintain-from-the-gallery"></a>Добавление vxMaintain из коллекции
Чтобы настроить интеграцию vxMaintain с Azure AD, необходимо добавить vxMaintain из коллекции в список управляемых приложений SaaS.

Чтобы добавить vxMaintain из коллекции, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com) в области слева нажмите кнопку **Azure Active Directory**. 

    ![Кнопка Azure Active Directory][1]

1. Щелкните **Корпоративные приложения** > **Все приложения**.

    ![Область "Корпоративные приложения"][2]
    
1. Чтобы добавить приложение, в верхней части диалогового окна **Все приложения** нажмите кнопку **Новое приложение**.

    ![Кнопка "Новое приложение"][3]

1. В поле поиска введите **vxMaintain**.

    ![Раскрывающийся список "Режим единого входа"](./media/vxmaintain-tutorial/tutorial_vxmaintain_search.png)

1. Из списка результатов выберите **vxMaintain** и нажмите кнопку **Добавить**.

    ![Ссылка на vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описана настройка и проверка единого входа Azure AD в vxMaintain с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, в Azure AD необходимо указать, какой пользователь в vxMaintain соответствует пользователю в Azure AD. То есть необходимо установить связь между пользователем Azure AD и соответствующим пользователем vxMaintain.

Чтобы установить эту связь, присвойте **имени пользователя** в Azure AD значение **имени пользователя** в vxMaintain.

Чтобы настроить и проверить единый вход Azure AD в vxMaintain, выполните действия в следующих стандартных блоках.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

В этом разделе описывается, как включить единый вход Azure AD на портале Azure и настроить единый вход в приложении vxMaintain. Для этого выполните следующее.

1. На портале Azure на странице интеграции с приложением **vxMaintain** щелкните **Единый вход**.

    ![Команда "Единый вход"][4]

1. Чтобы включить функцию единого входа, из раскрывающегося списка **Режим единого входа** выберите параметр **Вход на основе SAML**.
 
    ![Параметр "Вход на основе SAML"](./media/vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

1. В разделе **Домены и URL-адреса приложения vxMaintain** выполните следующие действия.

    ![Раздел "Домены и URL-адреса приложения vxMaintain"](./media/vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. В поле **Идентификатор** введите URL-адрес, используя следующий синтаксис: `https://<company name>.verisae.com`.

    б) В поле **URL-адрес ответа** введите URL-адрес, используя следующий синтаксис: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`.

    > [!NOTE] 
    > Приведенные выше значения используются только для примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь к [группе поддержки vxMaintain](https://www.hubspot.com/company/contact).
 
1. В разделе **Сертификат подписи SAML** щелкните **XML метаданных** и сохраните файл метаданных на компьютере.

    ![Раздел "Сертификат подписи SAML"](./media/vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

1. Щелкните **Сохранить**.

    ![Кнопка "Сохранить"](./media/vxmaintain-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход **vxMaintain**, отправьте скачанный **XML-файл метаданных** [группе поддержки vxMaintain](https://www.hubspot.com/company/contact).

> [!TIP]
> Настроив приложение, вы можете прочитать краткую версию предыдущих инструкций на [портале Azure](https://portal.azure.com). После добавления приложения из раздела **Active Directory** > **Корпоративные приложения** просто выберите вкладку **Единый вход** и ознакомьтесь со встроенной документацией, воспользовавшись разделом **Настройка**. 
>
>Чтобы узнать больше о встроенной документации, ознакомьтесь с разделом [Управление параметрами единого входа для корпоративных приложений](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
В этом разделе вы создадите на портале Azure тестового пользователя Britta Simon, выполнив следующие действия.

![Тестовый пользователь Azure AD][100]

1. На **портале Azure** в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/vxmaintain-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, выберите **Пользователи и группы** > **Все пользователи**.
    
    ![Ссылка "Все пользователи"](./media/vxmaintain-tutorial/create_aaduser_02.png)  
    Откроется диалоговое окно **Все пользователи**. 

1. Щелкните **Добавить**, чтобы открыть диалоговое окно **Пользователь**.
 
    ![Кнопка "Добавить"](./media/vxmaintain-tutorial/create_aaduser_03.png) 

1. В диалоговом окне **Пользователь** сделайте следующее.
 
    ![Диалоговое окно "Пользователь"](./media/vxmaintain-tutorial/create_aaduser_04.png) 

    a. В поле **Имя** введите **BrittaSimon**.

    б) В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, созданное в поле **Пароль**.

    4.3. Нажмите кнопку **Создать**.
 
### <a name="create-a-vxmaintain-test-user"></a>Создание тестового пользователя vxMaintain

В этом разделе вы создадите тестового пользователя Britta Simon в vxMaintain. Чтобы добавить пользователей на платформу vxMaintain, обратитесь к [группе поддержки vxMaintain](https://www.hubspot.com/company/contact). Прежде чем использовать единый вход, создайте и активируйте пользователей.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к vxMaintain. Для этого выполните следующее.

![Тестовый пользователь в списке "Отображаемое имя"][200] 

1. На портале Azure откройте представление **Приложения** и выберите представление **Каталог**. Щелкните **Корпоративные приложения** > **Все приложения**.

    ![Ссылка "Все приложения"][201] 

1. Из списка **Приложения** выберите **vxMaintain**.

    ![Ссылка на vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

1. В области слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202] 

1. Нажмите кнопку **Добавить**, затем в области **Добавление назначения** щелкните **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][203]

1. В диалоговом окне **Пользователи и группы** из списка **Пользователи** выберите **Britta Simon**. Нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** выберите **Назначить**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Проверка единого входа Azure AD

В этом разделе вы с помощью панели доступа выполните проверку конфигурации единого входа Azure AD.

Щелкнув элемент **vxMaintain** на панели доступа, вы автоматически войдете в приложение vxMaintain.

Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Дополнительная информация

* [Список руководств по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/vxmaintain-tutorial/tutorial_general_203.png

