---
title: Синхронизация хэшей паролей с Azure Active Directory | Документация Майкрософт
description: Синхронизация хэша паролей.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f588115e38ded07b79568c91b8d166949b34a886
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56179184"
---
# <a name="what-is-password-hash-synchronization-with-azure-ad"></a>Синхронизация хэша паролей с Azure Active Directory
Синхронизация хэша паролей является одним из методов входа, используемым для реализации гибридной идентификации. Azure AD Connect синхронизирует хэш пользовательских паролей между локальным экземпляром службы Active Directory и облачным экземпляром службы Azure AD.

Синхронизация хэшированных паролей — это расширение функции синхронизации каталогов, реализованной в службе синхронизации Azure AD Connect. Эту функцию можно использовать для входа в службу Azure AD, например Office 365. Вы можете входить в службу с помощью пароля, который используется на локальном экземпляре Active Directory.

![Что такое Azure AD Connect?](./media/how-to-connect-password-hash-synchronization/arch1.png)

За счет сокращения числа паролей, которые должны помнить пользователи, до одного, синхронизация паролей дает следующие преимущества: Синхронизация хэша паролей обеспечивает:

* Повышение производительности пользователей.
* Снижение затрат на техническую поддержку.  

При необходимости можно настроить синхронизацию хэша паролей в качестве резервного решения, если вы хотите использовать [федерацию со службами федерации Active Directory (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) в качестве метода входа.

Чтобы применить синхронизацию хэшированных паролей в своей среде выполните следующие действия.

* Установите Azure AD Connect.  
* Настройте синхронизацию каталогов между локальным экземпляром службы Active Directory и облачным экземпляром Azure Active Directory.
* Включите синхронизацию хэшированных паролей.



Дополнительные сведения см. в руководстве по [гибридной идентификации](whatis-hybrid-identity.md).




## <a name="next-steps"></a>Дальнейшие действия

- [Что собой представляет гибридная идентификация](whatis-phs.md)
- [Использование Azure AD Connect с расширением Azure AD Connect Health](whatis-azure-ad-connect.md)
- [Использование сквозной проверки подлинности](how-to-connect-pta.md)
- [Использование федерации](whatis-fed.md)
- [Использование единого входа](how-to-connect-sso.md)
- [Принцип работы синхронизации хэшей паролей](how-to-connect-password-hash-synchronization.md)
