---
title: Расширенная защита данных в Базе данных SQL Azure | Документация Майкрософт
description: Узнайте о таких функциях, как обнаружение и классификация конфиденциальных данных, управление уязвимостями базы данных, а также выявление аномальной активности, которая может указывать на угрозу для базы данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: monhaber
ms.reviewer: vanto
manager: craigg
ms.date: 03/31/2019
ms.openlocfilehash: a078ac38cef5b395a19481188c474c7f908160d5
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2019
ms.locfileid: "58791615"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Расширенная защита данных для Базы данных SQL Azure

Расширенная защита данных представляет собой унифицированный пакет расширенных возможностей безопасности SQL. Он включает в себя функции для обнаружения и классификации конфиденциальных данных, обнаружения и устранения потенциальных уязвимостей базы данных и обнаружения аномальных действий, которые могут указывать на угрозу для вашей базы данных. Эта служба предоставляет единый центр для включения этих возможностей и управления ими.

## <a name="overview"></a>Обзор

Дополнительные данные безопасности (ADS) предоставляет набор расширенные возможности безопасности SQL, включая данные обнаружения и классификации, оценка уязвимостей и Advanced Threat Protection.

- Служба [обнаружения и классификации данных](sql-database-data-discovery-and-classification.md) (в настоящее время находится на этапе предварительной версии) предоставляет возможности, встроенные в службу "База данных SQL Azure", для обнаружения, классификации, добавления меток и защиты конфиденциальных данных в базах данных. Она может использоваться для просмотра состояния классификации базы данных, а также отслеживания доступа к конфиденциальным данным в базе данных и за ее пределами.
- [Оценка уязвимостей](sql-vulnerability-assessment.md) — это легко настраиваемая служба, которая помогает обнаруживать, отслеживать и устранять потенциальные уязвимости базы данных. Эта служба обеспечивает представление о состоянии безопасности и предлагает практические действия для устранения проблем безопасности и усиления защиты базы данных.
- [Advanced Threat Protection](sql-database-threat-detection-overview.md) выявляет аномальные операции, указывающие на нестандартные и потенциально вредоносные попытки получить доступ к или воспользоваться вашей базы данных. Она непрерывно отслеживает базу данных для выявления подозрительных действий и немедленно выдает оповещения системы безопасности о потенциальных уязвимостях, атаках путем внедрения кода SQL и аномальных шаблонах доступа к базам данных. Advanced Threat Protection оповещения содержат сведения о подозрительных операциях и рекомендации о том, как исследовать причину и устранить угрозы.

Включите службу ADS SQL, чтобы активировать все ее функции. Вы можете включить ADS для всех баз данных SQL на вашем сервере или управляемом экземпляре Базы данных SQL Azure. Включение параметров ADS и управление ими требуют принадлежности к роли [диспетчера безопасности SQL](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager), администратора базы данных SQL или администратора сервера SQL. 

Цена на ADS соответствует тарифу на Центр безопасности Azure цен. категории "Стандартный", причем каждый защищенный сервер или управляемый экземпляр Базы данных SQL считается как один узел. Созданные защищенные ресурсы в Центре безопасности Azure ценовой категории "Стандартный" предоставляются бесплатно в течение пробного периода. Дополнительные сведения см. на странице [цен на центр безопасности Azure](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Начало работы с ADS

Ниже указаны шаги по началу работы с ADS.

## <a name="1-enable-ads"></a>1. Включение ADS

Чтобы включить ADS, перейдите в **Расширенная защита данных** под заголовком **Безопасность** в вашем сервере или управляемом экземпляре Базы данных SQL. Чтобы включить ADS для всех баз данных на сервере или управляемом экземпляре базы данных, щелкните **Включить Расширенную защиту данных на сервере**.

> [!NOTE]
> Учетная запись хранения автоматически создается и настраивается для хранения вашей **оценка уязвимостей** результаты проверки. Если вы уже включили функцию РЕКЛАМЫ для другого сервера в той же группе ресурсов и регионе, то используется существующую учетную запись хранения.

![Включение ADS](./media/sql-advanced-protection/enable_ads.png) 

> [!NOTE]
> Цена на ADS соответствует тарифу на Центр безопасности Azure ценовой категории "Стандартный", причем под одним узлом подразумевается сервер или управляемый экземпляр Базы данных SQL. Таким образом, за защиту всех баз данных на сервере или управляемом экземпляре с помощью ADS плата взимается только один раз. Вы можете для начала воспользоваться бесплатной пробной версией ADS.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Запуск классификации данных, отслеживания уязвимостей и изучения оповещений об угрозах

Щелкните карту **Обнаружение и классификация данных**, чтобы просмотреть рекомендуемые для классификации конфиденциальные столбцы и классифицировать данные с помощью постоянных меток конфиденциальности. Щелкните карту **Оценка уязвимостей**, чтобы просмотреть отчеты о поиске уязвимостей, управлять проверками и отслеживать состояние безопасности. Если были получены оповещения системы безопасности, нажмите кнопку **Advanced Threat Protection** карты для просмотра сведений, предупреждений, а консолидированный отчет на все предупреждения в подписку Azure через страница оповещений безопасности центра безопасности Azure .

## <a name="3-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>3. Управление настройками ADS на сервере или в управляемом экземпляре Базы данных SQL

Чтобы просмотреть параметры ADS и управлять ими, щелкните **Расширенная защита данных** под заголовком **Безопасность** в своем сервере или управляемом экземпляре Базы данных SQL. На этой странице можно включить или отключить ADS и изменить уязвимость оценки и параметры Advanced Threat Protection для всей базы данных SQL server или управляемого экземпляра.

![Параметры сервера](./media/sql-advanced-protection/server_settings.png) 

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. Управление параметрами ADS для базы данных SQL

Чтобы переопределить параметры ADS для конкретной базы данных, установите флажок, позволяющий **включить расширенную защиту данных на уровне базы данных**. Используйте этот параметр только в том случае, если у вас есть определенный требование получать отдельные предупреждения Advanced Threat Protection или результаты оценки уязвимостей для отдельной базы данных, вместо или в дополнение к оповещения и результаты, полученные для всех баз данных на сервер базы данных или управляемого экземпляра.

Когда флажок установлен, можно затем настроить соответствующие параметры для этой базы данных.
 
![Параметры базы данных и Advanced Threat Protection](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Параметры расширенной защиты данных для вашего сервера или управляемого экземпляра также доступны из панели базы данных ADS. Щелкните **Параметры** в основной области ADS, а затем выберите **Просмотреть параметры сервера Расширенной защиты данных**. 

![Параметры базы данных](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Дальнейшие действия 

- Дополнительные сведения об [обнаружении и классификации данных](sql-database-data-discovery-and-classification.md). 
- Дополнительные сведения об [оценке уязвимостей](sql-vulnerability-assessment.md). 
- Дополнительные сведения о [Advanced Threat Protection](sql-database-threat-detection.md)
- Дополнительные сведения о [Центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro).
