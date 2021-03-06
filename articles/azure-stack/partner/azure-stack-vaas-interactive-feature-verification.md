---
title: Проверочное тестирование функций в интерактивном режиме по модели "проверка как услуга" для Azure Stack | Документация Майкрософт
description: Узнайте, как создать тесты для проверки функций в интерактивном режиме по модели "проверка как услуга" для Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: d3db8ea8639f73f3522ddaa358195e7c9ef2f9a9
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57766008"
---
# <a name="interactive-feature-verification-testing"></a>Тестирование интерактивной проверки функций  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

На платформе тестирования для проверки функций в интерактивном режиме вы можете подать заявку на подготовку теста для вашей системы. Когда вы подаете заявку, специалисты Майкрософт с помощью платформы подготавливают тесты, этапы которых выполняются вручную и в интерактивном режиме. С помощью платформы специалисты Майкрософт могут объединить несколько отдельных автоматических тестов.

В этой статье описан простой сценарий, выполняемый вручную. Тест позволяет проверить замену диска в Azure Stack. На каждом этапе платформа собирает данные журнала диагностики. Вы можете отлаживать проблемы по мере их обнаружения. Платформа также позволяет отправлять журналы, созданные с использованием других средств или процессов, и оставлять отзывы о сценарии.

> [!Important]  
> В этой статье описаны этапы идентификации диска. Это просто демонстрация. Результаты, собранные в процессе прохождения теста, нельзя использовать для проверки нового решения.

## <a name="overview-of-interactive-testing"></a>Обзор интерактивного тестирования

Тест для выявления замены диска проводится в широком ряде сценариев. В этом примере тест состоит из пяти этапов.

1. Создание рабочего процесса **прохождения теста**.
2. Выбор **Disk Identification Test** (Тест на идентификацию диска).
3. Выполнение вручную следующего этапа после соответствующего запроса.
4. Проверка результатов выполнения сценария.
5. Отправка результатов теста специалистам корпорации Майкрософт.

## <a name="create-a-new-test-pass"></a>Создание процесса прохождения теста

Если у вас нет готового процесса прохождения теста, [запланируйте тест](azure-stack-vaas-schedule-test-pass.md), выполнив инструкции по ссылке.

## <a name="schedule-the-test"></a>Планирование теста

1. Выберите **Disk Identification Test** (Тест на идентификацию диска).

    > [!Note]  
    > Номер версии будет увеличиваться по мере того, как будут вноситься изменения во вспомогательные тесты. Всегда будет использоваться последняя версия, если Майкрософт не укажет иное.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image4.png)

1. Выберите **Edit** (Изменить) и укажите имя и пароль администратора домена.

1. Выберите агент или виртуальную машину для развертывания, где будет запущен тест.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image5.png)

1. Нажмите кнопку **Submit** (Отправить) для запуска теста.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image6.png)

1. В выбранном агенте перейдите в интерфейс для интерактивного теста.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image8.png)

1. Перейдите по ссылкам **Documentation** (Документация) и **Validation** (Проверка), чтобы ознакомиться с инструкциями от корпорации Майкрософт по выполнению этого сценария.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image9.png)

1. Щелкните **Далее**.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image10.png)

1. Следуйте инструкциям по выполнению скрипта предварительной проверки.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image11.png)

1. Когда скрипт предварительной проверки будет успешно завершен, выполните вручную сценарий (замена диска) согласно инструкциями, приведенным по ссылкам **Documentation** (Документация) и **Validation** (Проверка) на вкладке **Information** (Сведения).

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image12.png)

    > [!Important]  
    > Не закрывайте диалоговое окно, когда выполняете сценарий вручную.

1. По завершении этого сценария следуйте инструкциям по выполнению скрипта последующей проверки.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image13.png)

1. Когда выполняемый вручную сценарий (замена диска) будет успешно завершен, нажмите кнопку **Next** (Далее).

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image14.png)

    > [!Important]  
    > Если закрыть окно, тест остановится до того, как вы его завершите.

1. Оставьте отзыв о процессе тестирования. Ваши вопросы помогут нам оценить результативность и степень готовности этого сценария к выпуску.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image15.png)

1. Присоедините все файлы журналов, которые нужно отправить в корпорацию Майкрософт.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image16.png)

1. Примите условия лицензионного соглашения для отправки отзывов.

1. Нажмите кнопку **Submit** (Отправить), чтобы отправить результаты специалистам корпорации Майкрософт.

## <a name="next-steps"></a>Дополнительная информация

- [Мониторинг теста с помощью проверки как услуги Azure Stack](azure-stack-vaas-monitor-test.md)
