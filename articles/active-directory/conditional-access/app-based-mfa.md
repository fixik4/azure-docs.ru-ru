---
title: Краткое руководство. Требование многофакторной идентификации (MFA) для конкретных приложений с помощью условного доступа Azure Active Directory | Документы Майкрософт
description: В этом кратком руководстве вы узнаете, как привязать требования аутентификации к типу открываемого облачного приложения с помощью условного доступа Azure Active Directory (Azure AD).
services: active-directory
keywords: условный доступ к приложениям, условный доступ посредством Azure Active Directory, безопасный доступ к ресурсам организации, политики условного доступа
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/30/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: cbcb0271a1bd80f0f7155c379de7b5149c76fcca
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520434"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Краткое руководство. Требование Многофакторной идентификации для конкретных приложений с помощью условного доступа Azure Active Directory 

Чтобы упростить процесс входа пользователей, им можно разрешить выполнять вход в облачные приложения с помощью имени пользователя и пароля. Однако во многих средах существует несколько приложений, для которых целесообразно требовать более строгую форму проверки учетной записи, такую как многофакторная идентификация (MFA). Этот вид проверки подойдет, к примеру, для доступа к системе электронной почты или приложениям отдела кадров в организации. В Azure Active Directory (Azure AD) это можно сделать с помощью политики условного доступа.    

В этом кратком руководстве содержатся инструкции по настройке [политики условного доступа Azure AD](../active-directory-conditional-access-azure-portal.md), которая требует многофакторную идентификацию для ряда приложений в существующей среде.

![Создание политики](./media/app-based-mfa/32.png)


Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.



## <a name="prerequisites"></a>Технические условия 

Чтобы выполнить сценарий в этом кратком руководстве, вам понадобится:

- **доступ к Azure AD Premium** — условный доступ Azure AD является возможностью Azure AD Premium; 

- **тестовая учетная запись Isabella Simonsen** — сведения о создании тестовой учетной записи см. в разделе о [добавлении облачных пользователей](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).


Сценарий этого краткого руководства предполагает, что многофакторная идентификация каждого отдельного пользователя отключена в тестовой учетной записи. Дополнительные сведения см. в статье [Включение двухфакторной проверки подлинности пользователя](../authentication/howto-mfa-userstates.md).


## <a name="test-your-sign-in"></a>Проверка входа

Целью этого шага является ознакомление со входом в систему без политики условного доступа.

**Инициализация среды:**

1. Войдите на портал Azure с учетной записью Isabella Simonsen.

2. Выполните выход.


## <a name="create-your-conditional-access-policy"></a>Создание политики условного доступа 

В этом разделе показано, как создать необходимую политику условного доступа. В сценарии в этом кратком руководстве используются следующие компоненты:

- портал Azure в качестве заполнителя облачного приложения, которому требуется многофакторная идентификация; 
- пример пользователя для проверки политики условного доступа.  

В политике задайте следующие значения.

|Параметр |Значение|
|---     | --- |
|Пользователи и группы | Isabella Simonsen |
|Облачные приложения | Управление Microsoft Azure |
|Предоставление доступа | Требование многофакторной идентификации |
 

![Создание политики](./media/app-based-mfa/31.png)

 


**Чтобы настроить политику условного доступа, выполните следующие действия.**

