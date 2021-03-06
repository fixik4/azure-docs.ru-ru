---
title: Развертывание многоконтейнерных групп в службе "Экземпляры контейнеров Azure"
description: Узнайте, как развертывать группу контейнеров с несколькими контейнерами в экземплярах контейнеров Azure с помощью шаблона Azure Resource Manager.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 93f73e133e99025b479d0b38512e26088a8eaefa
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369126"
---
# <a name="deploy-a-multi-container-group-with-a-resource-manager-template"></a>Развертывание нескольких контейнеров с использованием шаблона Resource Manager

Служба "Экземпляры контейнеров Azure" поддерживает развертывание нескольких контейнеров в одном узле с использованием [группы контейнеров](container-instances-container-groups.md). Это полезно при создании сопроводительного приложения для ведения журнала, мониторинга или любой другой конфигурации, когда службе требуется еще один прикрепленный процесс.

Есть два метода развертывания многоконтейнерных групп с помощью Azure CLI:

* Развертывание шаблонов Resource Manager (в этой статье).
* [Развертывание файлов YAML](container-instances-multi-container-yaml.md).

Если вам нужно развернуть дополнительные ресурсы службы Azure (например, общие файловые ресурсы Azure) во время развертывания экземпляра контейнера, рекомендуется использовать развертывание с помощью шаблонов Resource Manager. Из-за более сжатого характера формата YAML-файла развертывание с помощью такого файла рекомендуется, если оно включает *только* экземпляры контейнера.

> [!NOTE]
> Многоконтенерные группы сейчас ограничены контейнерами Linux. Мы работаем над тем, чтобы обеспечить все функции для контейнеров Windows, но для текущей платформы есть отличия в [квотах и доступности регионов для службы "Экземпляры контейнеров Azure"](container-instances-quotas.md).

Примеры дополнительных шаблонов, см. в разделе [шаблонов Azure Resource Manager для экземпляров контейнеров Azure](container-instances-samples-rm.md). 

## <a name="configure-the-template"></a>Настройка шаблона

В разделах этой статьи рассматривается запуск простой конфигурации многоконтейнерного сопроводительного приложения. При этом развертывается шаблон Azure Resource Manager.

Начните с создания файла с именем `azuredeploy.json` и скопируйте в него приведенный ниже код JSON.

Этот шаблон Resource Manager определяет группу контейнеров с двумя контейнерами, общедоступным IP-адресом и двумя предоставленными портами. Эти контейнеры развертываются из общедоступных образов Microsoft. Первый контейнер в группе запускает приложение с выходом в Интернет. Второй контейнер (сопроводительный) осуществляет HTTP-запрос к основному веб-приложению через локальную сеть группы.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Чтобы использовать частный реестр образов контейнеров, добавьте объект в документ JSON в следующем формате: Пример реализации этой конфигурации см. в [справочнике по шаблонам Resource Manager для экземпляров контейнеров Azure][template-reference].

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>Развертывание шаблона

Создайте группу ресурсов с помощью команды [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Разверните шаблон с помощью команды [az group deployment create][az-group-deployment-create].

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
```

В течение нескольких секунд вы должны получить исходный ответ Azure.

## <a name="view-deployment-state"></a>Просмотр состояния развертывания

Чтобы просмотреть состояние развертывания, используйте команду [az container show][az-container-show] ниже.

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Если вы хотите просмотреть запущенное приложение, перейдите к его IP-адресу в своем браузере. Например, в этом примере выходных данных используется IP-адрес `52.168.26.124`:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-logs"></a>Просмотр журналов

Просмотрите выходные данные журнала контейнера с помощью команды [az container logs][az-container-logs]. Аргумент `--container-name` определяет контейнер, из которого извлекаются журналы. В этом примере указывается первый контейнер.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Выходные данные:

```bash
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Чтобы просмотреть журналы для сопроводительного контейнера, выполните ту же команду, указав имя второго контейнера.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Выходные данные:

```bash
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Как видите, сопроводительное приложение периодически выполняет HTTP-запрос к основному веб-приложению через локальную сеть группы, чтобы убедиться, что оно работает. Пример сопроводительного приложения можно расширить, реализовав активацию оповещения при получении кода ответа HTTP, отличающегося от 200 OK.

## <a name="next-steps"></a>Дальнейшие действия

В этой статье описаны шаги по развертыванию многоконтейнерного экземпляра контейнера Azure. Дополнительные сведения о службе "Экземпляры контейнеров Azure" см. в соответствующем руководстве.

> [!div class="nextstepaction"]
> [Руководство по использованию службы "Экземпляры контейнеров Azure"][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
