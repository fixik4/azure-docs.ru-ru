---
title: Проверка нового решения Azure Stack | Документация Майкрософт
description: Узнайте, как проверить новое решение Azure Stack с помощью проверки как услуги.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: fb7b98a5307606f7335e76ada42d583b975730f5
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759669"
---
# <a name="validate-a-new-azure-stack-solution"></a>Проверка нового решения Azure Stack

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Узнайте, как использовать рабочий процесс **проверки решения** для сертификации новых решений Azure Stack.

Решение Azure Stack является спецификацией оборудования, которая была совместно согласована между корпорацией Майкрософт и партнером и которая соответствует сертификационным требованиям логотипа Windows Server. Решение должно быть повторно сертифицировано при изменении спецификации оборудования. При возникновении дополнительных вопросов о том, когда необходимо повторно сертифицировать решения, свяжитесь с командой по адресу [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com).

Чтобы сертифицировать решение, необходимо дважды запустить рабочий процесс проверки решений. Запустите его один раз для *минимально* поддерживаемой конфигурации. Запустите его второй раз для *максимально* поддерживаемой конфигурации. Корпорация Майкрософт сертифицирует решение, если обе конфигурации прошли все тесты.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Создание рабочего процесса проверки решений

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Щелкните **Start** (Начать) на плитке **Solution Validations** (Проверка решений).

    ![Плитка рабочего процесса проверки решений](media/tile_validation-solution.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Выберите **конфигурацию решения**:
    - **Минимум** — для решения, настроенного с минимальным числом поддерживаемых узлов.
    - **Максимум** — для решения, настроенного с максимальным числом поддерживаемых узлов.
6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Сведения о проверке решения](media/workflow_validation-solution_info.png)

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Параметры среды невозможно изменить после создания рабочего процесса.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Вы будете перенаправлены на страницу сводок по тестам.

## <a name="run-solution-validation-tests"></a>Выполнение тестов проверки решений

На странице **сводки по тестам проверки решений** вы увидите список тестов, необходимых для завершения проверки.

В рабочих процессах проверки при **планировании** теста используются общие параметры уровня рабочего процесса, которые вы указали во время его создания (см. раздел [Workflow common parameters for Azure Stack Validation as a Service](azure-stack-vaas-parameters.md) (Общие параметры рабочих процессов для проверки как услуги в Azure Stack)). Если какие-либо из значений параметров станут недействительными, необходимо указать их повторно, как описано в разделе об [изменении параметров рабочего процесса](azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> При планировании проверочного теста через существующий экземпляр на портале будет создан новый экземпляр вместо старого. Журналы для старого экземпляра будут сохранены, но недоступны на портале.  
После успешного завершения теста действие **Расписание** становится недоступным.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]

2. Выберите следующие тесты:
    - Cloud Simulation Engine;
    - Compute SDK Operational Suite (Операционный набор для пакета SDK для вычислений);
    - Disk Identification Test (Тест на идентификацию диска);
    - KeyVault Extension SDK Operational Suite (Операционный набор для пакета SDK для расширения хранилища ключей);
    - KeyVault SDK Operational Suite (Операционный набор для пакета SDK для хранилища ключей);
    - Network SDK Operational Suite (Операционный набор для пакета SDK для сети);
    - Storage Account SDK Operational Suite.

3. В контекстном меню выберите **Расписание**, чтобы открыть командную строку для планирования выполнения тестового экземпляра.

4. Проверьте параметры теста, а затем выберите **Отправить**, чтобы запланировать выполнение теста.

![Выполнение теста проверки решений](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Дополнительная информация

- [Мониторинг теста с помощью проверки как услуги Azure Stack](azure-stack-vaas-monitor-test.md)