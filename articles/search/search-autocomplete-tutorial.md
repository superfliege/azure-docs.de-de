---
title: Tutorial zum Hinzufügen der automatischen Vervollständigung zu Ihrem Suchfeld – Azure Search
description: Beispiele zum Verbessern der Benutzeroberfläche mit Ihren datenorientierten Anwendungen mithilfe der automatischen Vervollständigung und Vorschlägen von Azure Search.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: mcarter
ms.custom: seodec2018
ms.openlocfilehash: 10f86a482fbb35e7276f8f689a7eba184a7b624b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316181"
---
# <a name="tutorial-add-auto-complete-to-your-search-box-using-azure-search"></a>Tutorial: Hinzufügen der automatischen Vervollständigung von Azure Search zu Ihrem Suchfeld

In diesem Tutorial erfahren Sie, wie Sie [Vorschläge](https://docs.microsoft.com/rest/api/searchservice/suggestions), die [automatische Vervollständigung](https://docs.microsoft.com/rest/api/searchservice/autocomplete) und [Facets](search-faceted-navigation.md) in der [REST-API für Azure Search](https://docs.microsoft.com/rest/api/searchservice/) und im [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet) zur Erstellung eines leistungsfähigen Suchfelds verwenden. *Vorschläge* enthalten Empfehlungen von tatsächlichen Ergebnisse basierend auf der Eingabe des Benutzers. Die *automatische Vervollständigung*, [eine neue Vorschaufunktion](search-api-preview.md) in Azure Search, stellt Benennungen aus dem Index bereit, um die Eingabe des Benutzers zu vervollständigen. Im Folgenden werden mehrere Techniken miteinander verglichen, um die Produktivität des Benutzers zu verbessern und auf schnelle und einfache Weise zu finden, wonach der Benutzer sucht, indem die Suchfunktion direkt bei der Eingabe zur Verfügung gestellt wird.

Dieses Tutorial führt Sie durch eine ASP.NET MVC-basierte Anwendung, die C#-Code zum Aufrufen der [Azure Search-.NET-Clientbibliotheken](https://aka.ms/search-sdk) und JavaScript zum direkten Aufrufen der REST-API für Azure Search verwendet. Die Anwendung für dieses Tutorial ist auf einen Index ausgerichtet, der mit [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)-Beispieldaten aufgefüllt wurde. Sie können entweder den Index verwenden, der in der NYCJobs-Demo bereits konfiguriert wurde, oder Ihren eigenen Index mithilfe eines Datenladers in der NYCJobs-Beispiellösung auffüllen. Im Beispiel werden die JavaScript-Bibliotheken [jQuery UI](https://jqueryui.com/autocomplete/) und [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) zur Erstellung eines Suchfelds verwendet, das die automatische Vervollständigung unterstützt. Bei Verwendung dieser Komponenten mit Azure Search werden Ihnen mehrere Beispiele zur Unterstützung der automatischen Vervollständigung durch Textvervollständigung in Ihrem Suchfeld angezeigt.

Sie führen die folgenden Aufgaben aus:

> [!div class="checklist"]
> * Herunterladen und Konfigurieren der Projektmappe
> * Hinzufügen von Suchdienstinformationen zu Anwendungseinstellungen
> * Implementieren eines Eingabefelds für die Suche
> * Hinzufügen von Unterstützung für eine Liste zur automatischen Vervollständigung, die von einer Remotequelle abgerufen wird 
> * Abrufen von Vorschlägen und automatischer Vervollständigung mit dem .Net SDK und der REST-API
> * Unterstützung der clientseitigen Zwischenspeicherung zum Verbessern der Leistung 

## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio 2017. Sie können die kostenlose [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) verwenden. 

* Laden Sie den beispielhaften [Quellcode](https://github.com/azure-samples/search-dotnet-getting-started) für das Tutorial herunter.

* (Optional) Ein aktives Azure-Konto und ein Azure Search-Dienst. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/free/)registrieren. Informationen zur Dienstbereitstellung finden Sie unter [Erstellen eines Suchdiensts](search-create-service-portal.md). Das Konto und der Dienst sind optional, da dieses Tutorial mit einem gehosteten NYCJobs-Index abgeschlossen werden kann, der bereits für eine andere Demo vorhanden ist.

* (Optional) Laden Sie [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)-Beispielcode herunter, um die NYCJobs-Daten in einen Index Ihres Azure Search-Diensts zu importieren.

> [!Note]
> Bei Verwendung des kostenlosen Azure Search-Diensts gilt eine Obergrenze von drei Indizes. Der Datenlader NYCJobs erstellt zwei Indizes. Vergewissern Sie sich, dass Ihr Dienst über genügend freie Kapazität für die neuen Indizes verfügt.

### <a name="set-up-azure-search-optional"></a>Einrichten von Azure Search (optional)

Führen Sie die Schritte in diesem Abschnitt aus, wenn die Daten für die NYCJobs-Beispielanwendung in Ihren eigenen Index importiert werden sollen. Dieser Schritt ist optional.  Wenn Sie den bereitgestellten Beispielindex verwenden möchten, fahren Sie mit dem nächsten Abschnitt fort, in dem das Beispiel ausgeführt wird.

1. Öffnen Sie in Visual Studio im Ordner „DataLoader“ des NYCJobs-Beispielcodes die Projektmappendatei „DataLoader.sln“.

1. Aktualisieren Sie die Verbindungsinformationen für Ihren Azure Search-Dienst.  Öffnen Sie die Datei „App.config“ im DataLoader-Projekt, und ändern Sie die App-Einstellungen für „TargetSearchServiceName“ und „TargetSearchServiceApiKey“, um Ihren Azure Search-Dienst und Ihren API-Schlüssel für den Azure Search-Dienst widerzuspiegeln.  Diese können im Azure-Portal gefunden werden.

1. Drücken Sie die Taste F5, um die Anwendung zu starten.  Dadurch werden 2 Indizes erstellt und die NYCJob-Beispieldaten importiert.

1. Öffnen Sie in Visual Studio im Beispielcode des Tutorials die Projektmappendatei „AutocompleteTutorial.sln“.  Öffnen Sie im Projekt „AutocompleteTutorial“ die Datei „Web.config“, und ändern Sie die Werte für „SearchServiceName“ und „SearchServiceApiKey“ in die oben angegebenen Werte.

### <a name="running-the-sample"></a>Ausführen des Beispiels

Sie können nun die Beispielanwendung des Tutorials ausführen.  Öffnen Sie in Visual Studio die Projektmappendatei „AutocompleteTutorial.sln“, um das Tutorial auszuführen.  Die Lösung enthält ein ASP.NET MVC-Projekt.  Drücken Sie die Taste F5, um das Projekt auszuführen, und laden Sie die Seite in einen Browser Ihrer Wahl.  Im oberen Bereich wird Ihnen eine Option zur Auswahl von C# oder JavaScript angezeigt.  Die Option C# wird über den Browser im HomeController aufgerufen und verwendet das .Net SDK von Azure Search zum Abrufen von Ergebnissen.  Die Option JavaScript ruft die REST-API für Azure Search direkt über den Browser auf.  Diese Option erzielt eine deutlich bessere Leistung, da der Controller aus dem Flow herausgenommen wird.  Sie können die Option auswählen, die Ihren Anforderungen und Sprachpräferenzen entspricht.  Auf der Seite gibt es mehrere Beispiele zur automatischen Vervollständigung mit einer entsprechenden Anleitung.  Jedes Beispiel enthält empfohlenen Beispieltext, den Sie ausprobieren können.  Versuchen Sie, in jedes Suchfeld einige Buchstaben einzugeben, um zu sehen, was geschieht.

## <a name="how-this-works-in-code"></a>Funktionsweise im Code

Nachdem Sie die Beispiele im Browser gesehen haben, schauen wir uns nun das erste Beispiel im Detail an, um zu überprüfen, welche Komponenten involviert sind und wie diese funktionieren.

### <a name="search-box"></a>Suchfeld

Das Suchfeld ist bei jeder Sprachauswahl identisch.  Öffnen Sie die Datei „Index.cshtml“ unter dem Ordner \Views\Home. Der Aufbau des Suchfelds selbst ist einfach:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Dies ist ein einfaches Feld für die Texteingabe mit einer Klasse für das Format, einer von JavaScript referenzierten ID und Platzhaltertext.  Das Besondere ist im JavaScript zu finden.

### <a name="javascript-code-c"></a>JavaScript-Code (C#)

Im C#-Sprachbeispiel wird in der Datei „Index.cshtml“ JavaScript verwendet, um die Bibliothek jQuery UI zur automatischen Vervollständigung nutzen zu können.  Diese Bibliothek fügt den Prozess der automatischen Vervollständigung zum Suchfeld hinzu, indem im MVC-Controller asynchrone Aufrufe zum Abrufen von Empfehlungen durchgeführt werden.  Schauen Sie sich den JavaScript-Code im ersten Beispiel an:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Dieser Code wird im Browser beim Laden der Seite ausgeführt, um die automatische Vervollständigung für das Eingabefeld „example1a“ zu konfigurieren.  `minLength: 3` stellt sicher, dass Empfehlungen nur angezeigt werden, wenn mindestens drei Zeichen in das Suchfeld eingegeben wurden.  Der Quellwert ist wichtig:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Diese Zeile zeigt der API zur automatischen Vervollständigung, wo die Liste der unter dem Suchfeld anzuzeigenden Elemente abgerufen werden kann.  Da es sich hierbei um ein MVC-Projekt handelt, wird die Funktion „Suggest“ (Vorschlagen) in „HomeController.cs“ aufgerufen.  Dies wird im nächsten Abschnitt näher erläutert.  Darüber hinaus werden einige Parameter zur Steuerung von Hervorhebungen, Fuzzyübereinstimmungen und Benennungen übergeben.  Die JavaScript-API zur automatischen Vervollständigung fügt den Parameter für Benennungen hinzu.

#### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Erweitern des Beispiels zur Unterstützung von Fuzzyübereinstimmungen

Mit der Fuzzysuche können Sie Ergebnisse basierend auf nahen Übereinstimmungen abrufen, auch wenn der Benutzer ein Wort im Suchfeld falsch schreibt.  Lassen Sie uns dies ausprobieren, indem wir die Quellzeile ändern, um die Fuzzyübereinstimmung zu aktivieren.

Ändern Sie die folgende Zeile:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Und zwar in diesen Code:

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

Starten Sie die Anwendung durch Drücken der Taste F5.

Geben Sie ein Wort wie z.B. „execative“ ein, und beachten Sie, wie Ergebnisse für „executive“ zurückgegeben werden, auch wenn diese nicht genau mit den von Ihnen eingegebenen Buchstaben übereinstimmen.

### <a name="homecontrollercs-c"></a>„HomeController.cs“ (C#)

Nachdem wird den JavaScript-Code in dem Beispiel überprüft haben, schauen wir uns nun den C#-Controllercode an, der die Empfehlungen tatsächlich über das .NET SDK von Azure Search abruft.

1. Öffnen Sie die Datei „HomeController.cs“ im Verzeichnis „Controllers“. 

1. Zunächst bemerken Sie möglicherweise im oberen Bereich der Klasse eine Methode mit dem Namen InitSearch.  Mit dieser Methode wird für den Azure Search-Dienst ein authentifizierter Client für den HTTP-Index erstellt.  Weitere Informationen zur Funktionsweise finden Sie im folgenden Tutorial: [Verwenden von Azure Search aus einer .NET-Anwendung](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

1. Rufen Sie die Funktion „Suggest“ (Vorschlagen) auf.

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

Die Funktion „Suggest“ (Vorschlagen) verwendet zwei Parameter, die bestimmen, ob Treffermarkierungen zurückgegeben werden sollen oder ob zusätzlich zur Eingabe des Suchbegriffs die Fuzzyübereinstimmung verwendet werden soll.  Die Methode erstellt das Objekt „SuggestParameters“, das anschließend an die Suggest-API übergeben wird. Das Ergebnis wird anschließend in JSON konvertiert, damit es im Client angezeigt werden kann.
(Optional) Fügen Sie am Anfang der Funktion „Suggest“ (Vorschlagen) einen Breakpoint hinzu und durchlaufen Sie den Code schrittweise.  Beachten Sie die vom SDK zurückgegebene Antwort und wie diese in das von der Methode zurückgegebene Ergebnis konvertiert wird.

Die anderen Beispiele auf der Seite folgen bei dem Hinzufügen von Treffermarkierungen, der Textvervollständigung bei Empfehlungen zur automatischen Vervollständigung und bei Facets zur Unterstützung des clientseitigen Zwischenspeicherns der Ergebnisse der automatischen Vervollständigung dem gleichen Muster.  Überprüfen Sie alle Beispiele, um zu verstehen, wie sie funktionieren und wie Sie diese in Ihrer Suchumgebung nutzen können.

### <a name="javascript-language-example"></a>Beispiel für JavaScript-Sprachen

Im Beispiel für JavaScript-Sprachen nutzt der JavaScript-Code auf der Seite „IndexJavaScript.cshtml“ die Bibliothek jQuery UI zur automatischen Vervollständigung.  Dies ist eine Bibliothek, deren Hauptaufgabe in der Darstellung eines ansehnlichen Suchfelds besteht und darin, die Durchführung asynchroner Aufrufe in Azure Search zum Abrufen von Empfehlungen zu erleichtern.  Schauen Sie sich den JavaScript-Code im ersten Beispiel an:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: function (request, response) {
        $.ajax({
            type: "POST",
            url: suggestUri,
            dataType: "json",
            headers: {
                "api-key": searchServiceApiKey,
                "Content-Type": "application/json"
            },
            data: JSON.stringify({
                top: 5,
                fuzzy: false,
                suggesterName: "sg",
                search: request.term
            }),
                success: function (data) {
                    if (data.value && data.value.length > 0) {
                        response(data.value.map(x => x["@@search.text"]));
                    }
                }
            });
        },
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Wenn Sie diesen mit dem Beispiel oben vergleichen, in dem der Home-Controller aufgerufen wird, werden Sie einige Ähnlichkeiten feststellen.  Die Konfiguration für die automatische Vervollständigung bei `minLength` und `position` ist identisch.  Die wesentliche Änderung ist in der Quelle zu finden.  Statt die Suggest-Methode im Home-Controller aufzurufen, wird in einer JavaScript-Funktion eine REST-Anforderung erstellt und mit AJAX ausgeführt.  Anschließend wird die Antwort als „Erfolgreich“ verarbeitet und als Quelle verwendet.

## <a name="takeaways"></a>Wesentliche Punkte

Dieses Tutorial veranschaulicht die grundlegenden Schritte zum Erstellen eines Suchfelds, das die automatische Vervollständigung und Vorschläge unterstützt.  Sie haben gesehen, wie Sie eine ASP.NET MVC-Anwendung erstellen und mit dem .NET SDK oder der REST-API für Azure Search Vorschläge abrufen können.

## <a name="next-steps"></a>Nächste Schritte

Integrieren Sie Vorschläge und die automatische Vervollständigung in Ihre Suchumgebung.  Bedenken Sie, wie die direkte Verwendung des .Net SDK oder der REST-API dabei helfen kann, dass Benutzer zur Steigerung ihrer Produktivität bei der Eingabe von der Leistungsfähigkeit von Azure Search profitieren können.

> [!div class="nextstepaction"]
> [REST-API zur automatischen Vervollständigung](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [REST-API für Vorschlage](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Facets-Indexattribut in einer REST-API zur Indexerstellung](https://docs.microsoft.com/rest/api/searchservice/create-index)

