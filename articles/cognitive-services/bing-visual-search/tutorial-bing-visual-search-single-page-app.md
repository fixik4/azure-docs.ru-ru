---
title: " Создание одностраничного веб-приложения. Визуальный поиск Bing"
titleSuffix: Azure Cognitive Services
description: В этой статье показано, как интегрировать API визуального поиска Bing в одностраничное веб-приложение.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: e06fd7a4b2d072e5528643c2c8517d7545c36ef3
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338660"
---
# <a name="create-a-visual-search-single-page-web-app"></a>Создание одностраничного веб-приложения для визуального поиска 

В API визуального поиска Bing предоставляет возможности аналогичную на Bing.com/images сведения об образе. Функция визуального поиска позволяет указать изображение и получить о нем аналитические сведения, например визуально подобные изображения, ресурсы покупок, веб-страницы, которые содержат это изображения, и многое другое. 

В этой статье объясняется, как расширить одностраничное веб-приложение для API Bing для поиска изображений. Чтобы просмотреть этот учебник или получить используемый здесь исходный код, обратитесь к разделу [Руководство. Создание одностраничного веб-приложения с помощью API Bing для поиска изображений](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md). 

Полный исходный код для этого приложения (после его расширения для использования API визуального поиска Bing) можно найти в [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchApp.html).

## <a name="prerequisites"></a>Технические условия

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="call-the-bing-visual-search-api-and-handle-the-response"></a>Вызов API визуального поиска Bing и обработка ответа

Измените код, приведенный в руководстве по службе "Поиск изображений Bing". Для этого добавьте следующий код в конец элемента `<script>` (перед началом закрывающего тега `</script>`). Следующий код обрабатывает ответ от API визуального поиска, перебирает результаты и отображает их.

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}
```

Следующий код отправляет поисковый запрос в API с помощью прослушивателя событий для вызова `handleVisualSearchResponse()`.


```javascript
function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;       
    
    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>Запись токена аналитики

Добавьте следующий код в объект `searchItemsRenderer`. Этот код добавляет ссылку **find similar** (найти похожие), которая при нажатии вызывает функцию `bingVisualSearch`. Функция получает токен imageInsightsToken в качестве аргумента.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Отображение похожих изображений

Добавьте следующий HTML-код в строку 601. Этот код разметки добавляет элемент, используемый для отображения результатов вызова API Bing для наглядного поиска.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Когда все новые фрагменты кода JavaScript и HTML-элементы на своих местах, результаты поиска отображаются при нажатии ссылки **find similar**. Щелкните эту ссылку, чтобы раздел **Similar** (Похожие) заполнился изображениями, похожими на выбранное вами изображение. Для просмотра изображений может потребоваться развернуть раздел **Similar**.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Обрезка и отправка изображения](tutorial-visual-search-crop-area-results.md)
