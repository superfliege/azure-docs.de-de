---
title: Visualisieren von Daten im Azure Time Series Insights Preview-Explorer | Microsoft-Dokumentation
description: In diesem Artikel werden die in der Time Series Insights Preview-Explorer-Web-App verfügbaren Funktionen und Optionen beschrieben.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: ed8a829bd73a53dc94bf8b08648b3d6684f718f6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58109762"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualisieren von Daten in der Explorer Preview

In diesem Artikel werden die in der Time Series Insights Preview-[Explorer-Web-App](https://insights.timeseries.azure.com/preview/samples) verfügbaren Funktionen und Optionen beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie den Azure Time Series Insights Preview-Explorer verwenden, müssen Sie Folgendes erledigt haben:

* Einrichten einer Time Series Insights-Umgebung. Weitere Informationen finden Sie unter [Tutorial: Azure Time Series Insights Preview](./time-series-insights-update-create-environment.md).
* Bereitstellung von Datenzugriff auf die Time Series Insights-Umgebung, die Sie für das Konto erstellt haben. Außer Ihnen, können Sie auch anderen Zugriff gewähren.
* Hinzufügen einer Ereignisquelle zu der Time Series Insights-Umgebung zum Senden von Daten per Push an die Umgebung.

## <a name="learn-about-the-azure-time-series-insights-preview-explorer"></a>Weitere Informationen zum Azure Time Series Insights Preview-Explorer

  ![explorer-one][1]

Der Azure Time Series Insights Preview-Explorer besteht aus folgenden Elementen:

* **Navigationsleiste**: Ermöglicht ihnen das Wechseln zwischen den Seiten für Analyse und Modelle.
* **Hierarchiestruktur**: Ermöglicht Ihnen die Auswahl bestimmter Datenelemente für die Diagrammdarstellung.
* **Zeitreihenquelle**: Zeigt Ihre aktuell ausgewählten Datenelemente an.
* **Diagrammbereich**: Zeigt Ihr aktuelles Arbeitsdiagramm an.
* **Zeitachse**: Ermöglicht Ihnen die Änderung Ihres Arbeitszeitraums.
* **App-Leiste**: Enthält Ihre Benutzerverwaltungsoptionen, wie den aktuellen Mandanten, und gestattet Ihnen die Änderung von Design- und Spracheinstellungen.

## <a name="time-series-insights-preview-environment-panel"></a>Time Series Insights Preview-Umgebungsbereich

Im Umgebungsbereich werden alle Time Series Insights-Umgebungen angezeigt, auf die Sie Zugriff haben. Die Liste enthält Umgebungen mit nutzungsbasierter Bezahlung, Preview- und S1/S2-Umgebungen (allgemeine Verfügbarkeit). Klicken Sie einfach auf die Time Series Insights-Umgebung, die Sie verwenden möchten.

  ![explorer-two][2]

## <a name="time-series-insights-preview-navigation-menu"></a>Time Series Insights Preview-Navigationsmenü

  ![explorer-three][3]

Mit dem Navigationsmenü können Sie zwischen den Time Series Insights-Apps wechseln:

* **Analysieren**: Ermöglicht Ihnen die Durchführung funktionsreicher Analysen mit Ihren modellierten oder nicht modellierten Zeitreihendaten sowie deren Darstellung als Diagramme.

* **Modell**: Ermöglicht Ihnen, neue Time Series Insights Preview-Typen, -Hierarchien und -Instanzen per Push in Ihr Time Series Insights-Modell zu übertragen.

## <a name="time-series-insights-preview-model-authoring"></a>Time Series Insights Preview-Modellerstellung

Mit dieser App können Sie die Vorgänge zum Erstellen, Lesen, Aktualisieren und Löschen (CRUD, Create, Read, Update, Delete) an Ihrem Zeitreihenmodell ausführen.  

* **Zeitreihenmodelltyp**: Time Series Insights-Typen ermöglichen das Definieren von Variablen oder Formeln zur Durchführung von Berechnungen. Sie sind mit einer bestimmten Time Series Insights-Instanz verknüpft. Ein Typ kann eine oder mehrere Variablen enthalten.
* **Zeitreihenmodellhierarchie**: Hierarchien sind systematische Organisationen Ihrer Daten. Hierarchien stellen die Beziehungen zwischen verschiedenen Entitäten in Ihren Time Series Insights-Daten dar.
* **Zeitreihenmodellinstanz**: Instanzen sind die Zeitreihen selbst. In den meisten Fällen sind sie die „DeviceID“ oder die „AssetID“, die der eindeutige Bezeichner des Objekts in der Umgebung ist.

Weitere Informationen zu Zeitreihenmodellen finden Sie unter [Zeitreihenmodelle](./time-series-insights-update-tsm.md).

## <a name="time-series-insights-preview-model-search-panel"></a>Time Series Insights Preview-Modellsuchbereich

Im Modellsuchbereich können Sie ganz einfach in Ihrer Zeitreihenmodellhierarchie suchen und navigieren, um die spezifischen Zeitreiheninstanzen zu finden, die Sie in Ihrem Diagramm anzeigen möchten. Wenn Sie Ihre Instanzen auswählen, werden sie sowohl dem aktuellen Diagramm als auch der Datenquelle hinzugefügt.

  ![explorer-four][4]

## <a name="time-series-insights-preview-well"></a>Time Series Insights Preview-Quelle

In der Quelle werden Instanzfelder und andere Metadaten angezeigt, die ausgewählten Zeitreiheninstanzen zugeordnet sind. Mit den Kontrollkästchen auf der rechten Seite können Sie bestimmte Instanzen im aktuellen Diagramm ausblenden oder anzeigen. Sie können auch bestimmte Datenelemente aus Ihrer aktuellen Datenquelle entfernen, indem Sie auf das rote X-Steuerelement rechts neben dem Element klicken.

  ![explorer-five][5]

Sie können den Telemetriebereich auch ausklappen, um eine bessere vertikale Ansicht der Elemente in Ihrer Datenquelle zu erhalten.

  ![explorer-six][6]

> [!NOTE]
> Wenn die folgende Meldung angezeigt wird, enthält die Instanz während des ausgewählten Zeitraums keine Daten. Um das Problem zu beheben, können Sie den Zeitraum erhöhen oder bestätigen, dass die Instanz Daten per Push überträgt.
>
> ![explorer-seven][7]

## <a name="time-series-insights-preview-chart"></a>Time Series Insights Preview-Diagramm

Mit dem Diagramm können Sie Zeitreiheninstanzen als Linien anzeigen. Sie können den Umgebungsbereich, das Datenmodell und den Zeitraumsteuerelement-Bereich reduzieren, indem Sie auf die Websteuerelemente klicken, um das Diagramm zu vergrößern.

  ![explorer-eight][8]

1. **Ausgewählter Datumsbereich**: Steuert, welche Datenelemente zur Visualisierung verfügbar sind.

1. **„Innerer Datumsbereich“-Schiebereglertool**: Verwenden Sie die zwei Endpunktsteuerelemente, indem Sie sie über den gewünschten Zeitraum ziehen.

1. **Zeitraum-Reduzierungssteuerelement**: Reduziert und erweitert den Zeitraumbereichs-Editor.

1. **Y-Achsenformat-Steuerelement**: Durchläuft die verfügbaren Y-Achsenoptionen:

    * `Default`: Jede Zeile hat eine eigene Y-Achse.
    * `Stacked`: Ermöglicht Ihnen das Stapeln mehrerer Zeilen auf derselben Y-Achse, wobei sich die Y-Achsendaten auf Grundlage der ausgewählten Zeile ändern.
    * `Shared`: Anzeige aller Y-Achsendaten gemeinsam.

1. **Aktuelles Datenelement**: Das aktuell ausgewählte Datenelement und seine zugehörigen Details.

Sie können ein Drilldown in einen bestimmten Datenschnitt ausführen, indem Sie auf einen Datenpunkt im aktuellen Diagramm klicken und dann den ausgewählten Bereich zum Endpunkt Ihrer Wahl ziehen. Klicken Sie mit der rechten Maustaste auf den abgeblendeten, ausgewählten Bereich, und klicken Sie wie in der folgenden Abbildung dargestellt auf „Zoom“:

  ![explorer-nine][9]

Nachdem Sie den Zoomvorgang durchgeführt haben, wird Ihr ausgewähltes Dataset angezeigt. Klicken Sie auf das Y-Achsenformat-Steuerelement, um die drei Y-Achsendarstellungen Ihrer Time Series Insights-Daten zu durchlaufen.

  ![explorer-ten][10]

Hier sehen Sie ein Beispiel für freigegebenen Y-Achsen:

  ![explorer-eleven][11]

## <a name="time-series-insights-preview-time-editor-panel"></a>Time Series Insights Preview-Zeit-Editor-Bereich

Wenn Sie mit Time Series Insights Preview arbeiten, wählen Sie zunächst einen Zeitraum aus. Der ausgewählte Zeitraum steuert das Dataset, das für die Manipulation durch die Time Series Insights Preview-Widgets verfügbar ist. Die folgenden Websteuerelemente stehen in Time Series Insights Preview zur Verfügung, um Ihren Arbeitszeitraum auszuwählen.

  ![explorer-twelve][12]

1. **„Innerer Datumsbereich“-Schiebereglertool**: Verwenden Sie die zwei Endpunktsteuerelemente, indem Sie sie über den gewünschten Zeitraum ziehen. Dieser innere Datumsbereich wird durch das „Äußerer Datumsbereich“-Schieberegler-Steuerelement begrenzt.

1. **Schaltflächen zum Erhöhen/Verringern des Datumsbereichs**: Sie können Ihren Zeitraum erhöhen oder verringern, indem Sie eine der Schaltflächen für das gewünschte Intervall auswählen.

1. **Zeitraum-Reduzierungssteuerelement**: Mit diesem Websteuerelement können Sie alle Steuerelemente ausblenden, mit Ausnahme des „Innerer Datumsbereich“-Schiebereglertools.

1. **„Äußerer Datumsbereich“-Schieberegler-Steuerelement**: Verwenden Sie die Endpunktsteuerelemente, um den äußeren Datumsbereich auszuwählen, der dem „Innerer Datumsbereich“-Steuerelement zur Verfügung gestellt wird.

1. **Dropdown zur schnellen Datumsbereichsänderung**: Hiermit können Sie schnell zwischen voreingestellten Zeitbereichen wechseln, z. B. „letzte 30 Minuten“, „letzte 12 Stunden“ oder einem benutzerdefinierten Bereich. Durch das Ändern dieses Werts werden auch die verfügbaren Intervallbereiche geändert, die im Intervallgrößen-Schiebereglertool besprochen werden.

1. **Intervallgröße-Schiebereglertool**: Hiermit können Sie die Intervalle innerhalb desselben Zeitraums vergrößern und verkleinern. Diese Aktion bietet eine genauere Steuerung der Bewegung zwischen größeren Zeitabschnitten. Sie zeigt Trends gleichmäßig bis hin zu kleinsten Zeitabschnitten wie Millisekunden an, sodass Sie präzise, hochauflösende Schnitte Ihrer Daten anzeigen können. Der Standardstartpunkt des Schiebereglers ist als optimierte Ansicht der Daten aus Ihrer Auswahl festgelegt und gleicht somit Auflösung, Abfragegeschwindigkeit und Granularität aus.

1. **Websteuerelement für „Von/Bis“ eines Datumsbereichs**: Mit diesem Websteuerelement können Sie ganz einfach auf Ihre gewünschten Datumsbereiche und Zeiträume klicken und diese auswählen. Sie können das Steuerelement auch verwenden, um zwischen verschiedenen Zeitzonen zu wechseln. Nachdem Sie die Änderungen vorgenommen haben, wählen Sie, um sie auf Ihren aktuellen Arbeitsbereich anzuwenden, **Speichern** aus.

   ![explorer-thirteen][13]

## <a name="time-series-insights-preview-navigation-panel"></a>Time Series Insights Preview-Navigationsbereich

Im Navigationsbereich von Time Series Insights Preview stehen folgende Funktionen zur Verfügung:

  ![explorer-fourteen][14]

### <a name="current-session-share-link-control"></a>Steuerelement für Freigabelink der aktuellen Sitzung

  ![explorer-fifteen][15]

Wählen Sie das Link-Websteuerelement (hervorgehoben), aus, um eine URL zum Speichern oder Freigeben Ihrer aktuellen Azure Time Series Insights-Arbeitssitzung zu generieren, die Folgendes enthält:

* Aktuell ausgewählter Zeitraum
* Aktuell ausgewählte Intervallgröße
* Aktuell ausgewählte Datenquelle

### <a name="tenant-section"></a>Mandantenabschnitt

  ![explorer-sixteen][16]

* Zeigt Ihre aktuellen Time Series Insights-Anmeldekontoinformationen an.
* Ermöglicht Ihnen das Wechseln zwischen den verfügbaren Time Series Insights-Designs.

### <a name="theme-selection"></a>Designauswahl

Azure Time Series Insights Preview unterstützt zwei Designs:

* **Helles Design**: Das Standarddesign, das im gesamten vorliegenden Dokument gezeigt wird.
* **Dunkles Design**:  Stellt den Explorer wie hier gezeigt dar:

  ![explorer-seventeen][17]

Hier können Sie außerdem zwischen den unterstützten Sprachen wechseln.

## <a name="s1s2-environment-controls"></a>Steuerelemente für S1/S2-Umgebungen

### <a name="time-series-insights-preview-terms-panel"></a>Time Series Insights Preview-Begriffebereich

Dieser Abschnitt gilt nur für vorhandene S1/S2-Umgebungen, die versuchen, den Explorer mit der aktualisierten Benutzeroberfläche zu verwenden. Sie können auch das Produkt mit allgemeiner Verfügbarkeit und die Preview in Kombination verwenden. Wir haben dem aktualisierten Explorer einige Funktionen aus der vorhandenen Benutzeroberfläche hinzugefügt, aber Sie können die vollständige Benutzeroberflächenerfahrung für S1/S2-Umgebungen im vorhandenen Time Series Insights-Explorer erhalten.  

Anstelle der Hierarchie wird der Time Series Insights-Begriffebereich angezeigt, in dem Sie Abfragen in Ihrer Umgebung definieren können. Sie können damit Ihre Daten auf Grundlage eines Prädikats filtern.

  ![explorer-eighteen][18]

Der Time Series Insights Preview-Begriffe-Editor-Bereich akzeptiert die folgenden Parameter:

**Where**: Mit der Where-Klausel können Sie Ihre Ereignisse mithilfe der in der folgenden Tabelle aufgeführten Gruppe von Operanden schnell filtern. Wenn Sie eine Suche durch Auswählen eines Operanden ausführen, wird das Prädikat automatisch auf Basis dieser Suche aktualisiert. Folgende Operandentypen werden unterstützt:

| Vorgang | Unterstützte Typen   | Notizen |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan, NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan, NULL | Alle Operanden müssen denselben Typ aufweisen oder eine NULL-Konstante sein. |
| `HAS` | Zeichenfolge | Im rechten Bereich sind nur konstante Zeichenfolgenliterale zulässig. Leere Zeichenfolgen und NULL sind nicht zulässig. |

Informationen zu den unterstützten Abfragevorgängen und Datentypen finden Sie unter [Time Series Expression (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Beispiele für Where-Klauseln

  ![explorer-nineteen][19]

**Measure**: In dieser Dropdownliste werden alle numerische Spalten angezeigt (**Doubles**), die Sie als Elemente für Ihr aktuelles Diagramm verwenden können.

**Aufteilen nach**: In diesem Dropdown werden alle in Ihrem Modell verfügbaren kategorischen Spalten (Zeichenfolgen) angezeigt, nach denen Sie Ihre Daten gruppieren können. Sie können bis zu fünf Begriffe hinzufügen, die auf derselben X-Achse angezeigt werden. Geben Sie Ihre gewünschten Parameter ein, und wählen Sie dann **Hinzufügen** aus, um einen neuen Begriff hinzuzufügen.

  ![explorer-twenty][20]

Sie können Elemente im Diagrammbereich ein- und ausblenden, indem Sie das Symbol „Sichtbar“ auswählen, wie in der folgenden Abbildung dargestellt. Sie können Abfragen vollständig entfernen, indem Sie auf das rote **x** klicken.

  ![explorer-twenty-one][21]

## <a name="next-steps"></a>Nächste Schritte

Entsprechende Informationen finden Sie in den folgenden Artikeln:
* [Speicherung und Datenerfassung in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md)
* [Datenmodellierung](./time-series-insights-update-tsm.md)
* [Diagnose und Problembehandlung](./time-series-insights-update-how-to-troubleshoot.md)

<!-- Images -->
[1]: media/v2-update-explorer/explorer-one.png
[2]: media/v2-update-explorer/explorer-two.png
[3]: media/v2-update-explorer/explorer-three.png
[4]: media/v2-update-explorer/explorer-four.png
[5]: media/v2-update-explorer/explorer-five.png
[6]: media/v2-update-explorer/explorer-six.png
[7]: media/v2-update-explorer/explorer-seven.png
[8]: media/v2-update-explorer/explorer-eight.png
[9]: media/v2-update-explorer/explorer-nine.png
[10]: media/v2-update-explorer/explorer-ten.png
[11]: media/v2-update-explorer/explorer-eleven.png
[12]: media/v2-update-explorer/explorer-twelve.png
[13]: media/v2-update-explorer/explorer-thirteen.png
[14]: media/v2-update-explorer/explorer-fourteen.png
[15]: media/v2-update-explorer/explorer-fifteen.png
[16]: media/v2-update-explorer/explorer-sixteen.png
[17]: media/v2-update-explorer/explorer-seventeen.png
[18]: media/v2-update-explorer/explorer-eighteen.png
[19]: media/v2-update-explorer/explorer-nineteen.png
[20]: media/v2-update-explorer/explorer-twenty.png
[21]: media/v2-update-explorer/explorer-twenty-one.png
