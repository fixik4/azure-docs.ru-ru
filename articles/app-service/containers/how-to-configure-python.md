---
title: Настройка приложений Python для Службы приложений Azure
description: В этом руководстве описываются возможности создания и настройки приложений Python для Службы приложений Azure под управлением Linux.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/29/2019
ms.author: astay;cephalin;kraigb
ms.custom: seodec18
ms.openlocfilehash: 6965379aadefd110ce6e46e105bbde10626b63c1
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892173"
---
# <a name="configure-your-python-app-for-azure-app-service"></a>Настройка приложения Python для Службы приложений Azure
В этой статье описывается, как [Служба приложений Azure](app-service-linux-intro.md) запускает приложения Python, а также как вы можете настроить поведение Службы приложений при необходимости. Приложения Python должны быть развернуты со всеми необходимыми модулями [pip](https://pypi.org/project/pip/). Механизм развертывания Службы приложений (Kudu) автоматически активирует виртуальное окружение и запускает команду `pip install -r requirements.txt` при развертывании [репозитория Git](../deploy-local-git.md) или [ZIP-пакета](../deploy-zip.md) с включенными процессами сборки.

> [!NOTE]
> [Python в версии Службы приложений для Windows](https://docs.microsoft.com/visualstudio/python/managing-python-on-azure-app-service) использовать не рекомендуется.
>

## <a name="show-python-version"></a>Просмотр версии Python

Чтобы отобразить текущую версию Python, выполните следующую команду в [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource_group_name> --name <app_name> --query linuxFxVersion
```

Чтобы отобразить все поддерживаемые версии Python, выполните следующую команду в [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep PYTHON
```

Вы можете запустить неподдерживаемую версию Python, выполнив сборку собственного образа контейнера. Дополнительные сведения см. в статье об [использовании пользовательского образа Docker](tutorial-custom-docker-image.md).

## <a name="set-python-version"></a>Выбор версии Python

Выполните следующую команду в [Cloud Shell](https://shell.azure.com), чтобы задать для Python версию 3.7:

```azurecli-interactive
az webapp config set --resource-group <group_name> --name <app_name> --linux-fx-version "PYTHON|3.7"
```

## <a name="container-characteristics"></a>Характеристики контейнера

Приложения Python, развернутые в службе приложений под управлением Linux, выполняются в контейнере Docker, который определен в репозитории GitHub, [Python 3.6](https://github.com/Azure-App-Service/python/tree/master/3.6.6) или [Python 3.7](https://github.com/Azure-App-Service/python/tree/master/3.7.0).
Этот контейнер отличается следующими характеристиками.

- Приложения запускаются с помощью [HTTP-сервера Gunicorn WSGI](https://gunicorn.org/), используя дополнительные аргументы `--bind=0.0.0.0 --timeout 600`.
- По умолчанию базовый образ включает в себя веб-платформу Flask, но контейнер также поддерживает другие платформы, совместимые с WSGI и с Python 3.7, например Django.
- Чтобы установить дополнительные пакеты, такие как Django, создайте файл [*requirements.txt*](https://pip.pypa.io/en/stable/user_guide/#requirements-files) в корневой папке проекта, используя `pip freeze > requirements.txt`. Затем опубликуйте проект в службе приложений с помощью развертывания Git, которое автоматически выполняет `pip install -r requirements.txt` в контейнере для установки зависимостей приложения.

## <a name="container-startup-process"></a>Процесс запуска контейнера

Во время запуска служба приложений под управлением контейнера Linux выполнит следующие действия.

1. Используйте [пользовательскую команду запуска](#customize-startup-command), если предоставлена такая возможность.
2. Проверьте наличие [приложения Django](#django-app) и запустите для него сервер Gunicorn, если он обнаружен.
3. Проверьте наличие [приложения Flask](#flask-app) и запустите для него сервер Gunicorn, если он обнаружено.
4. Если другие приложения не найдены, запускается приложение по умолчанию, встроенное в контейнер.

В следующих разделах приведены дополнительные сведения о каждом параметре.

### <a name="django-app"></a>Приложение Django

Для приложений Django служба приложений выполняет поиск файла с именем `wsgi.py` в вашем коде приложения, а затем запускает Gunicorn, используя следующую команду:

```bash
# <module> is the path to the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Если необходим определенный контроль над командой запуска, используйте пользовательские команды запуска и замените `<module>` именем модуля, который содержит *wsgi.py*.

### <a name="flask-app"></a>Приложение Flask

Для Flask Служба приложений выполняет поиск файла с именем *application.py* или *app.py* и запускает Gunicorn следующим образом:

```bash
# If application.py
gunicorn --bind=0.0.0.0 --timeout 600 application:app
# If app.py
gunicorn --bind=0.0.0.0 --timeout 600 app:app
```

Если модуль основного приложения содержится в другом файле, используйте другое имя для объекта приложения или, если вы хотите указать дополнительные аргументы для Gunicorn, используйте пользовательскую команду запуска.

### <a name="default-behavior"></a>Поведение по умолчанию

Если в службе приложений не найдена пользовательская команда, приложение Django или Flask, тогда она запускает приложение по умолчанию с разрешением только для чтения, расположенное в папке _opt/defaultsite_. Приложение по умолчанию выглядит следующим образом:

![Служба приложений по умолчанию на веб-странице Linux](media/how-to-configure-python/default-python-app.png)

## <a name="customize-startup-command"></a>Настройка команды запуска

Вы можете задать поведение при запуске контейнера, указав пользовательскую команду запуска Gunicorn. Например, если вы используете приложение Flask, главным модулем которого является *hello.py*, и объект приложения Flask в этом файле имеет имя `myapp`, тогда команда будет выглядеть следующим образом:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
```

Если главный модуль находится в подпапке, например `website`, укажите эту подпапку с помощью аргумента `--chdir`:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 --chdir website hello:myapp
```

Кроме того, вы можете добавить любые дополнительные аргументы в командную строку для Gunicorn, например `--workers=4`. Дополнительные сведения см. в статье [Running Gunicorn](https://docs.gunicorn.org/en/stable/run.html) (Запуск Gunicorn) (docs.gunicorn.org).

Чтобы использовать другой сервер (не Gunicorn), например [aiohttp](https://aiohttp.readthedocs.io/en/stable/web_quickstart.html), можно выполнить команду:

```bash
python3.7 -m aiohttp.web -H localhost -P 8080 package.module:init_func
```

Чтобы предоставить пользовательскую команду, выполните следующие действия:

1. Перейдите на страницу [Параметры приложения](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) на портале Azure.
1. В настройках **среды выполнения** установите для параметра **Стек** значение **Python 3.7** и введите команду непосредственно в поле **Загрузочный файл**.
Кроме того, вы можете сохранить команду в текстовый файл в корневом каталоге проекта, используя имя, например *startup.txt* (или любое другое). Затем разверните этот файл в службе приложений и замените имя файла в поле **Загрузочный файл**. Этот параметр позволяет управлять командой в репозитории исходного кода, а не на портале Azure.
1. Щелкните **Сохранить**. Служба приложений автоматически перезапустится, и через несколько секунд вы увидите применение команды запуска.

> [!Note]
> Служба приложений игнорирует все ошибки, возникающие при обработке файла с пользовательской командой, и продолжает процесс запуска путем поиска приложений Django и Flask. Если вы не видите ожидаемое поведение, убедитесь, что файл запуска развернут в службе приложений и что в нем нет ошибок.

## <a name="access-environment-variables"></a>Доступ к переменным среды

В Службе приложений можно задать параметры приложения вне кода приложения (см. раздел об [установке переменных среды](../web-sites-configure.md)). Затем вы сможете получать к ним доступ, используя стандартный шаблон [os.environ](https://docs.python.org/3/library/os.html#os.environ). Например, для доступа к параметру приложения с именем `WEBSITE_SITE_NAME` используйте следующий код:

```python
os.environ['WEBSITE_SITE_NAME']
```

## <a name="detect-https-session"></a>Обнаружение сеанса HTTPS

В Службе приложений [завершение SSL-запросов](https://wikipedia.org/wiki/TLS_termination_proxy) происходит в подсистеме балансировки нагрузки сети, поэтому все HTTPS-запросы достигают вашего приложения в виде незашифрованных HTTP-запросов. Если логика вашего приложения проверяет, зашифрованы ли пользовательские запросы, проверяйте заголовок `X-Forwarded-Proto`.

```python
if 'X-Forwarded-Proto' in request.headers and request.headers['X-Forwarded-Proto'] == 'https':
# Do something when HTTPS is used
```

Популярные веб-платформы позволяют получить доступ к информации `X-Forwarded-*` в стандартном шаблоне приложения. В [CodeIgniter](https://codeigniter.com/) функция [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) по умолчанию проверяет значение `X_FORWARDED_PROTO`.

## <a name="troubleshooting"></a>Устранение неполадок

- **После развертывания кода приложения отображается приложение по умолчанию.** Приложение по умолчанию отображается, потому что вы либо не развертывали код приложения в Службу приложений, либо она не смогла найти ваш код приложения и вместо этого использовала приложение по умолчанию.
- Перезапустите службу приложений, подождите 15–20 секунд и снова проверьте приложение.
- Убедитесь, что вы используете службу приложений для Linux, а не экземпляр для Windows. В Azure CLI выполните команду `az webapp show --resource-group <resource_group_name> --name <app_service_name> --query kind`, заменив `<resource_group_name>` и `<app_service_name>` соответствующими значениями. Команда должна вывести строку `app,linux`. Если этого не произошло, заново создайте службу приложений и выберите Linux.
- Используйте SSH или консоль Kudu для подключения непосредственно к службе приложений и убедитесь, что файлы существуют в каталоге *site/wwwroot*. Если файлов нет, проверьте процесс развертывания и повторно разверните приложение.
- Если файлы имеются, значит службе приложений не удалось определить конкретный загрузочный файл. Убедитесь, что ваше приложение структурировано, так как служба приложений ожидает [Djangо](#django-app) или [Flask](#flask-app) или использует пользовательскую команду запуска.
- **В браузере появилось сообщение "Служба недоступна".** Истекло время ожидания браузером ответа от службы приложений, что указывает, что она запустила сервер Gunicorn, но аргументы, указывающие код приложения, неверные.
- Обновите браузер, особенно если вы используете самую низкую ценовую категорию в плане службы приложений. Приложению может потребоваться больше времени для запуска, например при использовании бесплатных уровней, и оно начнет отвечать после обновления браузера.
- Убедитесь, что ваше приложение структурировано, так как служба приложений ожидает [Djangо](#django-app) или [Flask](#flask-app) или использует [пользовательскую команду запуска](#customize-startup-command).
- Используйте SSH или консоль Kudu для подключения к службе приложений, а затем изучите журналы диагностики, которые хранятся в папке *LogFiles*. Дополнительные сведения о ведении журналов см. в статье [Включение ведения журнала диагностики для веб-приложений в службе приложений Azure](../troubleshoot-diagnostic-logs.md).
