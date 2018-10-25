---
title: 'Tutorial: Erstellen einer einseitigen Web-App – Visuelle Bing-Suche'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird beschrieben, wie Sie die API für die visuelle Bing-Suche in einer Single-Page-Webanwendung verwenden.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 10/04/2017
ms.author: bking
ms.openlocfilehash: a63107b86e82bf0bcd89523588414b45e6e21d5a
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465196"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Tutorial: Single-Page-Web-App für die visuelle Suche

Die API für die visuelle Bing-Suche umfasst ähnliche Möglichkeiten wie die Bilddetails auf „bing.com/images“. Bei der visuellen Suche können Sie ein Bild angeben und erhalten weitere Informationen zu dem Bild, wie z.B. visuell ähnliche Bilder, Einkaufsquellen, Webseiten mit diesem Bild und vieles mehr. 

In diesem Tutorial wird die Single-Page-Web-App aus dem Tutorial für die Bing-Bildersuche erweitert (siehe [Single-Page-Web-App](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)). Den vollständigen Quellcode zum Starten mit diesem Tutorial finden Sie unter [Single-Page-Web-App (Quellcode)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md). Den abschließenden Quellcode dieses Tutorials finden Sie unter [Single-Page-Web-App für die visuelle Suche](tutorial-bing-visual-search-single-page-app-source.md).

Die folgenden Aufgaben werden beschrieben:

> [!div class="checklist"]
> * Aufrufen der API für die visuelle Bing-Suche mit einem Bildauswertungstoken
> * Anzeigen ähnlicher Bilder

## <a name="call-bing-visual-search"></a>Aufrufen der visuellen Bing-Suche
Bearbeiten Sie das Tutorial für die Bing-Bildersuche, und fügen Sie den folgenden Code am Ende des Skriptelements in Zeile 409 hinzu. Dieser Code ruft die API für die visuelle Bing-Suche auf und zeigt die Ergebnisse an.

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

## <a name="capture-insights-token"></a>Erfassen des Auswertungstokens
Fügen Sie den folgenden Code im `searchItemsRenderer`-Objekt in Zeile 151 hinzu. Dieser Code fügt einen Link **find similar** hinzu, mit dem durch Klicken die Funktion `bingVisualSearch` aufgerufen wird. Die Funktion empfängt das Bildauswertungstoken (ImageInsightsToken) als Argument.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Anzeigen ähnlicher Bilder
Fügen Sie den folgenden HTML-Code in Zeile 601 hinzu. Dieser Markupcode fügt ein Element hinzu, mit dem die Ergebnisse des Aufrufs der API für die visuelle Bing-Suche angezeigt werden.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Mit dem neuen JavaScript-Code und den HTML-Elementen werden Suchergebnisse mit einem Link **find similar** angezeigt. Klicken Sie auf den Link, um den Abschnitt **Similar** mit Bildern zu füllen, die dem ausgewählten Bild ähneln. Möglicherweise müssen Sie den Abschnitt **Similar** erweitern, um die Bilder anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Quelle der Single-Page-Web-App für die visuelle Suche](tutorial-bing-visual-search-single-page-app-source.md)
> [Referenz zur API für die visuelle Bing-Suche](https://aka.ms/bingvisualsearchreferencedoc)