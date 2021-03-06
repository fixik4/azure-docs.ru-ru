---
title: Настройка Пакета средств разработки Azure Stack (ASDK) после его развертывания | Документация Майкрософт
description: Описание изменений, которые мы рекомендуем внести в конфигурацию после установки Пакета средств разработки Azure Stack (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/10/2018
ms.openlocfilehash: b1b11dc27b279173ede4498ca353aea4018ea8f9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58102440"
---
# <a name="post-asdk-installation-configuration-tasks"></a>Настройка, выполняемая после установки ASDK

После [ установки Пакета средств разработки Azure Stack (ASDK)](asdk-install.md) вам необходимо внести в конфигурацию несколько изменений. Для этого на главном компьютере с ASDK нужно выполнить вход с правами пользователя AzureStack или AzureStackAdmin. 

## <a name="install-azure-stack-powershell"></a>Установка PowerShell для Azure Stack

Для работы с Azure Stack необходимы модули Azure PowerShell, совместимые с Azure Stack.

Команды PowerShell для Azure Stack устанавливаются с использованием коллекции PowerShell. Чтобы зарегистрировать репозиторий PSGallery, откройте сеанс Windows PowerShell с повышенными привилегиями и выполните в нем следующие команды.

``` Powershell
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

Для указания совместимых с Azure Stack модулей AzureRM рекомендуем использовать профили версии API.  Профили версий API позволяют управлять различиями между версиями Azure и Azure Stack. Профиль версии API — это набор модулей AzureRM PowerShell с определенными версиями API. Модуль **AzureRM.Bootstrapper**, доступный в коллекции PowerShell, предоставляет командлеты PowerShell, необходимые для работы с профилями версий API.

Последнюю версию модуля PowerShell для Azure Stack можно установить в двух режимах: с подключением главного компьютера ASDK к Интернету или без него:

> [!IMPORTANT]
> Прежде чем устанавливать нужную версию, обязательно [удалите все установленные модули Azure PowerShell](../azure-stack-powershell-install.md#3-uninstall-existing-versions-of-the-azure-stack-powershell-modules).

- **При наличии подключения к Интернету** с главного компьютера ASDK. Выполните следующий скрипт PowerShell, чтобы установить эти модули там, где установлен пакет средств разработки.

- Azure Stack 1901 и последующих версий.

    ```PowerShell
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Install-Module AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.0
    ```

    > [!Note]  
    > Модуль Azure Stack версии 1.7.0 является критическим изменением. Чтобы выполнить миграцию из Azure Stack 1.6.0, см. руководство по миграции, указанное [здесь](https://aka.ms/azspshmigration170).

  - Azure Stack 1811.

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.6.0.
    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

  - Azure Stack 1810 и прежних версий.

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.5.0.
    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

  Если установка выполнена успешно, в выходных данных указываются модули AzureRM и AzureStack.

- **Без подключения к Интернету** с главного компьютера ASDK. При установке в автономном режиме следует заранее скачать модули PowerShell на компьютер, подключенный к Интернету, с помощью следующих команд PowerShell:

  ```PowerShell
  $Path = "<Path that is used to save the packages>"

  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
  
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
  ```

  После этого перенесите скачанные пакеты на компьютер ASDK и зарегистрируйте это расположение в качестве репозитория по умолчанию, а затем установите модули AzureRM и AzureStack из этого репозитория:

    ```PowerShell  
    $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
    $RepoName = "MyNuGetSource"

    Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

    Install-Module AzureRM -Repository $RepoName

    Install-Module AzureStack -Repository $RepoName
    ```

## <a name="download-the-azure-stack-tools"></a>Скачивание средств Azure Stack

[AzureStack-Tools](https://github.com/Azure/AzureStack-Tools) — это репозиторий GitHub, в котором размещены модули PowerShell для администрирования и развертывания ресурсов в Azure Stack. Чтобы получить эти средства, клонируйте репозиторий GitHub или скачайте папку AzureStack-Tools, выполнив следующий скрипт:

  ```PowerShell
  # Change directory to the root directory.
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  invoke-webrequest `
    https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  expand-archive master.zip -DestinationPath . -Force

  # Change to the tools directory.
  cd AzureStack-Tools-master
  ```

## <a name="validate-the-asdk-installation"></a>Проверка установки ASDK

Чтобы убедиться, что развертывание ASDK прошло успешно, примените командлет Test-AzureStack следующим образом:

1. Войдите на главный компьютер ASDK с именем AzureStack\AzureStackAdmin.
2. Откройте PowerShell от имени администратора (не путайте с интегрированной средой сценариев Windows PowerShell).
3. Выполните команду `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Выполните команду `Test-AzureStack`

Выполнение тестов может занять несколько минут. Если установка выполнена успешно, выходные данные выглядят примерно так:

![Test-AzureStack](media/asdk-post-deploy/test-azurestack.png)

Если операция закончится сбоем, выполните рекомендации из раздела об устранении неполадок.

## <a name="reset-the-password-expiration-policy"></a>Политика сброса срока действия пароля 

Чтобы убедиться, что срок действия пароля узла комплекта разработки не завершится до окончания периода ознакомления, выполните следующие действия после развертывания ASDK.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Изменение политики срока действия паролей из PowerShell

В консоли Powershell с повышенными привилегиями выполните команду:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Изменение политики срока действия паролей вручную

1. На компьютере с пакетом средств разработки откройте **Управление групповыми политиками** (GPMC.MMC) и последовательно выберите **Управление групповой политикой**, **Лес: azurestack.local**, **Домены**, **azurestack.local**.
2. Щелкните правой кнопкой мыши **политику домена по умолчанию** и выберите **Изменить**.
3. В редакторе "Управление групповыми политиками" последовательно выберите **Конфигурация компьютера**, **Политики**, **Параметры Windows**, **Параметры безопасности**, **Политики учетных записей**, **Политика паролей**.
4. В области справа дважды щелкните параметр **Максимальный срок действия пароля**.
5. В диалоговом окне **Maximum password age Properties** (Свойства максимального срока действия пароля) измените значение параметра **Срок истечения действия пароля** на **180** и нажмите кнопку **ОК**.

![Консоль управления групповой политикой](media/asdk-post-deploy/gpmc.png)

## <a name="enable-multi-tenancy"></a>Включение поддержки мультитенантности

Для развертываний с помощью Azure AD необходимо [включить поддержку мультитенантности](../azure-stack-enable-multitenancy.md#enable-multi-tenancy) для установки ASDK.

> [!NOTE]
> Если для входа на портал Azure Stack используются учетные записи администратора или пользователя из доменов, отличных от того, который использовался для регистрации Azure Stack, имя домена, использованного для регистрации Azure Stack, необходимо добавить в URL-адрес портала. Например, если Azure Stack зарегистрирован с использованием домена fabrikam.onmicrosoft.com, а учетная запись для входа является admin@contoso.com, URL-адрес для входа на портал пользователей будет таким: https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

## <a name="next-steps"></a>Дополнительная информация

[Регистрация ASDK в Azure AD](asdk-register.md)
