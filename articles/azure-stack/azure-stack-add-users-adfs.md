---
title: Добавление пользователей для работы с ADFS в Azure Stack | Документация Майкрософт
description: Сведения о добавлении пользователей для работы с развертываниями Azure Stack с помощью служб федерации Active Directory
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2019
ms.author: patricka
ms.reviewer: unknown
ms.lastreviewed: 02/21/2019
ms.openlocfilehash: 1b47739200c79317273ea0c788f21a7ee4a3b818
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56648512"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Добавление пользователей Azure Stack в AD FS
Оснастку **Пользователи и компьютеры Active Directory** можно использовать для добавления дополнительных пользователей в среду Azure Stack, используя AD FS в качестве поставщика удостоверений.

## <a name="add-windows-server-active-directory-users"></a>Добавление пользователей Windows Server Active Directory
> [!TIP]
> В этом примере используется пакет по умолчанию azurestack.local ASDK для Active Directory. 

1. Войдите в компьютер с учетной записью, предоставляющей доступ к инструментам администрирования Windows, и откройте новую консоль управления Microsoft (MMC).
2. Щелкните **Файл > Add or remove snap-in** (Добавить или удалить оснастку).
3. Выберите **Active Directory — пользователи и компьютеры** > **AzureStack.local** > **Пользователи**.
4. Выберите **Действия** > **Создать** > **Пользователь**.
5. В окне "Создать объект — пользователь" укажите сведения о пользователе. Щелкните **Далее**.
6. Укажите и подтвердите пароль.
7. Нажмите кнопку **Далее**, чтобы подтвердить значения. Выберите **Готово**, чтобы создать пользователя.


## <a name="next-steps"></a>Дополнительная информация
[Создание субъектов-служб](azure-stack-create-service-principals.md)