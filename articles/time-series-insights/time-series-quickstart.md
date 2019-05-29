---
title: 'Schnellstart: Azure Time Series Insights-Explorer | Microsoft-Dokumentation'
description: Dieser Schnellstart veranschaulicht die ersten Schritte mit dem Azure Time Series Insights-Explorer im Webbrowser zum Visualisieren großer IoT-Datenmengen. Übersicht über die wichtigsten Features in einer Demoumgebung.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: be663520c9c1afd11ace57cd9dcb8ffb8219b86b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696956"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Schnellstart: Erkunden von Azure Time Series Insights

Es wird ein Schnellstart-Explorer für die ersten Schritte mit Azure Time Series Insights in einer kostenlosen Demoumgebung bereitgestellt. Durch ihn erfahren Sie, wie Sie große IoT-Datenmengen im Webbrowser visualisieren, und lernen die wichtigsten Features kennen, die zurzeit allgemein verfügbar sind.

Azure Time Series Insights ist ein vollständig verwalteter Analyse-, Speicher- und Visualisierungsdienst, der die simultane Untersuchung und Analyse von Milliarden von IoT-Ereignissen vereinfacht. Sie erhalten globale Einblicke in Ihre Daten und können basierend darauf Ihre IoT-Lösung schneller überprüfen und kostenintensive Ausfallzeiten unternehmenskritischer Geräte vermeiden. Durch Azure Time Series Insights können Sie versteckte Trends aufdecken, Abweichungen erkennen und Ursachenanalysen nahezu in Echtzeit durchführen.

Zur Erhöhung der Flexibilität kann Azure Time Series Insights über seine leistungsfähigen [REST-APIs](./time-series-insights-update-tsq.md) und das [Client-SDK](./tutorial-create-tsi-sample-spa.md) einer bereits vorhandenen Anwendung hinzugefügt werden. Die APIs ermöglichen es Ihnen, Zeitreihendaten zu speichern und abzufragen sowie Zeitreihendaten in einer Clientanwendung Ihrer Wahl zu nutzen. Sie können sich auch dafür entscheiden, das Client-SDK zu verwenden, um Ihrer vorhandenen Anwendung Benutzeroberflächenkomponenten hinzuzufügen.

Der Time Series Insights-Explorer ist eine geführte Tour durch die Features, die zurzeit allgemein verfügbar sind.

## <a name="prepare-the-demo-environment"></a>Vorbereiten der Demoumgebung

1. Erstellen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), falls noch keines erstellt wurde.

1. Navigieren Sie in Ihrem Browser zur [Demo der allgemeinen Verfügbarkeit](https://insights.timeseries.azure.com/demo).

1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit Ihren Azure-Anmeldeinformationen beim Time Series Insights-Explorer an.

1. Die Seite zur Schnelleinführung in Time Series Insights wird angezeigt. Klicken Sie auf **Weiter**, um die Schnelleinführung zu starten.

   [![Klicken Sie auf „Weiter“](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox).

## <a name="explore-the-demo-environment"></a>Erkunden der Demoumgebung

1. Der **Zeitauswahlbereich** wird angezeigt. Wählen Sie in diesem Bereich den Zeitrahmen aus, der dargestellt werden soll.

   [![Zeitauswahlbereich](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Ziehen Sie die Region, und klicken Sie dann auf die Schaltfläche **Suchen**.

   [![Auswählen eines Zeitrahmens](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   In Time Series Insights wird eine Diagrammvisualisierung für den angegebenen Zeitrahmen angezeigt. In dem Liniendiagramm können Sie verschiedene Aktionen ausführen, z.B. Filtern, Anheften, Sortieren und Stapeln.

   Um zum **Zeitauswahlbereich** zurückzukehren, klicken Sie wie dargestellt auf den Pfeil nach unten:

   [![Diagramm](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Klicken Sie im **Begriffsbereich** auf **Hinzufügen**, um einen neuen Suchbegriff hinzuzufügen.

   [![Hinzufügen eines Elements](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. Im Diagramm können Sie eine Region auswählen, mit der rechten Maustaste auf die Region klicken und **Explore Events** (Ereignisse durchsuchen) auswählen.

   [![Ereignisse untersuchen](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   Ein Raster der Rohdaten aus der durchsuchten Region wird angezeigt:

   [![Rasteransicht](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Auswählen und Filtern von Daten

1. Bearbeiten Sie die Begriffe, um die Werte im Diagramm zu ändern, und fügen Sie einen weiteren Begriff hinzu, um verschiedene Werttypen übergreifend zu korrelieren:

   [![Hinzufügen eines Begriffs](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. Geben Sie in das Feld **Datenreihe filtern...** einen Filterbegriff zum improvisierten Filtern von Datenreihen ein. Geben Sie für den Schnellstart **Station5** ein, um die Temperatur und den Druck für die Station übergreifend zu korrelieren.

   [![Datenreihen filtern](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Nach Abschluss des Schnellstarts können Sie das Beispiel-DataSet verwenden, um verschiedene Visualisierungen zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

Nun können Sie Ihre eigene Time Series Insights-Umgebung erstellen:
> [!div class="nextstepaction"]
> [Planen Ihrer Time Series Insights-Umgebung](time-series-insights-environment-planning.md)
