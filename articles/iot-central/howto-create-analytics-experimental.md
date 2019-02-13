---
title: Analysieren Ihrer Gerätedaten in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Analysieren Sie Ihre Gerätedaten in Ihrer Azure IoT Central-Anwendung.
author: lmasieri
ms.author: lmasieri
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 851a509421924a2ea6607020e7749c10dcbec01d
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55772514"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Gewusst wie: Analysieren von Gerätedaten durch Analysen


*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*


Azure IoT Central stellt umfassende Analysefunktionen bereit, um große Mengen an Daten von Ihren Geräten zu verarbeiten. Um zu beginnen, wechseln Sie im links angezeigten Navigationsmenü zu **Analyse**. 

## <a name="querying-your-data"></a>Abfragen Ihrer Daten

Sie müssen zunächst eine **Gerätegruppe** auswählen, einen **Filter** hinzufügen (optional) und einen **Zeitraum** auswählen, um zu starten. Klicken Sie anschließend auf *Ergebnisse anzeigen*, um mit der Visualisierung Ihrer Daten zu beginnen.


* **Gerätegruppen:** Eine [Gerätegruppe](howto-use-device-sets-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) ist eine benutzerdefinierte Gruppe Ihrer Geräte. Beispiele: „Alle Kühlschränke in Oakland“ oder „Alle Windturbinen der Version 2.0“.

<!---
to-do: confirm if 10 is the max number of filters
to-do: do we need to explain how fiters work?
--->

* **Filter:** Sie können Ihrer Suche optional Filter hinzufügen, um Ihre Daten zu verfeinern. Sie können bis zu 10 Filter gleichzeitig hinzufügen. Beispielsweise können Sie für „Alle Kühlschränke in Oakland“ diejenigen herausfiltern, deren Temperatur über 15 Grad angestiegen ist. 
* **Zeitraum:** Standardmäßig werden die Daten der letzten 10 Minuten abgerufen. Sie können diesen Wert in einen der vordefinierten Zeitbereiche ändern oder einen benutzerdefinierten Zeitraum auswählen. 

 ![Analyseabfrage](media/howto-create-analytics-experimental/analytics-query.png)

## <a name="visualizing-your-data"></a>Visualisieren Ihrer Daten

Nachdem Sie Ihre Daten abgefragt haben, können Sie damit beginnen, sie zu visualisieren. Sie können Messungen anzeigen/ausblenden, die Art der Datenaggregation ändern und die Daten nach unterschiedlichen Geräteeigenschaften weiter unterteilen.  

