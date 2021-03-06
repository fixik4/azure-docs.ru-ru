---
title: Развертывание Kubernetes в Azure Stack с помощью Azure Active Directory (Azure AD) | Документация Майкрософт
description: Узнайте, как развернуть Kubernetes в Azure Stack с помощью Azure Active Directory (Azure AD).
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 6e4402be7108f242e1d285ebe91dfece744f0805
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57532156"
---
# <a name="deploy-kubernetes-to-azure-stack-using-azure-active-directory"></a>Развертывание Kubernetes в Azure Stack с помощью Azure Active Directory

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

> [!Note]  
> Система Kubernetes доступна в Azure Stack в предварительной версии. Сейчас в предварительной версии не поддерживаются сценарии работы с Azure Stack в автономном режиме.

Вы можете выполнить действия, описанные в этой статье, чтобы развернуть и настроить ресурсы для Kubernetes при использовании Azure Active Directory (Azure AD) в качестве службы управления удостоверениями в одной скоординированной операции.

## <a name="prerequisites"></a>Предварительные требования

Прежде чем начать работу, убедитесь в наличии необходимых разрешений и в готовности Azure Stack.

1. Проверьте, можете ли вы создавать приложения в клиенте Azure Active Directory (Azure AD). Эти разрешения потребуются для развертывания Kubernetes.

    Инструкции по проверке разрешений см. в разделе [Check Azure Active Directory permissions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) (Проверка разрешений Azure Active Directory).

1. Создайте пару открытого и закрытого ключей SSH для входа на виртуальную машину Linux в Azure Stack. Открытый ключ потребуется при создании кластера.

    Инструкции по создания ключа см. в разделе [SSH Key Generation](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation) (Создание ключа SSH).

1. Убедитесь в наличии действующей подписки на портале клиента Azure Stack, а также достаточного количества общедоступных IP-адресов для добавления новых приложений.

    Кластер не может быть развернут в подписке **администратора** Azure Stack. Необходимо использовать подписку **пользователя**. 

1. Если в Marketplace нет кластера Kubernetes, обратитесь к администратору Azure Stack.

## <a name="create-a-service-principal"></a>Создание субъекта-службы

Настройка субъекта-службы в Azure AD Субъект-служба предоставляет приложению доступ к ресурсам Azure Stack.

