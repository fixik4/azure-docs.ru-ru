---
title: Обновление 1901 для Azure Stack | Документация Майкрософт
description: Сведения о новых возможностях и известных проблемах в обновлении 1901 для интегрированных систем Azure Stack, а также о том, где можно скачать это обновление.
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
ms.topic: article
ms.date: 03/20/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 03/20/2019
ms.openlocfilehash: e02a09bdc8bd80b93f7fa33632c32a75c1d705bd
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226867"
---
# <a name="azure-stack-1901-update"></a>Обновление 1901 для Azure Stack

*Область применения: интегрированные системы Azure Stack*

В этой статье описывается содержимое пакета обновления 1901. Обновление содержит улучшения, исправления и новые функции для этой версии Azure Stack. В этой статье также описываются известные проблемы этого выпуска и содержится ссылка для скачивания обновления. Известные проблемы можно разделить на проблемы, которые непосредственно относятся к процессу обновления, и проблемы со сборкой (после установки).

> [!IMPORTANT]  
> Этот пакет обновления предназначен только для интегрированных систем Azure Stack. Не применяйте этот пакет обновления к Пакету средств разработки Azure Stack.

## <a name="build-reference"></a>Указание сборки

Номер сборки обновления Azure Stack 1901 — **1.1901.0.95** или **1.1901.0.99** (после 26 февраля 2019 г.). Ознакомьтесь со следующим примечанием.

> [!IMPORTANT]  
> Корпорация Майкрософт обнаружила проблему, которая может повлиять на клиентов, выполняющих обновление с версии 1811 (1.1811.0.101) до версии 1901. Для ее устранения выпущен обновленный пакет 1901: сборка 1.1901.0.99, обновленная с 1.1901.0.95. Клиентам, уже выполнившим обновление до 1.1901.0.95, не нужно выполнять дальнейшие действия.
>
> Для подключенных клиентов, работающих с версией 1811, новый пакет версии 1901 (1.1901.0.99) появится на портале администратора автоматически. Они должны установить его, когда все будет готово. Автономные клиенты могут скачать и импортировать новый пакет версии 1901, используя тот же процесс, [описанный здесь](azure-stack-apply-updates.md).
>
> У клиентов с обеими версиями 1901 не возникнут проблемы при установке следующего полного пакета или пакета с исправлением.

## <a name="hotfixes"></a>Исправления

