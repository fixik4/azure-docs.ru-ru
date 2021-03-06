---
title: Установка PowerShell для Azure Stack | Документация Майкрософт
description: Узнайте, как установить PowerShell для Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/08/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 4e623c6a2423d2e61334932d0c40f05e548d3c38
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58109871"
---
# <a name="install-powershell-for-azure-stack"></a>Установка PowerShell для Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Для работы с облаком необходимо установить модули PowerShell, совместимые с Azure Stack. Совместимость обеспечивается с помощью функции *профилей API*.

Профили API позволяют управлять различиями между версиями Azure и Azure Stack. Профиль версии API — это набор модулей Azure Resource Manager PowerShell с определенными версиями API. Каждая облачная платформа имеет набор поддерживаемых профилей версий API. К примеру, Azure Stack поддерживает определенную версию профиля, например **2018-03-01-hybrid**. При установке профиля устанавливается набор модулей Azure Resource Manager PowerShell, которые соответствуют выбранному профилю.

Вы можете установить совместимые модули PowerShell для Azure Stack в подключенном, частично подключенном к Интернету или в сценарии без подключения. Из этой статьи можно узнать подробные инструкции по установке PowerShell для Azure Stack в приведенных ниже случаях.

## <a name="1-verify-your-prerequisites"></a>1. Проверка необходимых компонентов

Перед началом работы с Azure Stack и PowerShell необходимо убедится в выполнении следующих условий:

- **PowerShell версии 5.0**. Чтобы проверить используемую версию, выполните **$PSVersionTable.PSVersion** и сравните номер **основной** версии. Если у вас нет PowerShell 5.0, следуйте инструкциям из раздела [Обновление существующей версии Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]
  > Для PowerShell 5.0 требуется компьютер с ОС Windows.

- **Откройте командную строку PowerShell с повышенными привилегиями**.
  Запуск PowerShell следует выполнять с правами администратора.

