---
title: Перенос баз знаний с помощью QnA Maker
titleSuffix: Azure Cognitive Services
description: Переместите базы знаний, созданные с помощью QnA Maker, в новую базу знаний.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/13/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: e91f41633bf6cf6581c237e7634150a5b92746a7
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245921"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Миграция базы знаний с помощью экспорта и импорта

Чтобы перенести базу знаний, необходимо экспортировать одну базы знаний, а затем импортировать ее в другую базу знаний. 

## <a name="prerequisites"></a>Предварительные требования

* Перед началом работы создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Настройте новую [службу QnA Maker](../How-To/set-up-qnamaker-service-azure.md).

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Перенос базы знаний из QnA Maker
1. Войдите на [портал QnA Maker](https://qnamaker.ai).
1. Выберите базу знаний, которую нужно перенести.

1. На странице **Settings** (Параметры) выберите **Export knowledge base** (Экспортировать базу знаний), чтобы скачать TSV-файл с содержимым базы знаний: вопросами, ответами, метаданными и именами источников данных, из которых они были извлечены.

1. Выберите **Create a knowledge base** (Создать базу знаний) в верхнем меню и создайте пустую базу знаний. 

    ![Настройка источников данных](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Введите **имя** для службы. Поддерживаются повторяющиеся имена, а также специальные знаки в имени.

1. Нажмите кнопку **Создать**.

    ![Создание базы знаний](../media/qnamaker-how-to-create-kb/create-kb.png)

1. В новой базе знаний откройте вкладку **Settings** (Параметры) и выберите **Import knowledge base** (Импортировать базу знаний). При этом будут импортированы вопросы, ответы и метаданные, а также сохранены имена источников данных, из которых они были извлечены.

   ![Импорт базы знаний](../media/qnamaker-how-to-migrate-kb/Import.png)

1. **Проверьте** новую базу знаний с помощью панели "Тестирование". Узнайте, как [проверить базу знаний](../How-To/test-knowledge-base.md).
1. **Опубликуйте** базу знаний. Узнайте, как [опубликовать базу знаний](../How-To/publish-knowledge-base.md).
1. Конечную точку можно использовать в коде приложения или бота. Сведения о [создании бота QnA](../Tutorials/create-qna-bot.md).

    ![Значения QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

    На этом этапе все содержимое базы знаний, вопросы, ответы и метаданные, а также имена исходных файлов и URL-адреса будут импортированы в новую базу знаний. 

## <a name="chat-logs-and-alterations"></a>Журналы чатов и варианты
Преобразования (синонимы) без учета регистра не импортируются автоматически. Используйте [интерфейсы API версии 2](https://aka.ms/qnamaker-v2-apis), чтобы экспортировать варианты слов из старой базы знаний. Затем используйте [интерфейсы API версии 4](https://aka.ms/qnamaker-v4-apis), чтобы переместить эти варианты в новую базу знаний.

Возможность переноса журналов чатов не предусмотрена, так как новая база знаний использует для хранения журналов чатов Application Insights. 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Редактирование базы знаний](../How-To/edit-knowledge-base.md)
