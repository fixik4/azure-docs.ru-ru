---
title: Использование средства проверки Azure Stack | Документация Майкрософт
description: Сведения о том, как собирать файлы журналов для диагностики в Azure Stack.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: adshar
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: eca66bc2e737b0f9a9954cad21a446e82d753f84
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203817"
---
# <a name="validate-azure-stack-system-state"></a>Проверка состояния системы Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Для оператора Azure Stack очень важна возможность по запросу определять работоспособность и состояние вашей системы. Средство проверки Azure Stack (**Test-AzureStack**) представляет собой командлет PowerShell, который позволяет выполнить в системе ряд тестов для выявления сбоев. Обычно при обращении в службу поддержки клиентов служб Майкрософт (CSS) для устранения проблем вам будут предлагать запустить это средство из [привилегированной конечной точки (PEP)](azure-stack-privileged-endpoint.md). Получив сведения о работоспособности и состоянии всей системы, служба поддержки клиентов переходит к сбору и анализу подробных журналов по тем областям, где произошла ошибка, и в сотрудничестве с вами устраняет проблему.

## <a name="running-the-validation-tool-and-accessing-results"></a>Запуск средства проверки и доступ к результатам

Как уже упоминалось ранее, средство проверки запускается из привилегированной конечной точки. Каждый тест возвращает состояние **успеха или сбоя** в окне PowerShell. Кроме того, создается подробный отчет в формате HTML, который можно получить позднее при [сборе журналов](azure-stack-diagnostics.md). Ниже приводится описание полного процесса проверочного тестирования. 

1. Получите доступ к привилегированной конечной точке. Выполните следующие команды, чтобы создать сеанс подключения к привилегированной конечной точке:

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > Для доступа к привилегированной конечной точке на главном компьютере ASDK укажите значение AzS-ERCS01 для параметра -ComputerName.

