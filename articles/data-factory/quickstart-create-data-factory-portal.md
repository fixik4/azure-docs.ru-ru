---
title: Создание фабрики данных Azure с помощью пользовательского интерфейса службы "Фабрика данных Azure" | Документация Майкрософт
description: Создайте фабрику данных с конвейером, в котором данные копируются из одного расположения в хранилище BLOB-объектов Azure в другое.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 6f5a4e04c0d135e85624b04dbcdcda6b7d15a427
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2019
ms.locfileid: "55989395"
---
# <a name="quickstart-create-a-data-factory-by-using-the-azure-data-factory-ui"></a>Краткое руководство. Создание фабрики данных с помощью пользовательского интерфейса службы "Фабрика данных Azure"

> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Версия 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Текущая версия](quickstart-create-data-factory-portal.md)

В этом кратком руководстве описано создание и мониторинг фабрики данных Azure с помощью пользовательского интерфейса службы "Фабрика данных Azure". Конвейер, который вы создадите в этой фабрике данных, *копирует* данные из одной папки в другую в хранилище BLOB-объектов Azure. Инструкции по *преобразованию* данных с помощью Фабрики данных Azure см. в статье[ Преобразование данных в облаке с помощью действия Spark в фабрике данных Azure](tutorial-transform-data-spark-portal.md).

> [!NOTE]
> Если вы еще не работали со службой "Фабрика данных Azure", ознакомьтесь с [общими сведениями](data-factory-introduction.md) о ней перед изучением этого руководства. 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Видео 
Это видео поможет составить представление о пользовательском интерфейсе службы "Фабрика данных": 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Создание фабрики данных

