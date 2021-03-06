---
title: SAML единого входа для локальных приложений с Azure Active Directory Application Proxy (Предварительная версия) | Документация Майкрософт
description: Узнайте, как обеспечить единый вход для локального приложения, опубликованные через прокси приложения, защищенные с помощью проверки подлинности SAML.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 415b33dce42945c40aedd996d4dcfa5c6b987b44
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58336224"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy-preview"></a>SAML единого входа для локальных приложений с помощью прокси приложения (Предварительная версия)

Можно предоставить единый вход (SSO) для локальных приложений, защищенных с помощью проверки подлинности SAML и обеспечить удаленный доступ к этим приложениям через прокси приложения. С помощью SAML единого входа Azure Active Directory (Azure AD) проходит аутентификацию в приложении с помощью учетной записи пользователя Azure AD. Azure AD передает приложению данные для входа через протокол соединения. Также можно сопоставить пользователей с конкретными ролями приложений на основе правил, определенных в утверждениях SAML. Включив прокси приложения в дополнение к SAML SSO пользователи получают внешний доступ к приложению и возможности единого входа.

Приложения должен иметь возможность использовать маркеры SAML, выданные **Azure Active Directory**. Эта конфигурация не применяется к приложениям с помощью поставщика удостоверений на предприятии. В таких случаях мы рекомендуем отслеживать [ресурсы по переносу приложений в Azure AD](migration-resources.md).

Единый вход SAML с помощью прокси приложения также работает с функцией шифрования маркеров SAML. Дополнительные сведения см. в разделе [шифрования маркеров SAML Azure AD настроить](howto-saml-token-encryption.md).

## <a name="publish-the-on-premises-application-with-application-proxy"></a>Публикация локального приложения с помощью прокси приложения

Прежде чем предоставлять единый вход для локальных приложений, убедитесь, что вы включили прокси приложения и установили соединитель. См. в разделе [добавления локального приложения для удаленного доступа через прокси приложения в Azure AD](application-proxy-add-on-premises-application.md) дополнительные как.

Когда вы будете в учебнике учитывайте следующее:

* Опубликуйте приложение в соответствии с инструкциями в этом руководстве. Не забудьте выбрать **Azure Active Directory** как **предварительная проверка подлинности** метод приложения (шаг 4 [Добавление локального приложения в Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad
)).
* Копировать **внешний URL-адрес** для приложения.
* Рекомендуется используйте пользовательские домены, по возможности для взаимодействия с пользователем оптимизированный. Дополнительные сведения о [работа с пользовательскими доменами в прокси приложения Azure AD](application-proxy-configure-custom-domain.md).
* Добавить по крайней мере одного пользователя в приложение и убедитесь, что тестовой учетной записи имеет доступ к локальному приложению.

## <a name="set-up-saml-sso"></a>Настройка единого входа SAML

1. На портале Azure выберите **Azure Active Directory > корпоративные приложения** и выберите приложение из списка.
1. В приложении **Обзор** выберите **единого входа**.
1. Выберите **SAML** как единственный метод единого входа.
1. В **задать вверх единого входа с помощью SAML** странице, изменить **базовой конфигурации SAML** данных и выполните действия, описанные в [ввод базовой конфигурации SAML](configure-single-sign-on-non-gallery-applications.md#saml-based-single-sign-on) для настройки на основе SAML Проверка подлинности для приложения.

   * Убедитесь, что **URL-адрес ответа** корневой или совпадает с путем под **внешний URL-адрес** для локального приложения, которое было добавлено для удаленного доступа через прокси приложения в Azure AD.

     ![Введите основные данные конфигурации SAML](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)

    > [!NOTE]
    > Если приложение серверной части ожидает **URL-адрес ответа** быть внутренний URL-адрес, необходимо установить Мои приложения безопасного входа в систему расширение на устройствах пользователей. Это расширение будет автоматически перенаправлен на соответствующую службу прокси приложения. Чтобы установить расширение, см. в разделе [вход расширение защищенного Мои приложения](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

## <a name="test-your-app"></a>Тестирование приложения

Когда вы завершите выполнение этих шагов, ваше приложение будет настроено и запущено. Для тестирования приложения:

1. Откройте браузер и перейдите к внешний URL-адрес, созданный при публикации приложения. 
1. Войдите с помощью тестовой учетной записи, назначенной приложению.

## <a name="next-steps"></a>Дальнейшие действия

- [Как прокси приложения Azure AD предоставляет единый вход?](application-proxy-single-sign-on.md)
- [Устранение неполадок прокси-сервера приложений](application-proxy-troubleshoot.md)
