---
title: Защита веб-служб с помощью SSL
titleSuffix: Azure Machine Learning service
description: Узнайте, как защитить веб-службы развернуты в службе машинного обучения Azure, позволяя HTTPS. HTTPS обеспечивает защиту данных, отправленных клиентами с помощью безопасности транспортного уровня (TLS), является заменой слои безопасного сокетов (SSL). Он также используется клиентами для проверки удостоверения веб-службы.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 1a6aa75f3d25cd88cd1edb9b2cdcfabc3b4ec8f9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58103899"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Использование SSL для защиты веб-служб, развернутых с помощью Службы машинного обучения Azure

В этой статье вы узнаете, как защитить веб-службы, развернутые с помощью службы Машинного обучения Azure. Можно ограничить доступ к веб-служб и защиты данных, отправленных клиентами с помощью [гипертекстовый протокол безопасного (HTTPS)](https://en.wikipedia.org/wiki/HTTPS).

HTTPS используется для защиты обмена данными между клиентом и веб-службу, шифровать обмен данными между ними. Шифрование осуществляется с использованием [безопасности транспортного уровня (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). Иногда это по-прежнему упоминается как Secure Sockets Layer (SSL), которая была создана до TLS.

> [!TIP]
> Пакета SDK для Azure Machine Learning используется термин «SSL» для свойств, связанных с включением защиты обмена данными. Это не означает, что TLS не используется веб-службой, так же, протокол SSL — это более запоминающееся термин для многих читателей.

TLS и SSL, используют __цифровые сертификаты__, которые используются для выполнения проверки, шифрования и идентификации. Дополнительные сведения о работы цифровых сертификатов, см. в статье Википедии на [инфраструктуры открытых ключей (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!Warning]
> Если не включить и использовать протокол HTTPS для веб-службы, данные, отправляемые из службы и могут быть видны людям, в Интернете.
>
> HTTPS также позволяет клиенту проверки подлинности сервера, который соединяется с. Это позволяет защитить клиентов от [man-in--middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) атак.

Защита новой веб-службы или уже существующей выглядит следующим образом:

1. Получите имя домена.

2. Получите цифровой сертификат.

3. Разверните или обновите веб-службу со включенным параметром SSL.

4. Обновите имя DNS, чтобы оно указывало на веб-службу.

При защите веб-служб в разных [целевых объектах развертывания](how-to-deploy-and-where.md) есть небольшие отличия.

## <a name="get-a-domain-name"></a>Получение доменного имени

Если у вас еще нет имени домена, вы можете приобрести его у __регистратора доменных имен__. Процесс различается у разных регистраторов, как и стоимость. Регистратор также предоставляет вам инструменты для управления доменным именем. Эти инструменты используются для сопоставления полное доменное имя (например, www\.contoso.com) на IP-адрес размещения веб-службу.

## <a name="get-an-ssl-certificate"></a>Получите SSL-сертификат.

Существует много способов, чтобы получить SSL-сертификат (сертификат). Наиболее распространенным является покупка в одном из __центров сертификации__. Независимо от того, где вы получаете сертификат, вам нужны следующие файлы:

* __Сертификат__. Сертификат должен содержать полную цепочку сертификатов и должен быть в кодировке PEM.
* __Ключ__. Ключ должен быть в кодировке PEM.

При запросе сертификата вы должны предоставить полное доменное имя (FQDN) адреса, который вы планируете использовать для этой веб-службы. Например, www\.contoso.com. Адрес, указанный в сертификате, и адрес, используемый клиентами, сравниваются при проверке удостоверения веб-службы. Если адреса не совпадают, клиенты получат сообщение об ошибке.

> [!TIP]
> Если центр сертификации не может предоставить сертификат и ключ в виде файлов в кодировке PEM, вы можете использовать такую служебную программу, как [OpenSSL](https://www.openssl.org/), чтобы изменить формат.

> [!WARNING]
> Эти самозаверяющие сертификаты следует использовать только для разработки. Они не должны использоваться в рабочей среде. Самозаверяющие сертификаты могут вызвать проблемы в клиентских приложениях. Дополнительные сведения см. в документации для сетевых библиотек, используемых в клиентском приложении.

## <a name="enable-ssl-and-deploy"></a>Включение SSL и развертывание службы

Чтобы развернуть (или развернуть повторно) службу со включенным SSL, установите для параметра `ssl_enabled` значение `True` везде, где это применимо. Установите для параметра `ssl_certificate` значение файла __сертификата__, а для параметра `ssl_key` значение __ключа__.

+ **Развертывание в Службе Azure Kubernetes (AKS)**

  При подготовке кластера AKS укажите значения для параметров, связанных с SSL, как показано во фрагменте кода:

    ```python
    from azureml.core.compute import AksCompute

    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Развертывание в Экземплярах контейнеров Azure (ACI)**

  При развертывании в ACI укажите значения для параметров, связанных с SSL, как показано во фрагменте кода:

    ```python
    from azureml.core.webservice import AciWebservice

    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Развертывание в программируемых пользователем вентильных матрицах (FPGA)**

  При развертывании в FPGA укажите значения для параметров, связанных с SSL, как показано во фрагменте кода:

    ```python
    from azureml.contrib.brainwave import BrainwaveWebservice

    deployment_config = BrainwaveWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem")
    ```

## <a name="update-your-dns"></a>Обновление DNS

Теперь необходимо обновить DNS, чтобы оно указывало на веб-службу.

+ **Для ACI**:

  Используйте инструменты, предоставленные вашим регистратором доменных имен, чтобы обновить запись DNS для вашего доменного имени. Запись должна указывать на IP-адрес службы.

  В зависимости от регистратора и срока жизни, настроенного для имени домена, может потребоваться от нескольких минут до нескольких часов, прежде чем клиенты смогут разрешить имя домена.

+ **Для AKS**:

  Обновите имя DNS на вкладке "Конфигурация" окна "Общедоступный IP-адрес" кластера AKS, как показано на изображении. Общедоступный IP-адрес можно найти как один из типов ресурсов в группе ресурсов, содержащей узлы агента AKS и другие сетевые ресурсы.

  ![Служба машинного обучения Azure. Защита веб-служб с помощью SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Дальнейшие действия
Вы узнаете, как выполнять следующие задачи:
+ [Использование модели Машинного обучения, развернутой в качестве веб-службы](how-to-consume-web-service.md)
+ [Безопасное выполнение экспериментов и формирование выводов в пределах виртуальной сети Azure](how-to-enable-virtual-network.md)
