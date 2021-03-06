---
title: Планирование емкости для ролей сервера службы приложений Azure в Azure Stack | Документация Майкрософт
description: Планирование емкости для ролей сервера службы приложений Azure в Azure Stack
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/13/2019
ms.openlocfilehash: 06bafbcf3e668ba17b1245b9352e942e02569997
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "57852378"
---
# <a name="capacity-planning-for-azure-app-service-server-roles-in-azure-stack"></a>Планирование емкости для ролей сервера службы приложений Azure в Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Чтобы настроить готовое для рабочей среды развертывание Службы приложений Azure в Azure Stack, необходимо запланировать емкость, которую система будет поддерживать.  

Эта статья содержит рекомендации по минимальному числу экземпляров и номеров SKU для вычислений, которое следует использовать для любого рабочего развертывания.

Запланировать стратегию емкости службы приложений можно с помощью следующих правил.

| Роль сервера службы приложений | Минимальное рекомендуемое число экземпляров | Рекомендуемый номер SKU для вычислений|
| --- | --- | --- |
| Controller | 2 | A1 |
| Внешний интерфейс | 2 | A1 |
| управления | 2 | A3 |
| ИЗДАТЕЛЬ | 2 | A1 |
| Рабочие роли — общие | 2 | A1 |
| Рабочие роли — выделенные | 2 на уровень | A1 |

## <a name="controller-role"></a>Роль контроллера

**Рекомендуемые минимальные ресурсы**: два экземпляра A1 Standard

Контроллер службы приложений Azure, как правило, характеризуется низким потреблением ресурсов ЦП, памяти и сетевых ресурсов. Но для обеспечения высокой доступности необходимо иметь два контроллера. Два контроллера — это также максимально разрешенное количество. Второй контроллер веб-сайтов можно создать непосредственно из установщика во время развертывания.

## <a name="front-end-role"></a>Роль внешнего интерфейса

**Рекомендуемые минимальные ресурсы**: два экземпляра A1 Standard

Внешний интерфейс направляет запросы к рабочим ролям в зависимости от их доступности. Для обеспечения высокой доступности следует иметь несколько внешних интерфейсов и можно иметь более двух. При планировании емкости допустим, что каждое ядро может обрабатывать приблизительно 100 запросов в секунду.

## <a name="management-role"></a>Роль управления

**Рекомендуемые минимальные ресурсы**: два экземпляра A3 Standard

Роль управления Службы приложений Azure отвечает за конечные точки API и Azure Resource Manager для службы приложений и расширения портала (порталы администратора, клиента и Функций), а также за службу данных. Для роли сервера управления, как правило, требуется всего около 4 ГБ ОЗУ в рабочей среде. Тем не менее могут возникать высокие уровни использования ЦП при одновременном выполнении многих задач управления (например, создание веб-сайта). Для обеспечения высокой доступности этой роли необходимо назначить несколько серверов, а на каждом сервере должно быть как минимум два ядра.

## <a name="publisher-role"></a>Роль издателя

**Рекомендуемые минимальные ресурсы**: два экземпляра A1 Standard

Если многие пользователи проводят публикацию одновременно, в роли издателя может возникать интенсивное использование ЦП. Для обеспечения высокой доступности должны быть доступны несколько ролей издателя. Издатель обрабатывает только трафик FTP и FTPS.

## <a name="web-worker-role"></a>Рабочая роль

**Рекомендуемые минимальные ресурсы**: два экземпляра A1 Standard

Для обеспечения высокой доступности необходимо иметь по крайней мере четыре рабочие роли: две для общего режима веб-сайта и две для каждого уровня выделенной рабочей роли, которую вы собираетесь предлагать. Общий и выделенный режимы вычислений предоставляют клиентам различные уровни обслуживания. Возможно, понадобятся дополнительные рабочие роли, если многие клиенты:

- используют уровни рабочих ролей выделенного режима вычислений (которые являются ресурсоемкими);
- работают в общем режиме вычислений.

После того как пользователь создаст план службы приложений для номера SKU выделенного режима вычислений, число рабочих ролей, указанное в этом плане службы приложений, больше не будет доступно для пользователей.

Чтобы предоставить пользователям Функции Azure в модели плана потребления, необходимо развернуть общие рабочие роли.

При выборе количества общих рабочих ролей учтите следующие рекомендации:

