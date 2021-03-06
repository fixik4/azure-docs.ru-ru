---
title: Создание центра Интернета вещей Azure персональные информационные панели | Документация Майкрософт
description: Как пользователь Узнайте, как создавать и управлять ими личных панелей мониторинга.
author: dominicbetts
ms.author: dobett
ms.date: 02/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: fb74669dcaa802ad06a9c4dff3ffdf25726f518c
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2019
ms.locfileid: "57316071"
---
# <a name="create-and-manage-personal-dashboards"></a>Создание и управление ими персональные информационные панели

**Панели мониторинга** — это страница, загружающий при первом переходе к приложению. Объект **построитель** приложения определяет панели мониторинга приложения по умолчанию для всех пользователей. Эта панель мониторинга по умолчанию можно заменить собственным, панель мониторинга персонализированные приложения. Может иметь несколько панелей мониторинга, которые отображают различные данные и переключаться между ними.

## <a name="create-dashboard"></a>Создание панели мониторинга

На следующем рисунке показаны панели мониторинга в приложения, созданного из **пример Contoso** шаблона. На панели мониторинга приложения по умолчанию можно заменить личной информационной панели. Чтобы сделать это, выберите **+ создать** в верхней правой части страницы.

![Панель мониторинга для приложений на основе шаблона «Пример Contoso»](media/howto-personalize-dashboard/defaultdashboard.png)

Выбрав **+ создать**, откроется редактор панелей мониторинга. В редакторе можно присвоить информационной панели имя и выбрать элементы из библиотеки. Библиотека содержит плитки и панели мониторинга примитивы, которые можно использовать для настройки панели мониторинга.

![Библиотека панелей мониторинга](media/howto-personalize-dashboard/dashboardeditor.png)

Например, можно добавить **параметров устройств и свойства** плитки для отображения значений параметров и свойств для одного из управляемых устройств. Для этого выберите **Шаблон устройства**, а затем **Экземпляр устройства**. Предоставьте плитки, название и выберите **параметр** или **свойство** для отображения. На следующем снимке экрана показан **скорость вентилятора** отключена, чтобы добавить к плитке. Выберите **сделать** для сохранения изменений к панели мониторинга.

![Форма настройки сведений об устройстве со сведениями о параметрах и свойствах](media/howto-personalize-dashboard/dashboardsetting.png)

Теперь при просмотре личные панели мониторинга, вы увидите новую плитку с **скорость вентилятора** для устройства:

![Вкладка "Панель мониторинга", на которой отображаются параметры и свойства плитки](media/howto-personalize-dashboard/personaldashboard.png)

Вы можете изучить другие плитки типы в библиотеке для обнаружения для дальнейшей настройки личных панелей мониторинга.

## <a name="manage-dashboards"></a>Управление панелями мониторинга

У вас есть несколько персональные информационные панели и переключаться между ними, или выберите на панели мониторинга приложения по умолчанию:

![Панель мониторинга коммутатора](media/howto-personalize-dashboard/switchdashboards.png)

Можно изменить личные информационные панели и удалите те из них, которые больше не нужны:

![Удалить панель мониторинга](media/howto-personalize-dashboard/managedashboards.png)

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали, как создавать и управлять ими персональные информационные панели, вы можете:

> [!div class="nextstepaction"]
> [Узнайте, как настроить параметры приложения](howto-manage-preferences.md)
