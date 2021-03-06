---
title: Планирование развертывания службы файлов Azure | Документы Майкрософт
description: Узнайте, что необходимо учесть при планировании развертывания службы файлов Azure.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 69ca9474c613752b98efa6bb236919508a2fe430
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753696"
---
# <a name="planning-for-an-azure-files-deployment"></a>Планирование развертывания службы файлов Azure
[Служба файлов Azure](storage-files-introduction.md) предлагает полностью управляемые общие файловые ресурсы в облаке, доступ к которым можно получить с помощью стандартного отраслевого протокола SMB. Так как служба файлов Azure является полностью управляемой, ее развертывание в рабочих сценариях гораздо проще развертывания файлового сервера или устройства NAS и управления им. В этой статье рассматриваются аспекты, которые следует учитывать при развертывании файлового ресурса Azure для использования в рабочей среде организации.

## <a name="management-concepts"></a>Основные принципы управления
 На схеме ниже показаны компоненты управления службой файлов Azure.

![Структура файла](./media/storage-files-introduction/files-concepts.png)

* **Учетная запись хранения**. Весь доступ к хранилищу Azure осуществляется с помощью учетной записи хранения. Сведения о емкости учетной записи хранения см. в статье [о целевых показателях масштабируемости и производительности службы хранилища Azure](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **Общая папка**. Общая папка хранилища файлов представляет собой общую папку с файлами в Azure, использующую протокол SMB. Все каталоги и файлы должны быть созданы в родительской общей папке. Учетная запись может содержать любое количество общих папок, а общая папка может содержать любое количество файлов, общий размер которых не превышает 5 ТБ.

* **Каталог**. Необязательная иерархия каталогов.

* **Файл**. Файл в общей папке. Файлы могут иметь размер до 1 ТБ.

* **Формат URL-адреса**. В запросах к общей папке Azure, выполненных с помощью протокола REST службы файлов, к файлам можно обращаться, используя URL-адрес в следующем формате:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/<file>
    ```

## <a name="data-access-method"></a>Метод доступа к данным
Служба файлов Azure предлагает два встроенных удобных метода доступа к данным, которые можно использовать по отдельности или в сочетании друг с другом.

1. **Прямой доступ к облаку**. Общую папку Azure можно вставить в ОС [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) или [Linux](storage-how-to-use-files-linux.md) с помощью протокола SMB или REST API службы файлов. При использовании SMB операции чтения и записи выполняются непосредственно в файловом ресурсе в Azure. Для подключения к виртуальной машине в Azure клиент SMB в операционной системе должен поддерживать по меньшей мере протокол SMB 2.1. Для локального подключения, например к рабочей станции пользователя, клиент SMB на рабочей станции должен поддерживать по меньшей мере протокол SMB 3.0 (с шифрованием). Кроме SMB, новые приложения или службы могут напрямую обращаться к файловому ресурсу посредством File REST, который является простым и масштабируемым API-интерфейсом для разработки программного обеспечения.
2. **Синхронизация файлов Azure** С помощью службы "Синхронизация файлов Azure" можно реплицировать файловые ресурсы на локальные серверы Windows Server или в Azure. Ваши пользователи могут получить доступ к файловому ресурсу через Windows Server, например через общий ресурс SMB или NFS. Это полезно в сценариях, когда доступ к данным и их изменения выполняются далеко за пределами центров обработки данных Azure, например как в сценарии с филиалом. Данные можно реплицировать между несколькими конечными точками Windows Server, например между несколькими филиалами. Наконец, данные могут быть размещены на нескольких уровнях в службе файлов Azure так, что они по-прежнему будут доступны через сервер, однако на сервере отсутствует полная копия данных. Данные можно легко извлечь при открытии пользователем.

В следующей таблице показано, как пользователи и приложения могут обращаться к файловому ресурсу Azure.

| | Прямой доступ к облаку | Служба синхронизации файлов Azure |
|------------------------|------------|-----------------|
| Какие протоколы необходимо использовать? | Служба файлов Azure поддерживает SMB 2.1, SMB 3.0 и File REST API. | Доступ к файловому ресурсу Azure с помощью любого поддерживаемого протокола в Windows Server (SMB, NFS, FTPS и т. д.) |  
| Где выполняется рабочая нагрузка? | **В Azure**. Служба "Файлы Azure" обеспечивает прямой доступ к данным. | **В локальной среде с медленным сетевым подключением**: клиенты Windows, Linux и macOS могут подключить локальную общую папку службы файлов Windows в качестве быстрого кэша общей папки Azure. |
| Какой уровень списков ACL требуется? | Уровень общего доступа и уровень файла. | Уровень общего доступа, уровень файла и уровень пользователя. |

## <a name="data-security"></a>Защита данных
Служба файлов Azure предлагает несколько встроенных возможностей для обеспечения безопасности данных.

* Поддержка шифрования для протоколов сетевой передачи: шифрование SMB 3.0 и REST службы файлов по HTTPS. По умолчанию: 
    * Клиенты, которые поддерживают шифрование SMB 3.0, отправляют и получают данные по зашифрованному каналу.
    * Клиенты, которые не поддерживают SMB 3.0 с шифрованием, могут взаимодействовать между ЦОД по протоколам SMB 2.1 или SMB 3.0 без шифрования. Клиенты SMB не могут взаимодействовать между ЦОД по протоколам SMB 2.1 или SMB 3.0 без шифрования.
    * Клиенты могут взаимодействовать через File REST по протоколу HTTP или HTTPS.
* Шифрование неактивных данных ([Технология шифрования службы хранилища Azure](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)). Шифрование службы хранилища (SSE) включено для всех учетных записей хранения. Неактивные данные шифруются с помощью полностью управляемых ключей. Шифрование неактивных данных не увеличивает затраты на хранение и не снижает производительность. 
* Необязательное требование к зашифрованным данным во время передачи: если выбран этот вариант, служба файлов Azure запретит доступ к данным по незашифрованным каналам. В частности, будут разрешены только подключения по протоколам HTTPS и SMB 3.0 с шифрованием. 

    > [!Important]  
    > Необходимость безопасной передачи данных приведет к сбою старых клиентов SMB, не поддерживающих взаимодействие с SMB 3.0 с шифрованием. Дополнительные сведения см. в статьях [Использование общей папки Azure в Windows](storage-how-to-use-files-windows.md), [Использование файлов Azure в Linux](storage-how-to-use-files-linux.md), [Подключение общей папки Azure через протокол SMB в macOS](storage-how-to-use-files-mac.md).

Для обеспечения максимальной безопасности настоятельно рекомендуется всегда включать шифрование данных при хранении и шифрование данных во время передачи при каждом использовании современных клиентов для доступа к данным. Например, если необходимо подключить общую папку в виртуальной машине с Windows Server 2008 R2, которая поддерживает только SMB 2.1, необходимо разрешить передачу незашифрованного трафика в хранилище, так как SMB 2.1 не поддерживает шифрование.

Если для доступа к файловому ресурсу Azure вы используете службу "Синхронизация файлов Azure", для синхронизации данных с серверами Windows Server всегда будут использоваться протоколы HTTPS и SMB 3.0 с шифрованием (независимо от того, нужно ли вам шифровать неактивные данные).

## <a name="file-share-performance-tiers"></a>Уровни производительности файлового ресурса
Файлы Azure поддерживают два уровня производительности: "Стандартный" и "Премиум".

* **Файловые ресурсы уровня "Стандартный"** поддерживаются переменными жесткими дисками (HDD), которые обеспечивают надежную производительность для рабочих нагрузок операций ввода-вывода. Они являются менее чувствительными к изменению производительности, такой как файловых ресурсов общего назначения и среды разработки и тестирования. Файловые ресурсы уровня "Стандартный" доступны только в модели выставления счетов с оплатой по мере использования.
* **Файловые ресурсы уровня "Премиум" (предварительная версия)** поддерживаются твердотельными накопителями (SSD), которые обеспечивают согласованную высокую производительность и низкую задержку в течение нескольких миллисекунд для большинства операций ввода-вывода и для рабочих нагрузок с большим числом операций ввода-вывода. Это делает их пригодными для широкого круга рабочих нагрузок, таких как базы данных, размещение веб-сайтов, среды разработки и т. д. Файловые ресурсы уровня "Премиум" доступны только в подготовленной модели выставления счетов.

### <a name="provisioned-shares"></a>Подготовленные ресурсы
Файловые ресурсы уровня "Премиум" предоставляются на основе зафиксированной пропорции ГиБ/операции ввода-вывода в секунду/пропускная способность. Для каждого подготовленного ГиБ ресурс будет выдаваться на одну пропускную способность операций ввода-вывода в секунду и 0,1 МиБ/с пропускной способности в условиях максимальной нагрузки на ресурс. Минимально допустимый уровень подготовки составляет 100 ГиБ с минимальным числом операций ввода-вывода в секунду и пропускной способностью. Размер ресурса может быть увеличен в любое время и уменьшен в любое время, но уменьшить ресурс можно только один раз в 24 часа с момента последнего увеличения.

Наилучшим возможным образом все ресурсы могут ограничиваться до трех операций ввода-вывода в секунду на ГиБ подготовленного хранилища в течение 60 минут или дольше в зависимости от размера ресурса. Новые ресурсы начинаются с полного пакетного кредита на основе подготовленной мощности.

| Подготовленная емкость | 100 ГиБ | 500 ГБ | 1 ТиБ | 5 ТиБ | 
|----------------------|---------|---------|-------|-------|
| Базовые показатели операций ввода-вывода в секунду | 100 | 500 | 1024 | 5,120 | 
| Ограничения пакета | 300 | 1500 | 3072 | 15,360 | 
| Пропускная способность | 110 МиБ/с | 150 МиБ/с | 202 МиБ/с | 612 МиБ/с |

## <a name="file-share-redundancy"></a>Избыточность файловых ресурсов
Служба файлов Azure поддерживает три варианта избыточности данных: локально избыточное хранилище (LRS), хранилище, избыточное в пределах зоны, (ZRS) и геоизбыточное хранилище (GRS). В следующих разделах описаны различия между разными вариантами обеспечения избыточности.

### <a name="locally-redundant-storage"></a>Локально избыточное хранилище
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Хранилище, избыточное в пределах зоны
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Геоизбыточное хранилище
> [!Warning]  
> Если вы используете общую папку Azure в качестве конечной точки облака в учетной записи хранения GRS, нет необходимости выполнять отработку отказа учетной записи хранения. Это приведет к прекращению синхронизации или даже к непредвиденной потере данных, если некоторые файлы недавно стали многоуровневыми. В случае потери региона Azure корпорация Майкрософт активирует отработку отказа учетной записи хранения способом, который совместим со службой "Синхронизация файлов Azure".

[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Модель роста данных
Сейчас максимальный размер общего файлового ресурса Azure — 5 ТиБ. Из-за этого ограничения при развертывании файлового ресурса Azure следует учитывать ожидаемый рост объема данных. 

Служба "Синхронизация файлов Azure" позволяет синхронизировать несколько файловых ресурсов Azure с одним файловым сервером Windows. В этом случае старые и очень большие файловые ресурсы, размещенные в локальных средах, можно ввести под контроль службы синхронизации файлов Azure. Дополнительные сведения см. в статье [Планирование развертывания службы файлов Azure](storage-files-planning.md).

## <a name="data-transfer-method"></a>Метод передачи данных
Существует множество простых вариантов массовой передачи данных из существующего файлового ресурса, такого как файловый ресурс в локальной среде, в службу файлов Azure. В число распространенных методов входят следующие (список не является исчерпывающим).

* **Синхронизация файлов Azure** Во время первой синхронизации между файловым ресурсом Azure (облачная конечная точка) и пространством имен каталогов Windows (серверная конечная точка) служба "Синхронизация файлов Azure" реплицирует все данные из существующей общей папки в службу "Файлы Azure".
* **[Служба импорта и экспорта Azure](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**. Служба "Импорт и экспорт Azure" позволяет безопасно переносить большие объемы данных в общую папку Azure, отправляя жесткие диски в центр обработки данных Azure. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)** Robocopy — это хорошо известное средство копирования, которое входит в состав Windows и Windows Server. Robocopy можно применять для передачи данных в службу файлов Azure путем локального подключения файлового ресурса и последующего использования подключенного расположения в качестве назначения в команде Robocopy.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)** AzCopy — это служебная программа командной строки. Она предназначена для копирования данных из службы файлов Azure, хранилища BLOB-объектов Azure и обратно с помощью простых команд, обеспечивающих оптимальную производительность. Эта программа доступна для Windows и Linux.

## <a name="next-steps"></a>Дополнительная информация
* [Planning for an Azure File Sync Deployment](storage-sync-files-planning.md) (Планирование развертывания службы синхронизации файлов Azure)
* [Deploying Azure Files](storage-files-deployment-guide.md) (Развертывание службы файлов Azure)
* [Развертывание службы синхронизации файлов Azure (предварительная версия)](storage-sync-files-deployment-guide.md)
