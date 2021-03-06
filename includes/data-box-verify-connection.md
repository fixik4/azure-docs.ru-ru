---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 8a09a52db40f4f52219bce3e703e275b0f310c1a
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550494"
---
Чтобы подключится к учетной записи хранения и проверить подключения, выполните следующие действия.

1. В Обозревателе службы хранилища откройте диалоговое окно **Подключиться к службе хранилища Azure**. В диалоговом окне **Подключение к службе хранилища Azure** выберите **Использовать имя и ключ учетной записи хранения**.

    ![Панель мониторинга Data Box](media/data-box-verify-connection/data-box-connect-via-rest-9.png)

2. Введите ваше **Имя учетной записи** и **Ключ учетной записи** (значение ключа 1 на странице **Подключиться и копировать** в локальном пользовательском веб-интерфейсе). Для домена конечных точек хранилища выберите **Другие (ввести ниже)** и вставьте конечную точку службы больших двоичных объектов, как показано ниже. Установите флажок для параметра **Использовать HTTP** только в случае передачи через *http*. Не устанавливайте этот флажок при использовании *https*. Щелкните **Далее**.

    ![Панель мониторинга Data Box](media/data-box-verify-connection/data-box-connect-via-rest-11.png)    

3. Проверьте сведения в диалоговом окне **Сводка подключения**. Нажмите кнопку **Подключиться**.

    ![Панель мониторинга Data Box](media/data-box-verify-connection/data-box-connect-via-rest-12.png)

4. Успешно добавленная учетная запись отображается в левой области Обозревателя службы хранилища, а к ее имени будет добавлен текст (Внешняя, Другая). Щелкните **Контейнеры больших двоичных объектов**, чтобы просмотреть контейнер.

    ![Панель мониторинга Data Box](media/data-box-verify-connection/data-box-connect-via-rest-17.png)
