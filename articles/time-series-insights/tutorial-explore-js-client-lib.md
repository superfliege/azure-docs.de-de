---
title: 'Tutorial: Erkunden der Azure Time Series Insights-JavaScript-Clientbibliothek | Microsoft-Dokumentation'
description: Informieren Sie sich über die Azure Time Series Insights-JavaScript-Clientbibliothek und das zugehörige Programmiermodell.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: f231fa7624a2babea2a3d91076ad0348b3c9e976
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540373"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Tutorial: Erkunden der Azure Time Series Insights-JavaScript-Clientbibliothek

Die JavaScript D3-basierte TSI-Clientbibliothek wurde entwickelt, um Webentwickler beim Abfragen und Visualisieren von in Time Series Insights (TSI) gespeicherten Daten zu unterstützen.  In diesem Tutorial erkunden Sie die TSI-Clientbibliothek und das zugehörige Programmiermodell anhand einer Beispielwebanwendung.

In den Themen dieses Tutorials können Sie mit der Bibliothek experimentieren, um sich mit dem Zugriff auf TSI-Daten und der Verwendung von Diagrammsteuerelementen zum Rendern und Visualisieren von Daten vertraut zu machen. Ziel des Tutorials ist es, Ihnen genügend Informationen bereitzustellen, damit Sie die Bibliothek in Ihrer eigenen Webanwendung verwenden können.

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
> * TSI-Beispielanwendung
> * TSI-JavaScript-Clientbibliothek
> * Verwendung der Bibliothek durch die Beispielanwendung zum Visualisieren von TSI-Daten

## <a name="video"></a>Video: 