1. Запустите веб-браузер **Microsoft Edge** или **Google Chrome**. Сейчас только эти браузеры поддерживают пользовательский интерфейс фабрики данных.
1. Перейдите на [портал Azure](https://portal.azure.com). 
1. В меню слева выберите **Создать ресурс**, **Аналитика**, **Фабрика данных**. 
   
   ![Выбор фабрики данных в области "Создать"](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
1. На странице **Новая фабрика данных** введите **ADFTutorialDataFactory** в поле **Имя**. 
      
   ![Страница "Новая фабрика данных"](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Имя фабрики данных Azure должно быть *глобально уникальным*. При возникновении указанной ниже ошибки измените имя фабрики данных (например, на **&lt;ваше_имя_&gt;ADFTutorialDataFactory**) и попробуйте создать фабрику данных снова. Правила именования для артефактов службы "Фабрика данных" см. в [этой](naming-rules.md) статье.
  
   ![Ошибка "Имя недоступно"](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
1. В поле **Подписка** выберите подписку Azure, в рамках которой вы хотите создать фабрику данных. 
1. Для **группы ресурсов** выполните одно из следующих действий:
     
   - Выберите **Use existing** (Использовать имеющуюся) и выберите имеющуюся группу ресурсов в списке. 
   - Выберите **Создать новую**и укажите имя группы ресурсов.   
         
   Сведения о группах ресурсов см. в статье, где описывается [использование групп ресурсов для управления ресурсами Azure](../azure-resource-manager/resource-group-overview.md).  
1. Укажите **V2** при выборе **версии**.
1. В поле **Расположение** выберите расположение фабрики данных.

   В списке отображаются только расположения, которые поддерживаются Фабрикой данных и в которых будут храниться метаданные Фабрики данных Azure. Учтите, что связанные хранилища данных, (такие как служба хранилища Azure и база данных SQL Azure) и вычислительные среды (например, Azure HDInsight), используемые фабрикой данных, могут находиться в других регионах.

1. Нажмите кнопку **Создать**.

1. Когда создание завершится, откроется страница **Фабрика данных**. Выберите элемент **Author & Monitor** (Создание и мониторинг), чтобы открыть на отдельной вкладке приложение пользовательского интерфейса службы "Фабрика данных Azure".
   
   ![Домашняя страница фабрики данных с элементом Author & Monitor (Создание и мониторинг)](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
1. На странице **Let's get started** (Начало работы) переключитесь на вкладку **Автор** на панели слева. 

    ![Страница Let's get started (Начало работы)](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-a-linked-service"></a>Создание связанной службы
На этом этапе вы создадите связанную службу, чтобы связать учетную запись хранения Azure с фабрикой данных. Связанная служба содержит сведения о подключении, используемые фабрикой данных для подключения к ней в среде выполнения.

1. Выберите **Подключения** и нажмите кнопку **Создать** на панели инструментов. 

   ![Кнопки для создания подключения](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
1. На странице **New Linked Service** (Новая связанная служба) выберите **Хранилище BLOB-объектов Azure** и щелкните **Продолжить**. 

   ![Выбор элемента "Хранилище BLOB-объектов Azure"](./media/quickstart-create-data-factory-portal/select-azure-blob-linked-service.png)
1. Затем сделайте следующее: 

   a. Введите **AzureStorageLinkedService** в поле **имени**.

   б) Выберите имя учетной записи хранения Azure в списке **имен учетных записей хранения**.

   c. Нажмите кнопку **Test connection** (Проверка подключения), чтобы проверить подключение службы "Фабрика данных" к учетной записи хранения. 

   4.3. Чтобы сохранить связанную службу, щелкните **Готово**. 

   ![Параметры связанной службы хранилища Azure](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 

## <a name="create-datasets"></a>Создание наборов данных
В ходе этой процедуры вы создадите два набора данных, **InputDataset** и **OutputDataset**. Эти наборы данных имеют тип **AzureBlob**. Они будут ссылаться на связанную службу хранилища Azure, созданную в предыдущем разделе. 

Входной набор данных представляет исходные данные в папке входных данных. В определении входного набора данных укажите контейнер больших двоичных объектов (**adftutorial**), папку (**input**) и файл (**emp.txt**), определяющие расположение исходных данных. 

Выходной набор данных представляет данные, которые копируются в место назначения. В определении выходного набора данных укажите контейнер больших двоичных объектов (**adftutorial**), папку (**output**) и файл, определяющие расположение копируемых данных. С каждым запуском конвейера связан уникальный идентификатор, который можно получить из системной переменной **RunId**. Имя выходного файла вычисляется динамически на основе идентификатора выполнения текущего конвейера.   

В параметрах связанной службы вы указали учетную запись хранения Azure, которая содержит исходные данные. В параметрах исходного набора данных следует указать точное расположение исходных данных (контейнер больших двоичных объектов, папку и файл). В параметрах целевого набора данных следует указать точное расположение для копирования данных (контейнер больших двоичных объектов, папку и файл). 
 
1. Нажмите кнопку **+** (плюс) и выберите **Набор данных**.

   ![Меню для создания набора данных](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
1. На странице **Новый набор данных** выберите **Хранилище BLOB-объектов Azure** и щелкните **Готово**. 

   ![Выбор элемента "Хранилища BLOB-объектов Azure"](./media/quickstart-create-data-factory-portal/select-azure-blob-dataset.png)
1. На вкладке **Общие** для набора данных введите **InputDataset** в поле **Имя**. 

1. Перейдите на вкладку **Подключение** и выполните следующие действия: 

    a. Выберите **AzureStorageLinkedService** в списке **Связанная служба**.

    б) Возле поля **Путь к файлу** нажмите кнопку **Обзор**.

    ![Вкладка "Подключение" и кнопка "Обзор"](./media/quickstart-create-data-factory-portal/file-path-browse-button.png) в. В окне **Choose a file or folder** (Выберите файл или папку) перейдите к папке **input** контейнера **adftutorial**, выберите файл **emp.txt** и нажмите кнопку **Готово**.

    ![Выбор входного файла](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    
    4.3. (Необязательно.) Щелкните **Preview data** (Просмотр данных), чтобы изучить содержимое файла emp.txt.     

1. Повторите эти шаги, чтобы создать выходной набор данных:  

   a. Нажмите кнопку **+** (плюс) и выберите **Набор данных**.

   б) На странице **Новый набор данных** выберите **Хранилище BLOB-объектов Azure** и щелкните **Готово**.

   c. В таблице **Общие** укажите имя **OutputDataset**.

   4.3. На вкладке **Подключение** выберите **AzureStorageLinkedService** в качестве связанной службы и введите для папки значение **adftutorial/output** в поле каталога. Если папка **output** отсутствует, действие копирования создаст ее во время выполнения.

## <a name="create-a-pipeline"></a>Создание конвейера 
На этом этапе вы создадите и проверите конвейер с действием копирования, которое использует входной и выходной наборы данных. При помощи действия копирования данные копируются из файла, указанного в параметрах входного набора данных, в файл, указанный в параметрах выходного набора данных. Если входной набор данных определяет только папку (но не имя файла), при помощи действия копирования в место назначения копируются все файлы из исходной папки. 

1. Нажмите кнопку **+** (плюс) и выберите **Pipeline** (Конвейер). 

   ![Меню для создания конвейера](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
1. На вкладке **Общие** укажите значение **CopyPipeline** в поле **Имя**. 

1. На панели элементов **Действия** разверните **Переместить и преобразовать**. Перетащите действие **Копировать** с панели элементов **Действия** в область конструктора конвейера. Также на панели элементов **Действия** можно применить поиск. Введите **CopyFromBlobToBlob** в поле **имя**.

   ![Общие параметры действия копирования](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
1. Откройте вкладку **Источник** в параметрах действия копирования и выберите **InputDataset** в списке **исходных наборов данных**.

1. Откройте вкладку **Приемник** в параметрах действия копирования и выберите **OutputDataset** в списке **целевых наборов данных**.

1. На панели инструментов конвейера над холстом щелкните **Проверка**, чтобы проверить параметры конвейера. Убедитесь, что проверка конвейера прошла успешно. Чтобы закрыть окно проверки, нажмите кнопку **>>** (стрелка вправо). 

## <a name="debug-the-pipeline"></a>Выполнение отладки конвейера
На этом этапе вы выполните отладку конвейера перед его развертыванием в службе "Фабрика данных". 

1. На панели инструментов конвейера над холстом щелкните **Отладка**, чтобы активировать тестовый запуск. 
    
1. Убедитесь, что сведения о состоянии запуска для конвейера отображаются на вкладке **Выходные данные** в нижней части окна. 

1. Убедитесь, что выходной файл появился в папке **output** конвейера **adftutorial**. Если указанной папки выходных данных нет, она будет автоматически создана в службе "Фабрика данных". 

## <a name="trigger-the-pipeline-manually"></a>Aктивация конвейера вручную
На этом шаге вы развернете сущности (связанные службы, наборы данных и конвейеры) в службе "Фабрика данных Azure". После этого вы вручную запустите конвейер. 

1. Перед запуском конвейера необходимо опубликовать сущности в службе "Фабрика данных Azure". Для этого щелкните **Publish All** (Опубликовать все) в верхней части окна. 

   ![Кнопка "Опубликовать"](./media/quickstart-create-data-factory-portal/publish-button.png)
1. Чтобы вручную запустить конвейер, выберите **Trigger** (Активировать) на панели инструментов конвейера и **Trigger Now** (Активировать сейчас). 

## <a name="monitor-the-pipeline"></a>Мониторинг конвейера

1. Перейдите на вкладку **Мониторинг** слева. Нажмите кнопку **Обновить**, чтобы обновить этот список.

   ![Вкладка для наблюдения за запуском конвейера с кнопкой "Обновить"](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
1. Щелкните ссылку **View Activity Runs** (Просмотр запусков действий) в списке **Действия**. На этой странице отображаются сведения о состоянии запуска для действия копирования. 

   ![Выполнение действий конвейера](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
1. Чтобы просмотреть сведения об операции копирования, щелкните ссылку **Сведения** (изображение очков) в столбце **Действия**. Дополнительные сведения о свойствах см. в [обзоре действия копирования](copy-activity-overview.md). 

   ![Сведения об операции копирования](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
1. Убедитесь, что в папке **output** появился новый файл. 
1. Чтобы вернуться из режима **Запуски операции** в режим **Pipeline Runs** (Запуски конвейера), щелкните ссылку **Конвейеры**. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>запуск конвейера по расписанию;
Эта процедура необязательна в этом руководстве. Вы можете создать *триггер планировщика* для периодического запуска конвейера (ежечасно, ежедневно и т. д.). На этом этапе вы создадите триггер для выполнения каждую минуту до указанного времени и даты. 

1. Переключитесь на вкладку **Автор**. 

1. Перейдите к конвейеру, выберите **Trigger** (Активировать) на панели инструментов конвейера, а затем выберите **Создать/изменить**. 

1. На странице **Add Triggers** (Добавление триггеров) щелкните **Choose trigger** (Выбор триггера), а затем — **Создать**. 

1. На странице **Новый триггер** в поле **End** (Окончание) выберите **On Date** (Дата) и укажите время окончания (на несколько минут позже текущего). Затем щелкните **Применить**. 

   Каждый запуск конвейера оплачивается, поэтому не следует выбирать время окончания, наступающее намного позднее времени начала. Убедитесь, что выбран один и тот же день. Но при этом обязательно оставьте достаточно времени, чтобы конвейер успел запуститься между временем публикации и временем окончания. Триггер вступит в силу только после публикации решения в службе "Фабрика данных", а не сразу при сохранении триггера в пользовательском интерфейсе. 

   ![Настройки триггера](./media/quickstart-create-data-factory-portal/trigger-settings.png)
1. На странице **New Trigger** (Создание триггера) установите флажок **Activated** (Активировано), а затем нажмите кнопку **Далее**. 

   ![Флажок Activated (Активировано) и кнопка "Далее"](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
1. Ознакомьтесь с предупреждающим сообщением и нажмите кнопку **Готово**.

   ![Предупреждение и кнопка "Готово"](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
1. Щелкните **Опубликовать все**, чтобы перенести изменения в службу "Фабрика данных". 

1. Перейдите на вкладку **Мониторинг** слева. Щелкните **Обновить**, чтобы обновить список. Вы увидите, что конвейер запускается каждую минуту в течение всего периода — от времени публикации до времени окончания. 

   Обратите внимание на значения в столбце **Активировано**. Здесь указан запуск вручную, который вы выполняли на предыдущем шаге (**Trigger Now** (Запустить сейчас)). 

   ![Список активированных запусков](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
1. Перейдите к представлению **Запуски триггеров**. 

   ![Переход к представлению Trigger Runs (Запуски триггеров)](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
1. Убедитесь, что для каждого запуска конвейера в папке **output** создается выходной файл вплоть до указанного времени окончания. 

## <a name="next-steps"></a>Дополнительная информация
В этом примере конвейер копирует данные из одного расположения в другое в хранилище BLOB-объектов Azure. Перейдите к [руководствам](tutorial-copy-data-portal.md), чтобы узнать об использовании фабрики данных в различных сценариях. 