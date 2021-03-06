---
title: Использование сайта решений Интернета вещей Azure — Azure | Документация Майкрософт
description: В этой статье описывается использование веб-сайта AzureIoTSolutions.com для развертывания акселератора решений.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: dobett
ms.openlocfilehash: 87f6b9cef50e4b8c388be835b2aa7bed8177ac4b
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601089"
---
# <a name="use-the-azureiotsolutionscom-site-to-deploy-your-solution-accelerator"></a>Использование сайта azureiotsolutions.com для развертывания акселератора решений

Акселераторы решений Интернета вещей Azure можно развернуть в подписке Azure на сайте [AzureIoTSolutions.com](https://www.azureiotsolutions.com/Accelerators). На сайте AzureIoTSolutions.com вы найдете акселераторы решений Microsoft с открытым исходным кодом и акселераторы решений от партнеров. Эти акселераторы решений соответствуют [эталонной архитектуре Интернета вещей Azure](https://aka.ms/iotrefarchitecture). Сайт можно использовать для быстрого развертывания акселератора решений в качестве демонстрационной или производственной среды.

![AzureIoTSolutions.com](media/iot-accelerators-permissions/iotsolutionscom.png)

> [!TIP]
> Если вам требуется больший контроль над процессом развертывания, можно использовать [CLI для развертывания акселератора решений](iot-accelerators-remote-monitoring-deploy-cli.md).

Акселераторы решений можно развернуть в следующих конфигурациях.

* **Стандартный**. Развертывание расширенной инфраструктуры для разработки в рабочей среде. С помощью Службы контейнеров Azure можно развернуть микрослужбы на нескольких виртуальных машинах Azure. Kubernetes управляет контейнерами Docker, на которых размещаются отдельные микрослужбы.
* **Базовый**. Версия с низкой стоимостью для демонстрации или тестирования развертывания. Все микрослужбы развертываются на одной виртуальной машине Azure.
* **Локальные среды**. Развертывание на локальном компьютере для тестирования и разработки. При этом подходе выполняется развертывание микрослужб в локальном контейнере Docker и подключение к облачным службам: Центр Интернета вещей, база данных Azure Cosmos DB и служба хранения Azure.

Каждый акселератор решений использует свое сочетание служб Azure, например Центр Интернета вещей, Azure Stream Analytics и Cosmos DB. Чтобы узнать больше, посетите сайт [AzureIoTSolutions.com](https://www.azureiotsolutions.com/Accelerators) и выберите акселератор решений.

## <a name="sign-in-at-azureiotsolutionscom"></a>Вход на azureiotsolutions.com

Перед развертыванием акселератора решений необходимо войти на сайт AzureIoTSolutions.com, используя учетные данные, связанные с подпиской Azure. Если ваша учетная запись связана с несколькими клиентами Microsoft Azure Active Directory (AD), используйте **раскрывающийся список выбора учетной записи**, чтобы выбрать каталог для использования.

Ваши разрешения на развертывание акселераторов решений, управление пользователями и управление службами Azure зависят от вашей роли в выбранном каталоге. Вот типичные роли Azure AD, связанные с акселераторами решений.

* **Глобальный администратор**. В одном клиенте AAD может быть несколько [глобальных администраторов](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

  * Создавая клиент AAD, вы по умолчанию становитесь его глобальным администратором.
  * Глобальный администратор может развертывать базовые и стандартные акселераторы решений.

* **Пользователь домена**. На каждый клиент AAD может приходиться множество пользователей домена. Пользователь домена может развертывать базовый акселератор решений.

* **Гостевой пользователь**. В одном клиенте AAD может быть множество гостевых пользователей. Гостевые пользователи не могут развертывать акселератор решений в клиенте Azure AD.

Дополнительные сведения о пользователях и ролях в Azure AD см. в следующих ресурсах.

* [Создание пользователей в Azure Active Directory](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
* [Назначение пользователей приложениям](../active-directory/manage-apps/assign-user-or-group-access-portal.md)

## <a name="choose-your-device"></a>Выбор устройства

На сайте AzureIoTSolutions.com есть ссылки на [Каталог устройств, сертифицированных по программе Microsoft Azure Certified for IoT](https://catalog.azureiotsolutions.com/).

В каталоге перечислены сотни сертифицированных устройств Интернета вещей, которые вы можете подключать к акселераторам решений для создания своего решения Интернета вещей.

![Каталог устройств](media/iot-accelerators-permissions/devicecatalog.png)

Если вы изготовитель оборудования, нажмите **Стать партнером**, чтобы узнать больше о партнерстве с корпорацией Майкрософт по программе Microsoft Azure Certified for IoT.

## <a name="next-steps"></a>Дополнительная информация

Чтобы опробовать один из акселераторов решений Интернета вещей, см. следующие краткие руководства.

* [Краткое руководство. Использование облачного решения для удаленного мониторинга](quickstart-remote-monitoring-deploy.md)
* [Краткое руководство. Пробное использование облачного решения для управления промышленными устройствами Центра Интернета вещей](quickstart-connected-factory-deploy.md)
* [Краткое руководство. Использование облачных решений для анализа прогнозного обслуживания на подключенных устройствах](quickstart-predictive-maintenance-deploy.md)
* [Краткое руководство по развертыванию и запуску решения "Симулятор устройств" на основе облака](quickstart-device-simulation-deploy.md)
