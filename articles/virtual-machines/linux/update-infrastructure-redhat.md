---
title: Инфраструктура обновления Red Hat Update Infrastructure | Документация Майкрософт
description: Узнайте об инфраструктуре обновления Red Hat Update Infrastructure для предоставляемых по запросу экземпляров Red Hat Enterprise Linux в Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 1/7/2019
ms.author: borisb
ms.openlocfilehash: 018ad851b223caa4017d544ca12e2a6397654205
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793930"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat Update Infrastructure для предоставляемых по запросу виртуальных машин Red Hat Enterprise Linux в Azure
 [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) позволяет поставщикам облачных служб (например, Azure) создавать зеркальные копии размещенного с помощью Red Hat содержимого репозитория, создавать пользовательские репозитории с содержимым для Azure и предоставлять пользовательским виртуальным машинам доступ к этому содержимому.

Образы Red Hat Enterprise Linux (RHEL) с оплатой по мере использования (PAYG) предварительно настроены для доступа к Azure RHUI. Никаких дополнительных настроек не требуется. Чтобы получить последние обновления, выполните `sudo yum update`, когда экземпляр RHEL будет готов. Плата за эту службу входит в стоимость программного обеспечения RHEL (PAYG).

Дополнительные сведения об образах RHEL в Azure, включая политики публикации и хранения, можно найти [здесь](./rhel-images.md).

