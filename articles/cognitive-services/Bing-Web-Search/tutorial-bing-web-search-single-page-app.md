---
title: Verwenden der Bing-Websuche-API in einer Single-Page-Web-App | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Bing-Websuche-API in einer Single-Page-Webanwendung verwenden.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: f22e38a1d6ee4042684b9822b58669bed6fe29a0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377034"
---
# <a name="tutorial-single-page-web-app"></a>Tutorial: Single-Page-Web-App

Mit der Bing-Websuche-API können Sie das Web durchsuchen und mithilfe von Suchabfragen unterschiedliche Ergebnistypen finden. In diesem Tutorial wird eine Single-Page-Webanwendung erstellt, die unter Verwendung der Bing-Websuche-API Suchergebnisse direkt auf der Seite anzeigt. Die Anwendung enthält HTML-, CSS- und JavaScript-Komponenten.

<!-- Remove until this can be replaced with a sanitized version.
![[Single-page Bing Web Search app]](media/cognitive-services-bing-web-api/web-search-spa-demo.png)
-->

> [!NOTE]
> Die JSON-Antwort und HTTP-Anforderungsinformationen werden über die JSON- und HTTP-Header am unteren Rand der Seite angezeigt, wenn Sie auf diese klicken. Diese Details sind hilfreich, wenn Sie sich genauer mit dem Dienst befassen.

In dieser Tutorial-App wird Folgendes veranschaulicht:

> [!div class="checklist"]
> * Aufrufen der Bing-Websuche-API in JavaScript
> * Übergeben von Suchoptionen an die Bing-Websuche-API
> * Anzeigen von Suchergebnissen für Webinhalte, Nachrichten, Bilder und Videos
> * Seitenweises Anzeigen von Suchergebnissen
> * Verarbeiten der Bing-Client-ID und des API-Abonnementschlüssels
> * Behandeln von möglicherweise auftretenden Fehlern

Die Tutorialseite ist unabhängig von anderen Komponenten und verwendet keine externen Frameworks oder Stylesheets. Auch Bilddateien kommen nicht zum Einsatz. Die Seite greift nur auf die am häufigsten unterstützten Features für JavaScript zurück und kann in aktuellen Versionen aller gängigen Webbrowser ausgeführt werden.

In diesem Tutorial werden nur ausgewählte Teile des Quellcodes erläutert. Der vollständige Quellcode ist [auf einer separaten Seite](tutorial-bing-web-search-single-page-app-source.md) verfügbar. Kopieren und fügen Sie diesen Code in einen Text-Editor ein, und speichern Sie diesen unter dem Namen `bing.html`.

## <a name="app-components"></a>App-Komponenten

Die Tutorialanwendung setzt sich ebenso wie alle anderen Single-Page-Webanwendungen aus drei Teilen zusammen:

> [!div class="checklist"]
> * Im HTML-Teil werden die Struktur und der Inhalt der Seite definiert.
> * Im CSS-Teil wird das Layout der Seite festgelegt.
> * Im JavaScript-Teil wird das Verhalten der Seite definiert.

In diesem Tutorial wird ein Großteil des HTML- und CSS-Codes nicht ausführlich behandelt, da dieser selbsterklärend ist.

Der HTML-Teil enthält das Suchformular, in dem der Benutzer eine Abfrage eingibt und Suchoptionen auswählt. Das Formular wird vom JavaScript-Teil ausgewertet, der die Suche mithilfe des `onsubmit`-Attributs des `<form>`-Tags ausführt:

```html
<form name="bing" onsubmit="return newBingWebSearch(this)">
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

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/search";

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

Das `onsubmit`-Attribut des HTML-Tags `form` ruft die `bingWebSearch`-Funktion auf, mit der die Suchergebnisse zurückgegeben werden. `bingWebSearch` verwendet zur Authentifizierung aller Abfragen `getSubscriptionKey`. Wie in der obigen Definition gezeigt, fordert `getSubscriptionKey` den Benutzer zur Eingabe des Schlüssels auf, falls dieser noch nicht eingegeben wurde. Der Schlüssel wird anschließend gespeichert und kann von der Anwendung auch später verwendet werden.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Auswählen der Suchoptionen

![[Bing-Websuche-API-Formular]](media/cognitive-services-bing-web-api/web-search-spa-form.png)

Das HTML-Formular enthält folgende Elemente:

| | |
|-|-|
| `where` | Ein Dropdownmenü zur Auswahl des Markts (Ort und Sprache), der für die Suche verwendet wird. |
| `query` | Das Textfeld, in das die Suchbegriffe eingegeben werden. |
| `what` | Kontrollkästchen, mit denen bestimmte Ergebnisse weiter oben in der Liste der Suchergebnisse angezeigt werden. Wenn beispielsweise das Kontrollkästchen für Bilder angeklickt wird, ändert sich deren Relevanzreihenfolge, und sie werden bei der Darstellung der Suchergebnisse priorisiert. |
| `when` | Dropdownmenü, mit dem die Suche optional auf den letzten Tag, die letzte Woche oder den letzten Monat eingeschränkt werden kann. |
| `safe` | Ein Kontrollkästchen, mit dem festgelegt wird, ob das SafeSearch-Feature von Bing verwendet werden soll. Mit diesem werden Inhalte herausgefiltert, die für Minderjährige nicht geeignet sind. |
| `count` | Verborgenes Feld. Die Anzahl der Suchergebnisse, die bei jeder Anforderung zurückgegeben werden. Damit lässt sich die Anzahl der angezeigten Ergebnisse pro Seite festlegen. |
| `offset` | Verborgenes Feld. Der Offset für das erste Suchergebnis der Anforderung. Dieser wird zur Unterteilung der Suchergebnisse in mehrere Seiten verwendet. Der Offset wird bei einer neuen Anforderung auf `0` zurückgesetzt. |

> [!NOTE]
> Die Bing-Websuche-API stellt noch weitere Abfrageparameter bereit. Hier werden nur einige wenige verwendet.

Die JavaScript-Funktion `bingSearchOptions()` konvertiert diese Felder in das für die Bing-Suche-API erforderliche Format.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var what = [];
    for (var i = 0; i < form.what.length; i++) 
        if (form.what[i].checked) what.push(form.what[i].value);
    if (what.length) {
        options.push("promote=" + what.join(","));
        options.push("answerCount=9");
    }
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    options.push("textDecorations=true");
    options.push("textFormat=HTML");
    return options.join("&");
}
```

Der Parameter `SafeSearch` kann bei einem API-Aufruf beispielsweise die Werte `strict`, `moderate` oder `off` annehmen, wobei `moderate` der Standardwert ist. In unserem Formular wird hingegen ein Kontrollkästchen verwendet, das nur über zwei Zustände verfügt. Der JavaScript-Code konvertiert diese Einstellung entweder in `strict` oder `off` (`moderate` wird nicht verwendet).

Wenn eines der **Promote**-Kontrollkästchen aktiviert ist, wird der Abfrage zusätzlich ein `answerCount`-Parameter hinzugefügt. Bei der Verwendung des Parameters `promote` ist `answerCount` erforderlich. Letzterer wird auf `9` festgelegt und gibt dabei die Anzahl der Ergebnistypen an, die von der Bing-Websuche-API unterstützt werden. Durch diese Wertzuweisung wird sichergestellt, dass die größtmögliche Anzahl von Ergebnistypen zurückgegeben wird.

> [!NOTE]
> Durch die Priorisierung eines Ergebnistyps wird *nicht* sichergestellt, dass die Suchergebnisse tatsächlich den Ergebnistyp enthalten. Vielmehr erhöht sich dadurch die Relevanz dieser Ergebnistypen ausgehend von den üblichen Relevanzwerten. Wenn Sie Suchergebnisse auf bestimmte Ergebnisse einschränken möchten, können Sie dazu den `responseFilter`-Abfrageparameter verwenden oder einen anderen Endpunkt wie die Bing-Bildersuche-API oder die Bing-News-Suche-API verwenden.

Zusätzlich werden `textDecoration`- und `textFormat`-Abfrageparameter gesendet, damit der Suchbegriff in den Suchergebnissen fett hervorgehoben wird. Diese Werte sind im Skript hartcodiert.

## <a name="performing-the-request"></a>Ausführen der Anforderung

