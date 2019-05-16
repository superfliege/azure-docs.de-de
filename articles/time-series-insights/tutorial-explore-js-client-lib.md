---
title: 'Tutorial: Erkunden der Azure Time Series Insights-JavaScript-Clientbibliothek | Microsoft-Dokumentation'
description: Informieren Sie sich über die Azure Time Series Insights-JavaScript-Clientbibliothek und das zugehörige Programmiermodell.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 8cb1d06872f7eae04bac934220da9d58982d0f4b
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233736"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Tutorial: Erkunden der Azure Time Series Insights-JavaScript-Clientbibliothek

Die JavaScript D3-basierte TSI-Clientbibliothek wurde entwickelt, um Webentwickler beim Abfragen und Visualisieren von in Time Series Insights (TSI) gespeicherten Daten zu unterstützen. Dieses Tutorial führt Sie anhand einer gehosteten Beispielanwendung durch die TSI-Clientbibliothek und das Programmiermodell.

Das Tutorial beschreibt das Arbeiten mit der Bibliothek, den Zugriff auf TSI-Daten und die Verwendung von Diagrammsteuerelementen zum Rendern und Visualisieren von Daten. Sie lernen auch, mit verschiedenen Arten von Diagrammen zu experimentieren, um Daten zu visualisieren. Am Ende des Tutorials können Sie die Clientbibliothek zum Integrieren von TSI-Funktionen in Ihre eigene Web-App verwenden.

Insbesondere erfahren Sie mehr zu:

> [!div class="checklist"]
> * TSI-Beispielanwendung
> * TSI-JavaScript-Clientbibliothek
> * Verwendung der Bibliothek durch die Beispielanwendung zum Visualisieren von TSI-Daten

