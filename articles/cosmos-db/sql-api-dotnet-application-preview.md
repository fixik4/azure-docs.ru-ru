---
title: Руководство по разработке веб-приложения ASP.NET MVC с использованием Azure Cosmos DB с помощью предварительной версии пакета SDK для .NET
description: В этом руководстве описано, как создать веб-приложение ASP.NET MVC с использованием Azure Cosmos DB. Вы можете хранить и получать доступ к данным JSON из приложения со списком задач, размещенного в Azure.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/03/2018
ms.author: dech
ms.openlocfilehash: bf1da7e8a1041b15076ebda6eeac9b0a75c567c0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "57857170"
---
# <a name="tutorial-develop-an-aspnet-mvc-web-application-with-azure-cosmos-db-by-using-net-preview-sdk"></a>Руководство. Разработка веб-приложения ASP.NET MVC с использованием Azure Cosmos DB с помощью предварительной версии пакета SDK для .NET 

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [.NET (предварительная версия)](sql-api-dotnet-application-preview.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)


В этом руководстве показано, как использовать Azure Cosmos DB для хранения данных и обеспечения доступа к ним из приложения ASP.NET MVC, размещенного в Azure. В этом руководстве используется пакет SDK версии 3 для .NET, который в данный момент находится в предварительной версии. На следующем рисунке показана веб-страница, которая создается с помощью примера в этой статье:
 
![Снимок экрана: список дел веб-приложения MVC, созданный с помощью этого руководства по ASP.NET MVC](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-image01.png)

Если у вас нет времени для работы с руководством, можно скачать полный пример проекта с [GitHub][GitHub]. 

Темы, рассматриваемые в этом руководстве:

> [!div class="checklist"]
> * Создание учетной записи Azure Cosmos.
> * Создание приложения ASP.NET MVC.
> * Подключение приложения к Azure Cosmos DB. 
> * Выполнение операций CRUD с данными.

> [!TIP]
> Предполагается, что у вас есть опыт использования ASP.NET MVC и веб-сайтов Azure. Если вы никогда не работали с ASP.NET или [необходимыми инструментами](#prerequisites), мы рекомендуем скачать полный пример проекта с портала [GitHub][GitHub], добавить требуемые пакеты NuGet и запустить его. Когда создадите проект, вы можете просмотреть эту статью, чтобы разобраться в коде этого проекта.

## <a name="prerequisites"></a>Предварительные требования 

Перед выполнением инструкций, приведенных в этой статье, обеспечьте наличие следующих ресурсов:

* **Активная учетная запись Azure**. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) , прежде чем начинать работу. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

* Пакет Microsoft Azure SDK для .NET (Visual Studio 2017), доступный через Visual Studio Installer.

Все снимки экранов в этой статье сделаны с помощью Microsoft Visual Studio Community 2017. Если система настроена с помощью другой версии, то вполне вероятно, что ваши экраны и параметры могут не соответствовать полностью, но если выполнить все вышеуказанные требования, то это решение должно работать.

## <a name="create-an-azure-cosmos-account"></a>Шаг 1. Создание учетной записи Azure Cosmos

