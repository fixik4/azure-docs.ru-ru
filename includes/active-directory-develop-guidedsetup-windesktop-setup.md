---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 2a7734f729c4b1db7e8c0b4571e8792373ee11ae
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203212"
---
## <a name="set-up-your-project"></a>Настройка проекта

В этом разделе вы создадите проект, чтобы продемонстрировать, как интегрировать классическое приложение для Windows .NET (XAML) с *входом с учетной записью Майкрософт*, что позволит приложению выполнять запрос к веб-API, требующим маркер.

В приложении, которое будет создано при работе с этим руководством, отображается кнопка, используемая для вызова графа, области для отображения результатов на экране, и кнопка выхода.

> [!NOTE]
> Предпочитаете скачать этот пример проекта Visual Studio? [Скачайте проект](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) и перейдите к [настройке](#register-your-application), чтобы настроить пример кода перед его выполнением.
>

Для создания приложения выполните следующие действия.

1. В Visual Studio выберите **Файл** > **Создать** > **Проект**.
2. В разделе **Шаблоны** выберите **Visual C#**.
3. Выберите **WPF App** (.NET Framework) в зависимости от используемой версии Visual Studio.

## <a name="add-msal-to-your-project"></a>Добавление MSAL в проект

1. В Visual Studio выберите **Сервис** > **Диспетчер пакетов NuGet**> **Консоль диспетчера пакетов**.
2. В окне консоли диспетчера пакетов вставьте следующую команду Azure PowerShell:

    ```powershell
    Install-Package Microsoft.Identity.Client
    ```

    > [!NOTE] 
    > Эта команда устанавливает библиотеку проверки подлинности Майкрософт. MSAL обрабатывает получение, кэширование и обновление маркеров пользователей, которые используются для доступа к программным интерфейсам, защищенным Azure Active Directory версии 2.0
    >

## <a name="add-the-code-to-initialize-msal"></a>Добавление кода для инициализации MSAL

На этом шаге вы создадите класс для обработки взаимодействия с MSAL, например обработки маркеров.

1. Откройте файл *App.xaml.cs*, а затем добавьте ссылку для MSAL в класс:

    ```csharp
    using Microsoft.Identity.Client;
    ```
   <!-- Workaround for Docs conversion bug -->

2. Обновите класс app следующим образом:

    ```csharp
    public partial class App : Application
    {
        //Below is the clientId of your app registration. 
        //You have to replace the below with the Application Id for your app registration
        private static string ClientId = "your_client_id_here";

        public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

    }
    ```

## <a name="create-the-application-ui"></a>Создание пользовательского интерфейса приложения

В этом разделе описывается, как приложение может запрашивать защищенный внутренний сервер, например Microsoft Graph. 

Файл *MainWindow.xaml* должен создаваться автоматически как часть шаблона проекта. Откройте этот файл, а затем замените узел *\<Grid>* вашего приложения следующим кодом:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
