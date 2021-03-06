---
title: Устранение проблем с сертификатом для Azure Stack | Документация Майкрософт
description: Использование средства проверки готовности Azure Stack для просмотра и устранения проблем с сертификатом.
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
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/19/2018
ms.openlocfilehash: 009eb56621f7cd395c3d2eefb29b9fa624af888b
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57778201"
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Устранение распространенных проблем с сертификатами PKI Azure Stack

Сведения в этой статье помогут вам понять и устранить распространенные проблемы с сертификатами PKI Azure Stack. Вы можете обнаруживать проблемы, [проверяя сертификаты PKI Azure Stack](azure-stack-validate-pki-certs.md) с помощью средства проверки готовности Azure Stack. Средство проверяет, чтобы сертификаты соответствовали требованиям PKI к развертыванию и смене секретов Azure Stack, и записывает результаты в файл [report.json](azure-stack-validation-report.md).  

## <a name="pfx-encryption"></a>Шифрование PFX-файлов

**Ошибка**. Алгоритм шифрования PFX-файлов не соответствует TripleDES-SHA1.

**Исправление**. Экспортируйте PFX-файлы, используя шифрование **TripleDES-SHA1**. Это стандартный вариант для всех клиентов Windows 10 при экспорте из оснастки сертификатов или при использовании командлета `Export-PFXCertificate`.

## <a name="read-pfx"></a>Чтение PFX-файла

**Предупреждение.** Пароль защищает только конфиденциальные сведения в сертификате.  

**Исправление.** Экспортируйте PFX-файлы с необязательным параметром **Включить конфиденциальность сертификата**.  

**Сбой.** Недопустимый PFX-файл.  

**Исправление.** Повторно экспортируйте сертификат, выполнив действия, описанные в статье [Prepare Azure Stack PKI certificates for deployment](azure-stack-prepare-pki-certs.md) (Подготовка сертификатов PKI Azure Stack к развертыванию).

## <a name="signature-algorithm"></a>Алгоритм подписи

**Ошибка.** Алгоритм подписи — SHA1.

**Исправление.** Выполните шаги, описанные в статье о создании запроса на подпись сертификатов (CSR) Azure Stack, чтобы повторно создать такой запрос с использованием алгоритма подписи SHA256. Затем отправьте CSR в центр сертификации, чтобы повторно выдать сертификат.

## <a name="private-key"></a>Закрытый ключ

**Сбой.** Закрытый ключ отсутствует или не содержит атрибут локального компьютера.  

**Исправление.** Повторно экспортируйте сертификат из компьютера, создавшего CSR, выполнив инструкции из статьи [Подготовка сертификатов PKI Azure Stack для развертывания или смены секретов](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment). Эти действия включают экспорт из хранилища сертификатов на локальном компьютере.

## <a name="certificate-chain"></a>Цепочка сертификатов

**Сбой.** Цепочка сертификатов не полная.  

**Исправление.** Сертификаты должны содержать завершенную цепочку сертификатов. Повторно экспортируйте сертификат, выполнив действия, описанные в статье о [подготовке сертификатов PKI Azure Stack к развертыванию](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment), и выберите параметр **Включить по возможности все сертификаты в путь сертификации**.

## <a name="dns-names"></a>DNS-имена

**Сбой.** Параметр сертификата DNSNameList не содержит имя конечной точки службы Azure Stack или допустимое совпадение подстановочных знаков. Совпадения подстановочных знаков допустимы только для крайнего левого пространства имен DNS-имени. Например, _*. region.domain.com_ допустим только для *portal.region.domain.com*, но не для _*.table.region.domain.com_.

**Исправление**. Выполните шаги, описанные в статье о создании запроса на подпись сертификатов Azure Stack, чтобы повторно создать CSR с правильным DNS-именем для поддержки конечных точек Azure Stack. Повторно отправьте CSR в центр сертификации, а затем экспортируйте сертификат с компьютера, создавшего CSR, выполнив инструкции, описанные в статье о [подготовке сертификатов PKI Azure Stack к развертыванию](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment).  

## <a name="key-usage"></a>Использование ключа

**Ошибка.** В свойстве "Использование ключа" отсутствует атрибут цифровой подписи или шифрования ключей, либо в свойстве "Улучшенный ключ" отсутствует атрибут проверки подлинности сервера или проверки подлинности клиента.  

**Исправление.** Выполните инструкции, приведенные в статье о [создании запроса на подпись сертификатов Azure Stack](azure-stack-get-pki-certs.md), чтобы повторно создать CSR с правильными атрибутами использования ключа. Повторно отправьте запрос CSR в центр сертификации и убедитесь, что шаблон сертификата не перезаписывает свойство "Использование ключа" в запросе.