Azure Stack выпускает исправления на регулярной основе. Обязательно установите [последнее исправление Azure Stack](#azure-stack-hotfixes) для обновления 1811 перед обновлением Azure Stack до версии 1901.

Исправления Azure Stack применимы только к интегрированным системам Azure Stack. Не устанавливайте исправления в пакете ASDK.

> [!TIP]  
> Подпишитесь на следующие веб-каналы *RSS* или *Atom*, чтобы следить за исправлениями Azure Stack:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Исправления для Azure Stack

- **1809**: [KB 4481548 — исправление Azure Stack 1.1809.12.114](https://support.microsoft.com/help/4481548/).
- **1811**: нет текущих исправлений.
- **1901**: [KB 4481548 — исправление Azure Stack 1.1901.2.103](https://support.microsoft.com/help/4494720).

## <a name="prerequisites"></a>Предварительные требования

> [!IMPORTANT]
> - Установите [последнее исправление для Azure Stack](#azure-stack-hotfixes) версии 1811 (если она у вас есть) перед обновлением до версии 1901.

- Перед началом установки этого обновления запустите [Test-AzureStack](azure-stack-diagnostic-test.md) со следующими параметрами для проверки состояния Azure Stack и устраните все найденные операционные проблемы, включая все предупреждения и сбои. Кроме того, просмотрите активные предупреждения и решите проблемы с теми, которые требуют принятия мер:

    ```PowerShell
    Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

- При управлении Azure Stack с помощью System Center Operations Manager (SCOM) обновите пакет управления Microsoft Azure Stack до версии 1.0.3.11 перед обновлением этой инфраструктуры до версии 1901.

## <a name="new-features"></a>новые функции;

Это обновление включает в себя следующие новые функции и улучшения для Azure Stack:

- Управляемые образы в Azure Stack позволяют создавать на обобщенной виртуальной машине (как неуправляемой, так и управляемой) объект управляемого образа, который может создавать только виртуальные машины с управляемыми дисками в будущем. Дополнительные сведения см. в статье об [Управляемых дисках Azure Stack](user/azure-stack-managed-disk-considerations.md#managed-images).

- **AzureRm 2.4.0**
   * **AzureRm.Profile**  
         Исправления: `Import-AzureRmContext` правильно десериализирует сохраненный маркер.  
   * **AzureRm.Resources**  
         Исправления: `Get-AzureRmResource` выполняет запросы по типу ресурса (не чувствителен к регистру).  
   * **Azure.Storage**  
         Модуль развертывания AzureRm теперь включает уже опубликованную версию 4.5.0, поддерживающую **api-version 2017-07-29**.  
   * **AzureRm.Storage**  
         Модуль развертывания AzureRm теперь включает уже опубликованную версию 5.0.4, поддерживающую **api-version 2017-10-01**.  
   * **AzureRm.Compute**  
         Добавлены простые наборы параметров в `New-AzureRmVM` и `New-AzureRmVmss`. Параметр `-Image` поддерживает указание пользовательских образов.  
   * **AzureRm.Insights**  
         Модуль развертывания AzureRm теперь включает уже опубликованную версию 5.1.5, поддерживающую **api-version 2018-01-01**, для типов ресурсов метрик и определений метрик.

- **AzureStack 1.7.0**. Это выпуск с критическим изменением. Дополнительные сведения о критических изменениях находятся по ссылке https://aka.ms/azspshmigration170.
   * **Модуль Azs.Backup.Admin**  
         Критическое изменение. Изменения резервного копирования в режиме шифрования на основе сертификатов. Прекращена поддержка симметричных ключей.  
   * **Модуль Azs.Fabric.Admin**  
         `Get-AzsInfrastructureVolume` больше не поддерживается. Используйте новый командлет `Get-AzsVolume`.  
         `Get-AzsStorageSystem` больше не поддерживается.  Используйте новый командлет `Get-AzsStorageSubSystem`.  
         `Get-AzsStoragePool` больше не поддерживается. Объект `StorageSubSystem` содержит свойство capacity.  
   * **Модуль Azs.Compute.Admin**  
         Исправления: `Add-AzsPlatformImage`, `Get-AzsPlatformImage`. Вызов `ConvertTo-PlatformImageObject` происходит только по правильному пути.  
         Исправления: `Add-AzsVmExtension`, `Get-AzsVmExtension`. Вызов ConvertTo-VmExtensionObject происходит только по правильному пути.  
   * **Модуль Azs.Storage.Admin**  
         Исправления: новая квота хранилища использует значения по умолчанию, если значения отсутствуют.

Дополнительные сведения об обновленных модулях см. в [справочных материалах по модулям Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0&viewFallbackFrom=azurestackps-1.7.0).

## <a name="fixed-issues"></a>Исправленные проблемы

- Исправлена проблема, из-за которой на портале отображался параметр для создания VPN-шлюзов на основе политик, которые не поддерживаются в Azure Stack. Этот параметр удален с портала.

<!-- 16523695 – IS, ASDK -->
- Исправлена проблема, из-за которой после обновления параметров DNS для виртуальной сети с **DNS Azure Stack** на **пользовательскую службу DNS** экземпляры не обновлялись для использования нового параметра.

- <!-- 3235634 – IS, ASDK -->
  Исправлена проблема, из-за которой развертываемые виртуальные машины с размерами, содержащими суффикс **v2**, например **Standard_A2_v2**, требовали указания суффикса в следующем виде: **Standard_A2_v2** (строчная v). Как и в глобальной среде Azure, теперь можно использовать **Standard_A2_V2** (V прописными буквами).

<!-- 2869209 – IS, ASDK --> 
- Исправлена проблема, из-за которой в [командлет Add-AzsPlatformImage](/powershell/module/azs.compute.admin/add-azsplatformimage) нужно было передавать параметр **-OsUri** в качестве универсального кода ресурса (URI) учетной записи хранения, куда будет отправлен диск. Теперь можно также использовать и локальный путь к диску.

<!--  2795678 – IS, ASDK --> 
- Исправлена проблема, приводившая к появлению предупреждения при использовании портала для создания виртуальной машины в ценовой категории "Премиум" (DS, Ds_v2, FS, FSv2). Виртуальная машина создавалась в учетной записи хранения ценовой категории "Стандартный". Хотя это не влияло ни на функциональность, ни на количество операций ввода-вывода в секунду, ни на выставление счетов, предупреждение было исправлено.

<!-- 1264761 - IS ASDK -->  
- Исправлена проблема с компонентом **Контроллер работоспособности**, из-за которой создавались следующие оповещения. Эти оповещения можно игнорировать.

    - Предупреждение № 1:
       - Имя.  Роль инфраструктуры неработоспособна.
       - Уровень серьезности. Предупреждение
       - Компонент. Контроллер работоспособности.
       - Описание. Контроллер работоспособности "Сканер пульса" недоступен. Это может повлиять на отчеты о работоспособности и метрики.  

    - Предупреждение №2:
       - Имя.  Роль инфраструктуры неработоспособна.
       - Уровень серьезности. Предупреждение
       - Компонент. Контроллер работоспособности.
       - Описание. Контроллер работоспособности "Сканер ошибок" недоступен. Это может повлиять на отчеты о работоспособности и метрики.


<!-- 3507629 - IS, ASDK --> 
- Устранена проблема, из-за которой установка значения 0 для квоты Управляемых дисков в разделе [тип квот вычислений](azure-stack-quota-types.md#compute-quota-types) была эквивалентна значению по умолчанию (2048 ГиБ). Теперь нулевое значение квоты учитывается.

<!-- 2724873 - IS --> 
- Устранена проблема, из-за которой при использовании командлетов PowerShell **Start-AzsScaleUnitNode** или **Stop-AzsScaleUnitNode** для управления единицами масштабирования первая попытка запустить или остановить единицу масштабирования могла завершиться сбоем.

<!-- 2724961- IS ASDK --> 
- Устранена проблема, из-за которой при регистрации поставщика ресурсов **Microsoft.Insight** в параметрах подписки и при создании виртуальной машины Windows с включенной диагностикой гостевой ОС на странице обзора виртуальной машины на графике загрузки ЦП в процентах не отображались данные метрик. Теперь данные отображаются правильно.

- Устранена проблема, из-за которой запуск командлета **Get-AzureStackLog** после выполнения **Test-AzureStack** в одном и том же сеансе привилегированной конечной точки (PEP) завершался ошибкой. Теперь можно использовать тот же сеанс PEP, в котором выполнен командлет **Test-AzureStack**.

<!-- bug 3615401, IS -->
- Устранена проблема с автоматическим резервным копированием, из-за которой служба планировщика неожиданно переходила в отключенное состояние. 

<!--2850083, IS ASDK -->
- Удалена кнопка **Reset Gateway** (Сброс шлюза) на портале Azure Stack, которая вызывала ошибку при нажатии. Эта кнопка не выполняла никаких функций в Azure Stack, так как в Azure Stack есть мультитенантный шлюз, а не выделенные экземпляры виртуальных машин для VPN-шлюза каждого клиента. Поэтому ее удалили во избежание путаницы. 

<!-- 3209594, IS ASDK -->
- Удалена ссылка на **действующие правила безопасности** из колонки **свойств сети**, так как эта функция не поддерживается в Azure Stack. Наличие ссылки создавало впечатление, что эта функция поддерживается, но она не работает. Мы удалили ссылку, чтобы избежать путаницы.

<!-- 3139614 | IS -->
- Устранена проблема, из-за которой после установки обновления Azure Stack от изготовителя оборудования на портале администратора Azure Stack не появлялось уведомление **Доступно обновление**.

## <a name="changes"></a>изменения

<!-- 3083238 IS -->
- Улучшения безопасности в этом обновлении приводят к увеличению размера резервной копии роли службы каталогов. Инструкции по изменению размера для внешнего хранилища содержатся в [документации по резервному копированию инфраструктуры](azure-stack-backup-reference.md#storage-location-sizing). Это изменение приводит к более длительному времени завершения резервного копирования из-за большого объема передачи данных. Это изменение влияет на интегрированные системы. 

- Начиная с января 2019 г. можно развертывать кластеры Kubernetes в подключенных метках Azure Stack, зарегистрированных в службах федерации Active Directory (AD FS) (подключение к Интернету не требуется). Следуйте указаниям, приведенным [здесь](azure-stack-solution-template-kubernetes-cluster-add.md), чтобы скачать новый элемент Marketplace Kubernetes. Следуйте указаниям, приведенным [здесь](user/azure-stack-solution-template-kubernetes-adfs.md), чтобы развернуть кластер Kubernetes. Обратите внимание на новые параметры, позволяющие указать, где зарегистрирована целевая система: в ADD или в AD FS. Для систем, зарегистрированных в AD FS, доступны новые поля для ввода параметров Key Vault, в котором хранится сертификат развертывания.

   Обратите внимание, что даже с поддержкой AD FS для развертывания кластеров Kubernetes требуется доступ к Интернету.

- После установки обновлений или исправлений в Azure Stack могут быть введены новые функции, которые требуют предоставления новых разрешений одному или нескольким приложениям идентификации. Для предоставления этих разрешений требуется доступ администратора к домашнему каталогу, поэтому автоматически это выполнить нельзя. Например: 

   ```powershell
   $adminResourceManagerEndpoint = "https://adminmanagement.<region>.<domain>"
   $homeDirectoryTenantName = "<homeDirectoryTenant>.onmicrosoft.com" # This is the primary tenant Azure Stack is registered to

   Update-AzsHomeDirectoryTenant -AdminResourceManagerEndpoint $adminResourceManagerEndpoint `
     -DirectoryTenantName $homeDirectoryTenantName -Verbose
   ```

- Существует новый подход для точного планирования емкости Azure Stack. Ограничение на количество создаваемых виртуальных машин было введено в 1901 обновлении.  Данное ограничение является временным и предназначено для предотвращения сбоев в решении. Источник ошибок стабильности при большом количестве виртуальных машин обрабатывается, но точное время его устранения определено не было. Ограничение в 60 виртуальных машин на один сервер и общий лимит решений в 700 было введено после обновления 1901.  Например, ограничение 8 сервера виртуальной машины Azure Stack составит 480 (8 * 60).  А для 12 и 16 серверов Azure Stack решением число ограничения будет определено как 700. Ограничение было создано, чтобы учесть все возможные вычислительные мощности, например, резерв для устойчивости и соотношение виртуальных и физических ресурсов ЦП, которое оператор хотел бы сохранить в метке. Дополнительные сведения см. в разделе о новом выпуске планировщика ресурсов.  
Если предел масштабирования виртуальной машины постигнут, в качестве результата будут возвращены следующие коды ошибок: VMsPerScaleUnitLimitExceeded, VMsPerScaleUnitNodeLimitExceeded. 
 

- Версия API вычислений возросла до 2017-12-01.

- При резервном копировании инфраструктуры для шифрования данных резервных копий теперь требуется сертификат только с открытым ключом (CER). Поддержка симметричных ключей шифрования является нерекомендуемой, начиная с версии 1901. Если резервное копирование инфраструктуры было настроено до обновления до версии 1901, ключи шифрования останутся на месте. Будет еще как минимум 2 обновления с поддержкой обратной совместимости, чтобы вы смогли обновить настройки резервного копирования. Дополнительные сведения см. в статье [Рекомендации по службе резервного копирования инфраструктуры](azure-stack-backup-best-practices.md).

## <a name="common-vulnerabilities-and-exposures"></a>Распространенные уязвимости и риски

В рамках этого обновления устанавливаются следующие обновления системы безопасности:  

- [CVE-2018-8477](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8477)
- [CVE-2018-8514](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8514)
- [CVE-2018-8580](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8580)
- [CVE-2018-8595](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8595)
- [CVE-2018-8596](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8596)
- [CVE-2018-8598](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8598)
- [CVE-2018-8621](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8621)
- [CVE-2018-8622](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8622)
- [CVE-2018-8627](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8627)
- [CVE-2018-8637](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8637)
- [CVE-2018-8638](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8638)
- [ADV190001](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190001)
- [CVE-2019-0536](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0536)
- [CVE-2019-0537](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0537)
- [CVE-2019-0545](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0545)
- [CVE-2019-0549](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0549)
- [CVE-2019-0553](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0553)
- [CVE-2019-0554](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0554)
- [CVE-2019-0559](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0559)
- [CVE-2019-0560](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0560)
- [CVE-2019-0561](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0561)
- [CVE-2019-0569](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0569)
- [CVE-2019-0585](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0585)
- [CVE-2019-0588](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0588)


Дополнительные сведения об этих уязвимостях доступны по предыдущим ссылкам или в статье базы знаний Майкрософт [4480977](https://support.microsoft.com/en-us/help/4480977).

## <a name="known-issues-with-the-update-process"></a>Известные проблемы с процессом обновления

- Если при выполнении [Test-AzureStack](azure-stack-diagnostic-test.md) тест **AzsInfraRoleSummary** или **AzsPortalApiSummary** завершается ошибкой, вам будет предложено запустить **Test-AzureStack** с параметром `-Repair`.  При выполнении этой команды происходит сбой со следующим сообщением об ошибке: `Unexpected exception getting Azure Stack health status. Cannot bind argument to parameter 'TestResult' because it is null.`

- При запуске [Test-AzureStack](azure-stack-diagnostic-test.md) выводится предупреждающее сообщение от контроллера управления основной платой (BMC). Это предупреждение можно проигнорировать.

- <!-- 2468613 - IS --> Во время установки этого обновления могут отображаться оповещения с заголовком `Error – Template for FaultType UserAccounts.New is missing.` Их можно игнорировать. Эти оповещения закроются автоматически после завершения установки этого обновления.

## <a name="post-update-steps"></a>Действия после обновления

- После установки этого обновления установите все применимые исправления. Дополнительные сведения см. в разделе [Исправления](#hotfixes), а также в статье [Политика обслуживания Azure Stack](azure-stack-servicing-policy.md).  

- Извлеките ключи шифрования неактивных данных и надежно сохраните их за пределами развертывания Azure Stack. Следуйте указаниям [о том, как извлечь ключи](azure-stack-security-bitlocker.md).

## <a name="known-issues-post-installation"></a>Известные проблемы (после установки)

Ниже перечислены известные проблемы после установки этой версии сборки.

### <a name="portal"></a>Microsoft Azure

<!-- 2930820 - IS ASDK --> 
- На порталах администратора и пользователя при поиске "Docker" элемент возвращается неправильно. Он недоступен в Azure Stack. При попытке его создания откроется колонка с указанием ошибки. 

<!-- 2931230 – IS  ASDK --> 
- Планы, добавленные в подписку пользователя как дополнительные, невозможно удалить даже при удалении основного плана из подписки. Дополнительный план будет оставаться в подписке, пока подписки, которые ссылаются на него, не будут удалены. 

<!-- TBD - IS ASDK --> 
- Два типа административной подписки, которые были добавлены в версии 1804, использовать не рекомендуется. Это такие типы подписки: **Подписка для учета** и **Подписка для** Эти типы подписок еще не готовы к использованию, но их уже можно увидеть в новых средах Azure Stack, начиная с версии 1804. Продолжайте использовать тип подписки **Поставщик по умолчанию**.

<!-- 3557860 - IS ASDK --> 
- Удаление подписки пользователя приводит к появлению потерянных ресурсов. Чтобы избежать этого, следует сначала удалить ресурсы пользователя или всю группу ресурсов и лишь затем удалять подписки пользователя.

<!-- 1663805 - IS ASDK --> 
- Вы не можете просматривать разрешения для подписки на порталах Azure Stack. [Используйте PowerShell](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan), чтобы проверить разрешения.

<!-- ### Health and monitoring -->

### <a name="compute"></a>Службы вычислений

- При создании новой виртуальной машины Windows (VM) может отображаться следующая ошибка.

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   Ошибка возникает, если вы включаете диагностику загрузки на виртуальной машине, но удаляете свою учетную запись хранения диагностики загрузки. Чтобы обойти эту проблему, повторно создайте учетную запись хранения с тем же именем, которое вы использовали ранее.

<!-- 2967447 - IS, ASDK, to be fixed in 1902 -->
- Интерфейс создания масштабируемого набора виртуальных машин предлагает образ на основе CentOS 7.2 в качестве варианта для развертывания. Так как этот образ не поддерживается в Azure Stack, выберите другую операционную систему для развертывания или используйте шаблон Azure Resource Manager, указав другой образ CentOS, скачанный оператором до развертывания из Marketplace.  

<!-- TBD - IS ASDK --> 
- После применения обновления 1901 могут возникнуть следующие проблемы при развертывании виртуальных машин с Управляемыми дисками.

   - Если подписка была создана до обновления 1808, при развертывании виртуальной машины с Управляемыми дисками может произойти сбой с сообщением о внутренней ошибке. Чтобы устранить ошибку, выполните следующие действия для каждой подписки.
      1. На портале клиента перейдите в раздел **Подписки** и найдите подписку. Щелкните **Поставщики ресурсов**, выберите **Microsoft.Compute**, а затем — **Повторная регистрация**.
      2. В той же подписке перейдите в раздел **Управление доступом (IAM)** и убедитесь, что в списке есть **AzureStack-DiskRP-Client**.
   - Если вы настроили среды с несколькими клиентами, при развертывании виртуальных машин в подписке, которая связана с гостевым каталогом, возможен сбой с сообщением о внутренней ошибке. Для устранения этой ошибки выполните действия, описанные в [этой статье](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory), чтобы перенастроить все гостевые каталоги.

- Виртуальная машина Ubuntu 18.04, созданная с включенной авторизацией SSH, не позволяет использовать ключи SSH для входа. В качестве решения используйте расширение доступа к виртуальной машине для Linux для реализации ключей SSH после подготовки или проверку подлинности на основе пароля.

### <a name="networking"></a>Сеть  

<!-- 3239127 - IS, ASDK -->
- При изменении на портале Azure Stack статического IP-адреса для конфигурации IP, которая привязана к сетевому адаптеру, подключенному к экземпляру виртуальной машины, вы увидите предупреждение: 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`.

    Вы можете игнорировать это сообщение, так как IP-адрес изменится, даже если не перезапустить экземпляр виртуальной машины.

<!-- 3632798 - IS, ASDK -->
- На портале при добавлении правила безопасности входящего трафика и выборе в качестве источника **тега службы** в списке **тегов источников** отображаются несколько вариантов, которые недоступны для Azure Stack. Ниже перечислены варианты, допустимые в Azure Stack:

  - **Интернет**;
  - **VirtualNetwork**;
  - **AzureLoadBalancer**.
  
    Другие варианты не поддерживаются как теги источников в Azure Stack. Аналогично, если добавить правило безопасности для исходящего трафика и выбрать **тег службы** как целевой объект, отобразится тот же список вариантов для **тега источника**. Единственные допустимые варианты такие же, как и для **тегов источников** (приведены в предыдущем списке).

- Группы безопасности сети (NSG) работают в Azure Stack не так, как в глобальной среде Azure. В Azure можно задать несколько портов в одном правиле группы безопасности сети (с помощью портала, PowerShell и шаблонов Resource Manager). Однако в Azure Stack невозможно задать несколько портов в одном правиле группы безопасности сети с помощью портала. Чтобы обойти эту проблему, используйте шаблон Resource Manager или PowerShell для установки этих дополнительных правил.

<!-- 3203799 - IS, ASDK -->
- В настоящее время Azure Stack не поддерживает подключение более 4 сетевых интерфейсов (NIC) к экземплярам виртуальных машин независимо от размера экземпляра.

<!-- ### SQL and MySQL-->

### <a name="app-service"></a>Служба приложений

<!-- 2352906 - IS ASDK --> 
- Прежде чем создавать первую функцию Azure в подписке, пользователь должен зарегистрировать поставщик ресурсов хранилища.


<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->

## <a name="download-the-update"></a>Скачивание обновления

Пакет обновления 1901 для Azure Stack можно скачать [здесь](https://aka.ms/azurestackupdatedownload). 

Только подключенные сценарии: развертывания Azure Stack будут периодически проверять защищенную конечную точку и автоматически сообщать, доступно ли обновление для вашего облака. Дополнительные сведения см. в статье [Управление обновлениями для Azure Stack](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Дополнительная информация

- [Общие сведения об управлении обновлениями в Azure Stack](azure-stack-updates.md).  
- [Применение обновлений в Azure Stack](azure-stack-apply-updates.md).
- В [этой статье](azure-stack-servicing-policy.md) описаны политика обслуживания для интегрированных систем Azure Stack и действия, необходимые для сохранения поддерживаемого состояния системы.  
- Сведения об использовании привилегированной конечной точки (PEP) для отслеживания и возобновления обновлений см. в статье [Мониторинг обновлений в Azure Stack с помощью привилегированной конечной точки](azure-stack-monitor-update.md).  