> [!NOTE]
> * Das Tutorial verwendet eine kostenlose, gehostete [Time Series Insights-Webdemo](https://insights.timeseries.azure.com/clientsample).
> * Die Quelldateien der Time Series Insights-Beispiel-App finden Sie im [GitHub-Beispielrepository](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Lesen Sie die [Referenzdokumentation zum TSI-Client](https://github.com/microsoft/tsiclient/blob/master/docs/API.md).

## <a name="video"></a>Video

### <a name="in-this-video-we-introduce-the-open-source-time-series-insights-javascript-sdkbr"></a>In diesem Video stellen wir das Open Source-JavaScript-SDK für Time Series Insights vor.</br>

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird das Feature **Entwicklertools** Ihres Browsers verwendet. In modernen Webbrowsern ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/) usw.) lässt sich die **Web Inspector-Ansicht** in der Regel über den Hotkey `F12` aufrufen. Andernfalls klicken Sie mit der rechten Maustaste auf eine Webseite und wählen dann **Element untersuchen** aus.

## <a name="time-series-insights-sample-application"></a>Time Series Insights-Beispielanwendung

In diesem gesamten Tutorial wird eine kostenlose, gehostete Time Series Insights-Beispiel-App verwendet, um den Quellcode der Anwendung und die TSI-JavaScript-Clientbibliothek zu erkunden. Dabei lernen Sie das Interagieren mit TSI in JavaScript und das Visualisieren von Daten in Diagrammen und Graphen.

1. Navigieren Sie zur [Time Series Insights-Beispielanwendung](https://insights.timeseries.azure.com/clientsample). Die folgende Anmeldeaufforderung wird angezeigt:

   [![TSI-Client – Beispiel für Aufforderung zur Anmeldung](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Wählen Sie **Anmelden** aus, um Ihre Anmeldeinformationen einzugeben oder auszuwählen. Verwenden Sie ein Unternehmenskonto (Azure Active Directory) oder ein persönliches Konto (Microsoft-Konto oder MSA).

   [![TSI-Client – Beispiel für Eingabeaufforderung für Anmeldeinformationen](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Nach der Anmeldung wird eine Seite mit verschiedenen Arten von Diagrammen angezeigt, die mit TSI-Daten ausgefüllt sind. Ihr Benutzerkonto und die Option **Abmelden** werden oben rechts angezeigt:

   [![TSI-Client – Beispiel für Hauptseite nach der Anmeldung](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Quelle und Struktur der Seite

Zuerst betrachten wir den [HTML- und JavaScript-Quellcode](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) der gerenderten Webseite:

1. Öffnen Sie die **Entwicklertools** in Ihrem Browser. Sehen Sie sich die HTML-Elemente an, aus denen die aktuelle Seite besteht (auch als HTML- oder DOM-Struktur bezeichnet).

1. Erweitern Sie die Elemente `<head>` und `<body>`, und beachten Sie die folgenden Abschnitte:

   * Unter dem Element `<head>` befinden sich Seitenmetadaten und Abhängigkeiten, die die Ausführung der App ermöglichen:
     * Ein `<script>`-Element, das zum Verweisen auf die Azure Active Directory Authentication Library **adal.min.js** (auch kurz als ADAL bezeichnet) verwendet wird. ADAL ist eine JavaScript-Bibliothek, über die die OAuth 2.0-Authentifizierung (Anmeldung) und die Tokenbeschaffung für den Zugriff auf APIs bereitgestellt werden.
     * Mehrere `<link>`-Elemente für Stylesheets (auch als CSS bezeichnet), z.B. **sampleStyles.css** und **tsiclient.css**. Die Stylesheets werden verwendet, um visuelle Formatierungsdetails für Seiten zu steuern, z.B. Farben, Schriftarten, Abstände usw.
     * Ein `<script>`-Element, das zum Verweisen auf die TSI-JavaScript-Clientbibliothek **tsiclient.js** verwendet wird. Die Bibliothek wird von der Seite verwendet, um TSI-Dienst-APIs aufzurufen und Diagrammsteuerelemente auf der Seite zu rendern.

     >[!NOTE]
     > * Der Quellcode für die JavaScript-ADAL steht im Repository [azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js) zur Verfügung.
     > * Der Quellcode für die TSI-JavaScript-Clientbibliothek ist im Repository [tsiclient](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial) verfügbar.

   * Unter dem `<body>`-Element finden Sie `<div>`-Elemente, die das Layout von Elementen auf der Seite definieren, sowie ein weiteres `<script>`-Element:
     * Mit dem ersten `<div>`-Element wird das Dialogfeld **Anmelden** (`id="loginModal"`) angegeben.
     * Das zweite `<div>`-Element fungiert als übergeordnetes Element für:
       * Ein `<div>`-Headerelement, das oben auf der Seite (`class="header"`) für Statusmeldungen und Anmeldeinformationen verwendet wird.
       * Ein `<div>`-Element für die restlichen Elemente des Hauptteils der Seite, einschließlich aller Diagramme (`class="chartsWrapper"`).
       * Ein `<script>`-Abschnitt, der den gesamten JavaScript-Code für die Steuerung der Seite enthält.

   [![TSI-Client – Beispiel mit Entwicklertools](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Wenn Sie das `<div class="chartsWrapper">`-Element erweitern, werden weitere untergeordnete `<div>`-Elemente angezeigt. Diese Elemente werden verwendet, um die einzelnen Beispiele für Diagrammsteuerelemente zu positionieren. Beachten Sie, dass verschiedene `<div>`-Elementpaare vorhanden sind – eines für jedes Diagrammbeispiel:

   * Das erste Element (`class="rowOfCardsTitle"`) enthält einen Kurztitel, der beschreibt, was die Diagramme veranschaulichen. Beispiel: `Static Line Charts With Full-Size Legends.`
   * Das zweite Element (`class="rowOfCards"`) ist ein übergeordnetes Element, das weitere untergeordnete `<div>`-Elemente enthält, mit denen die eigentlichen Diagrammsteuerelemente in einer Zeile positioniert werden.

   [![Body-div-Elemente](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Erweitern Sie jetzt das `<script type="text/javascript">`-Element direkt unter dem `<div class="chartsWrapper">`-Element. Beachten Sie den Anfang des JavaScript-Abschnitts auf Seitenebene, der verwendet wird, um die gesamte Seitenlogik (Authentifizierung, Aufrufen von TSI-Dienst-APIs, Rendern der Diagrammsteuerelemente usw.) zu verarbeiten:

   [![Textskript](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="tsi-javascript-client-library-concepts"></a>Konzepte der TSI-JavaScript-Clientbibliothek

Die TSI-Clientbibliothek (**tsclient.js**) bietet Abstraktionen für zwei wichtige JavaScript-Funktionalitäten:

* **Wrappermethoden für den Aufruf der TSI-Abfrage-APIs:** REST-APIs, mit denen Sie mithilfe von Aggregatausdrücken TSI-Daten abrufen können. Die Methoden sind unter dem `TsiClient.Server`-Namespace der Bibliothek organisiert.

* **Methoden zum Erstellen und Auffüllen mehrerer Typen von Diagrammsteuerelementen**: Methoden, die zum Rendern der aggregierten TSI-Daten auf einer Webseite verwendet werden. Die Methoden sind unter dem `TsiClient.UX`-Namespace der Bibliothek organisiert.

Durch diese Vereinfachungen können Entwickler leichter Benutzeroberflächenkomponenten für Graphen und Diagramme mit Einbindung von TSI-Daten erstellen.

### <a name="authentication"></a>Authentication

Die [Time Series Insights-Beispielanwendung](https://insights.timeseries.azure.com/clientsample) ist eine Einzelseiten-App, welche die OAuth 2.0-Benutzerauthentifizierung von ADAL unterstützt:

1. Bei der Verwendung von ADAL zur Authentifizierung muss die Client-App in Azure Active Directory registriert sein. Tatsächlich ist die Einzelseiten-App für die Verwendung des [Ablaufs der impliziten OAuth 2.0-Gewährung](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow) registriert.
1. Daher muss die Anwendung einige der Registrierungseigenschaften zur Laufzeit angeben. Dazu gehören die Client-GUID (`clientId`) und der Umleitungs-URI (`postLogoutRedirectUri`).
1. Später fordert die App ein **Zugriffstoken** von Azure Active Directory an. Das Zugriffstoken wird für einen begrenzten Satz an Berechtigungen für einen bestimmten Dienst- oder API-Bezeichner ausgestellt (`https://api.timeseries.azure.com`). Die Tokenberechtigungen werden im Namen des angemeldeten Benutzers ausgegeben. Der Bezeichner für den Dienst bzw. die API ist eine weitere Eigenschaft, die in der Azure Active Directory-Registrierung der App enthalten ist.
1. Nachdem ADAL das Zugriffstoken an die App zurückgegeben hat, wird es beim Zugriff auf die TSI-Dienst-APIs als **Bearertoken** übergeben.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Weitere Informationen über von Microsoft unterstützte ADAL-Bibliotheken finden Sie in der [ADAL-Referenzdokumentation](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries).

### <a name="control-identification"></a>Identifizierung der Steuerelemente

Im vorliegenden Beispiel sind `<div>`-Elemente im übergeordneten `<body>`-Element so angeordnet, dass ein sinnvolles Layout für alle auf der Seite gerenderten Diagrammsteuerelemente erstellt wird.

Jedes `<div>`-Element gibt Eigenschaften für die Positionierung und visuellen Attribute von Diagrammsteuerelementen an. Die Eigenschaften des HTML-Elements `id` dienen als eindeutige Bezeichner, die an bestimmte Steuerelemente zum Rendern und Aktualisieren visualisierter Daten gebunden werden.

### <a name="aggregate-expressions"></a>Aggregatausdrücke

Die TSI-Clientbibliothek-APIs verwenden Aggregatausdrücke:

* Ein Aggregatausdruck bietet die Möglichkeit, einen oder mehrere **Suchbegriffe** zu erstellen.

* Die Client-APIs sind so konzipiert, dass sie eine ähnliche Funktionalität für eine andere Demo-App ([Time Series Insights-Explorer](https://insights.timeseries.azure.com/demo)) bereitstellen, die Suchzeitspannen, where-Prädikate, Measures und splitBy-Werte verwendet.

* Die meisten Clientbibliotheks-APIs akzeptieren ein Array aus Aggregatausdrücken, die vom Dienst zum Erstellen einer TSI-Datenabfrage verwendet werden können.

### <a name="call-pattern"></a>Aufrufmuster

Das Auffüllen und Rendern von Diagrammsteuerelementen folgt einem allgemeinen Muster. Dieses allgemeine Muster zieht sich durch die gesamte Beispiel-App und hilft Ihnen bei der Verwendung der Clientbibliothek:

1. Deklarieren Sie ein `array`, das mindestens einen TSI-Aggregatausdruck enthalten soll:

   ```javascript
   var aes =  [];
   ```

1. Erstellen Sie *1* bis *n* Aggregatausdrucksobjekte. Fügen Sie diese dann dem Aggregatausdrucksarray hinzu:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **aggregateExpression-Parameter**

   | Parameter | BESCHREIBUNG | Beispiel |
   | --------- | ----------- | ------- |
   | `predicateObject` | Der Datenfilterausdruck. |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | Der Eigenschaftenname des verwendeten Measures. | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | Die gewünschten Aggregationen der Measureeigenschaft. | `['avg', 'min']` |
   | `searchSpan`      | Die Dauer und Intervallgröße des Aggregatausdrucks. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | Die Zeichenfolgeneigenschaft, nach der die Aufteilung erfolgen soll (optional – darf nicht NULL sein). | `{property: 'Station', type: 'String'}` |
   | `color`         | Die Farbe der Objekte, die Sie rendern möchten. | `'pink'` |
   | `alias`           | Ein Anzeigename für den Aggregatausdruck. | `'Factory3Temperature'` |
   | `contextMenuActions` | Ein Array aus Aktionen, die an die Zeitreihenobjekte in einer Visualisierung gebunden werden sollen (optional). | Weitere Informationen finden Sie im Abschnitt [Popupkontextmenüs](#pop-up-context-menus). |

1. Rufen Sie mithilfe von `TsiClient.Server`-APIs eine TSI-Abfrage auf, um die Aggregatdaten anzufordern:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates-Parameter**

   | Parameter | BESCHREIBUNG | Beispiel |
   | --------- | ----------- | ------- |
   | `token`     | Das Zugriffstoken für die TSI-API. |  `authContext.getTsiToken()` Weitere Informationen finden Sie im Abschnitt [Authentifizierung](#authentication). |
   | `envFQDN`   | Der vollständig qualifizierte Domänenname (FQDN) für die TSI-Umgebung. | Im Azure-Portal, z.B. `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Ein Array mit TSI-Abfrageausdrücken. | Verwenden Sie die `aes`-Variable, wie oben beschrieben: `aes.map(function(ae){return ae.toTsx()}`. |

1. Transformieren Sie das von der TSI-Abfrage zurückgegebene komprimierte Ergebnis zur Visualisierung in JSON:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. Erstellen Sie mithilfe der `TsiClient.UX`-APIs ein Diagrammsteuerelement, und binden Sie es an eines der `<div>`-Elemente auf der Seite:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Füllen Sie das Diagrammsteuerelement mit den transformierten JSON-Datenobjekten auf, und rendern Sie das Steuerelement auf der Seite:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Rendern von Steuerelementen

Die TSI-Clientbibliothek bietet acht eindeutige, sofort einsatzbereite Steuerelemente für die Analyse:

* **Liniendiagramm**
* **Kreisdiagramm**
* **Balkendiagramm**
* **heatmap**
* **Hierarchiesteuerelemente**
* **Raster mit Zugriffsmöglichkeit**
* **diskrete Ereigniszeitreihen**
* **Zustandsübergang-Zeitreihen**

### <a name="line-bar-pie-chart-examples"></a>Beispiele für Linien-, Balken- und Kreisdiagramme

Sehen Sie sich den Demo-Code an, mit dem einige der standardmäßigen Diagrammsteuerelemente gerendert werden. Beachten Sie das Programmiermodell und die Muster für die Erstellung dieser Steuerelemente. Achten Sie besonders auf den HTML-Abschnitt unter dem Kommentar`// Example 3/4/5`, der die Steuerelemente mit den HTML-`id`-Werten `chart3`, `chart4` und `chart5` rendert.

Sie erinnern sich sicher aus **Schritt 3** im Abschnitt [Quelle und Struktur der Seite](#page-source-and-structure) daran, dass Diagrammsteuerelemente in Zeilen auf der Seite angeordnet sind und jede dieser Zeilen über eine Zeile mit einem beschreibenden Titel verfügt. In diesem Beispiel werden die drei Diagramme unter dem `<div>`-Element des Titels `"Multiple Chart Types From the Same Data"`aufgefüllt und sind an die drei `<div>`-Elemente unterhalb des Titels gebunden:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

Im folgenden Abschnitt des JavaScript-Codes werden die oben beschriebenen Muster verwendet: Erstellen von TSI-Aggregatausdrücken, Verwenden dieser Ausdrücke zum Abfragen von TSI-Daten und Rendern der drei Diagramme. Beachten Sie die drei Typen aus dem Namespace `tsiClient.ux`: `LineChart`, `BarChart` und `PieChart`. Hiermit werden die entsprechenden Diagramme erstellt und gerendert. Beachten Sie auch, dass alle drei Diagramme das gleiche `transformedResult` für Aggregatausdrucksdaten verwenden können:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Die drei Diagramme sehen nach dem Rendern folgendermaßen aus:

[![Multiple chart types from the same data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Erweiterte Funktionen

Die TSI-Clientbibliothek verfügt über mehrere zusätzliche Funktionen, mit denen Sie Datenvisualisierungen kreativ implementieren können.

### <a name="states-and-events"></a>Zustände und Ereignisse

Eine erweiterte Funktionalität ist die Möglichkeit, Diagrammen Zustandsübergänge und diskrete Ereignisse hinzuzufügen. Diese Funktion ist nützlich zum Hervorheben von Incidents, für Warnungen sowie für das Erstellen von Zustandsschaltern (z. B. Ein/Aus).

Sehen Sie sich den Code um den Kommentar `// Example 10` an. Der Code rendert unter dem Titel `"Line Charts with Multiple Series Types"` ein Liniendiagramm-Steuerelement und bindet es an das `<div>`-Element mit dem HTML-`id`-Wert `chart10`.

1. Zuerst wird eine Struktur mit dem Namen `events4` definiert, in der die Zustandswechselelemente enthalten sind, die nachverfolgt werden sollen. Die Struktur enthält Folgendes:

   * Einen Zeichenfolgenschlüssel mit dem Namen `Component States`.
   * Ein Array mit Wertobjekten, die die Zustände darstellen. Jedes Objekt enthält Folgendes:
     * Einen Zeichenfolgenschlüssel mit einem JavaScript-ISO-Zeitstempel.
     * Ein Array mit den Zustandsmerkmalen: eine Farbe und eine Beschreibung.

2. Als nächstes wird die `events5`-Struktur für das `Incidents`-Element definiert, das ein Array der nachzuverfolgenden Ereigniselemente enthält. Die Arraystruktur weist die gleiche Form wie die Struktur auf, die für `events4` angegeben ist.

3. Schließlich wird das Liniendiagramm gerendert, und die beiden Strukturen mit den Optionsparametern für das Diagramm werden übergeben: `events:` und `states:`. Beachten Sie die weiteren Optionsparameter zum Angeben eines `tooltip:`-, `theme:`- oder `grid:`-Elements.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Die rautenförmigen Marker/Popupfenster werden verwendet, um visuell auf Incidents hinzuweisen, und die farbigen Balken/Popupfenster entlang der Zeitskala weisen visuell auf Zustandsänderungen hin:

[![Line Charts with Multiple Series Types](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Popupkontextmenüs

Eine weitere erweiterte Funktionalität ist die Möglichkeit zum Erstellen von benutzerdefinierten Kontextmenüs (per Rechtsklick aufrufbare Popupmenüs). Benutzerdefinierte Kontextmenüs eignen sich gut zum Ermöglichen von Aktionen und logischen nächsten Schritten innerhalb des Bereichs Ihrer Anwendung.

Sehen Sie sich den Code um den Kommentar `// Example 13/14/15` an. Mit diesem Code wird zunächst ein Liniendiagramm unter dem Titel `"Line Chart with Context Menu to Create Pie/Bar Chart"` gerendert, und das Diagramm wird an das `<div>`-Element mit dem HTML-`id`-Wert `chart13` gebunden.

Mithilfe von Kontextmenüs stellt das Liniendiagramm die Möglichkeit bereit, dynamisch ein Kreis- und ein Balkendiagramm zu erstellen, die an `<div>`-Elemente mit den IDs `chart14` und `chart15` gebunden sind. Darüber hinaus verwenden sowohl das Kreis- als auch das Balkendiagramm Kontextmenüs, um jeweils eigene Funktionen zu aktivieren: die Möglichkeit, Daten aus dem Kreis- in das Balkendiagramm zu kopieren, und die Möglichkeit, Balkendiagrammdaten an das Browserkonsolenfenster auszugeben.

1. Zuerst wird eine Reihe von benutzerdefinierten Aktionen definiert. Jede Aktion enthält ein Array mit einem oder mehreren Elementen. Mit jedem Element wird ein einzelnes Kontextmenüelement definiert:

   * `barChartActions`: Mit dieser Aktion wird das Kontextmenü für das Kreisdiagramm definiert, das ein Element zum Definieren eines einzelnen Elements enthält:
     * `name`: Der Text, der für das Menüelement verwendet wird: "Parameter an der Konsole ausgeben".
     * `action`: Die Aktion, die dem Menüelement zugeordnet ist. Die Aktion ist immer eine anonyme Funktion, für die drei Argumente basierend auf dem Aggregatausdruck verwendet werden, mit dem das Diagramm erstellt wird. In diesem Fall werden die Argumente in das Browserkonsolenfenster geschrieben:
       * `ae`: Das Array mit den Aggregatausdrücken.
       * `splitBy`: Der `splitBy`-Wert.
       * `timestamp`: Der Zeitstempel.

   * `pieChartActions`: Diese Aktion definiert das Kontextmenü für das Balkendiagramm, das ein Element enthält, um ein einzelnes Element zu definieren. Form und Schema entsprechen dem obigen `barChartActions`-Element. Beachten Sie aber, dass die `action`-Funktion sich erheblich unterscheidet, da sie das Balkendiagramm instanziiert und rendert. Beachten Sie auch, dass das `ae`-Argument verwendet wird, um das Array mit dem Aggregatausdruck anzugeben, das zur Laufzeit beim Öffnen des Menüelements übergeben wird. Die Funktion legt auch die `ae.contextMenu`-Eigenschaft mit dem `barChartActions`-Kontextmenü fest.
   * `contextMenuActions`: Mit dieser Aktion wird das Kontextmenü für das Liniendiagramm definiert, das drei Elemente enthält, um drei Menüelemente zu definieren. Die Form und das Schema für die einzelnen Elemente entsprechen der Form und dem Schema der oben beschriebenen Elemente. Genauso wie beim `barChartActions`-Element schreibt das erste Element die drei Funktionsargumente in das Browserkonsolenfenster. Ebenso wie beim `pieChartActions`-Element instanziieren und rendern die letzten beiden Elemente das Kreis- bzw. Balkendiagramm. Die letzten beiden Elemente legen auch ihre `ae.contextMenu`-Eigenschaften mit den Kontextmenüs `pieChartActions` bzw. `barChartActions` fest.

2. Als Nächstes werden zwei Aggregatausdrücke per Pushvorgang an das `aes`-Array für Aggregatausdrücke übertragen, und das `contextMenuActions`-Array wird für jeden Ausdruck angegeben. Diese Ausdrücke werden mit dem Liniendiagramm-Steuerelement verwendet.

3. Zunächst wird nur das Liniendiagramm gerendert. Aus diesem können zur Laufzeit sowohl das Kreis- als auch das Balkendiagramm gerendert werden.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

Im Screenshot sind die Diagramme mit ihren jeweiligen Popupkontextmenüs dargestellt. Das Kreis- und das Balkendiagramm wurden mithilfe der Kontextmenüoptionen des Liniendiagramms dynamisch erstellt.

[![Line Chart with Context Menu to Create Pie/Bar Chart](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Pinsel

Pinsel werden zum Angeben eines Zeitbereichs verwendet, um Aktionen wie das Zoomen und Untersuchen zu definieren.

Der Code, der zum Veranschaulichen von Pinseln verwendet wird, wurde im vorherigen Beispiel „Line Chart with Context Menu to Create Pie/Bar Chart“ gezeigt, in dem Popupkontextmenüs beschrieben wurden.

1. Pinselaktionen ähneln einem Kontextmenü darin, dass sie eine Reihe von benutzerdefinierten Aktionen für den Pinsel definieren. Jede Aktion enthält ein Array mit einem oder mehreren Elementen. Mit jedem Element wird ein einzelnes Kontextmenüelement definiert:
   * `name`: Der Text, der für das Menüelement verwendet wird: "Parameter an der Konsole ausgeben".
   * `action`: Die Aktion, die mit dem Menüelement verknüpft ist. Hierbei handelt es sich immer um eine anonyme Funktion, für die zwei Argumente verwendet werden. In diesem Fall werden die Argumente in das Browserkonsolenfenster geschrieben:
      * `fromTime`: Der `from`-Zeitstempel der Pinselauswahl.
      * `toTime`: Der `to`-Zeitstempel der Pinselauswahl.

2. Pinselaktionen werden als weitere Diagrammoptionseigenschaft hinzugefügt. Beachten Sie die `brushContextMenuActions: brushActions`-Eigenschaft, die an den `linechart.Render`-Aufruf übergeben wird.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Liniendiagramm mit Kontextmenü zum Erstellen von Kreis-/Balkendiagrammen mit Pinseln](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anmelden bei der TSI-Beispielanwendung und Erkunden der Anwendung und ihrer Quellen
> * Verwenden von APIs in der TSI-JavaScript-Clientbibliothek
> * Verwenden von JavaScript zum Erstellen und Auffüllen von Diagrammsteuerelementen mit TSI-Daten

Wie bereits erläutert, verwendet die TSI-Beispielanwendung ein Demonstrationsdataset. Informationen dazu, wie Sie Ihre eigene TSI-Umgebung und Ihr eigenes Dataset erstellen, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen einer Azure Time Series Insights-Umgebung](tutorial-create-populate-tsi-environment.md)

Oder schauen Sie sich die Quelldateien der TSI-Beispielanwendung an:

> [!div class="nextstepaction"]
> [TSI-Beispiel-App-Repository](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

Lesen Sie die Referenzdokumentation zur TSI-Client-API.

> [!div class="nextstepaction"]
> [TSI-API-Referenzdokumentation](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