- **Память**. Это наиболее важный ресурс для рабочей роли. Недостаток памяти влияет на производительность веб-сайта, когда объем виртуальной памяти переносится с диска. Для каждого сервера требуется примерно 1,2 ГБ ОЗУ для операционной системы. Объем ОЗУ, превышающий это пороговое значение, можно использовать для запуска веб-сайтов.
- **Процент активных веб-сайтов**. Как правило, около 5 процентов приложений в службе приложений Azure в развертывании Azure Stack являются активными. Но процент приложений, которые активны в любой момент, может быть выше или ниже. При частоте активных приложений в 5 процентов максимальное число приложений, которое можно разместить в развертывании Службы приложений Azure в Azure Stack, не должно превышать число активных веб-сайтов, умноженное на 20 (5 x 20 = 100).
- **Средний объем памяти**. Средний объем памяти для приложений, наблюдаемых в рабочих средах, равен примерно 70 МБ. С помощью этого показателя объем памяти, выделенный для всех компьютеров или виртуальных машин с рабочими ролями, можно рассчитать следующим образом:

   `Number of provisioned applications * 70 MB * 5% - (number of web worker roles * 1044 MB)`

   Например, при наличии 5000 приложений в среде, в которой выполняется 10 рабочих ролей, размер ОЗУ каждой виртуальной машины рабочей роли должен быть 7060 МБ:

   `5,000 * 70 * 0.05 – (10 * 1044) = 7060 (= about 7 GB)`

   Сведения о добавлении дополнительных экземпляров рабочих ролей см. в статье [Служба приложений в Azure Stack: добавление рабочих ролей и ролей инфраструктуры](azure-stack-app-service-add-worker-roles.md).

### <a name="additional-considerations-for-dedicated-workers-during-upgrade-and-maintenance"></a>Дополнительные рекомендации по выделенным рабочим ролям во время обновления и обслуживания

Во время обновления и обслуживания рабочих ролей Служба приложений Azure в Azure Stack в любой момент может выполнить обслуживание 20 % каждого уровня рабочих ролей.  Следовательно, администраторы облака должны поддерживать 20 % пула нераспределенных рабочих ролей для каждого уровня рабочих ролей, чтобы гарантировать, что во время обновления и обслуживания в клиенте не будут возникать проблемы в работе службы.  Например, при наличии 10 рабочих ролей одного уровня для выполнения обновления и обслуживания следует убедиться, что 2 из них не распределены. Если все 10 рабочих ролей распределены, необходимо масштабировать уровень рабочих ролей для поддержания пула нераспределенных рабочих ролей. Во время обновления и обслуживания Служба приложений Azure переместит рабочие нагрузки в нераспределенные рабочие роли, чтобы убедиться, что рабочие нагрузки продолжат работать. Однако отсутствие во время обновления доступных нераспределенных рабочих ролей может привести к простою рабочей нагрузки клиента.  Что касается общих рабочих ролей, клиентам не нужно подготавливать дополнительные рабочие роли, так как служба автоматически выделит приложения клиента в доступных рабочих ролях, чтобы обеспечить высокую доступность, однако на этом уровне требуются минимум две рабочих роли.

Администраторы облака могут отслеживать распределение уровня рабочих ролей в области администрирования Службы приложений на портале администрирования Azure Stack.  Перейдите к Службе приложений, а затем выберите "Рабочие уровни" на панели слева.  В таблице "Рабочие уровни" указаны: название и размер уровня рабочих ролей, используемый образ, количество доступных рабочих ролей (нераспределенных), общее число рабочих ролей на каждом уровне и общее состояние уровня рабочих ролей.

![Администрирование Службы приложений — "Рабочие уровни"][1]

## <a name="file-server-role"></a>Роль файлового сервера

Для роли файлового сервера вы можете использовать автономный файловый сервер для разработки и тестирования. Например, при развертывании Службы приложений Azure с помощью Пакета средств разработки Azure Stack (ASDK) можно использовать этот [шаблон](https://aka.ms/appsvconmasdkfstemplate).  В рабочей среде следует использовать предварительно настроенный файловый сервер Windows или предварительно настроенный файловый сервер не под управлением Windows.

В рабочих средах роль файлового сервера проводит большое количество дисковых операций ввода-вывода. Так как в ней размещается все содержимое и файлы приложений для веб-сайтов пользователей, для этой роли необходимо предварительно настроить один из следующих ресурсов:

- файловый сервер Windows;
- кластер файлового сервера Windows;
- файловый сервер не под управлением Windows;
- кластер файлового сервера не под управлением Windows;
- запоминающее устройство, подключаемое к сети.

Дополнительные сведения см. в разделе [Подготовка файлового сервера](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server).

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в следующих руководствах.

[Подготовка к работе со службой приложений в Azure Stack](azure-stack-app-service-before-you-get-started.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-capacity-planning/worker-tier-allocation.png