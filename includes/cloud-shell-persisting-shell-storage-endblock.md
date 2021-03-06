---
author: cynthn
ms.service: azure
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: c2ed33aea77b5478e8d17f6bd0213ef3e778b806
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572618"
---
## <a name="transfer-local-files-to-cloud-shell"></a>Передача локальных файлов в Cloud Shell
Каталог `clouddrive` синхронизируется c колонкой хранилища на портале Azure. Используйте эту колонку для перемещения локальных файлов в файловый ресурс и из него. Обновленные файлы в Cloud Shell отобразятся в графическом пользовательском интерфейсе хранилища файлов после обновления колонки.

### <a name="download-files"></a>Скачивание файлов

![Вывод списка локальных файлов](../articles/cloud-shell/media/persisting-shell-storage/download.png)
1. На портале Azure перейдите к подключенному файловому ресурсу.
2. Выберите целевой файл.
3. Нажмите кнопку **Скачать**.

### <a name="upload-files"></a>Отправка файлов

![Локальные файлы для передачи](../articles/cloud-shell/media/persisting-shell-storage/upload.png)
1. Перейдите к подключенному файловому ресурсу.
2. Нажмите кнопку **Отправить**.
3. Выберите файл или файлы, которые необходимо передать.
4. Подтвердите передачу.

Теперь эти файлы должны появиться в каталоге `clouddrive` в Cloud Shell.