- **Доступ к коллекции PowerShell**. Вам потребуется доступ к [коллекции PowerShell](https://www.powershellgallery.com). Коллекция является центральным репозиторием содержимого PowerShell. Модуль **PowerShellGet** содержит командлеты для обнаружения, установки, обновления и публикации артефактов PowerShell, таких как модули, ресурсы DSC, возможности ролей и скрипты из коллекции PowerShell и других частных репозиториев. Если PowerShell используется в автономном режиме, необходимо извлечь ресурсы с компьютера с подключением к Интернету и сохранить их в доступном на отключенном компьютере месте.


## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. Проверка доступности коллекции PowerShell

Проверьте, зарегистрирована ли коллекция PSGallery в качестве репозитория.

> [!Note]  
> На этом шаге необходим доступ к Интернету.

Откройте командную строку PowerShell с повышенными привилегиями и выполните следующие командлеты:

```PowerShell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
```

Если репозиторий не зарегистрирован, откройте сеанс Windows PowerShell с повышенными привилегиями и выполните в нем следующую команду.

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Удалите существующие версии модулей PowerShell для Azure Stack

Прежде чем устанавливать нужную версию, обязательно удалите все установленные ранее модули PowerShell AzureRM для Azure Stack. Удалить их можно с помощью следующих двух способов.

1. Чтобы удалить имеющиеся модули PowerShell AzureRM, закройте все активные сеансы PowerShell и выполните следующие командлеты:

    ```PowerShell
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose
    ```
    Если возникает ошибка, например The module is already in use (Модуль уже используется), закройте сеансы PowerShell, которые используют модули, и повторно запустите приведенный выше скрипт.

2. Удалите все папки, имена которых начинаются с `Azure` или `Azs.`, из папок `C:\Program Files\WindowsPowerShell\Modules` и `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules`. При удалении этих папок удаляются все установленные модули PowerShell.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. С подключением. Установка PowerShell для Azure Stack с подключением к Интернету

Для работы с Azure Stack 1808 или более поздней версии требуется профиль версии API **2018-03-01-hybrid**. Чтобы получить доступ к профилю, необходимо установить модуль **AzureRM.Bootstrapper**. В дополнение к модулям AzureRM нужно установить модули PowerShell, предназначенные для Azure Stack. Требуемый профиль версии API и модули PowerShell Azure Stack будут зависеть от выполняемой версии Azure Stack.

Установка происходит в три этапа.

1. Установка PowerShell для Azure Stack в зависимости от версии Azure Stack
2. Включение дополнительных возможностей хранилища
3. Подтверждение установки PowerShell

### <a name="install-azure-stack-powershell"></a>Установка PowerShell для Azure Stack

Выполните следующий скрипт PowerShell, чтобы установить эти модули на рабочей станции разработки:

- Azure Stack 1901 и последующих версий.

    ```PowerShell
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Install-Module AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.0
    ```

    > [!Note]  
    > Модуль Azure Stack версии 1.7.0 является критическим изменением выпуска. Чтобы выполнить миграцию из Azure Stack 1.6.0, см. руководство по миграции, указанное [здесь](https://aka.ms/azspshmigration170).
    > Модуль AzureRm версии 2.4.0 включает критическое изменение для командлета Remove-AzureRmStorageAccount. Для удаления учетной записи хранения без подтверждения этот командлет ожидает параметр -Force.
- Azure Stack 1811.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

- Azure Stack 1810 и прежних версий.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

> [!Note]  
> Чтобы обновить Azure PowerShell с версии **2017-03-09-profile** до версии **2018-03-01-hybrid**, см. руководство по миграции [здесь](https://github.com/azure/azure-powershell/blob/AzureRM/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

### <a name="enable-additional-storage-features"></a>Включение дополнительных возможностей хранилища

Чтобы использовать дополнительные компоненты хранилища (упомянутые в разделе "С подключением"), также загрузите и установите следующие пакеты.

```PowerShell
# Install the Azure.Storage module version 4.5.0
Install-Module -Name Azure.Storage -RequiredVersion 4.5.0 -Force -AllowClobber

# Install the AzureRm.Storage module version 5.0.4
Install-Module -Name AzureRM.Storage -RequiredVersion 5.0.4 -Force -AllowClobber

# Remove incompatible storage module installed by AzureRM.Storage
Uninstall-Module Azure.Storage -RequiredVersion 4.6.1 -Force

# Load the modules explicitly specifying the versions
Import-Module -Name Azure.Storage -RequiredVersion 4.5.0
Import-Module -Name AzureRM.Storage -RequiredVersion 5.0.4
```

### <a name="confirm-the-installation-of-powershell"></a>Подтверждение установки PowerShell

Подтвердите установку, выполнив следующую команду:

```PowerShell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

Если установка выполнена успешно, в выходных данных указываются модули AzureRM и AzureStack.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Без подключения. Установка PowerShell без подключения к Интернету

В автономном режиме вам следует сначала скачать модули PowerShell на компьютер с подключением к Интернету, а затем перенести их туда, где будет устанавливаться Пакет средств разработки Azure Stack.

Войдите на компьютер с подключением к Интернету и выполните следующие сценарии для скачивания пакетов Azure Resource Manager и AzureStack в зависимости от используемой версии Azure Stack.

Установка состоит из четырех шагов.

1. Установка PowerShell для Azure Stack, подключенной к компьютеру
2. Включение дополнительных возможностей хранилища
3. Транспортировка пакетов PowerShell для отключенной рабочей станции
4. Подтверждение установки PowerShell


### <a name="install-azure-stack-powershell"></a>Установка PowerShell для Azure Stack

- Azure Stack 1901 или более поздняя версия.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.4.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.0
    ```

    > [!Note]  
    > Модуль Azure Stack версии 1.7.0 является критическим изменением. Чтобы выполнить миграцию из Azure Stack 1.6.0, см. руководство по миграции [здесь](https://github.com/Azure/azure-powershell/tree/AzureRM/documentation/migration-guides/Stack).


  - Azure Stack 1811 или более ранней версии.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.6.0
    ```

  - Azure Stack 1809 или более ранней версии.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ```

    > [!NOTE]
    > На компьютерах без подключения к Интернету советуем выполнить приведенный ниже командлет для отключения сбора данных телеметрии. Без выключения сбора данных телеметрии вы можете ощутить снижение производительности командлетов. Это относится только к компьютерам без подключения к Интернету.
    > ```PowerShell
    > Disable-AzureRmDataCollection
    > ```

### <a name="enable-additional-storage-features"></a>Включение дополнительных возможностей хранилища

Чтобы использовать дополнительные компоненты хранилища (упомянутые в разделе "С подключением"), также загрузите и установите следующие пакеты.

```PowerShell
$Path = "<Path that is used to save the packages>"
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name Azure.Storage -Path $Path -Force -RequiredVersion 4.5.0
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRm.Storage -Path $Path -Force -RequiredVersion 5.0.4
```

### <a name="add-your-packages-to-your-workstation"></a>Добавление пакетов на рабочую станцию

1. Скопируйте скачанные пакеты на USB-устройство.

2. Войдите на отключенную рабочую станцию и скопируйте пакеты с USB-устройства в нужное расположение на ней.

3. Теперь зарегистрируйте это расположение в качестве репозитория по умолчанию и установите из этого репозитория модули AzureRM и AzureStack:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

### <a name="confirm-the-installation-of-powershell"></a>Подтверждение установки PowerShell

Подтвердите установку, выполнив следующую команду:

```PowerShell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. Настройка PowerShell для использования прокси-сервера

Если для доступа к Интернету требуется прокси-сервер, необходимо сначала настроить PowerShell для использования имеющегося прокси-сервера.

1. Откройте командную строку PowerShell с повышенными привилегиями.
2. Выполните следующие команды:

   ```PowerShell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Дополнительная информация

 - [Download Azure Stack tools from GitHub](azure-stack-powershell-download.md) (Скачивание средств Azure Stack из GitHub).
 - [Configure the Azure Stack user's PowerShell environment](user/azure-stack-powershell-configure-user.md) (Настройка пользовательской среды PowerShell в Azure Stack).
 - [Configure the Azure Stack operator's PowerShell environment](azure-stack-powershell-configure-admin.md) (Настройка среды PowerShell для оператора Azure Stack).
 - [Manage API version profiles in Azure Stack](user/azure-stack-version-profiles.md) (Управление профилями версий API в Azure Stack).
