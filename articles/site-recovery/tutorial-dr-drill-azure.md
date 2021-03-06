---
title: Выполнение отработки аварийного восстановления для локальных компьютеров в Azure с помощью службы Azure Site Recovery | Документы Майкрософт
description: Сведения о выполнении отработки аварийного восстановления из локальной среды в Azure с помощью службы Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 7e85226d15b818dda65600760b3950fab9dd7aaf
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312331"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Выполнение отработки аварийного восстановления в Azure

В этой статье описано, как выполнять аварийное восстановление локальных компьютеров в Azure путем отработки тестового отказа. Выполнение проверяет стратегию репликации без потери данных.

Это четвертое руководство в серии, в которой показано, как настроить аварийное восстановление для локальных виртуальных машин VMware или виртуальных машин Hyper-V в Azure.

В этом руководстве предполагается, что вы выполнили задачи в первых трех руководствах:
- В [первом руководстве](tutorial-prepare-azure.md) рассмотрена настройка компонентов Azure, необходимых для аварийного восстановления VMware.
- Во [втором руководстве](vmware-azure-tutorial-prepare-on-premises.md) рассмотрена подготовка локальных компонентов для аварийного восстановления и изучены предварительные требования.
- В [третьем руководстве](vmware-azure-tutorial.md) рассмотрена настройка и включение репликации для локальной ВМ VMware.
- В руководствах описан **самый простой способ развертывания для определенного сценария**. В них везде, где возможно, используются значения по умолчанию, и описаны не все возможные параметры и пути. Больше узнать об отработке тестового отказа можно в этом [разделе](site-recovery-test-failover-to-azure.md).

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Настройка изолированной сети для тестовой отработки отказа
> * Подготовка к подключению к виртуальным машинам Azure после отработки отказа
> * Запуск тестовой отработки отказа для одного компьютера



## <a name="verify-vm-properties"></a>Проверка свойств виртуальной машины

Перед запуском тестовой отработки отказа проверьте свойства виртуальной машины и убедитесь, что виртуальная машина [Hyper-V](hyper-v-azure-support-matrix.md#replicated-vms) или [VMware](vmware-physical-azure-support-matrix.md#replicated-machines) соответствует требованиям Azure.

1. В разделе **Защищенные элементы** щелкните **Реплицированные элементы** и выберите виртуальную машину.
2. В области **Реплицированный элемент** находятся сводные данные о виртуальной машине, включая состояние работоспособности и последние доступные точки восстановления. Щелкните **Свойства**, чтобы просмотреть дополнительные сведения.
3. В области **Вычисления и сеть** можно изменить имя Azure, группу ресурсов, целевой размер, группу доступности и параметры управляемого диска.
4. Можно просмотреть или изменить параметры сети, включая сеть (или подсеть), в которой будет размещаться виртуальная машина Azure после отработки отказа, и IP-адрес, который будет ей назначен.
5. В разделе **Диски** отображаются сведения о дисках операционной системы и дисках данных на виртуальной машине.

## <a name="create-a-network-for-test-failover"></a>Создание сети для тестовой отработки отказа

Для тестовой отработки отказа рекомендуется выбрать сеть, изолированную от рабочей сети сайта восстановления, указанной в разделе параметров **Вычисления и сеть** для каждой виртуальной машины. При создании виртуальной сети Azure она по умолчанию изолируется от других сетей. Тестовая сеть должна имитировать рабочую сеть:

- В тестовой сети должно быть такое же количество подсетей, что и в рабочей сети. Подсети должны иметь одинаковые имена.
- Тестовая сеть должна использовать тот же диапазон IP-адресов.
- Обновите DNS тестовой сети, используя IP-адрес, указанный для виртуальной машины DNS в параметрах **Вычисления и сети**. Дополнительные сведения см. в разделе [Рекомендации по тестированию отработки отказа](site-recovery-active-directory.md#test-failover-considerations).

## <a name="run-a-test-failover-for-a-single-vm"></a>Запуск тестовой отработки отказа для одной виртуальной машины

При запуске тестовой отработки отказа происходит следующее:

1. выполняется проверка на соблюдение всех необходимых условий для отработки отказа;
2. операция отработки отказа обрабатывает данные для создания виртуальной машины Azure; если выбрана последняя точка восстановления, на основе этих данных создается точка восстановления;
3. создается виртуальная машина Azure с использованием данных, обработанных на предыдущем шаге.

Запустите тестовую отработку отказа следующим образом:

1. В разделе **Параметры** > **Реплицированные элементы** выберите виртуальную машину и щелкните **+Тестовая отработка отказа**.
2. Для работы с этим руководством выберите **последнюю отработанную** точку восстановления. При этом будет выполнена отработка отказа виртуальной машины до последней доступной точки во времени. Для нее отображается метка времени. Этот вариант не требует времени на обработку данных, обеспечивая низкий показатель целевого времени восстановления.
3. На странице **Тестовая отработка отказа** выберите целевую сеть Azure, к которой будут подключаться виртуальные машины Azure после отработки отказа.
4. Нажмите кнопку **ОК** , чтобы запустить отработку отказа. За ходом выполнения можно следить в окне свойств, которое можно открыть, щелкнув виртуальную машину. Также можно щелкнуть задание **Тестовая отработка отказа** в разделе "Имя хранилища" > **Параметры** > **Задания** >
   **Задания Site Recovery**.
5. Когда отработка отказа завершится, на вкладке **Виртуальные машины** портала Azure появится реплика виртуальной машины. Убедитесь, что виртуальная машина имеет соответствующий размер, подключена к соответствующей сети и работает.
6. Теперь вы можете подключиться к реплицированной виртуальной машине в Azure.
7. Чтобы удалить виртуальные машины Azure, созданные во время тестовой отработки отказа, щелкните **Очистить тестовую отработку отказа** на виртуальной машине. В разделе **Примечания** можно записать и сохранить любые замечания, связанные с тестовой отработкой отказа.

В некоторых сценариях требуется дополнительная обработка отработки отказа, которая длится около 8–10 минут. Тестовая отработка отказа для компьютеров Linux VMware, виртуальных машин VMware без включенной службы DHCP и виртуальных машин VMware без загрузочных драйверов storvsc, vmbus, storflt, intelide, atapi может занимать больше времени.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Подготовка к подключению виртуальных машин Azure после отработки отказа

Если после отработки отказа нужно подключиться к виртуальным машинам Azure по RDP или SSH, выполните требования, которые перечислены в [таблице](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).

Для устранения проблем с подключением после отработки отказа выполните шаги, описанные [здесь](site-recovery-failover-to-azure-troubleshoot.md).

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Выполнение отработки отказа и восстановления размещения локальных виртуальных машин VMware](vmware-azure-tutorial-failover-failback.md).
> [Выполнение отработки отказа и восстановления размещения локальных виртуальных машин Hyper-V](hyper-v-azure-failover-failback-tutorial.md).
