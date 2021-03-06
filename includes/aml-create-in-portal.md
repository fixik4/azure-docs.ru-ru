---
title: включение файла
description: включение файла
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/24/2018
ms.openlocfilehash: 05331c710817e575deb7729189c9b2d8ccbafd7d
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54489603"
---
1. Войдите на [портал Azure](https://portal.azure.com/) с помощью учетных данных для используемой подписки Azure. 

   ![Портал Azure](./media/aml-create-in-portal/portal-dashboard.png)

1. Выберите **Создать ресурс** в левом верхнем углу портала.

   ![Создание ресурса на портале Azure](./media/aml-create-in-portal/portal-create-a-resource.png)

1. В строке поиска введите **машинное обучение**. Выберите результат поиска **рабочей области Службы машинного обучения**.

   ![Поиск рабочей области](./media/aml-create-in-portal/allservices-search.PNG)

1. Прокрутите вниз панель **рабочей области Службы машинного обучения** и выберите **Создать**, чтобы начать создание.

   ![Создание](./media/aml-create-in-portal/portal-create-button.png)

1. Настройте рабочую область на панели **рабочей области Службы машинного обучения**.

   Поле|ОПИСАНИЕ
   ---|---
   имя рабочей области. |Введите уникальное имя для идентификации рабочей области. В этом примере мы используем **docs-ws**. Имена должны быть уникальными в группе ресурсов. Используйте имя, которое легко запомнить и отличить от рабочих областей, созданных другими пользователями.  
   Подписка |Выберите подписку Azure, которую нужно использовать.
   Группа ресурсов | Используйте группу ресурсов, которая есть в подписке, или введите имя, чтобы создать группу ресурсов. Группа ресурсов — это контейнер, содержащий связанные ресурсы для решения Azure. В этом примере мы используем **docs-aml**. 
   Расположение | Выберите ближайшее к пользователям и ресурсам данных расположение. В нем будет создана рабочая область.

   ![Создание рабочей области](./media/aml-create-in-portal/workspace-create.png)

1. Чтобы начать процесс создания, выберите **Создать**. Создание рабочей области может занять несколько минут.

1. Вы можете проверить состояние развертывания, щелкнув значок уведомлений (**колокольчик**) на панели инструментов.

1. По завершении процесса появится сообщение об успешном развертывании. Оно также присутствует в области уведомлений. Чтобы просмотреть новую рабочую область, выберите **Перейти к ресурсу**.

   ![Состояние создания рабочей области](./media/aml-create-in-portal/notifications.png)
