---
title: Erkunden der Time Series Insights-JavaScript-Clientbibliothek
description: Informieren Sie sich über die Time Series Insights-JavaScript-Clientbibliothek und das zugehörige Programmiermodell.
documentationcenter: ''
services: time-series-insights
author: BryanLa
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: time-series-insights
ms.workload: na
ms.tgt_pltfrm: ''
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2018
ms.author: bryanla
ms.openlocfilehash: 5b845f36dbb65b38d0e4ac2a118277027239b3d6
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="tutorial-explore-the-time-series-insights-javascript-client-library"></a>Tutorial: Erkunden der Time Series Insights-JavaScript-Clientbibliothek

Um Entwickler bei der Abfrage und Visualisierung von Daten zu unterstützen, die in Time Series Insights (TSI) gespeichert sind, haben wir eine JavaScript D3-basierte Steuerelementbibliothek entwickelt, die diese Arbeit erleichtert. In diesem Tutorial erkunden Sie die TSI-JavaScript-Clientbibliothek und das zugehörige Programmiermodell anhand einer Beispielwebanwendung. 

In den behandelten Themen können Sie experimentieren, erfahren mehr über den Zugriff auf TSI-Daten und verwenden Diagrammsteuerelemente zum Rendern und Visualisieren von Daten. Ziel des Tutorials ist es, Ihnen genügend Informationen bereitzustellen, sodass Sie die Bibliothek in Ihrer eigenen Webanwendung verwenden können.

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
> * Die TSI-Beispielanwendung 
> * Die TSI-JavaScript-Clientbibliothek
> * Verwendung der Bibliothek durch die Beispielanwendung zum Visualisieren von TSI-Daten

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial verwendet das Feature „Entwicklertools“ (auch als „DevTools“ oder „F12“ bezeichnet), das Sie in den meisten modernen Webbrowsern wie [Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [Firefox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/) und weiteren finden. Wenn Sie mit diesem Feature noch nicht vertraut sind, empfiehlt es sich, das Feature zunächst in Ihrem Browser zu erkunden, bevor Sie mit dem Tutorial fortfahren. 

## <a name="the-time-series-insights-sample-application"></a>Die Time Series Insights-Beispielanwendung

In diesem Tutorial wird die Time Series Insights-Beispielanwendung verwendet, um den Quellcode der Anwendung einschließlich der Verwendung der TSI-JavaScript-Clientbibliothek zu untersuchen. Es handelt sich um eine Single-Page-Webanwendung, die die Verwendung der Bibliothek für die Abfrage und Visualisierung von Daten aus einer TSI-Beispielumgebung veranschaulicht. 

1. Navigieren Sie zur [Time Series Insights-Beispielanwendung](https://insights.timeseries.azure.com/clientsample). Es wird eine Seite ähnlich der folgenden angezeigt, und Sie werden aufgefordert, sich anzumelden: ![Aufforderung zur Anmeldung bei der TSI-Beispielanwendung](media/tutorial-explore-js-client-lib/tcs-sign-in.png)

2. Klicken Sie auf die Schaltfläche „Log in", und geben Sie Ihre Anmeldeinformationen ein bzw. wählen sie aus. Sie können ein Unternehmens-/Organisationskonto (Azure Active Directory) oder ein persönliches Konto (Microsoft-Konto oder MSA) verwenden. 

   ![TSI-Beispielanwendung, Eingabeaufforderung für Anmeldeinformationen](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)

3. Nach der erfolgreichen Anmeldung sehen Sie eine Seite ähnlich der folgenden, die mit TSI-Daten aufgefüllte Beispieldiagramme in verschiedenen Formatvorlagen enthält. Beachten Sie auch das Benutzerkonto und den Link zum Abmelden in der rechten oberen Ecke: ![Startseite der TSI-Beispielanwendung nach der Anmeldung](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)

### <a name="page-source-and-structure"></a>Quelle und Struktur der Seite

Zuerst sehen wir uns den HTML- und JavaScript-Quellcode der Seite an, die in Ihrem Browser gerendert wurde. Wir werden nicht alle Elemente im Einzelnen erläutern, aber Sie werden genügend Informationen zu den wichtigsten Abschnitten erhalten, sodass Sie ein Gefühl dafür bekommen, wie die Seite funktioniert:

1. Öffnen Sie die Entwicklertools in Ihrem Browser, und sehen Sie sich die HTML-Elemente an, aus denen die aktuelle Seite zusammengesetzt ist. Diese werden auch als HTML- oder DOM-Struktur bezeichnet.

2. Erweitern Sie die Elemente `<head>` und `<body>`, und beachten Sie die folgenden Abschnitte:
   - Unter `<head>` sehen Sie Elemente, die zusätzliche Dateien abrufen, um die Funktionsweise der Seite zu unterstützen:
     - Ein `<script>`-Element zum Verweisen auf die Azure Active Directory Authentication Library (adal.min.js) – auch bekannt als ADAL. Dies ist eine JavaScript-Bibliothek, die OAuth 2.0-Authentifizierung (Anmeldung) und Tokenabruf für den Zugriff auf APIs bereitstellt:
     - `<link>`-Elemente für Cascading Stylesheets (sampleStyles.css, tsiclient.css) – auch bekannt als CSS. Diese werden zum Steuern der visuellen Elemente einer Seite verwendet, z.B. für Farben, Schriftarten, Abstände usw. 
     - Ein `<script>`-Element zum Verweisen auf die TSI-Clientbibliothek (tsiclient.js) – eine JavaScript-Bibliothek, die von der Seite verwendet wird, um TSI-Dienst-APIs aufzurufen und Diagrammsteuerelemente auf der Seite zu rendern.

     >[!NOTE]
     > Der Quellcode für die JavaScript-ADAL steht im Repository [azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js) zur Verfügung.  
     > Der Quellcode für die TSI-JavaScript-Clientbibliothek ist im Repository [tsiclient](https://github.com/Microsoft/tsiclient) verfügbar.

   - Unter `<body>` finden Sie `<div>`-Elemente, die als Container fungieren, um das Layout von Elementen auf der Seite zu definieren, sowie ein weiteres `<script>`-Element:
     - Das erste `<div>`-Element gibt das Dialogfeld für die Anmeldung an (`id="loginModal"`).
     - Das zweite `<div>`-Element fungiert als übergeordnetes Element für Folgendes:
       - Ein `<div>`-Headerelement, das oben auf der Seite für Statusmeldungen und Anmeldeinformationen verwendet wird (`class="header"`).
       - Ein `<div>`-Element für die restlichen Elemente des Hauptteils der Seite, einschließlich aller Diagramme (`class="chartsWrapper"`).
       - Ein `<script>`-Abschnitt, der den gesamten JavaScript-Code enthält, der die Seite steuert.

   [![TSI-Beispielanwendung mit Entwicklertools](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

3. Wenn Sie das `<div class="chartsWrapper">`-Element erweitern, finden Sie dort weitere untergeordnete `<div>`-Elemente, die zum Positionieren der einzelnen Beispiele für Diagrammsteuerelemente verwendet werden. Beachten Sie, dass verschiedene `<div>`-Elementpaare vorhanden sind – eines für jedes Diagrammbeispiel:
   - Das erste Elementpaar (`class="rowOfCardsTitle"`) enthält einen Kurztitel, der beschreibt, was die Diagramme veranschaulichen. Beispiel: „Statische Liniendiagramme mit Legenden in voller Größe“
   - Das zweite Paar (`class="rowOfCards"`) ist ein übergeordnetes Element, das weitere untergeordnete `<div>`-Elemente enthält, die die eigentlichen Diagrammsteuerelemente auf einer Zeile positionieren. 

  ![Ansicht der div-Elemente des Hauptteils](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)

4. Erweitern Sie jetzt das `<script type="text/javascript">`-Element direkt unter dem `<div class="chartsWrapper">`-Element. Sie sehen den Anfang des JavaScript-Abschnitts auf Seitenebene, der die gesamte Seitenlogik für Aspekte wie Authentifizierung, Aufrufen von TSI-Dienst-APIs, Rendern der Diagrammsteuerelemente und vieles mehr verarbeitet:

  ![Ansicht der Skripts des Hauptteils](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)

## <a name="tsi-client-javascript-library-concepts"></a>Konzepte der TSI-JavaScript-Clientbibliothek

Die TSI-Clientbibliothek (tsclient.js) stellt eine Abstraktion für zwei wichtige Kategorien bereit:

- **Wrappermethoden für den Aufruf der TSI-Abfrage-APIs**: REST-APIs, mit denen Sie mithilfe von Aggregatausdrücken TSI-Daten abrufen können und die im `TsiClient.Server`-Namespace der Bibliothek organisiert sind. 
- **Methoden zum Erstellen und Auffüllen verschiedener Arten von Diagrammsteuerelementen**: Diese werden zum Rendern der aggregierten TSI-Daten in einer Webseite verwendet und sind im `TsiClient.UX`-Namespace der Bibliothek organisiert. 

Die folgenden Konzepte sind universell und gelten ganz allgemein für die TSI-Clientbibliotheks-APIs. 

### <a name="authentication"></a>Authentifizierung

Wie bereits erwähnt, handelt es sich bei diesem Beispiel um eine Single-Page-Anwendung, die zur Benutzerauthentifizierung die OAuth 2.0-Unterstützung in der ADAL verwendet. In diesem Abschnitt des Skripts finden sich einige interessante Punkte:

1. Damit die ADAL für die Authentifizierung verwendet werden kann, muss sich die Clientanwendung selbst in der Azure Active Directory-Anwendungsregistrierung registrieren. Da es sich um eine SPA handelt, wird diese Anwendung für die Verwendung des „impliziten“ OAuth 2.0-Autorisierungsflows registriert. Entsprechend gibt die Anwendung zur Laufzeit einige der Registrierungseigenschaften für die Flowverwendung an, wie z.B. die Client-ID-GUID (`clientId`) und den Umleitungs-URI (`postLogoutRedirectUri`).

2. Später fordert die Anwendung ein „Zugriffstoken“ von Azure AD an. Das Zugriffstoken wird für einen begrenzten Satz an Berechtigungen für einen bestimmten Dienst- oder API-Bezeichner ausgegeben (https://insights.timeseries.azure.com), der auch als „Tokenempfänger“ bezeichnet wird. Die Tokenberechtigungen werden im Namen des angemeldeten Benutzers ausgegeben. Der Bezeichner für den Dienst bzw. die API ist eine weitere Eigenschaft, die in der Azure AD-Registrierung der Anwendung enthalten ist. Sobald die ADAL das Zugriffstoken an die Anwendung zurückgegeben hat, wird es beim Zugriff auf die TSI-Dienst-APIs als „Bearertoken“ übergeben. 

   [!code-javascript[head-sample](~/samples-javascript/pages/index.html?range=140-199&highlight=4-9,36-39)]

### <a name="control-identification"></a>Identifizierung der Steuerelemente

Wie bereits erläutert, stellen die `<div>`-Elemente im `<body>`-Element das Layout für sämtliche Diagrammsteuerelemente bereit, die auf der Seite gezeigt werden. Jedes dieser Elemente gibt Eigenschaften für die Positionierung und visuellen Attribute eines Diagrammsteuerelements an, einschließlich einer `id`-Eigenschaft. Die `id`-Eigenschaft gibt einen eindeutigen Bezeichner an – dieser wird im JavaScript-Code verwendet, um beim Rendern und Aktualisieren für eine Identifizierung und Bindung aller Steuerelemente zu sorgen. 

### <a name="aggregate-expressions"></a>Aggregatausdrücke

Die TSI-Clientbibliothek-APIs macht ausführlich Gebrauch von Aggregatausdrücken. Ein Aggregatausdruck bietet die Möglichkeit, einen oder mehrere „Suchbegriffe“ zu erstellen. Die APIs ähneln der Art und Weise, in der der [Time Series Insights-Explorer](https://insights.timeseries.azure.com/demo) searchSpan, where-Prädikate, Measures und splitBy-Werte verwendet. Die meisten Bibliotheks-APIs akzeptieren ein Array aus Aggregatausdrücken, die vom Dienst zum Erstellen einer TSI-Datenabfrage verwendet werden können.

### <a name="call-pattern"></a>Aufrufmuster

Das Auffüllen und Rendern von Diagrammsteuerelementen folgt einem allgemeinen Muster. Sie finden dieses Muster im gesamten JavaScript-Code für die Seite, der die Steuerelemente für die TSI-Beispielanwendung instanziiert und lädt:

1. Deklarieren Sie ein Array, das mindestens einen TSI-Aggregatausdruck enthalten soll.  

   ```javascript
   var aes =  [];
   ```

2. Erstellen Sie 1 bis n Aggregatausdrucksobjekte, und fügen Sie diese zum Aggregatausdrucksarray hinzu.  

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```
   **aggregateExpression-Parameter**

   | Parameter | BESCHREIBUNG | Beispiel |
   | --------- | ----------- | ------- |
   | predicateObject | Der Datenfilterausdruck. |`{predicateString: "Factory = 'Factory3'"}` |
   | measureObject   | Der Eigenschaftenname des verwendeten Measures. | `{property: 'Temperature', type: "Double"}` |
   | measureTypes    | Die gewünschten Aggregationen der Measureeigenschaft. | `['avg', 'min']` |
   | searchSpan      | Die Dauer und Intervallgröße des Aggregatausdrucks. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | splitByObject   | Die Zeichenfolgeneigenschaft, nach der die Aufteilung erfolgen soll (optional – darf nicht NULL sein). | `{property: 'Station', type: 'String'}` |
   | color           | Die Farbe der Objekte, die Sie rendern möchten. | `'pink'` |
   | alias           | Ein Anzeigename für den Aggregatausdruck. | `'Factory3Temperature'` |
   | contextMenuActions | Ein Array aus Aktionen, die an die Zeitreihenobjekte in einer Visualisierung gebunden werden sollen (optional). | Weitere Informationen finden Sie unter [„Popupkontextmenüs“ im Abschnitt „Erweiterte Funktionen“](#popup-context-menus). |

3. Rufen Sie mithilfe von `TsiClient.Server`-APIs eine TSI-Abfrage auf, um die Aggregatdaten anzufordern.  

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```
   **getAggregates-Parameter**

   | Parameter | BESCHREIBUNG | Beispiel |
   | --------- | ----------- | ------- |
   | token     | Zugriffstoken für die TSI-API. | `authContext.getTsiToken()` Informationen dazu finden Sie im Abschnitt [Authentifizierung](#authentication). |
   | envFQDN     | Vollständig qualifizierter Domänenname für die TSI-Umgebung. | Im Azure-Portal z.B. `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`: |
   | aeTsxArray | Array von TSI-Abfrageausdrücken. | Verwenden Sie die `aes`-Variable, wie oben beschrieben: `aes.map(function(ae){return ae.toTsx()}` |

4. Transformieren Sie das von der TSI-Abfrage zurückgegebene komprimierte Ergebnis zur Visualisierung in JSON.

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

5. Erstellen Sie mithilfe der `TsiClient.UX`-APIs ein Diagrammsteuerelement, und binden Sie es an eines der `<div>`-Elemente auf der Seite.

   ```javascript
   var lineChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

6. Füllen Sie das Diagrammsteuerelement mit den transformierten JSON-Datenobjekten auf, und rendern Sie es auf der Seite.

   ```javascript
   lineChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Rendern von Steuerelementen

Die Bibliothek macht zurzeit acht eindeutige Steuerelemente für die Analyse verfügbar. Diese sind: Liniendiagramm, Kreisdiagramm, Balkendiagramm, Heatmap, Hierarchiesteuerelemente, Raster mit Zugriffsmöglichkeit, diskrete Ereigniszeitreihen und Zustandsübergang-Zeitreihen.   

### <a name="line-bar-pie-chart-examples"></a>Beispiele für Linien-, Balken- und Kreisdiagramme 

Sehen Sie sich zuerst den Code einiger standardmäßiger Diagrammsteuerelemente an, die in der Anwendung veranschaulicht werden, sowie das Programmiermodell bzw. die Programmiermuster für ihre Erstellung. Achten Sie besonders auf den HTML-Abschnitt unter dem Kommentar `// Example 3/4/5`, der die Steuerelemente mit den ID-Werten `chart3`, `chart4` und `chart5` rendert. 

Sie erinnern sich sicher aus Schritt 3 im Abschnitt [Quelle und Struktur der Seite](#page-source-and-structure), dass Diagrammsteuerelemente in Zeilen auf der Seite angeordnet sind und jede dieser Zeilen über eine Zeile mit einem beschreibenden Titel verfügt. In diesem Beispiel befinden sich die drei Diagramme, die aufgefüllt werden, unter dem `<div>`-Titel „Multiple Chart Types From the Same Data“, der an die drei `<div>`-Elemente darunter gebunden ist:

[!code-javascript[code-sample1-line-bar-pie](~/samples-javascript/pages/index.html?range=60-74&highlight=1,5,9,13)]

Der folgende Abschnitt des JavaScript-Codes verwendet das oben beschriebene Muster, um TSI-Aggregatausdrücke zu erstellen, diese für die Abfrage von TSI-Daten zu verwenden und die drei Diagramme zu rendern. Beachten Sie die drei Typen `LineChart`, `BarChart` und `PieChart` aus dem Namespace `tsiClient.ux`, mit denen die entsprechenden Diagramme erstellt und gerendert werden. Beachten Sie auch, dass alle drei Diagramme die gleichen Aggregatausdrucksdaten `transformedResult` verwenden können:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/index.html?range=236-257&highlight=13-14,16-17,19-20)]

Die drei Diagramme sehen nach dem Rendern folgendermaßen aus:

[![Mehrere Diagrammtypen aus denselben Daten](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Erweiterte Funktionen

Die Bibliothek macht auch einige optionale erweiterte Funktionen verfügbar machen, die Sie nutzen können.  

### <a name="states-and-events"></a>Zustände und Ereignisse

Ein Beispiel für die erweiterte Funktionalität ist die Möglichkeit, Diagrammen Zustandsübergänge und diskrete Ereignisse hinzuzufügen. Dieses Feature ist nützlich zum Hervorheben von Incidents, für Warnungen sowie für Zustandsschalter wie „Ein“ und „Aus“. 

Sehen Sie sich den Code des HTML-Abschnitts unter dem Kommentar `// Example 10` an. Der Code rendert unter dem Titel „Line Charts with Multiple Series Types“ ein Liniendiagramm-Steuerelement und bindet es an das `<div>`-Element mit dem ID-Wert `chart10`:

1. Zuerst wird eine Struktur namens `events4` definiert, in der die Zustandswechselelemente enthalten sind, die nachverfolgt werden sollen. Sie enthält folgende Elemente:
   - Einen Zeichenfolgenschlüssel namens `"Component States"` 
   - Ein Array aus Wertobjekten, die die Zustände repräsentieren. Jedes dieser Objekte enthält Folgendes:
     - Einen Zeichenfolgenschlüssel mit einem JavaScript-ISO-Zeitstempel
     - Ein Array mit den Merkmalen des Zustands
       - Eine Farbe
       - Eine Beschreibung

2. Dann wird die `events5`-Struktur für das `"Incidents"`-Element definiert, das ein Array der nachzuverfolgenden Ereigniselemente enthält. Die Arraystruktur weist die gleiche Form auf wie die für `events4` skizzierte. 

3. Schließlich wird das Liniendiagramm gerendert, und die beiden Strukturen mit den Optionsparametern für das Diagramm werden übergeben: `events:` und `states:`. Beachten Sie die weiteren Optionsparameter zum Angeben eines `tooltip:`-, `theme:`- oder `grid:`-Elements. 

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/index.html?range=332-384&highlight=5,26,51)]

Die rautenförmigen Marker/Popupelemente werden verwendet, um visuell auf Incidents hinzuweisen, und die farbigen Balken/Popupelemente entlang der Zeitskala weisen visuell auf Zustandsänderungen hin:

[![Line Charts with Multiple Series Types](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="popup-context-menus"></a>Popupkontextmenüs

Ein weiteres Beispiel für die erweiterte Funktionalität sind benutzerdefinierte Kontextmenüs (Popupmenüs, die per Rechtsklick aufgerufen werden), die nützlich sind, um Aktionen und logische nächste Schritte innerhalb des Geltungsbereichs Ihrer Anwendung zu aktivieren.

Sehen Sie sich hier den auf den HTML-Code folgenden Code unter `// Example 13/14/15` an. Diese Code rendert zunächst ein Liniendiagramm unter dem Titel „Line Chart with Context Menu to Create Pie/Bar Chart“, das an das `<div>`-Element mit dem ID-Wert `chart13` gebunden ist. Mithilfe von Kontextmenüs stellt das Liniendiagramm die Möglichkeit bereit, dynamisch ein Kreis- und ein Balkendiagramm zu erstellen, die an `<div>`- Elemente mit den IDs `chart14` bzw. `chart15` gebunden sind. Darüber hinaus verwenden sowohl das Kreis- als auch das Balkendiagramm Kontextmenüs, um jeweils eigene Funktionen zu aktivieren: die Möglichkeit, Daten aus dem Kreis- in das Balkendiagramm zu kopieren, und die Möglichkeit, Balkendiagrammdaten an das Browserkonsolenfenster auszugeben.

1. Zuerst wird eine Reihe von benutzerdefinierten Aktionen definiert. Jede Aktion enthält ein Array mit einem oder mehreren Elementen, wobei jedes Element ein einzelnes Kontextmenüelement definiert: 
   - `barChartActions`: definiert das Kontextmenü für das Kreisdiagramm, das ein Element enthält, um ein einzelnes Element zu definieren:
     - `name`: der für das Menüelement verwendete Text: „Print parameters to console“
     - `action`: die mit dem Menüelement verknüpfte Aktion. Bei dieser Element handelt es sich immer um eine anonyme Funktion, die drei Argumente basierend auf dem Aggregatausdruck akzeptiert, mit dem das Diagramm erstellt wird. In diesem Fall werden sie an das Browserkonsolenfenster geschrieben:
       - `ae`: das Array mit den Aggregatausdrücken
       - `splitBy`: der splitBy-Wert
       - `timestamp`: der Zeitstempel
   - `pieChartActions`: definiert das Kontextmenü für das Balkendiagramm, das ein Element enthält, um ein einzelnes Element zu definieren. Form und Schema entsprechen den obigen `barChartActions`-Elementen. Beachten Sie jedoch, dass die `action`-Funktion sich erheblich unterscheidet, da sie das Balkendiagramm instanziiert und rendert. Beachten Sie auch, dass die Funktion das `ae`-Argument verwendet, um das Array mit den Aggregatausdrücken anzugeben, das zur Laufzeit beim Popup des Menüelements übergeben wird. Die Funktion legt auch die `ae.contextMenu`-Eigenschaft mit dem `barChartActions`-Kontextmenü fest.
   - `contextMenuActions`: definiert das Kontextmenü für das Liniendiagramm, das drei Elemente enthält, um drei Menüelemente zu definieren. Form und Schema jedes Elements sind identisch mit den vorherigen Elementen. Genauso wie `barChartActions` schreibt das erste Element die drei Funktionsargumente an das Browserkonsolenfenster. Ebenso wie `pieChartActions` instanziieren und rendern die letzten beiden Elemente das Kreis- bzw. Balkendiagramm. Die letzten beiden Elemente legen auch ihre `ae.contextMenu`-Eigenschaften mit den Kontextmenüs `pieChartActions` bzw. `barChartActions` fest.

2. Dann werden zwei Aggregatausdrücke per Push an das `aes`-Array für Aggregatausdrücke übertragen, um das `contextMenuActions`-Array für jeden Ausdruck anzugeben. Diese werden mit dem Liniendiagramm-Steuerelement verwendet.

3. Zunächst wird nur das Liniendiagramm gerendert. Aus diesem können zur Laufzeit sowohl das Kreis- als auch das Balkendiagramm gerendert werden.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/index.html?range=456-535&highlight=7,16,29,61-64,78)]

Der Screenshot zeigt die Diagramme mit ihren jeweiligen Popupkontextmenüs. Das Kreis- und das Balkendiagramm wurden mithilfe der Kontextmenüoptionen des Liniendiagramms dynamisch erstellt:

[![Line Chart with Context Menu to Create Pie/Bar Chart](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Pinsel

Mit Pinseln kann ein Zeitbereich angegeben werden, um Aktionen wie Zoomen und Untersuchen zu definieren. 

Der Code, der zum Veranschaulichen von Pinseln verwendet wird, wurde auch im vorherigen Beispiel „Line Chart with Context Menu to Create Pie/Bar Chart“ gezeigt, bei dem es um [Popupkontextmenüs](#popup-context-menus-section) geht. 

1. Pinselaktionen ähneln einem Kontextmenü sehr: Sie definieren eine Reihe von benutzerdefinierten Aktionen für den Pinsel. Jede Aktion enthält ein Array mit einem oder mehreren Elementen, wobei jedes Element ein einzelnes Kontextmenüelement definiert:
   - `name`: der für das Menüelement verwendete Text: „Print parameters to console“
   - `action`: die mit dem Menüelement verknüpfte Aktion. Bei dieser handelt es sich immer um eine anonyme Funktion, die zwei Argumente akzeptiert. In diesem Fall werden sie an das Browserkonsolenfenster geschrieben:
      - `fromTime`: der from-Zeitstempel der Pinselauswahl
      - `toTime`: der to-Zeitstempel der Pinselauswahl

2. Pinselaktionen werden als weitere Diagrammoptionseigenschaft hinzugefügt. Beachten Sie die `brushContextMenuActions: brushActions`-Eigenschaft, die an den `linechart.Render`-Aufruf übergeben wird.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/index.html?range=521-535&highlight=1,13)]

![Liniendiagramm mit Kontextmenü zum Erstellen von Kreis-/Balkendiagrammen mithilfe von Pinseln](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anmelden bei der TSI-Beispielanwendung und Erkunden der Anwendung und ihrer Quellen
> * Verwenden von APIs in der TSI-JavaScript-Clientbibliothek
> * Verwenden von JavaScript zum Erstellen und Auffüllen von Diagrammsteuerelementen mit TSI-Daten

Wie bereits erläutert, verwendet die TSI-Beispielanwendung ein Demonstrationsdataset. Weitere Informationen dazu, wie Sie Ihre eigene TSI-Umgebung und Ihr eigenes Dataset erstellen, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Planen Ihrer Azure Time Series Insights-Umgebung](time-series-insights-environment-planning.md)


