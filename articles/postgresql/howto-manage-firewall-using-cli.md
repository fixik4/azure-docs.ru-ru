---
title: Создание правил брандмауэра базы данных Azure для PostgreSQL и управление ими с помощью Azure CLI
description: В этой статье описывается, как создать базу данных Azure для правил брандмауэра PostgreSQL и управлять ею с помощью интерфейса командной строки Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 05/4/2018
ms.openlocfilehash: bde43a02b844d0dc0be8b6a270ac94e0b1422b69
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2019
ms.locfileid: "55733286"
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Создание правил брандмауэра базы данных Azure для PostgreSQL и управление ими с помощью Azure CLI
Правила брандмауэра уровня сервера позволяют администраторам управлять доступом к серверу базы данных Azure для PostgreSQL с указанного IP-адреса или диапазона IP-адресов. С помощью удобных команд Azure CLI можно создавать, обновлять, удалять, выводить список и отображать правила брандмауэра для управления сервером. Обзор брандмауэров базы данных Azure для PostgreSQL приведен в разделе [Правила брандмауэра сервера базы данных Azure для PostgreSQL](concepts-firewall-rules.md).

## <a name="prerequisites"></a>Предварительные требования
Прежде чем приступить к выполнению этого руководства, необходимы следующие компоненты:
- Установите служебную программу командной строки [Azure CLI](/cli/azure/install-azure-cli) или используйте Azure Cloud Shell в браузере.
- [сервер и база данных Azure для PostgreSQL](quickstart-create-server-database-azure-cli.md);

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Настройка правил брандмауэра сервера базы данных Azure для PostgreSQL
Команда [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule) используется для настройки правил брандмауэра.

## <a name="list-firewall-rules"></a>Вывод списка правил брандмауэра 
Чтобы вывести список имеющихся правил брандмауэра сервера, запустите команду [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule).
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Выходные данные будут содержать правила брандмауэра (если они имеются) в используемом по умолчанию формате JSON. Вы можете использовать `--output table`, чтобы получить выходные данные в виде более удобной таблицы.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Создание правила брандмауэра
Чтобы создать правило брандмауэра на сервере, выполните команду [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule). 

```
To allow access to a singular IP address, provide the same address in the `--start-ip-address` and `--end-ip-address`, as in this example, replacing the IP shown here with your specific IP.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Чтобы разрешить приложениям подключаться с IP-адресов Azure к серверу службы "База данных Azure для PostgreSQL", укажите в качестве начального и конечного IP-адресов 0.0.0.0, как показано в примере.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Этот параметр позволяет настроить брандмауэр так, чтобы разрешить все подключения из Azure, включая подключения из подписок других клиентов. При выборе этого параметра убедитесь, что используемое имя для входа и разрешения пользователя предоставляют доступ только авторизованным пользователям.
> 

При успешном выполнении команды ее выходные данные будут содержать сведения о созданном правиле брандмауэра в используемом по умолчанию формате JSON. Если возникнет сбой, выходные данные будут содержать вместо этого сообщение об ошибке.

## <a name="update-firewall-rule"></a>Обновление правила брандмауэра 
Обновите существующее правило брандмауэра на сервере с помощью команды [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule). В качестве входных данных укажите имя существующего правила брандмауэра, которое нужно обновить, а также атрибуты начального и конечного IP-адресов.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.0
```
При успешном выполнении команды ее выходные данные будут содержать сведения об обновленном правиле брандмауэра в используемом по умолчанию формате JSON. Если возникнет сбой, выходные данные будут содержать вместо этого сообщение об ошибке.
> [!NOTE]
> Если правило брандмауэра не существует, оно будет создано командой update.

## <a name="show-firewall-rule-details"></a>Отображение сведений о правиле брандмауэра
Вы также можете отобразить сведения об имеющемся правиле брандмауэра на уровне сервера, выполнив команду [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule).
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
При успешном выполнении команды ее выходные данные будут содержать сведения об указанном правиле брандмауэра в используемом по умолчанию формате JSON. Если возникнет сбой, выходные данные будут содержать вместо этого сообщение об ошибке.

## <a name="delete-firewall-rule"></a>Удаление правила брандмауэра
Чтобы отменить доступ к серверу для диапазона IP-адресов, удалите существующее правило брандмауэра, выполнив команду [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule). Укажите имя существующего правила брандмауэра.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
При успешном выполнении выходные данные отсутствуют. В случае сбоя возвращается сообщение об ошибке.

## <a name="next-steps"></a>Дополнительная информация
- Аналогичным образом можно использовать веб-браузер, чтобы [создать правила брандмауэра базы данных Azure для PostgreSQL и управлять ими с помощью портала Azure](howto-manage-firewall-using-portal.md).
- Узнайте больше о [правилах брандмауэра сервера базы данных Azure для PostgreSQL](concepts-firewall-rules.md).
- Справка по подключению к серверу базы данных Azure для PostgreSQL доступна в статье [Библиотеки подключений для базы данных Azure для PostgreSQL](concepts-connection-libraries.md).
