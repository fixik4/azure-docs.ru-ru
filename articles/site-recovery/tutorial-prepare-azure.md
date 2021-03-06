---
title: Подготовка Azure для аварийного восстановления локальных компьютеров с помощью Azure Site Recovery | Документация Майкрософт
description: Узнайте, как подготовить Azure для аварийного восстановления локальных компьютеров с помощью Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8bdb711d39f514375362235388943ec42451b312
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315578"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Подготовка ресурсов Azure для аварийного восстановления локальных компьютеров

 Служба [Azure Site Recovery](site-recovery-overview.md) помогает реализовать стратегию непрерывности бизнес-процессов и аварийного восстановления (BCDR), обеспечивая работоспособность бизнес-приложений во время запланированных и незапланированных простоев. Site Recovery управляет аварийным восстановлением локальных компьютеров и виртуальных машин Azure, включая операции репликации, отработки отказа и восстановления.

Эта статья является первой в серии руководств, в которой показано, как настроить аварийное восстановление для локальных виртуальных машин. Эта информация пригодится вам для защиты локальных виртуальных машин VMware и Hyper-V, а также физических серверов.

> [!NOTE]
> В руководствах приводится самый простой способ развертывания для сценария. В них везде, где возможно, используются значения по умолчанию, и описаны не все возможные параметры и пути. Подробные инструкции см. в разделе **Инструкции** для соответствующего сценария.

В этой статье показано, как подготовить компоненты Azure для репликации локальных виртуальных машин (Hyper-V или VMware) и физических серверов Windows или Linux в Azure. Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Убедитесь, что ваша учетная запись Azure предоставляет разрешения на репликацию.
> * Создайте хранилище служб восстановления, которое содержит метаданные и сведения о конфигурации для виртуальных машин и другие компоненты репликации.
> * Настройка сети Azure. При создании виртуальных машин Azure после отработки отказа они присоединяются к этой сети.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/), прежде чем начинать работу.

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на [портале Azure](https://portal.azure.com).

## <a name="verify-account-permissions"></a>Проверка разрешений учетной записи

Если вы создали бесплатную учетную запись Azure, вы являетесь администратором подписки. Если вы не администратор подписки, свяжитесь с администратором, чтобы получить необходимые разрешения. Чтобы включить репликацию для новой виртуальной машины, необходимо иметь следующие разрешения:

- разрешение на создание виртуальной машины в выбранной группе ресурсов;
- разрешение на создание виртуальной машины в выбранной виртуальной сети;
- разрешение на запись в учетную запись хранения;
- разрешение на запись в управляемый диск.

Для выполнения этих задач вашей учетной записи нужно назначить встроенную роль участника виртуальных машин. Кроме того, для управления операциями Site Recovery в хранилище вашей учетной записи нужно назначить встроенную роль участника Site Recovery.


## <a name="create-a-recovery-services-vault"></a>Создание хранилища служб восстановления

1. На портале Azure щелкните **+Создать ресурс**, а затем в Marketplace выберите **Службы восстановления**.
2. Щелкните **Backup and Site Recovery (OMS)** и на соответствующей странице нажмите кнопку **Создать**. 
1. В поле **Хранилище служб восстановления** > **Имя** введите понятное имя для идентификации хранилища. В этой серии руководств используется имя **ContosoVMVault**.
2. В разделе **Группа ресурсов** создайте группу ресурсов или выберите имеющуюся. При работе с этим руководством используется **contosoRG**.
3. В поле **Расположение** выберите регион, в котором будет размещаться хранилище. выбираем значение **Западная Европа**.
4. Для быстрого доступа к новому хранилищу с панели мониторинга выберите **Закрепить на панели мониторинга** > **Create** (Создать).

   ![Создание хранилища](./media/tutorial-prepare-azure/new-vault-settings.png)

   Новое хранилище появится в колонке **Dashboard** (Панель мониторинга)  > **All resources** (Все ресурсы) и на основной странице **Хранилища служб восстановления**.

## <a name="set-up-an-azure-network"></a>Настроить сеть

При создании виртуальных машин Azure из управляемых дисков после отработки отказа они присоединяются к сети.

1. На [портале Azure](https://portal.azure.com) выберите **Создать ресурс** > **Networking** (Сеть)  > **Виртуальная сеть**.
2. Сохраните вариант **Resource Manager** в качестве модели развертывания.
3. В разделе **Name** (Имя) введите имя сети. Имя должно быть уникальным в пределах группы ресурсов Azure. При работе с этим руководством используется имя **ContosoASRnet**.
4. Укажите группу ресурсов, в которой будет создана сеть. Мы используем уже существующую группу ресурсов **contosoRG**.
5. В поле **Диапазон адресов** введите диапазон для сети **10.0.0.0/24**. Подсети для этой сети не используются.
6. В разделе **Subscription** (Подписка) выберите подписку, в которой будет создана сеть.
7. В разделе **Расположение** выберите **Западная Европа**. Сеть должна располагаться в том же регионе, что и хранилище служб восстановления.
8. Мы сохраняем значения по умолчанию: базовые средства защиты от атак DDoS и отсутствие конечной точки службы в сети.
9. Нажмите кнопку **Создать**.

   ![Создать виртуальную сеть](media/tutorial-prepare-azure/create-network.png)

   Создание виртуальной сети занимает несколько секунд. После завершения этого процесса она отобразится на панели мониторинга портала Azure.

## <a name="useful-links"></a>Полезные ссылки

- [Описание сетей Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).
- [Общие сведения об управляемых дисках Azure](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).



## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Подготовка локальных серверов VMware для аварийного восстановления в Azure](tutorial-prepare-on-premises-vmware.md)
