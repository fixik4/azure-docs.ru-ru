---
title: Общие сведения о службе "Записные книжки Azure"
description: Запустите в облаке записные книжки Jupyter в облаке, используя бесплатную службу "Записные книжки Azure", не требующую настройки.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 9cea5a8e-c52d-4bdc-9e4a-cecdc1ad02c1
ms.service: azure
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/11/2019
ms.author: kraigb
ms.openlocfilehash: 9916b75a15098acbafc1cb1f6d44d948cf6de851
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57777725"
---
# <a name="overview-of-azure-notebooks"></a>Общие сведения о службе "Записные книжки Azure"

Записные книжки Azure — это бесплатная размещенная служба для разработки и запуска записных книжек Jupyter в облаке без их установки. [Jupyter](https://jupyter.org/) (прежнее название — IPython) — это проект с открытым кодом, который позволяет легко объединять текст с разметкой, исполняемый код, хранимые данные, графики и визуализации на одном общем холсте, который называется *записной книжкой* (изображение предоставлено сайтом jupyter.org):

[![Примеры записных книжек Jupyter](https://jupyter.org/assets/jupyterpreview.png)](https://jupyter.org/assets/jupyterpreview.png#lightbox)

Из-за мощного сочетания кода, графики и пояснений Jupyter стал популярным для различных целей, включая инструкции по обработке и анализу данных, очистку и преобразование данных, цифровое и статистическое моделирование, а также разработку моделей машинного обучения.

## <a name="hassle-free-experience"></a>Беспроблемная среда

С помощью службы "Записные книжки Azure" вы сможете быстро приступить к созданию прототипов, обработке и анализу данных, научным исследованиям, а также освоить программирование на Python:

- Специалист по анализу и обработке данных имеет мгновенный доступ к полной среде Anaconda без установки.
- Преподаватель может предоставлять учащимся беспроблемную среду Python.
- Докладчик может выступить с речью или провести вебинар, при этом участникам не придется тратить 45 минут на установку программного обеспечения.
- Разработчик или разработчик-любитель может использовать записные книжки в качестве электронного блокнота для быстрого написания кода.

Записные книжки становятся еще более эффективнее, так как люди могут совместно с ними работать с помощью доступной в браузере облачной службы, например "Записные книжки Azure" (в предварительной версии). Так как среда Jupyter находится в облаке, пользователям не нужно устанавливать ее локально или беспокоиться о ее обслуживании. Облако также упрощает совместное использование записных книжек (и связанных файлов данных) с другими полномочными пользователями, позволяя избежать усложнений, возникающих при использовании внешних средств, таких как репозитории управления версиями. С помощью службы "Записные книжки Azure" пользователи также могут копировать (или клонировать) записные книжки в собственные учетные записи для редактирования или экспериментирования, что особенно полезно в учебных целях.

Так как служба "Записные книжки Azure" является общей платформой для создания, выполнения и совместного использования кода, ее можно применять для различных сценариев:

- Изучение нового языка программирования — воспользуйтесь одним из [руководств на главной странице](https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb).
- Изучение обработки и анализа данных — ознакомьтесь с [книгой Джейка Вандерпласа (Jake VanderPlas)](https://notebooks.azure.com/jakevdp/projects/PythonDataScienceHandbook).
- [Чтение курса](https://notebooks.azure.com/garth-wells/projects/CUED-IA-Computing-Michaelmas) для сотен учащихся.
- Проведение вебинара по сети или на конференции без затрат времени на установку. 
- Разрешение пользователям GitHub напрямую загружать и запускать записные книжки, [создав индикатор событий запуска в GitHub](https://notebooks.azure.com/help/projects/sharing/create-a-github-badge).
- Предоставление [слайд-шоу как в PowerPoint](https://notebooks.azure.com/help/jupyter-notebooks/slides), где в слайдах присутствует исполняемый код.

Если кратко, служба "Записные книжки Azure" помогает более эффективно выполнить работу и таким образом достигнуть более высоких результатов.

> [!Note]
> Дополнительные сведения о самом Jupyter можно найти на сайте [jupyter.org](https://jupyter.org/) и в [документации по Jupyter](https://jupyter-notebook.readthedocs.io/en/latest/).

## <a name="pricing-and-quotas"></a>Цены и квоты

Записные книжки Azure — это бесплатная служба, но каждый проект ограничен 4 ГБ памяти и 1 ГБ данных во избежание злоупотребления. Уполномоченные пользователи, превысившие эти ограничения, должны ответить на запрос Captcha, чтобы продолжить использование записных книжек.

Чтобы снять все ограничения, войдите в службу "Записные книжки Azure" с учетной записью с помощью Azure Active Directory (например, с учетной записью организации). Если эта учетная запись связана с подпиской Azure, можно подключиться к любому экземпляру виртуальной машины для обработки и анализа данных в этой подписке. Дополнительные сведения см. в разделе об уровне вычислительной среды в руководстве [Администрирование и настройка проектов](configure-manage-azure-notebooks-projects.md#compute-tier).

## <a name="available-kernels-and-environments"></a>Доступные ядра и среды

Для каждой записной книжки нужно выбрать ядро (это среда выполнения), которое используется для запуска любых ячеек кода. Служба "Записные книжки Azure" поддерживает следующие ядра:

- Python 2.7 + Anaconda 2-5.3.0;
- Python 3.6 + Anaconda3-5.3.0;
- Python 3.5 + Anaconda3-4.2.0 (вскоре будет устаревшим);
- R 3.4.1 + Microsoft R Open 3.4.1;
- F# 4.1.9.

В службу "Записные книжки Azure" также входят дополнительные пакеты, помимо основных дистрибутивов. Например, ядра Python включают библиотеки numpy, pandas, scikit-learn, matplotlib и bokeh.

Вы также можете настроить проект так, чтобы создать среду для всех входящих в него записных книжек. Дополнительные сведения см. в [кратком руководстве по созданию проекта с использованием настраиваемой среды](quickstart-create-jupyter-notebook-project-environment.md).

В дополнение к основным дистрибутивам в службу "Записные книжки Azure" входят предварительно установленные дополнительные пакеты, которые полезны для специалистов по обработке и анализу данных. Вы также можете установить собственные пакеты, используя обычный процесс для каждого языка.

## <a name="pre-configured-jupyter-extensions"></a>Предварительно настроенные расширения Jupyter

Служба "Записные книжки Azure" снабжена следующими предварительно настроенными расширениями Jupyter:

- [RISE:](https://github.com/damianavila/RISE) расширение Jupyter для слайд-шоу (также известное как live_reveal). Дополнительные сведения см. в статье [Run a notebook slideshow](present-jupyter-notebooks-slideshow.md) (Запуск слайд-шоу на основе записной книжки).
- [JupyterLab:](https://github.com/jupyterlab/jupyterlab) вычислительная среда для работы с записными книжками Jupyter.
- [Altair:](https://github.com/ellisonbg/altair) декларативная, статистическая библиотека визуализации для Python.
- [BQPlot:](https://github.com/bloomberg/bqplot) интерактивная платформа построения графиков для записных книжек Jupyter.
- [IpyWidgets:](https://github.com/jupyter-widgets/ipywidgets) интерактивные мини-приложения HTML для записных книжек Jupyter.

## <a name="issues-and-getting-help"></a>Вопросы и получение справки

Так как служба "Записные книжки Azure" все еще доступна только в предварительной версии, в ней могут возникать временные сбои, которые могут быть чаще и длится дольше, чем в других службах Azure. Некоторые функции могут быть неполными или содержать ошибки.

В настоящее время не рекомендуется использовать предварительную версию службы "Записные книжки Azure" для критически важных для бизнеса приложений или для конфиденциальных записных книжек и данных.

Если у вас возникли вопросы о службе "Записные книжки Azure", разместите их в [репозитории GitHub](https://github.com/Microsoft/AzureNotebooks/issues).

## <a name="next-steps"></a>Дополнительная информация  

- [Обзор примеров записных книжек](azure-notebooks-samples.md)

- Краткие руководства:

  - [Создание и совместное использование записной книжки](quickstart-create-share-jupyter-notebook.md)
  - [Клонирование записной книжки](quickstart-clone-jupyter-notebook.md)
  - [Перенос локальной записной книжки Jupyter](quickstart-migrate-local-jupyter-notebook.md)
  - [Использование настраиваемой среды](quickstart-create-jupyter-notebook-project-environment.md)
  - [Вход и настройка идентификатора пользователя](quickstart-sign-in-azure-notebooks.md)

- Руководства:

  - [Создание и запуск записной книжки](tutorial-create-run-jupyter-notebook.md  )

- Статьи с инструкциями, посвященные таким темам:
  
  - [Создание и клонирование проектов](create-clone-jupyter-notebooks.md)
  - [Администрирование и настройка проектов](configure-manage-azure-notebooks-projects.md)
  - [Установка пакетов из записной книжки](install-packages-jupyter-notebook.md)
  - [Представление слайд-шоу](present-jupyter-notebooks-slideshow.md)
  - [Работа с файлами данных](work-with-project-data-files.md)
  - [Доступ к ресурсам данных](access-data-resources-jupyter-notebooks.md)
  - [Работа со Службами машинного обучения Azure](use-machine-learning-services-jupyter-notebooks.md)
