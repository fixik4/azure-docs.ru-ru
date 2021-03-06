---
title: Техническая информация для предложений приложений Power BI | Документация Майкрософт
description: Настройка полей технической информации для предложений приложений Power BI для Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: pbutlerm
ms.openlocfilehash: ca77da897eed51c8d832cad7052c2144d6ada562
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806492"
---
# <a name="power-bi-apps-technical-info-tab"></a>Вкладка технической информации о приложениях Power BI

На **новое предложение** странице **технические сведения о** tab, чтобы предоставить Power BI установщик URL-адрес пакета и другие сведения, необходимые для проверки нового предложения.  В исходной версии все приложения Power BI бесплатны и доступны для загрузки из AppSource. По этой причине нельзя определить хранения (SKU) для этого типа предложения.

![Технические сведения о вкладке](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Поля на вкладке с техническими сведениями 

На **технические сведения о** вкладку, завершите полях, описанных в следующей таблице. Символ звездочки (*) в конце метку поля означает, что поле является обязательным.

|        Поле          |  ОПИСАНИЕ                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **Installer URL** (URL-адрес установщика)     | Power BI создает этот URL-адрес, при публикации приложения и его распространение в рабочей среде.  Дополнительные сведения см. в разделе [публикация приложений с помощью панели мониторинга и отчеты в Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Validation instructions** (Инструкции по проверке)  |  Если вы хотите добавьте инструкции (до 3000 символов), чтобы помочь группе Microsoft проверки настройки, подключения и тестирование приложения. Включить типичных параметров конфигурации, учетные записи, параметры или другие сведения, которые можно использовать для тестирования режим подключения данных. Эта информация будет видна только команда проверки, и он используется только для целей проверки.  |
| **Is this app created as a Power BI content pack?** (Создается ли это приложение в виде пакета содержимого Power BI?) | В настоящее время это поле используется только для внутреннего использования. Оставьте значение по умолчанию **нет**. Если вы измените значение параметра на **Да**, можно остановить процесс публикации.  |  
|  |  |


## <a name="next-steps"></a>Дальнейшие действия

На [сведения Storefront](./cpp-storefront-details-tab.md) вкладке, предоставляют сведения о маркетинговых и юридических для вашего приложения.