* **Aufteilen nach:** Durch das Aufteilen von Daten nach Geräteeigenschaften können Sie weitere Detailinformationen zu Ihren Daten anzeigen. Beispielsweise können Sie Ihre Ergebnisse nach Geräte-ID oder Standort unterteilen.
<!---
to-do: confirm if 10 is the max number of measurements
--->
* **Messungen:** Sie können in einem Arbeitsschritt bis zu 10 verschiedene Telemetrieelemente ein-/ausblenden, die von Ihren Geräten gemeldet werden. Messungen können beispielsweise Informationen wie Temperatur und Feuchtigkeit umfassen. 
* **Aggregation:** Daten werden standardmäßig nach dem Durchschnitt aggregiert, aber Sie können die Datenaggregation Ihren Anforderungen entsprechend ändern. 

   ![Visualisierung der Analysedaten – „Teilen nach“](media/howto-create-analytics-experimental/analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Interaktion mit Ihren Daten

Es gibt verschiedene Möglichkeiten, Ihre Abfrageergebnisse weiter zu verändern, damit sie Ihren Visualisierungsanforderungen entsprechen. Sie können zwischen einer Diagramm- und einer Tabellenansicht wechseln, die Anzeige vergrößern oder verkleinern, Ihr Dataset aktualisieren und die Anzeige der Zeilen ändern.

* **Raster anzeigen:** Ihre Ergebnisse werden in einem Tabellenformat zur Verfügung gestellt, damit Sie den spezifischen Wert für jeden Datenpunkt anzeigen können. Diese Ansicht erfüllt außerdem die Standards der Barrierefreiheit. 
* **Diagramm anzeigen**: Ihre Ergebnisse werden in Form von Linien angezeigt, um Aufwärts-/Abwärtstrends und Anomalien leicht erkennbar zu machen. 

 ![Rasteransicht für Ihre Analyse](media/howto-create-analytics-experimental/analytics-showgrid.png)

Mit der Zoomfunktion können Sie sich auf bestimmte Daten konzentrieren. Wenn Sie einen bestimmten Zeitraum für Ihr Resultset genauer untersuchen möchten, können Sie den gewünschten Bereich mit dem Cursor markieren und über die verfügbaren Steuerelemente die folgenden Aktionen ausführen:
* **Vergrößern:** Nachdem Sie einen Zeitraum ausgewählt haben, wird die Zoomfunktion aktiviert, und Sie können die Datenansicht vergrößern.
* **Verkleinern:** Mit diesem Steuerelement können Sie die Vergrößerung Ihrer letzten Ansicht um eine Stufe verringern. Wenn Sie Ihre Datenansicht beispielsweise dreimal vergrößert haben, gelangen Sie mit dieser Funktion jeweils einen Schritt zurück.
* **Zoom zurücksetzen**: Nachdem Sie verschiedene Vergrößerungen/Verkleinerungen durchgeführt haben, können Sie die Anzeige mit diesem Steuerelement auf das ursprüngliche Resultset zurücksetzen. 

 ![Zoomvorgänge in Ihren Daten](media/howto-create-analytics-experimental/analytics-zoom.png)


Sie können den Linienstil an Ihre Anforderungen anpassen. Dabei stehen vier Optionen zur Auswahl:
* **Linie:** Zwischen jedem der Datenpunkte wird eine gerade Linie angezeigt. 
* **Geglättet**: Zwischen jedem Datenpunkt wird eine geschwungene Linie angezeigt.
* **Schritt:** Durch eine Linie an jedem Datenpunkt im Diagramm wird eine Stufenansicht erzeugt.
* **Punkt:** Alle Punkte im Diagramm werden ohne Verbindungslinien gezeichnet. 

 ![Verfügbare Linientypen bei der Analyse](media/howto-create-analytics-experimental/analytics-linetypes.png)

Schließlich können Sie Ihre Daten auf der Y-Achse anordnen, indem Sie einen dieser drei Modi auswählen:

* **Gestapelt:** Für jede Messung werden Diagramme als Stapel dargestellt, wobei jedes Diagramm mit einer eigenen Y-Achse versehen ist. Gestapelte Diagramme sind nützlich, wenn Sie mehrere Messungen ausgewählt haben und unterschiedliche Ansichten von diesen Messungen anzeigen möchten.
* **Nicht gestapelt:** Für jedes Measure wird ein Diagramm anhand einer Y-Achse dargestellt, wobei sich die Werte für die Y-Achse jedoch basierend auf dem markierten Measure ändern. Nicht gestapelte Diagramme sind nützlich, wenn Sie mehrere Measures überlagern und für denselben Zeitraum Muster in diesen Measures ermitteln möchten.
* **Gemeinsame Y-Achse**: Alle Diagramme besitzen dieselbe Y-Achse, und die Werte für die Achse ändern sich nicht. Diagramme mit gemeinsamen Y-Achsen sind hilfreich, wenn Sie ein einziges Measure untersuchen möchten und gleichzeitig die Daten nach bestimmten Kriterien aufteilen.

 ![Anordnung der Daten auf der Y-Achse mit unterschiedlichen Visualisierungsmodi](media/howto-create-analytics-experimental/analytics-yaxis.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie benutzerdefinierte Analysen für Ihre Azure IoT Central-Anwendung erstellt werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Eine Node.js-Anwendung vorbereiten und verbinden](howto-connect-nodejs-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)