### <a name="in-this-video-we-introduce-the-open-source-time-series-insights-javascript-sdkbr"></a>In diesem Video stellen wir das Open Source-JavaScript-SDK für Time Series Insights vor.</br>

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird das Feature „Entwicklertools“ (auch als „DevTools“ oder „F12“ bezeichnet) verwendet, das Sie in den meisten modernen Webbrowsern finden, z.B. [Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [Firefox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/) und anderen. Wenn Sie mit diesem Feature noch nicht vertraut sind, empfiehlt es sich, es zunächst in Ihrem Browser zu erkunden, bevor Sie mit dem Tutorial fortfahren.

## <a name="time-series-insights-sample-application"></a>Time Series Insights-Beispielanwendung

In diesem Tutorial wird die Time Series Insights-Beispielanwendung verwendet, um den Quellcode der Anwendung einschließlich der Verwendung der TSI-JavaScript-Clientbibliothek zu untersuchen. Das Beispiel umfasst eine Single-Page-Webanwendung (SPA), mit der die Verwendung der Bibliothek demonstriert wird. Im Beispiel wird veranschaulicht, wie Sie Daten aus einer TSI-Beispielumgebung abfragen und visualisieren.

1. Navigieren Sie zur [Time Series Insights-Beispielanwendung](https://insights.timeseries.azure.com/clientsample). Es wird eine Seite mit einer Aufforderung zur Anmeldung wie in der folgenden Abbildung angezeigt:

   ![TSI-Client – Beispiel für Aufforderung zur Anmeldung](media/tutorial-explore-js-client-lib/tcs-sign-in.png)

2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein (bzw. wählen Sie sie aus). Verwenden Sie ein Unternehmens- oder Organisationskonto (Azure Active Directory) oder ein persönliches Konto (Microsoft-Konto oder MSA).

   ![TSI-Client – Beispiel für Eingabeaufforderung für Anmeldeinformationen](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)

3. Nach der erfolgreichen Anmeldung wird eine Seite wie in der folgenden Abbildung angezeigt. Die Seite enthält mehrere Arten von Beispieldiagrammen, die mit TSI-Daten aufgefüllt wurden. Ihr Benutzerkonto und die Option **Abmelden** werden oben rechts angezeigt:

   ![TSI-Client – Beispiel für Hauptseite nach der Anmeldung](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)

### <a name="page-source-and-structure"></a>Quelle und Struktur der Seite

Zuerst sehen wir uns den HTML- und JavaScript-Quellcode im Hintergrund der Seite an, die in Ihrem Browser gerendert wurde. Wir werden nicht alle Elemente im Einzelnen erläutern, aber Sie erhalten genügend Informationen zu den wichtigsten Abschnitten, damit Sie ein Gefühl dafür bekommen, wie die Seite funktioniert:

1. Öffnen Sie die **Entwicklertools** in Ihrem Browser. Sehen Sie sich die HTML-Elemente an, aus denen die aktuelle Seite besteht (auch als HTML- oder DOM-Struktur bezeichnet).

2. Erweitern Sie die Elemente `<head>` und `<body>`, und beachten Sie die folgenden Abschnitte:

   - Unter dem `<head>`-Element sehen Sie Elemente, mit denen zusätzliche Dateien abgerufen werden, um die Funktionsweise der Seite zu unterstützen:
     - Ein `<script>`-Element, das zum Verweisen auf die Azure Active Directory Authentication Library **adal.min.js** (auch kurz als ADAL bezeichnet) verwendet wird. ADAL ist eine JavaScript-Bibliothek, über die die OAuth 2.0-Authentifizierung (Anmeldung) und die Tokenbeschaffung für den Zugriff auf APIs bereitgestellt werden.
     - Mehrere `<link>`-Elemente für Stylesheets (auch als CSS bezeichnet), z.B. **sampleStyles.css** und **tsiclient.css**. Die Stylesheets werden verwendet, um visuelle Formatierungsdetails für Seiten zu steuern, z.B. Farben, Schriftarten, Abstände usw.
     - Ein `<script>`-Element, das zum Verweisen auf die TSI-JavaScript-Clientbibliothek **tsiclient.js** verwendet wird. Die Bibliothek wird von der Seite verwendet, um TSI-Dienst-APIs aufzurufen und Diagrammsteuerelemente auf der Seite zu rendern.

     >[!NOTE]
     > Der Quellcode für die JavaScript-ADAL steht im Repository [azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js) zur Verfügung.
     > Der Quellcode für die TSI-JavaScript-Clientbibliothek ist im Repository [tsiclient](https://github.com/Microsoft/tsiclient) verfügbar.

   - Unter dem `<body>`-Element finden Sie `<div>`-Elemente, die als Container fungieren, um das Layout von Elementen auf der Seite zu definieren, sowie ein weiteres `<script>`-Element:
     - Mit dem ersten `<div>`-Element wird das Dialogfeld **Anmelden** (`id="loginModal"`) angegeben.
     - Das zweite `<div>`-Element fungiert als übergeordnetes Element für:
       - Ein `<div>`-Headerelement, das oben auf der Seite (`class="header"`) für Statusmeldungen und Anmeldeinformationen verwendet wird.
       - Ein `<div>`-Element für die restlichen Elemente des Hauptteils der Seite, einschließlich aller Diagramme (`class="chartsWrapper"`).
       - Ein `<script>`-Abschnitt, der den gesamten JavaScript-Code für die Steuerung der Seite enthält.

   [![TSI-Client – Beispiel mit Entwicklertools](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

3. Wenn Sie das `<div class="chartsWrapper">`-Element erweitern, werden noch andere untergeordnete `<div>`-Elemente angezeigt. Diese Elemente werden verwendet, um die einzelnen Beispiele für Diagrammsteuerelemente zu positionieren. Beachten Sie, dass verschiedene `<div>`-Elementpaare vorhanden sind – eines für jedes Diagrammbeispiel:

   - Das erste Element (`class="rowOfCardsTitle"`) enthält einen Kurztitel, der beschreibt, was die Diagramme veranschaulichen. Beispiel:  „Statische Liniendiagramme mit Legenden in voller Größe“
   - Das zweite Element (`class="rowOfCards"`) ist ein übergeordnetes Element, das weitere untergeordnete `<div>`-Elemente enthält, mit denen die eigentlichen Diagrammsteuerelemente in einer Zeile positioniert werden.

   ![Body-div-Elemente](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)

4. Erweitern Sie jetzt das `<script type="text/javascript">`-Element direkt unter dem `<div class="chartsWrapper">`-Element. Beachten Sie den Anfang des JavaScript-Abschnitts auf Seitenebene, der verwendet wird, um die gesamte Seitenlogik zu verarbeiten: Authentifizierung, Aufrufen von TSI-Dienst-APIs, Rendern der Diagrammsteuerelemente und vieles mehr:

   ![Body-Skript](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)

## <a name="tsi-javascript-client-library-concepts"></a>Konzepte der TSI-JavaScript-Clientbibliothek

Die TSI-Clientbibliothek **tsclient.js** stellt eine Abstraktion für zwei wichtige Kategorien bereit, auf die hier nicht näher eingegangen wird:

- **Wrappermethoden für den Aufruf der TSI-Abfrage-APIs**: REST-APIs, mit denen Sie mithilfe von Aggregatausdrücken TSI-Daten abrufen können. Die Methoden sind unter dem `TsiClient.Server`-Namespace der Bibliothek organisiert.
- **Methoden zum Erstellen und Auffüllen mehrerer Typen von Diagrammsteuerelementen**: Methoden, die zum Rendern der aggregierten TSI-Daten auf einer Webseite verwendet werden. Die Methoden sind unter dem `TsiClient.UX`-Namespace der Bibliothek organisiert.

Die folgenden Konzepte sind universell und gelten ganz allgemein für die TSI-Clientbibliotheks-APIs.

### <a name="authentication"></a>Authentifizierung

Wie bereits erwähnt, handelt es sich bei diesem Beispiel um eine Single-Page-Anwendung (SPA), die zur Benutzerauthentifizierung die OAuth 2.0-Unterstützung in der ADAL verwendet. In diesem Abschnitt des Skripts finden sich einige interessante Punkte:

1. Bei Verwendung von ADAL für die Authentifizierung muss sich die Clientanwendung selbst in der Azure Active Directory-Anwendungsregistrierung registrieren. Da es sich um eine SPA handelt, wird diese Anwendung für die Verwendung des „impliziten“ OAuth 2.0-Autorisierungsflows registriert. Entsprechend gibt die Anwendung zur Laufzeit einige der Registrierungseigenschaften für die Flowverwendung an, wie z.B. die Client-ID-GUID (`clientId`) und den Umleitungs-URI (`postLogoutRedirectUri`).

2. Später fordert die Anwendung ein „Zugriffstoken“ von Azure AD an. Das Zugriffstoken wird für einen begrenzten Satz an Berechtigungen für einen bestimmten Dienst- oder API-Bezeichner ausgestellt (https://api.timeseries.azure.com. Der Dienst- oder API-Bezeichner wird auch als „Tokenzielgruppe“ bezeichnet. Die Tokenberechtigungen werden im Namen des angemeldeten Benutzers ausgegeben. Der Bezeichner für den Dienst bzw. die API ist eine weitere Eigenschaft, die in der Azure AD-Registrierung der Anwendung enthalten ist. Nachdem die ADAL das Zugriffstoken an die Anwendung zurückgegeben hat, wird es beim Zugriff auf die TSI-Dienst-APIs als „Bearertoken“ übergeben.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=145-204&highlight=4-9,36-39)]

### <a name="control-identification"></a>Identifizierung der Steuerelemente

Wie bereits erläutert, stellen die `<div>`-Elemente im `<body>`-Element das Layout für sämtliche Diagrammsteuerelemente bereit, die auf der Seite gezeigt werden. Jedes `<div>`-Element gibt Eigenschaften für die Positionierung und visuellen Attribute eines Diagrammsteuerelements an, einschließlich einer `id`-Eigenschaft. Die `id`-Eigenschaft gibt einen eindeutigen Bezeichner an. Dieser wird im JavaScript-Code verwendet, um beim Rendern und Aktualisieren für eine Identifizierung und Bindung aller Steuerelemente zu sorgen.

### <a name="aggregate-expressions"></a>Aggregatausdrücke

Die TSI-Clientbibliothek-APIs macht ausführlich Gebrauch von Aggregatausdrücken. Ein Aggregatausdruck bietet die Möglichkeit, einen oder mehrere „Suchbegriffe“ zu erstellen. Die APIs sind ähnlich wie der [Time Series Insights-Explorer](https://insights.timeseries.azure.com/demo) aufgebaut, in dem Suchzeitspanne, where-Prädikate, Measures und splitBy-Werte verwendet werden. Die meisten Bibliotheks-APIs akzeptieren ein Array aus Aggregatausdrücken, die vom Dienst zum Erstellen einer TSI-Datenabfrage verwendet werden können.

### <a name="call-pattern"></a>Aufrufmuster

Das Auffüllen und Rendern von Diagrammsteuerelementen folgt einem allgemeinen Muster. Sie finden dieses Muster im gesamten JavaScript-Code für die Seite, der die Steuerelemente für die TSI-Beispielanwendung instanziiert und lädt:

1. Deklarieren Sie ein `array`, das mindestens einen TSI-Aggregatausdruck enthalten soll:

   ```javascript
   var aes =  [];
   ```

2. Erstellen Sie 1 bis n Aggregatausdrucksobjekte, und fügen Sie diese dem Aggregatausdrucksarray hinzu:

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
   | `contextMenuActions` | Ein Array aus Aktionen, die an die Zeitreihenobjekte in einer Visualisierung gebunden werden sollen (optional). | Weitere Informationen finden Sie unter [„Popupkontextmenüs“ im Abschnitt „Erweiterte Funktionen“](#popup-context-menus). |

3. Rufen Sie mithilfe von `TsiClient.Server`-APIs eine TSI-Abfrage auf, um die Aggregatdaten anzufordern:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates-Parameter**

   | Parameter | BESCHREIBUNG | Beispiel |
   | --------- | ----------- | ------- |
   | `token`     | Das Zugriffstoken für die TSI-API. |  `authContext.getTsiToken()` Weitere Informationen finden Sie im Abschnitt [Authentifizierung](#authentication). |
   | `envFQDN`   | Der vollständig qualifizierte Domänenname (FQDN) für die TSI-Umgebung. | Im Azure-Portal, z.B. `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Ein Array mit TSI-Abfrageausdrücken. | Verwenden Sie die `aes`-Variable, wie oben beschrieben: `aes.map(function(ae){return ae.toTsx()}`. |

4. Transformieren Sie das von der TSI-Abfrage zurückgegebene komprimierte Ergebnis zur Visualisierung in JSON:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

5. Erstellen Sie mithilfe der `TsiClient.UX`-APIs ein Diagrammsteuerelement, und binden Sie es an eines der `<div>`-Elemente auf der Seite:

   ```javascript
   var lineChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

6. Füllen Sie das Diagrammsteuerelement mit den transformierten JSON-Datenobjekten auf, und rendern Sie das Steuerelement auf der Seite:

   ```javascript
   lineChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Rendern von Steuerelementen

Über die TSI-Clientbibliothek werden derzeit acht eindeutige Analysesteuerelemente verfügbar gemacht: Liniendiagramm, Kreisdiagramm, Balkendiagramm, Wärmebild, Hierarchiesteuerelemente, Raster mit Zugriffsmöglichkeit, diskrete Ereigniszeitreihen und Zustandsübergangs-Zeitreihen.

### <a name="line-bar-pie-chart-examples"></a>Beispiele für Linien-, Balken- und Kreisdiagramme

Sehen Sie sich den Code hinter einigen standardmäßigen Diagrammsteuerelementen an, die in der Anwendung veranschaulicht werden, sowie das Programmiermodell bzw. die Programmiermuster für die Erstellung der Steuerelemente. Achten Sie besonders auf den HTML-Abschnitt unter dem Kommentar `// Example 3/4/5`, der die Steuerelemente mit den ID-Werten `chart3`, `chart4` und `chart5` rendert.

Sie erinnern sich sicher aus Schritt 3 im Abschnitt [Quelle und Struktur der Seite](#page-source-and-structure) daran, dass Diagrammsteuerelemente in Zeilen auf der Seite angeordnet sind und jede dieser Zeilen über eine Zeile mit einem beschreibenden Titel verfügt. In diesem Beispiel werden die drei Diagramme unter dem `<div>`-Element des Titels „Multiple Chart Types From the Same Data“ aufgefüllt und sind an die drei `<div>`-Elemente unterhalb des Titels gebunden:

[!code-javascript[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

Im folgenden Abschnitt des JavaScript-Codes werden die oben beschriebenen Muster verwendet: Erstellen von TSI-Aggregatausdrücken, Verwenden dieser Ausdrücke zum Abfragen von TSI-Daten und Rendern der drei Diagramme. Beachten Sie die drei Typen aus dem Namespace `tsiClient.ux`: `LineChart`, `BarChart` und `PieChart`. Hiermit werden die entsprechenden Diagramme erstellt und gerendert. Beachten Sie auch, dass alle drei Diagramme das gleiche `transformedResult` für Aggregatausdrucksdaten verwenden können:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Die drei Diagramme sehen nach dem Rendern folgendermaßen aus:

[![Multiple chart types from the same data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Erweiterte Funktionen

Über die TSI-Clientbibliothek werden auch einige optionale erweiterte Funktionen verfügbar gemacht, die Sie nutzen können.

### <a name="states-and-events"></a>Zustände und Ereignisse

Ein Beispiel für die erweiterte Funktionalität ist die Möglichkeit, Diagrammen Zustandsübergänge und diskrete Ereignisse hinzuzufügen. Dieses Feature ist nützlich zum Hervorheben von Incidents, für Warnungen sowie für Zustandsschalter wie „Ein“ und „Aus“.

Sehen Sie sich den Code im Hintergrund des HTML-Abschnitts unter dem Kommentar `// Example 10` an. Der Code rendert unter dem Titel „Line Charts with Multiple Series Types“ ein Liniendiagramm-Steuerelement und bindet es an das `<div>`-Element mit dem ID-Wert `chart10`.

1. Zuerst wird eine Struktur mit dem Namen `events4` definiert, in der die Zustandswechselelemente enthalten sind, die nachverfolgt werden sollen. Die Struktur enthält Folgendes:

   - Einen Zeichenfolgenschlüssel mit dem Namen `Component States`.
   - Ein Array mit Wertobjekten, die die Zustände darstellen. Jedes Objekt enthält Folgendes:
     - Einen Zeichenfolgenschlüssel mit einem JavaScript-ISO-Zeitstempel.
     - Ein Array mit den Zustandsmerkmalen: eine Farbe und eine Beschreibung.

2. Als Nächstes wird die `events5`-Struktur für „Incidents“ definiert, das ein Array der nachzuverfolgenden Ereigniselemente enthält. Die Arraystruktur weist die gleiche Form wie die Struktur auf, die für `events4` angegeben ist.

3. Schließlich wird das Liniendiagramm gerendert, und die beiden Strukturen mit den Optionsparametern für das Diagramm werden übergeben: `events:` und `states:`. Beachten Sie die weiteren Optionsparameter zum Angeben eines `tooltip:`-, `theme:`- oder `grid:`-Elements.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Die rautenförmigen Marker/Popupfenster werden verwendet, um visuell auf Incidents hinzuweisen, und die farbigen Balken/Popupfenster entlang der Zeitskala weisen visuell auf Zustandsänderungen hin:

[![Line Charts with Multiple Series Types](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Popupkontextmenüs

Ein weiteres Beispiel für die erweiterte Funktionalität sind benutzerdefinierte Kontextmenüs (per Rechtsklick aufrufbare Popupmenüs). Benutzerdefinierte Kontextmenüs eignen sich gut zum Ermöglichen von Aktionen und logischen nächsten Schritten innerhalb des Bereichs Ihrer Anwendung.

Sehen Sie sich den Code im Hintergrund des HTML-Abschnitts unter dem Kommentar `// Example 13/14/15` an. Mit diesem Code wird zunächst ein Liniendiagramm unter dem Titel „Line Chart with Context Menu to Create Pie/Bar Chart“ gerendert, und das Diagramm wird an das `<div>`-Element mit dem ID-Wert `chart13` gebunden. Mithilfe von Kontextmenüs stellt das Liniendiagramm die Möglichkeit bereit, dynamisch ein Kreis- und ein Balkendiagramm zu erstellen, die an `<div>`-Elemente mit den IDs `chart14` und `chart15` gebunden sind. Darüber hinaus verwenden sowohl das Kreis- als auch das Balkendiagramm Kontextmenüs, um jeweils eigene Funktionen zu aktivieren: die Möglichkeit, Daten aus dem Kreis- in das Balkendiagramm zu kopieren, und die Möglichkeit, Balkendiagrammdaten an das Browserkonsolenfenster auszugeben.

1. Zuerst wird eine Reihe von benutzerdefinierten Aktionen definiert. Jede Aktion enthält ein Array mit einem oder mehreren Elementen. Mit jedem Element wird ein einzelnes Kontextmenüelement definiert:

   - `barChartActions`: Mit dieser Aktion wird das Kontextmenü für das Kreisdiagramm definiert, das ein Element zum Definieren eines einzelnen Elements enthält:
     - `name`: Der Text, der für das Menüelement verwendet wird: "Parameter an der Konsole ausgeben".
     - `action`: Die Aktion, die dem Menüelement zugeordnet ist. Die Aktion ist immer eine anonyme Funktion, für die drei Argumente basierend auf dem Aggregatausdruck verwendet werden, mit dem das Diagramm erstellt wird. In diesem Fall werden die Argumente in das Browserkonsolenfenster geschrieben:
       - `ae`: Das Array mit den Aggregatausdrücken.
       - `splitBy`: Der splitBy-Wert.
       - `timestamp`: Der Zeitstempel.

   - `pieChartActions`: Diese Aktion definiert das Kontextmenü für das Balkendiagramm, das ein Element enthält, um ein einzelnes Element zu definieren. Form und Schema entsprechen dem obigen `barChartActions`-Element. Beachten Sie aber, dass die `action`-Funktion sich erheblich unterscheidet, da sie das Balkendiagramm instanziiert und rendert. Beachten Sie auch, dass das `ae`-Argument verwendet wird, um das Array mit dem Aggregatausdruck anzugeben, das zur Laufzeit beim Öffnen des Menüelements übergeben wird. Die Funktion legt auch die `ae.contextMenu`-Eigenschaft mit dem `barChartActions`-Kontextmenü fest.
   - `contextMenuActions`: Mit dieser Aktion wird das Kontextmenü für das Liniendiagramm definiert, das drei Elemente enthält, um drei Menüelemente zu definieren. Die Form und das Schema für die einzelnen Elemente entsprechen der Form und dem Schema der oben beschriebenen Elemente. Genauso wie beim `barChartActions`-Element schreibt das erste Element die drei Funktionsargumente in das Browserkonsolenfenster. Ebenso wie beim `pieChartActions`-Element instanziieren und rendern die letzten beiden Elemente das Kreis- bzw. Balkendiagramm. Die letzten beiden Elemente legen auch ihre `ae.contextMenu`-Eigenschaften mit den Kontextmenüs `pieChartActions` bzw. `barChartActions` fest.

2. Als Nächstes werden zwei Aggregatausdrücke per Pushvorgang an das `aes`-Array für Aggregatausdrücke übertragen, und das `contextMenuActions`-Array wird für jeden Ausdruck angegeben. Diese Ausdrücke werden mit dem Liniendiagramm-Steuerelement verwendet.

3. Zunächst wird nur das Liniendiagramm gerendert. Aus diesem können zur Laufzeit sowohl das Kreis- als auch das Balkendiagramm gerendert werden.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

Im Screenshot sind die Diagramme mit ihren jeweiligen Popupkontextmenüs dargestellt. Das Kreis- und das Balkendiagramm wurden mithilfe der Kontextmenüoptionen des Liniendiagramms dynamisch erstellt.

[![Line Chart with Context Menu to Create Pie/Bar Chart](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Pinsel

Pinsel werden zum Angeben eines Zeitbereichs verwendet, um Aktionen wie das Zoomen und Untersuchen zu definieren.

Der Code, der zum Veranschaulichen von Pinseln verwendet wird, wurde im vorherigen Beispiel „Line Chart with Context Menu to Create Pie/Bar Chart“ gezeigt, in dem [Popupkontextmenüs](#popup-context-menus-section) beschrieben wurden.

1. Pinselaktionen ähneln einem Kontextmenü darin, dass sie eine Reihe von benutzerdefinierten Aktionen für den Pinsel definieren. Jede Aktion enthält ein Array mit einem oder mehreren Elementen. Mit jedem Element wird ein einzelnes Kontextmenüelement definiert:
   - `name`: Der Text, der für das Menüelement verwendet wird: "Parameter an der Konsole ausgeben".
   - `action`: Die Aktion, die mit dem Menüelement verknüpft ist. Hierbei handelt es sich immer um eine anonyme Funktion, für die zwei Argumente verwendet werden. In diesem Fall werden die Argumente in das Browserkonsolenfenster geschrieben:
      - `fromTime`: Der from-Zeitstempel der Pinselauswahl.
      - `toTime`: Der to-Zeitstempel der Pinselauswahl.

2. Pinselaktionen werden als weitere Diagrammoptionseigenschaft hinzugefügt. Beachten Sie die `brushContextMenuActions: brushActions`-Eigenschaft, die an den `linechart.Render`-Aufruf übergeben wird.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

![Liniendiagramm mit Kontextmenü zum Erstellen von Kreis-/Balkendiagrammen mit Pinseln](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anmelden bei der TSI-Beispielanwendung und Erkunden der Anwendung und ihrer Quellen
> * Verwenden von APIs in der TSI-JavaScript-Clientbibliothek
> * Verwenden von JavaScript zum Erstellen und Auffüllen von Diagrammsteuerelementen mit TSI-Daten

Wie bereits erläutert, verwendet die TSI-Beispielanwendung ein Demonstrationsdataset. Informationen dazu, wie Sie Ihre eigene TSI-Umgebung und Ihr eigenes Dataset erstellen, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen einer Azure Time Series Insights-Umgebung](tutorial-create-populate-tsi-environment.md)