Die `BingWebSearch`-Funktion verwendet auf der Grundlage der Abfrage, der Optionszeichenfolge und des API-Schlüssels ein `XMLHttpRequest`-Objekt, um die Anforderung an den Bing-Websuche-API-Endpunkt zu senden.

```javascript
// perform a search given query, options string, and API key
function bingWebSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "paging1", "paging2", "error");

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
        return;
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
            if (jsobj._type === "SearchResponse" && "rankingResponse" in jsobj) {
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
> Eine erfolgreiche HTTP-Anforderung ist *nicht* zwangsläufig mit einer erfolgreichen Suche gleichzusetzen. Wenn bei einem Suchvorgang ein Fehler auftritt, gibt die Bing-Websuche-API einen HTTP-Statuscode zurück, der vom Code 200 abweicht und Fehlerinformationen in der JSON-Antwort enthält. Wenn die Anzahl der Anforderungen begrenzt war, gibt die API zusätzlich eine leere Antwort zurück.

Ein Großteil des Codes in den beiden vorangehenden Funktionen ist für die Fehlerbehandlung zuständig. In den folgenden Phasen können Fehler auftreten:

|Phase|Mögliche Fehler|Behandelt durch|
|-|-|-|
|Erstellen eines JavaScript-Anforderungsobjekts|Ungültige URL|`try`/`catch`-Block|
|Senden der Anforderung|Netzwerkfehler, abgebrochene Verbindungen|Ereignishandler `error` und `abort`|
|Durchführen der Suche|Ungültige Anforderung, ungültiger JSON-Code, Beschränkung der Anforderungsanzahl|Tests im `load`-Ereignishandler|

Fehler werden behandelt, indem `renderErrorMessage()` zusammen mit allen bekannten Fehlerinformationen aufgerufen wird. Wenn die Antwort alle Fehlertests besteht, wird `renderSearchResults()` zur Anzeige der Suchergebnisse auf der Seite aufgerufen.

## <a name="displaying-search-results"></a>Anzeigen der Suchergebnisse

Die Bing-Websuche-API setzt voraus, dass Sie sich die Ergebnisse in einer [festgelegten Reihenfolge anzeigen lassen](useanddisplayrequirements.md). Da die API viele unterschiedliche Antworttypen zurückgegeben kann, ist zur Anzeige der Ergebnisse mehr erforderlich als das Durchlaufen der übergeordneten `WebPages`-Collection in der JSON-Antwort. Wenn nur Ergebnisse eines Typs angezeigt werden sollen, können Sie den `responseFilter`-Abfrageparameter oder einen anderen Bing-Suche-API-Endpunkt verwenden.

In diesem Tutorial wird allerdings `rankingResponse` verwendet, um die Reihenfolge der Suchergebnisse festzulegen und diese anzuzeigen. Dieses Objekt verweist entweder auf Elemente in den Collections `WebPages` `News`, `Images` bzw. `Videos` oder aber auf andere übergeordnete Antwortcollections in der JSON-Antwort.

`rankingResponse` kann bis zu drei Suchergebnisse-Collections mit den Bezeichnungen `pole`, `mainline` und `sidebar` enthalten. 

Bei `pole` handelt es sich um das relevanteste Suchergebnis. Wenn dieses vorhanden ist, wird es in der Liste der Suchergebnisse oben angezeigt. `mainline` verweist auf den Hauptanteil der Suchergebnisse. Die Ergebnisse im Hauptbereich sollten unmittelbar nach `pole` oder ganz oben angezeigt werden, falls `pole` nicht vorhanden ist. 

Die letzte Collection `sidebar` verweist auf Hilfssuchergebnisse. Diese Ergebnisse entsprechen häufig verwandten Suchvorgängen oder Bildern. Diese Ergebnisse sollten nach Möglichkeit in einer Seitenleiste angezeigt werden. Wenn sich eine Seitenleiste aufgrund der Bildschirmgröße als unpraktisch erweist (beispielsweise bei einem mobilen Gerät), sollten diese Ergebnisse nach den `mainline`-Ergebnissen angezeigt werden.

Jedes Element in einer `rankingResponse`-Collection verweist auf die tatsächlichen Suchergebnisse. Dieser Verweis kann auf zwei Arten erfolgen.

| | |
|-|-|
|`id`|`id` ähnelt einer URL, sollte allerdings nicht für Links verwendet werden. Der `id`-Typ eines Rangfolgenergebnisses stimmt mit der `id` eines Suchergebniselements in einer Antwortcollection *oder* einer gesamten Antwortcollection (beispielsweise `Images`) überein.
|`answerType`, `resultIndex`|`answerType` verweist auf die übergeordnete Antwortcollection, die das Ergebnis enthält (beispielsweise `WebPages`). `resultIndex` verweist auf den Ergebnisindex innerhalb dieser Collection. Wenn `resultIndex` nicht angegeben wird, verweist das Rangfolgenergebnis auf die gesamte Collection.

> [!NOTE]
> Weitere Informationen zu diesem Teil der Suchantwort finden Sie unter [Sortieren von Ergebnissen nach Rangfolge](rank-results.md).

Sie können bei der Suche nach dem Suchergebniselement, auf das verwiesen wird, die für Ihre Anwendung am besten geeignete Methode verwenden. In diesem Tutorialcode werden `answerType` und `resultIndex` zum Finden der Suchergebnisse verwendet.

Abschließend soll nun die `renderSearchResults()`-Funktion betrachtet werden. Die Funktion durchläuft die drei `rankingResponse`-Collections, die die drei Bereiche der Suchergebnisse darstellen. Zum Rendern der Bereichsergebnisse wird `renderResultsItems()` für jeden Bereich aufgerufen.

```javascript
// render the search results given the parsed JSON response
function renderSearchResults(results) {

    // if spelling was corrected, update search field
    if (results.queryContext.alteredQuery) 
        document.forms.bing.query.value = results.queryContext.alteredQuery;

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

`renderResultsItems()` durchläuft wiederum die Elemente in jedem `rankingResponse`-Bereich, ordnet mithilfe der Felder `answerType` und `resultIndex` jedem Rangfolgenergebnis ein Suchergebnis zu und ruft die entsprechende Renderingfunktion auf, um den HTML-Code für das Ergebnis zu erstellen. 

Wenn `resultIndex` nicht für ein bestimmtes Rangfolgenelement angegeben wird, durchläuft `renderResultsItems()` alle Ergebnisse dieses Typs und ruft die Renderingfunktion für jedes Element auf. 

In jedem Fall wird der erzeugte HTML-Code in das entsprechende `<div>`-Element auf der Seite eingefügt.

```javascript
// render search results from rankingResponse object in specified order
function renderResultsItems(section, results) {

    var items = results.rankingResponse[section].items;
    var html = [];
    for (var i = 0; i < items.length; i++) {
        var item = items[i];
        // collection name has lowercase first letter while answerType has uppercase
        // e.g. `WebPages` rankingResult type is in the `webPages` top-level collection
        var type = item.answerType[0].toLowerCase() + item.answerType.slice(1);
        // must have results of the given type AND a renderer for it
        if (type in results && type in searchItemRenderers) {
            var render = searchItemRenderers[type];
            // this ranking item refers to ONE result of the specified type
            if ("resultIndex" in item) {
                html.push(render(results[type].value[item.resultIndex], section));
            // this ranking item refers to ALL results of the specified type
            } else {
                var len = results[type].value.length;
                for (var j = 0; j < len; j++) {
                    html.push(render(results[type].value[j], section, j, len));
                }
            }
        }
    }
    return html.join("\n\n");
}
```

## <a name="rendering-result-items"></a>Rendern von Ergebniselementen

Im JavaScript-Code dieses Tutorials enthält das `searchItemRenderers`-Objekt *Renderer*, also Funktionen, die für jeden Suchergebnistyp HTML-Code generieren.

```javascript
// render functions for various types of search results
searchItemRenderers = { 
    webPages: function(item) { ... },
    news: function(item) { ... },
    images: function(item, section, index, count) { ... },
    videos: function(item, section, index, count) { ... },
    relatedSearches: function(item, section, index, count) { ... }
}
```

> [!NOTE]
> Die Tutorial-App nutzt Renderer für Webseiten, Nachrichtenelemente, Bilder, Videos und verwandte Suchvorgänge. In Ihrer eigenen Anwendung sind Renderer für alle Ergebnistypen erforderlich, die möglicherweise empfangen werden. Hierzu können etwa Berechnungen, Rechtschreibvorschläge, Entitäten, Zeitzonen und Definitionen gehören.

Einige der im Tutorial verwendeten Renderingfunktionen akzeptieren nur den `item`-Parameter. Bei diesem handelt es sich um ein JavaScript-Objekt, das genau ein Suchergebnis darstellt. Andere Funktionen akzeptieren zusätzliche Parameter, mit denen Elemente je nach Kontext unterschiedlich gerendert werden. Ein Renderer, der diese Informationen nicht verwendet, muss diese Parameter nicht akzeptieren.

Folgende Kontextargumente sind verfügbar:

| | |
|-|-|
|`section`|Der Ergebnisbereich (`pole`, `mainline` oder `sidebar`), in dem das Element angezeigt wird.
|`index`<br>`count`|Verfügbar, wenn durch das `rankingResponse`-Element angegeben wird, dass alle Ergebnisse einer Collection angezeigt werden sollen. Andernfalls wird `undefined` verwendet. Für diese Parameter wird der Index des Elements innerhalb der zugehörigen Collection und die Gesamtzahl der Elemente in der Collection angegeben. Sie können mit diesen Informationen beispielsweise die Ergebnisse nummerieren oder für das erste und letzte Element unterschiedlichen HTML-Code erzeugen.|

In der Tutorial-App nutzen die Renderer `images` und `relatedSearches` Kontextargumente, um den erzeugten HTML-Code anzupassen. Der Renderer `images` soll nun genauer betrachtet werden:

```javascript
searchItemRenderers = { 
    // render image result using thumbnail
    images: function(item, section, index, count) {
        var height = 60;
        var width = Math.round(height * item.thumbnail.width / item.thumbnail.height);
        var html = [];
        if (section === "sidebar") {
            if (index) html.push("<br>");
        } else {
            if (!index) html.push("<p class='images'>");
        }
        html.push("<a href='" + item.hostPageUrl + "'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + " title='" + title + "' alt='" + title + "'>");
        html.push("</a>");
        return html.join("");
    }, // other renderers omitted
}
```

Die Funktion zum Rendern von Bildern führt folgende Vorgänge aus:

> [!div class="checklist"]
> * Berechnen der Miniaturbildgröße (die Breite variiert, die Höhe ist auf 60 Pixel festgelegt)
> * Kontextabhängiges Einfügen des HTML-Codes, der dem Bildergebnis vorausgeht
> * Erstellen des HTML-`<a>`-Tags, das einen Link zur Seite mit dem Bild erstellt
> * Erstellen des HTML-`<img>`-Tags, mit dem das Miniaturbild angezeigt wird 

Der Bildrenderer verwendet die Variablen `section` und `index`, um die Ergebnisse je nach Anzeigeposition unterschiedlich darzustellen. Zwischen den Bildergebnissen in der Seitenleiste wird ein Zeilenumbruch (`<br>`-Tag) eingefügt, damit in der Seitenleiste eine Spalte mit Bildern angezeigt wird. In anderen Bereichen geht dem ersten Bildergebnis `(index === 0)` ein `<p>`-Tag voran. Andernfalls würden sich die Miniaturbilder zu dicht beieinander befinden und sich der Größe des Browserfensters anpassen.

Die Größe des Miniaturbilds wird sowohl im `<img>`-Tag als auch in den Feldern `h` und `w` in der URL des Miniaturbilds verwendet. Der [Bing-Dienst für Miniaturbilder](resize-and-crop-thumbnails.md) übermittelt anschließend ein Miniaturbild in genau dieser Größe. Die Attribute `title` und `alt` zur Beschreibung des Bilds werden aus dem Bildnamen und dem Hostnamen aus der URL erstellt.

Die Bilder werden wie unten gezeigt im Hauptbereich für Suchergebnisse dargestellt.

![[Bing-Bilderergebnisse]](media/cognitive-services-bing-web-api/web-search-spa-images.png)

## <a name="persisting-client-id"></a>Persistentes Speichern der Client-ID

Antworten, die von Bing-Suche-APIs gesendet werden, können einen `X-MSEdge-ClientID`-Header enthalten, der mit den nachfolgenden Anforderungen an die API zurückgesendet werden sollte. Wenn mehrere Bing-Suche-APIs genutzt werden, sollte nach Möglichkeit dieselbe Client-ID für alle APIs verwendet werden.

Durch die Bereitstellung des `X-MSEdge-ClientID`-Headers können die Bing-Suche-APIs alle Suchvorgänge eines Benutzers zuordnen, was zwei entscheidende Vorteile mit sich bringt.

Erstens kann die Bing-Suchmaschine auf diese Weise Kontextinformationen aus vorherigen Suchvorgängen nutzen, um den Benutzern relevantere Ergebnisse anzuzeigen. Wenn ein Benutzer in der Vergangenheit beispielsweise nach Segelbegriffen gesucht hat, können bei einer Suche nach „Knoten“ möglicherweise Informationen zu Segelknoten zurückgegeben werden.

Zweitens wählt Bing möglicherweise Benutzer zufällig aus, die die Möglichkeit haben, neue Features zu testen, bevor diese allen Benutzern zur Verfügung gestellt werden. Indem Sie bei jeder Anforderung dieselbe Client-ID bereitstellen, stellen Sie sicher, dass Benutzer, die zur Nutzung eines neuen Features ausgewählt wurden, dieses Feature dauerhaft nutzen können. Ohne die Client-ID wird das Feature in den Suchergebnissen möglicherweise scheinbar willkürlich aus- oder eingeblendet.

Durch Browsersicherheitsrichtlinien (CORS) kann der `X-MSEdge-ClientID`-Header möglicherweise nicht von JavaScript verwendet werden. Diese Einschränkung tritt auf, wenn sich die Herkunft der Suchantwort von derjenigen der Seite unterscheidet, die den Suchvorgang angefordert hat. In einer Produktionsumgebung sollten Sie zum Umgang mit dieser Richtlinie ein serverseitiges Skript hosten, das den API-Aufruf für die Domain durchführt, die auch für die Webseite genutzt wird. Da die Herkunft des Skripts mit derjenigen der Webseite übereinstimmt, kann der `X-MSEdge-ClientID`-Header von JavaScript verwendet werden.

> [!NOTE]
> In einer Webanwendung für eine Produktionsumgebung sollten Sie die Anforderung in jedem Fall serverseitig ausführen. Andernfalls müsste der Schlüssel der Bing-Suche-API auf der Webseite hinterlegt werden, wo er im Quelltext für alle Personen zugänglich ist. Dies müssen Sie vermeiden, da ansonsten unbefugte Dritte Anforderungen unter Verwendung Ihres API-Abonnementschlüssels Anforderungen senden können, die Ihnen in Rechnung gestellt werden.

In der Entwicklungsphase können Sie die Bing-Websuche-API-Anforderung über einen CORS-Proxy senden. In der Antwort eines solchen Proxys befindet sich ein `Access-Control-Expose-Headers`-Header. Dieser enthält eine Whitelist mit Antwortheadern, die JavaScript zur Verfügung gestellt werden.

Die Installation eines CORS-Proxys, mit dem die Tutorial-App auf den Client-ID-Header zugreifen kann, ist schnell und unkompliziert. [Installieren Sie Node.js](https://nodejs.org/en/download/), falls Sie dies noch nicht getan haben. Geben Sie anschließend den folgenden Befehl in ein Befehlsfenster ein:

    npm install -g cors-proxy-server

Passen Sie den Bing-Websuche-API-Endpunkt in der HTML-Datei wie folgt an:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Starten Sie abschließend den CORS-Proxy mit dem folgenden Befehl:

    cors-proxy-server

Lassen Sie das Fenster während der Nutzung der Tutorial-App geöffnet. Wenn Sie das Fenster schließen, wird auch die Ausführung des Proxys beendet. Im Bereich mit den erweiterbaren HTTP-Headern unter den Suchergebnissen wird nun u.a. der `X-MSEdge-ClientID`-Header angezeigt. Hier können Sie überprüfen, ob dieser für alle Anforderungen identisch ist.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Visual Search mobile app tutorial (Tutorial: Erstellen einer mobilen App für die visuelle Suche)](computer-vision-web-search-tutorial.md)

> [!div class="nextstepaction"]
> [Referenz zur Bing-Websuche-API](//docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