## <a name="key-size"></a>Размер ключа

**Ошибка.** Размер ключа меньше 2048.

**Исправление.** Выполните инструкции из статьи о [создании запроса на подпись сертификатов Azure Stack](azure-stack-get-pki-certs.md), чтобы повторно создать CSR с ключом правильной длины (2048), а затем снова отправьте CSR в центр сертификации.

## <a name="chain-order"></a>Порядок в цепочке

**Сбой.** Неверный порядок в цепочке сертификатов.  

**Исправление.** Повторно экспортируйте сертификат, выполнив действия, описанные в статье о [подготовке сертификатов PKI Azure Stack к развертыванию](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment), и выберите параметр **Включить по возможности все сертификаты в путь сертификации**. Убедитесь, что для экспорта выбран только конечный сертификат.

## <a name="other-certificates"></a>Другие сертификаты

**Сбой.** Пакет PFX содержит сертификаты, не являющиеся конечными или частью цепочки сертификатов.  

**Исправление.** Повторно экспортируйте сертификат, выполнив действия, описанные в статье о [подготовке сертификатов PKI Azure Stack к развертыванию](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment), и выберите параметр **Включить по возможности все сертификаты в путь сертификации**. Убедитесь, что для экспорта выбран только конечный сертификат.

## <a name="fix-common-packaging-issues"></a>Устранение распространенных неполадок с упаковкой

**AzsReadinessChecker** содержит вспомогательный командлет `Repair-AzsPfxCertificate`, который может импортировать и экспортировать PFX-файл для исправления типичных проблем с созданием пакетов, включая следующие:

- *Алгоритм шифрования PFX-файлов* не соответствует TripleDES-SHA1.
- Отсутствие атрибута локального компьютера в *закрытом ключе*.
- Незавершенная или неправильная *цепочка сертификатов*. Локальный компьютер должен содержать цепочки сертификатов, если их нет в пакете PFX.
- *Другие сертификаты*.

`Repair-AzsPfxCertificate` не подходит для случаев, когда нужно создать новый запрос CSR и повторно выпустить сертификат.

### <a name="prerequisites"></a>Предварительные требования

На компьютере, на котором запущено средство, должны быть установлены следующие обязательные компоненты:

- Необходимо установить Windows 10 или Windows Server 2016 и обеспечить подключение к Интернету.
- Необходимо установить PowerShell 5.1 или более поздней версии. Чтобы проверить используемую версию, выполните следующий командлет PowerShell и проверьте значения *Major* (основной номер версии) и *Minor* (дополнительный номер версии):

   ```powershell
   $PSVersionTable.PSVersion
   ```

- Необходимо настроить [PowerShell для использования с Azure Stack](azure-stack-powershell-install.md).
- Скачайте последнюю версию [средства проверки готовности Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker).

### <a name="import-and-export-an-existing-pfx-file"></a>Импорт и экспорт имеющегося PFX-файла

1. На компьютере, который соответствует всем предварительным требованиям, откройте командную строку PowerShell с правами администратора и выполните следующую команду, чтобы установить AzsReadinessChecker.

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. В командной строке PowerShell выполните указанный ниже командлет, чтобы задать пароль PFX-файла. Замените значение `PFXpassword` фактическим паролем.

   ```powershell
   $password = Read-Host -Prompt PFXpassword -AsSecureString
   ```

3. В командной строке PowerShell выполните следующую команду, чтобы экспортировать новый PFX-файл.

   - Для `-PfxPath` укажите путь к PFX-файлу, с которым вы работаете. В следующем примере используется путь `.\certificates\ssl.pfx`.
   - Для `-ExportPFXPath` укажите расположение и имя PFX-файла для экспорта. В следующем примере используется путь `.\certificates\ssl_new.pfx`.

   ```powershell
   Repair-AzsPfxCertificate -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
   ```  

4. После завершения работы средства проверьте выходные данные, чтобы убедиться в успешном выполнении:

   ```powershell
   Repair-AzsPfxCertificate v1.1809.1005.1 started.
   Starting Azure Stack Certificate Import/Export
   Importing PFX .\certificates\ssl.pfx into Local Machine Store
   Exporting certificate to .\certificates\ssl_new.pfx
   Export complete. Removing certificate from the local machine store.
   Removal complete.
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Repair-AzsPfxCertificate Completed
   ```

## <a name="next-steps"></a>Дополнительная информация

- [Смена секретов в Azure Stack](azure-stack-rotate-secrets.md)