1. Войдите на глобальный [портал Azure](https://portal.azure.com).

1. Убедитесь, что при входе используется клиент Azure AD, связанный с экземпляром Azure Stack. Вы можете переключиться для входа, щелкнув значок фильтра на панели инструментов Azure.

    ![Выбор клиента AD](media/azure-stack-solution-template-kubernetes-deploy/tenantselector.png)

1. Создадим приложение Azure AD.

    a. Последовательно выберите элементы **Azure Active Directory** > **+ Регистрация приложений** > **Регистрация нового приложения**.

    b. Введите **имя** приложения.

    c. Выберите **Веб-приложение или API**.

    d. Введите `http://localhost` в поле **URL-адрес для входа**.

    c. Нажмите кнопку **Создать**.

1. Запишите значение **идентификатора приложения**. Идентификатор потребуется при создании кластера. Этот идентификатор называется **идентификатором клиента для субъекта-службы**.

1. Последовательно выберите **Параметры** > **Ключи**.

    a. Введите **описание**.

    b. Выберите для параметра **Срок действия** значение **Срок действия неограничен**.

    c. Щелкните **Сохранить**. Запишите строку ключа. Строка ключа потребуется при создании кластера. Этот ключ называется **секретом клиента субъекта-службы**.

## <a name="give-the-service-principal-access"></a>Предоставление доступа субъекту-службе

Предоставьте субъекту-службе доступ к вашей подписке, чтобы субъект мог создать ресурсы.

1.  Войдите на [портал Azure Stack](https://portal.local.azurestack.external/).

1. Выберите **Все службы** > **Подписки**.

1. Выберите подписку, созданную вашим оператором для использования кластера Kubernetes.

1. Выберите **Управление доступом (IAM)**, а затем **Добавить назначение ролей**.

1. Выберите роль **Участник**.

1. Выберите имя приложения, созданное для субъекта-службы. Возможно, потребуется ввести имя в поле поиска.

1. Выберите команду **Сохранить**.

## <a name="deploy-kubernetes"></a>Развертывание Kubernetes

1. Откройте [портал Azure Stack](https://portal.local.azurestack.external).

1. Выберите **+Создать ресурс** > **Служба вычислений** > **Кластер Kubernetes**. Нажмите кнопку **Создать**.

    ![Развертывание шаблона решений](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Основы

1. В колонке "Создание кластера Kubernetes" выберите **Основные сведения**.

    ![Развертывание шаблона решений](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Выберите идентификатор **подписки**.

1. Введите имя новой группы ресурсов или выберите существующую. Имя ресурса должно содержать буквенно-цифровые символы. Оно вводится в нижнем регистре.

1. Выберите **расположение** группы ресурсов. Это регион, выбранный для установки Azure Stack.

### <a name="2-kubernetes-cluster-settings"></a>2. Параметры кластера Kubernetes

1. В колонке "Создание кластера Kubernetes" выберите **Kubernetes Cluster Settings** (Параметры кластера Kubernetes).

    ![Развертывание шаблона решений](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-aad.png)

1. Введите **имя пользователя администратора виртуальной машины Linux**. Это имя пользователя для виртуальных машин Linux, которые являются частью кластера Kubernetes и динамического административного представления.

1. Введите **открытый ключ SSH**, используемый для авторизации на всех компьютерах Linux, созданных как часть кластера Kubernetes и динамического административного представления.

1. Введите **префикс DNS главного профиля**, уникальный для региона. Это уникальное для региона имя, например `k8s-12345`. Рекомендуем выбрать то же имя, что и для группы ресурсов.

    > [!Note]  
    > Для каждого кластера используйте новый уникальный префикс DNS главного профиля.

1. Выберите **Kubernetes Master Pool Profile Count** (Счетчик профилей главного пула Kubernetes). Счетчик содержит количество узлов в главном пуле. Здесь можно указать значение от 1 до 7, но оно должно быть нечетным.

1. Выберите **The VMSize of the Kubernetes master VMs** (Размер главных виртуальных машин Kubernetes).

1. Выберите **Kubernetes Node Pool Profile** (Счетчик профилей узлов Kubernetes). Счетчик содержит число агентов в кластере. 

1. Выберите **профиль хранилища**. Можно выбрать **Blob Disk** (Диск больших двоичных объектов) или **Managed Disk** (Управляемый диск). Так указывается размер виртуальных машин узлов Kubernetes. 

1. Выберите **Azure AD** в качестве **системы удостоверений Azure Stack** для установки Azure Stack. 

1. Введите **идентификатор клиента субъекта-службы**. Он используется поставщиком облачных служб Azure для Kubernetes. Идентификатор клиента определяется как идентификатор приложения во время создания субъекта-службы.

1. Введите **секрет клиента субъекта-службы**, полученный при создании субъекта-службы.

1. Введите **версию поставщика облачных служб Azure для Kubernetes**. Это версия поставщика Azure для Kubernetes. Azure Stack выпускает специальную сборку Kubernetes для каждой версии Azure Stack.

### <a name="3-summary"></a>3. Сводка

1. Выберите "Сводка". В колонке отобразится сообщение проверки параметров конфигурации кластера Kubernetes.

    ![Развертывание шаблона решений](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Проверьте настройки.

3. Щелкните **ОК**, чтобы развернуть кластер.

> [!TIP]  
>  Если у вас есть вопросы о развертывании, вы можете разместить свой вопрос или поискать ответы на вопрос на [форуме Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack).


## <a name="next-steps"></a>Дополнительная информация

[Подключение к кластеру](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)

[Получение доступа к панели мониторинга Kubernetes в Azure Stack](azure-stack-solution-template-kubernetes-dashboard.md)
