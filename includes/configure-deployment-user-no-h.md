---
title: включение файла
description: включение файла
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: e8888a0505a3a38d2844f82c0f7fff255d05353d
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261424"
---
В Azure Cloud Shell настройте учетные данные развертывания с помощью команды [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set). Пользователь развертывания необходим для развертывания FTP и локального репозитория Git в веб-приложении. Имя пользователя и пароль представляют уровень учетной записи. _Они отличаются от учетных данных подписки Azure._

В следующем примере замените *\<имя_пользователя>* и *\<пароль>* (включая скобки) новым именем пользователя и паролем. Имя пользователя должно быть уникальным в пределах Azure. Пароль должен содержать не менее восьми символов и включать два из трех следующих элементов: буквы, цифры и символы. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Вы получите выходные данные JSON с паролем, обозначенным как `null`. Если появляется сообщение об ошибке `'Conflict'. Details: 409`, измените имя пользователя. Если появляется сообщение об ошибке ` 'Bad Request'. Details: 400`, используйте более надежный пароль.

Это развертывание настраивается только один раз. Его можно использовать для всех развертываний Azure.

> [!NOTE]
> Запишите имя пользователя и пароль. В дальнейшем они понадобятся для развертывания веб-приложения.
>
>