2. Подключившись в привилегированной конечной точке, выполните следующую команду: 

   ```powershell
   Test-AzureStack
   ```

   Воспользуйтесь разделами [Рекомендации по настройке параметров](azure-stack-diagnostic-test.md#parameter-considerations) и [Примеры использования](azure-stack-diagnostic-test.md#use-case-examples), чтобы получить дополнительные сведения.

3. Если любой из тестов вернет состояние **FAIL** (Сбой), выполните такую команду:

   ```powershell
   Get-AzureStackLog -FilterByRole SeedRing -OutputSharePath "<path>" -OutputShareCredential $cred
   ```

   Этот командлет собирает журналы, созданные командлетом Test-AzureStack. Дополнительные сведения о журналах диагностики см. в статье [Средства диагностики Azure Stack](azure-stack-diagnostics.md). Нет необходимости собирать журналы или обращаться в службу поддержки пользователей, если тест возвращает состояние **WARN** (ПРЕДУПРЕЖДЕНИЕ).

4. Если вы запускаете средство проверки по просьбе службы поддержки пользователей, нужно будет передать сотруднику службы поддержки собранные журналы для продолжения работы по устранению проблемы.

## <a name="tests-available"></a>Доступные тесты

Средство проверки позволяет выполнить ряд тестов на системном уровне и проверить базовые облачные сценарии, чтобы составить представление о текущем состоянии системы и наличии проблем.

### <a name="cloud-infrastructure-tests"></a>Тесты облачной инфраструктуры

Эти тесты на уровне инфраструктуры выполняются без существенного влияния на работу системы и предоставляют сведения о различных системных компонентах и функциях. Сейчас тесты сгруппированы в следующие категории:

| Категория тестов                                        | Аргумент для параметров -Include (Включить) и -Ignore (Игнорировать) |
| :--------------------------------------------------- | :-------------------------------- |
| Сводка по оповещениям Azure Stack                            | AzsAlertSummary                   |
| Сводка по доступу к общему ресурсу резервных копий Azure Stack       | AzsBackupShareAccessibility       |
| Сводка по плоскости управления Azure Stack                    | AzsControlPlane                   |
| Сводка по Защитнику Azure Stack                         | AzsDefenderSummary                |
| Сводка по встроенному ПО для инфраструктуры размещения Azure Stack  | AzsHostingInfraFWSummary          |
| Сводка по инфраструктуре, размещающей облако Azure Stack     | AzsHostingInfraSummary            |
| Использование инфраструктуры, размещающей облако Azure Stack | AzsHostingInfraUtilization        |
| Производительность инфраструктуры Azure Stack                  | AzsInfraCapacity                  |
| Производительность инфраструктуры Azure Stack               | AzsInfraPerformance               |
| Сводка по роли инфраструктуры Azure Stack              | AzsInfraRoleSummary               |
| Сводка по обновлению Azure Stack                           | AzsInfraUpdateSummary             |
| Сводка по API и порталу Azure Stack                   | AzsPortalAPISummary               |
| События виртуальной машины для единицы масштабирования Azure Stack                     | AzsScaleUnitEvents                |
| Ресурсы виртуальной машины для единицы масштабирования Azure Stack                  | AzsScaleUnitResources             |
| Сводка по проверке SDN для Azure Stack                   | AzsSDNValidation                  |
| Сводка по роли Service Fabric для Azure Stack              | AzsSFRoleSummary                  |
| Сводка по BMC для Azure Stack                              | AzsStampBMCSummary                |
| Сводка по службам хранилища Azure Stack                 | AzsStorageSvcsSummary             |
| Сводка по хранилищу SQL для Azure Stack                        | AzsStoreSummary                   |
| Сводка по размещению виртуальных машин для Azure Stack                     | AzsVmPlacement                    |

### <a name="cloud-scenario-tests"></a>Тесты облачных сценариев

Помимо вышеперечисленных тестов инфраструктуры, вы можете запускать тесты облачных сценариев для проверки функциональности разных компонентов инфраструктуры. Для выполнения этих тестов требуются учетные данные администратора облака, так как они включают развертывание ресурсов. 
    > [!NOTE]
    > Currently you cannot run cloud scenario tests using Active Directory Federated Services (AD FS) credentials. 

Средство проверки умеет проверять следующие облачные сценарии:
- создание группы ресурсов;   
- создание плана;              
- создание предложения;            
- создание учетной записи хранения;   
- создание виртуальной машины; 
- операции с хранилищем BLOB-объектов;   
- операции с хранилищем очередей;  
- операции с хранилищем таблиц.  

## <a name="parameter-considerations"></a>Рекомендации по настройке параметров

- Параметр **List** (Список) позволяет отобразить все доступные категории тестов.

- Параметры **Include** (Включить) и **Ignore** (Пропустить) можно применять для включения или исключения определенных категорий тестов. В разделе [Доступные тесты](azure-stack-diagnostic-test.md#tests-available) вы найдете дополнительные сведения о сокращениях, которые можно использовать с этими аргументами.

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- В одном из тестов облачных сценариев выполняется развертывание виртуальной машины клиента. Чтобы отключить его, укажите параметр **DoNotDeployTenantVm**. 

- Параметр **ServiceAdminCredential** является обязательным для выполнения тестов облачных сценариев, как описано в разделе [Примеры использования](azure-stack-diagnostic-test.md#use-case-examples).

- Параметры **BackupSharePath** и **BackupShareCredential** используются при тестировании параметров резервного копирования инфраструктуры, как показано в разделе [Примеры использования](azure-stack-diagnostic-test.md#use-case-examples).

- Средство проверки также поддерживает стандартные параметры PowerShell: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable и OutVariable. См. дополнительные сведения об [общих параметрах](https://go.microsoft.com/fwlink/?LinkID=113216).  

## <a name="use-case-examples"></a>Примеры использования

### <a name="run-validation-without-cloud-scenarios"></a>Запуск проверки без облачных сценариев

Запустите средство проверки без параметра **ServiceAdminCredential**, чтобы пропустить тесты облачных сценариев: 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>Запуск проверки с облачными сценариями

Если средство проверки запущено с параметром **ServiceAdminCredentials**, по умолчанию оно выполняет тесты облачных сценариев: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

Если вы хотите выполнить ТОЛЬКО тесты облачных сценариев без остальных тестов, укажите параметр **Include**: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

Имя администратора облака следует указывать в формате имени участника-пользователя: serviceadmin@contoso.onmicrosoft.com (Azure AD). При появлении запроса введите пароль учетной записи администратора облака.

### <a name="run-validation-tool-to-test-system-readiness-before-installing-update-or-hotfix"></a>Запуск средства проверки для тестирования готовности системы перед установкой обновления или исправления

Прежде чем устанавливать обновление или исправление, следует запустить средство проверки для тестирования состояния Azure Stack:

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>Запуск средства проверки для тестирования параметров резервного копирования инфраструктуры

*Перед* настройкой резервного копирования инфраструктуры вы можете протестировать путь к общему ресурсу резервных копий и учетные данные с помощью теста **AzsBackupShareAccessibility**: 

  ```powershell
  New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
  ```

*После* настройки резервного копирования вы можете выполнить команду **AzsBackupShareAccessibility** для проверки доступа к общему ресурсу из ERCS.

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

Чтобы проверить новые учетные данные для настроенного общего ресурса резервных копий, выполните следующую команду: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```



## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о средствах диагностики Azure Stack и ведении журналов проблем см. в статье [Средства диагностики Azure Stack](azure-stack-diagnostics.md).

Дополнительные сведения об устранении неполадок см. в статье [Устранение неполадок, связанных с Microsoft Azure Stack](azure-stack-troubleshooting.md).