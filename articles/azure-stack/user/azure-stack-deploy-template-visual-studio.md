---
title: Развертывание шаблонов в Azure Stack с помощью Visual Studio | Документация Майкрософт
description: Узнайте, как развертывать шаблоны в Azure Stack с помощью Visual Studio.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: f4fda8bbbb1bf88934f641644f89c319317ec58f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55237527"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Развертывание шаблонов в Azure Stack с помощью Visual Studio

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

С помощью Visual Studio можно развертывать шаблоны Azure Resource Manager в Azure Stack.

## <a name="to-deploy-a-template"></a>Развертывание шаблона

1. [Выполните установку и подключение](azure-stack-install-visual-studio.md) к Azure Stack с помощью Visual Studio.
2. Откройте Visual Studio.
3. Выберите **Файл**, а затем выберите **Создать**. В диалоговом окне **Новый проект** выберите **Группа ресурсов Azure**.
4. Введите **имя** нового проекта и нажмите кнопку **ОК**.
5. В диалоговом окне **Выберите шаблон Azure** из раскрывающегося списка выберите **Azure Stack Quickstart** (Краткое руководство по Azure Stack).
6. Выберите **101-create-storage-account**, а затем нажмите кнопку **ОК**.
7. В новом проекте разверните узел **Шаблоны** в **обозревателе решений**, чтобы просмотреть доступные шаблоны.
8. В **обозревателе решений** выберите имя своего проекта, а затем щелкните **Развернуть**. Выберите **Новое развертывание**.
9. В диалоговом окне **Развернуть в группе ресурсов** из раскрывающегося списка **Подписка** выберите свою подписку Microsoft Azure Stack.
10. Выберите существующую группу ресурсов из раскрывающегося списка **Группа ресурсов** или создайте новую.
11. Из списка **Расположение группы ресурсов** выберите расположение, затем нажмите кнопку **Развернуть**.
12. В диалоговом окне **Изменить параметры** введите значения параметров (которые зависят от шаблона), а затем нажмите кнопку **Сохранить**.

## <a name="next-steps"></a>Дополнительная информация

* [Развертывание шаблонов с помощью командной строки](azure-stack-deploy-template-command-line.md)
* [Разработка шаблонов для Azure Stack](azure-stack-develop-templates.md)
