---
title: Verwenden der Bing-Bildersuche-API in einer Single-Page-Web-App | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Bing-Bildersuche-API in einer Single-Page-Webanwendung verwenden.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: d0e1dc24513c8fc3a405cf1c18f531a0c58fad13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377027"
---
# <a name="tutorial-single-page-web-app"></a>Tutorial: Single-Page-Web-App

Mit der Bing-Bildersuche-API können Sie das Web durchsuchen und für eine Suchabfrage relevante Bildergebnisse abrufen. In diesem Tutorial wird eine Single-Page-Webanwendung erstellt, die unter Verwendung der Bing-Bildersuche-API Suchergebnisse direkt auf der Seite anzeigt. Die Anwendung enthält HTML-, CSS- und JavaScript-Komponenten.

<!-- Remove until we can sanitize images
![[Single-page Bing Image Search app]](media/cognitive-services-bing-images-api/image-search-spa-demo.png)
-->

> [!NOTE]
> Die JSON-Antwort und HTTP-Anforderungsinformationen werden über die JSON- und HTTP-Überschriften am unteren Rand der Seite angezeigt, wenn Sie auf diese klicken. Diese Details sind hilfreich, wenn Sie sich genauer mit dem Dienst befassen.

Mit dieser Tutorial wird folgendes veranschaulicht:

> [!div class="checklist"]
> * Aufrufen der Bing-Bildersuche-API in JavaScript
> * Übergeben von Suchoptionen an die Bing-Bildersuche-API
> * Anzeigen von Suchergebnissen
> * Seitenweises Anzeigen von Suchergebnissen
> * Verarbeiten der Bing-Client-ID und des API-Abonnementschlüssels
> * Behandeln von möglicherweise auftretenden Fehlern

Die Tutorialseite ist unabhängig von anderen Komponenten und verwendet keine externen Frameworks oder Stylesheets. Auch Bilddateien kommen nicht zum Einsatz. Die Seite greift nur auf die am häufigsten unterstützten Features für JavaScript zurück und kann in aktuellen Versionen aller gängigen Webbrowser ausgeführt werden.

In diesem Tutorial werden nur ausgewählte Teile des Quellcodes erläutert. Der vollständige Quellcode ist [auf einer separaten Seite](tutorial-bing-image-search-single-page-app-source.md) verfügbar. Kopieren Sie diesen Code, fügen Sie ihn in einen Text-Editor ein, und speichern Sie die Datei als `bing.html`.

> [!NOTE]
> Dieses Tutorial ähnelt dem [Tutorial zum Verwenden der Bing-Websuche-API in Single-Page-Apps](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md) sehr stark, befasst sich aber nur mit Bildersuchergebnissen.

## <a name="app-components"></a>App-Komponenten

Die Tutorialanwendung setzt sich ebenso wie alle anderen Single-Page-Webanwendungen aus drei Teilen zusammen:

> [!div class="checklist"]
> * Im HTML-Teil werden die Struktur und der Inhalt der Seite definiert.
> * Im CSS-Teil wird das Layout der Seite festgelegt.
> * Im JavaScript-Teil wird das Verhalten der Seite definiert.

In diesem Tutorial wird ein Großteil des HTML- und CSS-Codes nicht ausführlich behandelt, da dieser selbsterklärend ist.

Der HTML-Teil enthält das Suchformular, in dem der Benutzer eine Abfrage eingibt und Suchoptionen auswählt. Das Formular wird vom JavaScript-Teil ausgewertet, der die Suche mithilfe des `onsubmit`-Attributs des `<form>`-Tags ausführt:

```html
<form name="bing" onsubmit="return newBingImageSearch(this)">
```

Der `onsubmit`-Handler gibt `false` zurück, wodurch verhindert wird, dass das Formular an den Server gesendet wird. Der JavaScript-Code ist dafür verantwortlich, die erforderlichen Informationen aus dem Formular abzurufen und die Suche durchzuführen.

Der HTML-Teil enthält auch die Bereiche (HTML-`<div>`-Tags), in denen die Suchergebnisse angezeigt werden.

