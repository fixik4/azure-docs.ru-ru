---
title: Проверка регистрации Azure для Azure Stack | Документация Майкрософт
description: Применение средства проверки готовности Azure Stack для проверки регистрации Azure.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: a777fc1d9052eb58bbebd319fe6cc7f42a09cb9a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403618"
---
# <a name="validate-azure-registration"></a>Проверка регистрации в Azure

Средство проверки готовности Azure Stack (**AzsReadinessChecker**) позволяет убедиться, что ваша подписка Azure готова к использованию Azure Stack. Проверьте регистрацию перед тем, как развертывать Azure Stack. Средство проверки готовности позволяет проверить следующее:

- Используемая вами подписка Azure является поддерживаемой. Подписки должны быть типа "Поставщик облачных служб" или "Соглашение Enterprise".
- Учетная запись, используемая для регистрации подписки в Azure, может войти в Azure и является владельцем подписки.

Дополнительные сведения о регистрации Azure Stack см. в [этой статье](azure-stack-registration.md).

## <a name="get-the-readiness-checker-tool"></a>Получение средства проверки готовности

Скачайте последнюю версию **AzsReadinessChecker** из [коллекции PowerShell](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Предварительные требования

Требуются следующие необходимые компоненты:

**Компьютер, на котором запускается это средство:**

- Необходимо установить Windows 10 или Windows Server 2016 и обеспечить подключение к Интернету.
- Необходимо установить PowerShell 5.1 или более поздней версии. Чтобы проверить используемую версию, выполните следующий командлет PowerShell и проверьте значения **Major** (основной номер версии) и **Minor** (дополнительный номер версии):  

  ```powershell
  $PSVersionTable.PSVersion
  ```

- [PowerShell для Azure Stack](azure-stack-powershell-install.md).
- Последняя версия средства [проверки готовности Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker).  

**В среде Azure Active Directory:**

- Определите имя пользователя и пароль для учетной записи, которая является владельцем подписки Azure, которую вы будете использовать с Azure Stack.  
- Определите идентификатор подписки Azure, которая будет использоваться.
- Задайте параметр **AzureEnvironment**, который будет использоваться. Поддерживаемые значения для параметра имени среды: **AzureCloud**, **AzureChinaCloud** или **AzureUSGovernment** (в зависимости от используемой подписки Azure).

## <a name="steps-to-validate-azure-registration"></a>Этапы проверки регистрации в Azure

1. На компьютере, который соответствует всем предварительным требованиям, откройте командную строку PowerShell с повышенными правами и выполните следующую команду, чтобы установить **AzsReadinessChecker**:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. В командной строке PowerShell выполните команду ниже, чтобы задать `$registrationCredential` в качестве учетной записи с правами владельца подписки. Замените `subscriptionowner@contoso.onmicrosoft.com` именами учетной записи и клиента:

   ```powershell
   $registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"
   ```

   > [!NOTE]
   > Как поставщик служб шифрования при использовании общих служб или подписки IUR вы должны указать учетные данные пользователя из этого соответствующего клиента AAD. Как правило, имя будет выглядеть примерно так: `subscriptionowner@iurcontoso.onmicrosoft.com`. Этот пользователь должен иметь соответствующие учетные данные, как описано выше.

3. В командной строке PowerShell выполните команду ниже, чтобы задать `$subscriptionID` в качестве используемой подписки Azure. Замените `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` фактическим идентификатором подписки:

   ```powershell
   $subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
   ```

4. В командной строке PowerShell выполните приведенную ниже команду, чтобы начать проверку подписки:

   - Укажите для `AzureEnvironment` значение **AzureCloud**, **AzureGermanCloud** или **AzureChinaCloud**.  
   - Укажите имя администратора Azure Active Directory и имя клиента Azure Active Directory.

   ```powershell
   Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID
   ```

5. Когда средство завершит работу, просмотрите выходные данные. Убедитесь, что состояние соответствует требованиям ко входу в систему и требованиям к регистрации. При успешном завершении проверки отобразится примерно такой результат:

   ```shell
   Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
   Checking Registration Requirements: OK
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsRegistrationValidation Completed
   ```

## <a name="report-and-log-file"></a>Файлы отчета и журнала

При каждом запуске проверки все результаты сохраняются в файлах **AzsReadinessChecker.log** и **AzsReadinessCheckerReport.json**. Расположение этих файлов указывается в PowerShell вместе с результатами проверки.

Эти файлы помогут передать сведения о состоянии проверки другим заинтересованным лицам перед развертыванием Azure Stack или для исследования проблем, обнаруженных при проверке. В обоих файлах сохраняются результаты каждой очередной проверки. В отчете содержатся подтверждения команды развертывания по конфигурации удостоверений. Файл журнала поможет командам развертывания или поддержки диагностировать проблемы с проверкой.

По умолчанию оба файла сохраняются в расположении **C:\Users\<имя_пользователя>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json**.  

- Чтобы задать другое расположение отчетов, при запуске проверки можно указать в конце командной строки параметр **-OutputPath** ***&lt;путь&gt;***.
- Укажите параметр **-CleanReport** в конце команды, чтобы удалить из файла **AzsReadinessCheckerReport.json** сведения о предыдущих запусках средства.

Дополнительные сведения об отчетах проверки Azure Stack можно найти [здесь](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Ошибки при проверке

Если проверка завершается ошибкой, сведения о сбое отображаются в окне PowerShell. Кроме того, сведения записываются в файл **AzsReadinessChecker.log**.

В следующих примерах приведены некоторые рекомендации по устранению распространенных ошибок проверки.

### <a name="user-must-be-an-owner-of-the-subscription"></a>Пользователь должен быть владельцем подписки

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Причина** — текущая учетная запись не имеет прав администратора подписки Azure.

**Разрешение**. Используйте учетную запись администратора подписки Azure, которой будет выставлен счет за использование развертывания Azure Stack.

### <a name="expired-or-temporary-password"></a>Пароль с истекшим сроком действия или временный пароль

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription [subscription ID] using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change Password.
Trace ID: [Trace ID]
Correlation ID: [Correlation ID]
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Причина** — вход с этой учетной записью невозможен, так как истек срок действия пароля или используется временный пароль.

**Решение** — выполните в PowerShell приведенную ниже команду и следуйте инструкциям на экране, чтобы сбросить пароль.

```powershell
Login-AzureRMAccount
```

Также вы можете войти на [портал Azure](https://portal.azure.com) как владелец учетной записи. В таком случае пользователь должен будет сменить пароль.

### <a name="unknown-user-type"></a>Неизвестный тип пользователя  

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription <subscription ID> using <account> failed with unknown_user_type: Unknown User Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Причина** — вход с этой учетной записью в указанную среду Azure Active Directory невозможен. В нашем примере параметр **AzureChinaCloud** имеет значение **AzureEnvironment**.  

**Решение** — убедитесь, что эта учетная запись существует в указанном окружении Azure. Выполните в PowerShell следующую команду, чтобы проверить допустимость учетной записи для указанной среды:

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

## <a name="next-steps"></a>Дальнейшие действия

- [Проверка удостоверения Azure](azure-stack-validate-identity.md)
- [Просмотр отчета о готовности](azure-stack-validation-report.md)
- [Общие рекомендации по интеграции Azure Stack](azure-stack-datacenter-integration.md)
