---
title: Включение резервного копирования для Azure Stack с помощью PowerShell | Документация Майкрософт
description: Включите службу резервного копирования инфраструктуры с помощью Windows PowerShell для восстановления Azure Stack в случае сбоя.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 03/14/2019
ms.openlocfilehash: 773e600577b35019b8a3619c7eec3e93b77a4382
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58085802"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Включение резервного копирования для Azure Stack с помощью PowerShell

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Включите службу "Резервное копирование инфраструктуры" с помощью Windows PowerShell, чтобы периодически создавать резервные копии:
 - внутренней службы идентификации и корневого сертификата;
 - планов, предложений и подписок пользователей;
 - квота сетевого пользователя, хранилища и вычисления;
 - секреты хранилища ключей пользователя;
 - ролей и политик RBAC пользователей.
 - пользовательские учетные записи хранения.

Чтобы включить резервное копирование, начать его и получить сведения о результатах резервного копирования через конечную точку управления оператором, вы можете использовать командлеты PowerShell.

## <a name="prepare-powershell-environment"></a>Подготовка среды PowerShell

Инструкции по настройке среды PowerShell приведены в статье [Установка PowerShell для Azure Stack](azure-stack-powershell-install.md). Сведения о том, как войти в Azure Stack, см. в статье [Настройка среды оператора и вход в Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Предоставление прав на совместное использование резервной копии, учетных данных и ключа шифрования для включения резервного копирования

В том же сеансе PowerShell измените следующий сценарий PowerShell, добавив переменные среды. Выполните обновленный сценарий, чтобы предоставить службе архивации инфраструктуры права на совместное использование учетных данных и ключа шифрования.

| Переменная        | ОПИСАНИЕ   |
|---              |---                                        |
| $username       | Введите **имя пользователя**, указав домен и имя пользователя для расположения общего диска. У этого пользователя должны быть достаточные права на чтение и запись файлов. Например, `Contoso\backupshareuser`. |
| $password       | Введите **пароль** пользователя. |
| $sharepath      | Введите путь к **расположению хранилища резервных копий**. В качестве пути к общей папке на отдельном устройстве нужно использовать UNC-строку. UNC-строка указывает расположение ресурсов, например общих файлов или устройств. Чтобы обеспечить доступность резервных копий, устройство должно находиться в отдельном расположении. |
| $frequencyInHours | Частота, измеряемая в часах, определяет, как часто создаются резервные копии. По умолчанию используется значение 12. Планировщик поддерживает число, находящееся в диапазоне межу 4 и 12.|
| $retentionPeriodInDays | Период хранения в днях определяет, сколько дней резервные копии будут храниться во внешнем расположении. По умолчанию используется значение 7. Планировщик поддерживает число, находящееся в диапазоне межу 2 и 14. Резервные копии, период хранения которых превысил допустимое значение, автоматически удаляются из внешнего расположения.|
| $encryptioncertpath | Применяется к версии 1901 и более поздним.  Этот параметр доступен в модуле Azure Stack версии 1.7 и более поздних. Путь к сертификату шифрования указывает путь к CER-файлу с помощью открытого ключа, используемого для шифрования данных. |
| $encryptionkey | Применяется к сборке 1811 и более ранних версий. Этот параметр доступен в модуле Azure Stack версии 1.6 и более ранних. Ключ, используемый при шифровании данных. Чтобы создать ключ, используйте командлет [New-AzsEncryptionKeyBase64](https://docs.microsoft.com/en-us/powershell/module/azs.backup.admin/new-azsencryptionkeybase64). |
|     |     |

### <a name="enable-backup-on-1901-and-beyond-using-certificate"></a>Включение резервного копирования в версии 1901 и более поздних с использованием сертификата
```powershell
    # Example username:
    $username = "domain\backupadmin"
 
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString

    # Create a self-signed certificate using New-SelfSignedCertificate, export the public key portion and save it locally.

    $cert = New-SelfSignedCertificate `
        -DnsName "www.contoso.com" `
        -CertStoreLocation "cert:\LocalMachine\My" 

    New-Item -Path "C:\" -Name "Certs" -ItemType "Directory" 

    #make sure to export the PFX format of the certificate with the public and private keys and then delete the certifcate from the local certificate store of the machine where you created the certificate
    
    Export-Certificate `
        -Cert $cert `
        -FilePath c:\certs\AzSIBCCert.cer 

    # Set the backup settings with the name, password, share, and CER certificate file.
    Set-AzsBackupConfiguration -BackupShare $sharepath -Username $username -Password $password -EncryptionCertPath "c:\temp\cert.cer"
```
### <a name="enable-backup-on-1811-or-earlier-using-certificate"></a>Включение резервного копирования в версии 1811 и более ранних с использованием сертификата
```powershell
    # Example username:
    $username = "domain\backupadmin"
 
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString

    # Create a self-signed certificate using New-SelfSignedCertificate, export the public key portion and save it locally.

    $key = New-AzsEncryptionKeyBase64
    $Securekey = ConvertTo-SecureString -String ($key) -AsPlainText -Force

    # Set the backup settings with the name, password, share, and CER certificate file.
    Set-AzsBackupConfiguration -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $Securekey
```

   
##  <a name="confirm-backup-settings"></a>Подтверждение параметров архивации

В этом же сеансе PowerShell выполните следующие команды:

   ```powershell
    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName
   ```

В результате должно отобразиться следующее.

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
   ```

## <a name="update-backup-settings"></a>Обновление резервного набора данных
В этом же сеансе PowerShell можно обновить значения по умолчанию для срока хранения и частоту резервного копирования. 

   ```powershell
    #Set the backup frequency and retention period values.
    $frequencyInHours = 10
    $retentionPeriodInDays = 5

    Set-AzsBackupConfiguration -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays

    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

В результате должно отобразиться следующее.

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

### <a name="azure-stack-powershell"></a>PowerShell для Azure Stack 
Set-AzsBackupConfiguration — это командлет PowerShell для настройки резервного копирования инфраструктуры. В предыдущих выпусках этим командлетом был Set-AzsBackupShare. Этот командлет требует наличия сертификата. Если резервное копирование инфраструктуры настроено с помощью ключа шифрования, вы не сможете обновить ключ шифрования или просмотреть свойства. Вам нужно использовать администратора PowerShell версии 1.6. 

Если резервное копирование инфраструктуры настроено перед обновлением до версии 1901, вы можете использовать версию 1.6 администратора PowerShell, чтобы настроить и просмотреть ключ шифрования. Версия 1.6 не позволит вам обновить ключ шифрования до файла сертификата.
Дополнительные сведения об установке правильной версии модуля см. в статье [Установка PowerShell для Azure Stack](azure-stack-powershell-install.md). 


## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о резервном копировании см. в статье [Резервное копирование для Azure Stack](azure-stack-backup-back-up-azure-stack.md)

Дополнительные сведения о подтверждении завершения резервного копирования см. в [этой статье](azure-stack-backup-back-up-azure-stack.md)