Сведения о политиках поддержки Red Hat для всех версий RHEL можно найти на странице [о жизненных циклах выпусков Red Hat Enterprise Linux](https://access.redhat.com/support/policy/updates/errata).

## <a name="important-information-about-azure-rhui"></a>Важные сведения об Azure RHUI
* Сейчас Azure RHUI поддерживает только последний вспомогательный выпуск в каждом семействе RHEL (RHEL6 или RHEL7). Чтобы обновить экземпляр виртуальной машины RHEL, подключенный к RHUI, до версии с последним дополнительным номером, выполните команду `sudo yum update`.

    Например, если вы подготовили виртуальную машину на основе образа RHEL 7.4 (с оплатой по мере использования) и выполнили команду `sudo yum update`, то виртуальная машина обновится до версии RHEL 7.6 (последний дополнительный номер версии в семействе RHEL7).

    Чтобы избежать этого, необходимо создать собственный образ, как описано в статье [Подготовка виртуальной машины на основе Red Hat для Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Затем нужно подключить его к другой инфраструктуре обновления ([непосредственно к серверам доставки содержимого Red Hat](https://access.redhat.com/solutions/253273) или [серверу Red Hat Satellite](https://access.redhat.com/products/red-hat-satellite)).

* В стоимость образа RHEL (PAYG) входит плата за доступ к инфраструктуре RHUI, размещенной в Azure. Отмена регистрации виртуальной машины RHEL (PAYG) в инфраструктуре RHUI в Azure не приведет к ее преобразованию в виртуальную машину с использованием собственной лицензии (BYOL). В случае регистрации одной и той же виртуальной машины в другом источнике обновлений вы можете нести двойные _косвенные_ расходы. Первая плата будет взиматься за использование программного обеспечения Azure RHEL. Вторая плата будет взиматься за подписки Red Hat, которые были приобретены ранее. Если требуется использовать другую инфраструктуру обновлений (не инфраструктуру RHUI, размещенную в Azure), рекомендуется создать и развернуть собственные образы (с использованием собственной лицензии). Этот процесс описан в разделе [Подготовка виртуальной машины на основе Red Hat для Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Образы RHEL (SAP PAYG) в Azure (RHEL for SAP, RHEL for SAP HANA и RHEL for SAP Business Applications) подключаются к выделенным каналам RHUI, которые остаются на определенной версии RHEL с дополнительным номером, что требуется для сертификации SAP.

* Доступ к размещенной в Azure инфраструктуре RHUI могут получать виртуальные машины с IP-адресами в рамках [диапазонов IP-адресов центра обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653). Если весь трафик виртуальной машины перенаправляется через локальную сетевую инфраструктуру, может потребоваться настройка определяемых пользователем маршрутов, чтобы виртуальные машины RHEL (PAYG) могли получить доступ к инфраструктуре RHUI в Azure.

## <a name="rhel-eus-and-version-locking-rhel-vms"></a>Предложение RHEL EUS и фиксация версии виртуальных машин RHEL
Некоторым клиентам может потребоваться зафиксировать определенную дополнительную версию RHEL для своих виртуальных машин RHEL. Вы можете зафиксировать определенный дополнительный номер версии для виртуальной машины RHEL, изменив репозитории таким образом, чтобы они указывали на репозитории Extended Update Support (EUS). Ниже приведены инструкции по фиксации конкретного дополнительного номера версии для ВМ RHEL.

>[!NOTE]
> Это применяется только для версий RHEL, для которых доступна EUS. На момент написания этой статьи это версии RHEL 7.2–7.6. Дополнительные сведения можно найти на странице [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Жизненный цикл Red Hat Enterprise Linux).

1. Отключите репозитории без поддержки EUS:
    ```bash
    sudo yum --disablerepo='*' remove 'rhui-azure-rhel7'
    ```

1. Добавьте репозитории EUS:
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7-eus.config' install 'rhui-azure-rhel7-eus'
    ```

1. Заблокируйте переменную releasever:
    ```bash
    echo $(. /etc/os-release && echo $VERSION_ID) > /etc/yum/vars/releasever
    ```

    >[!NOTE]
    > Выполнив приведенные выше инструкции, вы зафиксируете текущий дополнительный номер версии в качестве дополнительного номера версии RHEL. Введите конкретный дополнительный номер версии, если вам нужно обновить систему и зафиксировать дополнительный номер версии, который не является последним. Например, команда `echo 7.5 > /etc/yum/vars/releasever` зафиксирует версию RHEL 7.5

1. Обновление виртуальной машины RHEL
    ```bash
    sudo yum update
    ```

## <a name="the-ips-for-the-rhui-content-delivery-servers"></a>IP-адреса для серверов доставки содержимого RHUI

Инфраструктура RHUI поддерживается во всех регионах, где доступны предоставляемые по запросу образы RHEL. Сейчас в этот список входят общедоступные регионы, указанные на странице [состояния Azure](https://azure.microsoft.com/status/), регионы, обслуживающие государственные организации США, и регионы Microsoft Azure — Германия.

При использовании конфигурации сети для усиленного ограничения доступа с виртуальных машин RHEL (PAYG) убедитесь, что для приведенных ниже IP-адресов разрешена операция `yum update`, в зависимости от используемой среды.


```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193
13.72.14.155
52.244.249.194

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="azure-rhui-infrastructure"></a>Инфраструктура Azure RHUI


### <a name="update-expired-rhui-client-certificate-on-a-vm"></a>Обновление сертификата клиента RHUI с истекшим сроком действия на виртуальной машине

Если вы используете одну из предыдущих версий образа виртуальной машины RHEL, например RHEL 7.4 (URN образа: `RedHat:RHEL:7.4:7.4.2018010506`), будут возникать проблемы с подключением к инфраструктуре RHUI из-за просроченного SSL-сертификата клиента. Ошибка, которую вы увидите, может иметь вид _"Кэширующий узел SSL отклонил сертификат, срок действия которого истек"_ или _"Ошибка: Не удалось получить метаданные репозитория (repomd.xml) для репозитория:... Проверьте указанный путь и повторите попытку"_. Чтобы решить эту проблему, обновите пакет клиента RHUI на виртуальной машине, используя следующую команду.

```bash
sudo yum update -y --disablerepo='*' --enablerepo='*microsoft*'
```

Кроме того, с помощью команды `sudo yum update` можно также обновить пакет сертификата клиента (в зависимости от версии RHEL), несмотря на ошибки "Срок действия SSL-сертификата истек", которые будут отображаться для других репозиториев. После успешного обновления нормальное подключение к другим репозиториям RHUI должно быть восстановлено, как и возможность успешного выполнения команды `sudo yum update`.

При возникновении ошибки 404 при выполнении `yum update`, попробуйте выполнить следующие действия для обновления кэша yum:
```bash
sudo yum clean all;
sudo yum makecache
```

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Устранение неполадок подключения к инфраструктуре RHUI в Azure
Если вы столкнулись с проблемами при подключении к инфраструктуре RHUI в Azure с виртуальной машины RHEL (PAYG) в Azure, выполните следующие действия.

1. Проверьте конфигурацию виртуальной машины для конечной точки RHUI в Azure.

    a. Проверьте, содержит ли файл `/etc/yum.repos.d/rh-cloud.repo` ссылку на `rhui-[1-3].microsoft.com` в `baseurl` раздела `[rhui-microsoft-azure-rhel*]`. Если это так, то вы используете новую версию инфраструктуры RHUI Azure.

    2. Если он указывает на расположение с помощью шаблона `mirrorlist.*cds[1-4].cloudapp.net`, то требуется обновить конфигурацию. Вы используете устаревший моментальный снимок виртуальной машины. Его нужно обновить, чтобы в файле было указано расположение новой версии инфраструктуры RHUI в Azure.

1. Доступ к размещенной в Azure инфраструктуре RHUI могут получать только виртуальные машины с [IP-адресами в диапазонах центра обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653).

1. Если вы убедились, что IP-адрес подключаемой виртуальной машины находится в диапазоне IP-адресов Azure, но при использовании новой конфигурации вам по-прежнему не удается подключиться к инфраструктуре RHUI в Azure, обратитесь в службу поддержки Майкрософт или Red Hat.

### <a name="infrastructure-update"></a>Обновление инфраструктуры

В сентябре 2016 года была развернута обновленная инфраструктура RHUI в Azure. В апреле 2017 года работа старой инфраструктуры RHUI в Azure была завершена. Если вы использовали образы RHEL (PAYG) либо их моментальные снимки с сентября 2016 года или более позднего времени, вы автоматически подключились к новой версии инфраструктуры RHUI в Azure. Но если у вас есть более старые моментальные снимки виртуальных машин, их конфигурации необходимо обновить вручную, чтобы обеспечить доступ к инфраструктуре RHUI в Azure, как описано в следующем разделе.

Новые серверы RHUI в Azure развертываются с помощью [диспетчера трафика Azure](https://azure.microsoft.com/services/traffic-manager/). В диспетчере трафика одну конечную точку (rhui-1.microsoft.com) может использовать любая виртуальная машина независимо от региона.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Процедура обновления вручную для использования серверов Azure RHUI
Эта процедура приводится только для справки. Образы RHEL (PAYG) уже содержат правильную конфигурацию для подключения к инфраструктуре RHUI в Azure. Чтобы вручную обновить конфигурацию для использования серверов RHUI в Azure, выполните следующие действия.

1. Скачайте подпись открытого ключа с помощью curl.

   ```bash
   curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc
   ```

1. Проверьте, действителен ли скачанный ключ.

   ```bash
   gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
   ```

1. Просмотрите выходные данные, а затем проверьте `keyid` и `user ID packet`.

   ```bash
   Version: GnuPG v1.4.7 (GNU/Linux)
   :public key packet:
           version 4, algo 1, created 1446074508, expires 0
           pkey[0]: [2048 bits]
           pkey[1]: [17 bits]
           keyid: EB3E94ADBE1229CF
   :user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
   :signature packet: algo 1, keyid EB3E94ADBE1229CF
           version 4, created 1446074508, md5len 0, sigclass 0x13
           digest algo 2, begin of digest 1a 9b
           hashed subpkt 2 len 4 (sig created 2015-10-28)
           hashed subpkt 27 len 1 (key flags: 03)
           hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
           hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
           hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
           hashed subpkt 30 len 1 (features: 01)
           hashed subpkt 23 len 1 (key server preferences: 80)
           subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
           data: [2047 bits]
   ```

1. Установите открытый ключ.

   ```bash
   sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
   sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
   ```

1. Скачайте, проверьте и установить клиент диспетчера пакетов RPM (RPM).

    >[!NOTE]
    >Версии пакетов меняются. Если вы вручную подключаетесь к инфраструктуре RHUI в Azure, то последнюю версию пакета клиента для каждого семейства RHEL можно узнать, подготовив последний образ из коллекции.

   a. Скачайте компоненты.

    - Для RHEL 6:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.2-74.noarch.rpm
        ```

    - Для RHEL 7:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.2-74.noarch.rpm
        ```

   2. Выполните проверку.

   ```bash
   rpm -Kv azureclient.rpm
   ```

   c. Проверьте выходные данные, чтобы убедиться, что подпись пакета действительна.

   ```bash
   azureclient.rpm:
       Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
       Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
       V3 RSA/SHA256 Signature, key ID be1229cf: OK
       MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
   ```

   d. Установите RPM.

    ```bash
    sudo rpm -U azureclient.rpm
    ```

1. По завершении убедитесь, что вы можете получить доступ к инфраструктуре RHUI в Azure с виртуальной машины.

## <a name="next-steps"></a>Дальнейшие действия
* Сведения о создании виртуальной машины Red Hat Enterprise Linux на основе образа с оплатой по мере использования (PAYG) и о применении размещенной в Azure инфраструктуры RHUI см. на странице [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).
* Дополнительные сведения об образах Red Hat в Azure можно найти на [странице документации](./rhel-images.md).
* Сведения о политиках поддержки Red Hat для всех версий RHEL можно найти на странице [о жизненных циклах выпусков Red Hat Enterprise Linux](https://access.redhat.com/support/policy/updates/errata).