1. Войдите на [портал Azure](https://portal.azure.com) с правами глобального администратора, администратора безопасности или администратора условного доступа.

2. На портале Azure на панели навигации слева щелкните **Azure Active Directory**. 

    ![Azure Active Directory](./media/app-based-mfa/02.png)

3. На странице **Azure Active Directory** в разделе **Безопасность** щелкните **Условный доступ**.

    ![Условный доступ](./media/app-based-mfa/03.png)
 
4. На странице **Условный доступ** на панели инструментов сверху нажмите кнопку **Создать политику**.

    ![Добавить](./media/app-based-mfa/04.png)

5. На странице **Создание** в поле **Имя** введите **Require MFA for Azure portal access** (Требовать многофакторную идентификацию для доступа к порталу Azure).

    ![ИМЯ](./media/app-based-mfa/05.png)

6. В разделе **Назначение** щелкните **Пользователи и группы**.

    ![Пользователи и группы](./media/app-based-mfa/06.png)

7. На странице **Пользователи и группы** выполните следующие действия:

    ![Пользователи и группы](./media/app-based-mfa/24.png)

    a. Щелкните **Выбор пользователей и групп**, а затем выберите **Пользователи и группы**.

    2. Нажмите кнопку **Выбрать**.

    c. На странице **Выбор** выберите **Isabella Simonsen**, а затем нажмите кнопку **Выбрать**.

    d. На странице **Пользователи и группы** нажмите кнопку **Готово**.

8. Щелкните **Облачные приложения**.

    ![Облачные приложения](./media/app-based-mfa/08.png)

9. На странице **Облачные приложения** выполните следующие действия.

    ![Выбор облачных приложений](./media/app-based-mfa/26.png)

    a. Щелкните **Выбрать приложения**.

    2. Нажмите кнопку **Выбрать**.

    c. На странице **Выбор** выберите **Управление Microsoft Azure**, а затем нажмите кнопку **Выбрать**.

    d. На странице **Облачные приложения** нажмите кнопку **Готово**.


10. В разделе **Элементы управления доступом** щелкните **Предоставить**.

    ![Элементы управления доступом](./media/app-based-mfa/10.png)

11. На странице **Предоставить** выполните следующие действия:

    ![Предоставление доступа](./media/app-based-mfa/11.png)

    a. Выберите **Предоставить доступ**.

    a. Выберите **Требовать многофакторную проверку подлинности**.

    2. Нажмите кнопку **Выбрать**.

12. В разделе **Включение политики** щелкните **Вкл**.

    ![Включение политики](./media/app-based-mfa/18.png)

13. Нажмите кнопку **Создать**.


## <a name="evaluate-a-simulated-sign-in"></a>Оценка смоделированного входа

Теперь, когда вы настроили политику условного доступа, вероятно, вы захотите узнать, работает ли она так, как ожидалось. В качестве первого шага используйте инструмент What If для политик условного доступа, чтобы смоделировать вход тестового пользователя. При имитации оценивается влияние входа на политики и создается отчет об имитации.  

Для инициализации инструмента оценки What If задайте следующие значения:

- **Isabella Simonsen** в качестве пользователя; 
- **Управление Microsoft Azure** в качестве облачного приложения.

  После нажатия кнопки **What If** создается отчет о моделировании, содержащий следующие данные.

- **Require MFA for Azure portal access** (Требовать многофакторную идентификацию для доступа к порталу Azure) в разделе **Policies that will apply** (Применяемые политики) 
- **Require multi-factor authentication** (Требовать многофакторную идентификацию) в разделе **Grant Controls** (Элементы управления для предоставления).

![Инструмент политики What If](./media/app-based-mfa/23.png)



**Чтобы оценить политику условного доступа, выполните следующие действия.**

1. На странице [Условный доступ — политики](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) в меню в верхней части нажмите кнопку **What If**.  
 
    ![What If](./media/app-based-mfa/14.png)

2. Щелкните **Пользователи**, выберите **Isabella Simonsen**, а затем нажмите кнопку **Выбрать**.

    ![Пользователь](./media/app-based-mfa/15.png)

2. Чтобы выбрать облачное приложение, выполните следующие действия.

    ![Облачные приложения](./media/app-based-mfa/16.png)

    a. Щелкните **Облачные приложения**.

    2. На странице **Облачные приложения** щелкните **Выбрать приложения**.

    c. Нажмите кнопку **Выбрать**.

    d. На странице **Выбор** выберите **Управление Microsoft Azure**, а затем нажмите кнопку **Выбрать**.

    д. На странице "Облачные приложения" нажмите кнопку **Готово**.

3. Щелкните **What If**.


## <a name="test-your-conditional-access-policy"></a>Проверка политики условного доступа

В предыдущем разделе вы узнали, как оценить смоделированный вход. Помимо имитации необходимо проверить политику условного доступа, чтобы убедиться в правильности ее работы. 

Чтобы проверить политику, попробуйте войти на [портал Azure](https://portal.azure.com) с помощью тестовой учетной записи **Isabella Simonsen**. Должно появиться диалоговое окно, требующее выполнить настройку учетной записи для дополнительной проверки безопасности.

![Многофакторная Идентификация](./media/app-based-mfa/22.png)


## <a name="clean-up-resources"></a>Очистка ресурсов

Если тестовый пользователь и политика условного доступа больше не нужны, удалите их.

- Сведения об удалении пользователя Azure AD см. в разделе об [удалении пользователей из Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- Чтобы удалить политику, выберите ее и нажмите кнопку **Удалить** на панели инструментов быстрого доступа.

    ![Многофакторная Идентификация](./media/app-based-mfa/33.png)


## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Запрос на принятие условий использования](require-tou.md)
> [Блокировка доступа при обнаружении риска для сеанса](app-sign-in-risk.md)
