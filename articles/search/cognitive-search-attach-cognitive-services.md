---
title: Подключение ресурса Cognitive Services с набором навыков в службе "Поиск Azure"
description: Инструкции по подключению подписки всех служб Cognitive Services к когнитивному конвейеру обогащения в службе "Поиск Azure".
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: d5fdae09055f922fe9783f6eb074457af12c60df
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880421"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Подключение ресурса Cognitive Services с набором навыков в службе "Поиск Azure" 

Алгоритмы ИИ управляют [конвейерами когнитивного поиска](cognitive-search-concept-intro.md), которые используются для обработки неструктурированных данных в операциях индексирования службы "Поиск Azure". Эти алгоритмы основаны на [ресурсах Cognitive Services](https://azure.microsoft.com/services/cognitive-services/), включая [API компьютерного зрения](https://azure.microsoft.com/services/cognitive-services/computer-vision/) для анализа изображений и оптического распознавания символов (OCR), а также [API анализа текста](https://azure.microsoft.com/services/cognitive-services/text-analytics/) для распознавания сущностей, извлечения ключевых фраз и других возможностей.

Для увеличения рабочих нагрузок вы можете бесплатно расширить ограниченное число документов или присоединить оплачиваемый ресурс Cognitive Services для больших и более частых рабочих нагрузок. Из этой статьи вы узнаете, как связать ресурсы Cognitive Services с когнитивным набором навыков, чтобы дополнить данные во время [индексации службы "Поиск Azure"](search-what-is-an-index.md).

Если ваш конвейер состоит из навыков, не касающихся API-интерфейсов Cognitive Services, необходимо все равно вложить ресурс Cognitive Services. Это переопределяет **бесплатный** ресурс, который допускает только небольшое количество возможностей в день. Плата за навыки, которые не привязаны к API-интерфейсам Cognitive Services, не взимается. Эти навыки включают в себя: [пользовательские навыки](cognitive-search-create-custom-skill-example.md), [слияние текста](cognitive-search-skill-textmerger.md), [разделение текста](cognitive-search-skill-textsplit.md) и [формирователь](cognitive-search-skill-shaper.md).

> [!NOTE]
> С 21 декабря 2018 г. можно связывать ресурсы Cognitive Services с набором навыков службы "Поиск Azure". Это позволяет нам взимать плату за выполнение набора навыков. С этого момента мы также начали начислять плату за извлечение изображений при открытии документов. Извлечение текста из документов будет выполняться бесплатно, как и прежде.
>
> За операции с применением [встроенных навыков](cognitive-search-predefined-skills.md) взимается [плата по мере использования по тарифам для служб Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services), как если бы вы выполнили эту задачу непосредственно. Плата за извлечение изображения взимается по тарифам службы "Поиск Azure", которая сейчас предлагается по цене предварительной версии. Дополнительные сведения см. на странице [Цены на Поиск Azure](https://go.microsoft.com/fwlink/?linkid=2042400) и в разделе [Как работает выставление счетов](search-sku-tier.md#how-billing-works).


## <a name="use-free-resources"></a>Использование бесплатных ресурсов

Чтобы выполнить упражнения краткого руководства и руководства по когнитивному поиску, вы можете использовать ограниченный бесплатный вариант обработки. 

> [!Important]
> С 1 февраля 2019 года версия **Бесплатные (ограниченные обогащения данных)** будет ограничиваться только 20 документами в день. 

1. Откройте мастер **Импорт данных**.

   ![Команда импорта данных](media/search-get-started-portal/import-data-cmd2.png "Команда импорта данных")

1. Выберите источник данных и затем нажмите **Добавить когнитивный поиск (необязательно)**. Пошаговое руководство об этом мастере вы можете прочесть в статье [Краткое руководство. Использование встроенных инструментов на портале для импорта, индексирования и отправки запросов в Поиске Azure](search-get-started-portal.md).

1. Разверните **Подключение Cognitive Services** и выберите **.**

   ![Открыт раздел подключения Cognitive Services](./media/cognitive-search-attach-cognitive-services/attach1.png "Expanded Attach Cognitive Services").

Перейдите к следующему шагу, **Добавление обогащений**. Чтобы узнать о навыках, доступных на портале, откройте раздел ["Шаг 2. Добавление когнитивных навыков"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) в кратком руководстве о когнитивном поиске.

## <a name="use-billable-resources"></a>Использование ресурсов, которые подлежат оплате

Для рабочих нагрузок, превышающих 20 возможностей ежедневно, вам необходимо вложить оплачиваемый ресурс Cognitive Services. 

Плата взымается только за навыки, которые вызывают API-интерфейсы Cognitive Services. За навыки, которые основаны не на API, такие как [пользовательские навыки](cognitive-search-create-custom-skill-example.md), [слияние текста](cognitive-search-skill-textmerger.md), [разделение текста](cognitive-search-skill-textsplit.md) и [формирователь](cognitive-search-skill-shaper.md), плата не взымается.

1. В мастере **Импорт данных** в области **Подключение Cognitive Services** выберите существующий ресурс или щелкните **Создать ресурс Cognitive Services**.

1. Для варианта **Создать ресурс Cognitive Services** откроется новая вкладка, в которой можно создать ресурс. Присвойте ресурсу уникальное имя.

1. Если вы создаете новый ресурс Cognitive Services, **выберите тот же регион** что и ресурс службы поиска Azure.

1. Выберите ценовую категорию "все в одном", **S0**. Эта категория предоставляет вам возможности языка и зрения, которые поддерживают предопределенные навыки в когнитивном поиске.

    ![Создать новый ресурс Cognitive Services](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Создать новый ресурс Cognitive Services")

1. Щелкните **Создать**, чтобы зарегистрировать новый ресурс Cognitive Services. 

1. Вернитесь к предыдущей вкладке с мастером **Импорт данных**. Щелкните **Обновить**, чтобы увидеть ресурс Cognitive Services, а затем выберите его.

   ![Выбранный ресурс Cognitive Service](./media/cognitive-search-attach-cognitive-services/attach2.png "Selected Cognitive Service Resource")

1. Разверните раздел **Добавить обогащение**, чтобы выбрать конкретные когнитивные навыки, которые необходимо использовать для своих данных, и продолжить работу с оставшимся потоком. Чтобы узнать о навыках, доступных на портале, откройте раздел ["Шаг 2. Добавление когнитивных навыков"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) в кратком руководстве о когнитивном поиске.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Присоединение ресурса Cognitive Services с существующим набором навыков

Если вы уже имеете существующий набор навыков, вы можете присоединить его к новому или другому ресурсу Cognitive Services.

1. На странице **Обзор службы** щелкните **Наборы навыков**.

   ![Вкладка наборов навыков](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Вкладка наборов навыков")

1. Выберите имя для набора навыков и затем выберите существующий ресурс или создайте новый. Для подтверждения изменений нажмите кнопку **OK**. 

   ![Список набора навыков ресурса](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Список набора навыков ресурса")

Не забывайте, что **бесплатная версия (ограниченные обогащения данных)** ограничивается только 20 документами в день и что **Создать ресурс Cognitive Services** используется, чтобы зарегистрировать новый оплачиваемый ресурс. Создав новый ресурс, щелкните **Обновить**, чтобы обновить список ресурсов Cognitive Services, и выберите ресурс.

## <a name="attach-cognitive-services-programmatically"></a>Присоединение Cognitive Services программным способом

При определении набора навыков программным способом добавьте раздел `cognitiveServices` к набору навыков. В разделе включите ключ ресурса Cognitive Services, который хотите связать с набором навыков. Не забывайте, что ресурс должен находиться в том же регионе, что и служба "Поиск Azure". Также включите `@odata.type` и присвойте ему значение `#Microsoft.Azure.Search.CognitiveServicesByKey`. 

Этот шаблон приведен в примере ниже. Обратите внимание на раздел cognitiveServices в нижней части определения

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Пример: Расчет стоимости

Чтобы рассчитать стоимость, связанную с индексацией когнитивного поиска, начните с понимания, каков средний размер документа, чтобы иметь возможность проанализировать некоторые цифры. К примеру, для приблизительной оценки допустим:

+ 1000 файлов PDF;
+ по шесть страниц каждый;
+ по одному образу на странице (6000 образов);
+ З000 символов в месяц.

Предположим, что конвейер состоит из открытия документа с извлечением из каждого файла PDF изображения и текста, анализа изображений и распознавания текста (OCR), а также распознавания именованных сущностей организаций. 

В этом упражнении мы взяли самый дорогой тариф за одну транзакцию. Фактические расходы могут быть и ниже в связи с дифференцированной тарификацией. См. [Цены на Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Для открытия документа с содержимым в виде текста и изображения извлечение текста в настоящее время предоставляется бесплатно. Для 6000 образов, допустим по 1 доллару за извлечение каждых 1000 образов, общая стоимость на этом этапе равна 6,00 долларов.

2. Для OCR 6000 образов на английском языке когнитивные навыки OCR используют наилучший алгоритм (DescribeText). При условии что 1000 изображений для анализа будет стоить 2,50 долл., вы выплатите 15,00 долл. за этот шаг.

3. Для извлечения сущности нужно всего 3 текстовые записи на страницу (каждая запись — 1000 символов). Три текстовые записи на страницу * 6000 страниц = 18 000 текстовых записей. При тарифе 2,00 долл. за 1000 текстовых записей этот шаг будет стоить 36,00 долл.

Всего вы заплатите приблизительно 57,00 долларов, чтобы принять 1000 PDF-документов такого характера с описанным набором навыков. 

## <a name="next-steps"></a>Дальнейшие действия
+ [Цены на "Поиск Azure"](https://azure.microsoft.com/pricing/details/search/)
+ [Определение набора навыков](cognitive-search-defining-skillset.md)
+ [Создание набора навыков (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Сопоставление обогащенных полей](cognitive-search-output-field-mapping.md)
