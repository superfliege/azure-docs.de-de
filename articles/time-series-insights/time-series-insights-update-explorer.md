---
title: Visualisieren von Daten im Azure Time Series Insights Preview-Explorer | Microsoft-Dokumentation
description: In diesem Artikel werden die in der Time Series Insights Preview-Explorer-Web-App verfügbaren Funktionen und Optionen beschrieben.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: 359ef6ee8cfe4910e847376b7bd431a4c09aebc7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237681"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualisieren von Daten in der Explorer Preview

In diesem Dokument werden die UI/UX-Features und die Oberfläche der [Demo-Web-App](https://insights.timeseries.azure.com/preview/demo) zu Azure Time Series Insights Preview beschrieben. Insbesondere werden darin das Layout des gehosteten Beispiels, die Anpassungsoptionen für die Benutzeroberfläche und die Navigation durch die bereitgestellte Demo erläutert.

## <a name="prerequisites"></a>Voraussetzungen

Zum Einstieg in den Azure Time Series Insights Preview-Explorer müssen Sie Folgendes erledigt haben:

* Einrichten einer Time Series Insights-Umgebung. Wenn Sie mehr zur Bereitstellung einer Instanz erfahren möchten, probieren Sie unser Tutorial [Azure Time Series Insights Preview](./time-series-insights-update-create-environment.md) aus.
* [Bereitstellung von Datenzugriff](./time-series-insights-data-access.md) auf die Time Series Insights-Umgebung, die Sie für das Konto erstellt haben. Außer Ihnen, können Sie auch anderen Zugriff gewähren.
* Hinzufügen einer Ereignisquelle zur Time Series Insights-Umgebung zum Senden von Daten per Push an die Umgebung:
  * Informationen zum [Herstellen einer Verbindung mit einem Event Hub](./time-series-insights-how-to-add-an-event-source-eventhub.md)
  * Informationen zum [Herstellen einer Verbindung mit einem IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md)

## <a name="learn-about-the-preview-explorer"></a>Informationen zum Preview-Explorer

Der Azure Time Series Insights Preview-Explorer besteht aus folgenden Elementen:

[![Explorer-Ansicht](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

1. <a href="#environment-dropdown">**Umgebungsbereich**</a>: Zeigt Ihre Azure TSI-Umgebungen an.
1. <a href="#navigation-menu">**Navigationsmenü**</a>: Ermöglicht ihnen das Wechseln zwischen den Seiten **Analysieren** und **Modell**.
1. <a href="#hierarchy-tree">**Hierarchiestruktur**</a>: Ermöglicht Ihnen die Auswahl bestimmter Modell- und Datenelemente für die Diagrammdarstellung.
1. <a href="#preview-well">**Zeitreihenquelle**</a>: Zeigt Ihre zurzeit ausgewählten Datenelemente im Tabellenformat mit Farbcodierung an.
1. <a href="#preview-chart">**Diagrammbereich**</a>:  Zeigt Ihr aktuelles Arbeitsdiagramm an.
1. <a href="#time-editor-panel">**Zeitachse**</a>:  Ermöglicht Ihnen die Änderung Ihres Arbeitszeitraums.
1. <a href="#navigation-panel">**App-Leiste**</a>:  Enthält Ihre Benutzerverwaltungsoptionen, wie den aktuellen Mandanten, und gestattet Ihnen die Änderung von Design- und Spracheinstellungen.

## <a name="environment-dropdown"></a>Dropdown „Umgebung“

Im Dropdown „Umgebung“ werden alle Time Series Insights-Umgebungen angezeigt, auf die Sie Zugriff haben. Die Liste enthält Umgebungen mit nutzungsbasierter Bezahlung (Preview) und S1/S2-Umgebungen (allgemeine Verfügbarkeit). 

1. Klicken Sie einfach auf den Dropdownpfeil neben Ihrer angezeigten Umgebung:

   [![Die Systemsteuerung](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. Wählen Sie dann die gewünschte Umgebung aus.

## <a name="navigation-menu"></a>Navigationsmenü

  [![Das Navigationsmenü](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Im Navigationsmenü können Sie zwischen zwei Ansichten auswählen:

* **Analysieren**: Ermöglicht Ihnen die Durchführung funktionsreicher Analysen mit Ihren modellierten oder nicht modellierten Zeitreihendaten sowie deren Darstellung als Diagramme.
* **Modell**: Ermöglicht Ihnen, neue Time Series Insights Preview-Typen, -Hierarchien und -Instanzen per Push in Ihr Time Series Insights-Modell zu übertragen.

## <a name="hierarchy-tree"></a>Hierarchiestruktur

Die Hierarchiestruktur zeigt ausgewählte Datenelemente an, darunter Modelle, bestimmte Geräte und Sensoren auf Ihren Geräten.

### <a name="model-search-panel"></a>Modellsuchbereich

Im Modellsuchbereich können Sie ganz einfach in Ihrer Zeitreihenmodellhierarchie suchen und navigieren, um die spezifischen Zeitreiheninstanzen zu finden, die Sie in Ihrem Diagramm anzeigen möchten. Wenn Sie Ihre Instanzen auswählen, werden sie sowohl dem aktuellen Diagramm als auch der Datenquelle hinzugefügt.

  [![Der Modellsuchbereich](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Modellerstellung

Die Azure Time Series Insights Preview unterstützt die vollständigen CRUD-Vorgänge („Create“, „Read“, „Update“, „Delete“ – „Erstellen“, „Lesen“, „Aktualisieren“, „Löschen“) in Ihrem Zeitreihenmodell.  

* **Zeitreihenmodelltyp**: Time Series Insights-Typen ermöglichen das Definieren von Variablen oder Formeln zur Durchführung von Berechnungen. Sie sind mit einer bestimmten Time Series Insights-Instanz verknüpft. Ein Typ kann eine oder mehrere Variablen enthalten.
* **Zeitreihenmodellhierarchie**: Hierarchien sind systematische Organisationen Ihrer Daten. Hierarchien stellen die Beziehungen zwischen verschiedenen Entitäten in Ihren Time Series Insights-Daten dar.
* **Zeitreihenmodellinstanz**: Instanzen sind die Zeitreihen selbst. In den meisten Fällen sind dies die **DeviceID** oder die **AssetID**, bei der es sich um den eindeutigen Bezeichner des Objekts in der Umgebung handelt.

Weitere Informationen zu Zeitreihenmodellen finden Sie unter [Zeitreihenmodelle](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Vorschauquelle

In der Quelle werden Instanzfelder und andere Metadaten angezeigt, die ausgewählten TSI-Instanzen zugeordnet sind. Mit den Kontrollkästchen auf der rechten Seite können Sie bestimmte Instanzen im aktuellen Diagramm ausblenden oder anzeigen. Sie können auch bestimmte Datenelemente aus Ihrer aktuellen Datenquelle entfernen, indem Sie auf der linken Seite des Elements auf das rote Steuerelement **Löschen** (Papierkorb) klicken.

  [![Die Vorschauquelle](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Sie können auch das Layout Ihrer Diagrammseite **Analysieren** neu konfigurieren, indem Sie oben rechts das Symbol „Auslassungspunkte“ auswählen:

  [![Layoutoptionen für Telemetrie](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Wenn die folgende Meldung angezeigt wird, enthält die Instanz während des ausgewählten Zeitraums keine Daten. Um das Problem zu beheben, können Sie den Zeitraum erhöhen oder bestätigen, dass die Instanz Daten per Push überträgt.
>
> ![Keine Datenbenachrichtigung](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Vorschaudiagramm

Mit dem Diagramm können Sie TSI-Instanzen als Linien anzeigen. Sie können den Umgebungsbereich, das Datenmodell und den Zeitraumsteuerelement-Bereich reduzieren, indem Sie auf die Websteuerelemente klicken, um das Diagramm zu vergrößern.

  [![Vorschaudiagramm „Übersicht“](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

1. **Ausgewählter Datumsbereich**: Steuert, welche Datenelemente zur Visualisierung verfügbar sind.

1. **„Innerer Datumsbereich“-Schiebereglertool**: Verwenden Sie die zwei Endpunktsteuerelemente, indem Sie sie über den gewünschten Zeitraum ziehen.

1. **Zeitraum-Reduzierungssteuerelement**: Reduziert und erweitert den Zeitraumbereichs-Editor.

1. **Y-Achsenformat-Steuerelement**: Durchläuft die verfügbaren Y-Achsenoptionen:

    * `Default`: Jede Zeile hat eine eigene Y-Achse.
    * `Stacked`: Ermöglicht Ihnen das Stapeln mehrerer Zeilen auf derselben Y-Achse, wobei sich die Y-Achsendaten auf Grundlage der ausgewählten Zeile ändern.
    * `Shared`: Anzeige aller Y-Achsendaten gemeinsam.

1. **Aktuelles Datenelement**: Das aktuell ausgewählte Datenelement und seine zugehörigen Details.

Sie können ein Drilldown in einen bestimmten Datenschnitt ausführen, indem Sie auf einen Datenpunkt im aktuellen Diagramm klicken und dann den ausgewählten Bereich zum Endpunkt Ihrer Wahl ziehen. Klicken Sie mit der rechten Maustaste auf den abgeblendeten, ausgewählten Bereich, und klicken Sie, wie in der folgenden Abbildung dargestellt, auf **Zoom**:

  [![Vorschaudiagramm „Zoom“](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Nachdem Sie die Aktion **Zoom** ausgeführt haben, wird Ihr ausgewähltes Dataset angezeigt. Klicken Sie auf das Y-Achsenformat-Steuerelement, um die drei Y-Achsendarstellungen Ihrer Time Series Insights-Daten zu durchlaufen.

  [![Vorschaudiagramm „Y-Achse“](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Hier sehen Sie ein Beispiel für freigegebenen Y-Achsen:

  [![Vorschau der freigegebenen Y-Achse](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Editor-Bereich für Zeiten

Wenn Sie mit Time Series Insights Preview arbeiten, wählen Sie zunächst einen Zeitraum aus. Der ausgewählte Zeitraum steuert das Dataset, das für die Manipulation durch die Time Series Insights Preview-Widgets verfügbar ist. Die folgenden Websteuerelemente stehen in Time Series Insights Preview zur Verfügung, um Ihren Arbeitszeitraum auszuwählen.

  [![Zeitauswahlbereich](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **„Innerer Datumsbereich“-Schiebereglertool**: Verwenden Sie die zwei Endpunktsteuerelemente, indem Sie sie über den gewünschten Zeitraum ziehen. Dieser innere Datumsbereich wird durch das „Äußerer Datumsbereich“-Schieberegler-Steuerelement begrenzt.

1. **Schaltflächen zum Erhöhen/Verringern des Datumsbereichs**: Sie können Ihren Zeitraum erhöhen oder verringern, indem Sie eine der Schaltflächen für das gewünschte Intervall auswählen.

1. **Zeitraum-Reduzierungssteuerelement**: Mit diesem Websteuerelement können Sie alle Steuerelemente ausblenden, mit Ausnahme des „Innerer Datumsbereich“-Schiebereglertools.

1. **„Äußerer Datumsbereich“-Schieberegler-Steuerelement**: Verwenden Sie die Endpunktsteuerelemente, um den äußeren Datumsbereich auszuwählen, der dem „Innerer Datumsbereich“-Steuerelement zur Verfügung gestellt wird.

1. **Dropdown zur schnellen Datumsbereichsänderung**: Hiermit können Sie schnell zwischen voreingestellten Zeitbereichen wechseln, z. B. **letzte 30 Minuten**, **letzte 12 Stunden** oder einem **benutzerdefinierten Bereich**. Durch das Ändern dieses Werts werden auch die verfügbaren Intervallbereiche geändert, die im Intervallgrößen-Schiebereglertool besprochen werden.

1. **Intervallgröße-Schiebereglertool**: Hiermit können Sie die Intervalle innerhalb desselben Zeitraums vergrößern und verkleinern. Diese Aktion bietet eine genauere Steuerung der Bewegung zwischen größeren Zeitabschnitten. Sie zeigt Trends gleichmäßig bis hin zu kleinsten Zeitabschnitten wie Millisekunden an, sodass Sie präzise, hochauflösende Schnitte Ihrer Daten anzeigen können. Der Standardstartpunkt des Schiebereglers ist als optimierte Ansicht der Daten aus Ihrer Auswahl festgelegt und gleicht somit Auflösung, Abfragegeschwindigkeit und Granularität aus.

1. **Websteuerelement für „Von/Bis“ eines Datumsbereichs**: Mit diesem Websteuerelement können Sie ganz einfach auf Ihre gewünschten Datumsbereiche und Zeiträume klicken und sie auswählen. Sie können das Steuerelement auch verwenden, um zwischen verschiedenen Zeitzonen zu wechseln. Nachdem Sie die Änderungen vorgenommen haben, wählen Sie, um sie auf Ihren aktuellen Arbeitsbereich anzuwenden, **Speichern** aus.

   [![Auswahlbereich „In“ und „Aus“](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Navigationsbereich

Der Navigationsbereich „Time Series Insights Preview“ wird oben in Ihrer TSI-App angezeigt. Er bietet die folgenden Funktionen.

### <a name="current-session-share-link-control"></a>Steuerelement für Freigabelink der aktuellen Sitzung

  [![Symbol „Freigeben“](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Wählen Sie das neue Symbol **Freigeben** aus, um einen URL-Link für Ihr Team freizugeben.

  [![Freigeben Ihrer Instanz-URL](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Mandantenabschnitt

  [![Mandantenauswahl](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Zeigt Ihre aktuellen Time Series Insights-Anmeldekontoinformationen an.
* Ermöglicht Ihnen das Wechseln zwischen den verfügbaren Time Series Insights-Designs.
* Ermöglicht Ihnen die Anzeige der [Demo-Web-App](https://insights.timeseries.azure.com/preview/demo) zur Preview.

### <a name="theme-selection"></a>Designauswahl

Wenn Sie ein neues Design auswählen möchten, klicken Sie oben rechts auf Ihr Profilsymbol. Wählen Sie dann **Design ändern** aus.

  [![Designauswahl](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Die Sprachauswahl ist auch verfügbar, wenn Sie auf Ihr Profilsymbol klicken.

Azure Time Series Insights Preview unterstützt zwei Designs:

* **Helles Design**: Das Standarddesign, das im gesamten vorliegenden Dokument gezeigt wird.
* **Dunkles Design**:  Stellt den Explorer wie hier gezeigt dar:

  [![„Dunkles Design“ ausgewählt](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>Steuerelemente für S1/S2-Umgebungen

### <a name="preview-terms-panel"></a>Begriffebereich der Preview

Dieser Abschnitt gilt nur für vorhandene S1/S2-Umgebungen, die versuchen, den Explorer mit der aktualisierten Benutzeroberfläche zu verwenden. Sie können auch das Produkt mit allgemeiner Verfügbarkeit und die Preview in Kombination verwenden. Wir haben dem aktualisierten Explorer einige Funktionen aus der vorhandenen Benutzeroberfläche hinzugefügt, aber Sie können die vollständige Benutzeroberflächenerfahrung für S1/S2-Umgebungen im vorhandenen Time Series Insights-Explorer erhalten.  

Anstelle der Hierarchie wird der Time Series Insights-Begriffebereich angezeigt, in dem Sie Abfragen in Ihrer Umgebung definieren können. Sie können damit Ihre Daten auf Grundlage eines Prädikats filtern.

  [![Abfragebereich „Where“](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

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

  [![Beispiele für die „Where-Klausel“](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Measure**: Ein Dropdown mit allen numerischen Spalten (**Doubles**), die Sie als Elemente für Ihr aktuelles Diagramm verwenden können.

**Aufteilen nach**: In diesem Dropdown werden alle in Ihrem Modell verfügbaren kategorischen Spalten (Zeichenfolgen) angezeigt, nach denen Sie Ihre Daten gruppieren können. Sie können bis zu fünf Begriffe hinzufügen, die auf derselben X-Achse angezeigt werden. Geben Sie Ihre gewünschten Parameter ein, und wählen Sie dann **Hinzufügen** aus, um einen neuen Begriff hinzuzufügen.

  [![Abgerufene und gefilterte Ansicht eins](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Sie können Elemente im Diagrammbereich ein- und ausblenden, indem Sie das Symbol „Sichtbar“ auswählen, wie in der folgenden Abbildung dargestellt. Sie können Abfragen vollständig entfernen, indem Sie auf das rote **X** klicken.

  [![Abgerufene und gefilterte Ansicht zwei](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Datenspeicherung und Dateneingang](./time-series-insights-update-storage-ingress.md) in Azure Time Series Insights Preview.

- Lesen Sie das Time Series Insights Preview-Dokument zur [Datenmodellierung](./time-series-insights-update-tsm.md).

- Informieren Sie sich über die [Vorgehensweise zur Diagnose und Problembehandlung](./time-series-insights-update-how-to-troubleshoot.md) in Ihrer Time Series Insights-Instanz.