## <a name="managing-subscription-key"></a>Verwalten des Abonnementschlüssels

Zum Speichern des Schlüssels wird der persistente Browserspeicher verwendet. Auf diese Weise müssen Sie nicht den Abonnementschlüssel der Bing-Suche-API im Code hinterlegen. Wenn kein Schlüssel gespeichert ist, wird der Benutzer zur Eingabe des Schlüssels aufgefordert. Dieser wird dann für die spätere Verwendung gespeichert. Wird der Schlüssel zu einem späteren Zeitpunkt von der API zurückgewiesen, wird der gespeicherte Schlüssel für ungültig erklärt, und der Benutzer wird zur erneuten Eingabe aufgefordert.

In den Funktionen `storeValue` und `retrieveValue` wird entweder das `localStorage`-Objekt (falls vom Browser unterstützt) oder ein Cookie verwendet. Diese Funktionen werden in der `getSubscriptionKey()`-Funktion zum Speichern und Abrufen des Benutzerschlüssels verwendet.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
function getSubscriptionKey() {
    var key = retrieveValue(API_KEY_COOKIE);
    while (key.length !== 32) {
        key = prompt("Enter Bing Search API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(API_KEY_COOKIE, key);
    return key;
}
```

Das `onsubmit`-Attribut des HTML-Tags `<form>` ruft die `bingWebSearch`-Funktion auf, mit der die Suchergebnisse zurückgegeben werden. `bingWebSearch` verwendet zur Authentifizierung aller Abfragen `getSubscriptionKey`. Wie in der obigen Definition gezeigt, fordert `getSubscriptionKey` den Benutzer zur Eingabe des Schlüssels auf, fall dieser noch nicht eingegeben wurde. Der Schlüssel wird anschließend gespeichert und kann von der Anwendung auch später verwendet werden.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Auswählen der Suchoptionen

![[Bing-Bildersuche-Formular]](media/cognitive-services-bing-images-api/image-search-spa-form.png)

Das HTML-Formular enthält die folgenden Steuerelemente:

| | |
|-|-|
|`where`|Ein Dropdownmenü zur Auswahl des Marktes (Ort und Sprache), der für die Suche verwendet wird.|
|`query`|Das Textfeld, in das die Suchbegriffe eingegeben werden.|
|`aspect`|Optionsfelder, mit denen die Proportionen der gefundenen Bilder ausgewählt werden können: ungefähr quadratisch, breit oder hoch.|
|`color`|Damit wird eine Farbe, Schwarzweiß oder eine vorherrschende Farbe ausgewählt.
|`when`|Dropdownmenü, mit dem die Suche optional auf den letzten Tag, die letzte Woche oder den letzten Monat eingeschränkt werden kann.|
|`safe`|Ein Kontrollkästchen, mit dem festgelegt wird, ob das SafeSearch-Feature von Bing verwendet werden soll. Mit diesem werden Inhalte herausgefiltert, die für Minderjährige nicht geeignet sind.|
|`count`|Verborgenes Feld. Die Anzahl der Suchergebnisse, die bei jeder Anforderung zurückgegeben werden. Damit lässt sich die Anzahl der angezeigten Ergebnisse pro Seite festlegen.|
|`offset`|Verborgenes Feld. Der Offset für das erste Suchergebnis der Anforderung. Dieser wird zur Unterteilung der Suchergebnisse in mehrere Seiten verwendet. Der Offset wird bei einer neuen Anforderung auf `0` zurückgesetzt.|
|`nextoffset`|Verborgenes Feld. Nachdem ein Suchergebnis empfangen wurde, wird dieses Feld auf den Wert von `nextOffset` in der Antwort festgelegt. Durch Verwendung dieses Felds werden überlappende Ergebnisse auf aufeinander folgenden Seiten vermieden.|
|`stack`|Verborgenes Feld. Eine JSON-codierte Liste der Offsets vorhergehender Seiten mit Suchergebnissen, um zu vorherigen Seiten zurück zu navigieren.|

> [!NOTE]
> Die Bing-Bildersuche-API stellt noch weitere Abfrageparameter bereit. Hier werden nur einige wenige verwendet.

Die JavaScript-Funktion `bingSearchOptions()` konvertiert diese Felder in eine Teilabfragezeichenfolge in dem für die Bing-Suche-API erforderlichen Format.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var aspect = "all";
    for (var i = 0; i < form.aspect.length; i++) {
        if (form.aspect[i].checked) {
            aspect = form.aspect[i].value;
            break;
        }
    }
    options.push("aspect=" + aspect);
    if (form.color.value) options.push("color=" + form.color.value);
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    return options.join("&");
}
```

Für das SafeSearch-Feature können die Werte `strict`, `moderate` oder `off` festgelegt werden, wobei `moderate` der Standardwert ist. In unserem Formular wird hingegen ein Kontrollkästchen verwendet, das nur über zwei Zustände verfügt. Der JavaScript-Code konvertiert diese Einstellung entweder in `strict` oder `off` (`moderate` wird nicht verwendet).

## <a name="performing-the-request"></a>Ausführen der Anforderung

Die `BingImageSearch`-Funktion verwendet auf der Grundlage der Abfrage, der Optionszeichenfolge und des API-Schlüssels ein `XMLHttpRequest`-Objekt, um die Anforderung an den Bing-Bildersuche-API-Endpunkt zu senden.

```javascript
// perform a search given query, options string, and API key
function bingImageSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("results", "related", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
    var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

    // open the request
    try {
        request.open("GET", queryurl);
    } 
    catch (e) {
        renderErrorMessage("Bad request (invalid URL)\n" + queryurl);
        return false;
    }

    // add request headers
    request.setRequestHeader("Ocp-Apim-Subscription-Key", key);
    request.setRequestHeader("Accept", "application/json");
    var clientid = retrieveValue(CLIENT_ID_COOKIE);
    if (clientid) request.setRequestHeader("X-MSEdge-ClientID", clientid);
    
    // event handler for successful response
    request.addEventListener("load", handleBingResponse);
    
    // event handler for erorrs
    request.addEventListener("error", function() {
        renderErrorMessage("Error completing request");
    });

    // event handler for aborted request
    request.addEventListener("abort", function() {
        renderErrorMessage("Request aborted");
    });

    // send the request
    request.send();
    return false;
}
```

Nach erfolgreichem Abschluss der HTTP-Anforderung ruft JavaScript den `load`-Ereignishandler und damit die `handleBingResponse()`-Funktion auf, um eine erfolgreiche HTTP-GET-Anforderung an die API zu verarbeiten. 

```javascript
// handle Bing search request results
function handleBingResponse() {
    hideDivs("noresults");

    var json = this.responseText.trim();
    var jsobj = {};

    // try to parse JSON results
    try {
        if (json.length) jsobj = JSON.parse(json);
    } catch(e) {
        renderErrorMessage("Invalid JSON response");
    }

    // show raw JSON and HTTP request
    showDiv("json", preFormat(JSON.stringify(jsobj, null, 2)));
    showDiv("http", preFormat("GET " + this.responseURL + "\n\nStatus: " + this.status + " " + 
        this.statusText + "\n" + this.getAllResponseHeaders()));

    // if HTTP response is 200 OK, try to render search results
    if (this.status === 200) {
        var clientid = this.getResponseHeader("X-MSEdge-ClientID");
        if (clientid) retrieveValue(CLIENT_ID_COOKIE, clientid);
        if (json.length) {
            if (jsobj._type === "Images") {
                if (jsobj.nextOffset) document.forms.bing.nextoffset.value = jsobj.nextOffset;
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    }

    // Any other HTTP response is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSubscriptionKey();

        // some error responses don't have a top-level errors object, so gin one up
        var errors = jsobj.errors || [jsobj];
        var errmsg = [];

        // display HTTP status code
        errmsg.push("HTTP Status " + this.status + " " + this.statusText + "\n");

        // add all fields from all error responses
        for (var i = 0; i < errors.length; i++) {
            if (i) errmsg.push("\n");
            for (var k in errors[i]) errmsg.push(k + ": " + errors[i][k]);
        }

        // also display Bing Trace ID if it isn't blocked by CORS
        var traceid = this.getResponseHeader("BingAPIs-TraceId");
        if (traceid) errmsg.push("\nTrace ID " + traceid);

        // and display the error message
        renderErrorMessage(errmsg.join("\n"));
    }
}
```

> [!IMPORTANT]
> Eine erfolgreiche HTTP-Anforderung ist *nicht* zwangsläufig mit einer erfolgreichen Suche gleichzusetzen. Wenn bei einem Suchvorgang ein Fehler auftritt, gibt die Bing-Bildersuche-API einen HTTP-Statuscode zurück, der vom Code 200 abweicht und Fehlerinformationen in der JSON-Antwort enthält. Wenn die Anzahl der Anforderungen begrenzt war, gibt die API zusätzlich eine leere Antwort zurück.

Ein Großteil des Codes in den beiden vorangehenden Funktionen ist für die Fehlerbehandlung zuständig. In den folgenden Phasen können Fehler auftreten:

|Phase|Mögliche Fehler|Behandelt durch|
|-|-|-|
|Erstellen eines JavaScript-Anforderungsobjekts|Ungültige URL|`try`/`catch`-Block|
|Senden der Anforderung|Netzwerkfehler, abgebrochene Verbindungen|Ereignishandler `error` und `abort`|
|Durchführen der Suche|Ungültige Anforderung, ungültiger JSON-Code, Beschränkung der Anforderungsanzahl|Tests im `load`-Ereignishandler|

Fehler werden behandelt, indem `renderErrorMessage()` zusammen mit allen bekannten Fehlerinformationen aufgerufen wird. Wenn die Antwort alle Fehlertests besteht, wird `renderSearchResults()` zur Anzeige der Suchergebnisse auf der Seite aufgerufen.

## <a name="displaying-search-results"></a>Anzeigen der Suchergebnisse

Die Hauptfunktion zum Anzeigen der Suchergebnisse ist `renderSearchResults()`. Diese Funktion nimmt den vom Bing-Bildersuche-Dienst zurückgegebenen JSON-Code und rendert die Bilder sowie verwandte Suchvorgänge (falls vorhanden).

```javascript
function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    showDiv("results", renderImageResults(results.value));
    if (results.relatedSearches)
        showDiv("sidebar", renderRelatedItems(results.relatedSearches));
}
```

Die Hauptergebnisse der Bildersuche werden als `value`-Objekt der obersten Ebene in der JSON-Antwort zurückgegeben. Diese werden dann an die `renderImageResults()`-Funktion übergeben, die diese durchläuft und eine separate Funktion zum Rendern der einzelnen Elemente in HTML aufruft. Der resultierende HTML-Code wird an `renderSearchResults()` zurückgegeben, wo es in den Bereich `results` auf der Seite eingefügt wird.

```javascript
function renderImageResults(items) {
    var len = items.length;
    var html = [];
    if (!len) {
        showDiv("noresults", "No results.");
        hideDivs("paging1", "paging2");
        return "";
    }
    for (var i = 0; i < len; i++) {
        html.push(searchItemRenderers.images(items[i], i, len));
    }
    return html.join("\n\n");
}
```

Die Bing-Bildersuche-API gibt bis zu vier verschiedene Arten verwandter Ergebnisse zurück, und zwar jeweils in einem eigenen Objekt der obersten Ebene. Sie lauten wie folgt:

|||
|-|-|
|`pivotSuggestions`|Abfragen, bei denen ein Pivot-Wort in der ursprünglichen Suche durch ein anderes ersetzt wird. Wenn Sie beispielsweise nach „rote Blumen“ suchen, kann ein Pivot-Wort „rote“ sein, und ein Pivot-Vorschlag kann „gelbe Blumen“ sein.|
|`queryExpansions`|Abfragen, bei denen die ursprüngliche Suche durch Hinzufügen weiterer Begriffe eingegrenzt wird. Wenn Sie beispielsweise nach „Microsoft Surface“ suchen, kann eine Abfrageerweiterung „Microsoft Surface Pro“ sein.|
|`relatedSearches`|Abfragen, die ebenfalls von anderen Benutzern eingegeben wurden, die auch die ursprüngliche Suche eingegeben hatten. Wenn Sie z.B. nach „Mount Rainier“ suchen, kann eine verwandte Suche „Mount Saint Helens“ lauten.|
|`similarTerms`|Abfragen, die von der Bedeutung her der ursprünglichen Suche ähneln. Wenn Sie beispielsweise nach „Kätzchen“ suchen, kann ein ähnlicher Begriff „süß“ sein.|

Wie bereits bei `renderSearchResults()` gezeigt, werden nur die `relatedItems`-Vorschläge gerendert und die resultierenden Links auf der Randleiste der Seite eingefügt.

## <a name="rendering-result-items"></a>Rendern von Ergebniselementen

Im JavaScript-Code dieses Tutorials enthält das `searchItemRenderers`-Objekt *Renderer*, also Funktionen, die für jeden Suchergebnistyp HTML-Code generieren.

```javascript
searchItemRenderers = { 
    images: function(item, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```

Für eine Rendererfunktion können die folgenden Parameter angegeben werden:

| | |
|-|-|
|`item`|Das JavaScript-Objekt mit Eigenschaften des Elements, z.B. seine URL und Beschreibung.|
|`index`|Der Index des Ergebniselements innerhalb der Auflistung.|
|`count`|Die Anzahl der Elemente in der Auflistung der Suchergebniselemente.|

Die Parameter `index` und `count` können zum Nummerieren von Ergebnissen, zum Generieren von speziellem HTML-Code für den Anfang oder das Ende einer Auflistung, zum Einfügen von Zeilenumbrüchen nach einer bestimmten Anzahl von Elementen usw. verwendet werden. Wenn ein Renderer diese Funktionalität nicht benötigt, muss er diese beiden Parameter nicht akzeptieren.

Der Renderer `images` soll nun genauer betrachtet werden:

```javascript
    images: function (item, index, count) {
        var height = 120;
        var width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
        var html = [];
        if (index === 0) html.push("<p class='images'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<p class='images' style='max-width: " + width + "px'>");
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + "'>");
        html.push("<br>");
        html.push("<nobr><a href='" + item.contentUrl + "'>Image</a> - ");
        html.push("<a href='" + item.hostPageUrl + "'>Page</a></nobr><br>");
        html.push(title.replace("\n", " (").replace(/([a-z0-9])\.([a-z0-9])/g, "$1.<wbr>$2") + ")</p>");
        return html.join("");
    }, // relatedSearches renderer omitted
```

Die Funktion zum Rendern von Bildern führt folgende Vorgänge aus:

> [!div class="checklist"]
> * Berechnet die Miniaturbildgröße (die Breite variiert und weist einen Mindestwert von 120 Pixel auf, die Höhe ist auf 90 Pixel festgelegt).
> * Erstellt das HTML-`<img>`-Tag, mit dem das Miniaturbild angezeigt wird. 
> * Erstellt die HTML-`<a>`-Tags, die eine Verknüpfung mit dem Bild und der Seite, die das Bild enthält, darstellen.
> * Erstellt die Beschreibung, die Informationen über das Bild und die Website, auf der sich das Bild befindet, angibt.

Die `index`-Variable wird getestet, um ein `<p>`-Tag vor den ersten Bildergebnis einzufügen. Andernfalls würden sich die Miniaturbilder zu dicht beieinander befinden und sich der Größe des Browserfensters anpassen.

Die Größe des Miniaturbilds wird sowohl im `<img>`-Tag als auch in den Feldern `h` und `w` in der URL des Miniaturbilds verwendet. Der [Bing-Dienst für Miniaturbilder](resize-and-crop-thumbnails.md) übermittelt anschließend ein Miniaturbild in genau dieser Größe.

## <a name="persisting-client-id"></a>Persistentes Speichern der Client-ID

Antworten, die von Bing-Suche-APIs gesendet werden, können einen `X-MSEdge-ClientID`-Header enthalten, der mit den nachfolgenden Anforderungen an die API zurückgesendet werden sollte. Wenn mehrere Bing-Suche-APIs genutzt werden, sollte nach Möglichkeit dieselbe Client-ID für alle APIs verwendet werden.

Durch die Bereitstellung des `X-MSEdge-ClientID`-Headers können die Bing-Suche-APIs alle Suchvorgänge eines Benutzers zuordnen, was zwei entscheidende Vorteile mit sich bringt.

Erstens kann die Bing-Suchmaschine auf diese Weise Kontextinformationen aus vorherigen Suchvorgängen nutzen, um den Benutzern relevantere Ergebnisse anzuzeigen. Wenn ein Benutzer in der Vergangenheit beispielsweise nach Segelbegriffen gesucht hat, können bei einer Suche nach „Knoten“ möglicherweise Informationen zu Segelknoten zurückgegeben werden.

Zweitens wählt Bing möglicherweise Benutzer zufällig aus, die die Möglichkeit haben, neue Features zu testen, bevor diese allen Benutzern zur Verfügung gestellt werden. Indem Sie bei jeder Anforderung dieselbe Client-ID bereitstellen, stellen Sie sicher, dass Benutzer, die zur Nutzung eines neuen Features ausgewählt wurden, dieses Feature dauerhaft nutzen können. Ohne die Client-ID wird das Feature in den Suchergebnissen möglicherweise scheinbar willkürlich aus- oder eingeblendet.

Durch Browsersicherheitsrichtlinien (CORS) kann der `X-MSEdge-ClientID`-Header möglicherweise nicht von JavaScript verwendet werden. Diese Einschränkung tritt auf, wenn sich der Ursprung der Suchantwort von dem der Seite unterscheidet, die den Suchvorgang angefordert hat. In einer Produktionsumgebung sollten Sie zum Umgang mit dieser Richtlinie ein serverseitiges Skript hosten, das den API-Aufruf für die Domain durchführt, die auch für die Webseite genutzt wird. Da die Herkunft des Skripts mit derjenigen der Webseite übereinstimmt, kann der `X-MSEdge-ClientID`-Header von JavaScript verwendet werden.

> [!NOTE]
> In einer Webanwendung für eine Produktionsumgebung sollten Sie die Anforderung in jedem Fall serverseitig ausführen. Andernfalls müsste der Schlüssel der Bing-Suche-API auf der Webseite hinterlegt werden, wo er im Quelltext für alle Personen zugänglich ist. Dies müssen Sie vermeiden, da ansonsten unbefugte Dritte Anforderungen unter Verwendung Ihres API-Abonnementschlüssels senden können, die Ihnen in Rechnung gestellt werden.

In der Entwicklungsphase können Sie die Bing-Websuche-API-Anforderung über einen CORS-Proxy senden. In der Antwort eines solchen Proxys befindet sich ein `Access-Control-Expose-Headers`-Header. Dieser enthält eine Whitelist mit Antwortheadern, die JavaScript zur Verfügung gestellt wird.

Die Installation eines CORS-Proxys, mit dem die Tutorial-App auf den Client-ID-Header zugreifen kann, ist schnell und unkompliziert. [Installieren Sie Node.js](https://nodejs.org/en/download/), falls Sie dies noch nicht getan haben. Geben Sie anschließend den folgenden Befehl in ein Befehlsfenster ein:

    npm install -g cors-proxy-server

Passen Sie den Bing-Websuche-API-Endpunkt in der HTML-Datei wie folgt an:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Starten Sie abschließend den CORS-Proxy mit dem folgenden Befehl:

    cors-proxy-server

Lassen Sie das Fenster während der Nutzung der Tutorial-App geöffnet. Wenn Sie das Fenster schließen, wird auch die Ausführung des Proxys beendet. Im Bereich mit den erweiterbaren HTTP-Headern unter den Suchergebnissen wird nun u.a. der `X-MSEdge-ClientID`-Header angezeigt. Hier können Sie überprüfen, ob dieser für alle Anforderungen identisch ist.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Referenz zur Bing-Bildersuche-API](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