Давайте сначала создадим учетную запись Azure Cosmos. Если у вас уже есть учетная запись API SQL Azure Cosmos DB или вы используете эмулятор Azure Cosmos DB для этого руководства, то вы можете перейти к разделу [Создание приложения ASP.NET MVC](#create-a-new-mvc-application).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

В следующем разделе вы создадите приложение ASP.NET MVC. 

## <a name="create-a-new-mvc-application"></a>Шаг 2. Создание приложения ASP.NET MVC

1. В Visual Studio откройте меню **Файл** и выберите **Создать**, а затем — **Проект**. Откроется диалоговое окно **Новый проект** .

2. В окне **Новый проект** разверните **установленные** шаблоны, **Visual C#**, **Веб**, а затем выберите **Веб-приложение ASP.NET**. 

   ![Создание проекта веб-приложения ASP.NET](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-new-project-dialog.png)

3. В поле **Имя** введите имя проекта. В этом учебнике используется имя todo. Если вы решили использовать другое имя, в ситуациях, когда будет идти речь о пространстве имен todo, откорректируйте представленные образцы кода, используя имя своего приложения. 

4. Выберите **Обзор**, чтобы перейти к папке, в которой вы хотите создать проект, а затем выберите **.NET Framework 4.6.1** или более поздней версии. Нажмите кнопку **ОК**. 

5. Отобразится диалоговое окно **Создание веб-приложения ASP.NET**. На панели «Шаблоны» выберите **MVC**.

6. Нажмите кнопку **ОК**, и Visual Studio сформирует пустой шаблон ASP.NET MVC. 

7. После того как Visual Studio закончит создание шаблонного приложения MVC, у вас будет пустое приложение ASP.NET, которое можно запустить локально.

## <a name="add-nuget-packages"></a>Шаг 3. Добавление пакета Azure Cosmos DB NuGet в проект

Теперь, когда у нас есть большая часть кода платформы ASP.NET MVC, необходимого для этого решения, давайте добавим пакеты NuGet, требуемые для подключения к Azure Cosmos DB.

1. Пакет .NET SDK для Azure Cosmos DB комплектуется и распространяется как пакет NuGet. Чтобы получить пакет NuGet в Visual Studio, используйте диспетчер пакетов NuGet в Visual Studio: щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **Управление пакетами NuGet**.
   
   ![Снимок экрана: параметры контекстного меню проекта веб-приложения в обозревателе решений с выделенным параметром "Управление пакетами NuGet"](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-manage-nuget.png)
   
2. Откроется диалоговое окно **Управление пакетами NuGet** . В поле **обзора** NuGet введите **Microsoft.Azure.Cosmos**. В результатах найдите версию **Microsoft.Azure.Cosmos** 3.0.0.1-preview и установите ее. Загрузится и установится пакет Azure Cosmos DB, и все зависимости, такие как Newtonsoft.Json. В окне **предварительного просмотра** выберите **ОК**, после чего в окне **Принятие условий лицензионного соглашения** выберите **Принять**. Установка будет завершена.
   
   Для установки пакета NuGet можно также воспользоваться консолью диспетчера пакетов. Для этого в меню **Инструменты** выберите **Диспетчер пакетов NuGet**, а затем — **Консоль диспетчера пакетов**. При появлении запроса введите следующую команду:
   
   ```bash
   Install-Package Microsoft.Azure.Cosmos -Version 3.0.0.1-preview
   ```        

3. После установки пакета в решении Visual Studio будут содержаться две новые ссылки на библиотеку, Microsoft.Azure.Cosmos.Client и Newtonsoft.Json.
  
## <a name="set-up-the-mvc-application"></a>Шаг 4. Настройка приложения ASP.NET MVC

Теперь добавим модели, представления и контроллеры в это приложение MVC.

* [Добавление модели](#add-a-model).
* [Добавление контроллера](#add-a-controller).
* [Добавление представлений](#add-views).

### <a name="add-a-model"></a> Добавление модели

1. В **обозревателе решений** щелкните правой кнопкой мыши папку **Модели**, выберите **Добавить**, а затем щелкните **Класс**. Откроется диалоговое окно **Добавление нового элемента** .

1. Назовите новый класс **TodoItem.cs** и выберите **Добавить**. 

1. Далее замените код в классе "Todoitem" следующим кодом.

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Models/TodoItem.cs)]
   
   Хранимые данные в Azure Cosmos DB передаются по сети и сохраняются в виде JSON-файлов. Для управления сериализацией и десериализацией объектов с помощью JSON.NET можно использовать атрибут **JsonProperty**, как показано на примере созданного класса **TodoItem**. Вы можете не только контролировать формат имени свойства при его поступлении в JSON, но также и переименовывать свойства .NET, как вы сделали со свойством **Description**. 

### <a name="add-a-controller"></a>Добавление контроллера

1. В **обозревателе решений** щелкните правой кнопкой мыши папку **Контроллеры**, выберите **Добавить**, а затем щелкните **Контроллер**. Откроется диалоговое окно **Добавление элемента формирования шаблонов** .

1. Выберите **Контроллер MVC 5 — пустой** и нажмите кнопку **Добавить**.

   ![Снимок экрана: диалоговое окно "Добавление шаблона" с выделенным параметром "Контроллер MVC 5 — пустой"](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Назовите новый контроллер **ItemController и замените код в этом файле следующим:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Controllers/ItemController.cs)]

   Атрибут **ValidateAntiForgeryToken** используется здесь для защиты этого приложения от атак с подделкой межсайтовых запросов. Кроме добавления этого атрибута представления также должны работать с данным маркером защиты от подделки запросов. Дополнительные сведения об этом и примеры правильной реализации этой технологии см. в статье [Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET MVC Application][Preventing Cross-Site Request Forgery] (Предотвращение атак с подделкой межсайтовых запросов в приложениях ASP.NET MVC). Исходный код, предоставленный на [GitHub][GitHub], имеет полную реализацию.
   
   Для защиты от атак overposting также используется атрибут **Bind** в параметре метода. Дополнительные сведения см. в записи блога [Implementing Basic CRUD Functionality with the Entity Framework in ASP.NET MVC Application][Basic CRUD Operations in ASP.NET MVC] (Реализация базовых функций CRUD с помощью Entity Framework в приложении ASP.NET MVC).
    
### <a name="add-views"></a>Добавление представлений

Теперь давайте создадим следующие три представления. 

* [Добавление представления "Элемент списка"](#AddItemIndexView).
* [Добавление представления "Создание элементов"](#AddNewIndexView).
* [Добавление представления "Редактирование элементов"](#AddEditIndexView).

#### <a name="AddItemIndexView"></a>Добавление представления "Элемент списка"

1. В **обозревателе решений** разверните папку **Представления**, щелкните правой кнопкой мыши пустую папку **Элемент**, созданную Visual Studio при добавлении **ItemController** ранее, выберите **Добавить**, а затем щелкните **Представление**.
   
   ![Снимок экрана: обозреватель решений, отображающий папку "Элемент", созданную Visual Studio, с выделенными командами "Добавления представления"](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view.png)

2. В диалоговом окне **Добавление представления** обновите следующие значения.
   
   * В поле **Имя представления** введите ***Индекс***.
   * В поле **Шаблон** выберите ***Список***.
   * В поле **Класс модели** выберите ***Item (todo.Models)*** (Элемент (todo.Models)).
   * В поле страницы макета введите ***~/Views/Shared/_Layout.cshtml***.
     
   ![Снимок экрана: диалоговое окно "Добавление представления"](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view-dialog.png)

3. После добавления этих значений выберите **Добавить**, и Visual Studio создаст представление. После этого откроется созданный CSHTML-файл. Вы можете закрыть этот файл в Visual Studio и вернуться к нему позже.

#### <a name="AddNewIndexView"></a>Добавление представления "Создание элементов"

Аналогично тому, как вы создали представление для вывода списка элементов, создайте представление для создания элементов, выполнив следующие действия:

1. В **обозревателе решений**, щелкните правой кнопкой мыши папку **Элемент**, выберите **Добавить**, а затем щелкните **Представление**.

1. В диалоговом окне **Добавление представления** обновите следующие значения.
   
   * В поле **Имя представления** введите ***Создать***.
   * В поле **Шаблон** выберите ***Создать***.
   * В поле **Класс модели** выберите ***Item (todo.Models)*** (Элемент (todo.Models)).
   * В поле страницы макета введите ***~/Views/Shared/_Layout.cshtml***.
   * Выберите **Добавить**.
   
#### <a name="AddEditIndexView"></a>Добавление представления "Редактирование элементов"

И наконец, добавьте представление для редактирования элемента, сделав следующее:

1. В **обозревателе решений**, щелкните правой кнопкой мыши папку **Элемент**, выберите **Добавить**, а затем щелкните **Представление**.

1. В диалоговом окне **Добавление представления** выполните следующие действия.
   
   * В поле **Имя представления** введите ***Изменить***.
   * В поле **Шаблон** выберите ***Изменить***.
   * В поле **Класс модели** выберите ***Item (todo.Models)*** (Элемент (todo.Models)).
   * В поле страницы макета введите ***~/Views/Shared/_Layout.cshtml***.
   * Выберите **Добавить**.

Как только все будет готово, закройте документы cshtml в Visual Studio, вы вернетесь к ним позже.

## <a name="connect-to-cosmosdb"></a>Шаг 5. Подключение к Azure Cosmos DB 

Теперь, когда мы позаботились об основных ресурсах MVC, давайте рассмотрим добавление кода для подключения к Azure Cosmos DB и выполнения операций CRUD. 

### <a name="perform-crud-operations"></a> Выполнение операций CRUD с данными

Прежде всего следует добавить класс, который содержит необходимую логику для подключения к службе Azure Cosmos DB и ее использования. В этом руководстве мы инкапсулируем эту логику в класс с именем TodoItemService.cs. Этот код считывает значения конечной точки Azure Cosmos DB, формирующие файл конфигурации, и выполняет операции CRUD, такие как вывод списка незавершенных элементов, создание, изменение и удаление элементов. 

1. В **обозревателе решений** создайте папку в проекте с именем **Services**.

1. Щелкните правой кнопкой мыши папку **Services**, выберите **Добавить**, а затем — **Класс**. Назовите новый класс **TodoItemService** и выберите **Добавить**.

1. Добавьте следующий код в класс **TodoItemService** и замените код в этом файле на такой:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Services/TodoItemService.cs)]
 
1. Предыдущий код считывает значения строк подключения из файла конфигурации. Чтобы обновить значения строк подключения учетной записи Azure Cosmos, откройте в проекте файл **Web.config** и добавьте следующие строки в раздел `<AppSettings>`:

   ```csharp
    <add key="endpoint" value="<enter the URI from the Keys blade of the Azure Portal>" />
    <add key="primaryKey" value="<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>" />
    <add key="database" value="Tasks" />
    <add key="container" value="Items" />
   ```
 
1. Обновите значения endpoint и primarykey на значения, полученные из колонки **Ключи** на портале Azure. Используйте **URI** из колонки "Ключи" в качестве значения параметра endpoint. Используйте значение **первичного ключа** или **вторичного** из колонки "Ключи" для параметра ключей. Это касается подключения Azure Cosmos DB к приложению. Теперь можно добавить логику приложения.

1. Откройте файл **Global.asax.cs** и добавьте в метод **Application_Start** следующую строку: 
   
   ```csharp
   TodoItemService.Initialize().GetAwaiter().GetResult();
   ```

   На данном этапе ваше решение должно выполнить сборку проекта без каких-либо ошибок. Чтобы запустить приложение сейчас, нужно открыть **HomeController** и представление **Индекс** в этом контроллере. Это поведение по умолчанию для шаблона проекта MVC мы выбрали вначале. Давайте изменим маршрутизацию в данном приложении MVC, чтобы изменить это поведение.

1. Откройте файл ***App\_Start\RouteConfig.cs***, найдите строку, начинающуюся с "defaults:", и обновите ее следующим кодом:

   ```csharp
   defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }
   ```

   Этот код теперь сообщает ASP.NET MVC, что, если вы не указали значение в поле URL-адреса для управления поведением маршрутизации, вместо **главной страницы** нужно использовать **Элемент** в качестве контроллера, а **Индекс** — в качестве представления.

Теперь при запуске приложения вызывается **ItemController**, вызывающий методы GetItems из класса TodoItemService, который вы определите в следующем разделе. 

Если создать и запустить этот проект сейчас, отобразится примерно следующие данные.    

![Снимок экрана: веб-приложение "Список дел", созданное с помощью этого руководства по базе данных](./media/sql-api-dotnet-application-preview/build-and-run-the-project-now.png)


## <a name="run-the-application"></a>Шаг 6. Локальный запуск приложения

Для проверки приложения на локальном компьютере выполните следующие действия:

1. Чтобы создать приложение в режиме отладки, откройте Visual Studio и нажмите клавишу F5. После этого будет создано приложение и откроется окно браузера с пустой сеткой, которую мы уже видели:
   
   ![Снимок экрана: веб-приложение "Список дел", созданное с помощью этого руководства по базе данных](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item-a.png)
       
2. Щелкните ссылку **Создать**, введите значения в поля **Имя** и **Описание**. Не устанавливайте флажок **Выполнено**, иначе новый элемент добавится ​​в завершенном состоянии и не отобразится в начальном списке.
   
3. Щелкните **Создать**. Вы будете перенаправлены в представление **Индекс**, а в списке появится ваш элемент. Вы можете добавить еще несколько элементов в список задач.

    ![Снимок экрана: представление "Индекс"](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item.png)
  
4. Нажмите кнопку **Изменить** рядом с **Элемент**. Вы будете перенаправлены в представление **Изменить**, где сможете изменить любое свойство объекта, в том числе флажок **Выполнено**. Если установить флажок **Выполнено** и нажать кнопку **Сохранить**, **Элемент** будет удален из списка незавершенных задач.
   
   ![Снимок экрана: представление "Индекс" с установленным флажком возле параметра "Завершено"](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-completed-item.png)

5. После проверки приложения нажмите клавиши CONTROL+F5, чтобы остановить отладку приложения. Теперь все готово к развертыванию.

## <a name="deploy-the-application-to-azure"></a>Шаг 7. Развертывание приложения 
Теперь, когда у вас есть готовое приложение, которое корректно работает в Azure Cosmos DB, мы собираемся развернуть его в службу приложений Azure.  

1. Чтобы опубликовать это приложение, щелкните проект правой кнопкой мыши в **обозревателе решений** и выберите действие **Опубликовать**.
   
2. В диалоговом окне **Публикация** выберите **Служба приложений Microsoft Azure**, затем выберите **Создать**, чтобы создать профиль службы приложений, или щелкните **Выбрать существующий**, чтобы использовать имеющийся профиль.

3. При наличии профиля Службы приложений Azure выберите **подписку** из раскрывающегося списка. Используйте фильтр **Просмотр** для сортировки по группе ресурсов или типу ресурсов. Затем выполните поиск необходимой Службы приложений Azure и нажмите кнопку **ОК**. 
   
   ![Диалоговое окно "Служба приложений" в Visual Studio](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-app-service.png)

4. Чтобы создать профиль службы приложений Azure, в диалоговом окне **Публикация** выберите **Создать**. В диалоговом окне **Создать службу приложений** введите имя веб-приложения и соответствующую подписку, группу ресурсов и план службы приложений, а затем нажмите кнопку **Создать**.

   ![Диалоговое окно "Создать службу приложений" в Visual Studio](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-app-service.png)

Через несколько секунд Visual Studio опубликует ваше веб-приложение и запустит браузер, где вы увидите свой проект, запущенный в Azure!

## <a name="next-steps"></a>Дополнительная информация
В этом руководстве вы узнали, как создать веб-приложение ASP.NET MVC, которое может получить доступ к хранимым данным в Azure Cosmos DB. Теперь вы можете перейти к следующей статье:

> [!div class="nextstepaction"]
> [Создание веб-приложения Java с использованием Azure Cosmos DB и API SQL]( sql-api-java-application.md)


[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-todo-app
