---
title: Заметки о выпуске Пользовательской службы визуального распознавания
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: anroth
ms.openlocfilehash: 2b0d8b8a86c3105b1bda7fb0d72cbcb72ed82995
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044687"
---
# <a name="custom-vision-service-release-notes"></a>Заметки о выпуске Пользовательской службы визуального распознавания

## <a name="march-26-2019"></a>26 марта 2019 г.

- Пользовательская служба визуального распознавания вошел в общедоступной версии в Azure!
- Добавлена функция Advanced обучения с помощью новой машинного обучения серверной части для повышения производительности, особенно для сложных наборов данных и точное классификации. Этот углубленный курс можно указать, бюджет время вычислений для обучения и Custom Vision эксперименте общая будет определить оптимальные параметры обучения и с изменениями. Для быстрого итераций можно продолжать использовать существующие быстрого обучения.
- 3.0 появился API-интерфейсы. Объявлено о ближайшие устаревании API-интерфейсы pre-3.0 на 1 октября 2019 г. См. краткие руководства по документации для [.Net](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/csharp-tutorial), [Python](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/python-tutorial), [узел](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/node-tutorial), [Java](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/java-tutorial), или [Go](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/go-tutorial) примеры о том, как приступить к работе.
- Заменить «По умолчанию итераций» с публикация, Отмена публикации 3.0 интерфейсы API.
- Были добавлены новые цели экспорта модели. Экспорт файла Dockerfile будет обновлен для поддержки ARM для Raspberry Pi 3. Добавлена поддержка экспорта для [пакет средств разработки концепции искусственного Интеллекта.](https://visionaidevkit.com/).
- Увеличение ограничения тегов проекта до 500 для уровня S0. Допустимое количество увеличено до изображения каждого проекта до 100 000 для уровня S0.
- Удален домен для взрослых. Вместо этого рекомендуется общие домена.
- Объявлено о [цены](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) общедоступной в будущем.  

## <a name="february-25-2019"></a>25 февраля 2019 г.

- Объявлено о конце ограниченной пробной версии проекты (проекты, не связанные с ресурсом Azure), как Custom Vision незадолго до завершения его перемещения в общедоступной предварительной версии Azure. 25 марта 2019 г., начиная на сайте CustomVision.ai поддерживают только просмотра проектов, связанных с ресурсом Azure, такие как бесплатный концепции настраиваемый ресурс. Через 1 октября 2019 г. по-прежнему можно получить доступ к существующие проекты ограниченной пробной версии с помощью API компьютерного зрения Custom. Это даст вам время для обновления ключи API для любых приложений, который вы написали методологиях Custom. После 1 октября 2019 г. будут удалены все ограниченной пробной проекты, которые еще не перенесли в Azure.

## <a name="january-22-2019"></a>22 января 2019 г.

- Добавлена поддержка для новых регионов Azure: "Западная часть США 2", "Восточная часть США", "Восточная часть США 2", "Западная Европа", "Северная Европа", "Юго-Восточная Азия", "Восточная Австралия", "Центральная Индия", "Южная часть Соединенного Королевства", "Восточная Япония" и "Центрально-северная часть США". Поддержка по-прежнему действует для региона "Центрально-южная часть США".

## <a name="december-12-2018"></a>12 декабря 2018 г.

- Добавлена поддержка экспорта для моделей обнаружения объектов (представлен компактный домен для обнаружения объектов).
- Исправлен ряд проблем со специальными возможностями, связанных с улучшенным средством чтения с экрана и поддержкой навигации с помощью клавиатуры.
- Обновлен пользовательский интерфейс для средства просмотра изображений. Улучшены возможности для более быстрого добавления тегов при обнаружении объектов.  
- Обновлена базовая модель для домена обнаружения объектов, что улучшило обнаружение объектов.
- Исправления ошибок.

## <a name="november-6-2018"></a>6 ноября 2018 г.

- Добавлена поддержка домена Logo (Логотип) в функцию обнаружения объектов.

## <a name="october-9-2018"></a>9 октября 2018 г.

- Обнаружение объектов вводит предварительную платную версию. Теперь можно создавать проекты обнаружения объекта с ресурсом Azure.
- Добавлена ​​функция "Переместить в Azure" на веб-сайт, чтобы было проще обновить проект ограниченной пробной версии для связи с Azure. Связанный проект ресурсов (F0 или S0.) Его можно найти на странице "Параметры" для вашего продукта.  
- Добавлен экспорт ONNX 1.2, для поддержки Windows с обновленной версией Windows "Машинное обучение" (октябрь 2018).
Исправления ошибок, в том числе для экспорта ONNX с помощью специальных символов.

## <a name="august-14-2018"></a>14 августа 2018 г.

- На сайт customvision.ai добавлено мини-приложение "Начало работы", направляющее пользователя при обучении проекту.
- Дальнейшее совершенствование конвейера машинного обучения принесет преимущества проектам с большим количеством меток (новый уровень потерь).

## <a name="june-28-2018"></a>28 июня 2018 года

- Исправления ошибок и усовершенствования серверной части.
- Включить многоклассовой классификации, для проектов, где образы имеют ровно одна метка. В прогнозах для многоклассовой режима вероятности сумма будет равна одному (все образы делятся между указанным тегов).

## <a name="june-13-2018"></a>13 июня 2018 г.

- Обновление пользовательского интерфейса, ориентированное на простоту использования и доступность.
- Совершенствование конвейера машинного обучения принесет преимущества проектам с большим количеством тегов.
- Исправлена ошибка в экспорте TensorFlow. Включено управления версиями экспортируемой модели, поэтому итерации можно экспортировать несколько раз.

## <a name="may-7-2018"></a>7 мая 2018 г.

- Появилась предварительная версия возможности обнаружения объекта для проектов в пробной версии с ограниченным временем действия.
- Обновление до API версии 2.0
- Уровень S0 теперь вмещает до 250 тегов 50 000 изображений.
- Значительные улучшения в серверной части конвейера машинного обучения для проектов по классификации изображений. Эти преимущества будут доступны для проектов с обучением после 27 апреля 2018 г.
- Добавлена модель экспорта в ONNX для использования с машинным обучением Windows.
- Добавлен экспорт модели в Dockerfile. Это позволяет загрузить артефакты для создания собственных контейнеров Windows или Linux, включая DockerFile, модель TensorFlow и код службы.
- Для вновь обученных моделей, экспортированных в TensorFlow в доменах "Общее" (компактный) и "Достопримечательность" (компактный), [средние значения теперь равны (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample) для согласованности во всех проектах.

## <a name="march-1-2018"></a>1 марта 2018 г.

- Введенный предварительная платная версия и принятию на портал Azure. Теперь проекты можно подключить к ресурсам Azure с помощью уровня F0 (Бесплатный) или S0 (Стандартный). Появились проекты уровня S0 с вместимостью 100 тегов и 25 000 изображений.
- Изменения внутренних служб для конвейера машинного обучения/параметра нормализации. Так клиенты смогут точнее контролировать баланс точности и полноты при настройке порога вероятности. В рамках этих изменений порог вероятности по умолчанию на портале CustomVision.ai был установлен на 50 %.

## <a name="december-19-2017"></a>19 декабря 2017 г.

- Добавлен экспорт в Android (TensorFlow) в дополнение к добавленному ранее экспорту в iOS (CoreML). Благодаря этому обученную компактную модель можно экспортировать автономно в приложении.
- Добавлены компактные домены "Розничная торговля" и "Достопримечательности" для экспорта модели для этих доменов.
- Выпущены версии [API обучения 1.2](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) и [API прогнозирования 1.1](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Обновленные API-интерфейсы поддерживают экспорт моделей, новую операцию прогнозирования, которая не сохраняет изображения в разделе "Прогнозирование", а также пакетные операции в API обучения.
- Настройки пользовательского интерфейса, включая возможность видеть, какой домен использовался для обучения итерации.
- Обновленный [пакет SDK для C# и пример](https://github.com/Microsoft/Cognitive-CustomVision-Windows).